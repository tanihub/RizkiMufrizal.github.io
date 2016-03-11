---
layout: post
title: Belajar Membuat Sequence Pada PostgreSQL
modified:
categories: 
description: belajar membuat sequence pada postgresql
tags: [postgresql, sequence pada postgresql]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-03-11T21:15:46+07:00
---

Pada artikel [sebelumnya](https://rizkimufrizal.github.io/instalasi-dan-konfigurasi-postgresql/), penulis telah membahas mengenai bagaimana cara instalasi dan konfigurasi postgresql :). Jika pada artikel sebelumnya kita hanya belajar bagaimana cara instalasi, konfigurasi serta belajar mengenai DDL, DML dan DCL pada postgresql, sedangkan pada artikel ini kita akan membahas mengenai sequence pada postgresql :).

## Apa Itu Sequence Pada PostgreSQL ?

>>Sequence jika dilihat dari bahasa adalah urutan atau berurutan, Artinya sebuah key yang digenerate secara berurutan atau disebut juga dengan sequential key.

Sequence merupakan salah satu fitur yang terdapat pada postgresql. Sequence memiliki konsep yang sama dengan auto increment yang terdapat pada mysql akan tetapi terdapat beberapa perbedaan, diantaranya adalah.

1. Sequence dapat di custom, artinya kita dapat melakukan custom terhadap maksimal angka yang digenerate, minimal angka yang digenerate, besarnya increment atau pengulangan dan lain sebagainya.
2. Sequence bersifat dinamic, dimana kita dapat menentukan angka awal yang digenerate, berbeda dengan auto increment yang selalu diawali dengan angka 1.
3. Auto increment biasanya dibuat berbarengan dengan membuat tabel atau dapat juga pada saat melakukan alter tabel sedangkan sequence dibuat secara terpisah atau bisa juga menggunakan type data serial pada postgresql untuk membuat secara otomatis sequence sehingga sequence dengan type data serial sangatlah mirip dengan auto increment.

Berikut adalah list database yang menggunakan sequence :

* [PostgreSQL](http://www.postgresql.org/)
* [Oracle Database](https://www.oracle.com/database/index.html)
* [FireBird](http://www.firebirdsql.org/)
* [H2 Database](http://www.h2database.com/html/main.html)
* [HSQLDB](http://hsqldb.org/)

## Membuat Sequence Pada PostgreSQL

Sebelum membuat sequence, kita terlebih dahulu membuat tabel. Database yang akan digunakan adalah `belajar-postgresql` sama seperti pada artikel [instalasi dan konfigurasi postgresql](https://rizkimufrizal.github.io/instalasi-dan-konfigurasi-postgresql/). Untuk membuat tabel, silahkan jalanlan perintah berikut.

{% highlight sql %}
CREATE TABLE barang(
    id_barang integer NOT NULL PRIMARY KEY,
    nama_barang character(50) NOT NULL,
    jenis_barang character(5) NOT NULL,
    harga_barang money
);
{% endhighlight %}

Codingan sql diatas berfungsi untuk membuat tabel dengan nama `barang` dimana untuk type data angka kita akan gunakan integer, untuk string kita akan gunakan character sedangkan untuk uang kita gunakan type data money.

Setelah selesai, tahap selanjutnya adalah kita membuat sequence. Untuk membuat sequence jalankan perintah berikut.

{% highlight sql %}
CREATE SEQUENCE IF NOT EXISTS nomor_sequence
    INCREMENT BY 1
    MINVALUE 1
    MAXVALUE 987654321
    START WITH 1;
{% endhighlight %}

Berikut adalah penjelasan mengenai codingan diatas.

* `CREATE SEQUENCE IF NOT EXISTS nomor_sequence` berfungsi untuk membuat sebuah sequence dengan nama `nomor_sequence`, akan tetapi sebelum sequence dibuat terdapat pengecekan dengan adanya perintah `IF NOT EXISTS`. Jika sequence telah ada maka sequence tidak dibuat begitu pula sebaliknya.
* `INCREMENT BY 1` berfungsi sebagai tingkatan iterasi, maksudnya setiap kali data diinput maka sequence akan bertambah 1, anda dapat mengganti angka 1 menjadi angka berapa pun sesuai dengan keinginan anda.
* `MINVALUE 1` berfungsi untuk menandakan bahwa minimal value yang dihasilkan oleh sequence tersebut adalah angka 1.
* `MAXVALUE 987654321` merupakan kebalikan dari `MINVALUE` dimana pada sequence kita dapat menentukan batas maksimal key atau angka yang dihasilkan oleh sequence tersebut.
* `START WITH 1` berfungsi untuk mendeklarasikan bahwa sequence yang dibuat akan dimulai dari angka 1.

Untuk mengecek apakah sequence telah dibuat atau belum, bisa jalankan perintah berikut.

{% highlight sql %}
SELECT sequence_name FROM information_schema.sequences WHERE sequence_schema='public';
{% endhighlight %}

dan outputnya akan seperti ini jika sequence berhasil dibuat.

{% highlight sequence %}
sequence_name
----------------
 nomor_sequence
(1 row)

{% endhighlight %}

## Uji Coba Sequence Pada PostgreSQL

Untuk melakukan ujia coba sequence pada postgresql sangatlah gampang, kita hanya perlu melakukan insert sebuah data ke database. Silahkan jalankan perintah berikut.

{% highlight sql %}
INSERT INTO barang(id_barang, nama_barang, jenis_barang, harga_barang)
VALUES(nextval('nomor_sequence'), 'rinso', 'cair', 1000);
{% endhighlight %}

Untuk memanggil sequence, cukup dengan menggunakan sintak `nextval('nama sequence')`. Dimana pada insert data diatas, kita menggunakan sequence dengan nama `nomor_sequence` yang telah dibuat. Langkah selanjutnya lakukan pengecekan data dengan perintah.

{% highlight sql %}
SELECT * FROM barang;
{% endhighlight %}

maka outputnya akan seperti ini.

{% highlight bash %}
 id_barang |                nama_barang                | jenis_barang | harga_barang 
-----------+-------------------------------------------+--------------+--------------
         1 | rinso                                     | cair         |   Rp1.000,00
(1 row)
{% endhighlight %}

## Mengubah Sequence Pada PostgreSQL

Untuk mengubah sequence pada postgresql, kita dapat menggunakan perintah `alter`. Misalkan kita ingin merubah increment menjadi 2, maka anda dapat mengubah sequence tersebut dengan perintah.

{% highlight sql %}
ALTER SEQUENCE IF EXISTS nomor_sequence
    INCREMENT BY 2;
{% endhighlight %}

Setelah selesai, lakukan insert data kembali untuk mengetahu bahwa sequence telah berubah dengan perintah.

{% highlight sql %}
INSERT INTO barang(id_barang, nama_barang, jenis_barang, harga_barang)
VALUES(nextval('nomor_sequence'), 'aqua', 'cair', 5000);
{% endhighlight %}

Kemudian jalankan perintah `select` kembali untuk menampilkan barang, maka akan muncul output seperti berikut.

{% highlight bash %}
 id_barang |                 nama_barang              | jenis_barang | harga_barang 
-----------+------------------------------------------+--------------+--------------
         1 | rinso                                    | cair         |   Rp1.000,00
         3 | aqua                                     | cair         |   Rp5.000,00
(2 rows)
{% endhighlight %}

Bisa dilihat bahwa sequence nya berhasil di ubah, dimana id nya akan terus bertambah sebanyak 2 per inputan data.

## Menghapus Sequence Pada PostgreSQL

Untuk menghapus sequence, kita dapat menggunakan perintah `drop`. Untuk menghapus sequence `nomor_sequence` silahkan jalankan perintah berikut.

{% highlight sql %}
DROP SEQUENCE IF EXISTS nomor_sequence;
{% endhighlight %}

Kemudian lakukan pengecekan sequence kembali, jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
sequence_name 
---------------
(0 rows)
{% endhighlight %}

Sekian artikel mengenai belajar membuat sequence pada postgresql dan terima kasih :).