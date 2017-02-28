---
layout: post
title: Belajar OAuth2
modified:
categories:
description: Belajar OAuth2
tags: [Authentication, Authorization, OAuth2, Open Authorization]
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

>>OAuth2 adalah kepanjangan dari open authorization dimana OAuth2 banyak digunakan dikalangan developer sebagai proses authorization sebuah aplikasi.

Dengan menggunakan protokol ini, maka aplikasi pihak ketiga dapat mengakses data dari aplikasi yang telah kita bangun. Di dalam OAuth2 terdapat beberapa grant type diantaranya adalah :

### Grant Type Authorization Code

Pada grant type ini, kita akan menggunakan generate code yang berasal dari authorization server untuk ditukarkan dengan sebuah token, dimana token ini kita gunakan untuk mengakses sebuah resource / API.

Berikut adalah gambar flow untuk Authorization Code :

![oauth_authorization_code](../images/oauth_authorization_code.png)

Gambar diatas saya ambil dari [dokumentasi oracle](https://docs.oracle.com/cd/E39820_01/doc.11121/gateway_docs/content/oauth_flows.html). Gambar diatas menjelaskan bahwa ketika kita menggunakan OAuth2, maka kita seharusnya menggunakan 2 server, yaitu yang satu bertugas sebagai authorization server, dimana server ini yang akan memberikan hak akses kepada apilkasi pihak ketiga. Sedangkan server yang satu nya lagi sebagai resource server atau sebagai penyedia API.

Ketika ada aplikasi pihak ketiga akan mengakses API, API akan terlebih dahulu bertanya kepada authorization server, apakah token yang dikirim valid atau tidak, jika valid maka resource server akan memberikan resource yang dibutuhkan dan jika teryata tidak valid maka akan menampilkan bahwa aplikasi tersebut tidak berhak melakukan akses terhadap resource tersebut.

Pada flow ini, user nantinya akan melakukan login pada authorization server, dimana url pada login tersebut terdapat client id dan authorization type dari aplikasi yang akan akan mengakses resource tersebut. Ketika user melakukan approve, authorization server akan melakukan redirect ke aplikasi pihak ketiga dengan membawa sebuah code, dimana code ini akan digunakan oleh aplikasi pihak ketiga untuk menukarkan nya dengan access token, setelah berhasil ditukarkan maka code ini tidak dapat digunakan lagi. Setelah mendapatkan access token, aplikasi pihak ketiga dapat melakukan akses terhadap resource yang telah di approve. Authorization Code biasanya digunakan oleh aplikasi - aplikasi yang dapat menyimpan client id dan client secret secara aman, contonya seperti Java, NodeJS, Ruby, PHP, dan lain sebagainya.

### Grant Type Resource Owner Password Credentials

Pada grant type ini, kita akan menggunakan username dan password langsung dari owner nya. Grant type ini biasanya digunakan jika aplikasi yang dibangun merupakan aplikasi pribadi atau pembuat aplikasi berasal dari perusahaan yang sama, contohnya adalah ketika kita melakukan login pada aplikasi facebook.

Flow ini tidak direkomendasikan lagi dikarenakan aplikasi pihak ketiga diharuskan mengirimkan username dan password user sehingga memungkinkan aplikasi pihak ketiga dapat menggunakan akun owner nya. berikut adalah gambar flow grant type resource owner password credentials yang saya ambil dari [dokumentasi oracle](https://docs.oracle.com/cd/E39820_01/doc.11121/gateway_docs/content/oauth_flows.html).

![oauth_resource_owner_password_credentials.png](../images/oauth_resource_owner_password_credentials.png)

### Grant Type Client Credentials

Grant type selanjutnya adalah client credentials, dimana grant type ini biasanya digunakan oleh aplikasi - aplikasi yang telah menjalin kerja sama dengan suatu perusahaan. Masing - masing aplikasi pihak ketiga akan mendapatkan client id dan client secret, dengan menggunakan client id dan client secret, mereka menukarkan nya dengan access token, pada flow ini, tidak diberikan refresh token, hanya terdapat access token, sehingga apabila access token expired maka mereka diwajibkan untuk mengirim kembali client id dan client secret tersebut untuk ditukarkan dengan access token. berikut adalah gambar grant type client credentials yang saya ambil dari [dokumentasi oracle](https://docs.oracle.com/cd/E39820_01/doc.11121/gateway_docs/content/oauth_flows.html).

![oauth_client_credentials.png](../images/oauth_client_credentials.png)

### Grant Type Implicit Client

Grant type yang terakhir adalah implicit client, grant type ini biasanya digunakan untuk client yang tidak dapat menyimpan client secret dengan aman, contohnya seperti Angular JS, Vue JS, React JS dan lain sebagainya. Pada flow ini, terdapat beberapa langkah yang akan dijalankan, berikut adalah langkah - langkahnya :

* user mengakses aplikasi pihak ketiga dan melakukan login.
* aplikasi pihak ketiga akan melakukan redirect ke halaman login authorization server
* user melakukan approve
* aplikasi pihak ketiga mendaptkan code, kemudian ditukarkan dengan access token
* aplikasi pihak ketiga mengakses resource dengan access token tersebut
* resource server akan menanyakan apakah access token valid atau tidak kepada authorization server.

Pada grant type ini, biasanya authorization server tidak akan memberikan refresh token dikarenakan penyimpanan refresh token yang tidak aman. Berikut adalah gambar grant type implicit client yang saya ambil dari [dokumentasi digital ocean](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2).

![oauth2_implicit_client.png](../images/oauth2_implicit_client.png)

### Kata - Kata Asing Pada OAuth2

Pada tulisan diatas terdapat beberapa kata - kata asing, berikut adalah penjelasan nya.

* Client id : adalah id yang mewakili dari 1 client yang akan mengakses resource / API
* Client Secret : adalah pasangan dari client id, bisa disebut juga sebagai password nya
* access token : token yang digenerate oleh authorization server, token ini memiliki massa nya sehingga tidak dapat digunakan dalam jangka waktu yang lama.
* refresh token : token ini digunakan untuk meminta access token yang baru dikarenakan access token yang lama telah expired.
* resource server : server yang menyediakan data / API.
* authorization server : server yang melakukan pengecekan authentication dan authorization.

## Best Practice Implementasi OAuth2

Best practice yang saya buat pada artikel ini berasal dari flow teknologi [Spring OAuth2](https://projects.spring.io/spring-security-oauth/). Mungkin beberapa dari anda akan bertanya, mengapa menggunakan spring ? mengapa tidak menggunakan teknologi lain ?, yups menurut pengalaman saya, dari beberapa project yang pernah saya kerjakan, spring adalah salah satu framework yang sangat banyak menyediakan fitur - fitur yang dibutuhkan terutama untuk kebutuhan OAuth2 dibandingkan framework lain misalnya seperti sails js, express js dan sebagainya. Contohnya jika pada node js, saya menggunakan [oauth2orize](https://github.com/jaredhanson/oauth2orize), dimana library tersebut tidak menyediakan schema database dan tidak support untuk strategy in memory token store dan remote token service, Sehingga kita wajib membuat schema tersendiri dan harus menyesuaikan sendiri.

### Strategi OAuth2

Strategi yang biasanya saya gunakan jika menggunakan Spring OAuth2 :

* Authorization dan Resource server berada di dalam 1 war / aplikasi, contoh ini biasanya menggunakan in memory token sehingga antara Authorization dan Resource server biasanya saling bekerja sama.
* Authorization dan Resource server berada pada server yang berbeda atau dapat dikatakan bahwa mereka berbeda war / aplikasi. Untuk metode ini, kita mempunyai 2 strategi yaitu :
  - Remote token service, dimana Authorization server akan menggunakan in memory token, resource server akan melakukan pengecekan melalui protokol http apakah token valid atau invalid.
  - Sharing database, biasanya saya akan menggunakan strategi ini, mengapa demikian ? dikarenakan saya biasanya ingin agar data seperti grant type, expired token dan lain nya dapat berjalan secara dinamis sehingga kita tidak perlu ribet untuk mengubah codingan lagi. Schema OAuth2 pada Spring OAuth2 dapat anda lihat di [schema sql](https://github.com/spring-projects/spring-security-oauth/blob/master/spring-security-oauth2/src/test/resources/schema.sql) atau anda dapat menggunakan schema yang saya gunakan di [schema sql](https://github.com/RizkiMufrizal/Spring-OAuth2-Custom/blob/master/src/main/resources/db/migration/V0.0.1.20170207__Skema%20Awal.sql).

### Penggunaan Token

Setiap client biasanya bisa memiliki lebih dari 1 grant type yang telah kita jelaskan diatas. Bagaimana jika client memiliki banyak aplikasi, apakah setiap token yang digenerate akan disimpan di database ? bagaimana jika client tersebut memiliki banyak grant type ? nah ini merupakan dilema yang pernah saya temui pada saat membangun OAuth2 pada node js dikarenakan saya membuatnya mulai dari 0. Dikarenakan saya masih kurang paham mengenai OAuth2, saya mencoba membuat sebuah project OAuth2 dengan strategy sharing database pada Spring OAuth2, dan teryata flow dari Spring OAuth2 telah terstruktur, bahkan spring OAuth2 menyediakan custom token dengan menggunakan token extractor, tidak hanya custom token, kita juga dapat melakukan custom output token, misalnya kita ingin token yang digenerate adalah token JWT (Json Web Token). Berikut adalah flow yang harus anda terapkan jika anda membuat OAuth2 secara manual dengan menggunakan sharing database, flow ini berdasarkan referensi dari Spring OAuth2.

* Silahkan buat schema database seperti schema Spring OAuth2 diatas.
* Setiap grant type, hanya memiliki 1 access token dan refresh token.
* Pada saat generate token, kita akan melakukan pengecekan terlebih dahulu pada database, jika client id suatu client memiliki token maka kita akan cek apakah token tersebut telah expired atau belum, jika expired maka kita akan generate ulang token, token yang baru akan diupdate di database dan token tersebut kita berikan kepada client tersebut, jika token tidak expired maka kita tinggal mengembalikan token tersebut, jika token belum tersedia berdasarkan client id dan grant type maka kita generate token baru dan simpan ke database.
* Pada saat refresh token, kita juga akan cek terlebih dahulu, apakah token telah expired atau belum, jika expired maka token baru akan digenerate dan diupdate yang ada di database, jika tidak expired maka kita kembalikan token yang ada di database.

Berdasarkan flow diatas, dapat kita simpulkan bahwa setiap 1 client bisa mempunyai banyak grant type. Dimana setiap 1 client dengan 1 grant type hanya memiliki 1 token yang berlaku meskipun mereka memiliki banyak aplikasi, Jadi jika si client mempunyai 4 grant type, maka dia memiliki 4 token dimana token tersebut mempunya hak akses yang berbeda - beda, bisa saja yang 1 hanya memiliki hak akses read, yang satu nya hanya untuk write dan yang satu nya lagi untuk read dan write.

Bagi anda yang ingin melihat contoh source code codingan, silahkan lihat di [Spring OAuth2 Custom](https://github.com/RizkiMufrizal/Spring-OAuth2-Custom). Sekian artikel mengenai Belajar OAuth2, jika ada saran dan komentar silahkan isi dibawah dan terima kasih :)
