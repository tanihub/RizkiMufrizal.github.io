---
layout: post
title: Instalasi Dan Konfigurasi PostgreSQL
modified:
categories:
description: instalasi dan konfigurasi PostgreSQL pada linux
tags: [PostgreSQL, Instalasi PostgreSQL, pgadmin, instalasi pgadmin, query pada PostgreSQL]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-02-13T08:41:23+07:00
---

>>[PostgreSQL](http://www.postgresql.org/) adalah salah satu dbms yang menawarkan skalabilitas dan kinerja tinggi berbasis open source.

Pada artikel ini, penulis akan mencoba menjelaskan bagaimana instalasi dan konfigurasi postgreSQL pada linux.

## Instalasi PostgreSQL

Silahkan buat sebuah file `pgdg.list` di dalam folder `/etc/apt/sources.list.d/`. Kemudian isikan file tersebut dengan codingan berikut.

{% highlight bash %}
deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main
{% endhighlight %}

kemudian lakukan import key dengan menjalankan perintah berikut.

{% highlight bash %}
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
  sudo apt-key add -
{% endhighlight %}

kemudian lakukan update repository dengan perintah

{% highlight bash %}
sudo apt-get update
{% endhighlight %}

dan langkah selanjutnya adalah lakukan instalasi postgreSQL dengan perintah

{% highlight bash %}
sudo apt-get install postgresql-9.5
{% endhighlight %}

## Konfigurasi PostgreSQL

Setelah melakukan instalasi postgresql, langkah selanjutnya adalah melakukan konfigurasi postgreSQL. Konfigurasi yang akan kita lakukan adalah konfigurasi user dan instalasi pgadmin pada postgreSQL.

### Konfigurasi User Pada PostgreSQL

Silahkan jalankan perintah berikut untuk melakukan konfigurasi user pada postgreSQL.

{% highlight bash %}
sudo -u postgres psql postgres
{% endhighlight %}

Kemudian ketikan codingan `\password postgres`, kemudian tekan enter lalu masukkan password untuk default user yang kita gunakan, secara default user yang digunakan adalah `postgres`. Untuk keluar dari cli psql silahkan jalankan perintah `\q`.

### Membuat User Dan Database

Untuk membuat user baru kita dapat melakukannya dengan perintah berikut melalui terminal tanpa perlu login ke psql.

{% highlight bash %}
createuser -h localhost -U postgres -D -A -P rizki
{% endhighlight %}

perintah diatas terdapat fungsi diantaranya adalah

* createuser : untuk membuat user baru
* -h : artinya kita membuat user berdasarkan host di locahost
* -U : kita ingin membuat user berdasarkan user default dari postgreSQL, secara default `postgres` adalah user default pada postgreSQL
* -D : artinya user yang kita buat tidak dapat membuat database
* -A : artinya user yang kita buat tidak dapat membuat user baru
* -P : artinya user yang kita buat akan menggunakan password

maka akan muncul perintah seperti berikut

{% highlight bash %}
rizki@rizki-HP-431-Notebook-PC:~$ createuser -h localhost -U postgres -D -A -P rizki
Enter password for new role: 
Enter it again: 
Password:
{% endhighlight %}

berikut adalah penjelasan singkat dari masing - masing baris diatas.

* baris pertama dan kedua adalah password yang akan kita masukkan untuk user yang kita buat, misalnya disini penulis membuat user `rizki`
* baris ketiga adalah password dari default user atau super user yang kita gunakan, secara default kita masih menggunakan user postgres

Langkah selanjutnya adalah membuat database, untuk membuat database baru silahkan jalankan perintah berikut.

{% highlight bash %}
createdb -h localhost -U postgres -E utf-8 -O rizki belajar-postgresql
{% endhighlight %}

codingan diatas berfungsi untuk membuat database baru, berikut adalah penjelasan dari sintak diatas.

* createdb : untuk membuat database baru
* -h : host yang kita gunakan
* -U : user default untuk membuat database dan memiliki akses untuk membuat database baru
* -E : artinya database yang akan kita buat memiliki format encoding utf-8
* -O : artinya adalah database yang dibuat ini diberikan izin akses kepada user `rizki`
* belajar-postgresql : adalah nama dari database yang kita buat.

kemudian masukkan password berdasarkan user `postgres`. Untuk login ke database melalui terminal, jalankan perintah berikut.

{% highlight bash %}
psql -h localhost -U rizki belajar-postgresql
{% endhighlight %}

masukkan password berdasarkan user yang anda buat, maka akan muncul tampilan seperti berikut.

{% highlight bash %}
rizki@rizki-HP-431-Notebook-PC:~$ psql -h localhost -U rizki belajar-postgresql 
Password for user rizki: 
psql (9.5.1)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.

belajar-postgresql=>
{% endhighlight %}

### Melakukan Instalasi PGAdmin

>> [PGAdmin](http://www.pgadmin.org/) adalah salah satu editor atau GUI untuk melakukan akses terhadap database postgreSQL.

Sebenarnya masih banyak GUI yang lain untuk melakukan akses terhadap database postgreSQL, akan tetapi penulis hanya menggunakan pgAdmin, mengapa demikian ? dikarenakan kemudahan dalam konfigurasi database postgreSQL pada pgAdmin. Untuk melakukan instalasi pgAdmin silahkan jalankan perintah berikut.

{% highlight bash %}
sudo apt-get install pgadmin3
{% endhighlight %}

kemudian silahkan buka pgadmin, maka akan muncul tampilan seperti berikut.

![Screenshot from 2016-02-12 22:52:50.png](../images/Screenshot from 2016-02-12 22:52:50.png)

kemudian silahkan pilih file lalu pilih add server, kemudian silahkan lakukan konfigurasi seperti gambar dibawah ini

![Screenshot from 2016-02-12 22:54:52.png](../images/Screenshot from 2016-02-12 22:54:52.png)

jika berhasil maka anda dapat melakukan akses database seperti berikut.

![Screenshot from 2016-02-12 22:56:18.png](../images/Screenshot from 2016-02-12 22:56:18.png)

## Latihan Query SQL Pada PostgreSQL

Query yang akan kita pelajari disini hanya terdapat 4 yaitu dari `select`, `insert`, `update` dan `delete`. Untuk masalah query tidak memiliki perbedaan dengan dbms lain seperti mysql, mariadb, oracle dan lain sebagainya.

### Query Select

Query select berfungsi untuk mengambil data, sebelum menjalankan query select, kita akan membuat tabel terlebih dahulu, silahkan login terlebih dahulu melalui terminal anda ke psql. Setelah selesai kemudian jalankan perintah berikut.

{% highlight sql %}
CREATE TABLE mahasiswa(
    npm VARCHAR(8),
    nama VARCHAR(50) not null,
    kelas VARCHAR(5) not null,
    PRIMARY KEY(npm)
);
{% endhighlight %}

jalankan perintah diatas per baris, jika berhasil maka akan muncul seperti ini.

{% highlight bash %}
belajar-postgresql=> CREATE TABLE mahasiswa(
belajar-postgresql(> npm VARCHAR(8),
belajar-postgresql(> nama VARCHAR(50) not null,
belajar-postgresql(> kelas VARCHAR(5) not null,
belajar-postgresql(> PRIMARY KEY(npm)
belajar-postgresql(> );
CREATE TABLE
{% endhighlight %}

untuk mengambil semua data, kita dapat melakukannya dengan perintah.

{% highlight sql %}
SELECT * FROM mahasiswa;
{% endhighlight %}

maka akan muncul seperti ini.

{% highlight bash %}
belajar-postgresql=> SELECT * FROM mahasiswa;
 npm | nama | kelas 
-----+------+-------
(0 rows)
{% endhighlight %}

dapat dilihat bahwa data masih kosong dikarenakan kita belum melakukan inputan terhadap data mahasiswa.

### Query Insert

Query insert pada postgreSQL juga sama seperti pada dbms umunya, silahkan jalankan perintah berikut berikut untuk menyimpan sebuah data mahasiswa.

{% highlight sql %}
INSERT INTO mahasiswa (npm, nama, kelas) VALUES ('58412085', 'Rizki Mufrizal', '4IA04');
{% endhighlight %}

jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
belajar-postgresql=> INSERT INTO mahasiswa (npm, nama, kelas) VALUES ('58412085', 'Rizki Mufrizal', '4IA04');
INSERT 0 1
{% endhighlight %}

kemudian lakukan pengecekan apakah data telah disimpan atau tidak dengan perintah select, jika data tersimpan maka akan muncul output seperti berikut.

{% highlight bash %}
belajar-postgresql=> SELECT * FROM mahasiswa;
   npm    |      nama      | kelas 
----------+----------------+-------
 58412085 | Rizki Mufrizal | 4IA04
(1 row)
{% endhighlight %}

### Query Update

Query update biasanya digunakan untuk memperbarui sebuah data, untuk melakukan query update silahkan jalan perintah berikut.

{% highlight sql %}
UPDATE mahasiswa SET nama = 'Rizki M', kelas = '4IA04' WHERE npm = '58412085';
{% endhighlight %}

jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
belajar-postgresql=> UPDATE mahasiswa SET nama = 'Rizki M', kelas = '4IA04' WHERE npm = '58412085';
UPDATE 1
{% endhighlight %}

kemudian lakukan pengecekan lagi dengan query select dan outputnya seperti berikut.

{% highlight bash %}
belajar-postgresql=> SELECT * FROM mahasiswa;
   npm    |  nama   | kelas 
----------+---------+-------
 58412085 | Rizki M | 4IA04
(1 row)
{% endhighlight %}

### Query Delete

Query yang terakhir adalah delete. Query ini berfungsi untuk menghapus data pada database. Jalankan perintah berikut untuk menghapus data.

{% highlight sql %}
DELETE FROM mahasiswa WHERE npm = '58412085';
{% endhighlight %}

jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
belajar-postgresql=> DELETE FROM mahasiswa WHERE npm = '58412085';
DELETE 1
{% endhighlight %}

kemudian lakukan pengecekan kembali dengan perintah select dan berikut adalah hasilnya.

{% highlight bash %}
belajar-postgresql=> SELECT * FROM mahasiswa;
 npm | nama | kelas 
-----+------+-------
(0 rows)
{% endhighlight %}

Sekian artikel mengenai instalasi dan konfigurasi postgreSQL dan terima kasih :).