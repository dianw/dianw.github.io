title: "Cimande 2.0-RC2 Release"
date: 2011-07-14 09:07:56
tags:
---

Hampir tidak terasa sudah satu tahun sejak rencana major release BlueOxygen Workspace (Cimande) versi 2.0 dicetuskan dengan mengusung konsep Representational State Transfer (ReST). Diawali dengan lahirnya versi [Release Candidate 1 (RC1)](http://blogs.mervpolis.com/roller/dwx/entry/cimande_2_0_rc_apa "RC1") akhir tahun lalu, yang juga merupakan awal dari dimulainya mobilisasi terhadap beberapa produk BlueOxygen yang secara keseluruhan berjalan diatas platform Cimande. Dengan dukungan multiple representational yang masih dan terus disempurnakan hingga saat ini diharapkan dalam major release mendatang Blueoxygen Workspace benar-benar menjadi platform yang ramah akan perangkat mobile, baik itu mobile web maupun native (Android, iPhone, Blackberry, dll).

![](https://lh3.googleusercontent.com/-_x0yy3GfIXg/Th3z7hhf7wI/AAAAAAAAAVA/sxTU-Kq9Cgs/s640/243458_10150246861899085_675689084_8768923_3862612_o.jpg)

_Prototype Cimande versi tablet dengan jQuery Mobile SplitView_

Berbeda dengan versi sebelumnya, banyak hal spesial yang terjadi dalam pengembangan RC2 ini yang membuat waktu release sempat mundur dari jadwal yang sudah direncanakan. Sedikit bocoran, banyak diantaranya yang belum dituliskan pada roadmap dan tiba-tiba ada pada RC2 ini. Secara teknis, berikut beberapa perubahan dan fitur terbaru yang telah ditambahkan :

**Full ajax support**

Dengan dukungan ajax secara penuh, semua proses request dan response telah ditangani pada background sehingga membuat aplikasi berjalan lebih cepat. Modul-modul yang berjalan diatas Cimande Platform juga secara otomatis akan terintegrasi dengan fitur ajax ini sehingga aplikasi yang sudah berjalan sebelumnya tidak perlu ditulis ulang untuk menyesuaikan dengan versi Cimande ini. Penggunaan http method diluar POST dan GET dalam tag &lt;form&gt; seperti PUT, HEAD, DELETE juga dimunggkinkan dengan ketentuan browser mendukung XMLHttpRequest.

**Layout dan tema baru**

Setelah hampir satu dekade setia dengan tema lama, dalam rangkaian major release ini Cimande v2 telah berganti kulit dengan tema berlatar biru tua, warna khas BlueOxygen, serta dengan logo Blueoxygen terbaru.

![](https://lh3.googleusercontent.com/-1N-aZkH9GbA/Th3t-zY22ZI/AAAAAAAAAUw/vgnq6NFmRZg/s640/login.png)

![](https://lh6.googleusercontent.com/-vtwqoMZggdk/Th3uDizWtcI/AAAAAAAAAU0/UPWVon8VE0s/s640/workspace.png)

![](https://lh6.googleusercontent.com/-zpBs0hyESig/Th3uIR9_s-I/AAAAAAAAAU4/SLERuPVwoEg/s640/showcase.png)

**Maven Build Tool &amp; Archetype**

Yang paling menarik dari versi 2 ini adalah perubahan build tool yang selama ini menggunakan Ant menjadi Maven dengan plugin dan dependency managementnya yang sangat powerfull. Hal ini sedikit banyak juga telah membuang kesan Cimande yang kurang akur dengan IDE selain Eclipse. Sehingga developer yang akan mengembangkan aplikasi dia atas Platform Cimande dapat memilih menggunakan IDE sesuai dengan selera yang dikehendaki. Saat ini Cimande telah di-host di java.net, dengan Nexus yang telah termirror dengan [Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ccimande) membuat para pengembang cukup menggenerate archetype untuk membuat sebuah workspace yang siap pakai. Langkah untuk melakukan generate archetype berada pada artikel [berikut](http://blogs.mervpolis.com/roller/netoyaOzora/entry/cimande_archetype).

Hingga saat ini tim pengembang masih terus melakukan optimasi untuk relesase berikutnya. 

Hal yang tak pernah lupa untuk disebutkan, BlueOxygen Cimande merupakan produk OpenSource dibawah Lisensi Apache. Seluruh source code berada pada [cimande.java.net](http://cimande.java.net)<!--, dokumentasi produk berada pada link [berikut](http://www.slideshare.net/dwaditya/blueoxygen-cimande-overview) -->.