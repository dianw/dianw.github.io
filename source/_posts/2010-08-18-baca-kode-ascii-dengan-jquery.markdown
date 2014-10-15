---
layout: post
title: Baca Kode ASCII Dengan JQuery
comments: true
---
Wets... Waktunya balik ke client side lagi, ini nih saya mo sharing tentang cara baca kode ASCII di JQuery, eits... jangan dianggap saya kurang kerjaan, pembacaan kode ascii ini berguna untuk validasi, salah satunya di blog saya sebelumnya di sini yang menggunakan metode ini untuk membatasi karakter angka saja yang boleh ditulis di textfield.
Sebelumnya apa itu ASCII, berikut saya kutip dari Wikipedia
Kode Standar Amerika untuk Pertukaran Informasi atau ASCII (American Standard Code for Information Interchange) merupakan suatu standar internasional dalam kode huruf dan simbol seperti Hex dan Unicode tetapi ASCII lebih bersifat universal, contohnya 124 adalah untuk karakter "|". Ia selalu digunakan oleh komputer dan alat komunikasi lain untuk menunjukkan teks. Kode ASCII sebenarnya memiliki komposisi bilangan biner sebanyak 8 bit. Dimulai dari 0000 0000 hingga 1111 1111. Total kombinasi yang dihasilkan sebanyak 256, dimulai dari kode 0 hingga 255 dalam sistem bilangan Desimal.
Nah langsung aja terjun ke kodenya, pertama yang paling penting bikin formnya dulu (html biasa boleh lah :D)
{% codeblock lang:html %}
<table>
 <tbody>
  <tr>
  
   <td>Input</td>
   <td>
 <input type="text" id="input" size="10" style="text-align: center;">
 </td>
  </tr>
  <tr>
  
   <td>Kode ASCII</td>
   <td>
 <input type="text" id="code" size="10" disabled style="text-align: center;">
 </td>
  </tr>
  <tr>
  
   <td>Deskripsi</td>
   <td>
 <input type="text" id="desc" size="30" disabled>
 </td>
  </tr>
 </tbody>
</table>
 {% endcodeblock %}
Dan berikut kode untuk JavaScriptnya
{% codeblock lang:javascript %}
$(function(){
 $('#input').keydown(function(e) {
 // Baca ASCII nya di sini...
 var key = e.keyCode;
 // Validasi angka
 if (key &gt;= 48 &amp;&amp; key &lt;=57) {
 $('#desc').val('Angka');
 // Validasi huruf
 } else if (key &gt;= 65 &amp;&amp; key &lt;=90) {
 $('#desc').val('Huruf');
 // Selain ketentuan diatas :D
 } else {
 $('#desc').val('Bukan angka &amp; huruf');
 }
 $('#code').val(key);
 $('#input').val('');
 });
});
 {% endcodeblock %}
Nah cukup mudah kan (jawab ya!! peace), dan hasilnya adalah sebagai berikut
<script type="text/javascript">
$(function(){
  $('#inputan').keydown(function(e) {
			
	// Baca ASCII nya di sini...
	var key = e.keyCode;
			
	// Validasi angka
	if (key >= 48 && key <=57) {
	  $('#desc').val('Angka');
			
	// Validasi huruf
	} else if (key >= 65 && key <=90) {
	  $('#desc').val('Huruf');
				
	// Selain ketentuan diatas :D
	} else {
	  $('#desc').val('Bukan angka & huruf');
	}
			
	$('#code').val(key);
	$('#inputan').val('');
  });
});
</script>
<div class="well">
 <table>
 
  <tbody>
  
   <tr>
   
    <td class="span3">Input</td>
    <td>
 <input type="text" id="inputan" size="10" style="text-align: center;">
 </td>
   </tr>
   <tr>
   
    <td>Kode ASCII</td>
    <td>
 <input type="text" id="code" size="10" disabled style="text-align: center;">
 </td>
   </tr>
   <tr>
   
    <td>Deskripsi</td>
    <td>
 <input type="text" id="desc" size="30" disabled>
 </td>
   </tr>
  </tbody>
 </table>
</div>