---
layout: post
title: Belajar Laravel Bagian 2
modified:
categories: 
description: belajar laravel
tags: [laravel, vim, mvc, nginx, mariadb, hhvm, model, migration, seed]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-05-19T09:02:57+07:00
---

Pada [artikel sebelumnya](https://rizkimufrizal.github.io/belajar-laravel-bagian-1/), penulis telah menjelaskan bagaimana cara melakukan setup project laravel, mulai dari konfigurasi vagrant hingga mengenerate project laravel. Pada artikel kali ini, penulis akan membahas mengenai database migration, seed dan model pada laravel. 

## Konfigurasi Vim Pada Vagrant

Sebelum memulai ngoding, alangkah baiknya kita melakukan konfigurasi terlebih dahulu terhadap text editor yang akan kita gunakan. Vim merupakan salah satu text editor yang sangat banyak digunakan, terdapat banyak plugin yang dapat kita gunakan untuk memudahkan dalam proses development. 

Tahap pertama, kita akan melakukan konfigurasi plugin [pathogen](https://github.com/tpope/vim-pathogen) pada vim, silahkan jalankan perintah berikut.

{% highlight bash %}
mkdir -p ~/.vim/autoload ~/.vim/bundle && \
curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
{% endhighlight %}

Setelah selesai, langkah selanjutnya silahkan lakukan clone plugin berikut per baris.

{% highlight bash %}
git clone git://github.com/tpope/vim-sensible.git ~/.vim/bundle/vim-sensible
git clone git://github.com/altercation/vim-colors-solarized.git ~/.vim/bundle/vim-colors-solarized
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
git clone https://github.com/rstacruz/sparkup.git ~/.vim/bundle/sparkup
git clone https://github.com/ctrlpvim/ctrlp.vim.git ~/.vim/bundle/ctrlp.vim
{% endhighlight %}

Silahkan atur thema terminal anda menjadi solarized dark. Sekarang silahkan buka file `.vimrc` dengan vim, jalankan perintah berikut.

{% highlight bash %}
vim ~/.vimrc
{% endhighlight %}

Kemudian masukkan script untuk konfigurasi seperti berikut.

{% highlight vim %}
execute pathogen#infect()
syntax on

set nocompatible
filetype plugin indent on

syntax enable
let g:solarized_termcolors=256
autocmd BufNewFile,BufReadPost *.md set filetype=markdown

set ruler
set number

set expandtab
set shiftwidth=4
set softtabstop=4
set autoindent

set colorcolumn=+1

set autoread

set splitright
set splitbelow

let g:SuperTabClosePreviewOnPopupClose = 1

set rtp+=~/.vim/bundle/vundle
set runtimepath^=~/.vim/bundle/ctrlp.vim
call vundle#rc()
Bundle 'gmarik/vundle'
Bundle 'altercation/vim-colors-solarized'
Bundle 'ctrlpvim/ctrlp.vim'
Bundle 'rstacruz/sparkup'
{% endhighlight %}

Jika berhasil, maka output yang diharapkan adalah seperti berikut, vim akan memunculkan warna solarized dark dan juga menampilkan penomoran.

![Screenshot from 2016-05-14 22-15-15.png](../images/Screenshot from 2016-05-14 22-15-15.png)

## Belajar Perintah Vim

Sebelum kita lanjut, kita harus benar - benar bisa terlebih dahulu menggunakan vim :D. Tidak seperti text editor biasa yang langsung kita gunakan, vim mempunya beberapa perintah diantaranya adalah.

* `vim nama file` biasanya digunakan untuk membuat atau mengubah isi dari file tersebut.
* `i` atau insert berfungsi agar kita dalam melakukan perubahan pada file yang sedang aktif dan akan muncul tulisan insert dibagian bawah.
* `esc` untuk keluar dari mode edit document atau keluar dari perintah `i` sehingga document tidak dapat diubah.
* `:w` atau write berfungsi untuk menyimpan sebuah document yang telah kita buat atau yang telah kita ubah, jika anda telah selesai melakukan perubahan silahkan tekan `esc` lalu ketikan perintah `:w` maka document akan tersimpan.
* `:q` atau quit artinya anda keluar dari text editor.
* `:wq` atau write and quit artinya anda akan keluar dari text editor dan menyimpan perubahan file.
* `:q!` quit artinya anda keluar dari text editor tanpa menyimpan perubahan pada document.
* `:sp nama file` split horisontal artinya anda dapat membagi screen window editor anda berdasarkan horisontal.
* `:vsp nama file` split vertikal artinya anda dapat membagi screen window editor anda berdasarkan vertikal.
* `ctrl + w + w` berfungsi untuk berpindah dari 1 window ke window yang lain.

## Membuat Database Migration Pada Laravel

Setelah selesai belajar vim, langkah selanjutnya kita akan mencoba membuat database migration pada laravel. Database migration merupakan salah satu fitur dari laravel untuk mempermudah developer dalam mengembangkan schema database yang ingin digunakan. Di dalam bahasa pemrograman lain juga tersedia database migration seperti ruby on rails pada ruby, flyway pada java dan lain - lain.

Karena project yang akan kita buat adalah sebuah perpustakaan maka disini kita akan membuat database migration mengenai perpustakaan. Berikut adalah ERD dari pada aplikasi perpustakaan yang akan kita buat.

![Screenshot from 2016-05-18 21-49-10.png](../images/Screenshot from 2016-05-18 21-49-10.png)

Bagi yang masih bingung cara membaca ERD tersebut silahkan baca artikel [belajar membuat foreign key pada h2 database](http://rizkimufrizal.github.io/belajar-membuat-foreign-key-pada-h2-database/). Nah setelah mengetahui tentang ERD nya, selanjutnya kita akan membuat database migration dengan menggunakan perintah artisan pada laravel. Untuk membuat database migration untuk tabel buku maka jalankan perintah berikut.

{% highlight bash %}
php artisan make:migration create_buku_table --create=tb_buku
{% endhighlight %}

Jika berhasil maka outputnya akan seperti berikut.

![Screenshot from 2016-05-18 18-12-54.png](../images/Screenshot from 2016-05-18 18-12-54.png)

Langkah selanjutnya untuk membuat database migration untuk tabel mahasiswa, silahkan jalankan perintah berikut.

{% highlight bash %}
php artisan make:migration create_mahasiswa_table --create=tb_mahasiswa
{% endhighlight %}

Untuk tabel peminjaman, silahkan jalankan perintah berikut.

{% highlight bash %}
php artisan make:migration create_peminjaman_table --create=tb_peminjaman
{% endhighlight %}

Setelah selesai, tahap selanjutnya kita akan mengubah schema dari tabel - tabel yang telah kita buat. Silahkan buka file database migration untuk tabel buku yang berada di dalam folder `database/migrations`, biasanya filenya mengikuti format tanggal dan waktu pada saat membuat database migration, contohnya seperti berikut.

{% highlight text %}
2016_05_18_111241_create_buku_table.php
{% endhighlight %}

dimana 2016 adalah tahun, 05 adalah bulan, 18 adalah tanggal dan 111241 adalah sebagai waktu. Silahkan buka file database migration untuk tabel buku dengan perintah.

{% highlight bash %}
vim database/migrations/xxxx_xx_xx_xxxxxx_create_buku_table.php
{% endhighlight %}

Silahkan ganti dan sesuaikan huruf x dengan tanggal dan waktu pada saat file dibuat. Setelah dibuka, lalu silahkan ubah codingannya seperti berikut.

{% highlight php %}
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateBukuTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('tb_buku', function (Blueprint $table) {
            $table->uuid('id_buku');
            $table->string('judul_buku', 50);
            $table->string('nama_pengarang', 50);
            $table->integer('tahun_terbit');
            $table->string('penerbit', 50);
            $table->integer('jumlah_buku');
            $table->string('nomor_rak_buku', 50);
            $table->timestamps();

            $table->primary('id_buku');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('tb_buku');
    }
}
{% endhighlight %}

Dari codingan diatas dapat dilihat bahwa kita akan membuat sebuah tabel buku dengan nama tabel `tb_buku`. Disini penulis menggunakan uuid sebagai primary key sehingga tidak akan terjadi kerangkapan data. Langkah selanjutnya silahkan buka database migration untuk tabel mahasiswa dengan perintah.

{% highlight bash %}
vim database/migrations/xxxx_xx_xx_xxxxxx_create_mahasiswa_table.php
{% endhighlight %}

Kemudian ubah codingannya seperti berikut.

{% highlight php %}
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateMahasiswaTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('tb_mahasiswa', function (Blueprint $table) {
            $table->string('npm', 8);
            $table->string('nama', 50);
            $table->string('kelas', 6);
            $table->enum('jenis_kelamin', ['pria', 'wanita']);
            $table->text('alamat');
            $table->timestamps();

            $table->primary('npm');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('tb_mahasiswa');
    }
}
{% endhighlight %}

Struktur codingan diatas tidak ada yang berbeda dengan codingan sebelumnya, hanya saja disini terlihat berbeda pada bagian jenis kelamin, dimana pada bagian jenis kelamin kita akan menggunakan enum, mengapa enum ? dikarenakan jenis kelamin merupakan sebuah pilihan dan penulis ingin menjaga agar datanya tetap konsisten, jadi jika kita telah menetapkan pria dan wanita adalah sebagai jenis kelamin, maka user tidak dapat melakukan inputan selain pria dan wanita meskipun user ingin melakukan inputan data laki - laki dan perempuan, ini disebabkan data laki - laki dan perempuan tidak terdapat di dalam list enum. Selanjutnya silahkan buka database migration untuk tabel peminjaman, silahkan jalankan perintah berikut.

{% highlight bash %}
vim database/migrations/xxxx_xx_xx_xxxxxx_create_peminjaman_table.php
{% endhighlight %}

Kemudian ubah codingannya seperti berikut.

{% highlight php %}
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreatePeminjamanTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('tb_peminjaman', function (Blueprint $table) {
            $table->uuid('id_peminjaman');
            $table->date('tanggal_peminjaman');
            $table->date('tanggal_batas_pengembalian');
            $table->timestamps();

            $table->primary('id_peminjaman');

            //relasi foreign key
            $table->string('npm', 8);
            $table->char('id_buku', 36);

            $table->foreign('npm')
                ->references('npm')
                ->on('tb_mahasiswa')
                ->onDelete('cascade')
                ->onUpdate('cascade');

            $table->foreign('id_buku')
                ->references('id_buku')
                ->on('tb_buku');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('tb_peminjaman');
    }
}
{% endhighlight %}

Codingan untuk tabel peminjaman lumayan berbeda dengan tabel - tabel sebelumnya, mengapa demikian ? dikarenakan disini kita menggunakan foreign key untuk menghubungkan antara tabel buku, barang dan peminjaman. Relasi antara tabel mahasiswa dan peminjaman adalah one to many cascade sehingga apabila data mahasiswa dihapus maka secara otomatis data pada tabel peminjaman juga akan dihapus secara otomatis karena sifat cascade. Berbeda dengan tabel buku, tabel buku memiliki relasi one to many terhadap tabel peminjaman, akan tetapi tidak cascade sehingga apabila tabel buku dihapus, maka tabel peminjaman tidak ikut dihapus.

Setelah semuanya selesai, kita akan menjalankan migration tersebut. Akan tetapi sebelum menjalankan migration, kita harus terlebih dahulu melakukan konfigurasi pada environment. Silahkan buka file `.env` dengan perintah berikut.

{% highlight bash %}
vim .env
{% endhighlight %}

Kemudian ubah konfigurasi untuk koneksi database mysql menjadi seperti berikut.

>>Note : yang diubah hanyalah konfigurasi koneksi database mysql saja, jangan melakukan perubahan pada konfigurasi lain, terutama pada konfigurasi APP_KEY, karena jika key yang digunakan tidak sesuai dengan key yang digenerate pada saat proses pembuatan project maka akan muncul error key tidak valid.

{% highlight bash %}
APP_ENV=local
APP_DEBUG=true
APP_KEY=base64:ATM10q5JXcL37xckWACJ7xEIImqAeIRaJR0nRR6RT2A=
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=perpustakaan
DB_USERNAME=root
DB_PASSWORD=root

CACHE_DRIVER=file
SESSION_DRIVER=file
QUEUE_DRIVER=sync

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_DRIVER=smtp
MAIL_HOST=mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
{% endhighlight %}

Setelah selesai, lakukan akses database mysql dengan perintah berikut.

{% highlight bash %}
mysql -u root -p
{% endhighlight %}

Kemudian masukkan password `root`. Sebenarnya kita menggunakan database mariadb, bukan database mysql akan tetapi perintah yang digunakan adalah sama. Kemudian silahkan buat database `perpustakaan` dengan perintah berikut.

{% highlight sql %}
create database perpustakaan;
{% endhighlight %}

Setelah selesai, silahkan keluar dari mysql dengan perintah `exit`. Kemudian silahkan jalankan database migration dengan perintah berikut.

{% highlight bash %}
php artisan migrate
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
root@vagrant-ubuntu-trusty-64:/home/vagrant/Projects/Aplikasi-Perpustakaan# php artisan migrate
Migration table created successfully.
Migrated: 2014_10_12_000000_create_users_table
Migrated: 2014_10_12_100000_create_password_resets_table
Migrated: 2016_05_18_111241_create_buku_table
Migrated: 2016_05_18_111523_create_mahasiswa_table
Migrated: 2016_05_18_111553_create_peminjaman_table
{% endhighlight %}

## Membuat Model Pada Laravel

Pada [artikel sebelumnya](https://rizkimufrizal.github.io/belajar-laravel-bagian-1/), penulis telah menjelaskan bahwa laravel menggunakan pendekatan mvc pada projectnya. Nah yang pertama kali kita buat adalah model, dimana model ini merupakan represntasi dari pada tabel atau data yang terdapat pada database. Karena pada database migration terdapat 3 tabel maka kita harus membuat 3 model untuk mewakili tabel - tabel yang ada di database migration.

Untuk membuat model untuk buku, silahkan jalankan perintah berikut.

{% highlight bash %}
php artisan make:model Buku
{% endhighlight %}

Di bagian model, kita harus mengubah codingan sedikit dikarenakan nama tabel yang akan kita gunakan berbeda, silahkan buka model buku pada folder `app` dengan perintah.

{% highlight bash %}
vim app/Buku.php
{% endhighlight %}

Kemudian ubah codingannya menjadi seperti berikut.

{% highlight php %}
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Buku extends Model
{
    protected $table = 'tb_buku';

    public function peminjamans()
    {
        return $this->hasMany('App\Peminjaman');
    }
}
{% endhighlight %}

Wah beda banget sama codingan php biasanya :O, ada namespace dan use. Yups, sebenarnya tidak banyak perbedaan, mungkin jika anda awalnya pernah belajar java, di java terdapat package dan dapat melakukan import maka di php juga bisa akan tetapi hanya berbeda pada penamaan nya saja. Jika di java terdapat package maka di php kita menggunakan namespace, jika di java kita mengenal import maka di php kita mengenal use :). Pada model buku ini, kita mendeklarasikan nama tabel dimana `$table` ini sebenarnya merupakan override dari class model sehingga kita mengubah nama default dari tabel yang akan kita gunakan. Function `peminjamans` merupakan function yang penulis buat sendiri dengan tujuan karena model buku mempunya relasi dengan model peminjaman dan relasi ini adalah one to many sehingga pada tabel master yaitu buku dapat memiliki banyak peminjaman.

Untuk membuat model mahasiswa, silahkan jalankan perintah berikut.

{% highlight bash %}
php artisan make:model Mahasiswa
{% endhighlight %}

Kemudian ubah codingan pada model mahasiswa seperti berikut.

{% highlight php %}
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Mahasiswa extends Model
{
    protected $table = 'tb_mahasiswa';

    public function peminjamans()
    {
        return $this->hasMany('App\Peminjaman');
    }
}
{% endhighlight %}

Untuk membuat model peminjaman, silahkan jalankan perintah berikut.

{% highlight bash %}
php artisan make:model Peminjaman
{% endhighlight %}

Kemudian ubah codingan pada model peminjaman seperti berikut.

{% highlight php %}
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Peminjaman extends Model
{
    protected $table = 'tb_peminjaman';

    public function buku()
    {
        return $this->belongsTo('App\Buku');
    }

    public function mahasiswa()
    {
        return $this->belongsTo('App\Mahasiswa');
    }
}
{% endhighlight %}

Nah berbeda dengan codingan sebelumnya, karena model peminjaman ini hanya memiliki 1 buku dan 1 mahasiswa maka dia harus menggunakan perintah `belongsTo` untuk melakukan relasi dengan model buku dan mahasiswa.

## Membuat Database Seed Pada Laravel

Setelah selesai dengan model, langkah selanjutnya adalah membuat database seed pada laravel. Database seed ini berfungsi sebagai data awal atau bisa disebut sebagai data dami. Biasanya database seed ini digunakan untuk melakukan testing pada aplikasi atau untuk melakukan sebuah uji coba dengan jumlah data yang diperlukan. Karena terdapat 3 model maka kita membuat database seed sebanyak 3, dimana untuk membuat database seed untuk model buku, silahkan jalankan perintah berikut.

{% highlight bash %}
php artisan make:seeder BukuTableSeeder
{% endhighlight %}

Untuk database seed model mahasiswa, silahkan jalankan perintah berikut.

{% highlight bash %}
php artisan make:seeder MahasiswaTableSeeder
{% endhighlight %}

Untuk database seed model peminjaman, silahkan jalankan perintah berikut.

{% highlight bash %}
php artisan make:seeder PeminjamanTableSeeder
{% endhighlight %}

Langkah selanjutnya, silahkan buka file `ModelFactory.php` yang ada di dalam folder `database/factories` kemudian silahkan tambahkan codingan seperti berikut.

{% highlight php %}
<?php

/*
|--------------------------------------------------------------------------
| Model Factories
|--------------------------------------------------------------------------
|
| Here you may define all of your model factories. Model factories give
| you a convenient way to create models for testing and seeding your
| database. Just tell the factory how a default model should look.
|
*/

$factory->define(App\User::class, function (Faker\Generator $faker) {
    return [
        'name' => $faker->name,
        'email' => $faker->safeEmail,
        'password' => bcrypt(str_random(10)),
        'remember_token' => str_random(10),
    ];
});

//seeder buku
$factory->define(App\Buku::class, function (Faker\Generator $faker) {
    return [
        'id_buku' => $faker->uuid,
        'judul_buku' => $faker->sentence($nbWords = 3, $variableNbWords = true),
        'nama_pengarang' => $faker->name,
        'tahun_terbit' => $faker->year,
        'penerbit' => $faker->sentence($nbWords = 3, $variableNbWords = true),
        'jumlah_buku' => $faker->randomDigit,
        'nomor_rak_buku' => $faker->numerify('buku-###')
    ];
});

//seeder mahasiswa
$factory->define(App\Mahasiswa::class, function (Faker\Generator $faker) {
    return [
        'npm' => $faker->numberBetween($min = 00000000, 99999999),
        'nama' => $faker->name,
        'kelas' => $faker->numerify('k-##'),
        'jenis_kelamin' => $faker->randomElement($array = array('pria', 'wanita')),
        'alamat' => $faker->address
    ];
});

//seeder peminjaman
$factory->define(App\Peminjaman::class, function (Faker\Generator $faker) {
    return [
        'id_peminjaman' => $faker->uuid,
        'tanggal_peminjaman' => $faker->date(),
        'tanggal_batas_pengembalian' => $faker->date(),
        'npm' => factory(App\Mahasiswa::class)->create()->npm,
        'id_buku' => factory(App\Buku::class)->create()->id_buku
    ];
});
{% endhighlight %}

Untuk melakukan inisialisasi data, kita dapat melakukan nya di bagian model factory, dimana data akan secara otomatis diinisialisasikan oleh library faker. Library faker dapat melakukan generate data yang kita inginkan misalnya seperti nama orang, alamat, penomoran, uuid dan lain sebagainya. Dapat dilihat, bahwa di dalam class model factory, penulis membuat 3 seed untuk masing - masing model, nantinya datanya akan secara otomatis diinsert ke database. Pada bagian peminjaman terdapat perbedaan yaitu pada bagian `npm` dan `id_buku`, perbedaan ini dikarenakan pada saat melakukan insert pada peminjaman, maka data buku dan data mahasiswa diwajibkan terlebih dahulu ada. Apabila database seed peminjaman dijalankan maka secara otomatis akan dibuatkan data mahasiswa dan buku terlebih dahulu baru akan dibuatkan data peminjaman.

Kemudian silahkan buka file `BukuTableSeeder.php` yang terdapat di dalam folder `database/seeds` kemudian isikan codingan seperti berikut.

{% highlight php %}
<?php

use Illuminate\Database\Seeder;

class BukuTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        factory(App\Buku::class, 20)->create();
    }
}
{% endhighlight %}

Bisa dilihat bahwa pada class ini, jika database seed dijalankan maka akan ada sebanyak 20 data buku yang akan diinput ke database. Hal ini juga berlaku pada database seed mahasiswa dan peminjaman, untuk database seed mahasiswa, silahkan buka file `MahasiswaTableSeeder.php` kemudian ubah codingannya menjadi seperti berikut.

{% highlight php %}
<?php

use Illuminate\Database\Seeder;

class MahasiswaTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        factory(App\Mahasiswa::class, 20)->create();
    }
}
{% endhighlight %}

Setelah selesai, silahkan buka file `PeminjamanTableSeeder.php` kemudian ubah codingannya menjadi seperti berikut.

{% highlight php %}
<?php

use Illuminate\Database\Seeder;

class PeminjamanTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        factory(App\Peminjaman::class, 20)->create();
    }
}
{% endhighlight %}

Setelah selesai, langkah selanjutnya adalah kita harus mendeklarasikan masing - masing database seed yang telah kita buat ke dalam konfigurasi database seed. Silahkan buka file `DatabaseSeeder.php` yang terdapat di dalam folder `database/seeds` kemudian ubah codingannya menjadi seperti berikut.

{% highlight php %}
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $this->call(BukuTableSeeder::class);
        $this->call(MahasiswaTableSeeder::class);
        $this->call(PeminjamanTableSeeder::class);
    }
}
{% endhighlight %}

Akhirnya kita berada pada tahap yang terakhir :D. Tahap terakhir adalah kita akan menjalankan database seed yang telah kita buat sebelumnya :). Untuk menjalankan database seed silahkan jalankan perintah berikut.

{% highlight bash %}
php artisan db:seed
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

{% highlight bash %}
root@vagrant-ubuntu-trusty-64:/home/vagrant/Projects/Aplikasi-Perpustakaan# php artisan db:seed
Seeded: BukuTableSeeder
Seeded: MahasiswaTableSeeder
Seeded: PeminjamanTableSeeder
{% endhighlight %}

Atau jika anda ingin melakukan database migration sekaligus dengan database seed juga bisa dengan menjalankan perintah berikut.

{% highlight bash %}
php artisan migrate:refresh --seed
{% endhighlight %}

Berikut adalah output jika anda berhasil menjalankan perintah diatas.

{% highlight bash %}
root@vagrant-ubuntu-trusty-64:/home/vagrant/Projects/Aplikasi-Perpustakaan# php artisan migrate:refresh --seed
Rolled back: 2016_05_18_111553_create_peminjaman_table
Rolled back: 2016_05_18_111523_create_mahasiswa_table
Rolled back: 2016_05_18_111241_create_buku_table
Rolled back: 2014_10_12_100000_create_password_resets_table
Rolled back: 2014_10_12_000000_create_users_table
Migrated: 2014_10_12_000000_create_users_table
Migrated: 2014_10_12_100000_create_password_resets_table
Migrated: 2016_05_18_111241_create_buku_table
Migrated: 2016_05_18_111523_create_mahasiswa_table
Migrated: 2016_05_18_111553_create_peminjaman_table
Seeded: BukuTableSeeder
Seeded: MahasiswaTableSeeder
Seeded: PeminjamanTableSeeder
{% endhighlight %}

Jika anda masih penasaran dengan database seed apakah telah diinput datanya atau belum, silahkan lakukan pengecekan pada database mariadb anda :).

Sekian tutorial belajar laravel bagian 2, untuk bagian selanjutnya InsyaAllah akan segera di publish dan Terima kasih :).