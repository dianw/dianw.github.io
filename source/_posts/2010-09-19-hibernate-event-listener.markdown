---
layout: post
title: Hibernate Event Listener
comments: true
---
Barusan saya baca-baca tentang database trigger dan sempat coba-coba, malas juga ternyata nulis kodenya yang begitu banyak :p benar-benar programmer yang buruk, hehehehe... Dan seperti apa yang saya inginkan, tuink... sangat tepat, ternyata hibernate juga menyediakan fasilitas yang mirip seperti trigger pada database, yaitu Hibernate Event. Sebuah mekanisme yang akan otomatis dijalankan apabila pemicu dipanggil. Untuk penjelasan lebih lanjut tentang event dan trigger bisa lihat di sini dan di sini (lagi-lagi males nulis :p).
 Langsung saja ke kasusnya...
 Saya mempunyai dua buah entity yaitu Produk dan CatatanAktivitasProduk. Dan saya akan membuat sebuah mekanisme agar hibernate secara otomatis manambahkan baris data pada CatatanAktivitasProduk ketika user melakukan transaksi (CRUD) data terhadap table Produk. Baris data berupa tanggal transaksi, aktivitas (lihat, tambah, perbarui, ataupun hapus), dan id produk yang terpengaruh akibat transaksi yang dilakukan oleh user tersebut.
 Pertama berikut entity yang saya butuhkan.
{% codeblock lang:java %} package com.mervpolis.dwx.hibernate.entity;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
@Entity
@Table
public class Produk {
 @Id
 @GeneratedValue(strategy = GenerationType.IDENTITY)
 private long id;
 @Column
 private String nama;
 @Column
 private long harga;
 @Column
 private String deskripsi;
 public long getId() {
 return id;
 }
 public void setId(long id) {
 this.id = id;
 }
 public String getNama() {
 return nama;
 }
 public void setNama(String nama) {
 this.nama = nama;
 }
 public long getHarga() {
 return harga;
 }
 public void setHarga(long harga) {
 this.harga = harga;
 }
 public String getDeskripsi() {
 return deskripsi;
 }
 public void setDeskripsi(String deskripsi) {
 this.deskripsi = deskripsi;
 }
}
 {% endcodeblock %}
{% codeblock lang:java %} package com.mervpolis.dwx.hibernate.entity;
import java.sql.Timestamp;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.Table;
@Entity
@Table
public class CatatanAktivitasProduk {
 @Id
 @GeneratedValue(strategy=GenerationType.IDENTITY)
 private long id;
 @ManyToOne
 @JoinColumn(name = "produk_id")
 private Produk produk;
 @Column
 private String aktivitas;
 @Column
 private Timestamp tanggal;
 public long getId() {
 return id;
 }
 public void setId(long id) {
 this.id = id;
 }
 public Produk getProduk() {
 return produk;
 }
 public void setProduk(Produk produk) {
 this.produk = produk;
 }
 public String getAktivitas() {
 return aktivitas;
 }
 public void setAktivitas(String aktivitas) {
 this.aktivitas = aktivitas;
 }
 public Timestamp getTanggal() {
 return tanggal;
 }
 public void setTanggal(Timestamp tanggal) {
 this.tanggal = tanggal;
 }
}
 {% endcodeblock %}
 Dan selanjutnya adalah membuat listenernya yang akan otomatis melakukan aksi ketika user melakukan CRUD terhadap table Produk. Untuk contoh saya akan membuat listener yang akan menjalankan event setelah user melakukan insert data terhadap table Produk.
{% codeblock lang:java %} package com.mervpolis.dwx.hibernate.event;
import java.sql.Timestamp;
import org.hibernate.Session;
import org.hibernate.event.PostInsertEvent;
import org.hibernate.event.PostInsertEventListener;
import com.mervpolis.dwx.hibernate.entity.CatatanAktivitasProduk;
import com.mervpolis.dwx.hibernate.entity.Produk;
public class ProdukEventListener implements PostInsertEventListener {
 @Override
 public void onPostInsert(PostInsertEvent event) {
 if (event.getEntity() instanceof Produk) {
 CatatanAktivitasProduk aktivitasProduk = new CatatanAktivitasProduk();
 Session session = event.getSession();
 aktivitasProduk.setAktivitas("tambah");
 aktivitasProduk.setProduk((Produk) event.getEntity());
 aktivitasProduk
 .setTanggal(new Timestamp(System.currentTimeMillis()));
 session.saveOrUpdate(aktivitasProduk);
 }
 }
}
 {% endcodeblock %}
 Selanjutnya adalah mendaftarkan listener pada konfigurasi hibernate (hibernate.cfg.xml).
{% codeblock lang:xml %}
<!--?xml version="1.0" encoding="UTF-8"?-->
<hibernate-configuration>
 <session-factory>
 
  <property name="hibernate.connection.driver_class">
   com.mysql.jdbc.Driver
  </property>
  <property name="hibernate.connection.password">
   admin
  </property>
  <property name="hibernate.connection.url">
   jdbc:mysql://localhost:3306/test
  </property>
  <property name="hibernate.connection.username">
   root
  </property>
  <property name="hibernate.dialect">
   org.hibernate.dialect.MySQLDialect
  </property>
  <property name="hibernate.hbm2ddl.auto">
   update
  </property>
  <property name="hibernate.show_sql">
   true
  </property>
  <mapping class="com.mervpolis.dwx.hibernate.entity.Produk" />
  <mapping class="com.mervpolis.dwx.hibernate.entity.CatatanAktivitasProduk" />
  <listener class="com.mervpolis.dwx.hibernate.event.ProdukEventListener" type="post-insert" />
 </session-factory>
</hibernate-configuration>
 {% endcodeblock %}
 Selanjutnya saya akan mencoba menjalankan sebuah kode yang akan melakukan insert data terhadap table produk.
{% codeblock lang:java %} package com.mervpolis.dwx.hibernate;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.AnnotationConfiguration;
import com.mervpolis.dwx.hibernate.entity.Produk;
public class InsertData {
 public static void main(String[] args) {
 SessionFactory factory = new AnnotationConfiguration().configure()
 .buildSessionFactory();
 Session session = factory.openSession();
 Produk produk = new Produk();
 session.getTransaction().begin();
 produk.setNama("Sendal Jepit");
 produk.setHarga(2000);
 produk.setDeskripsi("Sendal yang dijepit");
 session.saveOrUpdate(produk);
 session.getTransaction().commit();
 }
}
 {% endcodeblock %}
 Dan apa yang terjadi ketika kode diatas saya jalankan? Secara otomatis hibernate akan menjalankan perintah yang saya taruh pada listener. Nahlho mana buktinya? Ada baiknya bila anda buktikan sendiri dengan mencoba membuat mekanisme seperti di atas :) selamat mencoba.