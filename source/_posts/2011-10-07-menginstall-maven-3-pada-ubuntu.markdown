---
layout: post
title: Menginstall Maven 3 pada Ubuntu
comments: true
---
Sekedar mengembalikan postingan yang sempat hilang ditelan bumi :D
 Posted on August 03, 2011 by Dian Aditya
 Hari ini saya telah berhasil mengupgrade versi Maven pada ubuntu 10.04 saya dari versi 2.x ke versi 3.x. Berbeda dengan versi sebelumnya, Maven 3 tidak tersedia pada paket update Ubuntu, sehingga cara instalasinya berbeda pula dengan Maven 2.
 Ilustrasi
<pre>$sudo apt-get install maven3</pre>
 Mungkin akan terasa nikmat jika perintah diatas berjalan dengan mulus :D
 Ok, saya rasa semuanya sudah mengerti. Berikut proses instalasi Maven 3 pada Ubuntu secara manual. Sebelumnya pastikan java telah terinstal terlebih dahulu.
<ol>
 <li>Download paket Maven 3 <a href="%0Ahttp://apache.the.net.id//maven/binaries/apache-maven-3.0.3-bin.tar.gz" title="Maven 3">di sini.</a> Kemudian extract paket yang sudah didownload. <br>
Pada kasus ini paket saya extract pada <code>/opt</code></li>
 <pre>$tar -xzvf apache-maven-3.0.3-bin.tar.gz</pre>
 <li>Tambahkan PATH variable pada environtment file. </li>
 <pre>$sudo gedit /etc/environment</pre>
 <pre>
JAVA_HOME="/opt/jdk1.6.0_21"
M3_HOME="/opt/apache-maven-3.0.3"
MAVEN_HOME="/opt/apache-maven-3.0.3"
M3="/opt/apache-maven-3.0.3/bin"
</pre>
 <li>Rubah PATH, biasanya seperti ini</li>
 <pre>PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games"</pre>
 Menjadi seperti berikut
 <pre>PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/opt/apache-maven-3.0.3/bin"</pre>
 <li>Logout</li>
</ol>
 Setelah proses diata selesai, login kembali, kemudian lakukan cek versi maven.
<pre>$mvn -version</pre>
 jika berhasil seharusnya konsol mengembalikan hasil seperti berikut
<pre>
Apache Maven 3.0.3 (r1075438; 2011-03-01 00:31:09+0700)
Maven home: /opt/apache-maven-3.0.3
Java version: 1.6.0_21, vendor: Sun Microsystems Inc.
Java home: /opt/jdk1.6.0_21/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "2.6.32-33-generic", arch: "i386", family: "unix"
</pre>