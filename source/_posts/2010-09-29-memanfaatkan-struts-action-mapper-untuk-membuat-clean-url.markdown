---
layout: post
title: Memanfaatkan Struts Action Mapper untuk Membuat Clean URL
comments: true
---
Ini merupakan salah satu favorit saya yang ada di Struts yaitu ActionMapper. Biar lebih jelas berikut saya kutip sedikit dari dokumentasi Struts2.
 ActionMapper interface menyediakan pemetaan antara HTTP Request dan Action Invocation Request atau sebaliknya.
 Lebih sederhananya dengan mengimplementasikan ActionMapper ini kita dapat menentukan action class yang akan dieksekusi berdasarkan HttpServletRequest yang kita terima.
 Sebagai contoh saya ingin mengubah gaya url seperti berikut ini
 &nbsp;
<pre>http://domain/project/employe?id=1234</pre>
 &nbsp;
 menjadi
 &nbsp;
<pre>http://domain/project/employe/1234</pre>
 &nbsp;
 sehingga saya menentukan bahwa baris setelah nama project merupakan nama action (/employee) dan baris setelah nama action adalah id (/1234).
 Berikut kodenya
{% codeblock lang:java %} package com.mervpolis.dwx.struts.mapper;
import java.util.HashMap;
import javax.servlet.http.HttpServletRequest;
import org.apache.struts2.dispatcher.mapper.ActionMapping;
import org.apache.struts2.dispatcher.mapper.DefaultActionMapper;
import com.opensymphony.xwork2.config.ConfigurationManager;
public class MyCustomMapper extends DefaultActionMapper {
 public ActionMapping getMapping(HttpServletRequest request,
 ConfigurationManager configManager) {
 ActionMapping mapping = super.getMapping(request, configManager);
 /**
 * Ketika bukan action yang dipanggil
 * ex: http://domain/project/gambar.jpg
 */
 if (mapping == null)
 return mapping;
 if (mapping.getParams() == null)
 mapping.setParams(new HashMap
 ());
 // Menambil url path
 String path = request.getServletPath().substring(1);
 // Memotong-motong path diantara karakter '/'
 String actions[] = path.split("/");
 // Array terakhir sebagai id
 String id = actions[actions.length - 1];
 // Array sebelum id sebagai actionName
 String actionName = actions[actions.length - 2];
 mapping.setName(actionName);
 mapping.getParams().put("id", id);
 return mapping;
 }
}
 {% endcodeblock %}
 Selanjutnya tinggal mendaftarkan ActionMapper dengan menambahkan properti pada struts.properties
<pre>struts.mapper.class=com.mervpolis.dwx.struts.mapper.MyCustomMapper</pre>
 atau dalam struts.xml
{% codeblock lang:xml %}
<constant name="struts.mapper.class" value="com.mervpolis.dwx.struts.mapper.MyCustomMapper" /> {% endcodeblock %}
 Kemudian action class cukup membuat method setId(String id) untuk mendapatkan id dari url, tidak hanya action class, dengan memanipulasi ActionMapper ini kita dapat pula menentukan method yang akan dieksekusi ketika action class dipanggil.