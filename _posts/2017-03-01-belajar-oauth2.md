---
layout: post
title: Belajar OAuth2
modified:
categories:
description: Belajar OAuth2
tags: [Authentication, Authorization, OAuth2, Open Authentication]
image:
  background: abstract-3.png
comments: true
share: true
date: 2017-03-01T20:15:28+07:00
---

Pada artikel ini, penulis akan membahas mengenai salah satu protokol Authorization yang banyak digunakan dikalangan developer. Sebelum memasuki pembahasan mengenai OAuth2, kita akan membahas terlebih dahulu mengenai Authentication dan Authorization.

## Apa Itu Authentication ?

>>Authentication adalah suatu proses untuk menentukan identitas seseorang.

Untuk menentukan identitas seseorang kita dapat menggunakan kerahasiaan informasi seperti contoh berikut

| Kerahasiaan Informasi | Deskripsi                                |
|-----------------------|------------------------------------------|
| Password              | Hanya pengguna yang mengetahui password  |
| PIN                   | Hanya pengguna yang mengetahui PIN       |
| private key           | hanya pengguna yang memiliki private key |
{: rules="groups"}

Dari kerahasiaan informasi diatas, kita dapat menentukan identitas seseorang, misalnya ketika seseorang login pada sebuah aplikasi, jika dia menggunakan username dan password yang benar maka sudah dipastikan bahwa yang login tersebut adalah pengguna sebenarnya.

Selain kerahasiaan informasi, untuk menentukan identitas seseorang kita dapat menggunakan keunikan dari seorang pengguna, contohnya.

| Keunikan Informasi | Deskripsi                         |
|--------------------|-----------------------------------|
| Retina             | Hanya dimiliki oleh satu pengguna |
| Fingerprint        | Hanya dimiliki oleh satu pengguna |
{: rules="groups"}

Setiap pengguna pasti mempunyai keunikan informasi seperti fingerprint, dimana fingerprint antara 1 pengguna dengan pengguna lain pasti memiliki perbedaan. Perbedaan ini dapat kita lihat dari garis - garis sidik jari yang berbeda - beda.

## Apa Itu Authorization ?

>>Authorization adalah proses dimana kita akan memberikan hak akses kepada pihak ketiga untuk dapat melakukan akses data yang kita miliki.

Proses authorization akan dilakukan jika anda telah melakukan authentication, karena authentication diperlukan untuk identifikasi user yang akan memberikan hak akses datanya. Berikut adalah contoh penggunaan authorization, misalnya anda memiliki akun [google](https://www.google.co.id/), kemudian anda ingin melakukan registrasi pada layanan web lain, pada web tersebut terdapat fitur login/register dengan menggunakan akun google, maka kita dapat menggunakan fitur tersebut. Pada saat fitur tersebut kita gunakan maka nantinya google akan menanyakan apakah kita memberikan hak akses agar aplikasi tersebut dapat mengakses data kita yang terdapat pada google. Jika memberikan hak akses maka web tersebut akan mendapatkan data kita, jika tidak maka google tidak akan memberikan data anda. Proses diatas adalah salah satu contoh dari proses authorization pada aplikasi.

## Apa Itu OAuth2 ?

>>OAuth2 adalah kepanjangan dari open authentication dimana OAuth2 banyak digunakan dikalangan developer sebagai proses authorization sebuah aplikasi.

Dengan menggunakan protokol ini, maka aplikasi pihak ketiga dapat mengakses data dari aplikasi yang telah kita bangun. Di dalam OAuth2 terdapat beberapa grant type diantaranya adalah :

### Grant Type Authorization Code

Pada grant type ini, kita akan menggunakan generate code yang berasal dari authorization server untuk ditukarkan dengan sebuah token, dimana token ini kita gunakan untuk mengakses sebuah resource / API.
