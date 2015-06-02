title: "Pakai Application Server yang Mana? (Ala Saya)"
date: 2012-06-28 23:35:10
tags:
---

<div style="text-align: justify; "> 

 Setelah sekian abad tidak melihat kalender, baru sadar ini umur ternyata dah nambah banyak. Eeeaaaa, tidak terasa sudah dua tahun lebih saya terlibat dari satu projek ke projek, bergelantungan mondar-mandir. Yah tentunya sudah lumayan juga jumlah aplikasi yang telah terdeploy ke berbagai tempat dengan beragam jenis application server. Mulai dari aplikasi yang mengharuskan berjalan diatas JavaEE container hingga stack yang diluar standart JEE seperti Cimande dengan Struts2-Spring-Hibernate yang pastinya bisa jalan di application server manapun (dengan tambahan telaten dan istiqomah untuk beberapa spesifik application server).

    <div> 

Yang akan saya bahas kali ini adalah beberapa pertimbangan (pribadi) tentang bagaimana memilih application server yang tepat untuk digunakan pada fase production. Sebagai permulaan ada 2 jenis kelompok besar dalam aplication server, kelompok pertama adalah full-blown Java EE container, sedangkan kelompompok ke-dua adalah servlet container. Kelompok pertama adalah dihuni oleh app server yang mendukung spesifikasi Java EE seperti EJB, JMS, JSF, dll, beberapa anggota kelompok ini adalah Glassfish, JBoss AS, Weblogic, Websphere, dan masih banyak lagi. Sedangkan kelompok ke-dua adalah app server yang tidak mendukung secara penuh spesifikasi JEE seperti Tomcat dan Jetty. Sudah dapat gambaran tentang dua kelompok besar tersebut? Oke, sekarang saya akan pilah-pilah sesuai dengan kemampuan masing-masih application server.

### Mandat dari client

Yang satu ini tidak bisa di ganggu gugat, kebanyakan client memang sudah bekerja sama dengan vendor-vendor kesayangan mereka termasuk urusan application server. Jadi bila kondisi seperti ini terjadi maka aplikasi-lah yang akan saya jadikan prioritas utama agar kompatibel dengan spesifik application server. Application server dalam kasus ini biasanya didominasi oleh produk vendor propietary seperti WLS dan WAS.

### Perlukah menggunakan teknologi Java EE?

Kalau boleh jujur saya punya pengalaman yang kurang baik dengan EJB (J2EE 1.4 waktu itu) yang diimplementasikan dalam aplikasi yang tidak terlalu besar (curhat). Ada yang bilang sih Java EE 6 sudah lebih baik. Pertimbangan selanjutnya adalah spesifikasi JEE yang lain seperti JMS, JSF, atau mungkin ESB, jika memang diperlukan saya akan mengutamakan app server opensource terlebih dahulu, dua favorit saya jatuh pada Glassfish dan JBossAS. Dua container tersebut memiliki komunitas yang cukup besar, tidak jarang saya menemukan penyelesaian suatu masalah cukup dengan mengetik keyword di google, bahkan tanpa perlu bertanya pada mailing list komunitasnya. Glassfish 3 sudah mendukung spesifikasi JEE 6, salah satu yang saya suka adalah fitur admin consolenya yang cukup lengkap, dan juga servlet engine-nya yang konon adalah fork dari Tomcat. JBossAS 7 cukup mengesankan dengan startup-nya yang sangat cepat termasuk admin consolenya yang sudah menggunakan ajax, walaupun menurut saya belum sekeren Glassfish punya. Dulu saya pernah berfikir setiap aplikasi yang bisa jalan di Tomcat pasti bisa di deploy di JEE container manapun, semuanya memang terbukti sebelum akhirnya saya bertemu dengan Weblogic, jadi mungkin tanpa alasan pertama saya sebisa mungkin akan menghindari app server yang satu ini.

### Cukup dengan servlet container?

Saya jarang ambil pusing bila diharuskan mendeploy aplikasi dibawah platform Cimande ataupun Yama. Ya, pada kenyataannya kedua framework kebanggaan ini bisa berjalan diatas servlet container saja. Beberapa project yang saya kerjakan dideploy diatas Tomcat. Jetty? Jujur saja saya belum pernah mencoba Jetty untuk kebutuhan production, namun untuk kebutuhan servlet container saya rasa Tomcat ataupun Jetty, tidak ada pilihan yang salah diantara kedua produk tersebut.

### Kebutuhan hit tinggi tanpa perlu banyak setting

Kalau yang satu ini saya belajar banyak dari roller, yap tempat saya posting blog ini yang hampir setahun lalu bermigrasi dengan mulus dari Tomcat6 ke Glassfish3\. Setelah migrasi ke Glassfish beberapa problem seperti timeout yang rutin setiap kali terjadi hit tinggi berangsur menghilang. Apakah artinya Tomcat tidak lebih tangguh Glassfish? Tentunya tidak pula, tetapi untuk konfigurasi minim (walaupun naif jika harus membandingkan keduanya) saya lebih memilih aman dengan menggunakan Glassfish.

Seluruh isi dari artikel ini merupakan opini subjektif berdasarkan pengalaman saya, apapun keputusan untuk menggunakan application server manapun dengan suatu alasan tertentu adalah menjadi hak masing-masih individu.

    </div> 
  </div>