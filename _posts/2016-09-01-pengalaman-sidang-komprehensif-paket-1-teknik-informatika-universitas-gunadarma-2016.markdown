---
layout: post
title: Pengalaman Sidang Komprehensif Paket 1 Teknik Informatika Universitas Gunadarma 2016
modified:
categories:
description: pengalaman sidang komprehensif paket 1 teknik informatika universitas gunadarma 2016
tags: [komprehensif, paket 1, teknik informatika, universitas gunadarma, 2016]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-09-01T23:59:47+07:00
---

Pada artikel kali ini, penulis akan membahas mengenai pengalaman penulis ketika mengikuti sidang komprehensif paket 1 teknik informatika di universitas gunadarma. Paket 1 terdiri dari mata kuliah yang bisa dibilang sedikit agak susah, mengapa demikian ? dikarenakan terdapat pemrograman web yang katanya sering disuruh ngoding di tempat tapi sebenarnya itu kembali ke pengujinya.

Penulis mendapatkan surat sidang, disana dicantumkan bahwa penulis sidang pada tanggal 1 september 2016. Penulis berangkat bersama teman - teman seperjuangan yaitu adhib dan aviv. Kami berangkat dari stasiun pondok cina sekitar jam 05:30 pagi, dan Alhamdulillah sampai di cikini jam 6:15. Sesampai disana, kami makan terlebih dahulu dan melalukan persiapan untuk menghadapi sidang.

Briefing dimulai pada jam 07:00, biasanya skripsi naik ke lantai 2 sedangkan bagi mereka yang komprehensif naik ke lantai 5. Setelah diberikan arahan, tiba waktunya pembagian sk dan kartu sidang. Penulis dipanggil dan maju ke depan untuk mengambil sk. Disana telah tertera dosen penguji dari masing - masing mata kuliah yang akan diujikan. Berikut adalah dosen penguji penulis.

| Nama Dosen            | Mata Kuliah               |
|:----------------------|:--------------------------|
|Suryadi SIS, Drs., MSc | Teori Bahasa Dan Automata |
|Haryanto, SSi., MM     | Pemrograman Web           |
|Maukar, Dr             | Rekayasa Perangkat Lunak  |
{: rules="groups"}

Ketiga dosen tersebut teryata berada pada lantai 6, akhirnya penulis menuju ke lantai 6. Disana terdapat panitia sidang, penulis langsung lapor ke bagian panitia sidang, lalu kita tunggu hingga dipanggil oleh bagian sidang. Kira -kira sekitar 20 menit, akhirnya penulis dipanggil oleh panitia sidang dan mempersilahkan masuk ke ruangan. Di dalam ruangan tersebut ketiga dosen tersebut telah hadir dan mereka duduk berseblahan. Berikut adalah percakapan pada saat sidang pemrograman web.

* Pak Haryanto : ya rizki, apa perbedaan antara pemrograman biasa dan pemrograman web ?
* Penulis : pemrograman biasa digunakan untuk membuat aplikasi pak, bisa aplikasi desktop, web dan mobile sedangkan pemrograman web biasanya untuk membuat sebuah aplikasi web pak.
* Pak Haryanto : kenapa tampilan web di hp dan di komputer berbeda ?
* Penulis : karena mereka menggunakan responsive design pak, biasanya menggunakan css
* Pak Haryanto : css itu bukan sesuatu yang sakti, bagaimana dia bisa tahu bahwa kita menggunakan android untuk mengakses web tersebut ?
* Penulis : nah disini nih mulai susah, penulis mulai mikir - mikir, sebenarnya yang bapak nya maksud mungkin gimana sih cara nya si server tau kalau yang ngakses web itu adalah perangkat android ? atau yang akses web itu adalah perangkat laptop. Akhirnya penulis menjawab user agent pak, nantinya di user agent akan disisipkan pada bagian header http.
* Pak Haryanto : user agent itu juga bukan sesuatu yang sakti loh
* Penulis : mencoba menjawab walaupun sedikit tidak yakin, sebenarnya kita bisa menggunakan meta yang ada di tag html pak, disana kita bisa mendeklarasikan berapa lebar dari device tersebut.
* Pak Haryanto : kalau kamu bilang meta berarti kamu harus tau dong tentang struktur data meta, struktur data pada meta apa ?
* Penulis : nah disini nih udah stack banget, penulis tidak bisa menjawab, akhirnya penulis bilang "saya tidak tahu pak"
* Pak Haryanto : oke lanjut ya, perbedaan html dan php apa ?
* html biasanya digunakan untuk antarmuka web pak, sedangkan php biasanya digunakan untuk melakukan sebuah proses misalnya perhitungan, koneksi ke database dan lain - lain pak.
* Pak Haryanto : IP dari localhost apa ?
* Penulis : 127.0.0.1 pak
* Pak Haryanto : oke lanjut ke sebelah, sambil nunjuk ke pak Suryadi

Nah kali ini penulis disidang oleh pak Suryadi untuk mata kuliah teori bahasa dan automata.

* Pak Suryadi : coba sebutkan grammer pada noam chomsky ?
* Penulis : ada UG, CSG, CFG dan RG pak
* Pak Suryadi : mesin nya ada apa aja ?
* Penulis : UG -> Mesin turing, CSG -> linear bounded automata (LBA), CFG -> automata pushdown, RG -> Automata Hingga
* Pak Suryadi : coba sebutkan dan jelaskan tuple pada RG ?
* Penulis : S -> stata awal, M -> transisi, Vt -> himpunan input simbol, K -> himpunan input stata, Z -> stata penerima
* Pak Suryadi : mengapa dikatakan stata penerima ?
* Penulis : dikatakan stata penerima dikarena jika terdapat sebuah kalimat dan kalimat tersebut berakhir pada stata penerima maka kalimat tersebut diterima oleh mesin tersebut pak.
* Pak Suryadi : kenapa diterima oleh mesin tersebut ?
* Penulis : ya karena bahasa tersebut sesuai dengan grammer nya pak
* Pak Suryadi : yap, mantap, saya kira kamu bakal bingung sama jawabannya, biasanya mahasiswa banyak yang kurang mengerti dibagian ini, sambil ketawa. Apa sih perbedaan nondeterministik dan deterministik ?
* Penulis : kalau AHN (deterministik) dimana 1 input simbol hanya mempunyai 1 transisi sedangkan pada AHN (nondeterministik) dimana 1 input simbol bisa mempunyai banyak transisi.
* Pak Suryadi : apakah Automata Hingga punya output ?
* Penulis : nah ini emang gx ada di materi, akhirnya penulis jawab "tidak ada pak" dan teryata benar
* Pak Suryadi : iya benar, bagaimana kita tahu kalau terjadi sebuah transisi ?
* Penulis : terjadi sebuah transisi jika sebuah stata menerima input simbol pak, nah nantinya akan ada transisi antar stata, dimana transisi ini nantinya akan ada tabel yang mendefiniskan perpindahan antar state berdasarkan simbol pak.
* Pak Suryadi : oke cukup, silahkan pindah ke samping, sambil nunjuk ke pak maukar.

Nah kali ini penulis disidang oleh pak Maukar untuk mata kuliah rekayasa perangkat lunak.

* Pak Maukar : Apa yang kamu pelajari ?
* Penulis : saya belajar mengenai tahapan pembuatan aplikasi pak
* Pak Maukar : oke coba sebutkan model - model pada tahapan pembuatan aplikasi, saya kasih kata kunci yaitu spiral
* Penulis : spiral, waterfall, incremental, v-model, prototype pak
* Pak Maukar : coba jelasin tentang waterfall
* Penulis : nah disini penulis jelasin panjang lebar sesuai dengan teori :D
* Pak Maukar : oke, coba sebutkan macam - macam testing ?
* Penulis : unit test, integrated test, system test, validated test dan disini penulis juga menjelaskan seluruh test tersebut.
* Pak Maukar : kamu tau kan top down test dan bottom up test ? nah disini bapaknya bikin model kira - kira seperti berikut.

![sidang.svg](../images/sidang.svg)

Nah menurut kamu, mana yang lebih optimal, top down test atau bottom up test ? jika benar, saya kasih nilai plus buat kamu

* Penulis : nah jadi tambah semangat buat nih buat sidang, hehehe. Disini penulis berfikir sebentar lalu penulis menjawab bottom up test pak
* Pak Maukar : alasannya ?
* Karena kita akan melakukan test module yang paling bawah terlebih dahulu pak, nah setelah module dibawah dilakukan test, baru module diatas nya kita lakukan test, dimana module diatasnya ini membutuhkan dependency dari module sebelumnya pak, Kemudian penulis membuat contoh seperti berikut.

![sidang2.svg](../images/sidang2.svg)

Nah bisa dilihat pak, dari model diatas, kita akan melakukan test terhadap module 1, 2, 3 dan 4 terlebih dahulu pak, nah module 5 itu butuh module 1 dan 2 pak, jika kita tidak test module 1 dan 2 terlebih dahulu maka besar kemungkinan akan terjadi kesalahan pak apalagi module 5 membutuhkan module 1 dan 2 pak.

* Pak Maukar : oke siip, boleh keluar

Dari dosen penguji diatas, penulis hanya mendapatkan kendala pada pweb saja tapi Alhamdulillah mendapatkan bantuan nilai plus pada saat sidang mata kuliah rekayasa perangkat lunak. Setelah selesai sidang, maka kita akan menunggu hasil sidang dan akan diumumkan hari itu juga. Pengumuman biasanya akan diumumkan sekitar pukul 14:00 atau 15:00. Pada saat Pengumuman, penulis dipanggil ke lantai 2 sebagai gelombang pertama, nah disana nantinya akan dibagi lagi menjadi kelompok - kelompok dan penulis termasuk ke dalam kelompok 2. Di gelombang 1 terdapat 3 kelompok dan Alhamdulillah ketiga kelompok tersebut lulus semua.

Yang harus dipersiapkan adalah materi dan kepahaman anda terhadap materi yang akan diujikan. Teori memang wajib anda kuasai tapi anda wajib paham akan materi tersebut karena terkadang mereka ingin tahu apakah anda telah paham tentang materi yang telah anda pelajari. Sekian mengenai pengalaman sidang komprehensif paket 1 teknik informatika universitas gunadarma 2016 dan terima kasih :D.
