---
layout: post
title: Membuat Backup Dan Restore Pada MongoDB
modified:
categories:
description: membuat backup dan restore pada mongodb
tags: [backup, restore, mongodb, docker, docker compose, robomongo]
image:
  background: abstract-3.png
comments: true
share: true
date: 2017-02-8T20:15:28+07:00
---

Bicara mengenai database, seorang database administrator harus mengetahui seluk beluk dari database, salah satunya adalah proses backup dan restore database. Database tanpa backup adalah suatu hal yang sangat fatal dikarenakan jika suatu waktu terjadi sesuatu pada database production maka hilang sudah semua harapan :D. Pada artikel ini, penulis akan mencoba membuat sebuah backup data lalu kita akan restore.

## Perlengkapan Project

Untuk membuat virtual database maka kita akan menggunakan docker, bagi anda yang belum mengerti tentang docker, silakan baca artikel [Belajar Docker](https://rizkimufrizal.github.io/belajar-docker/). Silahkan jalan docker anda, jika anda pengguna linux maka secara default service docker akan berjalan, jika anda pengguna osx maka anda harus menjalankan docker terlebih dahulu. Berikut adalah arsitektur yang akan kita gunakan untuk membuat backup dan restore database mongodb.

![arsitektur-backup-restore.svg](../images/arsitektur-backup-restore.svg)

Dari gambar diatas, kita dapat melihat bahwa nantinya akan ada 2 container, dimana container tersebut berisi database production dan database backup. Biasanya antara database production dan backup berada pada server yang berbeda.

## Membuat Backup Database

Sebelum kita membuat backup database, kita akan melakukan setup docker terlebih dahulu, silahkan buka terminal anda, lalu jalankan perintah berikut untuk mendownload image mongodb.

{% highlight bash %}
docker pull mongo
{% endhighlight %}

Setelah selesai, silahkan anda membuat `docker-compose.yml` karena kita akan menggunakan docker compose. Isikan source code seperti berikut.

{% highlight yml %}
mongodb-production:
  container_name: mongodb-production
  image: mongo:latest
  ports:
    - 27000:27017

mongodb-backup:
  container_name: mongodb-backup
  image: mongo:latest
  links:
    - mongodb-production
  ports:
    - 27001:27017
{% endhighlight %}

Dari konfigurasi diatas dapat kita lihat bahwa container mongodb-backup membutuhkan dependency dari mongodb-production. Untuk menjalankan kedua docker diatas, silahkan gunakan perintah berikut.

{% highlight bash %}
docker-compose up
{% endhighlight %}

Jika berhasil maka di terminal anda akan muncul seperti berikut.

![Screen Shot 2017-02-08 at 2.43.17 PM.png](../images/Screen Shot 2017-02-08 at 2.43.17 PM.png)

Langkah selanjutnya kita akan membuat file shell script, bagi anda yang belum paham shell script, silahkan baca pada artikel [Belajar Shell Script](https://rizkimufrizal.github.io/belajar-shell-script/). Silahkan buat sebuah file dengan nama `mongodb-backup.sh`, kemudian isikan codingan seperti berikut.

{% highlight bash %}
#!/bin/bash
# author Rizki Mufrizal

HOST="127.0.0.1"
PORT="27017"
REMOTE_DB="2016"
USER=""
PASS=""
PATH_BACKUP="/var/database-backup"

mongodump --db=$REMOTE_DB --username=$USER --password=$PASS --host=$HOST --port=$PORT --out=$PATH_BACKUP
{% endhighlight %}

Untuk parameter environment seperti nama database, username, password dan lain - lain kita langsung membuat per variabel sehingga nantinya hanya perlu diubah pada bagian variabel saja. Untuk melakukan backup, kita akan menggunakan `mongodump`, dimana mongodump ini mempunya beberapa parameter diantaranya adalah database yang diremote, username, password, host, port dan folder backup. Karena file shell script ini akan kita gunakan di dalam container docker maka kita harus melakukan setting ulang dockernya. Silahkan buat sebuah file dengan nama `Dockerfile` kemudian isikan codingan seperti berikut.

{% highlight bash %}
FROM mongo:latest

RUN mkdir /backup-restore
WORKDIR /backup-restore
ADD . /backup-restore
{% endhighlight %}

Setelah selesai, sekarang jalankan perintah berikut untuk menghapus container yang lama.

{% highlight bash %}
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)
{% endhighlight %}

Setelah selesai, kemudian kita akan membuat image baru dengan perintah berikut.

{% highlight bash %}
docker build -t rizki.mufrizal/mongo-backup .
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
Sending build context to Docker daemon 10.24 kB
Step 1/4 : FROM mongo:latest
 ---> 59dbd3885751
Step 2/4 : RUN mkdir /backup-restore
 ---> Running in f7710c593a1b
 ---> 7eeacbf6f587
Removing intermediate container f7710c593a1b
Step 3/4 : WORKDIR /backup-restore
 ---> 89413f734f6d
Removing intermediate container 019a58f8d2ba
Step 4/4 : ADD . /backup-restore
 ---> c593b1efba36
Removing intermediate container 3129347a292d
Successfully built c593b1efba36
{% endhighlight %}

Karena ada perubahan konfigurasi, maka kita harus melakukan perubahan pada file `docker-compose.yml` seperti berikut.

{% highlight yml %}
mongodb-production:
  container_name: mongodb-production
  image: mongo:latest
  ports:
    - 27000:27017

mongodb-backup:
  container_name: mongodb-backup
  image: rizki.mufrizal/mongo-backup
  links:
    - mongodb-production
  ports:
    - 27001:27017
{% endhighlight %}

Lalu jalankan kembali docker compose dengan perintah.

{% highlight bash %}
docker-compose up
{% endhighlight %}

### Test Backup Data

Setelah selesai melakukan konfigurasi, langkah selanjutnya kita akan mencoba mengakses database mongodb yang versi production dengan perintah berikut.

{% highlight bash %}
docker exec -i -t mongodb-production /bin/bash
{% endhighlight %}

Kemudian silahkan akses cli mongodb dengan perintah.

{% highlight bash %}
mongo
{% endhighlight %}

Kemudian jalankan perintah berikut untuk mengakses / membuat database.

{% highlight bash %}
use belajar-backup;
{% endhighlight %}

Kemudian untuk membuat sebuah collection, silahkan jalankan perintah berikut.

{% highlight bash %}
db.createCollection('tb_barang');
{% endhighlight %}

Untuk melakukan insert, silahkan jalankan perintah berikut.

{% highlight bash %}
db.tb_barang.insert(
  [
    { nama_barang: 'rinso', jumlah_barang: 10, harga_barang: 500 },
    { nama_barang: 'aqua', jumlah_barang: 50, harga_barang: 100 },
    { nama_barang: 'floridina', jumlah_barang: 10, harga_barang: 5000 }
  ]
);
{% endhighlight %}

Untuk melihat data di mongodb, anda dapat menggunakan perintah berikut.

{% highlight bash %}
db.tb_barang.find().pretty();
{% endhighlight %}

berikut adalah hasil outputnya.

{% highlight json %}
{
	"_id" : ObjectId("589afba9f2586584e62c70da"),
	"nama_barang" : "rinso",
	"jumlah_barang" : 10,
	"harga_barang" : 500
}
{
	"_id" : ObjectId("589afba9f2586584e62c70db"),
	"nama_barang" : "aqua",
	"jumlah_barang" : 50,
	"harga_barang" : 100
}
{
	"_id" : ObjectId("589afba9f2586584e62c70dc"),
	"nama_barang" : "floridina",
	"jumlah_barang" : 10,
	"harga_barang" : 5000
}
{% endhighlight %}

Setelah selesai, sekarang saatnya untuk membuat backup database, silahkan akses cli mongodb yang versi backup dengan perintah berikut.

{% highlight bash %}
docker exec -i -t mongodb-backup /bin/bash
{% endhighlight %}

Secara otomatis kita akan berada di folder `backup-restore`, di dalam folder tersebut terdapat file `mongodb-backup.sh` yang telah kita buat, akan tetapi host yang digunakan pastinya akan berbeda dikarenakan berbeda container, untuk mengecek host database mongodb versi production silahkan jalankan perintah berikut.

{% highlight bash %}
printenv | grep MONGO
{% endhighlight %}

Berikut adalah hasil outputnya.

{% highlight bash %}
MONGODB_PRODUCTION_PORT_27017_TCP_PROTO=tcp
MONGO_VERSION=3.4.2
MONGODB_PRODUCTION_ENV_GPG_KEYS=0C49F3730359A14518585931BC711F9BA15703C6
MONGODB_PRODUCTION_ENV_GOSU_VERSION=1.7
MONGODB_PRODUCTION_PORT_27017_TCP_ADDR=172.17.0.2
MONGODB_PRODUCTION_ENV_no_proxy=*.local, 169.254/16
MONGO_PACKAGE=mongodb-org
MONGODB_PRODUCTION_NAME=/mongodb-backup/mongodb-production
MONGODB_PRODUCTION_PORT_27017_TCP_PORT=27017
MONGODB_PRODUCTION_ENV_MONGO_PACKAGE=mongodb-org
MONGODB_PRODUCTION_ENV_MONGO_MAJOR=3.4
MONGODB_PRODUCTION_PORT=tcp://172.17.0.2:27017
MONGO_MAJOR=3.4
MONGODB_PRODUCTION_ENV_MONGO_VERSION=3.4.2
MONGODB_PRODUCTION_PORT_27017_TCP=tcp://172.17.0.2:27017
{% endhighlight %}

Kemudian silahkan buka kembali file `mongodb-backup.sh` lalu ubah konfigurasinya seperti berikut.

{% highlight bash %}
#!/bin/bash
# author Rizki Mufrizal

HOST=${MONGODB_PRODUCTION_PORT_27017_TCP_ADDR}
PORT=${MONGODB_PRODUCTION_PORT_27017_TCP_PORT}
REMOTE_DB="belajar-backup"
USER=""
PASS=""
PATH_BACKUP="/var/database-backup"

mongodump --db=$REMOTE_DB --username=$USER --password=$PASS --host=$HOST --port=$PORT --out=$PATH_BACKUP
{% endhighlight %}

Setelah selesai, silahkan berikan hak akses untuk execute dengan perintah.

{% highlight bash %}
chmod a+x mongodb-backup.sh
{% endhighlight %}

Kemudian jalankan file tersebut dengan perintah.

{% highlight bash %}
./mongodb-backup.sh
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
2017-02-08T13:00:52.092+0000	writing belajar-backup.tb_barang to
2017-02-08T13:00:52.095+0000	done dumping belajar-backup.tb_barang (3 documents)
{% endhighlight %}

Untuk memastikan bahwa database anda telah di backup, silahkan cek path folder backup anda, tadinya kita membuat path back up database di `/var/database-backup`, berikut adalah hasilnya jika berhasil.

![Screen Shot 2017-02-08 at 8.04.14 PM.png](../images/Screen Shot 2017-02-08 at 8.04.14 PM.png)

## Melakukan Restore Database

Pada tahap sebelumnya kita telah berhasil membuat backup database, nah sekarang kita akan melakukan restore database, sebelumnya, untuk mempermudah kita akan menggunakan [Robomongo](https://robomongo.org/) untuk mengakses database production. Silahkan download dan lakukan instalasi, jika telah selesai, silahkan buka robomongo anda, lalu lakukan konfigurasi seperti berikut.

![Screen Shot 2017-02-08 at 8.14.42 PM.png](../images/Screen Shot 2017-02-08 at 8.14.42 PM.png)

mengapa kita menggunakan port 27000 ? ya dikarenakan konfigurasi forward port yang ada pada docker. Sehingga setiap kita mengakses port 27000 maka docker akan melakukan forward ke port 27017. Jika anda melakukan akses database dan collection nya maka akan muncul output seperti berikut.

![Screen Shot 2017-02-08 at 8.15.21 PM.png](../images/Screen Shot 2017-02-08 at 8.15.21 PM.png)

Sekarang silahkan klik kanan di databasenya kemudian pilih menu drop database, maka secara otomatis database dihapus, nah sekarang kita akan melakukan restore. Silahkan akses terminal untuk database mongodb versi backup. Kemudian jalankan perintah berikut untuk melakukan restore database.

{% highlight bash %}
mongorestore /var/database-backup/belajar-backup/ --host=${MONGODB_PRODUCTION_PORT_27017_TCP_ADDR} --port=${MONGODB_PRODUCTION_PORT_27017_TCP_PORT} --db="belajar-backup" --username="" --password=""
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
2017-02-08T13:25:30.559+0000	the --db and --collection args should only be used when restoring from a BSON file. Other uses are deprecated and will not exist in the future; use --nsInclude instead
2017-02-08T13:25:30.560+0000	building a list of collections to restore from /var/database-backup/belajar-backup dir
2017-02-08T13:25:30.561+0000	reading metadata for belajar-backup.tb_barang from /var/database-backup/belajar-backup/tb_barang.metadata.json
2017-02-08T13:25:32.837+0000	restoring belajar-backup.tb_barang from /var/database-backup/belajar-backup/tb_barang.bson
2017-02-08T13:25:32.841+0000	no indexes to restore
2017-02-08T13:25:32.841+0000	finished restoring belajar-backup.tb_barang (3 documents)
2017-02-08T13:25:32.841+0000	done
{% endhighlight %}

Output diatas menandakan bahwa database telah berhasil direstore :D. Sekian artikel mengenai Membuat Backup dan Restore Pada MongoDB, semoga bermanfaat dan terima kasih :)
