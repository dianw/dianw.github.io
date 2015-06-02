title: "Baca Kode ASCII Dengan JQuery"
date: 2010-08-18 21:39:11
tags:
---

Wets... Waktunya balik ke client side lagi, ini nih saya mo sharing tentang cara baca kode ASCII di JQuery, eits... jangan dianggap saya kurang kerjaan, pembacaan kode ascii ini berguna untuk validasi, salah satunya di blog saya sebelumnya [di sini](http://blogs.mervpolis.com/roller/dwx/entry/konversi_nilai_resistor_dengan_jquery) yang menggunakan metode ini untuk membatasi karakter angka saja yang boleh ditulis di textfield.

Sebelumnya apa itu ASCII, berikut saya kutip dari Wikipedia

**Kode Standar Amerika untuk Pertukaran Informasi** atau **ASCII** (_American Standard Code for Information Interchange_) merupakan suatu standar internasional dalam kode huruf dan simbol seperti Hex dan Unicode tetapi ASCII lebih bersifat universal, contohnya 124 adalah untuk karakter &quot;|&quot;. Ia selalu digunakan oleh komputer dan alat komunikasi lain untuk menunjukkan teks. Kode ASCII sebenarnya memiliki komposisi bilangan biner sebanyak 8 bit. Dimulai dari 0000 0000 hingga 1111 1111\. Total kombinasi yang dihasilkan sebanyak 256, dimulai dari kode 0 hingga 255 dalam sistem bilangan Desimal.

Nah langsung aja terjun ke kodenya, pertama yang paling penting bikin formnya dulu (html biasa boleh lah :D)

<pre class="brush: html">
&lt;table&gt;
	&lt;tr&gt;
		&lt;td&gt;Input&lt;/td&gt;
		&lt;td&gt;
			&lt;input type="text" id="input" size="10" style="text-align: center;"&gt;
		&lt;/td&gt;
	&lt;/tr&gt;
	&lt;tr&gt;
		&lt;td&gt;Kode ASCII&lt;/td&gt;
		&lt;td&gt;
			&lt;input type="text" id="code" size="10" disabled="disabled" style="text-align: center;"&gt;
		&lt;/td&gt;
	&lt;/tr&gt;
	&lt;tr&gt;
		&lt;td&gt;Deskripsi&lt;/td&gt;
		&lt;td&gt;
			&lt;input type="text" id="desc" size="30" disabled="disabled"&gt;
		&lt;/td&gt;
	&lt;/tr&gt;
&lt;/table&gt;
</pre>

Dan berikut kode untuk JavaScriptnya

<pre class="brush: javascript">
$(function(){
	$('#input').keydown(function(e) {

		// Baca ASCII nya di sini...
		var key = e.keyCode;

		// Validasi angka
		if (key &gt;= 48 && key &lt;=57) {
		  $('#desc').val('Angka');

		// Validasi huruf
		} else if (key &gt;= 65 && key &lt;=90) {
		  $('#desc').val('Huruf');

		// Selain ketentuan diatas :D
		} else {
		  $('#desc').val('Bukan angka & huruf');
		}

		$('#code').val(key);
		$('#input').val('');
	});
});
</pre>

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
  <tr>
    <td class="span3">Input</td>
    <td>
      <input type="text" id="inputan" size="10" style="text-align: center;">
	</td>
  </tr>
  <tr>
	<td>Kode ASCII</td>
	<td>
	  <input type="text" id="code" size="10" disabled="disabled" style="text-align: center;">
	</td>
  </tr>
  <tr>
	<td>Deskripsi</td>
	<td>
	  <input type="text" id="desc" size="30" disabled="disabled">
	</td>
  </tr>
</table>
</div>