---
layout: post
title: Belajar Ionic
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-09-25T06:49:11+07:00
---

Beberapa tahun kebelakang, android merupakan sebuah teknologi yang sangat pesat berkembang. Seiring berkembanganya teknologi, para developer mulai banyak mengembangkan aplikasi - aplikasi berbasis android baik secara native maupun hybrid. Kali ini kita akan mencoba membuat sebuah aplikasi android dengan menggunakan teknologi ionic.

>Ionic merupakan sebuah framework untuk membuat aplikasi mobile hybrid dengan teknologi web.

Salah satu kelebihan ionic adalah mobile hybrid artinya kita hanya perlu melakukan 1 kali coding, dan hasil coding tersebut dapat kita build menjadi aplikasi Android dan IOS.

Berikut merupakan tahapan yang akan kita lakukan untuk membuat project dengan menggunakan ionic.

- Instalasi Android
- Instalasi Ionic
- Membuat Sebuah Aplikasi Android
- Membuat File APK
- Instalasi File APK Ke Device Android

##Instalasi Android

Bagi yang belum melakukan instalasi java, silahkan dilihan di [instalasi perlengkapan coding java](http://rizkimufrizal.github.io/instalasi-perlengkapan-coding-java/) Silahkan anda download [android studio](https://developer.android.com/sdk/index.html). Kemudian extract dan install sdk android versi 5.1.1 atau API 22. Setelah selesai, kita akan mulai melakukan path android. Jalankan perintah berkut.

`sudo gedit /etc/environment`

kemudian masukkan variabel berikut pada bagian atas

`ANDROID_HOME=/home/rizki/Android/Sdk`

Sesuaikan dengan directory anda. Kemudian tambahkan pada bagian PATH seperti ini

{% highlight bash %}
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/home/rizki/Android/Sdk/platform-tools:/home/rizki/Android/Sdk/tools"
{% endhighlight %}

Silahkan restart komputer anda agar variabel tersebut dapat terbaca di terminal anda.

##Instalasi Ionic

Untuk melakukan instalasi ionic, diwajibkan untuk melakukan instalasi node js, bagi yang belum melakukan instalasi node js silahkan lihat di [instalasi perlengkapan coding node JS](http://rizkimufrizal.github.io/instalasi-perlengkapan-coding-node-js/). Kemudian jalankan perintah berikut.

`npm install -g cordova ionic`

tunggu hingga instalasi selesai.

##Membuat Sebuah Aplikasi Android

Setelah selesai instalasi ionic, sekarang saatnya coding :D. sebelumnya berikut adalah syarat untuk coding ionic.

- Mengerti HTML khususnya html5
- Mengerti Angular JS, ini yang wajib anda kuasai untuk membuat aplikasi dengan ionic

Jalankan perintah berikut untuk membuat aplikasi android dengan ionic.

`ionic start Belajar-Ionic blank`

artinya kita ingin membuat sebuah aplikasi dengan nama `Belajar-Ionic` dengan menggunakan template 'blank'. Setelah selesai, kemudian masuk ke root folder project dengan menggunakan terminal lalu jalankan perintah berikut.

`ionic platform add android`

perintah diatas berfungsi untuk mendeklarasikan bahwa kita ingin membuat sebuah aplikasi mobile untuk android. Tunggu hingga hasil download selesai. Setelah selesai, kita coba jalankan dengan perintah.

`ionic run`

Berikut adalah hasilnya.
![Screenshot from 2015-09-24 23:02:16.png](../images/Screenshot from 2015-09-24 23:02:16.png)

sedangkan jika kita ingin melakukan uji coba terlebih dahulu, kita dapat melakukan akses aplikasi melalui browser dengan perintah.

`ionic serve`

lalu akses pada `http://localhost:8100/`. Berikut adalah tampilan pada web nya.
![Screenshot from 2015-09-24 23:35:09.png](../images/Screenshot from 2015-09-24 23:35:09.png)

##Membuat File APK

Tidak seperti android studio, untuk membuat file APK, kita akan menggunakan terminal :D dikarenakan ionic belum memiliki IDE seperti halnya kita melakukan coding android secara native. Masuk ke root folder project lalu jalankan perintah berikut.

`cordova build --release android`

Tunggu sejenak karena gradle akan melakukan build terhadap aplikasi kita, dan nantinya akan muncul file apk dengan nama `android-release-unsigned.apk` pada folder `platforms/android/build/outputs/apk/`. File apk ini masih belum bisa diinstall di hp dikarenakan belum adanya keystore. Langkah selanjutnya adalah masuk ke folder `platforms/android/build/outputs/apk/` dengan terminal lalu jalankan perintah berikut.

{% highlight bash %}
keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000
{% endhighlight %}

Masukkan password dan data lainnya. Jika berhasil maka di dalam folder tersebut akan digenerate sebuah keystore dengan nama `my-release-key.keystore`. Kemudian kita harus melakukan sign terhadap file apk agar dapat diinstall pada device android. File apk ini di sign dengan menggunakan keystore yang telah kita buat tadi dengan perintah.

{% highlight bash %}
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.keystore android-release-unsigned.apk alias_name
{% endhighlight %}

Tahap selanjutnya adalah silahkan anda download [zipaligner](https://github.com/RizkiMufrizal/zipaligner). Zipaligner berfungsi untuk membuat file apk yang telah release. Extract kemudian copy semua file zipaligner ke folder `platforms/android/build/outputs/apk/` kemudian beri hak akses dengan sintak `chmoa a+x zipalign` kemudian untuk membuat apk jalankan perintah berikut.

{% highlight bash %}
./zipalign -v 4 android-release-unsigned.apk Belajar-Ionic.apk
{% endhighlight %}

Jika berhasil maka terdapat file `Belajar-Ionic.apk` di dalam folder `platforms/android/build/outputs/apk/`.

##Instalasi File APK Ke Device Android

###Install Ke Emulator Android
Agar dapat melakukan instalasi pada emulator, silahkan jalankan terlebih dahulu emulator anda kemudian masuk ke folder `platforms/android/build/outputs/apk/` lalu install file apk tersebut dengan perintah.

`adb install Belajar-Ionic.apk`

Jika berhasil maka hasilnya seperti ini
![Screenshot from 2015-09-24 23:28:06.png](../images/Screenshot from 2015-09-24 23:28:06.png)

Kemudian cari aplikasi `Belajar-Ionic` pada menu emulator android anda.

###Install Ke Device Android
Silahkan sambungkan HP anda dengan menggunakan kabel data ke komputer anda. Kemudian aktifkan debungging USB pada menu opsi pengembangan. Kemudian lakukan pengecekan device dengan perintah.

`adb devices`

maka jika device ada akan muncul berdasarkan device anda, berikut contoh device saya.
![Screenshot from 2015-09-24 23:31:33.png](../images/Screenshot from 2015-09-24 23:31:33.png)

Kemudian install ke device dengan perintah

`adb install Belajar-Ionic.apk`

Sekian tutorial tentang Belajar Ionic, semoga bermanfaat dan terima kasih :).
