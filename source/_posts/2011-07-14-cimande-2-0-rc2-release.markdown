---
layout: post
title: Cimande 2.0-RC2 Release
comments: true
---
Hampir tidak terasa sudah satu tahun sejak rencana major release BlueOxygen Workspace (Cimande) versi 2.0 dicetuskan dengan mengusung konsep Representational State Transfer (ReST). Diawali dengan lahirnya versi Release Candidate 1 (RC1) akhir tahun lalu, yang juga merupakan awal dari dimulainya mobilisasi terhadap beberapa produk BlueOxygen yang secara keseluruhan berjalan diatas platform Cimande. Dengan dukungan multiple representational yang masih dan terus disempurnakan hingga saat ini diharapkan dalam major release mendatang Blueoxygen Workspace benar-benar menjadi platform yang ramah akan perangkat mobile, baik itu mobile web maupun native (Android, iPhone, Blackberry, dll).
Prototype Cimande versi tablet dengan jQuery Mobile SplitView
Berbeda dengan versi sebelumnya, banyak hal spesial yang terjadi dalam pengembangan RC2 ini yang membuat waktu release sempat mundur dari jadwal yang sudah direncanakan. Sedikit bocoran, banyak diantaranya yang belum dituliskan pada roadmap dan tiba-tiba ada pada RC2 ini. Secara teknis, berikut beberapa perubahan dan fitur terbaru yang telah ditambahkan :
Full ajax support
Dengan dukungan ajax secara penuh, semua proses request dan response telah ditangani pada background sehingga membuat aplikasi berjalan lebih cepat. Modul-modul yang berjalan diatas Cimande Platform juga secara otomatis akan terintegrasi dengan fitur ajax ini sehingga aplikasi yang sudah berjalan sebelumnya tidak perlu ditulis ulang untuk menyesuaikan dengan versi Cimande ini. Penggunaan http method diluar POST dan GET dalam tag
<form>
 seperti PUT, HEAD, DELETE juga dimunggkinkan dengan ketentuan browser mendukung XMLHttpRequest.
</form>
Layout dan tema baru
Setelah hampir satu dekade setia dengan tema lama, dalam rangkaian major release ini Cimande v2 telah berganti kulit dengan tema berlatar biru tua, warna khas BlueOxygen, serta dengan logo Blueoxygen terbaru.
Maven Build Tool &amp; Archetype
Yang paling menarik dari versi 2 ini adalah perubahan build tool yang selama ini menggunakan Ant menjadi Maven dengan plugin dan dependency managementnya yang sangat powerfull. Hal ini sedikit banyak juga telah membuang kesan Cimande yang kurang akur dengan IDE selain Eclipse. Sehingga developer yang akan mengembangkan aplikasi dia atas Platform Cimande dapat memilih menggunakan IDE sesuai dengan selera yang dikehendaki. Saat ini Cimande telah di-host di java.net, dengan Nexus yang telah termirror dengan Maven Central membuat para pengembang cukup menggenerate archetype untuk membuat sebuah workspace yang siap pakai. Langkah untuk melakukan generate archetype berada pada artikel berikut.
Hingga saat ini tim pengembang masih terus melakukan optimasi untuk relesase berikutnya.
Hal yang tak pernah lupa untuk disebutkan, BlueOxygen Cimande merupakan produk OpenSource dibawah Lisensi Apache. Seluruh source code berada pada cimande.java.net.