title: "Spring Classpath Resource, beda Middleware beda Konfigurasi (Tomcat vs Glassfish)"
date: 2011-06-13 21:57:48
tags:
---

Dua hari pusing mikirin session factory dari hibernate.cfg.xml yang gak kebaca di konfigurasi spring. Yap, baru-baru ini saya lagi sibuk jalanin [cimande v2](http://java.net/projects/cimande/) di glassfish (yang alhamdulillah baru jalan lancar setelah dua hari). Ya memang kedengarannya payah, tapi mungkin bisa jadi ada juga yang memiliki nasib mirip seperti saya. Berawal dari kisah buruk itulah dengan hati tulus ikhlas saya berusaha agar teman-teman yang lain tidak mengalami kejadian yang mirip seperti yang saya alami tersebut.

Jadi begini awal mula ceritanya, dari mulai kenal [cimande](http://java.net/projects/cimande/) sampai saya gede seperti ini belum pernah sekalipun saya coba jalanin ini aplikasi di container selain tomcat (sekarang sudah :D). Kemudian suatu hari muncullah titah dari Pak Bos untuk menjalankan cimande yang terdiri dari komposisi Struts2 + Spring + Hibernate di app server yang bernama Glassfish. 1, 2, 3, beberapa saat lancar-lancar aja (sampai muncul halaman login :P) dan akhirnya hal itupun terjadi. Masalah sederhana, gak mau login. Usut punya usut ternyata database tidak mengembalikan nilai yang diminta oleh client, usut punya usut lagi ternyata hibernate session factory yang diinject oleh spring bernilai kosong alias hibernate.cfg seakan tidak pernah dilahirkan. Anehnya di tomcat lancar-lancar saja, ya saya sadar kalo masalah beginian pasti saya yang salah.

Berikut konfigurasi saya sebelumnya

<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;bean</span><span style=" color:#000000; background-color:#fbfbfb;"> id=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;sessionFactory&quot;</span><span style=" color:#000000; background-color:#fbfbfb;">
    class=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean&quot;</span><span style=" color:#000000; background-color:#fbfbfb;">
    p:dataSource-ref=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;dataSource&quot;</span><span style=" color:#000000; background-color:#fbfbfb;">
    p:hibernateProperties=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;</span><span style=" color:#2a00ff; background-color:#fbfbfb;">**<u>classpath:hibernate.properties</u>**</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;</span><span style=" color:#7f0055; background-color:#fbfbfb;">&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
    </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;property</span><span style=" color:#000000; background-color:#fbfbfb;"> name=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;eventListeners&quot;</span><span style=" color:#7f0055; background-color:#fbfbfb;">&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
        </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;map&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
            </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;entry</span><span style=" color:#000000; background-color:#fbfbfb;"> key=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;merge&quot;</span><span style=" color:#7f0055; background-color:#fbfbfb;">&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
                </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;bean</span><span style=" color:#000000; background-color:#fbfbfb;">
                    class=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;org.springframework.orm.hibernate3.support.IdTransferringMergeEventListener&quot;</span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;">/&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
            </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/entry&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
        </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/map&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
    </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/property&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
    </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;property</span><span style=" color:#000000; background-color:#fbfbfb;"> name=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;configLocations&quot;</span><span style=" color:#7f0055; background-color:#fbfbfb;">&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
        </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;list&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
            </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;value&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">**<u>classpath:hibernate*.xml</u>**</span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/value&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
        </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/list&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
    </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/property&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
</span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/bean&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
</span></pre>

Yes, setelah bertapa beberapa hari akhirnya ketemulah biang errornya (coba tebak yang mana :P)

Dan berikut ini konfigurasi setelah saya rubah.

<pre style="color:#000000;background:#fbfbfb;padding:5px;border:#cecece 1px solid;overflow:auto;"><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;bean</span><span style=" color:#000000; background-color:#fbfbfb;"> id=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;sessionFactory&quot;</span><span style=" color:#000000; background-color:#fbfbfb;">
    class=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean&quot;</span><span style=" color:#000000; background-color:#fbfbfb;">
    p:dataSource-ref=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;dataSource&quot;</span><span style=" color:#000000; background-color:#fbfbfb;">
    p:hibernateProperties=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;/WEB-INF/classes/</span><span style=" color:#2a00ff; background-color:#fbfbfb;">**<u>hibernate.properties</u>**</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;</span><span style=" color:#7f0055; background-color:#fbfbfb;">&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
    </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;property</span><span style=" color:#000000; background-color:#fbfbfb;"> name=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;eventListeners&quot;</span><span style=" color:#7f0055; background-color:#fbfbfb;">&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
        </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;map&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
            </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;entry</span><span style=" color:#000000; background-color:#fbfbfb;"> key=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;merge&quot;</span><span style=" color:#7f0055; background-color:#fbfbfb;">&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
                </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;bean</span><span style=" color:#000000; background-color:#fbfbfb;">
                    class=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;org.springframework.orm.hibernate3.support.IdTransferringMergeEventListener&quot;</span><span style=" color:#000000; background-color:#fbfbfb;"> </span><span style=" color:#7f0055; background-color:#fbfbfb;">/&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
            </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/entry&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
        </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/map&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
    </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/property&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
    </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;property</span><span style=" color:#000000; background-color:#fbfbfb;"> name=</span><span style=" color:#2a00ff; background-color:#fbfbfb;">&quot;configLocations&quot;</span><span style=" color:#7f0055; background-color:#fbfbfb;">&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
        </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;list&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
            </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;value&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">**<u>/WEB-INF/classes/hibernate*.xml</u>**</span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/value&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
        </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/list&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
    </span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/property&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
</span><span style=" color:#7f0055; background-color:#fbfbfb;">&lt;/bean&gt;</span><span style=" color:#000000; background-color:#fbfbfb;">
</span></pre>

Setelah ditelusuri ternyata ketika dijalankan pada glassfish spring tidak mengenali classpathnya di sebelah mana, sehingga harus merujuk langsung ke tempat dimana semua file dicompile yaitu pada /WEB-INF/classes, dan walhasil cimande saya berhasil jalan di glassfish. Nah pertanyaan selanjutnya bagaimana jika saya menaruh file konfigurasinya di dalam jar file? Bagi yang bisa bantu ditunggu komennya di bawah tulisan ini. Semoga bermanfaat...