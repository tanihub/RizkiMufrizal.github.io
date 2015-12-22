---
layout: post
title: "Belajar Instalasi Software Di Linux"
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2015-12-09T18:32:13+07:00
---

Akhir - akhir ini, linux sudah banyak digunakan terutama mahasiswa yang suka ngoprek hal baru. Akan tetapi terdapat kendala diantara user windows yang migrasi ke linux yaitu masalah instalasi software. Tidak hanya di kalangan orang awam, bahkan mahasiswa juga mengalami hal yang serupa. Jika pada windows kita hanya klik pada file `setup` lalu tinggal klik tombol `next` dan `finish` selesai, hal ini berbeda dengan linux, bagaimana caranya ? sebelum kita membahas cara instalasi linux, penulis akan terlebih dahulu membahas tentang management package di dalam linux karena management package merupakan hal dasar yang harus dimengerti oleh seorang linux user :D.

##Package Management Pada Linux

Setiap distro linux memiliki `package management` yang berbeda - beda. Apa itu `Distro linux` ?

>>Distro Linux (singkatan dari distribusi Linux) adalah sebutan untuk sistem operasi komputer dan aplikasinya, merupakan keluarga Unix yang menggunakan kernel Linux. Distribusi Linux bisa berupa perangkat lunak bebas (open source) dan bisa juga berupa perangkat lunak komersial seperti Red Hat Enterprise, SuSE, dan lain-lain.

Dari pengertian diatas dapat kita simpulkan bahwa setiap yang menggunakan kernel linux maka dia termasuk salah satu distro linux. Berikut adalah contoh distro linux.

* Ubuntu
* Open Suse
* Linux Mint
* Fedora
* Red Hat
* Slackware
* Centos
* Elementary OS
* Kali Linux
* Dan lain - lain

Itu merupakan beberapa contoh dari distro linux, dan ada banyak sekali distro linux yang telah dikembangkan. Dari sekian banyak linux, yang menjadi induknya ada 3 distro yaitu `debian`, `slackware` dan `red hat`. Dari ketiga dsitro tersebut maka dikembangkan lah distro - distro lain seperti ubuntu, open suse, fedora dan lain sebagainya. Setiap distro yang merupakan turunan dari distro sebelumnya maka distro tersebut akan mengikuti `package management` pada distro sebelumnya, Apa itu `Package Management` ?

>>package management (atau package management system atau sistem manajemen paket) adalah kumpulan perangkat untuk mengotomatisasi proses instalasi, upgrade (perbaikan), konfigurasi, atau menghapus paket perangkat lunak dari sebuah komputer menggunakan cara tertentu.

Dari pengertian diatas dapat kita simpulkan bahwa package management setiap distro beda - beda. Pada artikel ini, penulis hanya membahas tentang package management pada keluarga debian. Berikut adalah contoh dari keluarga debian

* Debian
    * Ubuntu
        * Ubuntu Mate
        * Kubuntu
        * Xubuntu
        * Elementary OS
    * Kali Linux

Bisa dilihat bahwa distro ubuntu merupakan turunan dari distro debian sehingga package management yang akan digunakan oleh ubuntu akan sama seperti pada debian. Begitu pula halnya seperti distro ubuntu mate, kubuntu dan lain sebagainya. `Package management` yang terdapat pada keluarga debian yaitu `apt-get`, `apt-get` merupakan `package management` yang terdapat pada keluarga debian, salah satu fungsinya adalah mempermudah user dalam mencari dependency yang dibutuhkan oleh sebuah aplikasi, misalnya penulis ingin melakukan instalasi aplikasi `daftar barang`, nah aplikasi ini membutuhkan aplikasi lain untuk bisa jalan misalnya dia membutuhkan `gcc` maka secara otomatis `package management` akan mencari dependency yang dibutuhkan oleh aplikasi tersebut. `apt-get` sebenarnya adalah sebuah front end sedangkan back end nya adalah `dpkg` yang akan melakukan instalasi software. Bedanya apa ? bedanya adalah `apt-get` dapat mencari dependency sebuah aplikasi sedangkan `dpkg` tidak dapat mencari dependency tetapi `dpkg` hanya bertugas untuk melakukan instalasi sebuah aplikasi.

##Instalasi Software Dengan Package Management (apt-get)

Setelah mengetahui bagaimana package management pada keluarga debian, selanjutnya kita akan melakukan instalasi sebuah aplikasi. Misalnya disini penulis ingin melakukan instalasi software sticky notes pada ubuntu. Untuk melakukan instalasi sticky notes kita membutuhkan yang namanya `PPA`.

>>PPA ("Personal Package Archive") adalah penyedia repositori buatan pihak ketiga di Launchpad yang dapat anda gunakan untuk menginstal (atau upgrade) paket yang tidak tersedia dalam repository Ubuntu resmi.

Mengapa menggunakan PPA ? dikarenakan di dalam repository ubuntu resmi tidak menyediakan software - software yang kita inginkan misalnya seperti `java oracle` dan sebagainya. Untuk menambahkan PPA berikut adalah bentuk umumnya :

{% highlight bash %}
sudo add-apt-repository ppa:Pembuat PPA/Nama PPA
{% endhighlight %}

Jika kita ingin melakukan instalasi sticky notes maka kita akan menambahkan PPA seperti berikut ini, jangan lupa bahwa perintah ini menggunakan terminal linux.

{% highlight bash %}
sudo add-apt-repository ppa:umang/indicator-stickynotes
{% endhighlight %}

Jika berhasil maka akan muncul gambar seperti berikut.

![Screenshot from 2015-12-10 09:36:29.png](../images/Screenshot from 2015-12-10 09:36:29.png)

Setelah selesai, selanjutnya silahkan update repository di dalam komputer anda dengan perintah

{% highlight bash %}
sudo apt-get update
{% endhighlight %}

Jika telah selesai melakukan update maka hasilnya akan seperti ini.

![Screenshot from 2015-12-10 09:40:25.png](../images/Screenshot from 2015-12-10 09:40:25.png)

Langkah selanjutnya adalah melakukan instalasi sticky notes nya sendiri, berikut adalah bentuk umum instalasi dengan `apt-get`.

{% highlight bash %}
sudo apt-get install Nama Software
{% endhighlight %}

maka untuk instalasi sticky notes jalankan perintah berikut

{% highlight bash %}
sudo apt-get install indicator-stickynotes
{% endhighlight %}

Jika berhasil maka sticky notes akan terinstall dan diterminal anda akan muncul seperti ini.

![Screenshot from 2015-12-10 09:45:16.png](../images/Screenshot from 2015-12-10 09:45:16.png)

##Instalasi Software Dengan dpkg (Debian Package Manager)

Sebelumnya kita melakukan instalasi dengan menggunakan `apt-get` nah kali ini kita akan menggunakan fungsi `dpkg` untuk melakukan instalasi software. Syarat yang dibutuhkan untuk instalasi software dengan menggunakan dpkg adalah kita diharuskan mendownload file executable dengan extensi `.deb`. Misalnya penulis ingin melakukan instalasi sofware `mysql workbench`, silahkan anda download terlebih dahulu di [Mysql Workbench](https://dev.mysql.com/downloads/workbench/) jangan lupa sesuaikan dengan versi ubuntu anda.

Berikut merupakan bentuk umum untuk melakukan instalasi dengan menggunakan dpkg

{% highlight bash %}
sudo dpkg -i File Executable.deb
{% endhighlight %}

Setelah selesai mendownload file executable `mysql workbench` langkah selanjutnya pindahkan file tersebut ke folder `home`. Kemudian jalankan perintah berikut.

{% highlight bash %}
sudo dpkg -i mysql-workbench-community-6.3.5-1ubu1404-amd64.deb
{% endhighlight %}

Beberapa software jika kita melakukan instalasi dengan `dpkg` akan mengeluarkan `error`, contohnya adalah ketika kita melakukan instalasi `mysql workbench` akan muncul pesan error seperti berikut ini.

![Screenshot from 2015-12-10 10:03:06.png](../images/Screenshot from 2015-12-10 10:03:06.png)

Dari gambar diatas dapat dilihat bahwa software `mysql workbench` diinstall akan tetapi terdapat error sehingga software tidak dapat dibuka. Bagaimana cara mengatasinya ? silahkan scroll terminal anda ke atas maka terdapat tulisan seperti ini.

![Screenshot from 2015-12-10 10:03:24.png](../images/Screenshot from 2015-12-10 10:03:24.png)

Artinya adalah bahwa `mysql workbench` membutuhkan beberapa dependency library akan tetapi library tersebut belum diinstall di dalam komputer anda. Untuk mengatasi dependency library ini maka kita harus menggunakan fungsi `apt-get` silahkan sambungkan internet anda lalu jalankan perintah berikut.

{% highlight bash %}
sudo apt-get install -f
{% endhighlight %}

Fungsi perintah diatas adalah dia akan mencarikan dependency library apa saja yang dibutuhkan oleh software `mysql workbench` sehingga akan muncul seperti ini.

![Screenshot from 2015-12-10 10:14:01.png](../images/Screenshot from 2015-12-10 10:14:01.png)

Lalu ketik huruf `y` dan tekan `enter` maka dia akan mendownload dependency library dan melakukan instalasi software `mysql workbench`, Jika berhasil maka akan muncul seperti ini.

![Screenshot from 2015-12-10 10:16:50.png](../images/Screenshot from 2015-12-10 10:16:50.png)

##Uninstall Software

Setelah mengetahui bagaimana instalasi software, maka langkah selanjutnya adalah bagaimana cara uninstall software. Ada 2 cara, berikut adalah bentuk umumnya.

{% highlight bash %}
sudo apt-get remove Nama Sofware
{% endhighlight %}

dan 

{% highlight bash %}
sudo apt-get purge Nama Sofware
{% endhighlight %}

Perbedaanya adalah ketika menggunakan remove maka software akan diunstall sedangkan dengan menggunakan purge maka software diuninstall berserta konfigurasi softwarenya. Penulis akan melakukan uninstall terhadap software sticky notes maka jalankan perintah berikut.

{% highlight bash %}
sudo apt-get purge indicator-stickynotes
{% endhighlight %}

maka hasil akhirnya adalah seperti berikut ini.

![Screenshot from 2015-12-10 10:31:50.png](../images/Screenshot from 2015-12-10 10:31:50.png)

##Membuat Shortcut Software

Bagaimana membuat shortcut pada ubuntu jika software tersebut tidak perlu diinstall seperti Eclipse, IntelliJ IDEA, Spring Tool Suite, dan Andoid Studio ? kita bisa menggunakan aplikasi lain untuk membuat shortcut aplikasi tersebut. Silahkan jalankan perintah berikut.

{% highlight bash %}
sudo apt-get install --no-install-recommends gnome-panel
{% endhighlight %}

Misalnya penulis ingin membuat shortcut untuk spring tool suite, silahkan download terlebih dahulu di [Spring Tool Suite](https://spring.io/tools/sts/all) dan extrack. Setelah selesai buka terminal dan jalankan perintah.

{% highlight bash %}
sudo gnome-desktop-item-edit --create-new /usr/share/applications/
{% endhighlight %}

maka akan muncul sebuah windows `create launcher` silahkan isi sesuai dengan nama aplikasi, pada bagian command silahkan browser file executable. Setelah selesai lalu jalankan perintah berikut untuk memberikan hak akses.

{% highlight bash %}
sudo chmod a+x /usr/share/applications/Spring\ Tool\ Suite.desktop
{% endhighlight %}

Setelah selesai silahkan cari aplikasi tersebut di dashboard ubuntu anda :D. Sekian artikel mengenai belajar instalasi software di linux dan terima kasih :).