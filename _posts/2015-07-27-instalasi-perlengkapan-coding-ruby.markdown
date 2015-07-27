---
layout: post
title: Instalasi Perlengkapan Coding Ruby
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-07-27T07:06:33+07:00
---

Sebelumnya kita pernah membahas tentang bagaimana [instalasi java pada linux](http://rizkimufrizal.github.io/instalasi-perlengkapan-coding-java/). Kali ini penulis akan menjelaskan bagaimana instalasi ruby pada linux.

>Ruby adalah bahasa pemrograman dinamis berbasis skrip yang berorientasi obyek. Tujuan dari ruby adalah menggabungkan kelebihan dari semua bahasa-bahasa pemrograman skrip yang ada di dunia. Ruby ditulis dengan bahasa pemrograman C dengan kemampuan dasar seperti Perl dan Python.

Ruby juga memiliki berbagai framework diantaranya adalah

- Ruby On Rails (ROR)
- Sinatra
- Lotus
- Padrino
- Nyny
- Grape
- Nancy
- dan lain - lain

Baiklah untuk melakukan instalasi ruby pada linux, kita menggunakan [RVM (ruby version manager)](https://rvm.io/). Berikut adalah tahapan untuk melakukan instalasi ruby pada linux.

Buka terminal lalu install public key terlebih dahulu dengan perintah

{% highlight bash %}
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
{% endhighlight %}

Lalu kita melakukan instalasi ruby yang paling stable dengan perintah

{% highlight bash %}
\curl -sSL https://get.rvm.io | bash -s stable --ruby
{% endhighlight %}

Kemudian lakukan path pada linux, buka file `environment` dengan sintak `sudo gedit /etc/environment`. kemudian masukkan sintak `RVM_HOME=/home/rizki/.rvm` dan pada bagian PATH masukkan `/home/rizki/.rvm/bin`, jangan lupa sesuaikan dengan folder anda.

Restart komputer dan cek versi rvm, ruby dan gem dengan perintah

{% highlight bash %}
rvm -v
ruby -v
gem -v
{% endhighlight %}

Jika sudah, maka tahap selanjutnya adalah install `bundler`. Bundler ini sendiri berfungsi untuk mendownload dependency library yang dibutuhkan oleh project kita nantinya. Bedanya bundler dengan gem yaitu gem hanya sebagai penentu dari versi library yang akan kita gunakan sedangkan bundler akan mendownload semua kepentingan dari project dan akan memeriksa apakah sebuah library mempunyai missing dengan library yang lainnya. install bundler dengan perintah.

{% highlight bash %}
gem install bundler
{% endhighlight %}

kemudian cek versi bundler dengan perintah

{% highlight bash %}
bundler -v
{% endhighlight %}

Untuk melihat versi ruby yang telah diinstall di pc maka perintahnya

{% highlight bash %}
rvm list
{% endhighlight %}

jika ingin mengetahui versi release ruby dengan perintah

{% highlight bash %}
rvm list known
{% endhighlight %}

Untuk menggunakan interpreter ruby dapat dengan menggunakan perintah `irb` pada terminal. Selanjutnya untuk IDE ruby, penulis menggunakan ruby mine dari produknya jetbrains. Anda dapat menggunakan IDE selain dar ruby mine tapi penulis menyarankan menggunakan ruby mine dikarenakan karena kelangkapan dari fitur ruby mine seperti auto complete, support framework ruby on rails. Download terlebih dahulu di [ruby mine](https://www.jetbrains.com/ruby/). ekstrak pada folder tertentu, beri akses eksekusi pada file `rubymine.sh` pada folder bin, kemudian jalankan dengan perintah `./rubymine.sh`. Secara otomatis IDE tersebut akan membuat shortcut pada linux anda.

Sekian tutorial kali ini dan selamat coding ruby. Terima kasih :).
