title: "Mengimplementasikan Clean URL Parameter dengan Inca S2Rest Plugin"
date: 2012-06-12 03:26:33
tags:
---

<div style="text-align: justify; "> 

Sudah pernah baca blog saya sebelumnya tentang&nbsp;[Memanfaatkan Struts Action Mapper untuk Membuat Clean URL](http://blogs.mervpolis.com/roller/dwx/entry/memanfaatkan_struts_action_mapper_untuk)?&nbsp;

Beberapa waktu yang lalu Meruvian melaunching [Inca S2Rest Plugin](http://s2restplugins.java.net), ya plugin untuk Struts2 dengan fitur utama mengadopsikan arsitektur REST ke dalam Struts2 tentunya. Secara garis besar plugin ini bisa dibilang sebagai fusion-nya Convention Plugin dengan Rest Plugin yang sudah ada sejak dahulu kala, yang jelas dengan implementasi berbeda serta fitur yang jelas lebih banyak, ceile. Seperti harapan yang menjadi kenyataan,&nbsp;setelah&nbsp;beberapa tahun terakhir kita utak-atik action mapper dan kawan-kawannya akhirnya jadilah sebuah plugin untuk mengisi celah fitur yang tidak disertakan pada plugin Struts2 yang lain.

Pada artikel kali ini saya 'hanya' akan membahas salah satu fitur dari Inca yaitu dukungan untuk membuat clean url parameter. Nanti dulu deh, untuk fitur-fitur lain akan saya kupas lebih tajam dalam Developer Blog, terpisah dari blog ini, untuk saat ini bisa main-main dulu ke [wikinya](http://java.net/projects/s2restplugins/pages/Home "Inca S2REST Plugins Wiki Page").

Ok, kembali ke laptop, Inca S2Rest menyediakan fitur anotasi untuk membuat konfigurasi dapat di-embed ke dalam kode java, sehingga meminimalisir konfigurasi xml yang harus dimaintain oleh developer. Salah satu anotasi yang powerful adalah `@ActionParam`, anotasi ini berada pada level field untuk menangkap request parameter yang dikirim ke dalam action class, yap no getter and setter anymore. Anotasi ini membantu kita untuk mendapatkan request parameter, lalu bagaimana dengan membuat clean url?

Saya akan membuat sebuah contoh kasus sebuah action untuk menampilkan sebuah artikel berdasarkan tanggal lengkap beserta id dari artikel. Pertama kita tentukan format url yang akan digunakan:

    <pre>/articles/{tahun}/{bulan}/{tanggal}/{id}</pre> 

Saya kutip sedikit dari wiki:

By default, the Inca S2RestPlugin use Regex Pattern Matcher for its Pattern Matcher.

Sehingga pattern yang sudah saya tulis sebelumnya sudah memenuhi kualifikasi untuk menentukan sebuah action sesuai dengan format url. Waktunya membatik:

    <pre class="brush: java">@Action(name = "/article") // Deklarasi action class
public class ArticleAction {
	@ActionParam("th")
	private int year;

	@ActionParam("bln")
	private int month;

	@ActionParam("tgl")
	private int date;

	@ActionParam("id")
	private String articleId;

	@Action(name = "/{th}/{bln}/{tgl}/{id}", method = HttpMethod.GET)
	public String showArticle() {
		// lakukan sesuka hati disini
		// parameter akan dengan deklarasi @ActionParam akan terisi
		// apabila request url sudah sesuai dengan pattern pada 
		// properti 'name' dalam @Action
	}
}
</pre> 

 Langkah terakhir adalah membuat skenario request dengan url (sebagai contoh):&nbsp;`/article/2012/05/04/release-inca-sudirman-thamrin-ditutup-selama-sepekan`.

### **Menggunakan URL Pattern Matcher Lain**

Walaupun secara default Inca S2Rest menggunakan regex pattern matcher, developer dapat merubah dengan implementasi pattern matcher lain cukup dengan merubah konfigurasi:

    <pre class="brush: xml">&lt;constant name="struts.patternMatcher" value="struts" /&gt;
</pre> 

Contoh diatas apabila developer menghendaki untuk menggunakan struts (default) pattern matcher, maka contoh kode yang harus digunakan adalah:

    <pre class="brush: java">@Action(name="/article/*/*/*/*", params= { 
	@Param(name="th", value="{1}"),
	@Param(name="bln", value="{2}"),
	@Param(name="tgl", value="{3}"),
	@Param(name="id", value="{4}")
})
    </pre> 

Selamat mecoba dan mengimplementasikan Inca S2Rest Plugin pada aplikasi Struts2 anda.

  </div>