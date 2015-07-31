---
layout: post
title: Belajar Membuat Blog Dengan Jekyll
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-07-31T19:38:00+07:00
---

Zaman sekarang banyak banget blog yang ditawarkan dalam bantuk cms seperti blogspot, wordpress dan sebagainya. Akan tetapi ada salah satu tool yang dapat kita gunakan untuk membuat blog tanpa menggunakan cms yaitu [Jekyll](http://jekyllrb.com/).

>[Jekyll](http://jekyllrb.com/) merupakan sebuah tool yang disediakan untuk membuat sebuah blog.

Halaman blog pada jekyll dibuat dengan format markdown. Markdown adalah sebuah bentuk file yang biasanya digunakan untuk format penulisan. Markdown ditemukan oleh John Gruber. Jekyll ini menyediakan format penulisan markdown yang nantinya akan di convert ke html. Kita akan memulai membuat blog, berikut adalah tahap - tahapanya.

- Instalasi ruby, tutorial [instalasi ruby](http://rizkimufrizal.github.io/instalasi-perlengkapan-coding-ruby/) jika anda belum melakukan instalasi ruby.
- Instalasi jekyll
- Generate sebuah blog dengan jekyll
- Hosting blog ke github page

##Instalasi Jekyll

Instalasi jekyll sangat lah mudah, buka terminal atau tekan `ctrl + T` lalu jalankan perintah `gem install jekyll`. Jangan lupa hubungkan ke jaringan internet dan tunggu hingga instalasi selesai.

##Generate Blog Dengan Jekyll

Jekyll memiliki fitur scaffolding untuk mengenerate sebuah blog. Jalankan perintah `jekyll new NamaBlogAnda`, kali ini penulis membuat blog dengan nama `BelajarJekyll`, sesuaikan dengan nama blog anda. Jika penulisan jalankan maka perintahnya menjadi `jekyll new BelajarJekyll`. Masuk ke root folder lalu jalankan servernya dengan perintah `jekyll serve` lalu hit ke browser pada `http://127.0.0.1:4000/`. Berikut adalah gambar ketika server dijalankan.

![Screenshot from 2015-07-27 14:36:26.png](../images/Screenshot from 2015-07-27 14:36:26.png)

Sekilas telah berjalan dengan baik, oke mari kita custom sedikit dengan cara menambah tulisan atau mengedit tulisan tersebut. Untuk editornya bebas tapi penulis memberi saran agar menggunakan editor atom karena terdapat fitur preview markdown. Jika sudah, maka buka project blognya dengan menggunakan editor atom.

Berikut point penting untuk melakukan konfigurasi pada jekyll.

- file `_config.yml` berfungsi sebagai tempat konfigurasi nama blog, user dan sebagainya. Buka dan di dalamnya terdapat tulisan `title` dengan isian `Your awesome title`, ganti tulisan tersebut dengan `BelajarJekyll` lalu restart servernya, lihatlah perubahannya.
- folder `_posts` merupakan folder yang berisikan file markdown yang nanti akan di convert ke html. Ada beberapa template yang menggunakan bantuan octopress sehingga mempermudah pembuatan halaman markdown. Untuk template tersebut silahkan anda cari di google.
- folder `_layouts` berisi file html yang menjadi format penulisan markdown.
- folder `_includes` berisi file html yang merupakan file yang bisa di include ke page lainnya seperti header, footer dan sebagainya.
- folder `_site` meruapakan folder yang berisi hasil generate atau build blog.

Buka folder `_posts` lalu buat sebuah file dengan nama `2015-07-27-belajar-jekyll.markdown`. Biasanya filenya akan dibuat sesuai dengan tanggal kapan kita akan melakukan posting sebuah artikel. Kemudian masukkan beberapa kata seperti berikut.

{% highlight text %}

---
layout: post
title:  "Belajar Jekyll"
date:   2015-07-27 14:58:10
categories: jekyll update
---

Hello..., Saya sedang belajar membuat blog dengan jekyll.

{% endhighlight %}

Oke jalankan server lagi, lalu lihat perubahannya, akan ada 1 posting dengan judul belajar jekyll. Markdown juga menyediakan sintak highlight jika nantinya kita ingin melakukan posting dengan menggunakan sintak pemrograman. Penulis akan memberi contoh, masukkan sintak berikut ke dalam file yang tadi dibuat tepatnya dibawah tulisan `Hello..., Saya sedang belajar membuat blog dengan jekyll`. Kira - kira hasilnya seperti ini.

{% highlight text %}

---
layout: post
title:  "Belajar Jekyll"
date:   2015-07-27 14:58:10
categories: jekyll update
---

Hello..., Saya sedang belajar membuat blog dengan jekyll.
{% raw %}
{% highlight java %}

public class BelajarJekyll {
  public static void main(String[]args) {
    System.out.println("Hello..., Belajar Jekyll");
  }
}

{% endhighlight %}
{% endraw %}

{% endhighlight %}

Simpan dan langsung cek di browser maka akan tampil sintak highlight. Berikut adalah list sintak untuk highlight berbagai bahasa pemrograman.

- Cucumber ('*.feature')
- abap ('*.abap')
- ada ('*.adb', '*.ads', '*.ada')
- ahk ('*.ahk', '*.ahkl')
- apacheconf ('.htaccess', 'apache.conf', 'apache2.conf')
- applescript ('*.applescript')
- as ('*.as')
- as3 ('*.as')
- asy ('*.asy')
- bash ('*.sh', '*.ksh', '*.bash', '*.ebuild', '*.eclass')
- bat ('*.bat', '*.cmd')
- befunge ('*.befunge')
- blitzmax ('*.bmx')
- boo ('*.boo')
- brainfuck ('*.bf', '*.b')
- c ('*.c', '*.h')
- cfm ('*.cfm', '*.cfml', '*.cfc')
- cheetah ('*.tmpl', '*.spt')
- cl ('*.cl', '*.lisp', '*.el')
- clojure ('*.clj', '*.cljs')
- cmake ('*.cmake', 'CMakeLists.txt')
- coffeescript ('*.coffee')
- console ('*.sh-session')
- control ('control')
- cpp ('*.cpp', '*.hpp', '*.c++', '*.h++', '*.cc', '*.hh', '*.cxx', '*.hxx', '*.pde')
- csharp ('*.cs')
- css ('*.css')
- cython ('*.pyx', '*.pxd', '*.pxi')
- d ('*.d', '*.di')
- delphi ('*.pas')
- diff ('*.diff', '*.patch')
- dpatch ('*.dpatch', '*.darcspatch')
- duel ('*.duel', '*.jbst')
- dylan ('*.dylan', '*.dyl')
- erb ('*.erb')
- erl ('*.erl-sh')
- erlang ('*.erl', '*.hrl')
- evoque ('*.evoque')
- factor ('*.factor')
- felix ('*.flx', '*.flxh')
- fortran ('*.f', '*.f90')
- gas ('*.s', '*.S')
- genshi ('*.kid')
- glsl ('*.vert', '*.frag', '*.geo')
- gnuplot ('*.plot', '*.plt')
- go ('*.go')
- groff ('*.(1234567)', '*.man')
- haml ('*.haml')
- haskell ('*.hs')
- html ('*.html', '*.htm', '*.xhtml', '*.xslt')
- hx ('*.hx')
- hybris ('*.hy', '*.hyb')
- ini ('*.ini', '*.cfg')
- io ('*.io')
- ioke ('*.ik')
- irc ('*.weechatlog')
- jade ('*.jade')
- java ('*.java')
- js ('*.js')
- jsp ('*.jsp')
- lhs ('*.lhs')
- llvm ('*.ll')
- logtalk ('*.lgt')
- lua ('*.lua', '*.wlua')
- make ('*.mak', 'Makefile', 'makefile', 'Makefile.*', 'GNUmakefile')
- mako ('*.mao')
- maql ('*.maql')
- mason ('*.mhtml', '*.mc', '*.mi', 'autohandler', 'dhandler')
- modelica ('*.mo')
- modula2 ('*.def', '*.mod')
- moocode ('*.moo')
- mupad ('*.mu')
- mxml ('*.mxml')
- myghty ('*.myt', 'autodelegate')
- nasm ('*.asm', '*.ASM')
- newspeak ('*.ns2')
- objdump ('*.objdump')
- objectivec ('*.m')
- objectivej ('*.j')
- ocaml ('*.ml', '*.mli', '*.mll', '*.mly')
- ooc ('*.ooc')
- perl ('*.pl', '*.pm')
- php ('*.php', '*.php(345)')
- postscript ('*.ps', '*.eps')
- pot ('*.pot', '*.po')
- pov ('*.pov', '*.inc')
- prolog ('*.prolog', '*.pro', '*.pl')
- properties ('*.properties')
- protobuf ('*.proto')
- py3tb ('*.py3tb')
- pytb ('*.pytb')
- python ('*.py', '*.pyw', '*.sc', 'SConstruct', 'SConscript', '*.tac')
- rb ('*.rb', '*.rbw', 'Rakefile', '*.rake', '*.gemspec', '*.rbx', '*.duby')
- rconsole ('*.Rout')
- rebol ('*.r', '*.r3')
- redcode ('*.cw')
- rhtml ('*.rhtml')
- rst ('*.rst', '*.rest')
- sass ('*.sass')
- scala ('*.scala')
- scaml ('*.scaml')
- scheme ('*.scm')
- scss ('*.scss')
- smalltalk ('*.st')
- smarty ('*.tpl')
- sourceslist ('sources.list')
- splus ('*.S', '*.R')
- sql ('*.sql')
- sqlite3 ('*.sqlite3-console')
- squidconf ('squid.conf')
- ssp ('*.ssp')
- tcl ('*.tcl')
- tcsh ('*.tcsh', '*.csh')
- tex ('*.tex', '*.aux', '*.toc')
- text ('*.txt')
- v ('*.v', '*.sv')
- vala ('*.vala', '*.vapi')
- vbnet ('*.vb', '*.bas')
- velocity ('*.vm', '*.fhtml')
- vim ('*.vim', '.vimrc')
- xml ('*.xml', '*.xsl', '*.rss', '*.xslt', '*.xsd', '*.wsdl')
- xquery ('*.xqy', '*.xquery')
- xslt ('*.xsl', '*.xslt')
- yaml ('*.yaml', '*.yml')

cara penggunaanya adalah ganti pada bagian kata `java`.

{% highlight text %}

{% raw %}
{% highlight java %}

{% endhighlight %}
{% endraw %}

{% endhighlight %}

, misalnya kita ingin menggunakan `coffeescript` maka ganti menjadi

{% highlight text %}

{% raw %}
{% highlight coffeescript %}

{% endhighlight %}
{% endraw %}

{% endhighlight %}

lihat aturan diatas dan sesuaikan dengan ektensi masing - masing file.

##Hosting blog ke github page

Github page merupakan salah satu hosting gratis. Salah satu kelebihannya adalah kita dapat menggunakan jekyll pada github page tersebut. Bagi yang terbiasa dengan menggunakan version control git maka tidak lah susah untuk melakukan deploy ke github page, tidak seperti melakukan deploy ke open shift yang memerlukan banyak konfigurasi. Untuk melakukan deploy web, anda hanya perlu melakukan.

- Instalasi dan konfigurasi git. Bagi yang belum melakukannya dapat dilihat pada tutorial [Belajar Git](http://rizkimufrizal.github.io/belajar-git/).
- Buat repository baru dengan username akun anda seperti ini `username.github.io`, contohnya adalah `RizkiMufrizal.github.io`.
- clone repository tersebut dan copy semua file jekyll ke project
- kemudian lakukan add dengan perintah `git add --all`
- kemudian lakukan commit dengan perintah `git commit -m "Initial Website"`
- dan yang terakhir lakukan push dengan perintah `git push origin master`

Setelah selesai, anda dapat mengakses web anda pada `http://username.github.io/`. Demikian tutorial singkat untuk membuat blog dengan menggunakan jekyll dan terima kasih :).
