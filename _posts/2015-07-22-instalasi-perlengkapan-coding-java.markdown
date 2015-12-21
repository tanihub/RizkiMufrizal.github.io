---
layout: post
title: Instalasi Perlengkapan Coding Java
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-07-22T21:08:21+07:00
---

Untuk melakukan coding java, ada beberapa hal yang harus kita lakukan dintaranya adalah

- Instalasi JDK (Java Development Kit)
- Instalasi build tool java
- Instalasi IDE java

Pada tutorial ini, penulis hanya menjelaskan bagaimana instalasi pada sistem operasi linux, penulis mengunakan salah distro linux yaitu ubuntu.

##Instalasi JDK
Untuk melakukan instalasi pada ubuntu, tambahkan repository PPA berikut dengan menggunakan terminal

`sudo add-apt-repository ppa:webupd8team/java`

lalu lakukan update

`sudo apt-get update`

jika update telah selesai, maka lakukan instalasi dengan perintah

`sudo apt-get install oracle-java8-installer`

##Instalasi build tool java

Di dalam bahasa pemrograman java ada beberapa build tool diantaranya adalah

- [Maven](http://maven.apache.org/)
- [Gradle](https://gradle.org/)
- [Ant](http://ant.apache.org/)
- [Sbt](http://www.scala-sbt.org/)

Pada tutorial kali ini, kita hanya menggunakan maven sebagai build toolnya, silahkan anda download di [Maven](http://maven.apache.org/) dan ekstrak pada sebuah folder.

Instalasi JDK dan build tool telah dilakukan, tahap selanjutnya adalah menambahkan environment variabel sehingga variabel tersebut terbaca pada saat kita menggunakan terminal. buka file `environment` dengan perintah berikut

`sudo gedit /etc/environment`

kemudian sisipkan di baris paling atas

{% highlight bash %}
JAVA_HOME=/usr/lib/jvm/java-8-oracle/
M2_HOME=/home/rizki/programming/build-tool/apache-maven
{% endhighlight %}

ganti isi dari JAVA_HOME dan M2_HOME sesuai dengan folder anda, kemudian pada bagian `PATH` tambahkan dan jangan lupa sesuaikan dengan folder anda.

{% highlight bash %}
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/java-8-oracle/bin:/home/rizki/programming/build-tool/apache-maven/bin"
{% endhighlight %}

kemudian lakukan pengujian dengan menjalankan beberapa perintah berikut

{% highlight bash %}
java -version
mvn -version
{% endhighlight %}

##Instalasi IDE

Terdapat beberapa IDE yang sering digunakan oleh developer java diantaranya adalah

- [NetBeans](https://netbeans.org/)
- [Eclipse](http://www.eclipse.org/)
- [IntelliJ IDEA](https://www.jetbrains.com/idea/)

###Instalasi NetBeans

Download NetBeans pada [NetBeans](https://netbeans.org/), lalu beri akses eksekusi dengan perintah `chmod a+x netbeans.sh`. jalankan dengan perintah `./netbeans` maka akan muncul GUI instalasi netbeans.

###Instalasi Eclipse

Silahkan download pada [Eclipse](http://www.eclipse.org/), ekstrak folder tersebut lalu beri akses eksekusi dengan perintah `chmod a+x eclipse.sh`. dan jalankan eclipse dengan perintah `./eclipse`. Untuk memudahkan, maka buatlah shortcut untuk IDE tersebut.

###Instalasi IntelliJ IDEA

Download IDE tersebut pada [IntelliJ IDEA](https://www.jetbrains.com/idea/), lalu ekstrak pada folder tertentu. Di dalam folder tersebut terdapat folder bin yang di dalamnya terdapat file `idea.sh`, beri akses eksekusi dengan perintah `chmod a+x idea.sh` lalu jalankan file tersebut dengan perintah `./idea.sh` Secara otomatis IDE tersebut akan membuat shortcut pada linux anda.

Sekian tutorial kali ini dan selamat coding java. Terima kasih :).
