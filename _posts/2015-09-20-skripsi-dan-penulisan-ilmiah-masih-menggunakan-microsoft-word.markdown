---
layout: post
title: Skripsi Dan Penulisan Ilmiah Masih Menggunakan Microsoft Word ???
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-09-20T15:40:45+07:00
---

Kebanyakan mahasiswa masih menggunakan microsoft word untuk mengerjakan skripsi dan penulisan ilmiah. Microsoft word memang salah satu tool yang sangat populer untuk kegiatan menulis dan lain sebagainya. Tapi tahu kah anda bahwa menulis sebuah skripsi dan penulisan ilmiah merupakan pekerjaan yang sangat merepotkan jika menggunakan microsoft word ? bagaimana bisa ? baiklah, penulis akan menjelaskan mengapa microsoft word dapat merepotkan anda untuk menulis ?

- yang pertama adalah masalah penomoran halaman, microsoft word diharuskan untuk melakukan setting terlebih dahulu, anda tahu bahwa halaman paling depan menggunakan angka romawi dan dimulai bab 1 kita menggunakan angka biasa, ini membutuhkan settingan page break.
- Yang kedua, tata letak penulisan, penulisan anda awalnya rapi, tapi ketika ada salah satu yang harus diganti maka otomatis dari secara tidak langsung penulisan setelahnya akan hancur terlebih jika terdapat gambar dan tabel.
- Penulisan huruf yang tidak konsisten, ini dapat dilihat dari nama bab, nama section dan lain sebagainya.
- Jarak antar spasi tidak konsisten, ada yg 1,5 ada yg 2 bahakan ada 2,5 ini membuat penulisan menjadi acak - acakan.
- Daftar isi, daftar gambar dan daftar tabel diharuskan membuat sendiri, pembuatan nama tabel juga harus dibuat oleh pengguna misalnya `gambar 3.2 : helloword`
- Untuk perpustakaan, kita diharuskan untuk membuat sendiri tanpa bantuan lain, sehingga banyak tulisan daftar pustaka yang tidak sesuai aturan penulisan.

Trus bagaimana mengatasinya ? untuk soal penulisan ada beberapa tool yang disarankan, diantaranya adalah dengan menggunakan [LaTeX](https://www.latex-project.org/) dan [Markdown](https://en.wikipedia.org/wiki/Markdown). Diantara 2 tool tersebut, LaTeX merupakan salah satu tool yang paling banyak digunakan dalam bidang akademik sedangkan markdown lebih ke arah pembuatan ebook dan dokumentasi. Untuk penulisan skripsi dan penulisan ilmiah kita gunakan LaTex saja, tapi tunggu bukankah LaTeX itu diharuskan kita untuk coding secara manual ? yups tapi jangan khawatir karena ada sebuah tool lagi yang mirip fungsinya seperti microsoft word yaitu [LyX](http://www.lyx.org/).

##Instalasi LyX

Untuk pengguna windows silahkan anda download di [LyX](http://www.lyx.org/Download) sedangkan pengguna linux, kita akan menggunakan terminal.

tambahkan repsitory terlebih dahulu.

```
sudo add-apt-repository ppa:lyx-devel/release
```

kemudian lakukan update.

```
sudo apt-get update
```

kemudian install lyx dengan perintah.

```
sudo apt-get install lyx
```

Di linux, belum tersedia bahasa indonesia yang dikenali oleh LyX sehingga kita melakukan instalasi dependencynya dengan perintah.

```
sudo apt-get install texlive-lang-other
```

##Menggunakan LyX dan Template LyX

Silahkan download template LyX pada repository github saya di [TemplatePenulisanLyX](https://github.com/RizkiMufrizal/TemplatePenulisanLyX) kemudian buka file yang berektensi `.lyx` misalnya disini saya membuka template skripsi. maka hasilnya seperti ini.
![Screenshot from 2015-09-20 14:57:16.png](../images/Screenshot from 2015-09-20 14:57:16.png)

###Mengganti Nama Penulis, Dosen Pembimbing Dst

- Klik menu document lalu pilih settings
- lalu pilih menu LaTex Preamble
- Anda dapat mengganti nama, nama dosen pembimbing, kajur, dan lain sebaginya. Berikut contoh gambarnya
![Screenshot from 2015-09-20 15:01:05.png](../images/Screenshot from 2015-09-20 15:01:05.png)

###Membuat Table
- Klik menu insert lalu pilih float lalu pilih table
- maka dibuatkan sebuah frame beserta tabel yang keberapa, silahkan anda isi nama tabel tersebut
- Untuk memasukkan tabel, klik insert lalu pilih table, anda dapat memilih berapa kolom dan berapa baris.
- Untuk membuat agar tabel di tengah, klik kanan di dalam frame tabel lalu pilih paragraft setting lalu pilih center. Berikut merupakan hasilnya.
![Screenshot from 2015-09-20 15:07:19.png](../images/Screenshot from 2015-09-20 15:07:19.png)

###Memasukkan Gambar
- Klik menu insert lalu pilih float lalu pilih figure
- maka dibuatkan sebuah frame beserta gambar yang keberapa, silahkan anda isi nama gambar tersebut
- Untuk memasukkan tabel, klik insert lalu pilih Graphics, lalu pilih browse, cari gambar anda dan tekan oke.
- Untuk membuat agar gambar di tengah, klik kanan di dalam frame gambar lalu pilih paragraft setting lalu pilih center. Berikut merupakan hasilnya.
![Screenshot from 2015-09-20 15:10:35.png](../images/Screenshot from 2015-09-20 15:10:35.png)

###Membuat Kutipan
- Cari kalimat yang merupakan kutipan
- Kemudian pindahkan kursor pada akhir kalimat dikarenakan kutipan ada di akhir
- Pilih menu insert dan pilih citation
- pada menu citation, pada kolom `available citation` terdapat nama pengarang, pilih salah satu lalu add sehingga akan muncul pada kolom `selected citation`
- Kemudian klik oke, maka akan muncul kutipan tersebut. Berikut adalah gambarnya.
![Screenshot from 2015-09-20 15:17:01.png](../images/Screenshot from 2015-09-20 15:17:01.png)

###Membuat Daftar Pustaka
- Download terlebih dahulu [Jabef](http://jabref.sourceforge.net/) yang digunakan untuk memasukkan daftar isi
- Jabref bersifat portable sehingga dapat langsung dibuka
- Buka jabref, pilih menu file lalu pilih open database, pilih file `biblio.bib` maka tampilannya akan seperti ini.
![Screenshot from 2015-09-20 15:21:48.png](../images/Screenshot from 2015-09-20 15:21:48.png)
- Kemudian kita akan mencari daftar pustaka dengan bantuan [google scholar](https://scholar.google.co.id/), buka google scholar lalu cari sebuah pengarang dengan judul bukunya misalnya `ifnu bima java desktop` maka akan muncul seperti ini.
![Screenshot from 2015-09-20 15:24:24.png](../images/Screenshot from 2015-09-20 15:24:24.png)
- Pilih kutipan maka akan muncul sebuah popup, lalu pilih `BibTeX` maka akan muncul codingan BibTex
- copy codingan BibTex, kemudian ke jabref pilih menu bibtex lalu pilih new entry lalu pilih buku
- Akan muncul menu bibtex untuk mengisi data dari daftar perpustakaan, buka tab bibtex source, lalu pastekan codingan tadi save dan haslinya seperti ini.
![Screenshot from 2015-09-20 15:28:55.png](../images/Screenshot from 2015-09-20 15:28:55.png)
- Tahap terakhir adalah melakukan setting daftar pustaka pada LyX, Buka pada halaman akhir, terdapat tulisan `BibTex generated bibliography` klik tulisan tersebut maka akan muncul sebuah pop up. Pada bagian style pilih `apalike`. Ini Merupakan format Model Harvard/APA, kemudian pilih oke dan berikut hasil akhirnya.
![Screenshot from 2015-09-20 15:32:54.png](../images/Screenshot from 2015-09-20 15:32:54.png)

###konversi ke PDF
Untuk mendapatkan penulisan yang rapi, LyX dapat mengubah penulisan LaTeX menjadi pdf sehingga dapat dibuka di komputer mana pun. Untuk melakukan konversi dengan menggunakan tombol `ctrl + R`

Demikian tutorial singkat mengenai penulisan skripsi dan penulisan ilmiah, Semoga menjadi bahan pertimbangan ketika anda menulis dan terima kasih :).
