title: "Hibernate Event Listener"
date: 2010-09-19 09:45:28
tags:
---

Barusan saya baca-baca tentang database trigger dan sempat coba-coba, malas juga ternyata nulis kodenya yang begitu banyak :p benar-benar programmer yang buruk, hehehehe... Dan seperti apa yang saya inginkan, tuink... sangat tepat, ternyata hibernate juga menyediakan fasilitas yang mirip seperti trigger pada database, yaitu **Hibernate Event**. Sebuah mekanisme yang akan otomatis dijalankan apabila _pemicu_ dipanggil. Untuk penjelasan lebih lanjut tentang event dan trigger bisa lihat di [sini](http://docs.jboss.org/hibernate/stable/core/reference/en/html/events.html#objectstate-events) dan di [sini](http://en.wikipedia.org/wiki/Database_trigger) (lagi-lagi males nulis :p).

Langsung saja ke kasusnya...

Saya mempunyai dua buah entity yaitu Produk dan CatatanAktivitasProduk. Dan saya akan membuat sebuah mekanisme agar hibernate secara otomatis manambahkan baris data pada CatatanAktivitasProduk ketika user melakukan transaksi (CRUD) data terhadap table Produk. Baris data berupa tanggal transaksi, aktivitas (lihat, tambah, perbarui, ataupun hapus), dan id produk yang terpengaruh akibat transaksi yang dilakukan oleh user tersebut.

Pertama berikut entity yang saya butuhkan.

  <pre class="brush: java">package com.mervpolis.dwx.hibernate.entity;

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
  </pre> 
  <pre class="brush: java">package com.mervpolis.dwx.hibernate.entity;

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
  </pre> 

Dan selanjutnya adalah membuat listenernya yang akan otomatis melakukan aksi ketika user melakukan CRUD terhadap table Produk. Untuk contoh saya akan membuat listener yang akan menjalankan event **setelah** user melakukan insert data terhadap table Produk.

  <pre class="brush: java">package com.mervpolis.dwx.hibernate.event;

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
  </pre> 

Selanjutnya adalah mendaftarkan listener pada konfigurasi hibernate (hibernate.cfg.xml).

  <pre class="brush: xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!DOCTYPE hibernate-configuration PUBLIC
		"-//Hibernate/Hibernate Configuration DTD.0//EN"
		"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd"&gt;
&lt;hibernate-configuration&gt;
&lt;session-factory&gt;
&lt;property name="hibernate.connection.driver_class"&gt;com.mysql.jdbc.Driver&lt;/property&gt;
&lt;property name="hibernate.connection.password"&gt;admin&lt;/property&gt;
&lt;property name="hibernate.connection.url"&gt;jdbc:mysql://localhost:3306/test&lt;/property&gt;
&lt;property name="hibernate.connection.username"&gt;root&lt;/property&gt;
&lt;property name="hibernate.dialect"&gt;org.hibernate.dialect.MySQLDialect&lt;/property&gt;
&lt;property name="hibernate.hbm2ddl.auto"&gt;update&lt;/property&gt;
&lt;property name="hibernate.show_sql"&gt;true&lt;/property&gt;

		&lt;mapping class="com.mervpolis.dwx.hibernate.entity.Produk"/&gt;
		&lt;mapping class="com.mervpolis.dwx.hibernate.entity.CatatanAktivitasProduk"/&gt;

		&lt;listener class="com.mervpolis.dwx.hibernate.event.ProdukEventListener" type="post-insert"/&gt;
&lt;/session-factory&gt;
&lt;/hibernate-configuration&gt;
  </pre> 

Selanjutnya saya akan mencoba menjalankan sebuah kode yang akan melakukan insert data terhadap table produk.

  <pre class="brush: java">package com.mervpolis.dwx.hibernate;

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
  </pre> 

Dan apa yang terjadi ketika kode diatas saya jalankan? Secara otomatis hibernate akan menjalankan perintah yang saya taruh pada listener. Nahlho mana buktinya? Ada baiknya bila anda buktikan sendiri dengan mencoba membuat mekanisme seperti di atas :) selamat mencoba.