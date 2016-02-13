---
layout: post
title: Berkenalan Dengan Gulp
modified:
categories: 
description: belajar dan berkenalan dengan gulp
tags: [gulp, gulp html, gulp css, gulp js]
image:
  background: abstract-3.png
comments: true
share: true
date: 2015-12-27T11:30:20+07:00
---

Jika anda adalah seorang developert javascript terutama yang udah sering ngoding di node js pasti udah tidak asing lagi dengan yang namanya [gulp](http://gulpjs.com/) :D.

>>[Gulp](http://gulpjs.com/) adalah sebuah build tool yang biasanya digunakan untuk automated task atau bisa dibilang sebagai automator.

Biasanya gulp digunakan untuk melakukan serangkaian kerja seperti membuat minify terhadap file html, css, js, melakukan concat file js, dan sebagainya. Pada artikel kali ini, penulis ingin membahas bagaimana cara penggunaan gulp untuk setup development sebuah project :D.

##Instalasi Gulp

Karena gulp basic nya menggunakan node js maka anda diharuskan melakukan instalasi node js, yang belum melakukan instalasi node js silahkan lihat di [Instalasi Perlengkapan Coding NodeJS](http://rizkimufrizal.github.io/instalasi-perlengkapan-coding-node-js/).

Untuk melakukan instalasi gulp silahkan jalankan perintah

{% highlight bash %}
npm install -g gulp
{% endhighlight %}

##Membuat Project

Silahkan buat sebuah folder dimana saja, disini penulis membuat sebuah folder dengan nama `Belajar-Gulp`. Kemudian masuk ke folder tersebut dengan menggunakan terminal. Jalankan perintah berikut ini.

{% highlight bash %}
npm init
{% endhighlight %}

Kemudian isikan perintahnya sesuai dengan kebutuhan, jika sudah maka akan terbentuk file `package.json` dimana file ini nantinya akan berisi library yang kita butuhkan. Kemudian install library gulp untuk kebutuhan project dengan perintah.

{% highlight bash %}
npm install gulp --save-dev
{% endhighlight %}

Setelah selesai, tahap selanjutnya adalah kita menentukan list task yang akan kita lakukan. List task yang akan penulis lakukan adalah sebagai berikut.

* Task Minify file css, js dan html
* Task Server
* Task Watch
* Task LiveReload
* Task Clean dan Build

##Task Minify file css, js dan html

Silahkan anda membuat sebuah file di dalam folder tersebut dengan nama `gulpfile.js`. kemudian kita lakukan instalasi library gulp dengan perintah.

{% highlight bash %}
npm install gulp-htmlmin gulp-uglify gulp-minify-css gulp-concat --save-dev
{% endhighlight %}

###Minifiy CSS

Sama seperti sebelumnya, hanya saja pada minify kita memasukkan plugin minify css.

{% highlight js %}
var gulp = require('gulp');
var gulpMinifyCss = require('gulp-minify-css');

gulp.task('minify-css', function() {
  gulp.src('./src/index.css')
    .pipe(gulpMinifyCss({
      compatibility: 'ie8'
    }))
    .pipe(gulp.dest('./dist/'));
});
{% endhighlight %}

kemudian jalankan dengan perintah.

{% highlight bash %}
gulp minify-css
{% endhighlight %}

###Minifiy JS

Sebelum diminify maka kita lakukan concat terlebih dahulu, berikut adalah codingan untuk concat dan minify

{% highlight js %}
var gulp = require('gulp');
var gulpConcat = require('gulp-concat');
var gulpUglify = require('gulp-uglify');

gulp.task('minify-js', function() {
  gulp
    .src([
      './src/index1.js',
      './src/index2.js'
    ])
    .pipe(gulpConcat('bundle.js'))
    .pipe(gulpUglify())
    .pipe(gulp.dest('dist'));
});
{% endhighlight %}

kemudian jalankan dengan perintah.

{% highlight bash %}
gulp minify-js
{% endhighlight %}

###Minifiy HTML

Yang terakhir adalah membuat agar file html menjadi lebih kecil yaitu dengan menggunakan plugin `gulp-htmlmin`, berikut adalah kodingannya.

{% highlight js %}
var gulp = require('gulp');
var gulpHtmlmin = require('gulp-htmlmin');

gulp.task('minify-html', function() {
  gulp.src('src/*.html')
    .pipe(gulpHtmlmin({
      collapseWhitespace: true
    }))
    .pipe(gulp.dest('dist'))
});
{% endhighlight %}

kemudian jalankan dengan perintah.

{% highlight bash %}
gulp minify-html
{% endhighlight %}

##Task Server

Gulp juga menyediakan plugin untuk membuat sebuah server sehingga kita bisa langsung mengubah kodingan html, css dan js tanpa server back end. Silahkan jalankan perintah berikut untuk instalasi librarynya.

{% highlight bash %}
npm install --save-dev gulp-connect
{% endhighlight %}

kemudian tambahkan kodingan berikut pada file `gulpfile.js`

{% highlight js %}
var gulp = require('gulp');
var gulpConnect = require('gulp-connect');

gulp.task('server', function() {
  gulpConnect.server({
    root: 'src',
    livereload: true
  });
});
{% endhighlight %}

Codingan `root` berfungsi sebagai tempat folder aplikasi yang akan kita jalankan sedangkan livereload berfungsi agar server dapat melakukan hot reload. Silahkan jalankan dengan perintah.

{% highlight bash %}
gulp server
{% endhighlight %}

maka akan muncul log terminal seperti berikut.

{% highlight bash %}
[13:12:05] Using gulpfile ~/Documents/Github Page/Belajar-Gulp/gulpfile.js
[13:12:05] Starting 'server'...
[13:12:05] Finished 'server' after 11 ms
[13:12:05] Server started http://localhost:8080
[13:12:05] LiveReload started on port 35729
{% endhighlight %}

silahkan akses di browser pada `http://localhost:8080`.

##Task Watch

Task yang satu ini berfungsi untuk melihat semua aktifitas yang anda kerjakan :D. Fungsi yaitu setiap kali kita melakukan edit sebuah file maka dia akan melakukan task yang lain, misalnya jika kita mengedit file css, js dan html maka dia akan concat dan minify file - file tersebut. Masukkan codingan berikut pada file `gulpfile.js`.

{% highlight js %}
gulp.task('watch', function() {
  gulp.watch('./src/*.js', ['minify-js']);
  gulp.watch('./src/*.css', ['minify-css']);
  gulp.watch('./src/*.html', ['minify-html']);
});
{% endhighlight %}

kemudian jalankan dengan perintah

{% highlight bash %}
gulp watch
{% endhighlight %}

jika anda melakukan perubahan pada file js, css dan html maka dia akan melakukan compile ulang terhadap masing - masing file tersebut.

##Task LiveReload

Task ini biasanya digunakan ketika kita ingin browser melakukan refresh setiap kali kita mengedit file - file css, js dan html. Silahkan ubah kodingan berikut ini.

{% highlight js %}
gulp.task('minify-css', function() {
  gulp.src('./src/index.css')
    .pipe(gulpMinifyCss({
      compatibility: 'ie8'
    }))
    .pipe(gulp.dest('./dist/'))
    .pipe(gulpConnect.reload());
});

gulp.task('minify-js', function() {
  gulp
    .src([
      './src/index1.js',
      './src/index2.js'
    ])
    .pipe(gulpConcat('bundle.js'))
    .pipe(gulpUglify())
    .pipe(gulp.dest('dist'))
    .pipe(gulpConnect.reload());
});

gulp.task('minify-html', function() {
  gulp.src('src/*.html')
    .pipe(gulpHtmlmin({
      collapseWhitespace: true
    }))
    .pipe(gulp.dest('dist'))
    .pipe(gulpConnect.reload());
});

gulp.task('server', function() {
  gulpConnect.server({
    root: 'src',
    livereload: true
  });
});

gulp.task('watch', function() {
  gulp.watch('./src/*.js', ['minify-js']);
  gulp.watch('./src/*.css', ['minify-css']);
  gulp.watch('./src/*.html', ['minify-html']);
});

gulp.task('default', ['watch', 'server']);
{% endhighlight %}

kemudian jalankan dengan perintah

{% highlight bash %}
gulp
{% endhighlight %}

mengapa gulp aja ? dikarenakan kita telah melakukan deklarasi default sehingga gulp akan menjalankan perintah default tersebut. Kemudian akses `http://localhost:8080/` pada browser, ubah salah satu file maka browser akan otomatis melakukan reload.

##Task Clean dan Build

Yang terakhir adalah task clean dan build, biasanya kita gunakan jika kita ingin membuild semua file yang ada. Silahkan instal library seperti berikut.

{% highlight bash %}
npm install --save-dev gulp-clean gulp-sequence
{% endhighlight %}

kemudian masukkan kodingan seperti berikut.

{% highlight js %}
var clean = require('gulp-clean');
var gulpSequence = require('gulp-sequence');

gulp.task('clean', function() {
  return gulp.src('dist', {
    read: false
  })
    .pipe(clean());
});

gulp.task('build', gulpSequence('clean', 'minify-css', 'minify-js', 'minify-html'));
{% endhighlight %}

untuk menjalankannya dengan perintah

{% highlight bash %}
gulp build
{% endhighlight %}

Untuk source code nya silahkan lihat di [Belajar Gulp](https://github.com/RizkiMufrizal/Belajar-Gulp). Sekian artikel mengenai berkenalan dengan gulp dan terima kasih :).