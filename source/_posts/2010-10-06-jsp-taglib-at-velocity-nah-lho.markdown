---
layout: post
title: JSP Taglib at Velocity, Nah lho...
comments: true
---
Nah ini kasus baru ketika saya menggunakan velocity sebagai presentation layer, yap salah satu keunggulan dari velocity adalah kecepatannya karena velocity tidak membutuhkan proses tranformasi dan kompilasi layaknya JSP. Namun sedikit mengingatkan tentang pepatah lama, tak ada gading yang tak retak. Dan berdasarkan info yang pernah saya dengar salah satu keunggulan JSP dibanding velocity adalah taglibnya, dilema deh? Karena sayangnya menggunakan taglib memang lebih menyenangkan :D sempat terpikirkan untuk memindahkan semua presentation layer ke JSP sebelum akhirnya saya main ke Website Struts2. Hahai sebuah pencerahan datang?
 Velocity tags are extensions of the generic Struts Tags provided by the framework. You can get jump right in just by knowing the structure in which the tags can be accessed: #s*tag *(...) ... #end, where tag is any of the Struts Tags supported by the framework.
 Yayaya Velocity tags, dan berikut perbandingannya dengan JSP tags
{% codeblock lang:xml %}
<s:form action="updatePerson">
 <s:textfield label="First name" name="firstName" />
 <s:submit value="Update" />
</s:form>
 {% endcodeblock %}
 dan dalam velocity dapat dituliskan seperti berikut
<pre>#sform ("action=updatePerson")
	#stextfield ("label=First name" "name=firstName")
	#ssubmit ("value=Update")
#end
</pre>
 Dan akhirnya migrasi ke JSP pun untuk sementara dibatalkan :p