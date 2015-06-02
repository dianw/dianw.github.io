title: "Kekuatan dibalik Proxy dalam Java"
date: 2012-04-15 21:01:32
tags:
---

Proxy sudah diperkenalkan sejak java 1.3, salah satu kekuatan (yang tidak tersembunyi) dari java yang sangat berguna dalam pembuatan aplikasi dengan tingkat modularitas tinggi.

Sebelumnya kita ambil contoh dalam kehidupan coding sehari-hari, dengan atau tanpa disadari proxy ada dimana-mana seperti Spring yang mengimplementasikannya untuk AOP, Hibernate untuk lazy loading-nya, dan Google Guice dalam hal dependency Injection.

Dalam Java, dynamic proxy adalah sebuah intance pengganti dari object sesungguhnya. Dengan proxy, pemanggilan sebuah method (method invocation) dapat dicegat (intercepted) sehingga kita dapat memasukkan cutom code sebelum ataupun setelah pemaggilan method. Sangat powerful bukan?

Dalam penggunaannya, diluar dynamic proxy standart yang disediakan oleh JDK, banyak library yang menyediakan fitur proxy dalam penggunaannya, sebagai contoh Spring menyediakan JDK dynamic proxy dan CGLIB proxy bagi user untuk memilih salah satu. Begitu pula Hibernate yang menggunakan CGLIB untuk versi 3.x ke bawah yang kemudian berpindah menggunakan secara penuh Javassisst untuk menangani urusan proxy.

Ada beberapa perbedaan antara JDK dynamic proxy dengan CGLIB serta Javassist, akan saya jelaskan sebagai berikut

<span style="font-size: large; ">**JDK Dynamic Proxy**</span>

*   Proxy dibangun berdasarkan pada runtime interface, dengan kata lain proxy harus memiliki interface untuk dapat diciptakan.
*   Karena proxy dipaksa untuk menjadi implementasi dari sudatu interface maka sudah dipastikan kita tidak bisa melakukan casting terhadap class lain.
*   Object menjadi turunan dari `java.lang.reflect.Proxy` 

Berikut contoh kode sedehananya, sebuah proxy yang akan menghalau setiap method bernama `call` dengan mengembalikan nilai `null` apapun nilainya.

  <pre class="brush: java">class DisableEveryCall implements InvocationHandler {
	private Object object;

	public DisableEveryCall(Object object) {
		this.object = object;
	}

	public Object invoke(Object proxy, Method method, Object[] args)
		throws Throwable {

		if (method.getName().equalsIgnoreCase("call")) {
			return null;
		}

		return method.invoke(object, args);
	}
}
</pre> 

Method invoke akan meng-intercept setiap method, dalam kasus ini hanya method dengan nama `call` yang akan mereturn `null` dengan apapun kondisinya. Kemudian berikut cara membuat proxy-nya.

  <pre class="brush: java">Callable<string> callable = new Callable<string>() {
	public String call() throws Exception {
		return "Called!";
	}
};

Proxy proxy = (Proxy) Proxy.newProxyInstance(
	Callable.class.getClassLoader(),
	new Class[] { Callable.class }, new DisableEveryCall(callable));

callable = (Callable) proxy;
System.out.println(callable.call());
</pre> 

<span style="font-size: large; ">**CGLIB Proxy**</span>

*   Tidak mengharuskan interface.
*   Proxy dibangun dengan cara mensub-classkan terhadap class sesungguhnya. Hal ini mengindikasikan bahwa class apapun yang digunakan dapat dimungkinkan untuk dijadikan proxy.
*   CGLIB proxy bersifat final. Maka adalah sebuah hal sia-sia untuk memproxy sebuah proxy karena tidak akan berhasil. 

Mari membuat contoh yang agak berbeda, ketika kita memanggil method `toString` proxy akan mengembalikan dengan nilai `null` bagaimanapun caranya user melakukan override terhadap method tersebut.

  <pre class="brush: java">	
class DisableToString implements MethodInterceptor {
	public Object intercept(Object obj, Method method, Object[] args,
			MethodProxy proxy) throws Throwable {

		return "toString".equals(method.getName()) ? null : proxy
			.invokeSuper(obj, args);
	}
}
</pre> 

Pembuatan proxy dapat dilakukan dengan cara berikut

  <pre class="brush: java">	
Enhancer enhancer = new Enhancer();
enhancer.setSuperclass(Object.class);
enhancer.setCallback(new DisableToString());

Object proxy = enhancer.create();
System.out.println(proxy.toString());
</pre> 

<span style="font-size: large; ">**Javassist Proxy**</span>

Tidak banyak perbedaan antara Javassist dengan CGLIB proxy, penjelasan tersebut dapat langsung saya ambil dari dokumentasi source-nya.

  <div class="well"> 

<span style="font-size: large; ">**Package javassist.util.proxy**</span>

Dynamic proxy (similar to `Enhancer` of cglib).

  </div> 

Perbedaan paling utama adalah kita tidak dapat menambahkan sebuah method baru kedalam Javassist proxy, sedangkan hal tersebut dimungkinkan pada CGLIB proxy. Serta fitur utama dari Javassist sendiri adalah bytecode manipulation library yang lebih mengedepankan manipulasi class pada saat compile-time bahkan mendefinisikan class baru pada saat run-time daripada memproxynya, sangat powerful dalam sisi lain tapi tidak dengan fitur proxy-nya. Berikut beberapa hal dalam Javassist proxy:

*   Lebih lambat dari CGLIB proxy maupun JDK proxy (untuk saat ini) 

Agar mudah saya akan membuat contoh dengan pendekatan yang tidak jauh berbeda.

  <pre class="brush: java">class DisableToString implements MethodHandler {
	public Object invoke(Object self, Method thisMethod,
			Method proceed, Object[] args) throws Throwable {
		return null;
	}
}
</pre> 

Langkah pembuatan proxy-nya pun hampir sama

  <pre class="brush: java">ProxyFactory factory = new ProxyFactory();
factory.setSuperclass(Object.class);
factory.setFilter(new MethodFilter() {
	public boolean isHandled(Method m) {
		return "toString".equals(m.getName());
	}
});

Proxy proxy = (javassist.util.proxy.Proxy) factory
		.createClass().newInstance();
proxy.setHandler(new DisableToString());

Object object = proxy;
System.out.println(object.toString());
</pre> 

<span style="font-size: large; ">**Kesimpulan**</span>

Java memiliki banyak fitur menarik, salah satunya adalah dynamic proxy. Spring transaction manager dan Hibernate lazy loading mungkin adalah contoh yang paling mudah untuk memahami bagaimana proxy bekerja untuk meng-intercept sebuah method. Banyak pula library pembantu di luar sana yang menyediakan fitur proxy dengan berbagai keunggulan masing-masing.

Enjoy.