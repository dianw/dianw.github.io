---
layout: post
title: 'Menggunakan Spring Annotation (Struts2-Spring #2)'
comments: true
---
Ini lanjutan dari blog saya yang kemarin tentang Integrasi Struts2 dengan Spring Framework, dan yang ingin saya sharing kali ini gak akan jauh berbeda dari topik kemarin, yaitu tentang salah satu fitur dari Spring Framework yang sudah ada sejak versi 2.5 yaitu annotation. Dalam hal ini saya akan mencontohkan bagaimana melakukan autowiring terhadap DataSource (gak jauh dari yang kemarin, cuma ganti jadi anotasi doank).
Kali ini saya membuat kasus (lohloh bikin kasus) dimana saya memiliki sebuah Data Access Object (DAO) untuk melakukan transaksi database, dan DAO ini membutuhkan DataSource untuk dapat melakukan tugasnya yang akan digunakan oleh Controller untuk menyampaikannya kepada Viewer Layer. Hehehehe?. Saya juga bingung gimana menggambarkannya.
Langsung berangkat ke TKP.
Berikut sedikit modifikasi dari applicationContext-jdbc.xml
<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  1: <span style="color: #0000ff">&lt;?</span>xml version="1.0" encoding="UTF-8"<span style="color: #0000ff">?&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  2: <span style="color: #0000ff">&lt;</span><span style="color: #800000">beans</span> <span style="color: #ff0000">xmlns</span>=<span style="color: #0000ff">"http://www.springframework.org/schema/beans"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  3: 	<span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">xsi</span>=<span style="color: #0000ff">"http://www.w3.org/2001/XMLSchema-instance"</span> <span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">context</span>=<span style="color: #0000ff">"http://www.springframework.org/schema/context"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  4: 	<span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">p</span>=<span style="color: #0000ff">"http://www.springframework.org/schema/p"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  5: 	<span style="color: #ff0000">xsi</span>:<span style="color: #ff0000">schemaLocation</span>=<span style="color: #0000ff">"http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
</span></pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  6: 		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  7:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #ebebeb">  8: 	<span style="color: #0000ff">&lt;</span><span style="color: #c71585">context</span>:<span style="color: #800000">component</span>-<span style="color: #ff0000">scan</span> <span style="color: #ff0000">base</span>-<span style="color: #ff0000">package</span>=<span style="color: #0000ff">"com.mervpolis.dwx"</span> <span style="color: #0000ff">/&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  9:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 10: 	<span style="color: #0000ff">&lt;</span><span style="color: #800000">bean</span> <span style="color: #ff0000">id</span>=<span style="color: #0000ff">"dataSource"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 11: 		<span style="color: #ff0000">class</span>=<span style="color: #0000ff">"org.springframework.jdbc.datasource.DriverManagerDataSource"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 12: 		<span style="color: #ff0000">p</span>:<span style="color: #ff0000">driverClassName</span>=<span style="color: #0000ff">"com.mysql.jdbc.Driver"</span> <span style="color: #ff0000">p</span>:<span style="color: #ff0000">username</span>=<span style="color: #0000ff">"root"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 13: 		<span style="color: #ff0000">p</span>:<span style="color: #ff0000">password</span>=<span style="color: #0000ff">"admin"</span> <span style="color: #ff0000">p</span>:<span style="color: #ff0000">url</span>=<span style="color: #0000ff">"jdbc:mysql://localhost:3306/test"</span> <span style="color: #0000ff">/&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 14:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 15: <span style="color: #0000ff">&lt;/</span><span style="color: #800000">beans</span><span style="color: #0000ff">&gt;</span></pre></pre>
Nah ada satu baris mantra yang ditambahkan di situ yang berfungsi untuk melakukan scan terhadap semua class yang mengandung anotasi seperti @Autowired @Service @Component dll.
ItemDao.java
<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  1: <span style="color: #0000ff">package</span> com.mervpolis.dwx.struts2spring.dao;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  2:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  3: <span style="color: #0000ff">import</span> java.sql.ResultSet;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  4:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  5: <span style="color: #0000ff">public</span> <span style="color: #0000ff">interface</span> ItemDao {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  6: 	<span style="color: #0000ff">void</span> saveItem(String name, String price);
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  7:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  8: 	ResultSet getAllItems();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  9: }</pre></pre>
Berikut implementasinya
<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  1: <span style="color: #0000ff">package</span> com.mervpolis.dwx.struts2spring.dao;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  2:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  3: <span style="color: #0000ff">import</span> java.sql.Connection;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  4: <span style="color: #0000ff">import</span> java.sql.PreparedStatement;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  5: <span style="color: #0000ff">import</span> java.sql.ResultSet;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  6: <span style="color: #0000ff">import</span> java.sql.SQLException;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  7:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  8: <span style="color: #0000ff">import</span> javax.sql.DataSource;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  9:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 10: <span style="color: #0000ff">import</span> org.springframework.beans.factory.annotation.Autowired;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 11: <span style="color: #0000ff">import</span> org.springframework.stereotype.Service;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 12:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 13: <span style="color: #008000">/**
</span></pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 14:  * Sama dengan
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 15:  * &lt;code&gt;&amp;lt;bean id="itemDao" class="com.mervpolis.dwx.struts2spring.dao.ItemDaoImpl"&amp;gt;&lt;/code&gt;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 16:  *
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 17:  */
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 18: @Service
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 19: <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> ItemDaoImpl <span style="color: #0000ff">implements</span> ItemDao {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 20:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 21: 	<span style="color: #008000">/**
</span></pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 22: 	 * Nah setternya diganti dengan yang satu ini
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 23: 	 */
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 24: 	@Autowired
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 25: 	<span style="color: #0000ff">private</span> DataSource dataSource;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 26:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 27: 	<span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> saveItem(String name, String price) {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 28: 		<span style="color: #0000ff">try</span> {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 29: 			Connection connection = dataSource.getConnection();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 30:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 31: 			String sql = "<span style="color: #8b0000">INSERT INTO item (name, price) VALUES (?, ?)</span>";
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 32: 			PreparedStatement statement = connection.prepareStatement(sql);
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 33: 			statement.setString(1, name);
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 34: 			statement.setString(2, price);
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 35:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 36: 			statement.executeUpdate();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 37: 		} <span style="color: #0000ff">catch</span> (SQLException e) {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 38: 			e.printStackTrace();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 39: 		}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 40: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 41:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 42: 	<span style="color: #0000ff">public</span> ResultSet getAllItems() {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 43: 		<span style="color: #0000ff">try</span> {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 44: 			Connection connection = dataSource.getConnection();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 45:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 46: 			ResultSet resultSet = connection.prepareStatement(
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 47: 					"<span style="color: #8b0000">SELECT * FROM item</span>").executeQuery();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 48:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 49: 			<span style="color: #0000ff">return</span> resultSet;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 50: 		} <span style="color: #0000ff">catch</span> (SQLException e) {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 51: 			e.printStackTrace();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 52:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 53: 			<span style="color: #0000ff">return</span> <span style="color: #0000ff">null</span>;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 54: 		}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 55: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 56:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 57: }
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 58: </pre></pre>
Sudah mulai terlihat bedanya, untuk melakukan injection sudah tidak menggunakan setter lagi, tetapi dengan menggunakan @Autowired, sedangkan untuk pembuatan bean-nya digunakan @Service.
Sekarang tingga; menyuntikkan DAOnya ke controller
<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  1: <span style="color: #0000ff">package</span> com.mervpolis.dwx.struts2spring.action;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  2:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  3: <span style="color: #0000ff">import</span> java.sql.Connection;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  4: <span style="color: #0000ff">import</span> java.sql.ResultSet;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  5: <span style="color: #0000ff">import</span> java.util.ArrayList;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  6: <span style="color: #0000ff">import</span> java.util.HashMap;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  7: <span style="color: #0000ff">import</span> java.util.List;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  8: <span style="color: #0000ff">import</span> java.util.Map;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  9:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 10: <span style="color: #0000ff">import</span> javax.sql.DataSource;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 11:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 12: <span style="color: #0000ff">import</span> org.apache.struts2.convention.annotation.Action;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 13: <span style="color: #0000ff">import</span> org.apache.struts2.convention.annotation.Result;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 14: <span style="color: #0000ff">import</span> org.apache.struts2.convention.annotation.Results;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 15: <span style="color: #0000ff">import</span> org.springframework.beans.factory.annotation.Autowired;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 16:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 17: <span style="color: #0000ff">import</span> com.mervpolis.dwx.struts2spring.dao.ItemDao;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 18: <span style="color: #0000ff">import</span> com.mervpolis.dwx.struts2spring.datasource.DataSourceAware;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 19: <span style="color: #0000ff">import</span> com.opensymphony.xwork2.ActionSupport;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 20:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 21: <span style="color: #008000">/**
</span></pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 22:  * @author Dian Aditya
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 23:  *
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 24:  */
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 25: @Results({ @Result(name = ActionSupport.SUCCESS, type = "<span style="color: #8b0000">velocity</span>", location = "<span style="color: #8b0000">/view/item/item_list.vm</span>") })
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 26: <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> ItemController <span style="color: #0000ff">extends</span> ActionSupport {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 27:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 28: 	<span style="color: #0000ff">private</span> List result = <span style="color: #0000ff">new</span> ArrayList();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 29: 	<span style="color: #0000ff">private</span> String id;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 30:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 31: 	<span style="color: #008000">/**
</span></pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 32: 	 * Inject lagi
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 33: 	 */
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #ebebeb"> 34: 	@Autowired
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #ebebeb"> 35: 	<span style="color: #0000ff">private</span> ItemDao itemDao;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 36:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 37: 	@Action("<span style="color: #8b0000">/item/show</span>")
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 38: 	<span style="color: #0000ff">public</span> String show() <span style="color: #0000ff">throws</span> Exception {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 39: 		<span style="color: #0000ff">if</span> (itemDao == <span style="color: #0000ff">null</span>) {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 40: 			addActionError("<span style="color: #8b0000">Gagal menyuntik itemDao</span>");
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 41: 		} <span style="color: #0000ff">else</span> {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 42: 			ResultSet resultSet = itemDao.getAllItems();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 43: 			<span style="color: #0000ff">while</span> (resultSet.next()) {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 44: 				Map data = <span style="color: #0000ff">new</span> HashMap();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 45: 				data.put("<span style="color: #8b0000">name</span>", resultSet.getString("<span style="color: #8b0000">name</span>"));
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 46: 				data.put("<span style="color: #8b0000">price</span>", resultSet.getString("<span style="color: #8b0000">price</span>"));
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 47: 				result.add(data);
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 48: 			}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 49: 		}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 50:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 51: 		<span style="color: #0000ff">return</span> SUCCESS;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 52: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 53:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 54: 	<span style="color: #0000ff">public</span> String getId() {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 55: 		<span style="color: #0000ff">return</span> id;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 56: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 57:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 58: 	<span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> setId(String id) {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 59: 		<span style="color: #0000ff">this</span>.id = id;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 60: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 61:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 62: 	<span style="color: #0000ff">public</span> List getResult() {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 63: 		<span style="color: #0000ff">return</span> result;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 64: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 65: }
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 66: </pre></pre>
Dan coba dijalankan, hasilnya tidak akan jauh beda dari contoh sebelumnya.
&nbsp;
Sedikit bonus :p berikut contoh tanpa menggunakan anotasi.
<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  1: <span style="color: #0000ff">&lt;?</span>xml version="1.0" encoding="UTF-8"<span style="color: #0000ff">?&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  2: <span style="color: #0000ff">&lt;</span><span style="color: #800000">beans</span> <span style="color: #ff0000">xmlns</span>=<span style="color: #0000ff">"http://www.springframework.org/schema/beans"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  3: 	<span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">xsi</span>=<span style="color: #0000ff">"http://www.w3.org/2001/XMLSchema-instance"</span> <span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">context</span>=<span style="color: #0000ff">"http://www.springframework.org/schema/context"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  4: 	<span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">p</span>=<span style="color: #0000ff">"http://www.springframework.org/schema/p"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  5: 	<span style="color: #ff0000">xsi</span>:<span style="color: #ff0000">schemaLocation</span>=<span style="color: #0000ff">"http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
</span></pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  6: 		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  7:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  8: 	<span style="color: #0000ff">&lt;</span><span style="color: #800000">bean</span> <span style="color: #ff0000">id</span>=<span style="color: #0000ff">"dataSource"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  9: 		<span style="color: #ff0000">class</span>=<span style="color: #0000ff">"org.springframework.jdbc.datasource.DriverManagerDataSource"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 10: 		<span style="color: #ff0000">p</span>:<span style="color: #ff0000">driverClassName</span>=<span style="color: #0000ff">"com.mysql.jdbc.Driver"</span> <span style="color: #ff0000">p</span>:<span style="color: #ff0000">username</span>=<span style="color: #0000ff">"root"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 11: 		<span style="color: #ff0000">p</span>:<span style="color: #ff0000">password</span>=<span style="color: #0000ff">"admin"</span> <span style="color: #ff0000">p</span>:<span style="color: #ff0000">url</span>=<span style="color: #0000ff">"jdbc:mysql://localhost:3306/test"</span> <span style="color: #0000ff">/&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 12:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 13: 	<span style="color: #0000ff">&lt;</span><span style="color: #800000">bean</span> <span style="color: #ff0000">id</span>=<span style="color: #0000ff">"itemDao"</span> <span style="color: #ff0000">class</span>=<span style="color: #0000ff">"com.mervpolis.dwx.struts2spring.dao.ItemDaoImpl"</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 14: 		<span style="color: #ff0000">p</span>:<span style="color: #ff0000">dataSource</span>-<span style="color: #ff0000">ref</span>=<span style="color: #0000ff">"dataSource"</span> <span style="color: #0000ff">/&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 15:
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 16: <span style="color: #0000ff">&lt;/</span><span style="color: #800000">beans</span><span style="color: #0000ff">&gt;</span></pre></pre>
Atau secara sederhananya mungkin juga seperti ini.
<p<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb">
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  1: <span style="color: #008000">// org.springframework.jdbc.datasource.DriverManagerDataSource</span>
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  2: DriverManagerDataSource dataSource = <span style="color: #0000ff">new</span> DriverManagerDataSource();
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  3: dataSource.setDriverClassName("<span style="color: #8b0000">com.mysql.jdbc.Driver</span>");
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  4: dataSource.setUsername("<span style="color: #8b0000">root</span>");
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  5: dataSource.setPassword("<span style="color: #8b0000">admin</span>");
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  6: dataSource.setUrl("<span style="color: #8b0000">jdbc:mysql://localhost:3306/test</span>");
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  7:
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  8: <span style="color: #008000">// javax.sql.DataSource</span>
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9">  9: DataSource source = dataSource;
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 10:
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 11: <span style="color: #008000">// com.mervpolis.dwx.struts2spring.dao.ItemDaoImpl</span>
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 12: ItemDaoImpl daoImpl = <span style="color: #0000ff">new</span> ItemDaoImpl();
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 13: daoImpl.setDataSource(source);
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 14:
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 15: <span style="color: #008000">// com.mervpolis.dwx.struts2spring.dao.ItemDao</span>
</pre>
 <pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; background-color: #f9f9f9"> 16: ItemDao itemDao = daoImpl;</pre>
Cape juga ternyata, selamat menikmati hidangannya.
</p<pre>