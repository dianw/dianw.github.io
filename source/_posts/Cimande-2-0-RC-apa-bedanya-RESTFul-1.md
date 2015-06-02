title: "Cimande 2.0 RC, apa bedanya? (RESTFul #1)"
date: 2010-10-18 00:46:47
tags:
---

Setelah berjuang sekitar beberapa bulan terakhir, akhirnya hari ini Cimande v2.0 RC resmi dirilis. Sesuai dengan judul blog saya kali ini, Cimande 2? Apa bedanya dengan yang sudah-sudah? Pada versi terbaru ini Cimande 2.0 menggunakan arsitektur RESTFul web service. Salah satu arsitektur web yang beberapa tahun terakhir ini lagi hot-hotnya, dianggap sebagai arsitektur yang _cool, sederhana_ dan paling _ramah_ bagi user/aplikasi desktop maupun mobile secara umum. Juga sejalan dengan dengan trend baru Web 2.0 yang mulai menjauh dari SOAP dan beralih ke konsep REST.

Sedikit melenceng dari judul, pada bagian pertama ini saya belum akan membahas lebih detail mengenai teknis Cimande2, akan tetapi lebih ke konsep dan prinsip dari REST yang merupakan salah satu arsitektur yang digunakan oleh Cimande2 ini.

Berikut sedikit sejarah singkat tentang arsitektur REST :

> _Representational State Transfer (REST) merupakan arsitektur software untuk distribusi _[_hypermedia_](http://en.wikipedia.org/wiki/Hypermedia)_ diantaranya _[_World Wide Web_](http://en.wikipedia.org/wiki/World_Wide_Web)_. Istilah Representational State Transfer pertama kali diperkenalkan dan didefinisikan pada tahun 2000 oleh _[_Roy T Fielding_](http://en.wikipedia.org/wiki/Roy_Fielding)_ pada disertasi doktoralnya. Fielding merupakan salah satu dari penggagas spesifikasi HTTP 1.0 dan 1.1._

Saya akan menjelaskan secara singkat mengenai beberapa prinsip dasar dari arsitektur REST, diantaranya:

*   <div align="justify">Menggunakan _HTTP dan HTTP Method secara tepat._</div>
*   <div align="justify">Semua tentang_ Resource _dan_ URI_</div>
*   <div align="justify">_Stateless_</div>
*   <div align="justify">Mendukung _multiple representation_.</div>

### 

&#160;

**<font size="3">Menggunakan HTTP dan HTTP Method secara tepat</font>**

Salah satu karakteristik dari RESTFul Web Service adalah menggunakan HTTP, protokol yang digunakan lebih dari 60% browser di seluruh muka bumi ini untuk mengakses server. Ini merupakan salah satu alasan mengapa trend Web 2.0 lebih cenderung menjauhi SOAP, salah satu kutipan yang pernah saya dengar _?Mengapa menggunakan protokol seperti SOAP bila setiap hari sebagian besar penduduk bumi menggunakan HTTP.?_

Selanjutnya adalah penggunaan standard HTTP method diantaranya 

*   <div align="justify">POST untuk membuat sumberdaya (resource) pada server.</div>
*   <div align="justify">GET untuk menerima sumberdaya.</div>
*   <div align="justify">PUT untuk merubah atau memperbaharui sumberdaya; dan</div>
*   <div align="justify">DELETE untuk menghapus sumberdaya.</div>

Menggunakan HTTP method secara tepat?

Berikut dua contoh penggunaan method yang berbeda dengan menghasilhan efek samping yang sama.

> <font face="Courier New" size="3">GET /hapususer?nama=dian HTTP/1.1</font>

Ini sangat tidak dianjurkan karena method GET merupakan safe method yang seharusnya dalam penggunaannya tidak menghasilkan efek samping terhadap server. Dan berikut ini adalah contoh **_penggunaan HTTP method secara tepat_**.

> <font face="Courier New" size="3">DELETE /user/dian HTTP/1.1</font>

Sebagai pedoman umum, diharapkan mengikuti pedoman REST untuk menggunakan HTTP method secara eksplisit dengan menggunakan kata benda dalam URI, bukan kata kerja.

### 

&#160;

**<font size="3">Semua tentang Resource dan URI</font>**

Sumberdaya dapat berupa sebuah informasi maupun deskripsi dari sebuah _item_. Sebagai contoh _blog entry _ataupun _blog author_.

Setiap uri langsung merujuk kepada sumberdaya, tepat sasaran dan mudah ditebak. Bungung? Berikut beberapa contoh uri untuk mengakses _blog entry_ saya:

[http://blogs.mervpolis.com/roller/dwx/entry/hibernate_event_listener](http://blogs.mervpolis.com/roller/dwx/entry/hibernate_event_listener "http://blogs.mervpolis.com/roller/dwx/entry/hibernate_event_listener")

[http://blogs.mervpolis.com/roller/dwx/date/20101006](http://blogs.mervpolis.com/roller/dwx/date/20101006 "http://blog.mervpolis.com/roller/dwx/date/20101006")

[http://blogs.mervpolis.com/roller/dwx/category/Java](http://blogs.mervpolis.com/roller/dwx/category/Java "http://blogs.mervpolis.com/roller/dwx/category/Java")

Contoh URI diatas merupakan contoh penggunaan _directory structure-like URIs. _Beberapa pedoman tambahan dalam penggunaan struktur URI pada RESTFul Web Service adalah sebagai berikut:

*   <div align="justify">Sembunyikan ekstensi file pada _server-side scripting _(.asp, .php, .jsp), jika ada.</div>
*   <div align="justify">Menjaga agar tetap menggunakan _lowercase_.</div>
*   <div align="justify">Mengganti spasi dengan tanda hubung atau garis bawah.</div>
*   <div align="justify">Mencegah query string sebisa mungkin.</div>

&#160;

Cukup sekian untuk saat ini, poin ke 3 dan ke 4 selanjutnya akan saya jabarkan pada RESTFul bagian ke 2.