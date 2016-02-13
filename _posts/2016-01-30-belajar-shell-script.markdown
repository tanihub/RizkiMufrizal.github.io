---
layout: post
title: Belajar Shell Script
modified:
categories: 
description: belajar shell script pada linux
tags: [shell, shell script, linux, bash]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-01-30T17:16:48+07:00
---

Berhubung penulis sedang membuat konfigurasi dan instalasi aplikasi pada ubuntu dengan menggunakan bahasa shell maka pada artikel ini, penulis akan membahas sedikit tentang shell script :D. Sebelum penulis membahas tentang shell, alangkah baiknya kita membahas sedikit teori tentang shell.

## Apa itu Shell ?

>>Shell adalah sebuah program penterjemah yang berfungsi sebagai jembatan antara user dan kernel.

Biasanya shell akan menyediakan sebuah interface, dimana interface ini berfungsi sebagai tempat untuk memberikan perintah - perintah. Linux memiliki berbagai macam shell, diantaranya adalah :

* Bourne shell(sh)
* C shell(csh)
* Korn shell(ksh)
* Bourne again shell(bash)
* dsb

Pada artikel ini, penulis hanya menggunakan bash shell GNU yang merupakan pengembangan dari bourne shell.

## Apa itu Shell Script ?

>>Shell Script adalah sebuah bahasa pemrograman yang disusun berdasarkan perintah - perintah shell.

Jika anda menggunakan linux, maka menyusun perintah - perintah shell di dalam sebuah file shell sama seperti ketika anda membuat sebuah aplikasi. Agar artikel tidak terlalu panjang, mari kita bahas bagaimana implementasi shell script pada linux.

## Setup Vagrant

Seperti biasanya, penulis akan menggunakan vagrant. Bagi yang belum mengerti apa itu vagrant, silahkan akses di [Belajar Vagrant](http://rizkimufrizal.github.io/belajar-vagrant/). Kali ini kita akan menggunakan box [debian/jessie64](https://atlas.hashicorp.com/debian/boxes/jessie64) atau debian 8.

Silahkan buat sebuah folder `belajar-shell` kemudian jalankan perintah

{% highlight bash %}
vagrant init
{% endhighlight %}

Buka file `Vagrantfile` lalu ubah konfigurasinya menjadi seperti berikut.

{% highlight bash %}
Vagrant.configure(2) do |config|

    #konfigurasi box untuk sistem operasi debian 8 64 bit
    config.vm.box = "debian/jessie64"

    config.vm.provider "virtualbox" do |vb|
    
        #konfigurasi virtual box dengan ram 1 GB
        vb.memory = "1024"
    end

end
{% endhighlight %}

Kemudian jalankan vagrant dengan perintah

{% highlight bash %}
vagrant up
{% endhighlight %}

lalu login ke vagrant dengan perintah

{% highlight bash %}
vagrant ssh
{% endhighlight %}

## Membuat Hello Word Dengan Shell Script

Sebelum memulai, silahkan install editor terlebih dahulu, penulis menggunakan editor `vim`. Untuk melakukan instalasi vim, jalankan perintah berikut.

{% highlight bash %}
sudo apt-get install vim
{% endhighlight %}

Setelah selesai melakukan instalasi vim, selanjutkan buat sebuah file dengan perintah

{% highlight bash %}
touch belajar.sh
{% endhighlight %}

kemudian buka file `belajar.sh` dengan perintah

{% highlight bash %}
vim belajar.sh
{% endhighlight %}

Untuk memasukkan codingan, silahkan tekan tombol `i` kemudian ketikkan kodingan berikut.

{% highlight bash %}
#!/bin/sh
#
# Copyright (C) 2016 Rizki Mufrizal <mufrizalrizki@gmail.com>
#
# Distributed under terms of the MIT license.
#

echo "hello word"
{% endhighlight %}

setelah selesai, tekan tombol `esc` kemudian ketik tuliskan perintah

{% highlight bash %}
:wq
{% endhighlight %}

maka secara otomatis vim akan menyimpan codingan tersebut ke dalam file `belajar.sh`. Langkah selanjutnya adalah kita akan memberikan hak execute untuk file tersebut, Silahkan jalankan perintah berikut.

{% highlight bash %}
chmod a+x belajar.sh
{% endhighlight %}

untuk menjalankan file tersebut dengan perintah.

{% highlight bash %}
./belajar.sh
{% endhighlight %}

maka akan muncul output `hello word`, output tersebut berasal dari perintah `echo`, dimana echo disini sama seperti perintah `puts` pada ruby, `printf` pada bahasa c dan sama seperti bahasa pemrograman lainnya.

## Membuat Inputan

Tahap selanjutnya adalah membuat inputan, disini user akan memberikan sebuah inputan dimana inputan ini nantinya akan ditampikan lagi. Silahkan buka file `belajar.sh` lalu ubah codingannya menjadi seperti berikut.

{% highlight bash %}
#!/bin/sh
#
# Copyright (C) 2016 Rizki Mufrizal <mufrizalrizki@gmail.com>
#
# Distributed under terms of the MIT license.
#

echo -n "Masukkan nama anda : "
read nama

echo "hello $nama"
{% endhighlight %}

perintah `read` berfungsi untuk mengambil value dari inputan user, dimana inputan user tersebut akan disimpan ke dalam variabel `nama` lalu variabel nama tersebut di cetak pada perintah `echo` yang kedua.

## Membuat Konfigurasi File

Setelah melewati cara inputan user, langkah selanjutnya adalah kita ingin membuat konfigurasi file pada linux. Misalnya kita ingin membuat konfigurasi file environment pada debian dan melakukan update software. Untuk melakukan konfigurasi pada file environment, maka ubah codingan pada file `belajar.sh` menjadi berikut.

{% highlight bash %}
#!/bin/sh
#
# Copyright (C) 2016 Rizki Mufrizal <mufrizalrizki@gmail.com>
#
# Distributed under terms of the MIT license.
#

echo -n "masukkan user linux anda : "
read user

echo "install maven"
mkdir -p /home/$user/programming/build-tool/apache-maven
wget http://mirror.wanxp.id/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
tar -xvzf apache-maven-3.3.9-bin.tar.gz
rm apache-maven-3.3.9-bin.tar.gz
mv apache-maven-3.3.9/* /home/$user/programming/build-tool/apache-maven/
rmdir apache-maven-3.3.9/

echo "membuat file environment"
touch environment

echo "membuat path maven"
echo "M2_HOME=/home/$user/programming/build-tool/apache-maven" | tee -a /home/$user/environment

echo "copy file environment"
sudo cp environment /etc/environment
{% endhighlight %}

Perintah diatas berfungsi untuk melakukan konfigurasi maven, dimana kita akan membuat sebuah folder berdasarkan nama user linux, kemudian path ditambahkan ke dalam konfigurasi environment, hasil konfigurasi environment tersebut akan copy ke file environment yang aslinya.

File shell ini juga dapat melakukan perintah - perintah ubuntu seperti :

* apt-get update
* apt-get upgrade
* dsb

Pada artikel ini, penulis mencoba melakukan update dan upgrade aplikasi pada debian. Silahkan ubah codingan pada file `belajar.sh` seperti berikut.

{% highlight bash %}
#!/bin/sh
#
# Copyright (C) 2016 Rizki Mufrizal <mufrizalrizki@gmail.com>
#
# Distributed under terms of the MIT license.
#

echo -n "masukkan user linux anda : "
read user

echo "install maven"
mkdir -p /home/$user/programming/build-tool/apache-maven
wget http://mirror.wanxp.id/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
tar -xvzf apache-maven-3.3.9-bin.tar.gz
rm apache-maven-3.3.9-bin.tar.gz
mv apache-maven-3.3.9/* /home/$user/programming/build-tool/apache-maven/
rmdir apache-maven-3.3.9/

echo "membuat file environment"
touch environment

echo "membuat path maven"
echo "M2_HOME=/home/$user/programming/build-tool/apache-maven" | tee -a /home/$user/environment

echo "copy file environment"
sudo cp environment /etc/environment

echo "update aplikasi"
sudo apt-get update

echo "upgrade aplikasi"
sudo apt-get upgrade -y

echo "dist upgrade aplikasi"
sudo apt-get dist-upgrade -y
{% endhighlight %}

kemudian jalankan file tersebut, untuk nama user linux silahkan masukkan `vagrant` dan outpunya seperti berikut.

{% highlight bash %}
membuat file environment
membuat path maven
M2_HOME=/home/vagrant/programming/build-tool/apache-maven
copy file environment
update aplikasi
Ign http://httpredir.debian.org jessie InRelease            
Hit http://security.debian.org jessie/updates InRelease
Hit http://httpredir.debian.org jessie Release.gpg
Hit http://security.debian.org jessie/updates/main Sources
Hit http://security.debian.org jessie/updates/main amd64 Packages
Hit http://httpredir.debian.org jessie Release                                
Hit http://security.debian.org jessie/updates/main Translation-en  
Hit http://httpredir.debian.org jessie/main Sources
Hit http://httpredir.debian.org jessie/main amd64 Packages                
Hit http://httpredir.debian.org jessie/main Translation-en                
Reading package lists... Done
upgrade aplikasi
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
dist upgrade aplikasi
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
{% endhighlight %}

Sekian tutorial tentang belajar shell script, bagi anda yang ingin melihat konfigurasi untuk instalasi ubuntu, silahkan akses di [Perlengkapan Ubuntu](https://github.com/RizkiMufrizal/Perlengkapan-Ubuntu) dan terima kasih :).