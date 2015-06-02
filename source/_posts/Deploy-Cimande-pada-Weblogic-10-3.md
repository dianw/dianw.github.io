title: "Deploy Cimande pada Weblogic 10.3"
date: 2011-11-08 02:40:49
tags:
---

Baru hangat-hangat nih, berbarengan dengan Release Blueoxygen Cimande versi 2.0, ada request untuk menjalankannya pada Weblogic Middleware versi 10.3\. Semula saya pikir gak akan jauh beda dengan server-server enterprise lain yang pernah saya coba seperti Glassfish (saja). Dengan bermodal pede (tempe kedele) saya coba jalankan dengan menggunakan Cargo Maven Plugin dengan menambahkan konfigurasi seperti ini:

  <pre class="brush: xml">&lt;plugin&gt;
	&lt;groupId&gt;org.codehaus.cargo&lt;/groupId&gt;
	&lt;artifactId&gt;cargo-maven2-plugin&lt;/artifactId&gt;
	&lt;version&gt;1.1.3&lt;/version&gt;
	&lt;configuration&gt;
		&lt;container&gt;
			&lt;containerId&gt;weblogic103x&lt;/containerId&gt;
			&lt;home&gt;/home/dian/Oracle/Middleware/wlserver_10.3/&lt;/home&gt;
		&lt;/container&gt;
		&lt;cargo.protocol&gt;http&lt;/cargo.protocol&gt;
		&lt;cargo.hostname&gt;localhost&lt;/cargo.hostname&gt;
		&lt;cargo.servlet.port&gt;7001&lt;/cargo.servlet.port&gt;
		&lt;cargo.weblogic.administrator.user&gt;default&lt;/cargo.weblogic.administrator.user&gt;
		&lt;cargo.weblogic.administrator.password&gt;guecakep&lt;/cargo.weblogic.administrator.password&gt;
		&lt;context&gt;cimande&lt;/context&gt;
	&lt;/configuration&gt;
&lt;/plugin&gt;
  </pre> 

Dan benar ketika saya jalankan dengan perintah `**clean verify cargo:run**`, mantra yang tidak diharapkan muncul

  <pre style="overflow-x: scroll; ">Caused By: java.lang.ClassNotFoundException: org.apache.struts2.views.JspSupportServlet
   at weblogic.utils.classloaders.GenericClassLoader.findLocalClass(GenericClassLoader.java:297)
   at weblogic.utils.classloaders.GenericClassLoader.findClass(GenericClassLoader.java:270)
   at weblogic.utils.classloaders.ChangeAwareClassLoader.findClass(ChangeAwareClassLoader.java:64)
   at java.lang.ClassLoader.loadClass(ClassLoader.java:307)
   at java.lang.ClassLoader.loadClass(ClassLoader.java:248)
   at weblogic.utils.classloaders.GenericClassLoader.loadClass(GenericClassLoader.java:179)
   ...
  </pre> 

Yap, class yang diinginkan tidak ditemukan, dan ajaibnya seluruh library yang dibutuhkan (termasuk struts2-core-2.x.jar dimana terdapat class <span style="font-family: 'Dejavu Sans Mono'; "></span> `(org.apache.struts2.views.JspSupportServlet)` sudah ada di folder webapp library (WEB-INF/lib) di dalam WAR yang telah dicompile. Googling punya googling ada sedikit pencerahan, dimana seluruh library yang dibutuhkan oleh webapp harus diletakkan di root EAR project. Ya benar, agak aneh, ada pemaksaan untuk menjadikan project saya menjadi enterprise archive, walaupun didalamnya tidak ada EJB-nya. Sehingga untuk struktur maven-nya pun berubah menjadi seperti ini.

![](https://lh5.googleusercontent.com/-tnGOvOhTY8o/TrgevEbEohI/AAAAAAAAAWE/v5xWfQKgniE/s800/cimande-ee.png "Cimande EE")

Ada satu parent project yang memiliki 2 module yaitu berupa webapp (dengan packaging WAR) dan ear (dengan packaging EAR).

Karena semua library akan diletakkan pada EAR maka untuk menghindari duplikasi file jar, maka ada sedikit perubahan scope cimande-core pada webapp dari compile menjadi provided, artinya library tidak akan ikut disertakan ketika webapp di package menjadi WAR.

  <pre class="brush: xml">&lt;dependency&gt;
	&lt;groupId&gt;org.blueoxygen.cimande&lt;/groupId&gt;
	&lt;artifactId&gt;cimande-core&lt;/artifactId&gt;
	&lt;version&gt;${cimande.sdk.version}&lt;/version&gt;
	&lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;
  </pre> 

Berikut isi dari konfigurasi pom.xml pada parent project

  <pre class="brush: xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"&gt;
	&lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;
	&lt;groupId&gt;org.blueoxygen.cimande&lt;/groupId&gt;
	&lt;version&gt;1.0&lt;/version&gt;
	&lt;artifactId&gt;cimande-enterprise-parent&lt;/artifactId&gt;
	&lt;packaging&gt;pom&lt;/packaging&gt;
	&lt;name&gt;Cimande Enterprise Parent&lt;/name&gt;
	&lt;properties&gt;
		&lt;cimande.sdk.version&gt;2.0&lt;/cimande.sdk.version&gt;
	&lt;/properties&gt;
	&lt;modules&gt;
		&lt;module&gt;ear&lt;/module&gt;
		&lt;module&gt;webapp&lt;/module&gt;
	&lt;/modules&gt;
	&lt;build&gt;
		&lt;pluginManagement&gt;
			&lt;plugins&gt;
				&lt;plugin&gt;
					&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
					&lt;artifactId&gt;maven-site-plugin&lt;/artifactId&gt;
					&lt;configuration&gt;
						&lt;unzipCommand&gt;/usr/bin/unzip -o &gt; err.txt&lt;/unzipCommand&gt;
					&lt;/configuration&gt;
				&lt;/plugin&gt;
			&lt;/plugins&gt;
		&lt;/pluginManagement&gt;
	&lt;/build&gt;
	&lt;dependencies&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.blueoxygen.cimande&lt;/groupId&gt;
			&lt;artifactId&gt;cimande-core&lt;/artifactId&gt;
			&lt;version&gt;${cimande.sdk.version}&lt;/version&gt;
		&lt;/dependency&gt;
	&lt;/dependencies&gt;
&lt;/project&gt;
  </pre> 

Dan Cargo Maven Plugin untuk menjalankan Weblogic saya letakkan pada pom.xml module ear, untuk mendeploy aplikasi dalam bentuk EAR.

  <pre class="brush: xml">&lt;project&gt;
	&lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;
	&lt;groupId&gt;org.blueoxygen.cimande&lt;/groupId&gt;
	&lt;artifactId&gt;cimande-enterprise&lt;/artifactId&gt;
	&lt;packaging&gt;ear&lt;/packaging&gt;
	&lt;version&gt;1.0&lt;/version&gt;
	&lt;name&gt;Cimande Enterprise&lt;/name&gt;
	&lt;parent&gt;
		&lt;groupId&gt;org.blueoxygen.cimande&lt;/groupId&gt;
		&lt;artifactId&gt;cimande-enterprise-parent&lt;/artifactId&gt;
		&lt;version&gt;1.0&lt;/version&gt;
	&lt;/parent&gt;
	&lt;dependencies&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.blueoxygen.cimande&lt;/groupId&gt;
			&lt;artifactId&gt;cimande-webapp&lt;/artifactId&gt;
			&lt;version&gt;1.0&lt;/version&gt;
			&lt;type&gt;war&lt;/type&gt;
		&lt;/dependency&gt;
	&lt;/dependencies&gt;
	&lt;build&gt;
		&lt;plugins&gt;
			&lt;plugin&gt;
				&lt;artifactId&gt;maven-ear-plugin&lt;/artifactId&gt;
				&lt;configuration&gt;
					&lt;generateApplicationXml&gt;true&lt;/generateApplicationXml&gt;
					&lt;defaultLibBundleDir&gt;lib/&lt;/defaultLibBundleDir&gt;
					&lt;archive&gt;
						&lt;manifest&gt;
							&lt;addClasspath&gt;true&lt;/addClasspath&gt;
						&lt;/manifest&gt;
					&lt;/archive&gt;
				&lt;/configuration&gt;
			&lt;/plugin&gt;
			&lt;plugin&gt;
				&lt;groupId&gt;org.codehaus.cargo&lt;/groupId&gt;
				&lt;artifactId&gt;cargo-maven2-plugin&lt;/artifactId&gt;
				&lt;version&gt;1.1.3&lt;/version&gt;
				&lt;configuration&gt;
					&lt;container&gt;
						&lt;containerId&gt;weblogic103x&lt;/containerId&gt;
						&lt;home&gt;/home/dian/Oracle/Middleware/wlserver_10.3/&lt;/home&gt;
					&lt;/container&gt;
					&lt;cargo.protocol&gt;http&lt;/cargo.protocol&gt;
					&lt;cargo.hostname&gt;localhost&lt;/cargo.hostname&gt;
					&lt;cargo.servlet.port&gt;7001&lt;/cargo.servlet.port&gt;
					&lt;cargo.weblogic.administrator.user&gt;default&lt;/cargo.weblogic.administrator.user&gt;
					&lt;cargo.weblogic.administrator.password&gt;tulalit13&lt;/cargo.weblogic.administrator.password&gt;
					&lt;context&gt;cimande&lt;/context&gt;
				&lt;/configuration&gt;
			&lt;/plugin&gt;
		&lt;/plugins&gt;
	&lt;/build&gt;
	&lt;properties&gt;
		&lt;cimande.sdk.version&gt;2.0&lt;/cimande.sdk.version&gt;
	&lt;/properties&gt;
&lt;/project&gt;
  </pre> 

Tak lupa yang terakhir (juga hasil googling) buat file **weblogic-application.xml** pada direktori {ear.home}/src/main/application/META-INF/, yang merupakan file deployment descriptor spesifik Weblogic server untuk application.xml dari Sun Microsystem. Dalam file ini konfigurasi yang dibutuhkan seperti shared library dideskripsikan.

  <pre class="brush: xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;weblogic-application
	xmlns="http://www.bea.com/ns/weblogic/weblogic-application"&gt;
	&lt;application-param&gt;
		&lt;param-name&gt;webapp.encoding.default&lt;/param-name&gt;
		&lt;param-value&gt;UTF-8&lt;/param-value&gt;
	&lt;/application-param&gt;
	&lt;prefer-application-packages&gt;
		&lt;package-name&gt;antlr.*&lt;/package-name&gt;
		&lt;package-name&gt;javax.persistence.*&lt;/package-name&gt;
		&lt;package-name&gt;org.apache.commons.*&lt;/package-name&gt;
		&lt;package-name&gt;org.springframework.*&lt;/package-name&gt;
		&lt;package-name&gt;org.hibernate.*&lt;/package-name&gt;
	&lt;/prefer-application-packages&gt;
&lt;/weblogic-application&gt;
  </pre> 

Dan akhirnya setelah semua mantra yang dituliskan diatas dijalankan satu per satu secara berurutan, maka tibalah saat-saat yang mendebarkan yaitu menjalankan aplikasi. Berikut langkah-langkah untuk menjalankannya.

*   masuk ke dalam direktori parent-project, kemudian masukkan perintah `**mvn clean install**` untuk menginstal ear dan webapp pada repository lokal.
*   masuk ke dalam direktori ear kemudian jalankan perintah mvn cargo:run
*   buka browser, pada contoh konfigurasi diatas Cimande dapat diakses melalui http://localhost:7001/cimande-webapp 

Dan akhirnya dengan modal pede + googling saya berhasil menjalankan BlueOxygen Cimande pada Weblogic Server.

NB: mengingat konfigurasi diatas cukup rumit, tim developer saat ini sedang mempersiapkan Maven archetype untuk mempermudah user mengembangkan Cimande project diatas Weblogic dengan cukup menggenerate archetype yang akan segera tersedia di maven central.