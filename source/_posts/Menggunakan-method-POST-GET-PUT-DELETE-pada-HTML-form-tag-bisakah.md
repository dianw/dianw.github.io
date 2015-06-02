title: "Menggunakan method POST, GET, PUT, DELETE pada HTML form tag, bisakah?"
date: 2012-07-27 01:03:40
tags:
---

<div style="text-align: justify; "> 
    <div style="text-align: justify; "> 
      <div>Pernahkah anda menggunakan menggunakan http method selain `GET` dan `POST` pada browser. Yah, tentunya hanya hanya client seperti apache http client yang mensupport http request method seperti `PUT`, `DELETE`, ataupun `OPTION`. Sebenarnya saat ini hampir seluruh browser sudah dapat mengirimkan request dengan menggunakan http method tersebut melalui `XMLHttpRequest`, yap kuncinya ada di AJAX. Lalu bagaimana dengan html form tag?</div> 
      <div>
</div> 
      <pre class="brush: xml">&lt;form method="PUT"&gt;&lt;/form&gt;</pre> 
      <div>
</div> 
      <div>Sayangnya semua browser hanya mendukung `POST` dan `GET` untuk request pada form tag, jadi ilustrasi yang saya tulis diatas tidak dapat diimplementasikan. Lalu? Tunggu dulu, buat anda yang sudah main-main dengan Yama, semuanya bisa jadi mungkin. Saya akan memberikan sedikit contoh bagaimana method selain `POST` dan `GET` bisa dikirimkan ke server melalui browser, menggunakan form tag tentunya.</div> 
      <div>
</div> 
      <div>Sebagai pemanasan saya mulai dengan membuat sebuah proyek Yama.</div> 
      <div>
</div> 
      <pre>mvn archetype:generate \
	-DarchetypeGroupId=org.meruvian.yama \
	-DarchetypeArtifactId=yama-struts-archetype \
	-DarchetypeVersion=1.0.2 \
	-DgroupId=com.example.yama \
	-DartifactId=my-first-yama
      </pre> 
      <div>
</div> 
      <div>Oke kemudian saya akan membuat dua buah file jsp dibawah `/src/main/webapp/WEB-INF/test`, satu untuk mengirim request dan yang satunya lagi untuk menerima hasil request.</div> 
      <div>
</div> 
      <div>**index.jsp**</div> 
      <pre class="brush: xml">&lt;form method="delete" action="/test"&gt;
	&lt;input type="submit" value="Test http method!" class="btn"&gt;
&lt;form&gt;
</pre> 
      <div>
</div> 
      <div>**success.jsp**</div> 
      <pre class="brush: xml">Http method : &lt;%= request.getMethod() %&gt;
</pre> 
      <div>
</div> 
      <div>Kemudian saya buat action class.</div> 
      <pre class="brush: java">package com.example.yama.action;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.meruvian.inca.struts2.rest.ActionResult;
import org.meruvian.inca.struts2.rest.annotation.Action;
import org.meruvian.inca.struts2.rest.annotation.Action.HttpMethod;

@Action(name = "/")
public class TestAction {
	private static final Log LOG = LogFactory.getLog(TestAction.class);

	@Action(name = "/test", method = HttpMethod.GET)
	public ActionResult index() {
		return new ActionResult("/WEB-INF/test/index.jsp");
	}

	@Action(name = "/test", method = HttpMethod.DELETE)
	public ActionResult delete() {
		LOG.debug("Method delete executed!");

		return new ActionResult("/WEB-INF/test/success.jsp");
	}
}
</pre> 
      <div>
</div> 
      <div>Dan yang terakhir saya tambahkan link menu dengan mengedit file `menu.ftl` pada `/src/main/webapp/view`</div> 
      <pre class="brush: xml">&lt;li class="active"&gt;
	&lt;a href='&lt;@s.url value="/home" /&gt;'&gt;
		&lt;i class="icon-home icon-white"&gt;&lt;/i&gt; Home
	&lt;/a&gt;
&lt;/li&gt;
&lt;#if request.session.getAttribute("SPRING_SECURITY_CONTEXT")??&gt;
&lt;!-- Menu yang harus ditambahkan --&gt;
&lt;li&gt;
	&lt;a href='#/test'&gt;Test HTTP Method&lt;/a&gt;
&lt;/li&gt;
&lt;!-- End of menu yang harus ditambahkan --&gt;
&lt;li&gt;
	&lt;a href="&lt;@s.url value="/logout" /&gt;"&gt;
		&lt;@s.text name="frontent.logout.text" /&gt;
	&lt;/a&gt;
&lt;/li&gt;
&lt;/#if&gt;
</pre> 
      <div>
</div> 
      <div>Selanjutnya saya akan coba jalankan menggunakan jetty. `mvn jetty:run`</div> 

**Request:**

![Request](https://lh4.googleusercontent.com/-oUWCUUnNXEA/UBF9YoJg8XI/AAAAAAAAAZw/SVE1iKAX6DI/s800/2.png) 

**Response:**

![Response](https://lh4.googleusercontent.com/-iVZgri-Fae8/UBF9YnV0ZhI/AAAAAAAAAZ0/qauYyl7HH2U/s800/1.png) 

      <div>Secara teknis Yama menghandle setiap request pada tag form, kemudian mengkonversinya menjadi AJAX request. Jadi kita tidak perlu lagi membuat kode javascript untuk mengirimkan AJAX request.</div> 
    </div> 
  </div>