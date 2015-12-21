---
layout: post
title: "Instalasi Perlengkapan Coding PHP"
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2015-12-21T08:00:12+07:00
---

Sebelum kita melakukan coding PHP pada linux, kita diharuskan untuk melakukan instalasi PHP untuk kebutuhan coding. Yang kita butuhkan adalah PHP, web server apache, composer dan database. Berikut adalah tahapan untuk instalasi pada linux :).

##Instalasi Apache Web Server

>>[Apache web server](https://httpd.apache.org/) adalah server web yang dapat dijalankan di banyak sistem operasi (Unix, BSD, Linux, Microsoft Windows dan Novell Netware serta platform lainnya) yang berguna untuk melayani dan memfungsikan situs web. Protokol yang digunakan untuk melayani fasilitas web/www ini menggunakan HTTP.

Nah untuk menjalankan PHP kita dapat menggunakan web server apache ini, untuk melakukan instalasi apache silahkan jalankan perintah berikut ini.

{% highlight bash %}
sudo apt-add-repository ppa:ondrej/apache2
sudo apt-get update
sudo apt-get install apache2
{% endhighlight %}

Jika telah selesai, silahkan akses `http://127.0.0.1/` jika berhasil maka akan muncul halaman web tentang web server apache seperti berikut ini.

![Screenshot from 2015-12-21 08:07:45.png](../images/Screenshot from 2015-12-21 08:07:45.png)

##Instalasi PHP

>>[PHP](https://secure.php.net/) adalah singkatan dari "PHP: Hypertext Prepocessor", yaitu bahasa pemrograman yang digunakan secara luas untuk penanganan pembuatan dan pengembangan sebuah situs web dan bisa digunakan bersamaan dengan HTML. PHP diciptakan oleh Rasmus Lerdorf pertama kali tahun 1994.

Langkah selanjutnya adalah melakukan instalasi [PHP](https://secure.php.net/), karena [PHP 7](https://secure.php.net/) udah release maka kita akan menggunakan PHP 7 :D. Untuk melakukan instalasi PHP silahkan jalankan perintah berikut ini.

{% highlight bash %}
sudo add-apt-repository ppa:ondrej/php-7.0
sudo apt-get update
sudo apt-get install php7.0-cli php7.0-mysql php7.0-fpm php7.0-gd php7.0 libapache2-mod-php7.0 php7.0-mcrypt php7.0-common php7.0-snmp snmp php7.0-curl php7.0-json php7.0-pgsql
{% endhighlight %}

Setelah selesai, kita akan melakukan testing terlebih dahulu. Karena htdocs terdapat di dalam folder `/var/www/html/` maka berikan hak akses terlebih dahulu dengan perintah

{% highlight bash %}
sudo chmod 755 -R /var/www/
{% endhighlight %}

Setelah selesai jalankan perintah `nano /var/www/html/info.php` kemudian masukkan codingan berikut ini.
{% highlight php %}
<?php phpinfo(); ?>
{% endhighlight %}

kemudian save dengan perintah `ctrl + o` dan keluar dengan perintah `ctrl + x`. Jika sudah, akses `http://127.0.0.1/info.php` pada browser maka akan muncul versi php seperti berikut ini.

![](../images/Screenshot from 2015-12-21 08:22:57.png)

atau anda dapat juga mengecek `PHP` melalui terminal dengan perintah

{% highlight bash %}
php --version
{% endhighlight %}

maka akan muncul tulisan seperti berikut .

{% highlight bash %}
PHP 7.0.1-1+deb.sury.org~trusty+2 (cli) ( NTS )
Copyright (c) 1997-2015 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2015 Zend Technologies
    with Zend OPcache v7.0.6-dev, Copyright (c) 1999-2015, by Zend Technologies
{% endhighlight %}

##Instalasi MariaDB

>>[Mariadb](https://mariadb.org/) adalah produk fork dari [MySQL](https://www.mysql.com/) yang didukung langsung dari community. Sejak diakuisisinya MySQL oleh [Oracle](http://www.oracle.com/) pada September 2010, Monty Program sebagai penulis awal kode sumber MySQL memisahkan diri dari pengembangan dan membuat versi yang lebih sendiri yakni MariaDB.

Dari definisi diatas dapat ditarik kesimpulan bahwa sebenarnya mariadb dengan mysql tidak memiliki perbedaan yang jauh sehingga tidak ada masalah jika kita menggunakan mariadb. Salah satu faktor memilih mariadb adalah disupport oleh community dan juga memiliki performance yang lebih baik dari mysql. Untuk melakukan instalasinya silahkan jalankan perintah berikut.

{% highlight bash %}
sudo apt-get install software-properties-common
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
sudo add-apt-repository 'deb [arch=amd64,i386] http://mariadb.biz.net.id/repo/10.1/ubuntu trusty main'
sudo apt-get update
sudo apt-get install mariadb-server mariadb-client
{% endhighlight %}

pada saat instalasi nantinya anda akan diminta memasukkan password seperti ini

![](../images/Screenshot from 2015-12-21 08:22:58.jpg)

silahkan masukkan password anda, lalu tekan `tab` atau `space` maka cursor akan ke bagian `<oke>` kemudian tekan enter.

Jika instalasi telah selesai, anda dapat mengakses mariadb dengan perintah

{% highlight bash %}
mysql -u root -p
{% endhighlight %}

##Instalasi PhpMyAdmin

>>[phpMyAdmin](https://www.phpmyadmin.net/) adalah perangkat lunak bebas yang ditulis dalam bahasa pemrograman PHP yang digunakan untuk menangani administrasi MySQL. phpMyAdmin mendukung berbagai operasi MySQL, diantaranya (mengelola basis data, tabel-tabel, bidang (fields), relasi (relations), indeks, pengguna (users), perijinan (permissions), dan lain-lain).

Untuk administrasi mysql kita dapat menggunakan phpMyAdmin, silahkan download di [phpMyAdmin](https://www.phpmyadmin.net/downloads/) pilih yang `phpMyAdmin-4.5.2-all-languages.tar.xz` kemudian extrak dan rename foldernya dengan `phpmyadmin`, hasilnya ektraknya silahkan anda copy ke folder `/var/www/html/` kemudian akses pada `http://127.0.0.1/phpmyadmin/` maka akan muncul halaman phpmyadmin seperti berikut ini.

![Screenshot from 2015-12-21 08:40:35.png](../images/Screenshot from 2015-12-21 08:40:35.png)

Jika terdapat error seperti ini

![Screenshot from 2015-12-21 08:40:36.png](../images/Screenshot from 2015-12-21 08:40:36.png)

maka jalankan perintah

{% highlight bash %}
sudo service apache2 restart
{% endhighlight %}

kemudian akses kembali phpmyadmin anda :).

##Instalasi Composer

Tahap selanjutnya kita akan melakukan instalasi [Composer](https://getcomposer.org/). apa itu composer ?

>>[Composer](https://getcomposer.org/) merupakan sebuah build tool untuk dependency manager terhadap library yang dibutuhkan oleh project PHP.

Buka terminal pada folder yang akan dijadikan sebagai folder composer. Kemudian jalankan perintah berikut

{% highlight bash %}
curl -sS https://getcomposer.org/installer | php
{% endhighlight %}

Jika sudah, sekarang kita akan lakukan path, jalankan perintah `sudo gedit /etc/environment` kemudian sisipakan pada baris atas seperti berikut.

{% highlight bash %}
COMPOSER_HOME=/home/rizki/programming/build-tool/composer
{% endhighlight %}

ganti isian COMPOSER_HOME dengan directori tempat instalasi composer anda. Kemudian pada bagian `PATH` tambahkan seperti berikut.

{% highlight bash %}
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/home/rizki/programming/build-tool/composer"
{% endhighlight %}

jangan lupa sesuaikan dengan folder tempat instalasi composer anda. Restart PC anda dan cek composer anda dengan perintah

{% highlight bash %}
composer.phar --version
{% endhighlight %}