---
layout: post
title: Belajar Git
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-07-28T18:14:04+07:00
---

Untuk kerja team sebuah project, kita butuh tool untuk melakukan manajemen project. Dahulu biasanya penulis menggunakan [svn](https://subversion.apache.org/) untuk version control system (VCS), kali ini kita akan menggunakan teknologi baru dari version control system (VCS) yaitu [git](https://git-scm.com/).

>Version Control System (VCS) adalah sebuah sistem yang dapat mencatat setiap perubahan dari sebuah file sedangkan [git](https://git-scm.com/) merupakan salah satu contoh version control system yang diciptakan oleh linus torvalds yang dulunya digunakan untuk memanajemen project kernel linux.

Oke kita langsung mulai melakukan konfigurasi git pada linux. Berikut tahapan yang harus diperhatikan.

- Instalasi git dan Buat akun [github](https://github.com/).
- Konfigurasi git
- Buat repository baru dan belajar commit
- Penjelasan Gitignore

##Instalasi Git dan Buat Akun Github

Tambahkan repository PPA pada linux lewat terminal.

`sudo add-apt-repository ppa:git-core/ppa`

lalu lakukan update

`sudo apt-get update`

dan terakhir install git

`sudo apt-get install git`

Instalasi selesai, lalu silahkan bagi yang belum membuat akun, anda dapat melakukan registrasi di [Github](http://github.com). Github ini merupakan sebuah sebuah media social bagi developer, disana kita dapat melakukan upload project, berbagi project dan sebagainya.

##Konfigurasi Git

Setelah tahap instalasi, selanjutnya kita akan melakukan konfigurasi git. Konfigurasi pertama yaitu melakukan setting global konfigurasi pada git. Jalankan perintah berikut.

{% highlight lua %}
git config --global user.name "RizkiMufrizal"
git config --global user.email "mufrizalrizki@gmail.com"
git config --global color.ui true
{% endhighlight %}

Jangan lupa sesuaikan dengan identitas anda. Langkah selanjutnya adalah kita akan mengenerate ssh key yang nantinya kita butuhkan untuk di upload ke github. ssh key ini menggunakan algoritma rsa sehingga dia mempunyai public dan private key. Untuk mengenerate ssh tersebut jalankan perintah berikut.

`ssh-keygen -t rsa -C "mufrizalrizki@gmail.com"`

Oke selanjutnya kita akan melakukan copy dari isi ssh tersebut untuk github. Install terlebih dahulu `xclip` dengan perintah.

`sudo apt-get install xclip`

kemudian copy file ssh dengan perintah

`xclip -sel clip < ~/.ssh/id_rsa.pub`

Login ke akun gihub, lalu pilih menu `setting` pilih `ssh key` kemudian tambahkan ssh yang tadi kita copykan.

Kemudian untuk mengecek apakah ssh nya telah berhasil maka lakukan perintah berikut.

`ssh -T git@github.com`

Jika berhasil maka akan tampil seperti dibawah ini.

{% highlight text %}
Hi Rizki Mufrizal! You've successfully authenticated, but Github does not provide shell access
{% endhighlight %}

##Buat repository baru dan belajar commit

Untuk membuat repository di gitub, silahkan anda login dan buat sebuah repository. Penulis membuat repository dengan nama `Belajar-Git` Jika sudah, anda dapat melakukan clone repository atau melakukan remote repository tersebut pada komputer anda.

###Melakukan Clone dan commit project

Untuk melakukan clone repsitory sangat gampang. Pada bagian repository yang ada di github terdapat bagian url yang dapat kita copy yaitu ada pada bagian ini.

![Screenshot from 2015-07-27 21:06:12.png](../images/Screenshot from 2015-07-27 21:06:12.png)

Lalu copy dan clone dengan perintah

`git clone https://github.com/RizkiMufrizal/Belajar-Git.git`

Sesuaikan dengan repository anda. Kemudian buat sebuah file, disini penulis membuat sebuah file dengan nama `index.md` kemudian masukkan tulisan `hello word`. Kemudian kita dapat melakukan pengecekan status git dengan perintah `git status` dan akan muncul.

{% highlight bash %}

On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	index.md

nothing added to commit but untracked files present (use "git add" to track)

{% endhighlight %}

Menandakan bahwa file tersebut belum di add dan di commit. Lakukan add file tersebut dengan perintah `git add index.md` kemudian cek status lagi maka akan muncul.

{% highlight bash %}

On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   index.md

{% endhighlight %}

Tahap selanjutnya lakukan commit dengan perintah `git commit -m "Tambah file index.md"`. Message atau pesan tersebut dapat diganti sesuia dengan keinginan anda. Kemudian upload semua file tersebut ke github dengan cara menggunakan perintah `git push origin master`, git akan meminta username dan password anda. Jika telah selesai, lihat repository github anda, dan disana telah terdapat file `index.md`.

###Melakukan Remote dan commit project

Untuk melakuakn remote sebuah project tidak jauh berbeda dengan clone sebuah project. Setelah membuat project pada github kemudian anda buat sebuah folder sesuai dengan nama project anda di github. Kemudian masuk ke root project dan lakukan inisialisasi git dengan perintah `git init`. Tahap selanjutnya anda lakukan remote dengan perintah

`git remote add origin https://github.com/RizkiMufrizal/Belajar-Git.git`

Setelah selesai, anda download dulu source code yang sudah ada di github dengan perintah 'git pull origin master' maka akan muncul file `index.md`. Untuk melakukan commit dan push sama seperti perintah sebelumnya.

##Penjelasan Gitignore

Gitignore merupakan sebuah file yang berfungsi untuk mendeklarasikan file - file apa saja yang tidak akan di commit. File gitignore biasanya ditulis dengan nama `.gitignore`, file ini bersifat hidden sehingga untuk menampilkannya, kita harus melakukan perintah `ctrl + H`. Misalnya jika kita membuat project java dengan menggunakan maven, maka akan muncul folder `target` yang berisi kompalasi program maka folder tersebut kita daftarkan pada gitignore sehingga tidak akan di commit. Untuk mengetahui file apa saja yang yang di daftarkan pada gitignore untuk setiap bahasa pemrograman, anda dapat melihatnya di [repository gitignore](https://github.com/github/gitignore). 

Sekian tutorial Belajar Git, Semoga bermanfaat dan terima kasih :).
