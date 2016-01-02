---
layout: post
title: Belajar Vagrant
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2016-01-02T07:31:24+07:00
---

Ketika kita melakukan development terhadap sebuah aplikasi terkadang kita membutuhkan tool - tool penting untuk membantu pada developer untuk membuat sebuah aplikasi. Ada beberapa masalah yang sering ditemui oleh seorang developer diantaranya adalah :

* Kesulitan ketika antar developer menggunakan sistem operasi yang berbeda.
* Kesulitan ketika server yang digunakan menggunakan sistem operasi yang berbeda.
* Kesulitan ketika project yang sedang dikerjakan tidak dapat berjalan di PC yang lain.

Dari beberapa kesulitan tersebut kita dapat meminimalkannya dengan menggunakan [vagrant](https://www.vagrantup.com/). Apa itu vagrant ?

>>[Vagrant](https://www.vagrantup.com/) adalah sebuah software yang menggunakan teknologi virtual machine dimana kita dapat membuat lingkungan development secara portable, konsisten dan lebih fleksible.

Dikarenakan vagrant menggunakan teknologi virtual machine maka kita membutuhkan software seperti [virtual box](https://www.virtualbox.org/) dan [VmWare](http://www.vmware.com/). Tujuannya adalah kita ingin membuat sebuah lingkungan development secara portable, contohnya misalnya pada saat production kita akan menggunakan sistem operasi [ubuntu](http://www.ubuntu.com/) maka pada saat development kita akan menggunakan ubuntu sebagai sistem operasi sehingga pada saat proses deploy ke production diharapkan tidak ada lagi permasalahan yang muncul.

Ketika ingin melakukan duplikasi atau dibagikan kepada team developer yang lain maka kita tinggal menggunakan perintah vagrant sehingga secara otomatis vagrant akan melakukan konfigurasi pada PC masing - masing team developer sehingga mempermudah dalam development sebuah aplikasi.

##Instalasi Virtual Box

Pada artikel ini, penulis menggunakan virtual box dikarenakan virtual box merupakan software open source dan lebih mudah dikonfigurasikan. Silahkan buka file `sources.list` dengan menjalankan perintah.

{% highlight bash %}
sudo gedit /etc/apt/sources.list
{% endhighlight %}

kemudian tambahkan sintak berikut pada baris akhir

{% highlight bash %}
deb http://download.virtualbox.org/virtualbox/debian trusty contrib
{% endhighlight %}

Jangan lupa sesuiakan dengan sistem operasi anda, disini saya menggunakan ubuntu 14.04 yaitu trusty, Setelah selesai kemudian disimpan dan untuk memasukkan public key jalankan perintah berikut.

{% highlight bash %}
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
{% endhighlight %}

Kemudian untuk melakukan instalasi virtual box jalankan perintah berikut.

{% highlight bash %}
sudo apt-get update
sudo apt-get install virtualbox-5.0
{% endhighlight %}

##Instalasi Vagrant

Untuk melakukan instalasi vagrant silahkan download vagrant terlebih dahulu di [vagrant download](https://www.vagrantup.com/downloads.html), disini penulis menggunakan ubuntu 14.04 64 bit maka penulis memilih versi debian 64 bit. Untuk melakukan instalasinya silahkan jalankan perintah.

{% highlight bash %}
sudo dpkg -i vagrant_1.8.1_x86_64.deb
{% endhighlight %}

jangan lupa sesuaikan dengan nama file anda. Kemudian lakukan pengecekan vagrant dengan perintah.

{% highlight bash %}
vagrant --version
{% endhighlight %}

##Instalasi Box Vagrant

Box pada vagrant berfungsi sebagai virtual machine yang memuat sistem operasi dan disana juga terdapat seluruh konfigurasi yang kita lakukan. Dengan demikian maka secara tidak langsung sebenarnya kita menggunakan virtual machine maka oleh karena itu kita membutuhkan virtual box sebagai tempat virtual machine.

>>Mengapa kita menggunakan vagrant ? mengapa tidak menggunakan virtual box saja ?

Salah satu alasannya adalah ketika menggunakan vagrant maka vagrant akan melakukan semua konfigurasi hanya dengan satu perintah pada saat sistem operasi start up sedangkan jika kita menggunakan virtual box maka kita harus melakukan konfigurasi secara manual.

Repository box bisa dilihat di [repository box](https://atlas.hashicorp.com/boxes/search). Berikut adalah bentuk umum untuk melakukan instalasi box.

{% highlight bash %}
vagrant box add user/box
{% endhighlight %}

bisa dilihat bahwa untuk melakukan instalasi box sangatlah mudah yaitu kita hanya menggunakan perintah diatas dan anda hanya perlu mengganti perintah `user/box`. Perintah user berarti adalah pembuat box sedangkan perintah `box` adalah box yang ingin kita gunakan. Disini penulis menggunakan box `ubuntu/trusty64` maka jalankan perintah berikut.

{% highlight bash %}
vagrant box add ubuntu/trusty64
{% endhighlight %}

###PERHATIAN !

Untuk melakukan instalasi box membutuhkan koneksi internet dan membutuhkan instalasi yang lama dikarenakan kita akan mendownload sebuah sistem operasi yang berkisar 1 GB > tergantung dari sistem operasi yang akan digunakan.

##Membuat Project

Untuk membuat project, silahkan buat sebuah folder misalnya disini penulis membuat folder `ubuntu-vagrant`. Kemudian jalankan perintah berikut di dalam folder tersebut.

{% highlight bash %}
vagrant init
{% endhighlight %}

maka akan muncul output seperti berikut.

{% highlight bash %}
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
{% endhighlight %}

Kemudian buka file `Vagrantfile` ubah codingannya menjadi seperti berikut.

{% highlight bash %}
Vagrant.configure(2) do |config|
  
  #konfigurasi box untuk sistem operasi ubuntu trusty 64 bit
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |vb|
    
     # konfigurasi virtual box dengan ram 1 GB
     vb.memory = "1024"
   end
end
{% endhighlight %}

Setelah selesai untuk menjalankan silahkan jalankan perintah berikut.

{% highlight bash %}
vagrant up
{% endhighlight %}

dan berikut adalah output pada terminal.

{% highlight bash %}
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'ubuntu/trusty64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'ubuntu/trusty64' is up to date...
==> default: Setting the name of the VM: ubuntu-vagrant_default_1451702137384_76705
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
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
    default: /vagrant => /home/rizki/Documents/vagrant/ubuntu-vagrant
{% endhighlight %}

Jika box telah jalan, langkah selanjutnya adalah login ke dalam box yang telah kita jalankan. Untuk login ke dalam box silahkan jalankan perintah berikut.

{% highlight bash %}
vagrant ssh
{% endhighlight %}

Jika berhasil maka akan muncul output pada terminal seperti berikut.

{% highlight bash %}
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-74-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

 System information disabled due to load higher than 1.0

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.
{% endhighlight %}

dan secara otomatis terminal yang kita gunakan akan berubah menjadi seperti gambar berikut.

![Screenshot from 2016-01-02 09:41:17.png](../images/Screenshot from 2016-01-02 09:41:17.png)

##Perintah Vagrant

Di dalam vagrant terdapat beberapa perintah penting, berikut adalah beberapa perintah yang sering digunakan di dalam vagrant.

###Destroy

Perintah ini biasanya digunakan untuk menghapus project dan konfigurasi box yang telah kita buat. Untuk menghapus project kita dapat masuk ke dalam folder project dan menjalankan perintah.

{% highlight bash %}
vagrant destroy
{% endhighlight %}

###Halt

Perintah ini berfungsi untuk mematikan/shut down box sehingga konfigurasi yang telah kita kita buat di dalam box tidak akan hilang. Untuk mematikan box kita dapat masuk ke dalam folder project dan menjalankan perintah.

{% highlight bash %}
vagrant halt
{% endhighlight %}

###Suspend

Perintah ini biasanya kita gunakan untuk suspend atau sleep box. Sama seperti halt, semua konfigurasi yang telah kita buat tidak akan hilang. Untuk melakukan suspend pada project, kita dapat masuk ke dalam folder project dan menjalankan perintah.

{% highlight bash %}
vagrant suspend
{% endhighlight %}

##Provisioning

Provisioning adalah menginstall/mengkonfigurasi sistem yang terdapat di dalam sebuah box. Biasanya yang kita lakukan sehari - hari adalah melakukan instalasi apache, mysql dan sebagainya, akan tetapi hal - hal seperti ini akan merepotkan ketika kita diharuskan melakukan instalasi terus - menerus ketika kita melakukan `vagrant up` pada PC yang berbeda. Untuk mengoptimalkannya maka kita akan menggunakan fitur provisioning pada vagrant. Silahkan buat sebuah file `install.sh` di dalam folder `ubuntu-vagrant` kemudian isi dengan kodingan berikut.

{% highlight bash %}
#!/bin/sh

echo "mulai Provisioning"

echo "software source dan repository"
cp /vagrant/config/sources.list /etc/apt/sources.list
cp /vagrant/config/environment /etc/environment

echo "tambah repository nginx"
add-apt-repository -y ppa:nginx/stable

apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y

echo "Instalasi nginx"
apt-get install -y nginx
{% endhighlight %}

silahkan buat folder config, kemudian buat file `sources.list` di dalam folder config. Isikan codingan berikut ini.

{% highlight bash %}
deb http://kambing.ui.ac.id/ubuntu trusty main universe multiverse
deb http://kambing.ui.ac.id/ubuntu trusty-updates main universe multiverse
deb http://security.ubuntu.com/ubuntu trusty-security main universe multiverse
{% endhighlight %}

Langkah selanjutnya silahkan buat file `environment` di dalam folder config kemudian masukkan codingan berikut ini.

{% highlight bash %}
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
LC_CTYPE="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
{% endhighlight %}

Setelah selesai, langkah selanjutnya adalah lakukan konfigurasi pada file `Vagrantfile` seperti berikut.

{% highlight bash %}
Vagrant.configure(2) do |config|
  #konfigurasi box untuk sistem operasi ubuntu trusty 64 bit
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |vb|
    
     # konfigurasi virtual box dengan ram 1 GB
     vb.memory = "1024"
  end

  #konfigurasi provisioning
  config.vm.provision "shell", path: "install.sh"
end
{% endhighlight %}

Untuk menjalankan provisioning silahkan jalankan perintah berikut.

{% highlight bash %}
vagrant provision
{% endhighlight %}

##Konfigurasi Jaringan

Agar kita dapat melakukan akses terhadap jaringan yang terdapat di dalam box vagrant maka kita harus melakukan konfigurasi jaringan. Konfigurasi yang akan kita lakukan adalah port forwarding artinya jaringan yang ada di dalam box akan kita forward ke komputer local berdasarkan port. Misalnya kita ingin melakukan akses port 80 pada vagrant box dengan menggunakan port 8080 pada komputer local. Silahkan buka file `Vagrantfile` kemudian lakukan konfigurasi seperti berikut ini.

{% highlight bash %}
Vagrant.configure(2) do |config|
  #konfigurasi box untuk sistem operasi ubuntu trusty 64 bit
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |vb|
    
     # konfigurasi virtual box dengan ram 1 GB
     vb.memory = "1024"
  end

  #konfigurasi provisioning
  config.vm.provision "shell", path: "install.sh"
  
  #konfigurasi network
  #port forwarding
  config.vm.network "forwarded_port", guest: 80, host: 8080
end
{% endhighlight %}

Kemudian silahkan akses `http://127.0.0.1:8080/` pada browser anda. Berikut adalah outputnya.

![](../images/Screenshot from 2016-01-02 11:35:34.png)

Untuk source code nya silahkan lihat di [Belajar Vagrant](https://github.com/RizkiMufrizal/Belajar-Vagrant). Sekian artikel mengenai belajar vagrant dan terima kasih :).