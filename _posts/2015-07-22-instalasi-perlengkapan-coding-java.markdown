---
layout: post
title: Instalasi Perlengkapan Coding Java
modified:
categories:
description: instalasi java pada linux
tags: [instalasi java, maven, ant, gradle, sbt, java, netbeans, eclipse, intellij idea, instalasi java di ubuntu]
image:
  background: abstract-3.png
comments: true
share: true
date: 2015-07-22T21:08:21+07:00
---

Untuk melakukan coding java, ada beberapa hal yang harus kita lakukan dintaranya adalah

- Instalasi JDK (Java Development Kit)
- Instalasi build tool java
- Instalasi IDE java

Pada tutorial ini, penulis hanya menjelaskan bagaimana instalasi pada sistem operasi linux, penulis mengunakan salah distro linux yaitu ubuntu.

## Instalasi OpenJDK

JDK dan JRE yang akan kita gunakan adalah OpenJDK. Beberapa developer sedikit kebingungan untuk menggunakan OpenJDK dikarenakan font yang digunakan pada OpenJDK masih acak - acakan. Untuk mengatasi masalah tersebut, pada artikel ini akan dibahas sedikit bagaimana cara mengatasi masalah font OpenJDK pada ubuntu. Silahkan tambahkan PPA berikut untuk kebutuhan `font infinality`.

{% highlight bash %}
sudo add-apt-repository ppa:no1wantdthisname/ppa
{% endhighlight %}

Kemudian lakukan update seperti berikut.

{% highlight bash %}
sudo apt update
{% endhighlight %}

kemudian lakukan instalasi font infinality seperti berikut.

{% highlight bash %}
sudo apt install libfreetype6 fontconfig-infinality
{% endhighlight %}

Langkah selanjutnya adalah kita akan mengganti konfigurasi font infinality yang lama dengan perintah berikut.

{% highlight bash %}
sudo rm /etc/fonts/conf.avail/52-infinality.conf
sudo ln -s /etc/fonts/infinality/infinality.conf /etc/fonts/conf.avail/52-infinality.conf
{% endhighlight %}

Kemudian tambahkan PPA untuk font fix OpenJDK seperti berikut.

{% highlight bash %}
sudo add-apt-repository ppa:no1wantdthisname/openjdk-fontfix
{% endhighlight %}

Setelah selesai, tambahkan PPA berikut untuk repository OpenJDK.

{% highlight bash %}
sudo add-apt-repository ppa:openjdk-r/ppa
{% endhighlight %}

Kemudian jalankan perintah berikut untuk instalasi OpenJDK.

{% highlight bash %}
sudo apt install openjdk-8-jdk openjdk-8-jre
{% endhighlight %}

## Instalasi build tool java

Di dalam bahasa pemrograman java ada beberapa build tool diantaranya adalah

- [Maven](http://maven.apache.org/)
- [Gradle](https://gradle.org/)
- [Ant](http://ant.apache.org/)
- [Sbt](http://www.scala-sbt.org/)

Pada tutorial kali ini, kita hanya menggunakan maven sebagai build toolnya, silahkan anda download di [Maven](http://maven.apache.org/) dan ekstrak pada sebuah folder.

Instalasi JDK dan build tool telah dilakukan, tahap selanjutnya adalah menambahkan environment variabel sehingga variabel tersebut terbaca pada saat kita menggunakan terminal. buka file `environment` dengan perintah berikut

{% highlight bash %}
sudo gedit /etc/environment
{% endhighlight %}

kemudian sisipkan di baris paling atas

{% highlight bash %}
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
M2_HOME=/home/rizki/programming/build-tool/apache-maven
{% endhighlight %}

ganti isi dari JAVA_HOME dan M2_HOME sesuai dengan folder anda, kemudian pada bagian `PATH` tambahkan dan jangan lupa sesuaikan dengan folder anda.

{% highlight bash %}
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/java-8-openjdk-amd64/bin:/home/rizki/programming/build-tool/apache-maven/bin"
{% endhighlight %}

kemudian lakukan pengujian dengan menjalankan beberapa perintah berikut

{% highlight bash %}
java -version
mvn -version
{% endhighlight %}

## Instalasi IDE

Terdapat beberapa IDE yang sering digunakan oleh developer java diantaranya adalah

- [NetBeans](https://netbeans.org/)
- [Eclipse](http://www.eclipse.org/)
- [IntelliJ IDEA](https://www.jetbrains.com/idea/)

### Instalasi NetBeans

Download NetBeans pada [NetBeans](https://netbeans.org/), lalu beri akses eksekusi dengan perintah `chmod a+x netbeans.sh`. jalankan dengan perintah `./netbeans` maka akan muncul GUI instalasi netbeans.

### Instalasi Eclipse

Silahkan download pada [Eclipse](http://www.eclipse.org/), ekstrak folder tersebut lalu beri akses eksekusi dengan perintah `chmod a+x eclipse.sh`. dan jalankan eclipse dengan perintah `./eclipse`. Untuk memudahkan, maka buatlah shortcut untuk IDE tersebut.

### Instalasi IntelliJ IDEA

Download IDE tersebut pada [IntelliJ IDEA](https://www.jetbrains.com/idea/), lalu ekstrak pada folder tertentu. Di dalam folder tersebut terdapat folder bin yang di dalamnya terdapat file `idea.sh`, beri akses eksekusi dengan perintah `chmod a+x idea.sh` lalu jalankan file tersebut dengan perintah `./idea.sh` Secara otomatis IDE tersebut akan membuat shortcut pada linux anda.

Sekian tutorial kali ini dan selamat coding java. Terima kasih :).
