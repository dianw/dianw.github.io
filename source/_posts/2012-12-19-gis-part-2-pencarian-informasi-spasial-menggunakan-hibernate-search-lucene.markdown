---
layout: post
title: GIS Part 2 - Pencarian Informasi Spasial menggunakan Hibernate Search/Lucene
comments: true
---
Berikut ini adalah posting saya yang kedua mengenai GIS, karena topik ini dibagi menjadi 4 bagian, saya sangat menyarankan anda untuk membaca secara berurutan.

<ul>
 <li><a href="http://blogs.mervpolis.com/roller/dwx/entry/gis_part_1_mengkonsumsi_data" target="_blank">Part 1 - Mengkonsumsi Data Tiles dengan Leaflet</a></li>
 <li><a href="http://blogs.mervpolis.com/roller/dwx/entry/gis_part_2_pencarian_informasi" target="_blank">Part 2 - Pencarian Informasi Spasial menggunakan Hibernate Search/Lucene</a></li>
 <li>Part 3 - Membuat REST API dengan S2Rest Plugin</li>
 <li>Part 4 - Mengkonsumsi Informasi Spatial dengan Leaflet dan JQuery</li>
</ul>

Pencarian Informasi Spasial menggunakan Hibernate Search/Lucene
---

Pernah pakai aplikasi yang berhubungan dengan point of interest seperti foursquare atau google latitude? Ya dua aplikasi yang saya sebutkan tersebut adalah contoh implementasi dari sebuah service yang sangat berkaitan erat dengan lokasi dan koordinat. Ketika kita membuka aplikasi, melalui browser atau perangkat mobile misalnya, aplikasi akan secara otomatis membaca koordinat dimana perangkat berada melalui GPS (dengan seizin pemilik perangkat tentunya) dan kemudian menamplikan lokasi-lokasi terdekat dengan radius tertentu dari lokasi user berada.

Pada bagian ini saya akan membahas bagaimana implementasi pengolahan data spatial menggunakan Hibernate Search.Hibernate Search adalah salah satu komponen Hibernate yang mengintegrasikan kemampuan search engine dari Apache Lucence kedalam Hiberante dan JPA model. Apache Lucene adalah pustaka yang sangat powerful untuk melakukan pencarian teks bebas secara efisien, disamping itu Lucene juga mendukung spatial search yang akan saya bahas kali ini. Hibernate Search mulai mendukung spatial search sejak versi 4.2.0.Beta1, versi terupdate ketika tulisan ini dibuat adalah 4.2.0.Beta2.

Karena masih dalam versi beta, beberapa fitur masih belum dapat digunakan dengan sempurna, salah satunya adalah error untuk query sort berdasarkan jarak terdekat.

Dalam kasus kali ini saya akan menggunakan fitur spatial search untuk menampilkan objek terdekat dari suatu koordinat dalam radius tertentu (lihat ilustrasi dibawah ini). Kumpulan objek dan koordinat disimpan dalam database. Koordinat berbasis latitude (garis lintang) dan longitude (garis bujur). 

<img src="https://lh4.googleusercontent.com/-b_S9JbJhybw/UNEtqpncnMI/AAAAAAAAADY/oa6tkD_FLDc/s800/map_3km_radius.png" title="Monas, Radius 3 KM" alt="">

Pada ilustrasi diatas titik tengah berada pada monumen nasional, lingkaran berwarna abu-abu adalah daerah dalam radius 3 KM dari titik tengah. Titik biru yang berada di dalam lingkaran adalah daerah yang terseleksi. Sedangkan titik diluarnya adalah sebaliknya.

### Kode
Untuk permulaan mari membuat desain table sederhana, karena Hibernate berbasis Object Relational Mapping (ORM) untuk contoh ini saya akan membuat sebuah JPA entity untuk mewakilkan table.
{% codeblock lang:java %}
@Entity
@Table
@Indexed
@Spatial(name = "location")
public class Location implements Coordinates, Serializable {
 private long id;
 private String name;
 private Double latitude;
 private Double longitude;
 ...
}
 {% endcodeblock %}
Anotasi ```@Indexed``` mengindikasikan bahwa class tersebut akan diindex oleh Lucene. Anotasi ```@Spatial``` wajib agar data dapat diquery, entity juga diharuskan untuk mengimplementasikan interface ```org.hibernate.search.spatial.Coordinates```.
{% codeblock lang:java %}
public interface Coordinates {
 Double getLatitude();
 Double getLongitude();
}
 {% endcodeblock %}
Selanjutnya mari membuat data awal.
<table class="table table-bordered table-condensed table-striped">
 <thead>
  <tr>
   <th>ID</th>
   <th>NAME</th>
   <th>LATITUDE</th>
   <th>LONGITUDE</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>1</td>
   <td>Krukut</td>
   <td>-6.1583</td>
   <td>106.81458</td>
  </tr>
  <tr>
   <td>2</td>
   <td>Kampung Bali</td>
   <td>-6.18476</td>
   <td>106.8187</td>
  </tr>
  <tr>
   <td>3</td>
   <td>Kebon Sirih</td>
   <td>-6.18595</td>
   <td>106.83517</td>
  </tr>
  <tr>
   <td>4</td>
   <td>Senen</td>
   <td>-6.17657</td>
   <td>106.84376</td>
  </tr>
  <tr>
   <td>5</td>
   <td>Gondangdia</td>
   <td>-6.19449</td>
   <td>106.83105</td>
  </tr>
  <tr>
   <td>6</td>
   <td>Kemayoran</td>
   <td>-6.16684</td>
   <td>106.84874</td>
  </tr>
  <tr>
   <td>7</td>
   <td>Cikini</td>
   <td>-6.19397</td>
   <td>106.84221</td>
  </tr>
  <tr>
   <td>8</td>
   <td>Gunung Sahari</td>
   <td>-6.16086</td>
   <td>106.84462</td>
  </tr>
  <tr>
   <td>9</td>
   <td>Cideng</td>
   <td>-6.17554</td>
   <td>106.80891</td>
  </tr>
  <tr>
   <td>10</td>
   <td>Jati Pulo</td>
   <td>-6.18186</td>
   <td>106.80307</td>
  </tr>
  <tr>
   <td>11</td>
   <td>Tomang</td>
   <td>-6.17307</td>
   <td>106.79646</td>
  </tr>
  <tr>
   <td>12</td>
   <td>Glodok</td>
   <td>-6.14738</td>
   <td>106.81303</td>
  </tr>
  <tr>
   <td>13</td>
   <td>Cempaka Baru</td>
   <td>-6.17059</td>
   <td>106.86522</td>
  </tr>
 </tbody>
</table>

Langkah terakhir adalah membuat query untuk menampilkan lokasi dalam radius 3 KM dari titik tengah.

{% codeblock lang:java %}
/**
 *
 * @param radius
 * Radius dalam Kilometer
 * @param lat
 * Latitude
 * @param lng
 * Longitude
 * @return List Location dalam radius dari titik tengah (lat, lng)
 */
public List<location> getLocations(int radius, double lat, double lng) {
 FullTextEntityManager em = Search.getFullTextEntityManager(entityManager);
 QueryBuilder builder = em.getSearchFactory().buildQueryBuilder().forEntity(Location.class).get();
 Query query = builder.spatial().onCoordinates("location").within(radius, Unit.KM).ofLatitude(lat).andLongitude(lng).createQuery();
 FullTextQuery q = em.createFullTextQuery(query, Location.class);

 return q.getResultList();
}
 {% endcodeblock %}
</location>
Method diatas akan mencari instance (Location) yang berada di dalam radius dari latitude dan longitude yang telah dimasukkan.
