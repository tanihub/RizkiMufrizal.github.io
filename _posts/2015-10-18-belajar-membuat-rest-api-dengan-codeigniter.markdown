---
layout: post
title: Belajar Membuat REST API Dengan CodeIgniter
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-10-18T10:21:28+07:00
---

Jika kita bicara mengenai web pasti tidak terlepas dari bahasa pemrograman PHP, mengapa demikian ? dikarenakan PHP merupakan bahasa pemrograman yang sangat banyak digunakan untuk membuat sebuah web secara dinamis. Tidak hanya banyak digunakan, bahkan laboratorium teknik informatika di universitas gunadarma juga terdapat sebuah mata praktikum pemrograman web yang dikhususkan untuk tingkat 4 yang berfungsi menambah ilmu pengetahuan mahasiswa mengenai pemrograman berbasis web dengan menggunakan PHP. Salah satu framework yang akan digunakan adalah [CodeIgniter](https://codeigniter.com/).

Pada tutorial kali ini, penulis akan berbagi tulisan mengenai bagaimana membuat sebuah REST API dengan framework codeigniter.

>>[CodeIgniter](https://codeigniter.com/) adalah sebuah framework PHP yang berfungsi untuk mempermudah sebuah development aplikasi dengan pattern MVC (model view controller).

Beberapa dari kita pernah mendengar framework, framework sebenarnya merupakan sebuah kerangka kerja yang akan kita gunakan. Dengan adanya kerangka kerja maka akan mempermudah development sebuah aplikasi, adanya standarisasi pembuatan aplikasi sehingga memperkecil kesalahan antara 1 developer dengan developer yang lain.

Berikut merupakan tahapan yang akan kita kerjakan pada saat development REST API dengan codeigniter.

* Instalasi Perlengkapan PHP
* Membuat Database
* Membuat Dan Setting Project CodeIgniter
* Tahap Development REST API
* Uji Coba REST API

##Instalasi Perlengkapan PHP

Karena kita akan menggunakan PHP maka diwajibkan untuk install PHP. Untuk linux, silahkan jalankan perintah berikut.

{% highlight bash %}
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:ondrej/php5-5.6
sudo apt-get update
sudo apt-get install php5
{% endhighlight %}

Setelah selesai install mysql dengan perintah

{% highlight bash %}
sudo add-apt-repository ppa:ondrej/mysql-5.6
sudo apt-get update
sudo apt-get install mysql-server mysql-client
{% endhighlight %}

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
composer.phar -version
{% endhighlight %}

##Membuat Database

Oke setelah semua persiapan PHP selesai, kita mulai coding :D. Yang pertama kali kita buat adalah sebuah database beserta tabel dan columnnya. Silahkan buat sebuah database dengan nama `akademik` kemudian jalankan codingan berikut untuk membuat tabel dan columnnya.

{% highlight sql %}
create table mahasiswa(
    npm varchar(8) not null,
    nama varchar(45) not null,
    kelas varchar(5) not null,
    tanggalLahir date not null,
    primary key (npm)
) Engine=InnoDB;
{% endhighlight %}

##Membuat Dan Setting Project CodeIgniter

Setelah selesai dengan database selanjutnya kita akan mulai membuat project codeigniter dengan composer. Jalankan perintah berikut :

{% highlight bash %}
composer.phar create-project kenjis/codeigniter-composer-installer Belajar-REST-CodeIgniter
{% endhighlight %}

Kemudian update project dengan perintah `composer.phar update`. Kita membuat project codeigniter berasal dari repository [kenjis](https://github.com/kenjis/codeigniter-composer-installer) dikarenakan project codeigniter ini telah lengkap dan mudah dikonfigurasikan. Perintah diatas akan membuat sebuah project dengan nama `Belajar-REST-CodeIgniter`. Silahkan buka project tersebut dengan editor kesayangan anda.

Selanjutnya kita akan melakukan beberapa konfigurasi. Buka file `autoload.php` di dalam folder `application/config/` kemudian ubah seperti berikut.

{% highlight php %}
$autoload['libraries'] = array();
↓
$autoload['libraries'] = array('database');

#batas

$autoload['helper'] = array();
↓
$autoload['helper'] = array('url');
{% endhighlight %}

###Menghilangkan index.php Pada URI (Uniform Resource Identifier)

Jika anda tidak ingin menggunakan index.php pada URI (Uniform Resource Identifier) maka buka file `config.php` di dalam folder `application/config/` berikut perubahannya

{% highlight php %}
$config['composer_autoload'] = FALSE;
↓
$config['composer_autoload'] = realpath(APPPATH . '../vendor/autoload.php');

#batas

$config['index_page'] = 'index.php';
↓
$config['index_page'] = '';
{% endhighlight %}

##Tahap Development REST API

Silahkan buka file `database.php` di dalam folder `application/config/` kemudian isi konfigurasi seperti username, password, database dan hostnamenya. Oke karena codeigniter merupakan sebuah framework MVC (Model View Controller) maka kita akan mulai coding dari model. Buat sebuah file `Mahasiswa.php` di dalam folder `application/models/` dan berikut isi codingannya.

{% highlight php %}
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class Mahasiswa extends CI_Model {

  public function getCountMahasiswa()
  {
      return $this->db->count_all_results('mahasiswa', FALSE);
  }

  public function getMahasiswa($page, $size)
  {
      return $this->db->get('mahasiswa', $size, $page);
  }

  public function insertMahasiswa($dataMahasiswa)
  {
      $val = array(
        'npm' => $dataMahasiswa['npm'],
        'nama' => $dataMahasiswa['nama'],
        'kelas' => $dataMahasiswa['kelas'],
        'tanggalLahir' => $dataMahasiswa['tanggalLahir']
      );
      $this->db->insert('mahasiswa', $val);
  }

  public function updateMahasiswa($dataMahasiswa, $npm)
  {
    $val = array(
      'nama' => $dataMahasiswa['nama'],
      'kelas' => $dataMahasiswa['kelas'],
      'tanggalLahir' => $dataMahasiswa['tanggalLahir']
    );
    $this->db->where('npm', $npm);
    $this->db->update('mahasiswa', $val);
  }

  public function deleteMahasiswa($npm)
  {
    $val = array(
      'npm' => $npm
    );
    $this->db->delete('mahasiswa', $val);
  }

}
{% endhighlight %}

Berikut penjelasan mengenai kodingan diatas.

* function `getCountMahasiswa` berfungsi untuk mengambil banyak data mahasiswa, ini berfungsi untuk melakukan paging sebuah REST API.
* function `getMahasiswa` berfungsi untuk mengambil data dari tabel mahasiswa bisa dilihat dari codingan berikut `$this->db->get('mahasiswa')` bahwa tabel yang digunakan adalah tabel mahasiswa.
* function `insertMahasiswa` berfungsi untuk menyimpan data mahasiswa, disini terdapat sebuah parameter yaitu `$dataMahasiswa` yang berisi data mahasiswa. Data ini masih berupa array yang belum beraturan, sehingga diubah terlebih dahulu menjadi array biasa kemudian data tersebut disimpan.
* function `updateMahasiswa` juga sama seperti function insertMahasiswa, hanya saja function ini terdapat 2 parameter yaitu `$dataMahasiswa` dan `$npm` dikarenakan kita akan melakukan update berdasarakan npm seorang mahasiswa.
* dan yang terakhir adalah function `deleteMahasiswa` yang berfungsi untuk menghapus data berdasarkan npm.

Berikutnya adalah codingan dibagian controller.

{% highlight php %}
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class MahasiswaController extends CI_Controller {

  public function __construct()
  {
    parent::__construct();
    $this->load->model('Mahasiswa');
  }

  public function getMahasiswa($page, $size)
  {

    $response = array(
      'content' => $this->Mahasiswa->getMahasiswa(($page - 1) * $size, $size)->result(),
      'totalPages' => ceil($this->Mahasiswa->getCountMahasiswa() / $size));

    $this->output
      ->set_status_header(200)
      ->set_content_type('application/json', 'utf-8')
      ->set_output(json_encode($response, JSON_PRETTY_PRINT))
      ->_display();
      exit;
  }

  public function saveMahasiswa()
  {
      $data = (array)json_decode(file_get_contents('php://input'));
      $this->Mahasiswa->insertMahasiswa($data);

      $response = array(
        'Success' => true,
        'Info' => 'Data Tersimpan');

      $this->output
        ->set_status_header(201)
        ->set_content_type('application/json', 'utf-8')
        ->set_output(json_encode($response, JSON_PRETTY_PRINT))
        ->_display();
        exit;
  }

  public function updateMahasiswa($npm)
  {
    $data = (array)json_decode(file_get_contents('php://input'));
    $this->Mahasiswa->updateMahasiswa($data, $npm);

    $response = array(
      'Success' => true,
      'Info' => 'Data Berhasil di update');

    $this->output
      ->set_status_header(200)
      ->set_content_type('application/json', 'utf-8')
      ->set_output(json_encode($response, JSON_PRETTY_PRINT))
      ->_display();
      exit;
  }

  public function deleteMahasiswa($npm)
  {
    $this->Mahasiswa->deleteMahasiswa($npm);

    $response = array(
      'Success' => true,
      'Info' => 'Data Berhasil di hapus');

    $this->output
      ->set_status_header(200)
      ->set_content_type('application/json', 'utf-8')
      ->set_output(json_encode($response, JSON_PRETTY_PRINT))
      ->_display();
      exit;
  }

}
{% endhighlight %}

berikut penjelasan mengenai codingan diatas.

* function `__construct` berfungsi untuk meload model yang telah kita deklarasikan agar model tersebut dapat kita gunakan.
* function `getMahasiswa` berfungsi untuk mengambil seluruh data mahasiswa berdasarkan page dan size yang akan di request oleh client. Hasil query tersebut akan di encode menjadi format JSON, bisa dilihat pada bagian `json_encode`.
* function `saveMahasiswa` berfungsi untuk menyimpan data mahasiswa, data diambil dari response JSON, data yang berupa JSON di decode terlebih dahulu sehingga menjadi object array, lalu object array ini di kirim ke model untuk disimpan. Jika berhasil maka nanti akan muncul tampilan success tersimpan dalam bentuk format JSON.
* function `updateMahasiswa` sama seperti function saveMahasiswa, hanya saja function ini memiliki sebuah parameter `$npm` untuk memperbarui data.
* function `deleteMahasiswa` hanya menerima parameter `$npm` saja sehingga tidak ada JSON yang dikirim. Function ini berfungsi untuk menghapus data mahasiswa.

Tahap terakhir adalah melakukan konfigurasi routes, buka file `routes.php` di dalam folder `application/config`, berikut adalah codingannya.

{% highlight php %}
$route['api/mahasiswa/(:num)/(:num)']['GET'] = 'MahasiswaController/getMahasiswa/$1/$2';
$route['api/mahasiswa']['POST'] = 'MahasiswaController/saveMahasiswa';
$route['api/mahasiswa/(:any)']['PUT'] = 'MahasiswaController/updateMahasiswa/$1';
$route['api/mahasiswa/(:any)']['DELETE'] = 'MahasiswaController/deleteMahasiswa/$1';
{% endhighlight %}

Bisa dilihat bahwa kita melakukan custom terhadap URI tersebut, berikut penjelasannya.

* `api/mahasiswa/(:num)/(:num)[GET]` berfungsi untuk mengambil data mahasiswa melalui method GET pada protokol HTTP. jangan lupa bahwa URI ini mempunyai 2 parameter yaitu page dan size untuk melakukan paging data.
* `api/mahasiswa[POST]` berfungsi untuk menyimpan data mahasiswa, disini penulis menggunakan method POST, data yang dikirim berupa JSON.
* `api/mahasiswa/(:any)[PUT]` berfungsi untuk memperbarui data, karena memperbarui data, URI ini mempunyai sebuah parameter `npm`, data yang dikirim dalam bentuk JSON dan menggunakan method PUT pada protokol HTTP.
* `api/mahasiswa/(:any)[DELETE]` berfungsi untuk menghapus data mahasiswa menggunakan method DELETE pada protokol HTTP dan juga menggunakan parameter npm.

##Uji Coba REST API

Akhirnya selesai juga :D, Yuks lakukan uji coba REST API dengan [postman](https://www.getpostman.com/). Jalankan server PHP terlebih dahulu dengan cara, masuk ke folder root project dengan terminal lalu jalankan perintah `php -S localhost:8000 -t public/ bin/router.php` dan berikut adalah hasilnya.

![Screenshot from 2015-10-18 10:02:57.png](../images/Screenshot from 2015-10-18 10:02:57.png)

Kemudian kita lakukan uji coba dengan menggunakan method POST untuk menyimpan data seperti berikut.

![Screenshot from 2015-10-18 10:08:21.png](../images/Screenshot from 2015-10-18 10:08:21.png)

Berikut uji coba dengan menggunakan method GET untuk mengambil data

![Screenshot from 2015-10-18 10:09:17.png](../images/Screenshot from 2015-10-18 10:09:17.png)

Berikut uji coba dengan menggunakan method PUT untuk memperbarui data.

![Screenshot from 2015-10-18 10:10:01.png](../images/Screenshot from 2015-10-18 10:10:01.png)

Berikut uji coba dengan menggunakan method DELETE untuk menghapus data.

![Screenshot from 2015-10-18 10:10:47.png](../images/Screenshot from 2015-10-18 10:10:47.png)

Sekian tutorial membuat REST API dengan codeigniter dan terima kasih :). Untuk source code lengkap, penulis publish di [Belajar REST CodeIgniter](https://github.com/RizkiMufrizal/Belajar-REST-CodeIgniter).
