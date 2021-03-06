---
layout: post
title: GIS Part 1 - Mengkonsumsi Data Tiles dengan Leaflet
comments: true
---
Sesuai dengan prefix judul posting saya kali ini, GIS (Geographic Information System) atau dalam bahasa Indonesia disebut SIG (Sistem Informasi Geografis), saya akan membahas secara tuntas implementasi sederhana dari GIS dari awal hingga akhir. Ya, saya ulangi lagi, tuntas dari awal hingga akhir (jangan lupakan kata sederhana). Kasus yang akan saya jadikan contoh kali ini adalah pengolahan informasi lokasi berdasarkan koordinat, contoh paling populer dari kasus ini adalah Foursquare dan Google Latitude. Karena pembahasan kasus ini cukup panjang, saya akan membagi postingan ini menjadi 4 bab, diantaranya:
<ul>
 <li><a href="http://blogs.mervpolis.com/roller/dwx/entry/gis_part_1_mengkonsumsi_data" target="_blank">Part 1 - Mengkonsumsi Data Tiles dengan Leaflet</a></li>
 <li><a href="http://blogs.mervpolis.com/roller/dwx/entry/gis_part_2_pencarian_informasi" target="_blank">Part 2 - Pencarian Informasi Spasial menggunakan Hibernate Search/Lucene</a></li>
 <li>Part 3 - Membuat REST API dengan S2Rest Plugin</li>
 <li>Part 4 - Mengkonsumsi Informasi Spatial dengan Leaflet dan JQuery</li>
</ul>
Lengkap bukan?
Mengkonsumsi Data Tiles dengan Leaflet
---
 Tiles adalah ubin, lalu apa hubungannya ubin dengan peta? Analogi sederhananya adalah sebagai berikut, untuk melapisi dinding kamar mandi ataupun dapur di rumah anda, diperlukan kumpulan keramik berbentuk persegi yang ditempelkan secara rapi dalam susunan grid. Konsep inilah yang mendasari tiles pada peta, yaitu kumpulan bitmap berbentuk persegi yang disusun untuk membentuk sebuah peta.
 Tiles secara umum adalah gambar berukuran 256 x 256 pixel, meskipun tidak selalu begitu, sebagai contoh CloudMade menyediakan gambar berukuran 64 x 64 pixel untuk kebutuhan mobile, namun secara keseluruhan gambar 256 x 256 pixel menjadi standart secara de facto yang dipimpin oleh Google Maps.
 Dalam implementasinya banyak pustaka yang disediakan di internet untuk mengkonsumsi tiles. Untuk menampilakan ke dalam web umumnya pustaka dibangun diatas javascript, beberapa pustaka javascript yang populer digunakan adalah:
<ul>
 <li style="text-align: justify; "><a href="https://developers.google.com/maps/" target="_blank">Google Maps API</a>. Sedikit catatan untuk penggunaan GMaps API, layanan ini bersifat komersial untuk kondisi tertentu. Google juga menentukan kebijakan dengan menyertakan iklan di dalamnya. Ingin menggunakan kode javascript GMaps pada local intranet? Sepertinya anda perlu membayar sekitar $10.000 per tahun untuk dapat melakukannya.</li>
 <li style="text-align: justify; "><a href="http://openlayers.org/" target="_blank">Openlayers</a>, digunakan oleh <a href="http://www.openstreetmap.org/" target="_blank">OpenstreetMap</a>, kaya fitur, serta opensource. Akan tetapi cukup kompleks dan memiliki ukuran yang cukup besar untuk sebuah file javascript.</li>
 <li style="text-align: justify; "><a href="http://leaflet.cloudmade.com/" target="_blank">Leaflet</a>, dikembangkan oleh CloudMade dibawah lisensi BSD. Cukup ringan, stabil dan cepat jika dibandingkan dengan Openlayers, serta desain kode berbasis OOP yang sangat mudah dipahami. Foursquare adalah salah satu website yang menggunakan Leaflet.</li>
</ul>
 Dalam contoh kali ini saya akan menggunakan leaflet sebagai pustaka untuk mengkonsumsi dan menampilkan data tiles dari Osmosa salah satu reimplementasi dari OpenstreetMap. Untuk menampilkan sebuah peta menggunakan leaflet adalah cukup mudah, berikut beberapa baris kode yang perlu ditulis untuk menampilkan sebuah peta dan menampilkan lokasi dimana anda berada.
{% codeblock lang:javascript %}
(function() {
 var map = new L.Map('map', {zoom: 10});
 var markers = new L.LayerGroup();
 map.addLayer(markers);
 // Mengambil tiles dari Osmosa.net
 var tile = new L.TileLayer('http://www.osmosa.net/osm_tiles3/{z}/{x}/{y}.png', {
 attribution: 'Tiles CC-BY-SA Osmosa, data © OpenStreetMap contributors.',
 maxZoom: 18, minZoom: 3
 });
 map.addLayer(tile);
 // Mengambil posisi user, menampilkan titik dimana user berada
 navigator.geolocation.getCurrentPosition(function(p) {
 map.setView(new L.LatLng(p.coords.latitude, p.coords.longitude), 13);
 var marker = new L.Marker([p.coords.latitude, p.coords.longitude]);
 marker.addTo(markers).bindPopup('Anda berada di sini!').openPopup();
 });
})();
 {% endcodeblock %}
### Live Demo!
<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.4.4/leaflet.css">
<!--[if lte IE 8]>
    <link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.4.4/leaflet.ie.css" />
<![endif]-->
<script type="text/javascript" src="http://cdn.leafletjs.com/leaflet-0.4.4/leaflet.js"></script>
<div class="well" style="min-height: 200px;" id="map"></div>
Browser anda mungkin akan meminta persetujuan karena contoh kode diatas mencoba melacak lokasi dimana anda berada, ijinkan untuk dapat melihat live demo. Jangan khawatir karena saya tidak akan menyimpan data lokasi atau apapun yang melanggar privasi anda.
<script type="text/javascript">
(function() {
	var map = new L.Map('map', {zoom: 10});
	
	var markers = new L.LayerGroup();
	map.addLayer(markers);
	
//'http://www.osmosa.net/osm_tiles3/{z}/{x}/{y}.png'
	var tile = new L.TileLayer('http://tile.openstreetmap.org/{z}/{x}/{y}.png', {
		attribution: 'Tiles CC-BY-SA <a target="_blank" href="http://osmosa.net/">Osmosa</a>, data &copy; <a target="_blank" href="http://openstreetmap.org/">OpenStreetMap</a> contributors.',
		maxZoom: 18, minZoom: 3
	});
	
	map.addLayer(tile);
			
	navigator.geolocation.getCurrentPosition(function(p) {
		map.setView(new L.LatLng(p.coords.latitude, p.coords.longitude), 13);
		var marker = new L.Marker([p.coords.latitude, p.coords.longitude]);
		marker.addTo(markers).bindPopup('Anda berada di sini!').openPopup();
	});
})();
</script>