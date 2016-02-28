---
layout: post
title: Instalasi Perlengkapan Coding Kotlin
modified:
categories: 
description: instalasi perlengkapan coding kotlin
tags: [kotlin, bahasa pemrograman kotlin, instalasi perlengkapan kotlin, coding kotlin]
image:
  background: abstract-3.png
comments: true
share: true
date: 2016-02-28T16:41:12+07:00
---

## Apa itu Kotlin ?

>>[Kotlin](https://kotlinlang.org) adalah bahasa pemrograman pragmatis untuk JVM dan Android yang mengkombinasikan Object Oriented (OO) dan fitur fungsional dan fokus pada interoperabilitas, keamanan, kejelasan dan dukungan integrasi dengan berbagai tools major.

Kotlin dikembangankan oleh perusahaan jetbrains sejak tahun 2010 dan diumumkan pada bulan juli 2011. Kotlin telah di open source kan dan direlease diatas lisensi apache 2.

Mengapa disebut pemrograman pragmatis ? dikarenakan kotlin fokus pada interoperabilitas yaitu pengabungan antara project java dan kotlin. Dengan demikian, developer java dapat melakukan coding secara berbarengan dengan kotlin atau dengan kata lain di dalam sebuah project java bisa saja terdapat source code kotlin atau sebaliknya. Kotlin juga terintergrasi oleh tool - tool java seperti IntelliJ IDEA, Eclipse, maven, ant dan gradle.

## Instalasi Kotlin

Untuk melakukan instalasi kotlin, kita akan menggunakan [SDKMAN](http://sdkman.io/) (Software Development Kit Manager). Silahkan jalankan perintah berikut untuk instalasi SDKMAN.

{% highlight bash %}
curl -s get.sdkman.io | bash
{% endhighlight %}

Setelah selesai, kita ingin agar variabel sdkman terbaca oleh terminal, silahkan jalankan perintah berikut.

{% highlight bash %}
source "$HOME/.sdkman/bin/sdkman-init.sh"
{% endhighlight %}

Untuk melakukan pengecekan versio SDKMAN, silahkan jalankan perintah berikut.

{% highlight bash %}
sdk version
{% endhighlight %}

maka akan muncul output seperti berikut.

{% highlight bash %}
SDKMAN 3.3.2
{% endhighlight %}

Tahap selanjutnya kita akan melakukan instalasi kotlin, instalasi kotlin akan dilakukan oleh SDKMAN, silahkan jalankan perintah berikut.

{% highlight bash %}
sdk install kotlin
{% endhighlight %}

Kotlin yang diinstall adalah kotlin versi terbaru,SDKMAN tidak hanya untuk instalasi kotlin akan tetapi SDKMAN juga dapat melakukan instalasi groovy, grails dan gradle.

## Latihan Kotlin

Setelah selesai, kita akan coba latihan untuk belajar bahasa pemrograman kotlin. Silahkan gunakan editor anda, disini penulis menggunakan editor vim. Silahkan jalankan perintah berikut untuk memulai coding kotlin.

{% highlight bash %}
vim Belajar.kt
{% endhighlight %}

kemudian isikan codingan seperti berikut.

{% highlight bash %}
fun main(args: Array<String>) {
    println(hello("rizki"))
}

fun hello(nama: String): String {
    return "hello " + nama
}
{% endhighlight %}

berikut adalah penjelasan dari codingan diatas.

* `fun main` berfungsi sebagai method main
* `println` berfungsi untuk mencetak sama seperti `system.in` di dalam java
* `fun hello` adalah method dengan nama hello dengan type data string

Bisa dilihat contoh source code kotlin diatas lebih sederhana dan sangatlah mirip dengan java. Untuk melakukan compile, silahkan jalankan perintah berikut.

{% highlight bash %}
kotlinc Belajar.kt -include-runtime -d Belajar.jar
{% endhighlight %}

Arti dari perintah diatas adalah :

* kotlinc Belajar.kt : lakukan compile terhadap file `Belajar.kt`
* -include-runtime : jika kita membutuhkan runtime kotlin
* -d Belajar : hasil compile nya akan berupa file jar yang siap dijalankan

Untuk menjalankannya silahkan lakukan perintah berikut.

{% highlight bash %}
java -jar Belajar.jar
{% endhighlight %}

## Latihan Kotlin Dengan Maven

Jika awalnya kita hanya melakukan compile biasa, sekarang kita akan coba membuat sebuah project dengan maven akan tetapi menggunakan bahasa pemrograman kotlin. Untuk membuat project dengan maven, silahkan jalankan perintah berikut.

{% highlight bash %}
mvn archetype:generate \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DgroupId=com.rizki.mufrizal.belajarKotlin \
-DartifactId=Belajar-Kotlin
{% endhighlight %}

Silahkan hapus folder `src` kemudian jalankan perintah berikut untuk membuat folder untuk project kotlin.

{% highlight bash %}
mkdir -p src/main/kotlin/com/rizki/mufrizal/belajarKotlin
mkdir -p src/test/kotlin/com/rizki/mufrizal/belajarKotlin
{% endhighlight %}

Setelah selesai, lakukan konfigurasi pom.xml seperti berikut.

{% highlight xml %}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.rizki.mufrizal.belajarKotlin</groupId>
    <artifactId>Belajar-Kotlin</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>Belajar-Kotlin</name>
    <url>http://maven.apache.org</url>

    <properties>
        <kotlin.version>1.0.0</kotlin.version>
        <junit.version>4.12</junit.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-stdlib</artifactId>
            <version>${kotlin.version}</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <sourceDirectory>${project.basedir}/src/main/kotlin</sourceDirectory>
        <testSourceDirectory>${project.basedir}/src/test/kotlin</testSourceDirectory>

        <plugins>
            <plugin>
                <artifactId>kotlin-maven-plugin</artifactId>
                <groupId>org.jetbrains.kotlin</groupId>
                <version>${kotlin.version}</version>

                <configuration/>
                <executions>
                    <execution>
                        <id>compile</id>
                        <phase>compile</phase>
                        <goals>
                            <goal>compile</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>test-compile</id>
                        <phase>test-compile</phase>
                        <goals>
                            <goal>test-compile</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.2.1</version>
                <executions>
                    <execution>
                        <phase>test</phase>
                        <goals>
                            <goal>java</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <mainClass>com.rizki.mufrizal.belajarKotlin.BelajarKt</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
{% endhighlight %}

Setelah selesai, silahkan buat sebuah file `Belajar.kt` di dalam package com.rizki.mufrizal.belajarKotlin di dalam folder `src/main` Masukkan codingan seperti berikut.

{% highlight bash %}
package com.rizki.mufrizal.belajarKotlin

open class Belajar

    fun hello(nama: String): String {
        return "hello " + nama
    }

    fun main(args : Array<String>) {
        println(hello("rizki"))
    }
{% endhighlight %}

Kemudian buat file `BelajarTest.kt` di dalam package com.rizki.mufrizal.belajarKotlin di dalam folder `src/test` untuk kebutuhan testing, Masukkan codingan seperti berikut.

{% highlight bash %}
package com.rizki.mufrizal.belajarKotlin

import org.junit.Assert
import org.junit.Test

public class BelajarTest {

    @Test fun helloTest(): Unit {
        Assert.assertEquals("hello rizki", hello("rizki"))
    }

}
{% endhighlight %}

Codingan diatas berfungsi untuk melakukan testing terhadap method atau function hello, Kemudian jalankan dengan perintah berikut.

{% highlight bash %}
mvn clean package
{% endhighlight %}

Maka secara otomatis, maven akan melakukan test dan compile terhadap source kotlin, jika berhasil maka akan muncul seperti ini.

{% highlight bash %}
[INFO] Module name is Belajar-Kotlin
[INFO] 
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ Belajar-Kotlin ---
[INFO] Surefire report directory: /home/rizki/Belajar-Kotlin/target/surefire-reports

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.rizki.mufrizal.belajarKotlin.BelajarTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.065 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] 
[INFO] >>> exec-maven-plugin:1.2.1:java (default) > validate @ Belajar-Kotlin >>>
[INFO] 
[INFO] <<< exec-maven-plugin:1.2.1:java (default) < validate @ Belajar-Kotlin <<<
[INFO] 
[INFO] --- exec-maven-plugin:1.2.1:java (default) @ Belajar-Kotlin ---
hello rizki
[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ Belajar-Kotlin ---
[INFO] Building jar: /home/rizki/Belajar-Kotlin/target/Belajar-Kotlin-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 6.082 s
[INFO] Finished at: 2016-02-28T12:04:42+07:00
[INFO] Final Memory: 29M/304M
[INFO] ------------------------------------------------------------------------
{% endhighlight %}

Sekian artikel mengenai instalasi perlengkapan coding kotlin dan terima kasih :).