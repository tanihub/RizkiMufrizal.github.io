---
layout: post
title: Belajar Framework Spring
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-08-10T17:39:00+07:00
---

Setelah mempelajari sedikit tentang [framework hibernate](http://rizkimufrizal.github.io/belajar-hibernate), kali ini kita akan belajar tentang framework [spring](https://spring.io/). Apa itu spring ?

>[Spring](https://spring.io/) adalah salah satu framework untuk java enterprise. Hingga saat ini sangat banyak developer yang menggunakan framework ini dikarenakan kinerja tinggi, mudah diuji dan kode dapat digunakan kembali. Spring dibuat oleh Rod Johnson dan di di umumkan pada juni 2003 diatas lisensi apache. Spring digunakan untuk mengembangkan aplikasi java terutama dalam membangun sebuah aplikasi web diatas platfom JEE. Dan spring menerapakan kembali pemograman yang berbasis POJO (plain old java object).

[Spring](https://spring.io/) adalah salah satu framework yang menerapkan konsep DI (Depedency Injection) dimana depedency injection ini adalah salah satu penerapan dari konsep IoC (inversion of control). Banyak yang menjelaskan bahwa konsep DI dan IoC adalah dua hal yang sama. hal tersebut tidak benar, mengapa demikian ? karena IoC merupakan sebuah konsep yang paling umum dan salah satu penerapan konsep IoC dapat dengan menggunakan DI (depedency injection).

Oke kita akan mulai dengan membuat project spring. Seperti biasa, kita akan menggunakan maven sebagai build toolnya. Jalankan perintah berikut untuk membuat project.

{% highlight bash %}
mvn archetype:generate \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DgroupId=com.rizki.mufrizal.belajarSpring \
-DartifactId=Belajar-Spring
{% endhighlight %}

Import project tersebut dengan menggunakan IDE kesukaan anda, disini penulis menggunakan Intellij IDEA. Kemudian tambahkan depedency library pada file pom.xml seperti berikut.

{% highlight xml %}
<dependencies>

  <!--testing -->
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>4.2.0.RELEASE</version>
    <scope>test</scope>
  </dependency>

  <!-- spring core -->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>4.2.0.RELEASE</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.2.0.RELEASE</version>
  </dependency>

  <!-- slf4j -->
  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.12</version>
  </dependency>
  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.12</version>
  </dependency>
  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>1.7.12</version>
  </dependency>

</dependencies>

<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.2</version>
      <configuration>
        <source>1.8</source>
        <target>1.8</target>
      </configuration>
    </plugin>
  </plugins>
</build>

{% endhighlight %}

Oke setelah selesai, kita lanjut untuk melakukan coding. Berikut adalah tampilan struktur projectnya.

![Screenshot from 2015-07-28 18:23:56.png](../images/Screenshot from 2015-07-28 18:23:56.png)

Kita akan mulai dari class Sistem Operasi. Class sistem operasi merupakan sebuah class domain atau class model. Di dalam class ini akan kita deklarasikan berbagai properti yang dimiliki oleh si sistem operasi. Class sistem operasi ini akan dibuatkan bean yang nantinya akan diinject oleh spring. Masukkan codingan berikut pada class sistem operasi seperti berikut.

{% highlight java %}
public class SistemOperasi {

    private String namaSistemOperasi;
    private String versiSistemOperasi;

    //getter setter

    @Override
    public String toString() {
        return "Sistem Operasi anda adalah " + getNamaSistemOperasi() + " Dengan Versi " + getVersiSistemOperasi();
    }
}
{% endhighlight %}

Pada codingan diatas, kita mendeklarasikan dua property yaitu namaSistemOperasi dan versiSistemOperasi, property ini nantinya akan diinject melalui bean pada konfigurasi spring. Method toString berfungsi agar nanti kita dapat mengetahui apa isi dari property namaSistemOperasi dan versiSistemOperasi. Langkah selanjutnya buat sebuah file xml dengan nama `spring-config-context.xml` untuk konfigurasi spring dan berikut codingannya.

{% highlight xml %}

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="sistemOperasiUbuntu" class="com.rizki.mufrizal.belajarSpring.domain.SistemOperasi">
        <property name="namaSistemOperasi" value="Ubuntu"/>
        <property name="versiSistemOperasi" value="14.04"/>
    </bean>

    <bean id="sistemOperasiFedora" class="com.rizki.mufrizal.belajarSpring.domain.SistemOperasi">
        <property name="namaSistemOperasi" value="Fedora"/>
        <property name="versiSistemOperasi" value="fedora 22"/>
    </bean>

    <bean id="sistemOperasiElementary" class="com.rizki.mufrizal.belajarSpring.domain.SistemOperasi">
        <property name="namaSistemOperasi" value="Elementary os"/>
        <property name="versiSistemOperasi" value="Freya"/>
    </bean>

</beans>

{% endhighlight %}

konfigurasi diatas memiliki 3 bean, dan masing - masing bean mempunyai rujukan ke class Sistem Operasi, property namaSistemOperasi dan versiSistemOperasi akan diinject dengan value seperti diatas, dengan menggunakan id maka setiap bean mempunyai value yang berbeda - beda. Untuk dapat melakukan inject ini, spring menggunakan method setter yang telah kita deklarasikan pada class SistemOperasi sehingga apabila kita melakukan debug terhadap method getter maka akan muncul value dari masing - masing property. Kemudian gunakan class `App.java` yang dibuat oleh maven sebagai class main. Berikut codingan untuk main class.

{% highlight java %}

public class App {

    private static final Logger LOGGER = LoggerFactory.getLogger(App.class);

    public static void main(String[]args) {

        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-config-context.xml");

        SistemOperasi sistemOperasiUbuntu = (SistemOperasi) applicationContext.getBean("sistemOperasiUbuntu");
        SistemOperasi sistemOperasiFedora = (SistemOperasi) applicationContext.getBean("sistemOperasiFedora");
        SistemOperasi sistemOperasiElementary = (SistemOperasi) applicationContext.getBean("sistemOperasiElementary");

        LOGGER.debug("Cek Spring Pertama : {}", sistemOperasiUbuntu.toString());
        LOGGER.debug("Cek Spring Kedua   : {}", sistemOperasiFedora.toString());
        LOGGER.debug("Cek Spring Ketiga  : {}", sistemOperasiElementary.toString());

    }

}

{% endhighlight %}

Berikut penjelasan singkat mengenai kodingan diatas.

- ApplicationContext dan ClassPathXmlApplicationContext merupakan class yang bertanggung jawab untuk meload konfigurasi spring.
- SistemOperasi merupakan class domain yang telah kita buat, disini penulis membuat 3 object dari class SistemOperasi untuk melakukan inject terhadap 3 bean yang terdapat pada konfigurasi spring.
- dengan menggunakan bantuan library SLF4J maka kita lakukan debug terhadap masing - masing object.

Seperti biasa, untuk mempermudah debug aplikasi, silahkan buat sebuah file `log4j.properties` dan masukkan codingan berikut.

{% highlight properties %}

log4j.rootLogger=DEBUG, file, stdout

# Direct log messages to a log file
log4j.appender.file=org.apache.log4j.RollingFileAppender
log4j.appender.file.File=logs/LogFile.log
log4j.appender.file.MaxFileSize=5MB
log4j.appender.file.MaxBackupIndex=10
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d [%t] %p (%F:%L) - %m%n

# Direct log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d [%t] %-5p %c - %m%n

# %d untuk tanggal pada saat log dibuat
# %t untuk nama tread
# %p untuk level log
# %c untuk nama class
# %F untuk nama file source code
# %L untuk no baris
# %m%n untuk isi log nya

{% endhighlight %}

Tahap terakhir untuk memperoleh sebuah program yang baik maka kita buat sebuah class untuk testing aplikasi. Untuk belajar testing aplikasi, penulis akan membuatnya pada tutorial selanjutnya. Dengan menggunakan class `AppTest` yang dibuat oleh maven, masukkan codingan berikut agar kita dapat melakukan testing.

{% highlight java %}

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-config-context.xml")
@TestExecutionListeners({
        DependencyInjectionTestExecutionListener.class
    }
)
public class AppTest {

    @Qualifier("sistemOperasiUbuntu")
    @Autowired
    private SistemOperasi sistemOperasiUbuntu;

    @Qualifier("sistemOperasiFedora")
    @Autowired
    private SistemOperasi sistemOperasiFedora;

    @Qualifier("sistemOperasiElementary")
    @Autowired
    private SistemOperasi sistemOperasiElementary;

    private static final Logger LOGGER = LoggerFactory.getLogger(AppTest.class);

    @Test
    public void testSistemOperasiPertama() throws Exception {
        assertEquals(sistemOperasiUbuntu.getNamaSistemOperasi(), "Ubuntu");
        assertEquals(sistemOperasiUbuntu.getVersiSistemOperasi(), "14.04");
        LOGGER.debug("Test Sistem Operasi Pertama");
    }

    @Test
    public void testSistemOperasiKedua() throws Exception {
        assertEquals(sistemOperasiFedora.getNamaSistemOperasi(), "Fedora");
        assertEquals(sistemOperasiFedora.getVersiSistemOperasi(), "fedora 22");
        LOGGER.debug("Test Sistem Operasi Kedua");
    }

    @Test
    public void testSistemOperasiKetiga() throws Exception {
        assertEquals(sistemOperasiElementary.getNamaSistemOperasi(), "Elementary os");
        assertEquals(sistemOperasiElementary.getVersiSistemOperasi(), "Freya");
        LOGGER.debug("Test Sistem Operasi Ketiga");
    }

}

{% endhighlight %}

Jika telah selesai, mari kita jalankan :D. Berikut jalankan perintah berikut untuk melakukan compile dan testing.

{% highlight bash %}

mvn compile test

{% endhighlight %}

Jika tidak ada error maka akan muncul pesan seperti ini.

{% highlight bash %}

Results :

Tests run: 3, Failures: 0, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.903 s
[INFO] Finished at: 2015-07-28T19:24:13+07:00
[INFO] Final Memory: 16M/161M
[INFO] ------------------------------------------------------------------------

{% endhighlight %}

Kemudian jalankan aplikasi dengan perintah.

{% highlight bash %}

mvn exec:java -Dexec.mainClass="com.rizki.mufrizal.belajarSpring.App"

{% endhighlight %}

Sesuaikan dengan nama folder anda. Jika berhasil maka akan muncul output seperti ini.

{% highlight bash %}

2015-07-28 19:25:44,863 [com.rizki.mufrizal.belajarSpring.App.main()] DEBUG com.rizki.mufrizal.belajarSpring.App - Cek Spring Pertama : Sistem Operasi anda adalah Ubuntu Dengan Versi 14.04
2015-07-28 19:25:44,863 [com.rizki.mufrizal.belajarSpring.App.main()] DEBUG com.rizki.mufrizal.belajarSpring.App - Cek Spring Kedua   : Sistem Operasi anda adalah Fedora Dengan Versi fedora 22
2015-07-28 19:25:44,864 [com.rizki.mufrizal.belajarSpring.App.main()] DEBUG com.rizki.mufrizal.belajarSpring.App - Cek Spring Ketiga  : Sistem Operasi anda adalah Elementary os Dengan Versi Freya
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.982 s
[INFO] Finished at: 2015-07-28T19:25:44+07:00
[INFO] Final Memory: 12M/150M
[INFO] ------------------------------------------------------------------------

{% endhighlight %}

Sekian tutorial belajar spring dan Terima kasih :). Untuk source code lengkap, penulis publish di [Belajar Spring](https://github.com/RizkiMufrizal/Belajar-Spring).
