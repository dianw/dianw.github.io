---
layout: post
title: 'Cimande 2.0 RC, apa bedanya? (RESTFul #1)'
comments: true
---
Setelah berjuang sekitar beberapa bulan terakhir, akhirnya hari ini Cimande v2.0 RC resmi dirilis. Sesuai dengan judul blog saya kali ini, Cimande 2? Apa bedanya dengan yang sudah-sudah? Pada versi terbaru ini Cimande 2.0 menggunakan arsitektur RESTFul web service. Salah satu arsitektur web yang beberapa tahun terakhir ini lagi hot-hotnya, dianggap sebagai arsitektur yang cool, sederhana dan paling ramah bagi user/aplikasi desktop maupun mobile secara umum. Juga sejalan dengan dengan trend baru Web 2.0 yang mulai menjauh dari SOAP dan beralih ke konsep REST.
Sedikit melenceng dari judul, pada bagian pertama ini saya belum akan membahas lebih detail mengenai teknis Cimande2, akan tetapi lebih ke konsep dan prinsip dari REST yang merupakan salah satu arsitektur yang digunakan oleh Cimande2 ini.
Berikut sedikit sejarah singkat tentang arsitektur REST :
<blockquote>
 Representational State Transfer (REST) merupakan arsitektur software untuk distribusi hypermedia diantaranya World Wide Web. Istilah Representational State Transfer pertama kali diperkenalkan dan didefinisikan pada tahun 2000 oleh Roy T Fielding pada disertasi doktoralnya. Fielding merupakan salah satu dari penggagas spesifikasi HTTP 1.0 dan 1.1.
</blockquote>
Saya akan menjelaskan secara singkat mengenai beberapa prinsip dasar dari arsitektur REST, diantaranya:
<ul>
 <li>
  <div align="justify">
  
 Menggunakan
   <em>HTTP dan HTTP Method secara tepat.</em>
  </div>
 </li>
 <li>
  <div align="justify">
  
 Semua tentang
   <em> Resource </em>dan
   <em> URI</em>
  </div>
 </li>
 <li>
  <div align="justify">
  
   <em>Stateless</em>
  </div>
 </li>
 <li>
  <div align="justify">
  
 Mendukung
   <em>multiple representation</em>.
  </div>
 </li>
</ul>
###
&nbsp;
Menggunakan HTTP dan HTTP Method secara tepat
Salah satu karakteristik dari RESTFul Web Service adalah menggunakan HTTP, protokol yang digunakan lebih dari 60% browser di seluruh muka bumi ini untuk mengakses server. Ini merupakan salah satu alasan mengapa trend Web 2.0 lebih cenderung menjauhi SOAP, salah satu kutipan yang pernah saya dengar ?Mengapa menggunakan protokol seperti SOAP bila setiap hari sebagian besar penduduk bumi menggunakan HTTP.?
Selanjutnya adalah penggunaan standard HTTP method diantaranya
<ul>
 <li>
  <div align="justify">
  
 POST untuk membuat sumberdaya (resource) pada server.
  </div>
 </li>
 <li>
  <div align="justify">
  
 GET untuk menerima sumberdaya.
  </div>
 </li>
 <li>
  <div align="justify">
  
 PUT untuk merubah atau memperbaharui sumberdaya; dan
  </div>
 </li>
 <li>
  <div align="justify">
  
 DELETE untuk menghapus sumberdaya.
  </div>
 </li>
</ul>
Menggunakan HTTP method secara tepat?
Berikut dua contoh penggunaan method yang berbeda dengan menghasilhan efek samping yang sama.
<blockquote>
 GET /hapususer?nama=dian HTTP/1.1
</blockquote>
Ini sangat tidak dianjurkan karena method GET merupakan safe method yang seharusnya dalam penggunaannya tidak menghasilkan efek samping terhadap server. Dan berikut ini adalah contoh penggunaan HTTP method secara tepat.
<blockquote>
 DELETE /user/dian HTTP/1.1
</blockquote>
Sebagai pedoman umum, diharapkan mengikuti pedoman REST untuk menggunakan HTTP method secara eksplisit dengan menggunakan kata benda dalam URI, bukan kata kerja.
###
&nbsp;
Semua tentang Resource dan URI
Sumberdaya dapat berupa sebuah informasi maupun deskripsi dari sebuah item. Sebagai contoh blog entry ataupun blog author.
Setiap uri langsung merujuk kepada sumberdaya, tepat sasaran dan mudah ditebak. Bungung? Berikut beberapa contoh uri untuk mengakses blog entry saya:
http://blogs.mervpolis.com/roller/dwx/entry/hibernate_event_listener
http://blogs.mervpolis.com/roller/dwx/date/20101006
http://blogs.mervpolis.com/roller/dwx/category/Java
Contoh URI diatas merupakan contoh penggunaan directory structure-like URIs. Beberapa pedoman tambahan dalam penggunaan struktur URI pada RESTFul Web Service adalah sebagai berikut:
<ul>
 <li>
  <div align="justify">
  
 Sembunyikan ekstensi file pada
   <em>server-side scripting </em>(.asp, .php, .jsp), jika ada.
  </div>
 </li>
 <li>
  <div align="justify">
  
 Menjaga agar tetap menggunakan
   <em>lowercase</em>.
  </div>
 </li>
 <li>
  <div align="justify">
  
 Mengganti spasi dengan tanda hubung atau garis bawah.
  </div>
 </li>
 <li>
  <div align="justify">
  
 Mencegah query string sebisa mungkin.
  </div>
 </li>
</ul>
&nbsp;
Cukup sekian untuk saat ini, poin ke 3 dan ke 4 selanjutnya akan saya jabarkan pada RESTFul bagian ke 2.