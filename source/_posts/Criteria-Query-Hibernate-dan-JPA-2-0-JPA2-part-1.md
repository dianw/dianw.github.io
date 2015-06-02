title: "Criteria Query, Hibernate dan JPA 2.0 (JPA2 part 1)"
date: 2011-11-12 04:27:57
tags:
---

Salah satu tambahan terbesar dari spesifikasi JPA (Java Persistence API) versi 2.0 adalah criteria query. Dirilis pada Desember 2009, JPA 2.0 menyediakan API baru yang memungkinkan penggunaan criteria query, meskipun kita tahu pada framework ORM seperti Hibernate sudah terlebih dahulu menyediakan criteria query sejak versi 3.0\. Hmm, bagaimanapun JPA adalah spesifikasi standard untuk Persistence yang mana dengan JPA kita bisa memilih JPA provider seperti Hibernate, EclipseLink, OpenJPA, DataNucleus, ataupun ObjectDB sesuka hati tanpa perlu mengganti banyak konfigurasi karena seluruh abstraksi ada di bawah package javax.persistence.*

Pada postingan kali ini saya akan mengulas tentang penggunaan Criteria Query pada JPA (dengan Hibernate sebagai persistence provider) dan Qriteria Query pada Hibernate (native).

Sedikit berbeda dengan JPA 1.0, walaupun diklaim sebagai vendor-independent, kita tidak akan menemukan **javax.persistence:javax.persistence:2.0** pada Maven Central, melainkan kita harus mencarinya pada masing-masing ORM vendor. Sebagai contoh jika kita menggunakan hibernate maka artifact untuk JPA 2.0 adalah

  <pre class="brush: xml">&lt;dependency&gt;
	&lt;groupId&gt;org.hibernate.javax.persistence&lt;/groupId&gt;
	&lt;artifactId&gt;hibernate-jpa-2.0-api&lt;/artifactId&gt;
	&lt;version&gt;1.0.0.Final&lt;/version&gt;
	&lt;type&gt;jar&lt;/type&gt;
	&lt;scope&gt;compile&lt;/scope&gt;
&lt;/dependency&gt;
</pre> 

Oke marilah langsung saja kita menuju ke area perangnya, dimisalkan saya memiliki sebuah entity (karyawan).

  <pre class="brush: java">@Entity
@Table(name = "pegawai")
public class Pegawai {
	private long id;
	private String nama;
	private String telepon;
	private String alamat;

	// getter dan setter
}
</pre> 

Selanjutnya mari membuat sebuah JPA Query untuk menangani sebuah pencarian berdasarkan nama atau alamat pegawai.

  <pre class="brush: sql">SELECT p FROM Pegawai WHERE p.nama LIKE :nama OR p.alamat LIKE :alamat
</pre> 

<span style="font-size: large; ">**Criteria Query**</span>

**Hibernate**

Citeria query pada hibernate umunya cukup mudah, kita tinggal mendefinisikan class entity pada saat pembuatan criteria, kemudian memasukkan masing-masing ekspresi menggunakan class `org.hibernate.Criterion`.

  <pre class="brush: java">public List&lt;Pegawai&gt; getKaryawanByCriteriaH(String nama, String alamat) {
	Session session = sessionFactory.getCurrentSession();

	Criteria criteria = session.createCriteria(Pegawai.class);
	criteria.add(Restrictions.like("nama", nama));
	criteria.add(Restrictions.like("alamat", alamat));

	return criteria.list();
}
</pre> 

**JPA**

  <pre class="brush: java">public List&lt;Pegawai&gt; getKaryawanByCriteria(String nama, String alamat) {
	CriteriaBuilder builder = entityManager.getCriteriaBuilder();
	CriteriaQuery&lt;Pegawai&gt; query = builder.createQuery(Pegawai.class);

	Root&lt;Pegawai&gt; root = query.from(Pegawai.class);
	query.where(builder.like(root.&lt;String&gt; get("nama"), nama),
			builder.like(root.&lt;String&gt; get("alamat"), alamat));

	return entityManager.createQuery(query).getResultList();
}
</pre> 

Sekilas tidak jauh berbeda citeria query pada Hibernate dengan JPA, akan tetapi sedikit yang terlihat jelas spesifikasi JPA sedikit lebih kompleks (baca: njelimet) dibandingkan criteria query pada Hibernate.

<span style="font-size: large; ">**JPA Metamodel**</span>

Criteria Query pada JPA berbasiskan metamodel dari class yang telah terdaftar pada Persistence Unit. JPA 2 mendefinisikan typesafe Criteria API baru yang mengizinkan criteria query dibangun secara rapi/kuat. Penggunaan meta model pada JPA dan beberapa metode otomasi pembuatannya akan saya bahas pada postingan selanjutnya.