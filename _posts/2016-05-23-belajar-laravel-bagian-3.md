---
layout: post
title: Belajar Laravel Bagian 3
modified:
categories: 
description: belajar laravel
tags: [laravel, vim, mvc, nginx, mariadb, hhvm, controller, router, authentication, gulp, browserify, bootstrap, sass]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-05-23T10:00:45+07:00
---

Setelah menyelesaikan [artikel sebelumnya](https://rizkimufrizal.github.io/belajar-laravel-bagian-2/), penulis kembali menulis artikel lanjutan dari artikle sebelumnya :D. Artikel mengenai laravel ini sedikit panjang dikarenakan penulis sedang mempelajari dan mencoba menjelaskan lebih detail bagaimana cara kerja laravel itu sendiri. Pada artikel kali ini, penulis akan membahas mengenai controller, routing, view, valisasi sekaligus kita akan melakukan testing terhadap aplikasi yang telah kita buat.

## Membuat Controller Pada Laravel

Untuk membuat controller sama seperti pada saat membuat model, hanya saja kita menggunakan sintak yang sedikit berbeda. Sebelum membuat controller, disini kita membutuhkan library uuid untuk melakukan generate id seperti id buku dan id peminjaman, silahkan jalankan perintah berikut untuk mendownload dependency uuid.

{% highlight bash %}
composer require ramsey/uuid
{% endhighlight %}

Setelah selesai, silahkan jalankan perintah berikut untuk membuat controller untuk model buku.

{% highlight bash %}
php artisan make:controller BukuController
{% endhighlight %}

File `BukuController.php` terletak di dalam folder `app/Http/Controllers`, silahkan anda buku lalu ubah codingannya menjadi seperti berikut.

{% highlight php %}
<?php

namespace App\Http\Controllers;

use App\Buku;
use Illuminate\Http\Request;

use App\Http\Requests;
use Ramsey\Uuid\Uuid;

class BukuController extends Controller
{
    public function index()
    {
        $buku = Buku::paginate(10);
        return view('buku.index', ['bukus' => $buku]);
    }

    public function cariBuku(Request $request)
    {
        $key = $request->key;
        $value = $request->value;

        $buku = Buku::where($key, 'like', '%' . $value . '%')->paginate(10);
        return view('buku.index', ['bukus' => $buku]);
    }

    public function tambahBuku()
    {
        return view('buku.create');
    }

    public function simpanBuku(Request $request)
    {
        $this->validate($request, [
            'judul_buku' => 'required|max:50',
            'nama_pengarang' => 'required|max:50',
            'tahun_terbit' => 'required|integer',
            'penerbit' => 'required|max:50',
            'jumlah_buku' => 'required|integer',
            'nomor_rak_buku' => 'required|max:50',
        ]);

        $buku = new Buku;

        $buku->id_buku = Uuid::uuid4();
        $buku->judul_buku = $request->judul_buku;
        $buku->nama_pengarang = $request->nama_pengarang;
        $buku->tahun_terbit = $request->tahun_terbit;
        $buku->penerbit = $request->penerbit;
        $buku->jumlah_buku = $request->jumlah_buku;
        $buku->nomor_rak_buku = $request->nomor_rak_buku;

        $buku->save();

        \Session::flash('flash_message', 'data buku berhasil disimpan');

        return redirect('Buku');
    }

    public function editBuku($idBuku)
    {
        $buku = Buku::where('id_buku', $idBuku)->firstOrFail();
        return view('buku.edit')
            ->with('buku', $buku);
    }

    public function updateBuku(Request $request, $idBuku)
    {

        $buku = Buku::where('id_buku', $idBuku)->firstOrFail();

        $this->validate($request, [
            'id_buku' => 'required',
            'judul_buku' => 'required|max:50',
            'nama_pengarang' => 'required|max:50',
            'tahun_terbit' => 'required|integer',
            'penerbit' => 'required|max:50',
            'jumlah_buku' => 'required|integer',
            'nomor_rak_buku' => 'required|max:50'
        ]);

        Buku::where('id_buku', $buku->id_buku)
            ->update([
                'judul_buku' => $request->judul_buku,
                'nama_pengarang' => $request->nama_pengarang,
                'tahun_terbit' => $request->tahun_terbit,
                'penerbit' => $request->penerbit,
                'jumlah_buku' => $request->jumlah_buku,
                'nomor_rak_buku' => $request->nomor_rak_buku
            ]);

        \Session::flash('flash_message', 'data buku berhasil diubah');

        return redirect('Buku');
    }

    public function hapusBuku($idBuku)
    {
        $buku = Buku::where('id_buku', $idBuku)->firstOrFail();
        Buku::where('id_buku', $buku->id_buku)->delete();

        \Session::flash('flash_message', 'data buku berhasil dihapus');

        return redirect('Buku');
    }
}
{% endhighlight %}

Nah bisa kita lihat bahwa class controller ini akan mengatur hubungan antara view dan model. Class controller ini memiliki 7 method, dimana masing - masing method ini akan kita gunakan untuk melakukan manipulasi data. Karena kita menggunakan eloquent orm untuk basis data maka kita hanya perlu memanggil class model, dimana dengan class model ini kita dapat melakukan manipulasi data. Kemudian pada laravel, kita juga dapat melakukan validasi untuk setiap inputan dan kita dapat melakukan custom terhadap validasi yang kita inginkan, jika dlihat dari codingan diatas, anda dapat melihat validasinya yang berada pada codingan `$this->validate`. Anda juga dapat melihat codingan `\Session::flash`, codingan ini berfungsi untuk menampilkan notifikasi atau menampilkan info, misalnya jika data telah berhasil disimpan dan sebagainya.

Selanjutnya silahkan jalankan perintah berikut untuk membuat controller mahasiswa.

{% highlight bash %}
php artisan make:controller MahasiswaController
{% endhighlight %}

Kemudian ubah codingannya menjadi seperti berikut.

{% highlight php %}
<?php

namespace App\Http\Controllers;

use App\Mahasiswa;
use Illuminate\Http\Request;

use App\Http\Requests;

class MahasiswaController extends Controller
{
    public function index()
    {
        $mahasiswa = Mahasiswa::paginate(10);
        return view('mahasiswa.index', ['mahasiswas' => $mahasiswa]);
    }

    public function cariMahasiswa(Request $request)
    {
        $key = $request->key;
        $value = $request->value;

        $mahasiswa = Mahasiswa::where($key, 'like', '%' . $value . '%')->paginate(10);
        return view('mahasiswa.index', ['mahasiswas' => $mahasiswa]);
    }

    public function tambahMahasiswa()
    {
        return view('mahasiswa.create');
    }

    public function simpanMahasiswa(Request $request)
    {

        $this->validate($request, [
            'npm' => 'required|max:8',
            'nama' => 'required|max:50',
            'kelas' => 'required|max:6',
            'jenis_kelamin' => 'required',
            'alamat' => 'required'
        ]);

        $mahasiswa = new Mahasiswa;

        $mahasiswa->npm = $request->npm;
        $mahasiswa->nama = $request->nama;
        $mahasiswa->kelas = $request->kelas;
        $mahasiswa->jenis_kelamin = $request->jenis_kelamin;
        $mahasiswa->alamat = $request->alamat;

        $mahasiswa->save();

        \Session::flash('flash_message', 'data Mahasiswa berhasil disimpan');

        return redirect('Mahasiswa');
    }

    public function editMahasiswa($npm)
    {
        $mahasiswa = Mahasiswa::where('npm', $npm)->firstOrFail();
        return view('mahasiswa.edit')
            ->with('mahasiswa', $mahasiswa);
    }

    public function updateMahasiswa(Request $request, $npm)
    {

        $mahasiswa = Mahasiswa::where('npm', $npm)->firstOrFail();

        $this->validate($request, [
            'npm' => 'required|max:8',
            'nama' => 'required|max:50',
            'kelas' => 'required|max:6',
            'jenis_kelamin' => 'required',
            'alamat' => 'required'
        ]);

        Mahasiswa::where('npm', $mahasiswa->npm)
            ->update([
                'nama' => $request->nama,
                'kelas' => $request->kelas,
                'jenis_kelamin' => $request->jenis_kelamin,
                'alamat' => $request->alamat
            ]);

        \Session::flash('flash_message', 'data Mahasiswa berhasil diubah');

        return redirect('Mahasiswa');
    }


    public function hapusMahasiswa($npm)
    {
        $mahasiswa = Mahasiswa::where('npm', $npm)->firstOrFail();
        Mahasiswa::where('npm', $mahasiswa->npm)->delete();

        \Session::flash('flash_message', 'data mahasiswa berhasil dihapus');

        return redirect('Mahasiswa');
    }
}
{% endhighlight %}

Codingan yang akan kita gunakan untuk class controller mahasiswa adalah sama dengan class controller buku. Selanjutnya kita akan mencoba membuat controller untuk bagian peminjaman, silahkan jalankan perintah berikut untuk membuat class peminjaman controller.

{% highlight bash %}
php artisan make:controller PeminjamanController
{% endhighlight %}

Kemudian ubah codingannya menjadi seperti berikut.

{% highlight php %}
<?php

namespace App\Http\Controllers;

use App\Buku;
use App\Mahasiswa;
use App\Peminjaman;
use Illuminate\Http\Request;

use App\Http\Requests;
use Ramsey\Uuid\Uuid;

class PeminjamanController extends Controller
{
    public function index()
    {
        $peminjaman = \DB::table('tb_peminjaman')
            ->join('tb_buku', 'tb_peminjaman.id_buku', '=', 'tb_buku.id_buku')
            ->join('tb_mahasiswa', 'tb_peminjaman.npm', '=', 'tb_mahasiswa.npm')
            ->select(
                'tb_peminjaman.tanggal_batas_pengembalian',
                'tb_peminjaman.tanggal_peminjaman',
                'tb_peminjaman.id_peminjaman',
                'tb_mahasiswa.npm',
                'tb_mahasiswa.nama',
                'tb_mahasiswa.kelas',
                'tb_buku.judul_buku',
                'tb_buku.nama_pengarang',
                'tb_buku.tahun_terbit',
                'tb_buku.penerbit',
                'tb_buku.nomor_rak_buku'
            )
            ->paginate(10);
        return view('peminjaman.index', ['peminjamans' => $peminjaman]);
    }

    public function cariPeminjaman(Request $request)
    {
        $key = $request->key;
        $value = $request->value;

        $peminjaman = \DB::table('tb_peminjaman')
            ->join('tb_buku', 'tb_peminjaman.id_buku', '=', 'tb_buku.id_buku')
            ->join('tb_mahasiswa', 'tb_peminjaman.npm', '=', 'tb_mahasiswa.npm')
            ->select(
                'tb_peminjaman.tanggal_batas_pengembalian',
                'tb_peminjaman.tanggal_peminjaman',
                'tb_peminjaman.id_peminjaman',
                'tb_mahasiswa.npm',
                'tb_mahasiswa.nama',
                'tb_mahasiswa.kelas',
                'tb_buku.judul_buku',
                'tb_buku.nama_pengarang',
                'tb_buku.tahun_terbit',
                'tb_buku.penerbit',
                'tb_buku.nomor_rak_buku'
            )
            ->where($key, 'like', '%' . $value . '%')
            ->paginate(10);
        return view('peminjaman.index', ['peminjamans' => $peminjaman]);
    }

    public function tambahPeminjaman()
    {
        $mahasiswas = Mahasiswa::lists('nama', 'npm');
        $bukus = Buku::lists('judul_buku', 'id_buku');
        return view('peminjaman.create')
            ->with([
                'mahasiswas' => $mahasiswas,
                'bukus' => $bukus
            ]);
    }

    public function simpanPeminjaman(Request $request)
    {
        $this->validate($request, [
            'tanggal_peminjaman' => 'required|date',
            'tanggal_batas_pengembalian' => 'required|date',
            'mahasiswa' => 'required',
            'buku' => 'required'
        ]);

        $peminjaman = new Peminjaman;

        $peminjaman->id_peminjaman = Uuid::uuid4();
        $peminjaman->tanggal_peminjaman = $request->tanggal_peminjaman;
        $peminjaman->tanggal_batas_pengembalian = $request->tanggal_batas_pengembalian;
        $peminjaman->npm = $request->npm;
        $peminjaman->id_buku = $request->id_buku;

        $peminjaman->save();

        \Session::flash('flash_message', 'data peminjaman berhasil disimpan');

        return redirect('Peminjaman');
    }

    public function editPeminjaman($idPeminjaman)
    {
        $mahasiswas = Mahasiswa::lists('nama', 'npm');
        $bukus = Buku::lists('judul_buku', 'id_buku');

        $peminjaman = \DB::table('tb_peminjaman')
            ->join('tb_buku', 'tb_peminjaman.id_buku', '=', 'tb_buku.id_buku')
            ->join('tb_mahasiswa', 'tb_peminjaman.npm', '=', 'tb_mahasiswa.npm')
            ->select(
                'tb_peminjaman.tanggal_batas_pengembalian',
                'tb_peminjaman.tanggal_peminjaman',
                'tb_peminjaman.id_peminjaman',
                'tb_mahasiswa.npm',
                'tb_mahasiswa.nama',
                'tb_mahasiswa.kelas',
                'tb_buku.judul_buku',
                'tb_buku.nama_pengarang',
                'tb_buku.tahun_terbit',
                'tb_buku.penerbit',
                'tb_buku.nomor_rak_buku',
                'tb_buku.id_buku'
            )
            ->where('id_peminjaman', $idPeminjaman)->first();

        return view('peminjaman.edit')
            ->with('peminjaman', $peminjaman)
            ->with([
                'mahasiswas' => $mahasiswas,
                'bukus' => $bukus
            ]);
    }

    public function updatePeminjaman(Request $request, $idPeminjaman)
    {

        $peminjaman = Peminjaman::where('id_peminjaman', $idPeminjaman)->firstOrFail();

        $this->validate($request, [
            'tanggal_peminjaman' => 'required|date',
            'tanggal_batas_pengembalian' => 'required|date',
            'npm' => 'required',
            'id_buku' => 'required'
        ]);

        Peminjaman::where('id_peminjaman', $peminjaman->id_peminjaman)
            ->update([
                'tanggal_peminjaman' => $request->tanggal_peminjaman,
                'tanggal_batas_pengembalian' => $request->tanggal_batas_pengembalian,
                'npm' => $request->npm,
                'id_buku' => $request->id_buku
            ]);

        \Session::flash('flash_message', 'data peminjaman berhasil diubah');

        return redirect('Peminjaman');
    }

    public function hapusPeminjaman($idPeminjaman)
    {
        $peminjaman = Peminjaman::where('id_peminjaman', $idPeminjaman)->firstOrFail();
        Peminjaman::where('id_peminjaman', $peminjaman->id_peminjaman)->delete();

        \Session::flash('flash_message', 'data peminjaman berhasil dihapus');

        return redirect('Peminjaman');
    }
}
{% endhighlight %}

Nah terlihat sedikit perbedaan, dimana jika pada controller sebelumnya kita benar - benar menggunakan class model, dan mengapa sekarang kita menggunakan `\DB::table` untuk melakukan query ? ini disebabkan karena kita ingin melakukan custom query, dimana disini kita melakukan join sebanyak 3 tabel :). Kemudian pada function `tambahPeminjaman` mengapa kita mengambil data buku dan mahasiswa ? karena kita ingin menampilkan data mahasiswa dan buku dalam bentuk combo box, dimana nanti datanya ini diperlukan untuk penyimpanan data peminjaman buku yang dilakukan berdasarkan relasi one to many.

Setelah selesai membuat semua controller, disini penulis juga membuat authentikasi untuk user - user yang akan melakukan akses data. Untuk membuat proses authentikasi pada laravel sangatlah mudah, kita dapat menggunakan scalfolding dari laravel. Jalankan perintah berikut untuk membuat authentikasi pada laravel.

{% highlight bash %}
php artisan make:auth
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut pada terminal vagrant anda.

{% highlight bash %}
root@vagrant-ubuntu-trusty-64:/home/vagrant/Projects/Aplikasi-Perpustakaan# php artisan make:auth
Created View: /home/vagrant/Projects/Aplikasi-Perpustakaan/resources/views/auth/login.blade.php
Created View: /home/vagrant/Projects/Aplikasi-Perpustakaan/resources/views/auth/register.blade.php
Created View: /home/vagrant/Projects/Aplikasi-Perpustakaan/resources/views/auth/passwords/email.blade.php
Created View: /home/vagrant/Projects/Aplikasi-Perpustakaan/resources/views/auth/passwords/reset.blade.php
Created View: /home/vagrant/Projects/Aplikasi-Perpustakaan/resources/views/auth/emails/password.blade.php
Created View: /home/vagrant/Projects/Aplikasi-Perpustakaan/resources/views/layouts/app.blade.php
Created View: /home/vagrant/Projects/Aplikasi-Perpustakaan/resources/views/home.blade.php
Created View: /home/vagrant/Projects/Aplikasi-Perpustakaan/resources/views/welcome.blade.php
Installed HomeController.
Updated Routes File.
Authentication scaffolding generated successfully!
{% endhighlight %}

Kita tidak perlu untuk melakukan coding, karena secara otomatis laravel telah membuat fungsi untuk register, login dan logout sesuai dengan yang kita butuhkan. Password akan diinput oleh user akan di hash dengan menggunakan algoritma bcrypt sehingga keamanan data user lebih terjaga.

## Melakukan Konfigurasi Router Pada Laravel

Setelah selesai dengan controller, tahap selanjutnya kita akan mencoba melakukan konfigurasi router pada laravel. Router ini sebenarnya adalah route atau state dari pada sebuah url. Untuk melakukan konfigurasi router pada laravel, silahkan buka file `routes.php` di dalam folder `app/Http` kemudian ubah codingannya menjadi seperti berikut.

{% highlight php %}
<?php

/*
|--------------------------------------------------------------------------
| Application Routes
|--------------------------------------------------------------------------
|
| Here is where you can register all of the routes for an application.
| It's a breeze. Simply tell Laravel the URIs it should respond to
| and give it the controller to call when that URI is requested.
|
*/

Route::get('/', function () {
    return view('home');
});

//route buku
Route::get('Buku', ['middleware' => 'auth', 'uses' => 'BukuController@index']);
Route::post('CariBuku', ['middleware' => 'auth', 'uses' => 'BukuController@cariBuku']);
Route::get('TambahBuku', ['middleware' => 'auth', 'uses' => 'BukuController@tambahBuku']);
Route::post('SimpanBuku', ['middleware' => 'auth', 'uses' => 'BukuController@simpanBuku']);
Route::get('EditBuku/{idBuku}', ['middleware' => 'auth', 'uses' => 'BukuController@editBuku']);
Route::put('UpdateBuku/{idBuku}', ['middleware' => 'auth', 'uses' => 'BukuController@updateBuku']);
Route::delete('HapusBuku/{idBuku}', ['middleware' => 'auth', 'uses' => 'BukuController@hapusBuku']);

//route mahasiswa
Route::get('Mahasiswa', ['middleware' => 'auth', 'uses' => 'MahasiswaController@index']);
Route::post('CariMahasiswa', ['middleware' => 'auth', 'uses' => 'MahasiswaController@cariMahasiswa']);
Route::get('TambahMahasiswa', ['middleware' => 'auth', 'uses' => 'MahasiswaController@tambahMahasiswa']);
Route::post('SimpanMahasiswa', ['middleware' => 'auth', 'uses' => 'MahasiswaController@simpanMahasiswa']);
Route::get('EditMahasiswa/{npm}', ['middleware' => 'auth', 'uses' => 'MahasiswaController@editMahasiswa']);
Route::put('UpdateMahasiswa/{npm}', ['middleware' => 'auth', 'uses' => 'MahasiswaController@updateMahasiswa']);
Route::delete('HapusMahasiswa/{npm}', ['middleware' => 'auth', 'uses' => 'MahasiswaController@hapusMahasiswa']);

//route peminjaman
Route::get('Peminjaman', ['middleware' => 'auth', 'uses' => 'PeminjamanController@index']);
Route::post('CariPeminjaman', ['middleware' => 'auth', 'uses' => 'PeminjamanController@cariPeminjaman']);
Route::get('TambahPeminjaman', ['middleware' => 'auth', 'uses' => 'PeminjamanController@tambahPeminjaman']);
Route::post('SimpanPeminjaman', ['middleware' => 'auth', 'uses' => 'PeminjamanController@simpanPeminjaman']);
Route::get('EditPeminjaman/{idPeminjaman}', ['middleware' => 'auth', 'uses' => 'PeminjamanController@editPeminjaman']);
Route::put('UpdatePeminjaman/{idPeminjaman}', ['middleware' => 'auth', 'uses' => 'PeminjamanController@updatePeminjaman']);
Route::delete('HapusPeminjaman/{idPeminjaman}', ['middleware' => 'auth', 'uses' => 'PeminjamanController@hapusPeminjaman']);
Route::auth();
{% endhighlight %}

masing - masing route tersebut memiliki method yang berbeda - beda dikarenakan kebutuhan yang berbeda - beda, misalkan jika kita ingin mengambil data maka kita akan menggunakan method get, jika menyimpan data dengan menggunakan method post, mengubah data dengan menthod put dan menghapus data dengan method delete. Pada route ini, terdapat middleware dimana middleware ini akan dijalankan tepat pada saat sebuah function di dalam controller diakses. Middleware yang kita gunakan disini berfungsi jika user belum melakukan login, maka user akan diredirect ke halaman login terlebih dahulu. Untuk mendefinisikan route berdasarkan function pada controller tertentu tidaklah berbeda jika kita bandingkan dengan codeigniter, bisa dilihat untuk mendeklarasikan function pada controller tertentu kita dapat menggunakan codingan `nama controller@nama function`. Terdapat tanda `@` untuk memisahkan antara controller dan function.

## Membuat View Pada Laravel

Setelah melewati banyak konfigurasi pada model dan controller, tahap selanjutnya adalah kita akan membuat view :). Tahap ini merupakan tahap akhir dari project yang akan kita buat. Pada laravel, kita akan menggunakan template engine bawaan laravel yaitu `blade`. Dokumentasi tentang penggunaan view tidak dibahas pada code dokumentasi laravel, akan tetapi dibahas terpisah di [Laravel Collective](http://). Di dalam dokumentasi laravel collective, disana dijelaskan tentang bagaimana cara membuat form dan sebagainya dengan menggunakan blade, akan tetapi kita diharuskan melakukan instalasi library laravel collective pada project yang akan kita gunakan sekarang. Salah satu kelebihan dari laravel collective ini adalah kita dapat menggunakan teknologi CSRF sehingga kita tidak perlu repot - repot membuat konfigurasi CSRF lagi :D. Silahkan anda buka file `composer.json` yang berada pada root folder project anda. Kemudian tambahkan codingan seperti berikut pada bagian `require`.

{% highlight json %}
"laravelcollective/html": "5.2.*"
{% endhighlight %}

Jika dilihat dari seluruh codingannya akan menjadi seperti berikut.

{% highlight json %}
{
  "name": "laravel/laravel",
  "description": "The Laravel Framework.",
  "keywords": [
    "framework",
    "laravel"
  ],
  "license": "MIT",
  "type": "project",
  "require": {
    "php": ">=5.5.9",
    "laravel/framework": "5.2.*",
    "barryvdh/laravel-ide-helper": "^2.1",
    "ramsey/uuid": "^3.4",
    "laravelcollective/html": "5.2.*"
  },
  "require-dev": {
    "fzaninotto/faker": "~1.4",
    "mockery/mockery": "0.9.*",
    "phpunit/phpunit": "~4.0",
    "symfony/css-selector": "2.8.*|3.0.*",
    "symfony/dom-crawler": "2.8.*|3.0.*"
  },
  "autoload": {
    "classmap": [
      "database"
    ],
    "psr-4": {
      "App\\": "app/"
    }
  },
  "autoload-dev": {
    "classmap": [
      "tests/TestCase.php"
    ]
  },
  "scripts": {
    "post-root-package-install": [
      "php -r \"copy('.env.example', '.env');\""
    ],
    "post-create-project-cmd": [
      "php artisan key:generate"
    ],
    "post-install-cmd": [
      "Illuminate\\Foundation\\ComposerScripts::postInstall",
      "php artisan optimize"
    ],
    "post-update-cmd": [
      "Illuminate\\Foundation\\ComposerScripts::postUpdate",
      "php artisan optimize"
    ]
  },
  "config": {
    "preferred-install": "dist"
  }
}
{% endhighlight %}

Setelah selesai, silahkan jalankan perintah berikut untuk mendownload dependency yang kita butuhkan.

{% highlight bash %}
composer update
{% endhighlight %}

Kemudian silahkan buka file `app.php` yang berada di dalam folder `config` tambahkan pada bagian `providers` codingan seperti berikut.

{% highlight php %}
Collective\Html\HtmlServiceProvider::class,
{% endhighlight %}

maka hasilnya akan seperti berikut.

{% highlight php %}
<?php
    'providers' => [

        /*
         * Laravel Framework Service Providers...
         */
        Illuminate\Auth\AuthServiceProvider::class,
        Illuminate\Broadcasting\BroadcastServiceProvider::class,
        Illuminate\Bus\BusServiceProvider::class,
        Illuminate\Cache\CacheServiceProvider::class,
        Illuminate\Foundation\Providers\ConsoleSupportServiceProvider::class,
        Illuminate\Cookie\CookieServiceProvider::class,
        Illuminate\Database\DatabaseServiceProvider::class,
        Illuminate\Encryption\EncryptionServiceProvider::class,
        Illuminate\Filesystem\FilesystemServiceProvider::class,
        Illuminate\Foundation\Providers\FoundationServiceProvider::class,
        Illuminate\Hashing\HashServiceProvider::class,
        Illuminate\Mail\MailServiceProvider::class,
        Illuminate\Pagination\PaginationServiceProvider::class,
        Illuminate\Pipeline\PipelineServiceProvider::class,
        Illuminate\Queue\QueueServiceProvider::class,
        Illuminate\Redis\RedisServiceProvider::class,
        Illuminate\Auth\Passwords\PasswordResetServiceProvider::class,
        Illuminate\Session\SessionServiceProvider::class,
        Illuminate\Translation\TranslationServiceProvider::class,
        Illuminate\Validation\ValidationServiceProvider::class,
        Illuminate\View\ViewServiceProvider::class,

        /*
         * Application Service Providers...
         */
        App\Providers\AppServiceProvider::class,
        App\Providers\AuthServiceProvider::class,
        App\Providers\EventServiceProvider::class,
        App\Providers\RouteServiceProvider::class,

        Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
        Collective\Html\HtmlServiceProvider::class

    ],
{% endhighlight %}

Kemudian tambahkan codingan berikut pada bagian `aliases`.

{% highlight php %}
'Form' => Collective\Html\FormFacade::class,
'Html' => Collective\Html\HtmlFacade::class,
{% endhighlight %}

maka hasilnya akan seperti berikut.

{% highlight php %}
<?php
    'aliases' => [

        'App' => Illuminate\Support\Facades\App::class,
        'Artisan' => Illuminate\Support\Facades\Artisan::class,
        'Auth' => Illuminate\Support\Facades\Auth::class,
        'Blade' => Illuminate\Support\Facades\Blade::class,
        'Cache' => Illuminate\Support\Facades\Cache::class,
        'Config' => Illuminate\Support\Facades\Config::class,
        'Cookie' => Illuminate\Support\Facades\Cookie::class,
        'Crypt' => Illuminate\Support\Facades\Crypt::class,
        'DB' => Illuminate\Support\Facades\DB::class,
        'Eloquent' => Illuminate\Database\Eloquent\Model::class,
        'Event' => Illuminate\Support\Facades\Event::class,
        'File' => Illuminate\Support\Facades\File::class,
        'Form' => Collective\Html\FormFacade::class,
        'Gate' => Illuminate\Support\Facades\Gate::class,
        'Hash' => Illuminate\Support\Facades\Hash::class,
        'Html' => Collective\Html\HtmlFacade::class,
        'Lang' => Illuminate\Support\Facades\Lang::class,
        'Log' => Illuminate\Support\Facades\Log::class,
        'Mail' => Illuminate\Support\Facades\Mail::class,
        'Password' => Illuminate\Support\Facades\Password::class,
        'Queue' => Illuminate\Support\Facades\Queue::class,
        'Redirect' => Illuminate\Support\Facades\Redirect::class,
        'Redis' => Illuminate\Support\Facades\Redis::class,
        'Request' => Illuminate\Support\Facades\Request::class,
        'Response' => Illuminate\Support\Facades\Response::class,
        'Route' => Illuminate\Support\Facades\Route::class,
        'Schema' => Illuminate\Support\Facades\Schema::class,
        'Session' => Illuminate\Support\Facades\Session::class,
        'Storage' => Illuminate\Support\Facades\Storage::class,
        'URL' => Illuminate\Support\Facades\URL::class,
        'Validator' => Illuminate\Support\Facades\Validator::class,
        'View' => Illuminate\Support\Facades\View::class,

    ],
{% endhighlight %}

Langkah selanjutnya adalah melakukan instalasi gulp. Gulp merupakan salah satu build tool untuk automated task, bagi anda yang masih bingung bagaimana cara menggunakan gulp, silahkan lihat di artikel [belajar gulp](http://rizkimufrizal.github.io/berkenalan-dengan-gulp/). Untuk melakukan instalasi gulp, silahkan jalankan perintah berikut.

{% highlight bash %}
npm install -g gulp
{% endhighlight %}

Setelah selesai, kita membutuhkan dependency library bootsrap untuk mempercantik tampilan. Secara default, laravel telah mendaftakan library bootstrap-sass di dalam package.json, kita hanya perlu melakukan instalasi terhadap library tersebut dengan cara menjalankan perintah berikut.

{% highlight bash %}
npm install
{% endhighlight %}

Karena bootstrap memerlukan dependency library `jquery`, silahkan jalankan perintah berikut.

{% highlight bash %}
npm install jquery --save
{% endhighlight %}

Kemudian silahkan buka file `app.scss` yang berada di dalam folder `resources/assets/sass` kemudian ubah codingannya menjadi seperti berikut.

{% highlight scss %}
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap";
{% endhighlight %}

Setelah selesai, silahkan buat sebuah folder `js` di dalam folder `resources/assets` kemudian silahkan buat sebuah file `app.js` di dalam folder `resources/assets/js` dan masukkan codingan berikut untuk melakukan import jquery dan bootstrap.

{% highlight javascript %}
window.$ = window.jQuery = require('jquery');
require('bootstrap-sass');

$( document ).ready(function() {
    console.log($.fn.tooltip.Constructor.VERSION);
});
{% endhighlight %}

Karena file `app.js` belum terdaftar di dalam konfigurasi gulp, maka kita harus melakukan konfigurasi terlebih dahulu. Silahkan buka file `gulpfile.js` yang berada di root project, kemudian ubah codingannya menjadi seperti berikut.

{% highlight javascript %}
var elixir = require('laravel-elixir');

/*
 |--------------------------------------------------------------------------
 | Elixir Asset Management
 |--------------------------------------------------------------------------
 |
 | Elixir provides a clean, fluent API for defining some basic Gulp tasks
 | for your Laravel application. By default, we are compiling the Sass
 | file for our application, as well as publishing vendor resources.
 |
 */

elixir(function(mix) {
    mix.sass('app.scss')
        .browserify('app.js');
});
{% endhighlight %}

Bisa kita lihat dari codingan diatas, kita menggunakan browserify untuk meload file `app.js`, mengapa demikian ? dikarenakan kita menggunakan sintak amd (Asynchronous Module Definition) pada file `app.js`. Setelah selesai, karena scss merupakan preprocessor dari css maka kita tidak bisa secara langsung menjalanlkan file scss tersebut pada browser, maka kita harus mengubahnya terlebih dahulu menjadi css. Laravel telah menyediakan konfigurasi gulp untuk merubah preprocessor scss menjadi css, silahkan jalankan perintah berikut untuk mengubah scss menjadi css dan melakukan build terhadap file `app.js`.

{% highlight bash %}
gulp --production
{% endhighlight %}

Jika berhasil, maka output pada terminal akan menjadi seperti berikut.

{% highlight bash %}
root@vagrant-ubuntu-trusty-64:/home/vagrant/Projects/Aplikasi-Perpustakaan# gulp --production
[02:12:13] Using gulpfile /home/vagrant/Projects/Aplikasi-Perpustakaan/gulpfile.js
[02:12:13] Starting 'default'...
[02:12:13] Starting 'sass'...

Fetching Sass Source Files...
   - resources/assets/sass/app.scss

Saving To...
   - public/css/app.css

[02:12:20] Finished 'default' after 7.38 s
[02:12:24] gulp-notify: [Laravel Elixir] Sass Compiled!
[02:12:24] Finished 'sass' after 11 s
[02:12:24] Starting 'browserify'...

Fetching Browserify Source Files...
   - resources/assets/js/app.js

Saving To...
   - public/js/app.js

[02:12:37] gulp-notify: [Laravel Elixir] Browserify Compiled!
[02:12:37] Finished 'browserify' after 13 s
{% endhighlight %}

Nah dari output diatas dapat kita lihat bahwa gulp mengubah file `app.scss` menjadi file `app.css` yang terletak di dalam folder `public/css` sedangkan file `app.js` akan berada di dalam folder `public/js`.

Langkah selanjutnya kita akan mengatur view, semua view pada laravel terletak di dalam folder `resources/views`. Silahkan anda hapus file `welcome.blade.php` dikarenakan kita tidak lagi menggunakan file tersebut sebagai home page dari aplikasi kita melaikan file `home.blade.php` yang akan kita gunakan sebagai home page. Langkah selanjutnya silahkan buka file `app.blade.php` yang berada di folder `layouts` kemudian silahkan ubah menjadi seperti berikut.

{% highlight php %}
{% raw %}
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8"/>
        <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
        <meta name="viewport" content="width=device-width, initial-scale=1"/>

        <meta name="description" content="belajar laravel 5"/>
        <meta name="author" content="rizki mufrizal"/>

        <title>Belajar Laravel 5 @yield('title')</title>

        <link href="{{ asset('css/app.css') }}" rel="stylesheet" type="text/css"/>
        <style rel="stylesheet" type="text/css">
            body {
                min-height: 2000px;
                padding-top: 70px;
            }
        </style>

        <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
        <!--[if lt IE 9]>
        <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
        <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
        <![endif]-->
    </head>

    <body>

        <!-- Fixed navbar -->
        <nav class="navbar navbar-default navbar-fixed-top">
            <div class="container">
                <div class="navbar-header">
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                        <span class="sr-only">Toggle navigation</span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                    </button>
                    <a class="navbar-brand" href="{{ url('') }}">Belajar Laravel</a>
                </div>
                <div id="navbar" class="navbar-collapse collapse">
                    <ul class="nav navbar-nav">
                        <li class="active"><a href="{{ url('') }}">Home</a></li>
                        <li><a href="{{ url('Mahasiswa') }}">Data Mahasiswa</a></li>
                        <li><a href="{{ url('Buku') }}">Data Buku</a></li>
                        <li><a href="{{ url('Peminjaman') }}">Data Peminjaman</a></li>
                    </ul>

                    <ul class="nav navbar-nav navbar-right">
                        @if (Auth::guest())
                            <li><a href="{{ url('/login') }}">Login</a></li>
                            <li><a href="{{ url('/register') }}">Register</a></li>
                        @else
                            <li class="dropdown">
                                <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">
                                    {{ Auth::user()->name }} <span class="caret"></span>
                                </a>

                                <ul class="dropdown-menu" role="menu">
                                    <li><a href="{{ url('/logout') }}"><i class="fa fa-btn fa-sign-out"></i>Logout</a></li>
                                </ul>
                            </li>
                        @endif
                    </ul>

                </div>
            </div>
        </nav>

        <div class="container">

            @if(Session::has('flash_message'))
                <div class="alert alert-success">
                    {{ Session::get('flash_message') }}
                </div>
            @endif

            @yield('content')

        </div>

        <script src="{{ asset('js/app.js') }}" type="application/javascript"></script>
    </body>
</html>
{% endraw %}
{% endhighlight %}

Setelah selesai, kemudian silahkan buka file `home.blade.php` kemudian ubah codingannya menjadi seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')

@section('content')
    <div class="jumbotron text-center">
        <h1>Selamat Datang</h1>
        <p>Aplikasi Perpustakaan</p>
        <p>
            Aplikasi ini dibuat dalam rangka belajar laravel 5
        </p>
    </div>
@endsection
{% endraw %}
{% endhighlight %}

Kemudian silahkan akses `http://perpustakaan.com:8080` pada browser anda, jika berhasil maka outputnya akan muncul seperti berikut.

![Screenshot from 2016-05-21 22-29-07.png](../images/Screenshot from 2016-05-21 22-29-07.png)

Kemudian silahkan buat folder `mahasiswa`, `buku` dan `peminjaman` di dalam folder `views`, di dalam masing - masing folder tersebut silahkan buat file `create.blade.php`, `edit.blade.php` dan `index.blade.php`. Untuk codingan `index.blade.php` pada folder `buku` silahkan isikan codingan seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')
@section('title', 'Data Buku')
@section('content')

    <h1 class="text-center">Daftar Buku</h1>

    <div class="input-group">
        <a href="{{ url('TambahBuku') }}">
            <button class="btn btn-primary">Tambah Data</button>
        </a>
    </div>

    <p></p>

    {!! Form::open(array('url' => 'CariBuku', 'class'=>'form form-inline')) !!}
    {!! Form::select('key', array(
            'id_buku' => 'ID Buku',
            'judul_buku' => 'Judul Buku',
            'pengarang' => 'Pengarang',
            'tahun_terbit' => 'Tahun Terbit',
            'penerbit' => 'Penerbit',
            'jumlah_buku' => 'Jumlah Buku',
            'nomor_rak_buku' => 'Nomor Rak Buku'
        ), null, ['class' => 'form-control', 'placeholder' => 'Pilih Kata Kunci']) !!}
    {!! Form::text('value', null, ['class' => 'form-control', 'placeholder' =>'Cari Data Buku']) !!}
    {!! Form::submit('Cari', ['class'=>'btn btn-default']) !!}
    {!! Form::close() !!}

    <p></p>

    <table class="table table-bordered table-hover table-responsive table-striped">
        <thead>
        <tr>
            <th class="text-center">ID Buku</th>
            <th class="text-center">Judul Buku</th>
            <th class="text-center">Pengarang</th>
            <th class="text-center">Tahun Terbit</th>
            <th class="text-center">Penerbit</th>
            <th class="text-center">Jumlah Buku</th>
            <th class="text-center">Nomor Rak Buku</th>
            <th class="text-center">Aksi</th>
        </tr>
        </thead>
        <tbody>
        @foreach($bukus as $buku)
            <tr>
                <td>{{ $buku->id_buku }}</td>
                <td>{{ $buku->judul_buku }}</td>
                <td>{{ $buku->pengarang }}</td>
                <td>{{ $buku->tahun_terbit }}</td>
                <td>{{ $buku->penerbit }}</td>
                <td>{{ $buku->jumlah_buku }}</td>
                <td>{{ $buku->nomor_rak_buku }}</td>
                <td class="text-center">
                    <a href="{{ url("EditBuku/$buku->id_buku") }}">
                        <button class="btn btn-success">
                            Edit
                        </button>
                    </a>
                    {!! Form::open(array('url' => "HapusBuku/$buku->id_buku", 'method' => 'delete')) !!}
                    <button class="btn btn-danger">
                        Hapus
                    </button>
                    {!! Form::close() !!}
                </td>
            </tr>
        @endforeach
        </tbody>
    </table>

    {!! $bukus->render() !!}

@endsection
{% endraw %}
{% endhighlight %}

Untuk codingan `create.blade.php` pada folder `buku` silahkan isikan codingan seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')
@section('title', 'Tambah Data Buku')
@section('content')

    @if (count($errors) > 0)
        <div class="alert alert-danger">
            @foreach ($errors->all() as $error)
                <p>{{ $error }}</p>
            @endforeach
        </div>
    @endif

    {!! Form::open(array('url' => 'SimpanBuku')) !!}

    <div class="form-group">
        {!! Form::label('judul_buku', 'Judul Buku', ['class' => 'control-label']) !!}
        {!! Form::text('judul_buku', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Judul Buku']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('nama_pengarang', 'Nama Pengarang', ['class' => 'control-label']) !!}
        {!! Form::text('nama_pengarang', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Nama Pengarang']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('tahun_terbit', 'Tahun Terbit', ['class' => 'control-label']) !!}
        {!! Form::number('tahun_terbit', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Tahun Terbit']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('penerbit', 'Penerbit', ['class' => 'control-label']) !!}
        {!! Form::text('penerbit', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Nama Penerbit']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('jumlah_buku', 'Jumlah Buku', ['class' => 'control-label']) !!}
        {!! Form::number('jumlah_buku', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Jumlah Buku']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('nomor_rak_buku', 'Nomor Rak Buku', ['class' => 'control-label']) !!}
        {!! Form::text('nomor_rak_buku', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Nomor Rak Buku']) !!}
    </div>

    {!! Form::submit('Simpan', ['class' => 'btn btn-primary']) !!}

    {!! Form::close() !!}

@endsection
{% endraw %}
{% endhighlight %}

Untuk codingan `edit.blade.php` pada folder `buku` silahkan isikan codingan seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')
@section('title', 'Edit Data Buku')
@section('content')

    @if (count($errors) > 0)
        <div class="alert alert-danger">
            @foreach ($errors->all() as $error)
                <p>{{ $error }}</p>
            @endforeach
        </div>
    @endif

    {!! Form::model($buku, array('url' => "UpdateBuku/$buku->id_buku", 'method' => 'put')) !!}

    <div class="form-group">
        {!! Form::label('id_buku', 'ID Buku', ['class' => 'control-label']) !!}
        {!! Form::text('id_buku', null, ['class' => 'form-control', 'placeholder' => 'Masukkan ID Buku', 'disabled' => 'true']) !!}
        {!! Form::hidden('id_buku', null) !!}
    </div>

    <div class="form-group">
        {!! Form::label('judul_buku', 'Judul Buku', ['class' => 'control-label']) !!}
        {!! Form::text('judul_buku', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Judul Buku']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('nama_pengarang', 'Nama Pengarang', ['class' => 'control-label']) !!}
        {!! Form::text('nama_pengarang', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Nama Pengarang']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('tahun_terbit', 'Tahun Terbit', ['class' => 'control-label']) !!}
        {!! Form::number('tahun_terbit', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Tahun Terbit']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('penerbit', 'Penerbit', ['class' => 'control-label']) !!}
        {!! Form::text('penerbit', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Nama Penerbit']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('jumlah_buku', 'Jumlah Buku', ['class' => 'control-label']) !!}
        {!! Form::number('jumlah_buku', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Jumlah Buku']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('nomor_rak_buku', 'Nomor Rak Buku', ['class' => 'control-label']) !!}
        {!! Form::text('nomor_rak_buku', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Nomor Rak Buku']) !!}
    </div>

    {!! Form::submit('Update', ['class' => 'btn btn-success']) !!}

    {!! Form::close() !!}

@endsection
{% endraw %}
{% endhighlight %}

Setelah selesai dengan view buku, selanjutnya kita akan mencoba membuat view pada mahasiswa, untuk file `index.blade.php` pada folder `mahasiswa` silahkan isikan codingan seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')
@section('title', 'Data Mahasiswa')
@section('content')

    <h1 class="text-center">Daftar Mahasiswa</h1>

    <a href="{{ url('TambahMahasiswa') }}">
        <button class="btn btn-primary">Tambah Data</button>
    </a>

    <p></p>

    {!! Form::open(array('url' => 'CariMahasiswa', 'class'=>'form form-inline')) !!}
    {!! Form::select('key', array(
            'npm' => 'NPM',
            'nama' => 'Nama',
            'kelas' => 'Kelas'
        ), null, ['class' => 'form-control', 'placeholder' => 'Pilih Kata Kunci']) !!}
    {!! Form::text('value', null, ['class' => 'form-control', 'placeholder' =>'Cari Data Mahasiswa']) !!}
    {!! Form::submit('Cari', ['class'=>'btn btn-default']) !!}
    {!! Form::close() !!}

    <p></p>

    <table class="table table-bordered table-hover table-responsive table-striped">
        <thead>
        <tr>
            <th class="text-center">NPM</th>
            <th class="text-center">Nama</th>
            <th class="text-center">Kelas</th>
            <th class="text-center">Jenis Kelamin</th>
            <th class="text-center">Alamat</th>
            <th class="text-center">Aksi</th>
        </tr>
        </thead>
        <tbody>
        @foreach($mahasiswas as $mahasiswa)
            <tr>
                <td>{{ $mahasiswa->npm }}</td>
                <td>{{ $mahasiswa->nama }}</td>
                <td>{{ $mahasiswa->kelas }}</td>
                <td>{{ $mahasiswa->jenis_kelamin }}</td>
                <td>{{ $mahasiswa->alamat }}</td>
                <td class="text-center">
                    <a href="{{ url("EditMahasiswa/$mahasiswa->npm") }}">
                        <button class="btn btn-success">
                            Edit
                        </button>
                    </a>
                    {!! Form::open(array('url' => "HapusMahasiswa/$mahasiswa->npm", 'method' => 'delete')) !!}
                    <button class="btn btn-danger">
                        Hapus
                    </button>
                    {!! Form::close() !!}
                </td>
            </tr>
        @endforeach
        </tbody>
    </table>

    {!! $mahasiswas->render() !!}

@endsection
{% endraw %}
{% endhighlight %}

Untuk codingan `create.blade.php` pada folder `mahasiswa` silahkan isikan codingan seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')
@section('title', 'Tambah Data Mahasiswa')
@section('content')

    @if (count($errors) > 0)
        <div class="alert alert-danger">
            @foreach ($errors->all() as $error)
                <p>{{ $error }}</p>
            @endforeach
        </div>
    @endif

    {!! Form::open(array('url' => 'SimpanMahasiswa')) !!}

    <div class="form-group">
        {!! Form::label('npm', 'NPM', ['class' => 'control-label']) !!}
        {!! Form::text('npm', null, ['class' => 'form-control', 'placeholder' => 'Masukkan NPM']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('nama', 'Nama', ['class' => 'control-label']) !!}
        {!! Form::text('nama', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Nama']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('kelas', 'Kelas', ['class' => 'control-label']) !!}
        {!! Form::text('kelas', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Kelas']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('jenis_kelamin', 'Jenis Kelamin', ['class' => 'control-label']) !!}
        {!! Form::select('jenis_kelamin', array('pria' => 'Pria', 'wanita' => 'Wanita'), null, ['class' => 'form-control', 'placeholder' => 'Pilih Jenis Kelamin']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('alamat', 'Alamat', ['class' => 'control-label']) !!}
        {!! Form::textarea('alamat', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Alamat']) !!}
    </div>

    {!! Form::submit('Simpan', ['class' => 'btn btn-primary']) !!}

    {!! Form::close() !!}

@endsection
{% endraw %}
{% endhighlight %}

Untuk codingan `edit.blade.php` pada folder `mahasiswa` silahkan isikan codingan seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')
@section('title', 'Edit Data Mahasiswa')
@section('content')

    @if (count($errors) > 0)
        <div class="alert alert-danger">
            @foreach ($errors->all() as $error)
                <p>{{ $error }}</p>
            @endforeach
        </div>
    @endif

    {!! Form::model($mahasiswa, array('url' => "UpdateMahasiswa/$mahasiswa->npm", 'method' => 'put')) !!}

    <div class="form-group">
        {!! Form::label('npm', 'NPM', ['class' => 'control-label']) !!}
        {!! Form::text('npm', null, ['class' => 'form-control', 'placeholder' => 'Masukkan NPM', 'disabled' => 'true']) !!}
        {!! Form::hidden('npm', null) !!}
    </div>

    <div class="form-group">
        {!! Form::label('nama', 'Nama', ['class' => 'control-label']) !!}
        {!! Form::text('nama', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Nama']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('kelas', 'Kelas', ['class' => 'control-label']) !!}
        {!! Form::text('kelas', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Kelas']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('jenis_kelamin', 'Jenis Kelamin', ['class' => 'control-label']) !!}
        {!! Form::select('jenis_kelamin', array('pria' => 'Pria', 'wanita' => 'Wanita'), null, ['class' => 'form-control', 'placeholder' => 'Pilih Jenis Kelamin']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('alamat', 'Alamat', ['class' => 'control-label']) !!}
        {!! Form::textarea('alamat', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Alamat']) !!}
    </div>

    {!! Form::submit('Update', ['class' => 'btn btn-success']) !!}

    {!! Form::close() !!}

@endsection
{% endraw %}
{% endhighlight %}

Langkah yang terakhir, kita akan mengubah codingan view yang berada pada folder `peminjaman`, silahkan ubah file `index.blade.php` di dalam folder `peminjaman` seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')
@section('title', 'Data Peminjaman Buku')
@section('content')

    <h1 class="text-center">Daftar Peminjaman Buku</h1>

    <a href="{{ url('TambahPeminjaman') }}">
        <button class="btn btn-primary">Tambah Data</button>
    </a>

    <p></p>

    {!! Form::open(array('url' => 'CariPeminjaman', 'class'=>'form form-inline')) !!}
    {!! Form::select('key', array(
            'tanggal_peminjaman' => 'Tanggal Peminjaman',
            'tb_mahasiswa.npm' => 'NPM',
            'tb_buku.id_buku' => 'ID Buku'
        ), null, ['class' => 'form-control', 'placeholder' => 'Pilih Kata Kunci']) !!}
    {!! Form::text('value', null, ['class' => 'form-control', 'placeholder' =>'Cari Data Peminjaman Buku']) !!}
    {!! Form::submit('Cari', ['class'=>'btn btn-default']) !!}
    {!! Form::close() !!}

    <p></p>

    <table class="table table-bordered table-hover table-responsive table-striped">
        <thead>
        <tr>
            <th class="text-center">NPM</th>
            <th class="text-center">Nama</th>
            <th class="text-center">Kelas</th>
            <th class="text-center">Tanggal Peminjaman</th>
            <th class="text-center">Tanggal Pengembalian</th>
            <th class="text-center">Judul Buku</th>
            <th class="text-center">Pengarang</th>
            <th class="text-center">No Rak Buku</th>
            <th class="text-center">Aksi</th>
        </tr>
        </thead>
        <tbody>
        @foreach($peminjamans as $peminjaman)
            <tr>
                <td>{{ $peminjaman->npm }}</td>
                <td>{{ $peminjaman->nama }}</td>
                <td>{{ $peminjaman->kelas }}</td>
                <td>{{ $peminjaman->tanggal_peminjaman }}</td>
                <td>{{ $peminjaman->tanggal_batas_pengembalian }}</td>
                <td>{{ $peminjaman->judul_buku }}</td>
                <td>{{ $peminjaman->nama_pengarang }}</td>
                <td>{{ $peminjaman->nomor_rak_buku }}</td>
                <td class="text-center">
                    <a href="{{ url("EditPeminjaman/$peminjaman->id_peminjaman") }}">
                        <button class="btn btn-success">
                            Edit
                        </button>
                    </a>
                    {!! Form::open(array('url' => "HapusPeminjaman/$peminjaman->id_peminjaman", 'method' => 'delete')) !!}
                    <button class="btn btn-danger">
                        Hapus
                    </button>
                    {!! Form::close() !!}
                </td>
            </tr>
        @endforeach
        </tbody>
    </table>

    {!! $peminjamans->render() !!}

@endsection
{% endraw %}
{% endhighlight %}

Untuk codingan `create.blade.php` pada folder `peminjaman` silahkan isikan codingan seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')
@section('title', 'Tambah Data Peminjaman')
@section('content')

    @if (count($errors) > 0)
        <div class="alert alert-danger">
            @foreach ($errors->all() as $error)
                <p>{{ $error }}</p>
            @endforeach
        </div>
    @endif

    {!! Form::open(array('url' => 'SimpanPeminjaman')) !!}

    <div class="form-group">
        {!! Form::label('tanggal_peminjaman', 'Tanggal Peminjaman', ['class' => 'control-label']) !!}
        {!! Form::date('tanggal_peminjaman', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Tanggal Peminjaman']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('tanggal_batas_pengembalian', 'Tanggal Batas Pengembalian', ['class' => 'control-label']) !!}
        {!! Form::date('tanggal_batas_pengembalian', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Tanggal Batas Pengembalian']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('npm', 'Mahasiswa', ['class' => 'control-label']) !!}
        {!! Form::select('npm', $mahasiswas, null, ['class' => 'form-control', 'placeholder' => 'Pilih Mahasiswa']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('id_buku', 'Buku', ['class' => 'control-label']) !!}
        {!! Form::select('id_buku', $bukus, null, ['class' => 'form-control', 'placeholder' => 'Pilih Buku']) !!}
    </div>

    {!! Form::submit('Simpan', ['class' => 'btn btn-primary']) !!}

    {!! Form::close() !!}

@endsection
{% endraw %}
{% endhighlight %}

Untuk codingan `edit.blade.php` pada folder `peminjaman` silahkan isikan codingan seperti berikut.

{% highlight php %}
{% raw %}
@extends('layouts.app')
@section('title', 'Edit Data Peminjaman')
@section('content')

    @if (count($errors) > 0)
        <div class="alert alert-danger">
            @foreach ($errors->all() as $error)
                <p>{{ $error }}</p>
            @endforeach
        </div>
    @endif

    {!! Form::model($peminjaman, array('url' => "UpdatePeminjaman/$peminjaman->id_peminjaman", 'method' => 'put')) !!}

    <div class="form-group">
        {!! Form::label('id_peminjaman', 'ID Peminjaman', ['class' => 'control-label']) !!}
        {!! Form::text('id_peminjaman', null, ['class' => 'form-control', 'placeholder' => 'Masukkan ID Peminjaman', 'disabled' => 'true']) !!}
        {!! Form::hidden('id_peminjaman', null) !!}
    </div>

    <div class="form-group">
        {!! Form::label('tanggal_peminjaman', 'Tanggal Peminjaman', ['class' => 'control-label']) !!}
        {!! Form::date('tanggal_peminjaman', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Tanggal Peminjaman']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('tanggal_batas_pengembalian', 'Tanggal Batas Pengembalian', ['class' => 'control-label']) !!}
        {!! Form::date('tanggal_batas_pengembalian', null, ['class' => 'form-control', 'placeholder' => 'Masukkan Tanggal Batas Pengembalian']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('npm', 'Mahasiswa', ['class' => 'control-label']) !!}
        {!! Form::select('npm', $mahasiswas, null, ['class' => 'form-control', 'placeholder' => 'Pilih Mahasiswa']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('id_buku', 'Buku', ['class' => 'control-label']) !!}
        {!! Form::select('id_buku', $bukus, null, ['class' => 'form-control', 'placeholder' => 'Pilih Buku']) !!}
    </div>

    {!! Form::submit('Update', ['class' => 'btn btn-success']) !!}

    {!! Form::close() !!}

@endsection
{% endraw %}
{% endhighlight %}

Silahkan akses aplikasi anda di `http://perpustakaan.com:8080` kemudian lakukan registrasi, setelah selesai, maka lakukan login. Berikut adalah output untuk bagian register.

![Screenshot from 2016-05-23 09-41-17.png](../images/Screenshot from 2016-05-23 09-41-17.png)

Berikut adalah output untuk bagian login user.

![Screenshot from 2016-05-23 09-41-32.png](../images/Screenshot from 2016-05-23 09-41-32.png)

Berikut adalah output untuk halaman data buku.

![Screenshot from 2016-05-23 09-41-54.png](../images/Screenshot from 2016-05-23 09-41-54.png)

Berikut adalah output untuk halaman data mahasiswa.

![Screenshot from 2016-05-23 09-41-58.png](../images/Screenshot from 2016-05-23 09-41-58.png)

Berikut adalah output untuk halaman data peminjaman buku.

![Screenshot from 2016-05-23 09-42-03.png](../images/Screenshot from 2016-05-23 09-42-03.png)

Setelah menulis 3 artikel mengenai laravel, aplikasi perpustakaan dengan menggunakan laravel akhirnya selesai juga :D. Sekian tutorial belajar laravel bagian 3 dan Terima kasih :). Untuk source code lengkap, penulis publish di [Belajar Laravel](https://github.com/RizkiMufrizal/Belajar-Laravel).