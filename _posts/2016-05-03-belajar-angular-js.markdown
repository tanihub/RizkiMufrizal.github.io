---
layout: post
title: Belajar Angular JS
modified:
categories: 
description: belajar angular js
tags: [angular js, hello word angular js, mv*, crud angular js]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-05-03T22:13:58+07:00
---

## Apa Itu Angular JS ?

>>Angular JS adalah salah satu framework javascript yang mengusungkan pola MV* dan dikembangkan oleh google.

Angular JS biasanya digunakan sebagai framework di bagian front end suatu website. Salah satu kelebihan dari angular js ini adalah data binding sehingga antara model dan controller datanya akan dilakukan sinkronisasi secara asynchronous. Kelebihan angular yang lain adalah angular dapat melakukan routing, membuat sebuah directive dan biasanya digunakan sebagai client untuk sebuah web service.

## Membuat Hello Word Dengan Angular JS

Untuk mempermudah pemahaman, kita akan langsung mencoba membuat hello word dengan menggunakan angular js :D. Untuk mendownload kebutuhan dependency angular, penulis akan menggunakan [bower](http://bower.io/), karena menggunakan bower maka anda diwajibkan untuk melakukan instalasi node js. Bagi anda yang belum melakukan instalasi node js, silahkan lihat di [Instalasi Perlengkapan Coding Node JS](https://rizkimufrizal.github.io/instalasi-perlengkapan-coding-node-js/). Silahkan buat sebuah folder dengan nama `Belajar-AngularJS`, kemudian akses foder tersebut dengan terminal. Kemudian jalankan perintah berikut.

{% highlight bash %}
bower init
{% endhighlight %}

Kemudian silahkan isi konfigurasinya seperti berikut.

![Screenshot from 2016-05-03 10-40-10.png](../images/Screenshot from 2016-05-03 10-40-10.png)

Lalu tahap selanjutnya, kita akan mendownload dependency library yang diperlukan, silahkan jalankan perintah berikut.

{% highlight bash %}
bower install bootstrap angular --save
{% endhighlight %}

perintah diatas akan mendownload dependency bootstrap untuk tampilan web nya dan angular js. Setelah selesai, silahkan buat sebuah file `index.html` lalu isikan codingan seperti berikut.

{% highlight html %}
{% raw %}
<!DOCTYPE html>
<html lang="en" ng-app>
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Belajar Angular JS</title>

        <!-- Bootstrap -->
        <link href="./bower_components/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" type="text/css"/>

        <!-- HTML5 Shim and Respond.js IE8 support of HTML5 elements and media queries -->
        <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
        <!--[if lt IE 9]> <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.2/html5shiv.js"></script> <script src="https://oss.maxcdn.com/libs/respond.js/1.4.2/respond.min.js"></script> <![endif]-->
    </head>
    <body>

        <div class="container">
            <div class="row">
                <div class="col-lg-4">
                    <div class="form-group">
                        <label>Nama</label>
                        <input type="text" class="form-control" placeholder="Masukkan Nama Anda" ng-model="nama"/>
                    </div>
                </div>
            </div>

            <h2>Hello {{nama}}</h2>
        </div>

        <script type="text/javascript" src="./bower_components/jquery/dist/jquery.min.js"></script>
        <script type="text/javascript" src="./bower_components/bootstrap/dist/js/bootstrap.min.js"></script>
        <script type="text/javascript" src="./bower_components/angular/angular.min.js"></script>
    </body>
</html>
{% endraw %}
{% endhighlight %}

Berikut adalah penjelasan mengenai codingan diatas.

* `ng-app` berfungsi untuk mendeklarasikan bahwa file html ini akan dihandle oleh angular js.
* `ng-model` berfungsi sebagai model yang dapat menampung data, nantinya data ini akan ditampilkan.
* {% raw %} {{nama}} {% endraw %} berfungsi untuk menampilkan data, di dalam angular, kita akan menggunakan tanda {% raw %} {{}} {% endraw %} untuk menampilkan data

Nah setelah selesai, selanjutnya kita akan menjalankan file `index.html`, untuk menjalankannya, kita akan menggunakan plugin [http-server](https://github.com/indexzero/http-server) dari node js :D. Bagi anda yang masing bingung dengan node js, silahkan simak di artikel [instalasi perlengkapan node js](https://rizkimufrizal.github.io/instalasi-perlengkapan-coding-node-js/). Untuk melakukan instalasi http-server, silahkan jalankan perintah berikut.

{% highlight bash %}
 npm install -g http-server
{% endhighlight %}

Setelah selesai, jalankan perintah berikut pada root project untuk menjalankan http-server.

{% highlight bash %}
http-server
{% endhighlight %}

Jika berhasil, maka di terminal akan muncul output seperti berikut.

{% highlight bash %}
Starting up http-server, serving ./
Available on:
  http://127.0.0.1:8080
Hit CTRL-C to stop the server
{% endhighlight %}

Kemudian silahkan akses `http://127.0.0.1:8080` pada browser anda.

![Screenshot from 2016-05-03 10-40-11.png](../images/Screenshot from 2016-05-03 10-40-11.png)

## Membuat CRUD Sederhana Dengan Angular JS

Setelah selesai membuat hello word dengan angular js, tahap selanjutnya kita akan membuat fungsi crud dengan menggunakan angular. Silahkan ubah file `index.html` seperti berikut.

{% highlight html %}
{% raw %}
<!DOCTYPE html>
<html lang="en" ng-app="Belajar-AngularJS">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Belajar Angular JS</title>

        <!-- Bootstrap -->
        <link href="./bower_components/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" type="text/css"/>

        <!-- HTML5 Shim and Respond.js IE8 support of HTML5 elements and media queries -->
        <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
        <!--[if lt IE 9]> <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.2/html5shiv.js"></script> <script src="https://oss.maxcdn.com/libs/respond.js/1.4.2/respond.min.js"></script> <![endif]-->
    </head>
    <body ng-controller="AppController">

        <div class="container">
            <div class="row">
                <div class="col-lg-2">
                    <div class="form-group">
                        <label>Nama</label>
                        <input type="text" class="form-control" placeholder="Masukkan Nama Anda" ng-model="nama"/>
                    </div>

                    <h2>Hello {{nama}}</h2>
                </div>
                <div class="col-lg-10">

                    <p></p>

                    <button ng-click="tambahBarang()" type="button" class="btn btn-primary" data-toggle="modal" data-target="#modal">
                        Tambah Barang
                    </button>

                    <p></p>

                    <table class="table table-striped table-bordered table-hover">
                        <thead>
                            <tr>
                                <th class="text-center">ID Barang</th>
                                <th class="text-center">Nama Barang</th>
                                <th class="text-center">Jenis Barang</th>
                                <th class="text-center">Tanggal Kadaluarsa</th>
                                <th class="text-center">Aksi</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr ng-repeat="b in dataBarang">
                                <td>{{b.idBarang}}</td>
                                <td>{{b.namaBarang}}</td>
                                <td>{{b.jenisBarang}}</td>
                                <td>{{b.tanggalKadaluarsa}}</td>
                                <td class="text-center">
                                    <button type="button" class="btn btn-success" data-toggle="modal" data-target="#modal" ng-click="editBarang(b)">
                                        <i class="glyphicon glyphicon-pencil"></i>
                                    </button>
                                    <button type="button" class="btn btn-danger" ng-click="hapusBarang(b)">
                                        <i class="glyphicon glyphicon-trash"></i>
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="modal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                        <h4 ng-hide="enable" class="modal-title">Tambah Barang</h4>
                        <h4 ng-show="enable" class="modal-title">Edit Barang</h4>
                    </div>
                    <div class="modal-body">
                        <form>
                            <div ng-show="enable" class="form-group">
                                <label>ID Barang</label>
                                <input type="text" class="form-control" ng-model="inputDataBarang.idBarang" disabled/>
                            </div>
                            <div class="form-group">
                                <label>Nama Barang</label>
                                <input type="text" class="form-control" placeholder="Masukkan Nama Barang" ng-model="inputDataBarang.namaBarang"/>
                            </div>
                            <div class="form-group">
                                <label>Jenis Barang</label>
                                <select class="form-control" ng-model="inputDataBarang.jenisBarang">
                                    <option value="" disabled>Pilih Jenis Barang</option>
                                    <option value="gas">Gas</option>
                                    <option value="padat">Padat</option>
                                    <option value="cair">Cair</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label>Tanggal Kadaluarsa</label>
                                <input type="date" class="form-control" placeholder="Masukkan Tanggal Kadaluarsa" ng-model="inputDataBarang.tanggalKadaluarsa"/>
                            </div>
                        </form>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-warning" data-dismiss="modal">Batal</button>
                        <button ng-hide="enable" type="button" class="btn btn-success" data-dismiss="modal" ng-click="simpanBarang(inputDataBarang)">Simpan</button>
                        <button ng-show="enable" type="button" class="btn btn-success" data-dismiss="modal" ng-click="updateBarang(inputDataBarang)">Update</button>
                    </div>
                </div>
            </div>
        </div>

        <script type="text/javascript" src="./bower_components/jquery/dist/jquery.min.js"></script>
        <script type="text/javascript" src="./bower_components/bootstrap/dist/js/bootstrap.min.js"></script>
        <script type="text/javascript" src="./bower_components/angular/angular.min.js"></script>
        <script type="text/javascript" src="./app.js"></script>
    </body>
</html>
{% endraw %}
{% endhighlight %}

Codingan diatas berfungsi untuk melakukan proses crud dimana untuk proses memasukkan data, kita hanya menggunakan fungsi modal dari bootstrap.

Kemudian silahkan buat sebuah file `app.js` kemudian isikan codingan seperti berikut.

{% highlight javascript %}
'use strict';

angular.module('Belajar-AngularJS', [])
  .controller('AppController', ['$scope', function($scope) {

    $scope.dataBarang = [];
    $scope.inputDataBarang = {};

    function generateUUID() {
      var d = new Date().getTime();
      if (window.performance && typeof window.performance.now === "function") {
        d += performance.now();
      }
      var uuid = 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        var r = (d + Math.random() * 16) % 16 | 0;
        d = Math.floor(d / 16);
        return (c == 'x' ? r : (r & 0x3 | 0x8)).toString(16);
      });
      return uuid;
    }

    $scope.tambahBarang = function() {
      $scope.enable = false;
      $scope.inputDataBarang.idBarang = '';
      $scope.inputDataBarang.namaBarang = '';
      $scope.inputDataBarang.jenisBarang = '';
      $scope.inputDataBarang.tanggalKadaluarsa = '';
    };

    $scope.simpanBarang = function(barang) {
      $scope.dataBarang.push({
        'idBarang': generateUUID(),
        'namaBarang': barang.namaBarang,
        'jenisBarang': barang.jenisBarang,
        'tanggalKadaluarsa': barang.tanggalKadaluarsa
      });
    };

    $scope.editBarang = function(barang) {
      $scope.enable = true;
      $scope.index = $scope.dataBarang.indexOf(barang);
      $scope.inputDataBarang.idBarang = barang.idBarang;
      $scope.inputDataBarang.namaBarang = barang.namaBarang;
      $scope.inputDataBarang.jenisBarang = barang.jenisBarang;
      $scope.inputDataBarang.tanggalKadaluarsa = barang.tanggalKadaluarsa;
    };

    $scope.updateBarang = function(barang) {
      $scope.dataBarang[$scope.index].idBarang = barang.idBarang;
      $scope.dataBarang[$scope.index].namaBarang = barang.namaBarang;
      $scope.dataBarang[$scope.index].jenisBarang = barang.jenisBarang;
      $scope.dataBarang[$scope.index].tanggalKadaluarsa = barang.tanggalKadaluarsa;
    };

    $scope.hapusBarang = function(barang) {
      var result = confirm('Anda ingin menghapus data barang ?');
      if (result) {
        $scope.index = $scope.dataBarang.indexOf(barang);
        $scope.dataBarang.splice($scope.index, 1);
      }
    }

  }]);
{% endhighlight %}

Codingan diatas berfungsi untuk proses crud pada bagian angular js. Function generateUUID berfungsi untuk melakukan generate id barang secara random. Jika telah selesai, silahkan jalankan file `index.html`, jika berhasil maka akan muncul output seperti berikut.

output untuk proses insert.

![Screenshot from 2016-05-03 10-41-06.png](../images/Screenshot from 2016-05-03 10-41-06.png)

output untuk proses select.

![Screenshot from 2016-05-03 10-41-07.png](../images/Screenshot from 2016-05-03 10-41-07.png)

output untuk proses update

![Screenshot from 2016-05-03 10-41-16.png](../images/Screenshot from 2016-05-03 10-41-16.png)

output untuk proses delete

![Screenshot from 2016-05-03 10-41-24.png](../images/Screenshot from 2016-05-03 10-41-24.png)

Sekian tutorial belajar angular js dan Terima kasih :). Untuk source code lengkap, penulis publish di [Belajar Angular JS](https://github.com/RizkiMufrizal/Belajar-AngularJS).