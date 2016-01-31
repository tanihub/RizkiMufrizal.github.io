---
layout: post
title: Belajar Membuat Manual Book Dengan Markdown
modified:
categories: 
excerpt:
tags: [manual book, markdown, pandoc]
image:
  background: abstract-3.png
date: 2015-11-07T07:22:31+07:00
comments: true
---

Beberapa praktikum yang terdapat pada Laboratorium Teknik Informatika mempunyai tugas membuat project seperti mata praktikum pemrograman web, grafkom, pengantar kecerdasan buatan dan lain sebagainya. Masing - masing project diwajibkan membuat manual book tentang project yang dikerjakan. Pada artikel kali ini, penulis mengajak anda untuk membuat manual book dengan berbagai teknologi open source :D. Teknologi yang akan kita gunakan adalah

>>Pandoc adalah aplikasi yang dibuat oleh John MacFarlane, seorang profesor filosofi di University of California, Berkeley. Pandoc dibuat dengan bahasa pemrograman Haskell.

>>Markdown adalah sebuah markup language dengan plain text formatting syntax sehingga bisa di convert ke HTML dan format lainnya menggunakan tools yang sama. Markdown biasanya digunakan untuk mengedit readme dari sebuah open source, untuk menulis pesan di online forum, dan juga untuk membuat rich text dengan plain text editor.

>>LaTeX adalah bahasa markup atau sistem penyiapan dokumen untuk peranti lunak TeX. Tex merupakan program komputer yang digunakan untuk membuat typesetting suatu dokumen, atau membuat formula matematika. LaTeX memungkinkan penulis/penggunanya untuk melakukan typesetting dan mencetak hasil kerjanya dalam bentuk tipografi yag terbaik. Oleh karenanya LaTeX paling banyak digunakan oleh para matematikawan, ilmuwan, insinyur, akademisi, dan profesional lainnya. Pada awalnya LaTeX ditulis pada awal 1980-an oleh Leslie Lamport di SRI International. Versi paling mutakhir adalah LateX2e.

Mengapa menggunakan `markdown` ? berikut beberapa alasannya.

* Dapat di edit melalui editor apapun
* Sintak Mudah dipahami
* Dapat di convert ke dokument lain
* Sintak highlighting
* Auto generate daftar isi

##Instalasi Pandoc

Untuk melakukan instalasi silahkan jalankan perintah berikut.

{% highlight bash %}
sudo apt-get install pandoc texlive-latex-base texlive-xetex latex-beamer
{% endhighlight %}

##Download Template Manual Book

Silahkan download terlebih dahulu [template manual book markdown](https://github.com/RizkiMufrizal/Manual-Book-Markdown). Di dalam template tersebut terdapat beberapa file yaitu `01-teori.md`,`02-perancangan.md` dan lain - lain berfungsi mewakili dari masing - masing bab, misalnya `01-teori.md` mewakili dari bab 1 dan sebagainya.

##Cara Penggunaan Template

Silahkan buka template yang telah anda download dengan menggunakan editor anda misalnya dengan sublime, atom dan sebagainya. File yang akan kita ubah adalah file markdown dan latex.

###Mengubah Cover Manual Book

Silahkan buka file `template-penulisan.tex` lalu cari pada bagian 

{% highlight text %}
{% raw %}
\begin{framed}
    \begin{tabular}{ l c l }
        NPM & : & 58412085 \\
        Nama  & : & {$author$} \\
        Kelas & : & 4IA04 \\
        Jurusan & : & Teknik Informatika \\
        PJ & : & Mufrizal \\
    \end{tabular}
\end{framed}
{% endraw %}
{% endhighlight %}

Silahkan ubah sesuai dengan biodata anda. Setelah selesai, silahkan buka file `00-cover.md` dan ubah datanya seperti judul manual book, nama author dan tanggal dibuat manual book tersebut. Berikut adalah hasilnya.

![Screenshot from 2015-11-06 22:08:02.png](../images/Screenshot from 2015-11-06 22:08:02.png)

###Penulisan Section

Untuk menulis section dan subsection gunakan `#` berikut adalah contohnya :
{% highlight text %}
{% raw %}
#Teori
##Apa itu CodeIgniter ?
###Apa itu MVC ?
{% endraw %}
{% endhighlight %}

* tanda `#` berfungsi sebagai judul dari sebuah bab
* tanda `##` berfungsi sebagai section
* tanda `###` berfungsi sebagai subsection

Berikut adalah contoh hasilnya

![Screenshot from 2015-11-06 21:55:41.png](../images/Screenshot from 2015-11-06 21:55:41.png)

###Penulisan Highlighting

Salah Satu kelebihan markdown adalah sintak highlighting, untuk membuat sintak highlighting kita dapat menggunakan perintah berikut

{% highlight text %}
{% raw %}
```java
public class Latihan {
    public static void main(String[] args) {
        System.out.println("Hello Word");
    }
}
```
{% endraw %}
{% endhighlight %}

Anda tinggal mengubah tulisan `java` dengan bahasa pemrograman yang akan anda gunakan misalkan seperti php, javascript dan sebagainya. Berikut adalah hasilnya.

![Screenshot from 2015-11-06 21:58:33.png](../images/Screenshot from 2015-11-06 21:58:33.png)

###Menyisipkan Gambar

Markdown juga dapat menyisipankan yaitu melalui perintah LaTeX seperti berikut.

{% highlight text %}
{% raw %}
\begin{figure}[H]\centering\includegraphics[width=12.5cm]{gambar/mvc}\caption{Model View Controller}\end{figure}
{% endraw %}
{% endhighlight %}

Arti kodingan diatas kita akan memasukkan sebuah gambar yang berada pada folder `gambar` dengan nama file `mvc`. Disini terdapat `\caption` ini berfungsi untuk menamai gambar yang telah disisipkan. Sedangkan pada bagian `width=12.5cm` untuk menghitung lebar panjang sebuah gambar. `\centering` agar gambar yang digunakan berada pada bagian tengah dokument. Berikut adalah contoh penyisipan gambar.

![Screenshot from 2015-11-06 22:00:21.png](../images/Screenshot from 2015-11-06 22:00:21.png)

###Penulisan URL (Uniform Resource Locator)

Markdown juga mendukung penulisan URL sebuah situs. Berikut adalah contohnya.

{% highlight text %}
{% raw %}
["Heart Attack" - Demi Lovato (Sam Tsui & Chrissy Costanza of ATC)](https://www.youtube.com/watch?v=jDELybyZ4oU)
{% endraw %}
{% endhighlight %}

Diantara tanda `[]` kita dapat menyisipkan deskripsi dari pada URL tersebut sedangkan di dalam tanda `()` kita menyisipkan URL. Berikut adalah salah satu contoh URL.

![Screenshot from 2015-11-06 22:03:30.png](../images/Screenshot from 2015-11-06 22:03:30.png)

###Penulisn List Dan Nomor

Untuk menulis list atau penomoran kita dapat melakukannya dengan mudah. Berikut adalah sintak untuk menulis list.

{% highlight text %}
{% raw %}
* satu
* dua
* tiga

- empat
- lima
- enam

+ tujuh
+ delapan
+ sembilan
{% endraw %}
{% endhighlight %}

dapat dilihat bahwa penulisan untuk list dapat dilakukan dengan beberapa karakter yaitu dengan karakter `*`,`-`, dan `+`. Selanjutnya adalah untuk menulis penomoran pada markdown sangat mudah yaitu dengan perintah.

{% highlight text %}
{% raw %}
1. satu
2. dua
3. tiga
4. empat
5. lima
6. enam
7. tujuh
8. delapan
9. sembilan
{% endraw %}
{% endhighlight %}

##Convert Markdown Menjadi PDF

Langkah terakhir adalah mengubah markdown menjadi pdf. Silahkan jalankan perintah berikut.

{% highlight bash %}
pandoc \
--template template-penulisan.tex \
--variable mainfont="Droid Serif" \
--variable sansfont="Droid Sans" \
--variable fontsize=12pt \
--variable version=1.0 \
--latex-engine=xelatex --toc -N -V documentclass=report -o output.pdf 00-cover.md 01-teori.md 02-perancangan.md 03-implementasi.md 04-uji-coba.md
{% endhighlight %}

Maka akan muncul sebuah pdf dengan nama file `output`. Ini merupakan hasil akhir dari markdown, penulisan daftar isi juga disusun lebih rapi. Berikut adalah gambar dari daftar isi.

![Screenshot from 2015-11-06 22:05:36.png](../images/Screenshot from 2015-11-06 22:05:36.png)

Sekian artikel mengenai membuat manual book dengan teknologi markdown dan terima kasih :).