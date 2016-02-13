---
layout: post
title: Belajar Membuat Aplikasi Chat Dengan WebSocket
modified:
categories:
description: belajar membuat aplikasi chat dengan websocket menggunakan node js
tags: [chat, web socket, ajax, aplikasi chat, node js, express js, socket.io]
image:
  background: abstract-3.png
comments: true
share: true
date: 2015-10-14T15:43:18+07:00
---

Yups kali ini kita akan mencoba membuat sebuah aplikasi chat :D dengan menggunakan teknologi WebSocket. Apa itu WebSocket ?

>>WebSocket merupakan protokol yang digunakan untuk komunikasi dua arah. Berbeda dengan AJAX (Asynchronous JavaScript and XMLHTTP) yang hanya menggunakan komunikasi satu arah.

Pada AJAX, server menunggu request dari client sehingga jika client tidak mengirim request maka server tidak akan mengembalikan response. Berbeda dengan WebSocket, WebSocket dapat mengirim response ke client meskipun client tidak mengirim request. Penggunaan WebSocket sangat disarankan di beberapa tempat seperti pada aplikasi chat, aplikasi monitoring naik turun saham sebuah perusahaan, aplikasi monitoring naik turun kurs dan lain sebagainya. Dengan menggunakan komunikasi dua arah sehingga memungkinkan WebSocket menjadikan sebuah aplikasi berbasis realtime. Berikut adalah perbandingan sequence diagram pada AJAX dan WebSocket.

![Screenshot from 2015-10-14 15:22:24.jpg](../images/Screenshot from 2015-10-14 15:22:24.jpg)

Sebelum adanya WebSocket terdapat beberapa teknik untuk membuat aplikasi realtime yaitu.

* Pooling : Ini merupakan teknik yang sangat lama, dimana client setiap N (dalam angka) menit akan melakukan request secara berkala, sehingga apabila ada perubahan pada server, maka client akan ikut berubah akan tetapi disini terdapat kekurangan yaitu beban server akan semakin besar dikarenakan setiap N (dalam angkan) menit akan melakukan request ke server dan bayangkan jika terdapat ribuan client.

* Piggyback : Teknik ini akan mengembalikan sebuah response perubahan jika si client melakukan request ke server, dan perubahan ini dikirim melalui response, terdapat kelamahan juga yaitu apabila si client tidak melakukan request maka perubahan pun tidak akan terjadi.

* Reverse Ajax / Comet/ Long Pooling  : merupakan teknik yang efisien yaitu client melakukan request ke server, server tidak akan langsung mengembalikan response akan tetapi server akan membuka request dalam waktu tertentu, jika sudah pada waktunya, server akan mengembalikan response ke semua client. Teknik ini bisa realtime akan tetapi membutuhkan waktu karena server membuka request dalam waktu tertentu.

Oke dari sekian teknik, akhirnya kita menggunakan teknik WebSocket dikarenakan beberapa alasan, diantaranya WebSocket memungkinkan komunikasi dua arah, teknologi Html 5 telah mendukung WebSocket dan lain sebagainya.

Pada tutorial ini, kita akan membangun sebuah aplikasi chat dengan menggunakan node js. Mengapa dengan node js ? dikarenakan node js mendukung teknologi I/O Non Blocking sehingga mempermudah kita dalam mengembangkan aplikasi yang bersifat realtime.

Berikut tahapan yang akan kita lakukan.

* Setup Kebutuhan Server
* Setup Kebutuhan Client
* Membuat Server WebSocket Dengan Socket.IO
* Membuat Client WebSocket Dengan Socket.IO Dan Material Design
* Uji Coba Aplikasi Chat
* Deploy Aplikasi ke Heroku

##Setup Kebutuhan Server

Seperti biasa, buat sebuah folder dengan nama `Belajar-WebSocket-Socket.IO` kemudian masuk ke folder tersebut dengan terminal. Jalankan perintah `npm init` lalu input data anda sesuai dengan yang diminta. Kita membutuhkan beberapa library dan framework untuk membuat websocket, maka jalankan perintah berikut.

{% highlight bash %}
npm install express jade socket.io --save
{% endhighlight %}

##Setup Kebutuhan Client

Jalankan perintah `bower init`, sama seperti `npm init` input data sesuai dengan yang diminta. Pada tutorial ini, kita akan menggunakan material design, maka memerlukan beberapa library. Jalankn perintah berikut.

{% highlight bash %}
bower install bootstrap-material-design bootstrap socket.io-client --save
{% endhighlight %}

##Membuat Server WebSocket Dengan Socket.IO

Buatlah sebuah file `app.js` di dalam root folder project. Berikut adalah codingan untuk servernya.

{% highlight js %}
'use strict';

var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);
var path = require('path');

app.set('port', process.env.PORT || 3000);
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

app.use(require('express').static(path.join(__dirname, 'public')));
app.use(require('express').static(path.join(__dirname, 'bower_components')));

app.get('/', function(req, res) {
  return res.render('index');
});

io.on('connection', function(socket) {
  socket.on('chat:pesan', function(pesan) {
    io.emit('chat:pesan', pesan);
  });
});

http.listen(app.get('port'), function() {
  console.log('Server jalan di port ' + app.get('port'));
});
{% endhighlight %}

Berikut penjelasan dari codingan diatas :

* `var app = require('express')();` berfungsi untuk mendeklarasikan variabel untuk framework express js
* `var http = require('http').Server(app);` berfungsi untuk membuat server
* `var io = require('socket.io')(http);` berfungsi untuk membuat server dengan teknologi websocket
* `var path = require('path');` berfungsi sebagai path dari aplikasi
* `app.set('port', process.env.PORT);` berfungsi untuk deklarasi port dari aplikasi
* `app.set('views');` berfungsi untuk deklarasikan folder view / halaman html
* `app.set('view engine', 'jade');` berfungsi untuk mendeklarasikan bahwa kita menggunakan template engine [Jade](http://jade-lang.com/)
* `app.get('/'` berfungsi untuk melakukan render terhadap file index.jade, ini berfungsi sebagai root halaman dari aplikasi
* `io.on('connection', function(socket)` berfungsi untuk inisialisasi koneksi dengan websocket
* `socket.on('chat:pesan', function(pesan)` function ini menunggu event sebuah request dari client, sedangkan `chat:pesan` merupakan perintah socket yang akan dikirim dari client, jika perintah sama maka perintah yang di dalam function ini akan dijalankan. `pesan` yang merupakan parameter dari function diatas berfungsi sebagai data yang dikirim dari client, data dapat berupa object atau array dalam format [JSON](http://www.json.org/).
* `io.emit('chat:pesan', pesan);` berfungsi untuk memberikan response ke seluruh client, seperti pengertian diatas, jika client tidak melakukan request, server tetap dapat memberikan response ke client. Nah disini hanya 1 client yang melakukan request maka seluruh client nantinya akan mendapatkan response agar dapat yang diterima antara 1 client dengan clientnya sama.
* `http.listen(app.get('port')` berfungsi untuk menjalankan server.

Panjang juga penjelasannya :D , sekarang kita mulai coding untuk bagian clientnya.

##Membuat Client WebSocket Dengan Socket.IO Dan Material Design

Untuk client, kali ini kita akan gunakan material design, loh knp gx pakai bootstrap aja ? alasannya penulis pengen nyobain material design :P . Bosen kan kalau terus - terusan pakai bootstrap, sekali - kali pakai material design biar bagus tampilannya. Oke silahkan buat sebuah folder views di dalam root folder project. buat file `layout.jade` di dalam folder views, `layout.jade` berfungsi sebagai layout utama sehingga kita akan lakukan import js, css, img dan lain - lain disini. Berikut kodingannya.

{% highlight jade %}
doctype html
html(lang='en')
  head
    meta(charset='utf-8')
    meta(name='viewport', content='width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no')
    meta(name='description', content='Belajar WebSocket')
    meta(name='keywords', content='keywords')
    meta(name='author', content='Rizki Mufrizal')
    meta(name="robots", content="index, follow")

    block title
      title Default title

    //- ----- CSS Files ----- //
    link(rel='stylesheet', href='/bootstrap/dist/css/bootstrap.min.css')
    link(rel='stylesheet', href='/bootstrap-material-design/dist/css/material.min.css')

  body
    block content

    //- ----- JS Files ----- //
    script(type='text/javascript', src='/jquery/dist/jquery.min.js')
    script(type='text/javascript', src='/socket.io-client/socket.io.js')
    script(type='text/javascript', src='/bootstrap/dist/js/bootstrap.min.js')
    script(type='text/javascript', src='/bootstrap-material-design/dist/js/material.min.js')

    script(type='text/javascript', src='/scripts/BelajarWebSocket.js')  
{% endhighlight %}

Sama seperti html biasa, bedanya disini kita menggunakan template engine jade sehingga tidak perlu tag penutup. Oke selanjutnya buat file `index.jade` di dalam folder `views`, file `index.jade` nantinya akan berfungsi sebagai root page. Berikut kodingannya.

{% highlight jade %}
extends ./layout.jade

block title
  title Belajar WebSocket
block content
  div.container
    div.row
      div.col-xs-5.col-md-3
      div.col-xs-5.col-md-6
        div.panel.panel-primary
          div.panel-heading
            h3.panel-title Aplikasi Chat
          div.panel-body
            ul(id="listPesan")
          div.panel-footer
            div.form-group
              input(id="nama", type="text", placeholder="Masukkan Nama Anda").form-control.floating-label
            div.form-group
              textarea(id="pesan", placeholder="Masukkan Pesan Anda").form-control.floating-label
            button(id="kirim", type="button").btn.btn-primary.btn-fab.btn-raised.mdi-content-send
{% endhighlight %}

Teryata tidak hanya java yang bisa menggunakan extend, template engine juga bisa :D secara otomatis file `layout.jade` akan di include di dalam file `index.jade` ini salah satu fungsi template engine, jadinya kita tidak perlu banyak - banyak mendeklarasikan file js, css dan img :) . jangan lupa dari masing - masing komponent seperti `input`, `button` dan `list` dikasih `id` fungsinya nanti kita akan mengambil value dari masing - masing komponent tersebut dengan bantuan [JQuery](https://jquery.com/).

Selesai untuk layoutnya, selanjutnya buat folder `public` di dalam folder root folder project, buat juga folder `scripts` di dalam folder `public`. Buat sebuah file `BelajarWebSocket.js` di dalam folder `scripts`. Nah file `BelajarWebSocket.js` kita akan isi dengan konfigurasi [JQuery](https://jquery.com/) dan [Socket.IO](http://socket.io). Berikut codingan nya.

{% highlight js %}
'use strict';

var socket = io();
var DataChatKirim = {};

$(document).ready(function() {
  $('#kirim').on('click', function() {
    DataChatKirim.nama = $('#nama').val();
    DataChatKirim.pesan = $('#pesan').val();

    socket.emit('chat:pesan', DataChatKirim);
    $('#nama').val('');
    $('#pesan').val('');
  });
});

socket.on('chat:pesan', function(DataChat) {
  if (DataChatKirim.nama === DataChat.nama) {
    $('#listPesan').prepend($('<li class="list-group-item text-right">').text(DataChat.nama + ' : ' + DataChat.pesan));
  } else {
    $('#listPesan').prepend($('<li class="list-group-item text-left">').text(DataChat.nama + ' : ' + DataChat.pesan));
  }
});
{% endhighlight %}

Berikut penjelasan singkat dari codingan diatas.

* `var socket = io();` berfungsi untuk mendeklarasikan koneksi websocket
* `var DataChatKirim = {};` deklarasi untuk data chat yang dikirim
* `$(document).ready` merupakan deklarasi jquery untuk awal sebuah dokument
* `$('#kirim').on('click', function()` merupakan deklarasik jquery dengan mengambil sebuah id dengan nama `kirim`, id ini berasal dari sebuah button, kemudian jika ada yang mengirim pesan maka function dibawahnya akan dijalankan.
* `DataChatKirim.nama = $('#nama').val()` berfungsi untuk mengambil data nama, dapat dilihat bahwa `$('#nama').val()` berfungsi untuk mengambil data yang berasal dari id `nama`.
* `socket.emit('chat:pesan')` berfungsi untuk mengirim data chat ke server melalui websocket
* `$('#nama').val('');` merupakan perintah jquery untuk menghapus data yang terdapat di dalam form inputan
* `socket.on('chat:pesan')` sama seperti pada server, function ini berfungsi untuk menunggu response dari server, jika ada response maka perintah dibawah function ini akan dijalankan.
* `$('#listPesan').prepend()` berfungsi untuk menambah tag `<li>` pada tag `<ul>`, id `listPesan` berasal dari tag `<ul>` sehingga apabila ada chat yang masuk, maka tag `<li>` akan ditambah secara otomatis, ini merupakan salah satu konsep dari JQuery yaitu DOM (data object ).

##Uji Coba Aplikasi Chat

Silahkan jalankan aplikasi dengan perintah `nodemon app.js` atau `node app.js` kemudian hit pada browser dengan url `http://localhost:3000/` kemudian buka 2 browser lakukan chat. Berikut adalah hasilnya pada saat anda chat.

![Screenshot from 2015-10-14 13:56:19.png](../images/Screenshot from 2015-10-14 13:56:19.png)

Berikut tampilan list chat

![Screenshot from 2015-10-14 13:56:37.png](../images/Screenshot from 2015-10-14 13:56:37.png)

##Deploy Aplikasi ke Heroku

Heroku merupakan hosting gratis untuk percobaan aplikasi, heroku mendukung beberapa bahasa pemrograman diantaranya adalah javascript, disini node js sebagai javascript yang jalan pada bagian server. Silahkan daftar di [Heroku](https://www.heroku.com/
) kemudian jangan lupa setting key ssh anda.

Buatlah sebuah file `Procfile` di dalam root folder, kemudian isikan kodingan berikut.

{% highlight bash %}
web: node app.js
{% endhighlight %}

buat sebuah file `.gitignore` agar folder `node_modules` dan `bower_components` tidak ikut dicommit, masukkan codingan berikut pada file `.gitignore`.

{% highlight bash %}
bower_components/
node_modules/
{% endhighlight %}

Inisialisasi dengan menggunakan perintah `git init`, buatlah sebuat project pada dashboard heroku. Kemudian buka file `package.json` pada bagian `scripts` ganti `test` menjadi

{% highlight js %}
"postinstall": "./node_modules/bower/bin/bower install"
{% endhighlight %}

hasilnya seperti ini

{% highlight js %}
"scripts": {
  "postinstall": "./node_modules/bower/bin/bower install"
}
{% endhighlight %}

Install bower pada aplikasi dengan perintah `npm install bower --save`. Disini penulis membuat aplikasi pada heroku dengan nama `aplikasichat` kemudian jalankan perintah `heroku git:remote -a aplikasichat` pada root folder project. Tambah semua file agar dapat dicommit dengan perintah `git add .` Kemudian commit dengan perintah `git commit -am "aplikasi chat"` kemudian push semua source code ke heroku dengan perintah `git push heroku master`. Tunggu hingga selesai, jika berhasil maka akan muncul seperti ini.

![Screenshot from 2015-10-14 15:22:23.png](../images/Screenshot from 2015-10-14 15:22:23.png)

Oke aplikasi chat akhirnya berhasil online :D silahkan akses di [Aplikasi Chat](https://aplikasichat.herokuapp.com/) :) . Sekian tutorial membuat aplikasi chat dengan teknologi node js dan socket.io dan Terima kasih :). Untuk source code lengkap, penulis publish di [Belajar WebSocket](https://github.com/RizkiMufrizal/Belajar-WebSocket-Socket.IO).
