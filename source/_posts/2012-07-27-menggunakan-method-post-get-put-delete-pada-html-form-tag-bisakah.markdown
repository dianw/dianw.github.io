---
layout: post
title: Menggunakan method POST, GET, PUT, DELETE pada HTML form tag, bisakah?
comments: true
---
 
Pernahkah anda menggunakan menggunakan http method selain ```GET``` dan ```POST``` pada browser. Yah, tentunya hanya hanya client seperti apache http client yang mensupport http request method seperti ```PUT```, ```DELETE```, ataupun ```OPTION```. Sebenarnya saat ini hampir seluruh browser sudah dapat mengirimkan request dengan menggunakan http method tersebut melalui ```XMLHttpRequest```, yap kuncinya ada di AJAX. Lalu bagaimana dengan html form tag?
{% codeblock lang:xml %}
 <form method="PUT"></form> {% endcodeblock %}
 
Sayangnya semua browser hanya mendukung ```POST``` dan ```GET``` untuk request pada form tag, jadi ilustrasi yang saya tulis diatas tidak dapat diimplementasikan. Lalu? Tunggu dulu, buat anda yang sudah main-main dengan Yama, semuanya bisa jadi mungkin. Saya akan memberikan sedikit contoh bagaimana method selain ```POST``` dan ```GET``` bisa dikirimkan ke server melalui browser, menggunakan form tag tentunya.

Sebagai pemanasan saya mulai dengan membuat sebuah proyek Yama.
```
mvn archetype:generate \
 -DarchetypeGroupId=org.meruvian.yama \
-DarchetypeArtifactId=yama-struts-archetype \
-DarchetypeVersion=1.0.2 \
-DgroupId=com.example.yama \
-DartifactId=my-first-yama
```
 
Oke kemudian saya akan membuat dua buah file jsp dibawah ```/src/main/webapp/WEB-INF/test```, satu untuk mengirim request dan yang satunya lagi untuk menerima hasil request.

### index.jsp

{% codeblock lang:xml %}
<form method="delete" action="/test">
 <input type="submit" value="Test http method!" class="btn" />
</form>
{% endcodeblock %}

### success.jsp

{% codeblock lang:xml %}
Http method : <%= request.getMethod() %>
{% endcodeblock %}

Kemudian saya buat action class.
 </div>

{% codeblock lang:java %}
package com.example.yama.action;
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
{% endcodeblock %}

Dan yang terakhir saya tambahkan link menu dengan mengedit file ```menu.ftl``` pada ```/src/main/webapp/view```

{% codeblock lang:xml %}
<li class="active">
  <a href="<@s.url value=&quot;/home&quot; />">
    <i class="icon-home icon-white"></i> Home
  </a>
</li>
<#if request.session.getAttribute("SPRING_SECURITY_CONTEXT")??>
<!-- Menu yang harus ditambahkan -->
<li>
  <a href="#/test">Test HTTP Method</a>
</li>
<!-- End of menu yang harus ditambahkan -->
<li>
  <a href="<@s.url value="logout" />"></a>
  <@s.text name="frontent.logout.text" />
</li>
<!--#if-->
{% endcodeblock %}
 
Selanjutnya saya akan coba jalankan menggunakan jetty. ```mvn jetty:run```

### Request:
<img src="https://lh4.googleusercontent.com/-oUWCUUnNXEA/UBF9YoJg8XI/AAAAAAAAAZw/SVE1iKAX6DI/s800/2.png" alt="Request" />

### Response:
<img src="https://lh4.googleusercontent.com/-iVZgri-Fae8/UBF9YnV0ZhI/AAAAAAAAAZ0/qauYyl7HH2U/s800/1.png" alt="Response" />
 
Secara teknis Yama menghandle setiap request pada tag form, kemudian mengkonversinya menjadi AJAX request. Jadi kita tidak perlu lagi membuat kode javascript untuk mengirimkan AJAX request.
