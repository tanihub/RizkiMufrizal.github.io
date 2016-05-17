---
layout: post
title: Belajar Laravel Bagian 1
modified:
categories: 
description: belajar laravel
tags: [laravel, phpstorm, vagrant, codeigniter, mvc, nginx, mariadb, hhvm, ide-helper]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-05-14T10:56:41+07:00
---

## Apa Itu Laravel ?

>>[Laravel](https://laravel.com/) adalah framework berbasis PHP yang open source, menggunakan konsep model–view–controller (MVC). Laravel berada dibawah lisensi MIT.

Beberapa tahun yang lalu, framework laravel tidak begitu terkenal di dalam dunia pemrograman php, akan tetapi dengan perkembangan teknologi tidak dapat dipungkiri sehingga framework laravel berkembang begitu pesat. Terdapat banyak kelebihan di dalam framework laravel salah satu nya adalah memungkinkan kita menggunakan konsep orm, dimana di dalam laravel biasanya kita menggunakan eloquent orm untuk mengabstraksikan antara model object dengan database.

Singkat cerita, penulis ingin belajar mengenai framework laravel, biasanya penulis menggunakan framework [codeigniter](https://www.codeigniter.com/) untuk membuat aplikasi. Framework laravel merupakan framework yang telah banyak digunakan untuk aplikasi production, dan sangat dihandalkan untuk urusan keamanan sebuah website. Jika dilihat dari case study, framework laravel merupakan framework yang bisa dikatakan lumayan lama jika ingin anda pelajari dikarenakan berbagai konfigurasi dan pemahaman yang harus anda kuasai. Pada artikel ini, penulis akan membagi beberapa bagian dikarenakan artikel akan sangat panjang jika membahas laravel hanya pada 1 bagian artikel saja.

## Setup Project

Langkah yang pertama kali kita lakukan adalah melakukan setup project. Untuk mempermudah tahap development, kita akan menggunakan beberapa tool berikut diantaranya adalah

1. Vagrant, bagi yang belum mengerti apa itu vagrant, silahkan lihat di artikel [belajar vagrant](https://rizkimufrizal.github.io/belajar-vagrant/).
2. [Vim](http://www.vim.org/) merupakan editor yang akan kita gunakan

sedangkan tool yang lain seperti composer, hhvm, nginx dan lain sebagainya akan kita lakukan konfigurasi pada vagrant.

### Membuat Konfigurasi Vagrant

Silahkan buat sebuah folder `Belajar-Laravel`, kemudian akses folder tersebut dengan menggunakan terminal. Lalu kita akan melakukan inisialisasi vagrant dengan perintah.

{% highlight bash %}
vagrant init
{% endhighlight %}

maka akan dibentuk sebuah file yaitu `Vagrantfile`, silahkan buka file tersebut kemudian ubah seperti berikut.

{% highlight bash %}
Vagrant.configure(2) do |config|
  config.vm.box = 'ubuntu/trusty64'
  config.vm.provider 'virtualbox' do |v|
    v.memory = 1024
  end
  config.vm.provision 'shell', path: 'setup.sh'
  config.vm.network 'forwarded_port', guest: 80, host: 8080
  config.vm.synced_folder 'Projects', '/home/vagrant/Projects', create: true, owner: 'www-data', group: 'www-data'
end
{% endhighlight %}

kemudian silahkan buat sebuah file dengan nama `setup.sh` untuk konfigurasi provisioning, kemudian isikan codingan shell seperti berikut.

{% highlight bash %}
#!/bin/sh
#
# Copyright (C) 2016 Rizki Mufrizal <mufrizalrizki@gmail.com>
#
# Distributed under terms of the MIT license.
#

echo "proses provisioning"

echo "konfigurasi sources dan environment"
cp /vagrant/configuration/sources.list /etc/apt/sources.list
cp /vagrant/configuration/environment /etc/environment

echo "Tambah Repo Git"
sudo add-apt-repository -y ppa:git-core/ppa

echo "Tambah Repo Nginx"
add-apt-repository -y ppa:nginx/stable

echo "Tambah Repo Mariadb"
apt-get install software-properties-common
apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
add-apt-repository 'deb [arch=amd64,i386] http://mariadb.biz.net.id/repo/10.1/ubuntu trusty main'

echo "Tambah Repo HHVM"
apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0x5a16e7281be7a449
add-apt-repository "deb http://dl.hhvm.com/ubuntu $(lsb_release -sc) main"

apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y

echo "Konfigurasi Mariadb"
apt-get install -y debconf-utils
echo mariadb-server-10.1 mysql-server/root_password password root | debconf-set-selections
echo mariadb-server-10.1 mysql-server/root_password_again password root | debconf-set-selections

apt-get install -y git nginx mariadb-server mariadb-client hhvm vim

echo "Konfigurasi Virtual Host Nginx"
cp /vagrant/configuration/nginx-vhost /etc/nginx/sites-available/belajarlaravel
ln -s /etc/nginx/sites-available/belajarlaravel /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo "Konfigurasi HHVM"
/usr/share/hhvm/install_fastcgi.sh
update-rc.d hhvm defaults
/usr/bin/update-alternatives --install /usr/bin/php php /usr/bin/hhvm 60
service nginx restart

echo "Install Composer"
curl -sS https://getcomposer.org/installer | php
mv /home/vagrant/composer.phar /usr/local/bin/composer

echo "konfigurasi vim"
cp /vagrant/configuration/.vimrc /home/vagrant
{% endhighlight %}

Kemudian untuk environment pada vagrant silahkan buat sebuah folder dengan nama `configuration` dan buat lah file `environment`, `nginx-vhost`, `sources.list` dan `.vimrc` di dalam nya. Untuk file environment, silahkan isikan dengan value seperti berikut.

{% highlight bash %}
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
LC_CTYPE="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
{% endhighlight %}

untuk file nginx-vhost seperti berikut.

{% highlight bash %}
server {
  listen 80 default_server;
  listen [::]:80 default_server;
  root /home/vagrant/Projects/Aplikasi-Perpustakaan/public;
  index index.html index.php index.htm index.nginx-debian.html;

  server_name perpustakaan.com;
  include hhvm.conf;

  location / {
    try_files $uri $uri/ /index.php?$query_string;
  }
}
{% endhighlight %}

untuk file sources.list silahkan masukkan codingan berikut.

{% highlight bash %}
deb http://kambing.ui.ac.id/ubuntu trusty main universe multiverse
deb http://kambing.ui.ac.id/ubuntu trusty-updates main universe multiverse
deb http://security.ubuntu.com/ubuntu trusty-security main universe multiverse
{% endhighlight %}

dan yang terakhir untuk file .vimrc isikan seperti berikut.

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

Nah konfigurasi vagrant telah selesai, silahkan jalankan dengan perintah

{% highlight bash %}
vagrant up
{% endhighlight %}

untuk melakukan akses silahkan jalankan perintah berikut.

{% highlight bash %}
vagrant ssh
{% endhighlight %}

### Membuat Project Laravel Dengan Composer

Untuk membuat project laravel, kita dapat menggunakan composer. Akses folder Projects yang ada di home vagrant, lalu jalankan perintah berikut untuk membuat project laravel dengan menggunakan composer.

{% highlight bash %}
composer create-project --prefer-dist laravel/laravel Aplikasi-Perpustakaan
{% endhighlight %}

secara otomatis, composer akan membuatkan anda sebuah project laravel dengan nama Aplikasi-Perpustakaan. 

Setelah selesai, yang perlu diperhatikan lagi adalah bahwa agar project laravel benar - benar dapat menggunakan autocomplete pada vim, kita akan menggunakan [laravel IDE helper](https://github.com/barryvdh/laravel-ide-helper). Silahkan buka kembali terminal pada vagrant anda, silahkan akses folder project laravel anda, lalu ketikan perintah berikut.

{% highlight bash %}
composer require barryvdh/laravel-ide-helper
{% endhighlight %}

Jika telah selesai, silahkan buka file `app.php` yang berada di dalam folder `config/`, kemudian cari array `providers` silahkan tambahkan codingan seperti berikut.

{% highlight php %}
Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class
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

      Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class

],
{% endhighlight %}

Setelah selesai, silahkan jalankan perintah berikut untuk melakukan generate ide helper dengan perintah

{% highlight php %}
php artisan ide-helper:generate
{% endhighlight %}

Jika berhasil maka akan muncul output seperti berikut.

![Screenshot from 2016-05-14 10-44-28.png](../images/Screenshot from 2016-05-14 10-44-28.png)

Kemudian silahkan akses folder project anda untuk memberikan hak akses terhadap folder project laravel, sebelum memberikan hak akses, kita terlebih dahulu untuk membersihkan cache dengan perintah

{% highlight bash %}
php artisan cache:clear
{% endhighlight %}

langkah selanjutnya silahkan berikan hak akses dengan perintah

{% highlight bash %}
sudo chmod -R 777 *
{% endhighlight %}

kemudian lakukan load composer dengan perintah

{% highlight bash %}
composer dump-autoload
{% endhighlight %}

### Melakukan Setting Domain

Setelah melakukan konfigurasi seperti diatas, langkah selanjutnya adalah membuat domain sendiri, sehingga kita akan melakukan akses aplikasi dengan menggunakan nama domain. Silahkan buka file `hosts` dengan perintah

{% highlight bash %}
sudo nano /etc/hosts
{% endhighlight %}

lalu tambahkan script berikut.

{% highlight bash %}
127.0.0.1       perpustakaan.com
{% endhighlight %}

Silahkan akses web anda pada browser di `http://perpustakaan.com:8080`, jika berhasil maka outputnya seperti berikut.

![Screenshot from 2016-05-14 17-48-57.png](../images/Screenshot from 2016-05-14 17-48-57.png)

Sekian tutorial belajar laravel bagian 1, untuk bagian selanjutnya InsyaAllah akan segera di publish dan Terima kasih :).