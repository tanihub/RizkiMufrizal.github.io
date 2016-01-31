---
layout: post
title: Konfigurasi Reverse Proxy Pada Nginx
modified:
categories: 
excerpt:
tags: [nginx, reverse proxy, proxy]
image:
  background: abstract-3.png
date: 2016-01-03T11:15:23+07:00
comments: true
---

Sebelum kita membuat konfigurasi reverse proxy, ada baiknya kita membahas sekilas tentang proxy :).

##Apa Itu Proxy Server ?

>>Proxy Server adalah sebuah server yang menyediakan layanan perantara antara client host dengan server lain.

Berikut adalah gambaran umum dari proxy server

![Screenshot from 2016-01-03 17:07:12.png](../images/Screenshot from 2016-01-03 17:07:12.png)

Dari gambar diatas dapat dilihat bahwa sebenarnya user atau client melakukan akses terhadap proxy server, selanjutnya proxy server akan meneruskan akses ke server yang dituju. Biasanya proxy server digunakan sebagai cache, keamanan, load balancing dan lain sebagainya.

##Apa Itu Reverse Proxy ?

>>Reverse Proxy adalah salah satu jenis dari proxy, biasanya reverse proxy digunakan sebagai perantara antara client dengan web server.

Berikut adalah arsitektur dari reverse proxy.

![Screenshot from 2016-01-03 17:47:48.png](../images/Screenshot from 2016-01-03 17:47:48.png)

Dari gambar diatas dapat dilihat bahwa sebuah proxy dapat menghandle beberapa web server. Adapun cara kerjanya adalah client akan melakukan akses terhadap sebuah URL misalnya [https://www.google.co.id](https://www.google.co.id/) maka secara otomatis client akan melakukan request terlebih dahulu ke proxy server akan tetapi seolah - olah client melakukan request langsung ke web server. Setelah menerima request dari client, maka proxy server akan meneruskan request tersebut ke web server yang dituju. Untuk mempermudah pemahaman langsung saja kita melakukan setting reverse proxy pada nginx :D.

##Setup Vagrant

Pada artikel kali ini, penulis akan menggunakan vagrant. Bagi yang belum tau apa itu vagrant, silahkan lihat di [Belajar Vagrant](http://rizkimufrizal.github.io/belajar-vagrant/). Pada kesempatan kali ini, penulis menggunakan box [ubuntu/trusty64](https://atlas.hashicorp.com/ubuntu/boxes/trusty64) atau ubuntu 14.04 LTS.

Silahkan buat sebuah folder `ubuntu-nginx-reverse-proxy` kemudian jalankan perintah berikut di dalam folder tersebut.

{% highlight bash %}
vagrant init
{% endhighlight %}

Lalu buka file `Vagrantfile` lalu ubah codingannya menjadi seperti berikut.

{% highlight bash %}
Vagrant.configure(2) do |config|

    #konfigurasi box untuk sistem operasi ubuntu trusty 64 bit
    config.vm.box = "ubuntu/trusty64"

    config.vm.provider "virtualbox" do |vb|
    
        #konfigurasi virtual box dengan ram 1 GB
        vb.memory = "1024"
    end
    
    #konfigurasi provisioning
    config.vm.provision "shell", path: "install.sh"
    
    #konfigurasi network
    #port forwarding
    config.vm.network "forwarded_port", guest: 80, host: 8080
end
{% endhighlight %}

##Membuat File Executable Shell

File ini dibuat berfungsi sebagai provisioning pada vagrant, silahkan buat sebuah file `install.sh` kemudian isikan codingan seperti berikut.

{% highlight bash %}
#! /bin/sh
#
# install.sh
# Copyright (C) 2016 Rizki Mufrizal <mufrizalrizki@gmail.com>
#
# Distributed under terms of the MIT license.
#

echo "mulai Provisioning"

echo "setup software source dan repository"
cp /vagrant/config/sources.list /etc/apt/sources.list
cp /vagrant/config/environment /etc/environment

add-apt-repository -y ppa:git-core/ppa
add-apt-repository -y ppa:nginx/stable

echo "memulai update"
apt-get update

echo "mulai upgrade"
apt-get upgrade -y

echo "mulai dist upgrade"
apt-get dist-upgrade -y

echo "instalasi build essential"
apt-get install -y build-essential libssl-dev

echo "instalasi git"
apt-get install -y git

echo "instalasi nginx"
apt-get install -y nginx

echo "Konfigurasi reverse proxy nginx"
cp /vagrant/config/nginx-proxy /etc/nginx/sites-available/reverseproxy
ln -s /etc/nginx/sites-available/reverseproxy /etc/nginx/sites-enabled/reverseproxy
rm /etc/nginx/sites-enabled/default
service nginx restart

echo "instalasi node js"
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
apt-get install -y nodejs

echo "instalasi coffeescript"
npm install -g coffee-script

echo "instalasi nodemon"
npm install -g nodemon

echo "buat folder untuk project node js"
mkdir Belajar-Reverse-Proxy

echo "ke folder Belajar-Reverse-Proxy"
cd Belajar-Reverse-Proxy

echo "copy app.js ke folder Belajar-Reverse-Proxy"
cp /vagrant/config/app.js /home/vagrant/Belajar-Reverse-Proxy/app.js

echo "jalankan server"
nodemon app.js
{% endhighlight %}

##Membuat Konfigurasi Reverse Proxy Nginx

Langkah selanjutnya adalah silahkan anda buat sebuah folder `config`, di dalam folder tersebut silahkan buat sebuah file `nginx-proxy`. Kemudian isikan codingan berikut pada file tersebut.

{% highlight bash %}
server {
    listen 80;

    server_name ubuntu.nginx.com
    access_log /var/log/nginx/access.log;

    location / {
        proxy_pass http://127.0.0.1:3000;
    }

}
{% endhighlight %}

##Konfigurasi Repository Dan Environment

Karena kita menggunakan ubuntu, maka tahap selanjutnya kita melakukan konfigurasi repository dan environment pada ubuntu. Untuk konfigurasi repository, kita akan menggunakan repository [kambing ui](http://kambing.ui.ac.id/). Silahkan buat sebuah file `sources.list` kemudian isikan codingan berikut.

{% highlight bash %}
#repository kambing ui
deb http://kambing.ui.ac.id/ubuntu trusty main universe multiverse
deb http://kambing.ui.ac.id/ubuntu trusty-updates main universe multiverse
deb http://security.ubuntu.com/ubuntu trusty-security main universe multiverse
{% endhighlight %}

Untuk konfigurasi environment, silahkan buat sebuah file `environment` dan berikut adalah isi codingannya.

{% highlight bash %}
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
LC_CTYPE="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
{% endhighlight %}

##Membuat Aplikasi Web Node JS

Semua konfigurasi untuk bagian nginx telah disiapkan, langkah terakhir adalah membuat sebuah aplikasi web dengan node js yang berfungsi sebagai server side sedangkan nginx berfungsi sebagai reverse proxy. Silahkan buat sebuah file `app.js` di dalam folder `config` kemudian isikan codingan node js seperti berikut ini.

{% highlight bash %}
/*
 * Belajar-Reverse-Proxy.js
 * Copyright (C) 2016 Rizki Mufrizal <mufrizalrizki@gmail.com>
 *
 * Distributed under terms of the MIT license.
 */

var http = require('http');

http.createServer(function (req, res) {
    res.writeHead(200, {
        'Content-Type': 'text/plain'
    });
    res.end('Belajar Reverse Proxy Pada Nginx\n');
}).listen(3000, '127.0.0.1');

console.log('Server jalan di http://127.0.0.1:3000/');
{% endhighlight %}

##Arsitektur Reverse Proxy

Arsitektur reverse proxy yang akan digunakan adalah sebagai berikut.

![Screenshot from 2016-01-03 18:04:39.png](../images/Screenshot from 2016-01-03 18:04:39.png)

Dari gambar diatas, terdapat 2 teknologi yang kita gunakan diantaranya adalah.

* **Node js** : sebagai server side
* **Nginx** : sebagai reverse proxy

Node js berjalan pada port 3000 sedangkan nginx akan berjalan pada port 80. Agar nginx dapat dijadikan sebagai reverse proxy, kita harus melakukan beberapa konfigurasi pada nginx. Kongurasi tersebut sebenarnya telah kita buat pada file `install.sh`. Konfigurasi tersebut adalah pada bagian

{% highlight bash %}
#! /bin/sh
#
# install.sh
# Copyright (C) 2016 Rizki Mufrizal <mufrizalrizki@gmail.com>
#
# Distributed under terms of the MIT license.
#
echo "Konfigurasi reverse proxy nginx"
cp /vagrant/config/nginx-proxy /etc/nginx/sites-available/reverseproxy
ln -s /etc/nginx/sites-available/reverseproxy /etc/nginx/sites-enabled/reverseproxy
rm /etc/nginx/sites-enabled/default
service nginx restart
{% endhighlight %}

Dari codingan diatas dapat bahwa kita mencopy file `nginx-proxy` dari folder `/vagrant/config/` ke dalam folder `/etc/nginx/sites-available/`. Kemudian tahap selanjutnya kita melakukan symlink dari file `reverseproxy` ke dalam folder `/etc/nginx/sites-enabled/`.

##Testing Reverse Proxy

Tahap terakhir yaitu melakukan testing reverse proxy. Silahkan masuk ke folder `ubuntu-nginx-reverse-proxy` melalui terminal lalu jalankan perintah berikut.

{% highlight bash %}
vagrant up
{% endhighlight %}

Maka vagrant akan menjalankan sistem operasi ubuntu dan melakukan provisioning, berikut adalah outputnya.

{% highlight bash %}
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'ubuntu/trusty64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'ubuntu/trusty64' is up to date...
==> default: Setting the name of the VM: ubuntu-nginx-reverse-proxy_default_1452844055124_38807
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 80 (guest) => 8080 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default: 
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default: 
    default: Guest Additions Version: 4.3.34
    default: VirtualBox Version: 5.0
==> default: Mounting shared folders...
    default: /vagrant => /home/rizki/Documents/vagrant/ubuntu-nginx-reverse-proxy
==> default: Running provisioner: shell...
    default: Running: /tmp/vagrant-shell20160115-17586-14nuj0k.sh
{% endhighlight %}

Jika semua instalasi telah selesai, maka anda dapat melihat outputnya seperti berikut ini.

{% highlight bash %}
==> default: jalankan server
==> default: [nodemon] 1.8.1
==> default: [nodemon] to restart at any time, enter `rs`
==> default: [nodemon] watching: *.*
==> default: [nodemon] starting `node app.js`
==> default: Server jalan di http://127.0.0.1:3000/
{% endhighlight %}

Output diatas dapat dikatakan bahwa aplikasi node js telah berjalan dengan baik. Kemudian silahkan akses `http://127.0.0.1:8080/`. Berikut adalah tampilan hasilnya :).

![Screenshot from 2016-01-15 14:59:12.png](../images/Screenshot from 2016-01-15 14:59:12.png)

Untuk source code nya silahkan lihat di [ubuntu nginx reverse proxy](https://github.com/RizkiMufrizal/ubuntu-nginx-reverse-proxy). Sekian artikel mengenai konfigurasi reverse proxy pada nginx dan terima kasih :)