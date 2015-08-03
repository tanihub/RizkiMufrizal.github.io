---
layout: post
title: Belajar Ruby on Rails
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-08-03T07:36:10+07:00
---

Tutorial kali ini akan membahas tentang salah satu framework ruby yaitu [ruby on rails](http://rubyonrails.org/) atau sering disebut dengan ROR. Banyak yang mengambil struktur project dari ruby on rails beberapa diantaranya seperti [play framework](https://www.playframework.com/) (scala/java), [Spring Roo](http://projects.spring.io/spring-roo/) (java) dan [sails](http://sailsjs.org/) (node js). Salah satu kelebihan ruby on rails adalah framework ini sudah support dengan teknologi ORM, untuk penjelasan ORM ada di tutorial [belajar hibernate](http://rizkimufrizal.github.io/belajar-hibernate/). Tidak hanya fitur ORM, akan tetapi framework ini dilengkapi dengan dokumentasi yang lengkap serta komunitas framework yang paling banyak di dalam bahasa pemrograman ruby. Baiklah lets coding ruby with ruby on rails :D.

##Instalasi Ruby On Rails

Untuk melakukan instalasi ruby on rails, kita menggunakan [gem](https://rubygems.org/). Bagi yang belum melakukan instalasi ruby, dapat dilihat pada tutorial [instalasi ruby](http://rizkimufrizal.github.io/instalasi-perlengkapan-coding-ruby/). [Gem](https://rubygems.org/) ini ibarat NPM di dalam node js. Bingung dengan NPM ? pada tutorial selanjutnya akan ada postingan tentang node js. Untuk melakukan instalasi ruby on rails pada linux jalankan perintah berikut.

{% highlight bash %}
gem install rails
{% endhighlight %}

tunggu hingga instalasi selesai, biasanya memakan waktu sedikit dikarenakan butuh banyak dependency dan jangan lupa hidupakan internet anda.

##Generate Project Ruby On Rails

Untuk membuat project Ruby On rails sangatlah gampang, jalankan perintah berikut.

{% highlight bash %}
rails new Belajar-RubyOnRails
{% endhighlight %}

Perintah diatas akan membuat sebuah project dengan nama projectnya Belajar-RubyOnRails. Setelah membuat project, selanjutnya kita mendownload dependency yang dibutuhkan oleh project kita. Masuk ke folder project atau root project lalu jalankan perintah berikut.

{% highlight bash %}
bundle install
{% endhighlight %}

untuk mengupdate library juga dapat dilakukan dengan perintah.

{% highlight bash %}
bundle update
{% endhighlight %}

Untuk menjalankan aplikasinya, jalankan perintah `rails server` maka server rails akan jalan pada port 3000. Buka browser lalu hit pada localhost:3000. Berikut gambar ketika server dijalankan.

![Screenshot from 2015-07-25 22:14:15.png](../images/Screenshot from 2015-07-25 22:14:15.png)

Oke sekilas kita telah dapat menggunakan ruby on rails, kita akan memulai dengan basic CRUD pada ror.

ror merupakan salah satu framework yang bersifat mvc, maka kita membutuhkan view, controller dan model. kita akan mulai dari controller. ror menyediakan fitur scaffolding sehingga kita dapat generate controller melalui terminal. jalankan perintah berikut.

{% highlight bash %}
rails generate controller bukus index
{% endhighlight %}

Pada tutorial ini, kita akan menggunakan contoh data buku. Oke selanjutnya adalah memasukkan library twitter bootstrap ke project, kita akan menggunakan gem, buka file `Gemfile` pada root project dan sisipkan sintak.

{% highlight bash %}
gem 'bootstrap-sass', '~> 3.3.5.1'
gem 'autoprefixer-rails'
{% endhighlight %}

Setiap tambah atau ganti gem maka jangan lupa lakukan update dengan perintah `bundle install` atau `bundle update`. Buka file `application.css` yang ada dalam folder `app/assets/stylesheets`, ganti nama file tersebut dengan `application.css.sass`. Kemudian import library bootstrap ke dalam file `application.css.sass` dengan sintak.

{% highlight sass %}
@import "bootstrap-sprockets"
@import "bootstrap"
{% endhighlight %}

Bootstrap juga punya library dalam bentuk javascript maka kita butuh import juga ke javascript. Buka file `application.js` pada folder `app/assets/javascripts` dan sisipkan.

{% highlight js %}
//= require bootstrap-sprockets
{% endhighlight %}

Untuk mengecek bootstrap maka buka file `index.html.erb` pada folder `app/views/bukus` Kemudian sisipkan sintak berikut.

{% highlight html %}
<button class="btn btn-default">cek</button>
{% endhighlight %}

Silahkan jalankan server kembali, untuk melihat urlnya ada di file `routes.rb` di dalam folder `config`. Secara default, urlnya berada pada `http://localhost:3000/bukus/index`. Jika anda ingin melihat routes semua url maka gunakan perintah `rake routes`.

Oke kita akan memulai generate model, untuk generate model jalankan perintah berikut.

{% highlight bash %}
rails generate model buku idBuku:string judulBuku:string namaPengarang:string penerbit:string tahunTerbit:integer
{% endhighlight %}

Perintah diatas akan membuat sebuah file ruby dengan nama buku. untuk idBuku, judulBuku, namaPengarang, penerbit dan tahunTerbit merupakan kolom pada basis data yang akan digenerate nantinya. Jika ingin melihat strukturnya ada pada folder `db/migrate`. Untuk membuat tabel pada basis data silahkan jalankan perintah `rake db:migrate`.

Setelah berhasil dengan model, kita lanjut untuk membuat konfigurasi controllernya. Buka file `bukus_controller.rb` yang ada di dalam folder `app/controllers`, Kemudian masukkan sintak berikut.

{% highlight ruby %}

class BukusController < ApplicationController

  def index
    @bukus = Buku.all
  end

  def show
    @buku = Buku.find(params[:id])
  end

  def new
    @buku = Buku.new
  end

  def create
    @buku = Buku.new(buku_params)

    if @buku.save
      redirect_to bukus_path
    else
      render 'new'
    end

  end

  def edit
    @buku = Buku.find(params[:id])
  end

  def update
    @buku = Buku.find(params[:id])

    if @buku.update(buku_params)
      redirect_to bukus_path
    else
      render 'edit'
    end

  end

  def destroy
    @buku = Buku.find(params[:id])
    @buku.destroy
    redirect_to bukus_path
  end

  private
    def buku_params
      params.require(:buku).permit(:idBuku, :judulBuku, :namaPengarang, :penerbit, :tahunTerbit)
    end

end

{% endhighlight %}

Penulis akan menjelaskan beberapa fungsi sintak diatas.

- fungsi dari `def` adalah untuk mendeklarasikan sebuah function atau fungsi pada ruby.
- @bukus merupakan variabel yang diisi dengan nilai data seluruh buku. Data tersebut diperoleh dari `Buku.all`.
- Pada bagian method `show` kita akan menampilkan 1 object saja. Untuk mengambil object berdasarkan id maka kita gunakan sintak `Buku.find(params[:id])`.
- Method new berfungsi untuk redirect page baru sekalian membuat sebuah object baru.
- Method create berfungsi untuk melakukan penyimpanan data pada basis data. Jika data telah tersimpan maka akan dilakukan redirect ke index page.
- Method edit sama seperti method new hanya saja, method edit mengambil berdasarkan id tanpa membuat object baru.
- Method Update sama seperti method create, berbeda pada saat melakukan pengambilan object berdasarkan id.
- Method destroy berfungsi untuk menghapus data. Dilakukan pengambilan object terlebih dahulu baru dilakukan penghapusan data.

Lumayan panjang juga, oke lanjut ke bagian view. View pada ror dapat melakukan import antar page, kita akan membuat default page terlebih dahulu, dimulai dari `_navigation.erb`.

{% highlight html %}

<nav class="navbar navbar-default">
  <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#js-navbar-collapse">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="/">
        Belajar Ruby On Rails
      </a>
    </div>

    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <li class="active"><a href="/">Home</a></li>
      </ul>
    </div>
  </div>
</nav>

{% endhighlight %}

diatas adalah codingan untuk navigator sebuah web aja. selanjutnya kita membuat sebuah form, buat sebuah file `_form.erb` lalu masukkan sintak berikut.

{% highlight erb %}

<div class="container">
  <div class="row">
    <%= form_for(@buku) do |buku| %>

        <div class="form-group">
          <%= buku.label :ID_Buku %>
          <%= buku.text_field :idBuku, class:'form-control', type: 'text', placeholder: 'ID Buku' %>
        </div>

        <div class="form-group">
          <%= buku.label :Judul_Buku %>
          <%= buku.text_field :judulBuku, class:'form-control', type: 'text', placeholder: 'Judul Buku' %>
        </div>

        <div class="form-group">
          <%= buku.label :Nama_Pengarang %>
          <%= buku.text_field :namaPengarang, class:'form-control', type: 'text', placeholder: 'Nama Pengarang' %>
        </div>

        <div class="form-group">
          <%= buku.label :Penerbit %>
          <%= buku.text_field :penerbit, class:'form-control', type: 'text', placeholder: 'Penerbit' %>
        </div>

        <div class="form-group">
          <%= buku.label :Tahun_Terbit %>
          <%= buku.text_field :tahunTerbit, class:'form-control', type: 'number', placeholder: 'Tahun Terbit' %>
        </div>

        <%= buku.submit :Save, class:'btn btn-success' %>

    <% end %>
  </div>
</div>

{% endhighlight %}

Berikut adalah penjelasan tentang codingan diatas.

- `<%= form_for(@buku) do |buku| %>` merupakan sintak ruby, sintak ini berfungsi untuk melakukan render sebuah object `@buku` yang telah di deklarasikan pada controller.
- `<%= buku.label :ID_Buku %>` berfungsi sebagai label
- `<%= buku.text_field :idBuku, class:'form-control', type: 'text', placeholder: 'ID Buku' %>` berfungsi sebagai text field yang nantinya dapat diinput oleh user.
- `<%= buku.submit :Save, class:'btn btn-success' %>` berfungsi sebagai button agar dapat disubmit.

Buatlah file `new.html.erb` dan `edit.html.erb` di dalam folder `app/views/bukus` kemudian masukkan sintak.

{% highlight erb %}

<%= render 'navigation' %>

<%= render 'form' %>

{% endhighlight %}

Codingan diatas berfungsi untuk melakukan render terhadap page navigation dan form yang telah kita buat tadi atau bahasa kerennya kita include pagenya. Oke selanjutnya kita akan membuat sebuah page detail. Sama seperti tadi buat sebuah file `show.html.erb` dan masukkan kodingan berikut.

{% highlight erb %}

<%= render 'navigation' %>

<div class="container">
  <div class="row">
    <%= form_for(@buku) do |buku| %>

        <div class="form-group">
          <%= buku.label :ID_Buku %>
          <%= buku.text_field :idBuku, class:'form-control', type: 'text', placeholder: 'ID Buku', disabled: true %>
        </div>

        <div class="form-group">
          <%= buku.label :Judul_Buku %>
          <%= buku.text_field :judulBuku, class:'form-control', type: 'text', placeholder: 'Judul Buku', disabled: true %>
        </div>

        <div class="form-group">
          <%= buku.label :Nama_Pengarang %>
          <%= buku.text_field :namaPengarang, class:'form-control', type: 'text', placeholder: 'Nama Pengarang', disabled: true %>
        </div>

        <div class="form-group">
          <%= buku.label :Penerbit %>
          <%= buku.text_field :penerbit, class:'form-control', type: 'text', placeholder: 'Penerbit', disabled: true %>
        </div>

        <div class="form-group">
          <%= buku.label :Tahun_Terbit %>
          <%= buku.text_field :tahunTerbit, class:'form-control', type: 'number', placeholder: 'Tahun Terbit', disabled: true %>
        </div>

    <% end %>
  </div>
</div>

{% endhighlight %}

Codingan tersebut sama seperti dengan form pada page new dan edit hanya berbeda pada setiap text field akan disable berikut perintah disable ada di `disabled: true`. Dan yang terakhir adalah file `index.html.erb` merupakan page awal yang akan berisi tentang data. Masukkan codingan berikut.

{% highlight erb %}

<%= render 'navigation' %>

<div class="container">
  <div class="row">
    <%= link_to 'Tambah Data', new_buku_path, class: 'btn btn-default' %>
    <p></p>
    <table class="table table-bordered table-hover table-responsive">
      <thead>
      <tr>
        <th class="text-center">Id Buku</th>
        <th class="text-center">Judul Buku</th>
        <th class="text-center">Nama Pengarang</th>
        <th class="text-center">Penerbit</th>
        <th class="text-center">Tahun Terbit</th>
        <th class="text-center">Aksi</th>
      </tr>
      </thead>
      <tbody>
      <% @bukus.each do |buku| %>
          <tr>
            <td><%= buku.idBuku %></td>
            <td><%= buku.judulBuku %></td>
            <td><%= buku.namaPengarang %></td>
            <td><%= buku.penerbit %></td>
            <td><%= buku.tahunTerbit %></td>
            <td class="text-center">
              <%= link_to 'Detail', buku_path(buku), class: 'btn btn-default' %>
              <%= link_to 'Edit', edit_buku_path(buku), class: 'btn btn-success' %>
              <%= link_to 'Delete',buku_path(buku), method: :delete, data: {confirm: 'Data Dihapus ? '}, class: 'btn btn-danger' %>
            </td>
          </tr>
      <% end %>
      </tbody>
    </table>
  </div>
</div>

{% endhighlight %}

Sedikit penjelasan dari codingan diatas.

- `<%= link_to 'Tambah Data', new_buku_path, class: 'btn btn-default' %>` berfungsi sebagai button tambah data yang akan melakukan redirect page ke new page.
- `@bukus.each do |buku|` berfungsi untuk melakukan render data pada tabel html. `@bukus` merupakan variabel yang terdapat di controller yang berisi data - data buku.
- `<%= buku.idBuku %>` berfungsi untuk menampilkan data yang berbentuk array ke dalam tabel - tabel html.
- `<%= link_to 'Detail', buku_path(buku), class: 'btn btn-default' %>` merupakan sebuah button detail untuk menampilkan detail data.
- `<%= link_to 'Edit', edit_buku_path(buku), class: 'btn btn-success' %>` merupakan sebuah button edit untuk redirect ke page edit.
- `<%= link_to 'Delete',buku_path(buku), method: :delete, data: {confirm: 'Data Dihapus ? '}, class: 'btn btn-danger' %>` meruapkan sebuah button delete untuk menghapus data. sebelum menghapus akan ada muncuk tampilan info validasi penghapusan data.

Akhirnya selesai juga, sekarang kita lakukan testing Berikut tampilan halaman awalnya.

![Screenshot from 2015-07-27 11:14:10](../images/Screenshot from 2015-07-27 11:14:10.png)

Tampilan form untuk insert data baru

![Screenshot from 2015-07-27 11:14:00.png](../images/Screenshot from 2015-07-27 11:14:00.png)

Tampilan detail data buku

![Screenshot from 2015-07-27 11:14:14.png](../images/Screenshot from 2015-07-27 11:14:14.png)

Tampilan delete data buku

![Screenshot from 2015-07-27 11:14:34.png](../images/Screenshot from 2015-07-27 11:14:34.png)

Sekian tutorial belajar Ruby On Rails dan Terima kasih :). Untuk source code lengkap, penulis publish di [Belajar Ruby On Rails](https://github.com/RizkiMufrizal/Belajar-RubyOnRails).
