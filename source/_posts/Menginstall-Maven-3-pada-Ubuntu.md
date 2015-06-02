title: "Menginstall Maven 3 pada Ubuntu"
date: 2011-10-07 14:30:31
tags:
---

Sekedar mengembalikan postingan yang sempat hilang ditelan bumi :D

**Posted on August 03, 2011 by Dian Aditya**

Hari ini saya telah berhasil mengupgrade versi Maven pada ubuntu 10.04 saya dari versi 2.x ke versi 3.x. Berbeda dengan versi sebelumnya, Maven 3 tidak tersedia pada paket update Ubuntu, sehingga cara instalasinya berbeda pula dengan Maven 2.

Ilustrasi

    <pre>$sudo apt-get install maven3</pre> 

Mungkin akan terasa nikmat jika perintah diatas berjalan dengan mulus :D

Ok, saya rasa semuanya sudah mengerti. Berikut proses instalasi Maven 3 pada Ubuntu secara **manual**. Sebelumnya pastikan java telah terinstal terlebih dahulu.

1.  Download paket Maven 3 [di sini.](%0Ahttp://apache.the.net.id//maven/binaries/apache-maven-3.0.3-bin.tar.gz "Maven 3") Kemudian extract paket yang sudah didownload.
Pada kasus ini paket saya extract pada `/opt`

      <pre>$tar -xzvf apache-maven-3.0.3-bin.tar.gz</pre>2.  Tambahkan PATH variable pada environtment file.
      <pre>$sudo gedit /etc/environment</pre>
      <pre>
JAVA_HOME=&quot;/opt/jdk1.6.0_21&quot;
M3_HOME=&quot;/opt/apache-maven-3.0.3&quot;
MAVEN_HOME=&quot;/opt/apache-maven-3.0.3&quot;
M3=&quot;/opt/apache-maven-3.0.3/bin&quot;
</pre>3.  Rubah PATH, biasanya seperti ini

      <pre>PATH=&quot;/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games&quot;</pre>

Menjadi seperti berikut
      <pre>PATH=&quot;/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/opt/apache-maven-3.0.3/bin&quot;</pre>4.  Logout 

Setelah proses diata selesai, login kembali, kemudian lakukan cek versi maven.

    <pre>$mvn -version</pre> 

jika berhasil seharusnya konsol mengembalikan hasil seperti berikut

  <pre>
Apache Maven 3.0.3 (r1075438; 2011-03-01 00:31:09+0700)
Maven home: /opt/apache-maven-3.0.3
Java version: 1.6.0_21, vendor: Sun Microsystems Inc.
Java home: /opt/jdk1.6.0_21/jre
Default locale: en_US, platform encoding: UTF-8
OS name: &quot;linux&quot;, version: &quot;2.6.32-33-generic&quot;, arch: &quot;i386&quot;, family: &quot;unix&quot;
</pre>