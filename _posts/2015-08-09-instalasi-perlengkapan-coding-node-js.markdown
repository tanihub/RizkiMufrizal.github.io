---
layout: post
title: Instalasi Perlengkapan Coding Node JS
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-08-09T16:45:24+07:00
---

Perkembangan teknologi sangat pesat hingga menghasilkan berbagai teknologi. Diantaranya adalah berkembangannya bahasa pemrograman javascript. Javascript biasanya hanya berkerja pada bagian front end sebuah project akan tetapi pada hari ini, javascript telah dapat bekerja di bagian back end (server side) sebuah project. Javascript yang dapat jalan pada bagian server side adalah node js.

>[Node JS](https://nodejs.org/) adalah sebuah platform software yang dipakai untuk membangun aplikasi – aplikasi serverside yang fleksibel di sebuah network / jaringan. Node.js menggunakan JavaScript sebagai bahasa pemrogaman dan dapat dengan mudah menghasilkan throughput / pemrosesan tingkat tinggi melalui non-blocking I/O. Node.js memiliki fitur built-in HTTP server library yang menjadikannya mampu menjadi sebuah web server tanpa bantuan software lainnya seperti Apache atau Nginx.

[Node js](https://nodejs.org/) pertama kali dibuat oleh Ryan Dahl pada tahun 2009 yang kemudian berkembang pesat di bawah licensi Open Source MIT oleh sebuah perusahaan bernama Joyent Inc. Pada hakikatnya Node js dikembangkan berdasarkan teknologi Google V8 JavaScript engine serta berisi kompilasi skrip inti dan banyak modul siap pakai yang bermanfaat sehingga pengguna (dalam hal ini web developer) tidak perlu melakukan coding dan mendesain segalanya dari awal.

Berbagai framework juga muncul pada perkembangan node js diantaranya adalah

- express
- sails
- meteor
- Hapi
- Kraken
- Strong Loop
- dan lain - lain

Baiklah kita akan memulai bagaimana instalasi node js pada linux. Sebelum melakukan install node js, anda diwajibkan untuk melakukan instalasi version control git. Bagi yang belum melakukan instalasi git, silahkan lihat di tutorial [belajar git](http://rizkimufrizal.github.io/belajar-git). Yang perlu diperhatikan pada langkah instalasi node js adalah

- Clone NVM
- Instalasi dan konfigurasi Node JS
- Latihan Sekilas Tentang Node JS

##Clone NVM

Langkah selanjutnya adalah melakukan instalasi [NVM](https://github.com/creationix/nvm). Jika pada [instalasi ruby](http://rizkimufrizal.github.io/instalasi-perlengkapan-coding-ruby/) kita mengenal dengan [RVM(ruby version manager)](https://rvm.io/) maka pada node js juga terdapat [NVM(node version manager)](https://github.com/creationix/nvm). Bedanya adalah pada NVM kita harus melakukan clone repository NVM dengan menggunakan version control git. Untuk melakukan clone, jalankan perintah berikut.

{% highlight bash %}
{% raw %}
git clone https://github.com/creationix/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags`
{% endraw %}

{% endhighlight %}

NVM telah di clone pada folder .nvm, untuk melihat folder tersebut pada linux, kita harus tampilkan terlebih dahulu dengan perintah `ctrl + H` dikarenakan folder tersebut bersifat hidden.

##Instalasi dan konfigurasi Node JS

Kemudian kita akan melakukan PATH terhadap nvm. Jalankan perintah berikut.

`sudo gedit /etc/environment`

kemudian masukkan variabel berikut pada bagian atas.

`NVM_HOME=/home/rizki/.nvm`

Sesuaikan dengan directory anda. Kemudian tambahkan pada bagian PATH seperti ini.

{% highlight bash %}

PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/java-8-oracle/bin:/home/rizki/programming/build-tool/apache-maven/bin::/home/rizki/.rvm/bin:/home/rizki/.nvm"

{% endhighlight %}

Kemudian masuk ke folder .nvm dengan perintah

`cd ~/.nvm/`

kemudian jalankan perintah berikut untuk inisialisasi script nvm pada `.bashrc`

{% highlight bash %}

chmod a+x install.sh
./install.sh

{% endhighlight %}

Kemudian restart komputer anda dan lakukan pengecekan nvm dengan perintah.

`nvm --version`

Jika berhasil, kita akan melakukan instalasi node js dengan perintah.

`nvm install stable`

tunggu hingga proses download node js selesai. Node js yang di download adalah versi yang paling stable. Untuk mengecek versi node js dapat menggunakan perintah.

`nvm list-remote`

sedangkan jika ingin melihat list node js yang telah diinstall di pc dengan perintah.

`nvm list`

Untuk menghapus node js yang lama dapat dengan menggunakan perintah.

`nvm uninstall versi yang mw di uninstall`

contohnya seperti `nvm uninstall v0.12.6`. Untuk mengaktifkan node js yang paling stable maka gunakan perintah berikut.

`nvm alias default stable`

lakukan pengecekan node js pada terminal dengan perintah.

{% highlight bash %}

node -v
npm -v

{% endhighlight %}

##Latihan Sekilas Tentang Node JS

Setelah perjalanan panjang instalasi node js, kita mulai belajar sedikit bagaimana menggunakan node js :D. Buatlah sebuah folder untuk project kita, jika lewat terminal dengan perintah `mkdir BelajarNodeJS`. Kemudian buat sebuah file javascript dengan nama `app.js`, kemudian masukkan codingan berikut.

{% highlight js %}

var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(3000, '127.0.0.1');

console.log('Server running at http://127.0.0.1:3000/');

{% endhighlight %}

Sintak diatas berfungsi untuk membuat sebuah server, kemudian kita menampilkan tulisan hello word pada saat user melakukan akses pada `http://127.0.0.1:3000/` atau `http://localhost:3000/`. Untuk menjalankan servernya dengan perintah.

`node app.js`

###Instalasi Nodemon

Untuk mempermudah pengembangan aplikasi, kita membutuhkan server yang bisa autoreload ketika developer mengubah source code. Salah satu tool yang mendukung tersebut adalah dengan menggunakan [Nodemon](http://nodemon.io/). Untuk melakukan instalasi cukup dengan menggunakan perintah.

`npm install -g nodemon`

perintah `-g` berarti bersifat global. Kemudian untuk menjalankan source code tadi hanya dengan menggunakan perintah.

`nodemon app.js`

Salah satu kelebihan nodemon adalah dapat menjalankan source code dalam bentuk [coffee script](http://coffeescript.org/) dan terdapat fitur autoreload server. Coba anda ganti source code tanpa mematikan server, kemudian di save maka nodemon akan melakukan reload server.

###Instalasi Bower

Bower merupakan sebuah tool yang digunakan untuk melakukan manajemen kelengkapan library web seperti js, css dan img. Bower mirip dengan npm, bedanya adalah npm lebih di khususkan untuk manajemen kelengkapan library javascript. Baiklah untuk melakukan instalasi bower dapat dilakukan dengan perintah.

`npm install -g bower`

Sekian tutorial tentang instalasi perlengkapan coding node js, semoga bermanfaat dan terima kasih :).
