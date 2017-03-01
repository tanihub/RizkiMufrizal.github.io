---
layout: post
title: Belajar Docker
modified:
categories: 
description: belajar docker
tags: [docker, docker compose, docker image, docker container, vagrant, sails js, node js, mariadb, arsitektur docker]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-08-01T07:15:48+07:00
---

Pada artikel [Belajar Vagrant](https://rizkimufrizal.github.io/belajar-vagrant/), penulis sudah menulis sedikit mengenai tool - tool yang mempermudah kita dalam development sebuah aplikasi. Salah satu tool tersebut adalah vagrant, pada artikel sebelumnya penulis telah membahas mengenai berbagai masalah yang terjadi pada saat development sebuah aplikasi. Pada artikel ini, penulis akan mencoba membahas mengenai teknologi baru yang patut dicoba karena teknologi ini merupakan salah satu teknologi yang akan digunakan pada masa depan :D, teknologi itu adalah docker.

## Apa Itu Docker ?

>>[Docker](https://www.docker.com/) adalah sebuah project open source yang ditujukan untuk developer atau sysadmin untuk membangun, mengemas dan menjalankan aplikasi dimana pun di dalam sebuah container.

Mungkin anda sedikit bingung dengan pengertian diatas dikarenakan terlalu sulit untuk membayangkan bagaimana pengembangan aplikasi yang sebenarnya. Docker berfungsi sebagai virtualisasi sebuah sistem operasi atau sebuah server atau sebuah web server atau bahkan sebuah database server, dimana dengan menggunakan virtualisasi ini, diharapkan developer dapat mengembangkan aplikasi sesuai dengan spesifikasi server atau dengan kata lain, jika kita mengembangkan sebuah aplikasi lalu kita jalankan pada komputer kita sendiri maka secara otomatis aplikasi akan berjalan dengan baik, nah bagaimana jika server yang akan menjalankan aplikasi kita memiliki banyak perbedaan dengan komputer kita seperti perbedaan sistem operasi, arsitektur processor dan sebagainya. Dengan menggunakan virtualisasi ini maka para developer lebih mudah untuk mengatur mengenai deployment atau menjalankan aplikasi di server production.

Sebelum kita membahas mengenai docker lebih lanjut, kita akan mencoba membahas sedikit mengenai docker dan vagrant. Docker dan vagrant merupakan tool yang sama atau dapat dikatakan merupakan tool developer yang mempunyai fungsi yang sama, akan tetapi meski memiliki fungsi yang sama terdapat beberapa perbedaan sehingga kita perlu menentukan tool yang terbaik untuk melakukan development sebuah aplikasi. Beberapa perbedaan dapat dilihat melalui tabel berikut.

![comparefeat-dock-vagrant.png](../images/comparefeat-dock-vagrant.png)

gambar diatas dapat anda lihat [disini](https://www.ociweb.com/resources/publications/sett/march-2015-docker-vs-vagrant/). Dari gambar dapat dilihat bahwa terdapat banyak sekali perbedaan antara docker dan vagrant. Perbedaan yang sangat mencolok adalah docker menggunakan resource atau memory yang lebih sedikit ketimbang vagrant, ini dapat dilihat dari penggunaaan RAM, penggunaan images sistem operasi dan juga dapat dilihat perbedaannya, jika menggunakan vagrant maka kita wajib melakukan instalasi virtual machine seperti virtual box atau vmware, berbeda dengan docker menggunakan linux container sehingga kita tidak perlu melakukan instalasi virtual machine.

## Arsitektur Docker

![architecture-docker.svg](../images/architecture-docker.svg)

Gambar diatas merupakan arsitektur docker, dimana docker terdiri dari beberapa element yaitu docker client, docker daemon, docker container, docker images dan docker registry. Docker menggunakan teknologi client server untuk menghubungkan antara docker client dan docker daemon. Penulis akan menjelaskan sedikit mengenai istilah - istilah penting pada docker.

### Docker Daemon

Docker daemon berfungsi untuk membangun, mendistribusikan dan menjalankan container docker. User tidak dapat langsung menggunakan docker daemon, akan tetapi untuk menggunakan docker daemon maka user menggunakan docker client sebagai perantara atau cli.

### Docker Images

Docker images adalah sebuah template yang bersifat read only. Template ini sebenarnya adalah sebuah OS atau OS yang telah diinstall berbagai aplikasi. Docker images berfungsi untuk membuat docker container, dengan hanya 1 docker images kita dapat membuat banyak docker container.

### Docker Container

Docker container bisa dikatakan sebagai sebuah folder, dimana docker container ini dibuat dengan menggunakan docker container. Setiap docker container disimpan maka akan terbentuk layer baru tepat diatas docker images atau base image diatasnya. Contohnya misalkan kita menggunakan image ubuntu, kemudian kita membuat sebuah container dari image ubuntu tersebut dengan nama ubuntuku, kemudian kita lakukan instalasi sebuah software misalnya nginx maka secara otomatis container ubuntuku akan berada diatas layer image atau base image ubuntu. Anda dapat membuat banyak docker container dari 1 docker images. Docker container ini nantinya dapat dibuild sehingga akan menghasilkan sebuah docker images, dan docker images yang dihasilkan dari docker container ini dapat kita gunakan kembali untuk membuat docker container yang baru.

### Docker Registry

Docker registry adalah kumpulan docker image yang bersifat private maupun public yang dapat anda akses di [docker hub](https://hub.docker.com/). Dengan menggunakan docker registry, anda dapat menggunakan docker image yang telah dibuat oleh developer yang lain, sehingga mempermudahkan kita dalam pengembangan aplikasi.

## Instalasi Docker

Untuk melakukan instalasi docker, silahkan update terlebih dahulu repository ubuntu anda dengan perintah.

{% highlight bash %}
sudo apt update
{% endhighlight %}

Kemudian lakukan update CA certificates dengan perintah.

{% highlight bash %}
sudo apt-get install apt-transport-https ca-certificates
{% endhighlight %}

Kemudian tambahkan GPG key dengan perintah.

{% highlight bash %}
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
{% endhighlight %}

Silahkan buat file `docker.list` dengan perintah.

{% highlight bash %}
sudo gedit /etc/apt/sources.list.d/docker.list
{% endhighlight %}

Karena penulis menggunakan ubuntu 16.04 LTS maka silahkan tambahkan repository berikut.

{% highlight bash %}
deb https://apt.dockerproject.org/repo ubuntu-xenial main
{% endhighlight %}

Kemudian silahkan lakukan update kembali dengan perintah.

{% highlight bash %}
sudo apt update
{% endhighlight %}

Kemudian silahkan hapus docker yang lama jika anda pernah melakukan instalasi docker versi lama dengan perintah.

{% highlight bash %}
sudo apt purge lxc-docker
{% endhighlight %}

Lalu silahkan jalankan perintah berikut untuk melakukan instalasi docker.

{% highlight bash %}
sudo apt install docker-engine
{% endhighlight %}

Kemudian kita akan menjalankan service docker dengan perintah.

{% highlight bash %}
sudo systemctl start docker
{% endhighlight %}

lalu lakukan pengecekan status service docker dengan perintah.

{% highlight bash %}
sudo systemctl status docker
{% endhighlight %}

jika berhasil maka akan muncul output seperti berikut.

![Screenshot from 2016-07-29 10-25-59.png](../images/Screenshot from 2016-07-29 10-25-59.png)

Agar kita dapat menggunakan docker tanpa sudo maka kita harus melakukan beberapa konfigurasi. Silahkan buat docker group dengan perintah berikut.

{% highlight bash %}
sudo groupadd docker
{% endhighlight %}

kemudian tambahkan user ke docker group dengan perintah.

{% highlight bash %}
sudo usermod -aG docker rizki
{% endhighlight %}

silahkan ganti `rizki` dengan user linux anda. Kita akan melakukan test docker dengan perintah.

{% highlight bash %}
docker run hello-world
{% endhighlight %}

Jika berhasil maka anda akan melihat output seperti gambar berikut.

![Screenshot from 2016-07-30 09-38-04.png](../images/Screenshot from 2016-07-30 09-38-04.png)

### Instalasi Docker Compose

Setelah melakukan instalasi docker engine atau core docker, langkah selanjutnya kita akan melakukan instalasi docker compose, apa itu docker compose ?

>>[Docker compose](https://docs.docker.com/compose/) berfungsi untuk menjalankan container docker secara bersamaan.

docker compose ini sangat berguna ketika aplikasi kita terpisah - pisah pada komputer yang berbeda, contohnya adalah aplikasi yang dibuat berada pada 1 container sedangkan database yang akan digunakan oleh aplikasi tersebut berada pada container yang lain. Ketika menggunakan docker compose maka kita dapat menjalankan kedua container tersebut secara bersamaan dan bahkan kita dapat melakukan link ke container yang kita inginkan.

Untuk melakukan instalasi docker compose silahkan jalankan perintah berikut untuk melakukan akses root.

{% highlight bash %}
sudo -s
{% endhighlight %}

kemudian jalankan perintah berikut.

{% highlight bash %}
curl -L https://github.com/docker/compose/releases/download/1.8.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
{% endhighlight %}

Setelah selesai lalu beri hak akses eksekusi dengan perintah.

{% highlight bash %}
chmod +x /usr/local/bin/docker-compose
{% endhighlight %}

Lalu agar docker compose dapat diakses tanpa root, silahkan jalankan perintah berikut.

{% highlight bash %}
sudo chmod -R 777 /usr/local/bin/docker-compose
{% endhighlight %}

Kemudian lakukan pengecekan docker compose dengan perintah

{% highlight bash %}
docker-compose -version
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
docker-compose version 1.8.0, build f3628c7
{% endhighlight %}

## Latihan Sails JS dengan Docker

Untuk mempercepat latihan, kita akan mencoba menggunakan Sails JS, apa itu Sails JS ?

>>[Sails JS](http://sailsjs.org/) adalah salah satu framework mvc untuk node js.

Sails JS merupakan framework yang dikembangkan dari framework express js, dengan menggunakan scaffolding dari Sails JS maka Sails JS akan secara otomatis membuat project tanpa perlu membuat project dari awal. Sails JS telah menggunakan salah satu framework orm node js yaitu waterline sehingga mempermudah kita untuk melakukan migrasi antar database. Dan salah satu keunggulan dari Sails JS adalah mudahnya membuat API dengan menggunakan arsitektur REST.

Pada artikel ini, kita hanya akan membuat sebuah REST API dengan menggunakan Sails JS, dimana kita akan menggunakan database mariadb. Antara aplikasi dan database akan dibuatkan dua container yang berbeda, berikut adalah gambar arsitektur yang akan kita gunakan.

![Arsitektur Docker Sails.svg](../images/Arsitektur Docker Sails.svg)

Setelah mengetahui arsitektur yang akan kita gunakan, hal yang pertama kali kita lakukan sekarang adalah melakukan instalasi `Sails JS` pada pc kita terlebih dahulu, karena Sails JS merupakan framework node js maka kita membutuhkan instalasi node js, bagi anda yang belum melakukan instalasi node js, silahkan lihat artikel [instalasi perlengkapan coding node js ](https://rizkimufrizal.github.io/instalasi-perlengkapan-coding-node-js/). Untuk melakukan instalasi Sails JS silahkan jalankan perintah berikut.

{% highlight bash %}
npm -g install sails
{% endhighlight %}

Setelah selesai, kita akan membuat sebuah project dengan perintah.

{% highlight bash %}
sails new Belajar-Docker-SailsJS
{% endhighlight %}

Jika sudah, maka sails akan secara otomatis membuat sebuah project dengan nama `Belajar-Docker-SailsJS`. Kemudian silahkan buka dengan editor anda. Berikut adalah struktur folder projectnya.

![Screenshot from 2016-07-31 11-39-03.png](../images/Screenshot from 2016-07-31 11-39-03.png)

Secara default, Sails JS menggunakan database local atau embedded database, karena kita ingin menggunakan database mariadb maka kita perlu melakukan instalasi module `sails-mysql` dengan perintah.

{% highlight bash %}
npm install sails-mysql --save
{% endhighlight %}

Kemudian silahkan buka file `connections.js` yang ada di dalam folder `config` lalu kita akan melakukan konfigurasi untuk koneksi ke database mariadb pada bagian berikut.

{% highlight javascript %}
// someMysqlServer: {
//   adapter: 'sails-mysql',
//   host: 'YOUR_MYSQL_SERVER_HOSTNAME_OR_IP_ADDRESS',
//   user: 'YOUR_MYSQL_USER', //optional
//   password: 'YOUR_MYSQL_PASSWORD', //optional
//   database: 'YOUR_MYSQL_DB' //optional
// }
{% endhighlight %}

Karena kita belum mengetahui variabel apa saja yang dapat kita gunakan untuk melakukan koneksi database ke mariadb maka kita terlebih dahulu akan melakukan testing membuat container baru untuk mengetahui variabel environment dari database mariadb. Untuk membuat sebuah container silahkan jalankan perintah berikut.

{% highlight bash %}
docker run -d \
    --name latihan-container-mariadb \
    -e MYSQL_ROOT_PASSWORD=root \
    -e MYSQL_DATABASE=latihan \
    -e MYSQL_USER=rizki \
    -e MYSQL_PASSWORD=mufrizal \
    mariadb:10.1.16
{% endhighlight %}

Perintah diatas berfungsi untuk membuat sebuah container baru dengan nama `latihan-container-mariadb`, dimana container ini kita buat berdasarkan image `mariadb` dengan versi `10.1.16` dan pada saat pembuatan container ini, kita dapat melakukan set environment seperti database, password user dan lain sebagainya. Jika anda belum memiliki image mariadb maka secara otomatis, docker akan melakukan pull images mariadb yang berasal dari docker hub.

Jika telah berhasil, silahkan lakukan pengecekan docker image dengan perintah.

{% highlight bash %}
docker images
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
REPOSITORY    TAG        IMAGE ID        CREATED         SIZE
mariadb       10.1.16    9a0138c02438    38 hours ago    391.6 MB
{% endhighlight %}

kemudian lakukan pengecekan container dengan perintah.

{% highlight bash %}
docker ps
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
01444c38b227        mariadb:10.1.16     "docker-entrypoint.sh"   4 hours ago         Up 4 hours          3306/tcp            latihan-container-mariadb
{% endhighlight %}

Anda juga dapat mengecek container mana saja yang sedang jalan dengan menggunakan perintah.

{% highlight bash %}
docker ps -a
{% endhighlight %}

Langkah selanjutnya kita akan membuat sebuah container untuk aplikasi, dimana nantinya aplikasi ini akan melakukan koneksi ke mariadb yang berada pada container sebelumnya. Silahkan jalankan perintah berikut untuk membuat container aplikasi.

{% highlight bash %}
docker run --link latihan-container-mariadb:mariadb --name latihan-container-aplikasi -it node:6.3.1 bash
{% endhighlight %}

Perintah diatas berfungsi untuk membuat container dengan nama `latihan-container-aplikasi`, dimana container ini melakukan link ke container `latihan-container-mariadb`, container ini menggunakan image `node:6.3.1`. Setelah jalan, maka anda dapat melakukan akses container melalui terminal anda, lalu silahkan lakukan pengecekan environment mariadb dengan perintah.

{% highlight bash %}
printenv | grep MARIADB
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

![Screenshot from 2016-07-31 21-49-33.png](../images/Screenshot from 2016-07-31 21-49-33.png)

Setelah mengetahui variabel environment nya, kita balik lagi ke aplikasinya. Silahkan ubah codingan yang ada di dalam file `connections.js` seperti berikut.

{% highlight javascript %}
MariadbServer: {
  adapter: 'sails-mysql',
  host: process.env.MARIADB_PORT_3306_TCP_ADDR,
  user: process.env.MARIADB_ENV_MYSQL_USER,
  password: process.env.MARIADB_ENV_MYSQL_PASSWORD,
  database: process.env.MARIADB_ENV_MYSQL_DATABASE
},
{% endhighlight %}

Selanjutnya silahkan buka file `models.js` di dalam folder `config` lalu ubah codingannya menjadi seperti berikut.

{% highlight javascript %}
module.exports.models = {
  connection: 'MariadbServer',
  migrate: 'safe'
};
{% endhighlight %}

Setelah selesai melakukan semua konfigurasi tersebut, langkah selanjutnya adalah kita akan mencoba membuat API, untuk membuat API dengan menggunakan sails, silahkan jalankan perintah berikut.

{% highlight bash %}
sails generate api barang
{% endhighlight %}

maka secara otomatis sails akan membuat controller dan model, silahkan buka file `Barang.js` di dalam folder `models` kemudian ubah codingannya menjadi seperti berikut.

{% highlight javascript %}
/**
 * Barang.js
 *
 * @description :: TODO: You might write a short summary of how this model works and what it represents here.
 * @docs        :: http://sailsjs.org/documentation/concepts/models-and-orm/models
 */

module.exports = {
  attributes: {
    idBarang: {
      type: 'integer',
      required: true,
      primaryKey: true,
      autoIncrement: true,
      columnName: 'id_barang'
    },
    namaBarang: {
      type: 'string',
      required: true,
      size: 50,
      columnName: 'nama_barang'
    },
    jenisBarang: {
      type: 'string',
      enum: ['cair', 'gas', 'padat'],
      required: true,
      columnName: 'jenis_barang'
    },
    hargaBarang: {
      type: 'integer',
      required: true,
      columnName: 'harga_barang'
    },
    tanggalKadaluarsa: {
      type: 'date',
      required: true,
      columnName: 'tanggal_kadaluarsa'
    }
  }
};
{% endhighlight %}

Codingan diatas tidak akan penulis bahas dikarenkan nanti akan penulis jelaskan pada artikel berikutnya :). Langkah selanjutnya silahkan buat sebuah file `Dockerfile` di dalam root folder, kemudian isikan codingan seperti berikut.

{% highlight bash %}
FROM node:6.3.1

RUN apt-get update && apt-get install -y wget
ENV DOCKERIZE_VERSION v0.2.0
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz
RUN npm install -g sails
RUN mkdir /belajar-sailsjs-docker
WORKDIR /belajar-sailsjs-docker
ADD . /belajar-sailsjs-docker
RUN rm -rf node_modules
RUN npm install
{% endhighlight %}

Untuk dockerfile terdapat beberapa perintah yaitu.

* ADD : biasanya digunakan untuk melakukan copy file atau folder ke suatu direktory
* CMD : berfungsi untuk menjalankan sebuah perintah yang spesifik
* ENV : berfungsi untuk mendeklarasikan environment variabel
* FROM : berfungsi untuk mendeklarasikan base image yang akan digunakan.
* WORKDIR : berfungsi untuk mendefinisikan folder yang akan kita gunakan.
* MAINTAINER : berfungsi untuk mendeklarasikan nama author
* RUN : berfungsi untuk menjalankan perintah - perintah bash

Nah dari perintah - perintah diatas, penulis akan mendefinisikan arti dari codingan diatas.

* baris 1 : berfungsi untuk mendeklarasikan bahwa kita menggunakan image node dengan versi 6.3.1
* baris 2 : berfungsi untuk melakukan update dan instalasi software wget
* baris 3 : berfungsi untuk mendeklarasikan environment dockerize
* baris 4 : berfungsi untuk menjalankan perintah wget
* baris 5 : berfungsi untuk melakukan instalasi sails
* baris 6 : berfungsi untuk membuat folder/direktory untuk project
* baris 7 : berfungsi untuk mendefiniskan folder yang akan kita gunakan
* baris 8 : berfungsi untuk mengcopy seluruh file ke dalam folder `belajar-sailsjs-docker`
* baris 9 : berfungsi untuk menghapus folder `node_modules`
* baris 19 : berfungsi untuk melakukan instal dependency yang diperlukan oleh project

Karena kita tidak ingin file - file `node_modules`, `.tmp` dicopy ke docker maka kita perlu membuat file `dockerignore`. Silahkan buat sebuah file `.dockerignore` lalu isikan codingan seperti berikut.

{% highlight bash %}
node_modules
.tmp
*.md
.editorconfig
{% endhighlight %}

Setelah selesai, langkah selanjutnya kita akan membuat konfigurasi docker-compose, untuk membuat konfigurasi docker-compose silahkan buat sebuah file `docker-compose.yml` pada root project lalu isikan dengan codingan berikut.

{% highlight yaml %}
mariadb:
  image: mariadb:10.1.16
  environment:
    - MYSQL_ROOT_PASSWORD=root
    - MYSQL_DATABASE=microservicesailsjs
    - MYSQL_USER=rizki
    - MYSQL_PASSWORD=mufrizal

belajar-sailsjs-docker:
  image: rizki.mufrizal/belajar-docker-sailsjs
  ports:
    - "1337:1337"
  volumes:
    - .:/belajar-sailsjs-docker
  working_dir: /belajar-sailsjs-docker
  command: sh ./install.sh
  links:
    - mariadb
{% endhighlight %}

Bisa dilihat bahwa kita menggunakan konfigurasi docker compose versi 1, di dalam konfigurasi tersebut terdapat dua service/container yang akan dijalankan yaitu mariadb dan belajar-sailsjs-docker. Container belajar-sailsjs-docker mempunyai link terhadap container mariadb, sehingga container mariadb wajib dijalankan terlebih dahulu. Agar container mariadb dapat dijalankan terlebih dahulu maka kita dapat menggunakan [dockeriza](https://github.com/jwilder/dockerize), aplikasi sails harus dijalankan dengan perintah `sails lift` maka di dalam command kita menggunakan perintah `sails lift`. Pada codingan diatas kita menggunakan perintah `sh ./install.sh` karena kita membutuhkan beberapa command bash, maka silahkan anda buat sebuah file `install.sh` di dalam root project, lalu masukkan codingan seperti berikut.

{% highlight bash %}
dockerize -wait tcp://db:3306
sails lift
{% endhighlight %}

Setelah selesai, langkah selanjutnya kita akan melakukan build image terlebih dahulu dengan perintah.

{% highlight bash %}
docker build -t rizki.mufrizal/belajar-docker-sailsjs .
{% endhighlight %}

perintah diatas akan membuat sebuah image dengan nama `rizki.mufrizal/belajar-docker-sailsjs` dimana image ini dibuild berdasarkan konfigurasi dari file `Dockerfile`. Lalu selanjutnya kita akan menjalankan 2 container secara sekaligus dengan menggunakan docker compose, silahkan jalankan perintah berikut.

{% highlight bash %}
docker-compose up
{% endhighlight %}

Silahkan tunggu hingga aplikasi berjalan, jika berhasil maka akan muncul output seperti berikut.

![Screenshot from 2016-07-31 23-52-13.png](../images/Screenshot from 2016-07-31 23-52-13.png)

>>NOTE : jika masih terdapat error pada saat docker compose dijalankan, silahkan matikan kedua container tersebut dengan perintah `ctrl + c` kemudian jalankan kembali docker tersebut, hal ini disebabkan karena konfigurasi mariadb pada saat pertama kali dijalankan memerlukan waktu yang lebih lama dibandingkan dengan start up aplikasi sails.

### Menghapus Image Dan Container

Jika anda tidak memerlukan lagi docker image, anda dapat menghapusnya berdasarkan ID image, misalnya penulis menjalankan perintah.

{% highlight bash %}
docker images
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
REPOSITORY    TAG        IMAGE ID        CREATED         SIZE
mariadb       10.1.16    9a0138c02438    38 hours ago    391.6 MB
{% endhighlight %}

untuk menghapus image tersebut, anda dapat menggunakan perintah.

{% highlight bash %}
docker rmi 9a0138c02438
{% endhighlight %}

untuk menghapus seluruh image, anda dapat menggunakan perintah.

{% highlight bash %}
docker rmi $(docker images -q)
{% endhighlight %}

Untuk container juga sama, hanya saja container menggunakan sintak yang berbeda, jika image menggunakan perintah `rmi` maka container menggunakan perintah `rm`, akan tetapi jika container masih aktif maka anda perlu mematikan container terlebih dahulu dengan perintah stop. Comtohnya jika penulis menjalankan perintah.

{% highlight bash %}
docker ps -a
{% endhighlight %}

maka akan muncul output

{% highlight bash %}
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
01444c38b227        mariadb:10.1.16     "docker-entrypoint.sh"   4 hours ago         Up 4 hours          3306/tcp            latihan-container-mariadb
{% endhighlight %}

anda dapat mematikannya dengan perintah.

{% highlight bash %}
docker stop 01444c38b227
{% endhighlight %}

lalu menghapus container tersebut dengan perintah

{% highlight bash %}
docker rm 01444c38b227
{% endhighlight %}

untuk mematikan seluruh container, anda dapat menggunakan perintah.

{% highlight bash %}
docker stop $(docker ps -a -q)
{% endhighlight %}

untuk menghapus seluruh container, anda dapat menggunakan perintah.

{% highlight bash %}
docker rm $(docker ps -a -q)
{% endhighlight %}

### Uji Coba Aplikasi

Setelah panjang nulis artikel, akhirnya kita dapat melakukan uji coba :D. Untuk melakukan uji coba aplikasi, silahkan akses `http://localhost:1337` pada browser anda, jika berhasil maka akan muncul output seperti gambar berikut.

![Screenshot from 2016-07-31 23-57-07.png](../images/Screenshot from 2016-07-31 23-57-07.png)

Ini artinya, aplikasi yang telah kita buat telah berjalan dengan baik. Langkah selanjutnya kita akan melakukan uji coba REST API nya, silahkan buka postman, lalu konfigurasikan seperti berikut.

![Screenshot from 2016-08-01 00-04-30.png](../images/Screenshot from 2016-08-01 00-04-30.png)

Lalu tekan tombol send, maka data akan tersimpan, untuk mengetahui apakah data telah disimpan atau tidak silahkan akses `http://localhost:1337/barang` pada browser anda, jika berhasil maka akan muncul output seperti gambar berikut.

![Screenshot from 2016-08-01 00-08-24.png](../images/Screenshot from 2016-08-01 00-08-24.png)

Bagi anda yang ingin melihat contoh project docker yang lain, silahkan lihat di [Belajar Docker](https://github.com/RizkiMufrizal/Belajar-Docker). Sekian artikel mengenai belajar docker dan terima kasih :)