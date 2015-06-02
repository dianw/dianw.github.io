title: "Struts + Velocity, Solusi Mudah dan Ideal"
date: 2010-01-03 12:33:04
tags:
---

Saya masih tertarik nih dengan model aplikasi MVC, kali ini saya akan membahas sedikit tentang Controller pada aplikasi Java Web yang dikenal merupakan solusi MVC paling mudah yaitu Struts2\. Tapi ada yang tau apa itu Controller? Nih saya copas dari modul JENI :P ?Controller merupakan sebuah layer yang bekerja untuk mengurusi urusan _antar layer, _yang artinya bertanggung jawab terhadap eksekusi aplikasi.? Sedangkan untuk masalah Viewer Layer saya menggunakan Velocity yang saya rasa juga lebih mudah dibandingkan dengan JSP.

Langsusng saja dari pada bingung apa maksudnya mending kita langsung terjun ke lapangan. Untuk memulai membuat project pertama buka IDE anda :D dalam kasus kali ini saya menggunakan Eclipse IDE. Eits, jangan lupa untuk mendownload library, bagi anda yang belum mempunyai library Struts2 dan Velocity silakan download di [sini](http://www.apache.org "Download Struts2").

Nah mari menuju medan perang!

Pertama buat project baru.

Untuk Eclipse silakan klik ?File ? New ? Other ? Web ? Dynamic Web Project? Maka otomatis akan langsung terbentuk folder src dan WebContent. Copy semua library yang sudah didownload ke dalam folder WebContent/WEB-INF/lib maka library tersebut akan langsung dikenali sebagai _Web App Libraries_.

![Lib](http://lh3.ggpht.com/_ea6RQRVsoP4/S0AoGCHWERI/AAAAAAAAAGI/PYDs0kwbSVo/Lib.png "Lib")

Nah setelah semua library siap langkah pertama untuk membuat sebuah aplikasi Struts2 adalah dengan mengkonfigurasi web.xml yang terdapat pada folder WebContent/WEB-INF.

Berikut konfigurasi yang harus ditambahkan pada web.xml :

<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  1: <span style="color: #0000ff">&lt;?</span>xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;<span style="color: #0000ff">?&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  2: <span style="color: #0000ff">&lt;</span><span style="color: #800000">web</span>-<span style="color: #ff0000">app</span> <span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">xsi</span>=<span style="color: #0000ff">&quot;http://www.w3.org/2001/XMLSchema-instance&quot;</span> <span style="color: #ff0000">xmlns</span>=<span style="color: #0000ff">&quot;http://java.sun.com/xml/ns/javaee&quot;</span> <span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">web</span>=<span style="color: #0000ff">&quot;http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd&quot;</span> <span style="color: #ff0000">xsi</span>:<span style="color: #ff0000">schemaLocation</span>=<span style="color: #0000ff">&quot;http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd&quot;</span> <span style="color: #ff0000">id</span>=<span style="color: #0000ff">&quot;WebApp_ID&quot;</span> <span style="color: #ff0000">version</span>=<span style="color: #0000ff">&quot;2.5&quot;</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  3:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">display</span>-<span style="color: #ff0000">name</span><span style="color: #0000ff">&gt;</span>Struts2<span style="color: #0000ff">&lt;/</span><span style="color: #800000">display</span>-name<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0">  4:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">filter</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0">  5:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">filter</span>-<span style="color: #ff0000">name</span><span style="color: #0000ff">&gt;</span>struts2<span style="color: #0000ff">&lt;/</span><span style="color: #800000">filter</span>-name<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0">  6:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">filter</span>-<span style="color: #ff0000">class</span><span style="color: #0000ff">&gt;</span>org.apache.struts2.dispatcher.ng.filter. StrutsPrepareAndExecuteFilter<span style="color: #0000ff">&lt;/</span><span style="color: #800000">filter</span>-class<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0">  7:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">filter</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0">  8:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">filter</span>-<span style="color: #ff0000">mapping</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0">  9:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">filter</span>-<span style="color: #ff0000">name</span><span style="color: #0000ff">&gt;</span>struts2<span style="color: #0000ff">&lt;/</span><span style="color: #800000">filter</span>-name<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0"> 10:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">url</span>-<span style="color: #ff0000">pattern</span><span style="color: #0000ff">&gt;</span>/*<span style="color: #0000ff">&lt;/</span><span style="color: #800000">url</span>-pattern<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0"> 11:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">filter</span>-mapping<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 12:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">welcome</span>-<span style="color: #ff0000">file</span>-<span style="color: #ff0000">list</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 13:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">welcome</span>-<span style="color: #ff0000">file</span><span style="color: #0000ff">&gt;</span>index.jsp<span style="color: #0000ff">&lt;/</span><span style="color: #800000">welcome</span>-file<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 14:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">welcome</span>-file-list<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 15: <span style="color: #0000ff">&lt;/</span><span style="color: #800000">web</span>-app<span style="color: #0000ff">&gt;</span></pre></pre>

Selanjutnya kita buat class action dari Struts2, action ini adalah class yang fungsinya tidak jauh berbeda dengan class POJO yaitu menampung properti-properti yang akan dikenal pada presentation layer MVC yang mengimplementasikan action pada Struts2.

Berikut contohnya :

<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  1: <span style="color: #0000ff">package</span> com.mervpolis.struts;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  2: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  3: <span style="color: #0000ff">import</span> java.sql.Timestamp;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  4: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  5: <span style="color: #0000ff">import</span> com.opensymphony.xwork2.ActionSupport;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  6: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  7: <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> HelloWorldAction <span style="color: #0000ff">extends</span> ActionSupport {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  8: 	<span style="color: #0000ff">private</span> String pesan;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  9: 	<span style="color: #0000ff">private</span> String waktu;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 10: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0"> 11: 	<span style="color: #0000ff">public</span> String execute() {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0"> 12: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0"> 13: 		pesan = &quot;<span style="color: #8b0000">HELLO WORLD....!!! Method execute telah dijalankan....</span>&quot;;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0"> 14: 		waktu = <span style="color: #0000ff">new</span> Timestamp(System.currentTimeMillis()).toString();
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0"> 15: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0"> 16: 		<span style="color: #0000ff">return</span> SUCCESS;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #c0c0c0"> 17: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 18: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 19: 	<span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> setPesan(String pesan) {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 20: 		<span style="color: #0000ff">this</span>.pesan = pesan;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 21: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 22: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 23: 	<span style="color: #0000ff">public</span> String getPesan() {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 24: 		<span style="color: #0000ff">return</span> pesan;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 25: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 26: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 27: 	<span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> setWaktu(String waktu) {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 28: 		<span style="color: #0000ff">this</span>.waktu = waktu;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 29: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 30: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 31: 	<span style="color: #0000ff">public</span> String getWaktu() {
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 32: 		<span style="color: #0000ff">return</span> waktu;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 33: 	}
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 34: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 35: }</pre></pre>

Contoh diatas hampir mirip dengan class POJO yang memiliki method get dan set, perbedaannya adalah action dari Struts2 memiliki method execute() yang akan dijalankan ketika client memangil action tersebut.

Tinggal kita membuat viewnya, pada bagian ini sudah saya jelaskan sebelumnya kita akan menggunakan Velocity sebagai viewer layernya, kita buat file _hello.vm _pada folder WebContent/view.

<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  1: <span style="color: #0000ff">&lt;</span><span style="color: #800000">html</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  2:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">head</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  3:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">title</span><span style="color: #0000ff">&gt;</span>Project Struts2 Pertama<span style="color: #0000ff">&lt;/</span><span style="color: #800000">title</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  4:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">head</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  5:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">body</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  6:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>Pesan : $pesan<span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  7:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>Waktu hari ini : $waktu<span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  8:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">body</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  9: <span style="color: #0000ff">&lt;/</span><span style="color: #800000">html</span><span style="color: #0000ff">&gt;</span></pre></pre>

Sepertinya baris kode diatas sudah cukup untuk menjelaskan mengapa saya menyebut Velocity lebih simple daripada JSP. Sekarang bandingkan dengan hello.jsp berikut.

<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  1: <span style="color: #0000ff">&lt;</span>%@taglib prefix=&quot;s&quot; uri=&quot;/struts-tags&quot; %<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  2: <span style="color: #0000ff">&lt;</span><span style="color: #800000">html</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  3:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">head</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  4:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">title</span><span style="color: #0000ff">&gt;</span>Project Struts2 Pertama<span style="color: #0000ff">&lt;/</span><span style="color: #800000">title</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  5:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">head</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  6:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">body</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  7:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>Pesan : <span style="color: #0000ff">&lt;</span><span style="color: #c71585">s</span>:<span style="color: #800000">property</span> <span style="color: #ff0000">value</span>=<span style="color: #0000ff">&quot;pesan&quot;</span> <span style="color: #0000ff">/&gt;</span><span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  8:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>Waktu hari ini : <span style="color: #0000ff">&lt;</span><span style="color: #c71585">s</span>:<span style="color: #800000">property</span> <span style="color: #ff0000">value</span>=<span style="color: #0000ff">&quot;waktu&quot;</span> <span style="color: #0000ff">/&gt;</span><span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  9:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">body</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 10: <span style="color: #0000ff">&lt;/</span><span style="color: #800000">html</span><span style="color: #0000ff">&gt;</span></pre></pre>

Nah anda tinggal pilih lebih suka menggunakan viewer yang mana.

Sekarang kita masuk ke bagian inti dari semua itu yaitu membuat file konfigurasi struts.xml yang diletakkan pada folder src. Selanjutnya kita hanya perlu memapping action yang telah kita buat ke dalam file konfigurasi ini.

<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  1: <span style="color: #0000ff">&lt;?</span>xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; <span style="color: #0000ff">?&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  2: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  3: <span style="color: #0000ff">&lt;</span>!DOCTYPE struts PUBLIC
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  4:     &quot;-//Apache Software Foundation//DTD Struts Configuration 2.0//EN&quot;
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  5:     &quot;http://struts.apache.org/dtds/struts-2.0.dtd&quot;<span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  6: 
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  7: <span style="color: #0000ff">&lt;</span><span style="color: #800000">struts</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  8:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">package</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">&quot;default&quot;</span> <span style="color: #ff0000">extends</span>=<span style="color: #0000ff">&quot;struts-default&quot;</span> <span style="color: #ff0000">namespace</span>=<span style="color: #0000ff">&quot;/cobastruts&quot;</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  9:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">action</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">&quot;HelloVelocity&quot;</span> <span style="color: #ff0000">class</span>=<span style="color: #0000ff">&quot;com.mervpolis.struts.HelloWorldAction&quot;</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 10:       <span style="color: #0000ff">&lt;</span><span style="color: #800000">result</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">&quot;success&quot;</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">&quot;velocity&quot;</span><span style="color: #0000ff">&gt;</span>/view/hello.vm<span style="color: #0000ff">&lt;/</span><span style="color: #800000">result</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 11:     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">action</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 12:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">action</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">&quot;HelloJSP&quot;</span> <span style="color: #ff0000">class</span>=<span style="color: #0000ff">&quot;com.mervpolis.struts.HelloWorldAction&quot;</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 13:       <span style="color: #0000ff">&lt;</span><span style="color: #800000">result</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">&quot;success&quot;</span><span style="color: #0000ff">&gt;</span>/view/hello.jsp<span style="color: #0000ff">&lt;/</span><span style="color: #800000">result</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 14:     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">action</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0"> 15:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">package</span><span style="color: #0000ff">&gt;</span>
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff"> 16: <span style="color: #0000ff">&lt;/</span><span style="color: #800000">struts</span><span style="color: #0000ff">&gt;</span></pre></pre>

Buat file index.jsp yang akan me-redirect halaman langsung ke action, file ini akan dijalankan pertama kali saat aplikasi baru dijalankan.

<pre style="border-right: #cecece 1px solid; padding-right: 5px; border-top: #cecece 1px solid; padding-left: 5px; min-height: 40px; padding-bottom: 5px; overflow: auto; border-left: #cecece 1px solid; width: 550px; padding-top: 5px; border-bottom: #cecece 1px solid; background-color: #fbfbfb"><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  1: <span style="color: #0000ff">&lt;</span>%
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #ffffff">  2:   response.sendRedirect(&quot;cobastruts/HelloVelocity.action&quot;);
</pre><pre style="font-size: 12px; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; background-color: #f0f0f0">  3: %<span style="color: #0000ff">&gt;</span></pre></pre>

Sekarang coba jalankan aplikasi tersebut pada server dengan mengetik 

http://[host]:[port]/[namaproject]/

Dan hasilnya adalah :

![HasilAkhir](http://lh5.ggpht.com/_ea6RQRVsoP4/S0AoF421ARI/AAAAAAAAAGE/-R9AXDjIhX8/HasilAkhir.PNG "HasilAkhir") 

Selesai sudah bahasan hari ini :D Mudah bukan membuat aplikasi web menggunakan Controller Struts2, tinggal menambahkan kreativitas dan sedikit ide gila anda untuk menjadikan banyak aplikasi yang mungkin tidak pernah terpikirkan sebelumnya.