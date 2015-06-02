title: "Memanfaatkan Struts Action Mapper untuk Membuat Clean URL"
date: 2010-09-29 22:20:42
tags:
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

sehingga saya menentukan bahwa baris setelah nama project merupakan nama action (<font face="Courier New">/employee</font>) dan baris setelah nama action adalah id (<font face="Courier New">/1234</font>).

Berikut kodenya

  <pre class="brush: java">package com.mervpolis.dwx.struts.mapper;

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
			mapping.setParams(new HashMap<string, object="">());

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
  </string,></pre> 

Selanjutnya tinggal mendaftarkan ActionMapper dengan menambahkan properti pada struts.properties

  <pre>struts.mapper.class=com.mervpolis.dwx.struts.mapper.MyCustomMapper</pre> 

atau dalam struts.xml

  <pre class="brush: xml">&lt;constant name="struts.mapper.class" value="com.mervpolis.dwx.struts.mapper.MyCustomMapper"/&gt;</pre> 

Kemudian action class cukup membuat method setId(String id) untuk mendapatkan id dari url, tidak hanya action class, dengan memanipulasi ActionMapper ini kita dapat pula menentukan method yang akan dieksekusi ketika action class dipanggil.