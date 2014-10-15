---
layout: post
title: Data Access Object (DAO) dan Service/Facade Pattern, Apa dan Mengapa
comments: true
---
Sering saya mendapat pertanyaan dari teman maupun adik kelas tentang "Gimana caranya bikin DAO?", "Kenapa harus pake DAO?", ataupun pertanyaan paling general "DAO itu apaan sih?". Maklum, saya memang yang paling ngeyel mengkritik masalah bentuk dan kerapian baris kode aplikasi bikinan rekan-rekan seperjuangan yang berhubungan dengan database/persistence, bahkan tidak jarang mengesampingkan bussines proccess apikasi itu sendiri. Yah memang terlalu teknis sih, tapi di dunia open source kerapian kode itu nomor wahid (mungkin :D), ditonton dunia gitu, berbeda dengan barang close source yang orang cuma tau itu barang tiba-tiba ada, walaupun kerapian kode itu tetap penting. Mungkin tulisan adalah sedikit bentuk curhat saya kepada banyak pihak yang menganggap saya sebagai orang yang paling meyebalkan urusan coding selama ini (sok teraniaya banget :P). Memang saya sering jawab "cari di internet" atau "kan ada google" sebelum saya sadar bahwa mau gak mau, gak mau mau, mau mau enggak, ataupun mau mau mau, sebagian siswa Indonesia malas mebaca tulisan yang sedikit ribet, sedikit panjang, ditambah lagi pake bahasa Belanda. Komplit dah, dan akhirnya memutuskan untuk bertanya pada orang sok tau seperti saya :D Berikut sedikit jawaban dari saya, semoga bermanfaat...
Data Access Object
Data Access Object (DAO) merupakan sebuah object yang menyediakan sebuah abstract interface terhadap beberapa database atau mekanisme persistence, menyediakan beberapa operasi tertentu tanpa mengekspos detail database. Penerapan konsep ini sering disebut dengan separation of concern dimana setiap kode dipisahkan berdasarkan fungsinya sehingga kode diatasnya hanya perlu mengetahui secara abstrak cara mengakses data tanpa perlu mengetahui bagaimana akses ke sumber data diimplementasikan. DAO sering dikaitkan dengan Java EE dan akses ke relational database melalu JDBC API, karena memang DAO berasal dari pedoman praktek Sun Microsystem. Kebanyakan peggunaan DAO adalah satu objek DAO untuk satu objek entity.
Berikut contoh sederhana penggunaan DAO dengan penerapan aksess ke sumber data menggunakan JPA. Dimisalkan saya mempunyai dua buah class entity yaitu barang dan kategori barang.
KategoriBarang.java
<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>public</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>class</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> KategoriBarang {
    </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>private</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>String</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> kode;
    </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>private</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>String</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> nama;
    </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>private</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>String</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> deskripsi;
    </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>private</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>List</strong></span><span style=" color:#000000; background-color:#fbfbfb;">&lt;Barang&gt; daftarBarang;
    </span><span style=" color:#3f5fbf; background-color:#fbfbfb;">/**</span><span style=" color:#000000; background-color:#fbfbfb;">
</span><span style=" color:#3f5fbf; background-color:#fbfbfb;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style=" color:#7f9fbf; background-color:#fbfbfb;"><strong>*</strong></span><span style=" color:#3f5fbf; background-color:#fbfbfb;"> getter dan setter</span><span style=" color:#000000; background-color:#fbfbfb;">
</span><span style=" color:#3f5fbf; background-color:#fbfbfb;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*/</span><span style=" color:#000000; background-color:#fbfbfb;">
}
</span></pre>
Barang.java
<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>public</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>class</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> Barang {
  
    </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>private</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>String</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> kode;
    </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>private</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>String</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> nama;
    </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>private</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>long</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> harga;
    </span><span style=" color:#7f0055; background-color:#fbfbfb;"><strong>private</strong></span><span style=" color:#000000; background-color:#fbfbfb;"> KategoriBarang kategori;
    </span><span style=" color:#3f5fbf; background-color:#fbfbfb;">/**</span><span style=" color:#000000; background-color:#fbfbfb;">
</span><span style=" color:#3f5fbf; background-color:#fbfbfb;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style=" color:#7f9fbf; background-color:#fbfbfb;"><strong>*</strong></span><span style=" color:#3f5fbf; background-color:#fbfbfb;"> getter dan setter</span><span style=" color:#000000; background-color:#fbfbfb;">
</span><span style=" color:#3f5fbf; background-color:#fbfbfb;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*/</span><span style=" color:#000000; background-color:#fbfbfb;">
}
</span></pre>
Berikut contoh penggunaa DAO untuk kedua entity tersebut
<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>class</strong></span><span style=" color:#000000;"> BarangDAO {
    </span><span style=" color:#7f0055;"><strong>private</strong></span><span style=" color:#000000;"> EntityManager entityManager;
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> setEntityManager(EntityManager entityManager) {
        </span><span style=" color:#7f0055;"><strong>this</strong></span><span style=" color:#000000;">.entityManager = entityManager;
    }
    </span><span style=" color:#7f0055;"><strong>private</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>final</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> getAllQuery = </span><span style=" color:#2a00ff;">"SELECT b FROM Barang b"</span><span style=" color:#000000;">;
    </span><span style=" color:#7f0055;"><strong>private</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>final</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> getByIdQuery = </span><span style=" color:#2a00ff;">"FROM Barang b WHERE b.kode = :kode"</span><span style=" color:#000000;">;
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>List</strong></span><span style=" color:#000000;">&lt;Barang&gt; getAll() {
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> entityManager.createQuery(getAllQuery).getResultList();
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> Barang getById(</span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> kode) {
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> (Barang) entityManager.createQuery(getByIdQuery)
                .setParameter(</span><span style=" color:#2a00ff;">"kode"</span><span style=" color:#000000;">, kode).getSingleResult();
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> save(Barang barang) {
        entityManager.persist(barang);
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> delete(Barang barang) {
        entityManager.remove(barang);
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> KategoriBarang getKategori(</span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> kodeBarang) {
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> getById(kodeBarang).getKategori();
    }
}
</span></pre>
<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>class</strong></span><span style=" color:#000000;"> KategoriBarangDAO {
    </span><span style=" color:#7f0055;"><strong>private</strong></span><span style=" color:#000000;"> EntityManager entityManager;
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> setEntityManager(EntityManager entityManager) {
        </span><span style=" color:#7f0055;"><strong>this</strong></span><span style=" color:#000000;">.entityManager = entityManager;
    }
    </span><span style=" color:#7f0055;"><strong>private</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>final</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> getAllQuery = </span><span style=" color:#2a00ff;">"SELECT b FROM KategoriBarang b"</span><span style=" color:#000000;">;
    </span><span style=" color:#7f0055;"><strong>private</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>final</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> getByIdQuery = </span><span style=" color:#2a00ff;">"FROM KategoriBarang b WHERE b.kode = :kode"</span><span style=" color:#000000;">;
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>List</strong></span><span style=" color:#000000;">&lt;KategoriBarang&gt; getAll() {
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> entityManager.createQuery(getAllQuery).getResultList();
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> KategoriBarang getById(</span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> kode) {
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> (KategoriBarang) entityManager.createQuery(getByIdQuery)
                .setParameter(</span><span style=" color:#2a00ff;">"kode"</span><span style=" color:#000000;">, kode).getSingleResult();
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> save(KategoriBarang kategori) {
        entityManager.persist(kategori);
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> delete(KategoriBarang kategori) {
        entityManager.remove(kategori);
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>List</strong></span><span style=" color:#000000;">&lt;Barang&gt; getKategori(</span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> kodeKategoriBarang) {
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> getById(kodeKategoriBarang).getDaftarBarang();
    }
}
</span></pre>
Panjang bukan? Hehehehe, tenang saja di java semuanya bisa diakalin, kita tahu bahwa beberapa method seperti getAll, getById, save, dan delete merupakan method yang umum dan sering diguakan dalam persistence, dengan kata lain method-method tersebut dapat diabstraksi. Berikut diagramnya
Java juga menyediakan generic type sejak versi 5, sehingga dalam pembuatan DAO kita dapat menciptakan sebuah mantra yang dinamakan generic DAO, fungsi utama dari generic type ini adalah untuk menghandle beberapa fungsi yang mengembalikan nilai secara dinamis, sebagai contoh diatas adalah method getById. Berikut contoh kodenya.
<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>abstract</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>class</strong></span><span style=" color:#000000;"> GenericDAO&lt;T&gt; {
    </span><span style=" color:#7f0055;"><strong>protected</strong></span><span style=" color:#000000;"> EntityManager entityManager;
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> setEntityManager(EntityManager entityManager) {
        </span><span style=" color:#7f0055;"><strong>this</strong></span><span style=" color:#000000;">.entityManager = entityManager;
    }
    </span><span style=" color:#7f0055;"><strong>protected</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>abstract</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>Class</strong></span><span style=" color:#000000;">&lt;T&gt; getEntityClass();
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>List</strong></span><span style=" color:#000000;">&lt;T&gt; getAll() {
        </span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> query = </span><span style=" color:#2a00ff;">"FROM "</span><span style=" color:#000000;"> + getEntityClass().getName();
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> entityManager.createQuery(query).getResultList();
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> T getById(</span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> id) {
        </span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> query = </span><span style=" color:#2a00ff;">"FROM "</span><span style=" color:#000000;"> + getEntityClass().getName()
                + </span><span style=" color:#2a00ff;">" WHERE kode = :kode"</span><span style=" color:#000000;">;
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> (T) entityManager.createQuery(query).setParameter(</span><span style=" color:#2a00ff;">"kode"</span><span style=" color:#000000;">, id)
                .getSingleResult();
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> save(T entity) {
        entityManager.persist(entity);
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> delete(T entity) {
        entityManager.remove(entity);
    }
}
</span></pre>
Salah satu keuntungan menggunakan generic DAO adalah menghindari duplikasi data. Sehingga kita hanya perlu membuat turunan dari generic DAO tersebut dan menambahkan beberapa method spesifik bila diperlukan.
<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>class</strong></span><span style=" color:#000000;"> BarangDAO </span><span style=" color:#7f0055;"><strong>extends</strong></span><span style=" color:#000000;"> GenericDAO&lt;Barang&gt; {
    @Override
    </span><span style=" color:#7f0055;"><strong>protected</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>Class</strong></span><span style=" color:#000000;">&lt;Barang&gt; getEntityClass() {
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> Barang.class;
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> KategoriBarang getKategori(</span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> kodeBarang) {
        </span><span style=" color:#7f0055;"><strong>return</strong></span><span style=" color:#000000;"> getById(kodeBarang).getKategori();
    }
}
</span></pre>
Service / Facade
Facade merupakan sebuah objek yang berfungsi untuk menyederhanakan kumpulan kode besar seperti library. Dalam kasus database kita dapat menggunakan facade untuk mengelompokkan beberapa DAO dalam sebuah transaksi. Business logic juga dapat diletakkan pada bagian ini.
Berikut contoh penggunaan facade
<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>interface</strong></span><span style=" color:#000000;"> BarangService {
  
    </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> saveBarang(Barang barang);
  
    </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> setKategoriBarang(</span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> kodeBarang, </span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> kodeKategori);
  
}
</span></pre>
<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>class</strong></span><span style=" color:#000000;"> BarangServiceImpl </span><span style=" color:#7f0055;"><strong>implements</strong></span><span style=" color:#000000;"> BarangService {
  
    </span><span style=" color:#7f0055;"><strong>private</strong></span><span style=" color:#000000;"> BarangDAO barangDAO;
    </span><span style=" color:#7f0055;"><strong>private</strong></span><span style=" color:#000000;"> KategoriBarangDAO kategoriBarangDAO;
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> setBarangDAO(BarangDAO barangDAO) {
        </span><span style=" color:#7f0055;"><strong>this</strong></span><span style=" color:#000000;">.barangDAO = barangDAO;
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> setKategoriBarangDAO(KategoriBarangDAO kategoriBarangDAO) {
        </span><span style=" color:#7f0055;"><strong>this</strong></span><span style=" color:#000000;">.kategoriBarangDAO = kategoriBarangDAO;
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> saveBarang(Barang barang) {
        barangDAO.save(barang);
    }
    </span><span style=" color:#7f0055;"><strong>public</strong></span><span style=" color:#000000;"> </span><span style=" color:#7f0055;"><strong>void</strong></span><span style=" color:#000000;"> setKategoriBarang(</span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> kodeBarang, </span><span style=" color:#7f0055;"><strong>String</strong></span><span style=" color:#000000;"> kodeKategori) {
        KategoriBarang kategoriBarang = kategoriBarangDAO.getById(kodeKategori);
        Barang barang = barangDAO.getById(kodeBarang);
        barang.setKategori(kategoriBarang);
        barangDAO.save(barang);
    }
}
</span></pre>
Penggabungan antara DAO dan Service pattern secara disiplin dapat menghasilkan kode yang bukan hanya mudah dibaca maupun ditesting, bahkan dapat dimodifikasi tanpa merubah modul yang menggunakan DAO maupun service, sebagai contoh ketika saya ingin mengganti akses ke database menggunakan JDBC, hanya perlu modifikasi pada bagian DAO tanpa merubah sama sekali bagian business logic pada service. Selain itu juga meningkatkan efisiensi dan kinerja dari lapisan data karena merupakan standart software yang dapat digunakan kembali (reusable).