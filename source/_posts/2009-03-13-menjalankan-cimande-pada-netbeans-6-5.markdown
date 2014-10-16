---
layout: post
title: Menjalankan Cimande Pada NetBeans 6.5
comments: true
---
<font color="green" face="arial" align="justify">
&nbsp;&nbsp;&nbsp;Iseng - iseng kemarin waktu pusing ngerjain project di kantor, eh tiba - tiba terbesit sebuah pikiran "Bisa nggak yach kalo cimande dijalankan di NetBeans??" Berbekal sebuah penasaran besar setelah menemui beberapa error akhirnya sukses juga.<br>
&nbsp;&nbsp;&nbsp;Nah, nggak usah basa-basi lagi, sekarang akan saya jabarkan sedikit eksperimen yang telah saya lakukan tadi .... <br><br>
Pertama - tama Buka NetBeans IDE, kebetulan NetBeans IDE yang saya pakai versi 6.5 (windows) dan cimande 1.3. Selanjutnya buat project baru, lalu pilih kategori "Java Web". Kemudian isi nama project (Cimande), lalu tekan next, pilih server tomcat (dalam hal ini tomcat sudah saya install bersamaan dengan instalasi netbeans yang defaultnya tomcat tidak ikut diinstall). Lalu Tekan Finish. <br><br>
<img src="http://nagasakti.mervpolis.com/roller/dwx/resource/netbeans/newproject.PNG" width="400"><br>
<img src="http://nagasakti.mervpolis.com/roller/dwx/resource/netbeans/server.PNG" width="400"><br><br>
&nbsp;&nbsp;&nbsp;Setelah project selesai dibuat, silakan hapus file "index.jsp" dan yang ada pada folder Web Pages dan folder WEB-INF karena file tersebut merupakan file default dari NetBeans. <br>
&nbsp;&nbsp;&nbsp;Sekarang sejenak kita tinggalkan NetBeansIDE. Masuk ke direktori cimande berada. Copy semua isi dari folder WebContent kemudian paste pada Netbeans dengan click kanan pada Web Pages kemudian pilih menu paste.<br><br>
<img src="http://nagasakti.mervpolis.com/roller/dwx/resource/netbeans/paste.PNG"><br><br>
&nbsp;&nbsp;&nbsp;Setelah selesai lakukan hal yang sama pada folder src/config ke dalam folder Configuration File pada Netbeans. Jangan lupa pula mengisi library dengan click kanan pada folder library lalu pilih "Add JAR/Folder", masukkan *.jar file yang ada pada folder lib.<br><br>
<img src="http://nagasakti.mervpolis.com/roller/dwx/resource/netbeans/addjar.PNG"><br><br>
&nbsp;&nbsp;&nbsp;Dan yang terakhir copy folder classes ke dalam folder WEB-INF pada netbeans.<br><br>
<img src="http://nagasakti.mervpolis.com/roller/dwx/resource/netbeans/classes.PNG"><br><br>
Nah Sekarang waktunya anda mencoba menjalankan project yang telah anda buat tadi.<br><br>
<img src="http://nagasakti.mervpolis.com/roller/dwx/resource/netbeans/output.PNG"><br><br>
Jika proses berjalan seperti pada gambar di atas maka proses berhasil dan NetBeans otomatis akan membuka web browser. Jangan lupa import database agar cimande dapat berjalan. Selamat mencoba <br>
Special Thanks to : Vick
</font>