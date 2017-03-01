---
layout: post
title: Heroku Sebagai Komputasi Modern
modified:
categories: 
description: heroku sebagai komputasi modern
tags: [komptutasi modern, heroku, cloud computing, PaaS, IaaS, IaaS]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-03-04T07:58:26+07:00
---

Pada zaman sekarang, perkembangan teknologi sangatlah pesat. Perkembangan tersebut dapat kita lihat dari lahir nya berbagai teknologi - teknologi baru seperti ada nya smartphone, penyimpanan secara online, perkembangan virtual reality dan sebagainya. Perkembangan teknologi ini sering sekali dikaitkan dengan komputasi modern, mengapa demikian ? dikarenakan pada zaman sekarang banyak sekali perkembangan teknologi khususnya di bidang komputerisasi.

## Apa Itu Cloud Computing ?

>>Cloud Computing adalah sebuah model untuk kenyamanan, akses jaringan on-demand untuk menyatukan pengaturan konfigurasi sumber daya komputasi (seperti, jaringan, server, media penyimpanan, aplikasi, dan layanan) yang dapat dengan cepat ditetapkan dan dirilis dengan usaha manajemen yang minimal atau interaksi dengan penyedia layanan [1].

Jika kita lihat dari pengertian diatas, cloud computing merupakan salah satu contoh dari komputer modern dimana jika kita melihat ke bagian user, cloud computing ini akan menyediakan storange dimana user dapat melakukan akses dimana dan kapan saja. Tidak hanya menyediakan storange, cloud computing juga berfungsi dalam hal deployment sebuah aplikasi, kita misalkan penulis mempunyai sebuah aplikasi web dimana aplikasinya akan di deploy ke sebuah server. Server yang digunakan untuk menjalankan aplikasi web telah dirancang sedemikian rupa sehingga sesuai dengan definisi cloud. Penulis memberikan contoh, misalkan server yang kita gunakan memiliki ram hanya 2 gb, ketika si aplikasi hanya menggunakan ram sebanyak 1 gb maka secara otomatis server akan menurunkan ram secara otomatis menjadi 1 gb. Bagaimana dengan ram yang 1 gb lagi ? ram tersebut akan di share ke pengguna lain yang membutuhkan ram lebih, misalnya aplikasi yang kita deploy tiba - tiba membutuhkan ram sebanyak 3 gb dikarenakan beban akses yang terlalu banyak maka secara otomatis ram akan ditambahkan.

Dari pembahasan diatas dapat kita lihat bahwa sebuah server cloud sangat lah berbeda dengan server hosting. Dimana jika kita menggunakan hosting maka automatic task seperti diatas tidak dapat dilakukan, berbeda dengan cloud computing yang dapat menjalankan secara otomatis.

### Apa Saja Model Layanan Cloud Computing ?

Jika dilihat dari segi model layanan, cloud computing terdapat 3 model yaitu : [2]

1. Software As A Service (SaaS) yaitu sebuah layanan yang dapat langsung diakses oleh user biasanya sudah dalam bantuk aplikasi. Contohnya adalah gmail, google docs, drop box dan sebagainya.
2. Platform As A Service (PaaS) yaitu pada layanan ini, semua kebutuhan user akan aplikasi sudah tersedia misalkan seperti instalasi sistem operasi, web server, database server dan lain - lain sehingga pada layanan ini, user hanya akan melakukan konfigurasi pada level aplikasi. Contohnya adalah openshift, Google App Engine dan lain - lain.
3. Infrastructure As A Service (IaaS) yaitu pada layanan ini, hampir semua bisa level bisa dipilih kecuali level hardware. Pada level ini, user dapat memilih sistem operasi, alokasi memory dan sebagainya. Contohnya adalah Digital Ocean, Microsoft Azure IaaS dan lain - lain.

Berikut adalah gambar dari ketiga model layanan tersebut.

![Screenshot from 2016-03-03 17:09:20.png](../images/Screenshot from 2016-03-03 17:09:20.png)

## Apa Hubungan Heroku Dengan Komputasi Modern ?

>>Heroku adalah adalah salah satu web hosting berbasis cloud, biasanya heroku digunakan untuk mengembangkan aplikasi web dengan berbagai bahasa pemrograman seperti java, ruby, python, node js, php dan lain - lain.

Berikut adalah gambar web dari heroku.

![Screenshot from 2016-03-03 16:37:28.png](../images/Screenshot from 2016-03-03 16:37:28.png)

Jika kita lihat dari definisinya, dapat dilihat bahwa heroku mempunyai keterkaitan yang sangat erat dengan komputasi modern. Berikut adalah beberapa faktor bahwa heroku mempunyai hubungan dengan heroku.

1. Heroku adalah salah satu web hosting berbasis cloud sehingga dapat diakses dimana dan kapan saja sehingga sesuai dengan kriteria komputasi modern.
2. Heroku termasuk ke dalam kriteria Platform As A Service (PaaS), sehingga bagi anda yang ingin melakukab deploy aplikasi ke heroku cukup hanya dengan melakukan konfigurasi aplikasi yang ingin di deploy.
3. Heroku dapat memanage sistem operasi, ram, hardisk dan sebagainya secara otomatis sehingga heroku juga dapat mendeteksi bahasa apa yang digunakan oleh sebuah aplikasi.
4. Aplikasi web yang ada di heroku hanya berjalan selama 18 jam dalam sehari, otomatisasi task tersebut adalah sebuah proses scheduling sehingga dapat dikatakan heroku adalah komputasi modern.
5. Heroku dapat melakukan deploy secara otomatis terhadap aplikasi yang kita upload melalui tool git.

Dari faktor - faktor diatas dapat disimpulkan bahwa heroku merupakan salah satu contoh dari komputasi modern.

## Kesimpulan

Cloud computing adalah salah satu dari contoh komputasi modern. Cloud computing terdapat beberapa model layanan yaitu IaaS, PaaS dan IaaS. Heroku adalah salah satu web hosting berbasis cloud computing dan heroku menerapkan model PaaS sehingga heroku adalah salah satu contoh web dari komputasi modern.

## Daftar Pustaka

* [1] Mell, Peter and Grance, Tim, 2011,The NIST Definition of Cloud Computing, National Institute of Standards and Technology (NIST), http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-145.pdf (diakses 3 Maret 2016)
* [2] Ahmad Ashari, Herri Setiawan, 2011, Cloud Computing : Solusi ICT ?
* [3] Erick Kurniawan, 2015, Penerapan Teknologi Cloud Computing Di Universitas, Studi Kasus: Fakultas Teknologi Informasi UKDW