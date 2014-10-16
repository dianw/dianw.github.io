---
layout: post
title: Deploy Cimande pada Weblogic 10.3
comments: true
---

Baru hangat-hangat nih, berbarengan dengan Release Blueoxygen Cimande versi 2.0, ada request untuk menjalankannya pada Weblogic Middleware versi 10.3. Semula saya pikir gak akan jauh beda dengan server-server enterprise lain yang pernah saya coba seperti Glassfish (saja). Dengan bermodal pede (tempe kedele) saya coba jalankan dengan menggunakan Cargo Maven Plugin dengan menambahkan konfigurasi seperti ini:

{% codeblock lang:xml %}
<plugin>
  <groupid>org.codehaus.cargo</groupid>
  <artifactid>cargo-maven2-plugin</artifactid>
  <version>1.1.3</version>
  <configuration>
    <container>
      <containerid>weblogic103x</containerid>
      <home>/home/dian/Oracle/Middleware/wlserver_10.3/</home>
    </container>
    <cargo.protocol>http</cargo.protocol>
    <cargo.hostname>localhost</cargo.hostname>
    <cargo.servlet.port>7001</cargo.servlet.port>
    <cargo.weblogic.administrator.user>default</cargo.weblogic.administrator.user>
    <cargo.weblogic.administrator.password>guecakep</cargo.weblogic.administrator.password>
    <context>cimande</context>
  </configuration>
</plugin>
{% endcodeblock %}

Dan benar ketika saya jalankan dengan perintah clean verify cargo:run, mantra yang tidak diharapkan muncul
```
Caused By: java.lang.ClassNotFoundException: org.apache.struts2.views.JspSupportServlet
   at weblogic.utils.classloaders.GenericClassLoader.findLocalClass(GenericClassLoader.java:297)
   at weblogic.utils.classloaders.GenericClassLoader.findClass(GenericClassLoader.java:270)
   at weblogic.utils.classloaders.ChangeAwareClassLoader.findClass(ChangeAwareClassLoader.java:64)
   at java.lang.ClassLoader.loadClass(ClassLoader.java:307)
   at java.lang.ClassLoader.loadClass(ClassLoader.java:248)
   at weblogic.utils.classloaders.GenericClassLoader.loadClass(GenericClassLoader.java:179)
   ...
```

Yap, class yang diinginkan tidak ditemukan, dan ajaibnya seluruh library yang dibutuhkan (termasuk struts2-core-2.x.jar dimana terdapat class (org.apache.struts2.views.JspSupportServlet) sudah ada di folder webapp library (WEB-INF/lib) di dalam WAR yang telah dicompile. Googling punya googling ada sedikit pencerahan, dimana seluruh library yang dibutuhkan oleh webapp harus diletakkan di root EAR project. Ya benar, agak aneh, ada pemaksaan untuk menjadikan project saya menjadi enterprise archive, walaupun didalamnya tidak ada EJB-nya. Sehingga untuk struktur maven-nya pun berubah menjadi seperti ini.

Ada satu parent project yang memiliki 2 module yaitu berupa webapp (dengan packaging WAR) dan ear (dengan packaging EAR).

Karena semua library akan diletakkan pada EAR maka untuk menghindari duplikasi file jar, maka ada sedikit perubahan scope cimande-core pada webapp dari compile menjadi provided, artinya library tidak akan ikut disertakan ketika webapp di package menjadi WAR.
{% codeblock lang:xml %}
<dependency>
  <groupid>org.blueoxygen.cimande</groupid>
  <artifactid>cimande-core</artifactid>
  <version>${cimande.sdk.version}</version>
  <scope>provided</scope>
</dependency>
{% endcodeblock %}

Berikut isi dari konfigurasi pom.xml pada parent project

{% codeblock lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.blueoxygen.cimande</groupId>
    <version>1.0</version>
    <artifactId>cimande-enterprise-parent</artifactId>
    <packaging>pom</packaging>
    <name>Cimande Enterprise Parent</name>
    <properties>
        <cimande.sdk.version>2.0</cimande.sdk.version>
    </properties>
    <modules>
        <module>ear</module>
        <module>webapp</module>
    </modules>
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-site-plugin</artifactId>
                    <configuration>
                        <unzipCommand>/usr/bin/unzip -o > err.txt</unzipCommand>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
    <dependencies>
        <dependency>
            <groupId>org.blueoxygen.cimande</groupId>
            <artifactId>cimande-core</artifactId>
            <version>${cimande.sdk.version}</version>
        </dependency>
    </dependencies>
</project>
 {% endcodeblock %}


Dan Cargo Maven Plugin untuk menjalankan Weblogic saya letakkan pada pom.xml module ear, untuk mendeploy aplikasi dalam bentuk EAR.

{% codeblock lang:xml %}
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.blueoxygen.cimande</groupId>
    <artifactId>cimande-enterprise</artifactId>
    <packaging>ear</packaging>
    <version>1.0</version>
    <name>Cimande Enterprise</name>
    <parent>
        <groupId>org.blueoxygen.cimande</groupId>
        <artifactId>cimande-enterprise-parent</artifactId>
        <version>1.0</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.blueoxygen.cimande</groupId>
            <artifactId>cimande-webapp</artifactId>
            <version>1.0</version>
            <type>war</type>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-ear-plugin</artifactId>
                <configuration>
                    <generateApplicationXml>true</generateApplicationXml>
                    <defaultLibBundleDir>lib/</defaultLibBundleDir>
                    <archive>
                        <manifest>
                            <addClasspath>true</addClasspath>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.codehaus.cargo</groupId>
                <artifactId>cargo-maven2-plugin</artifactId>
                <version>1.1.3</version>
                <configuration>
                    <container>
                        <containerId>weblogic103x</containerId>
                        <home>/home/dian/Oracle/Middleware/wlserver_10.3/</home>
                    </container>
                    <cargo.protocol>http</cargo.protocol>
                    <cargo.hostname>localhost</cargo.hostname>
                    <cargo.servlet.port>7001</cargo.servlet.port>
                    <cargo.weblogic.administrator.user>default</cargo.weblogic.administrator.user>
                    <cargo.weblogic.administrator.password>tulalit13</cargo.weblogic.administrator.password>
                    <context>cimande</context>
                </configuration>
            </plugin>
        </plugins>
    </build>
    <properties>
        <cimande.sdk.version>2.0</cimande.sdk.version>
    </properties>
</project>
{% endcodeblock %}

Tak lupa yang terakhir (juga hasil googling) buat file weblogic-application.xml pada direktori {ear.home}/src/main/application/META-INF/, yang merupakan file deployment descriptor spesifik Weblogic server untuk application.xml dari Sun Microsystem. Dalam file ini konfigurasi yang dibutuhkan seperti shared library dideskripsikan.

{% codeblock lang:xml %}
<!--?xml version="1.0" encoding="UTF-8"?-->
<?xml version="1.0" encoding="UTF-8"?>
<weblogic-application
    xmlns="http://www.bea.com/ns/weblogic/weblogic-application">
    <application-param>
        <param-name>webapp.encoding.default</param-name>
        <param-value>UTF-8</param-value>
    </application-param>
    <prefer-application-packages>
        <package-name>antlr.*</package-name>
        <package-name>javax.persistence.*</package-name>
        <package-name>org.apache.commons.*</package-name>
        <package-name>org.springframework.*</package-name>
        <package-name>org.hibernate.*</package-name>
    </prefer-application-packages>
</weblogic-application>
{% endcodeblock %}

Dan akhirnya setelah semua mantra yang dituliskan diatas dijalankan satu per satu secara berurutan, maka tibalah saat-saat yang mendebarkan yaitu menjalankan aplikasi. Berikut langkah-langkah untuk menjalankannya.

* Masuk ke dalam direktori parent-project, kemudian masukkan perintah ```mvn clean install``` untuk menginstal ear dan webapp pada repository lokal.
* Masuk ke dalam direktori ear kemudian jalankan perintah ```mvn cargo:run```
* Muka browser, pada contoh konfigurasi diatas Cimande dapat diakses melalui http://localhost:7001/cimande-webapp

Dan akhirnya dengan modal pede + googling saya berhasil menjalankan BlueOxygen Cimande pada Weblogic Server.

NB: mengingat konfigurasi diatas cukup rumit, tim developer saat ini sedang mempersiapkan Maven archetype untuk mempermudah user mengembangkan Cimande project diatas Weblogic dengan cukup menggenerate archetype yang akan segera tersedia di maven central.
