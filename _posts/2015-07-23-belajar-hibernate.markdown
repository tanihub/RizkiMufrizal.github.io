---
layout: post
title: Belajar Hibernate
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-07-23T11:35:10+07:00
---

Setelah melakukan banyak [konfigurasi untuk coding java](http://rizkimufrizal.github.io/instalasi-perlengkapan-coding-java/), selanjutnya kita akan belajar tentang framework hibernate. Sebelum kita belajar hibernate, alangkah baiknya kita mengenal terlebih dahulu konsep dari hibernate itu sendiri.

##Object Relational Mapping (ORM)

>Object Relational Mapping atau lebih sering disebut ORM adalah sebuah konsep dimana sebuah object merupakan representasi dari basis data yang berbasis relational.

Mungkin sedikit bingung dengan pengertian diatas, untuk mempermudah pemahaman, penulis akan memberi contoh berikut.

di dalam basis data yang relational kita mengenal adanya tabel seperti dibawah ini.

| Id Barang | Nama Barang | Jenis Barang | Tanggal Kadaluarsa |
|:----------|:------------|:-------------|:------------------:|
|A001       | Rinso       | Cair         | 1 Januari 2016     |
|A002       | Tango       | Padat        | 12 September 2015  |
{: rules="groups"}

dengan menggunakan sintak SQL maka querynya adalah sebagai berikut

{% highlight sql %}
create table tb_barang(
    idBarang varchar(150) not null,
    namaBarang varchar(45),
    jenisBarang varchar(10),
    tanggalKadaluarsa date,
    primary key (idBarang)
)
{% endhighlight %}

diatas adalah contoh dengan menggunakan basis data relational, maka berikutnya bagaimana kita menghubungkan dengan pemrograman berorientasi objek. Berikut adalah contoh jika kita membuatnya ke dalam pemrograman berorientasi objek.

{% highlight java %}
public class Barang {
  private String idBarang;
  private String namaBarang;
  private String jenisBarang;
  private String tanggalKadaluarsa;

  //getter setter
}
{% endhighlight %}

Dapat disimpulkan bahwa sebuah class adalah representasi dari sebuah tabel dan sebuah property atau variabel adanya representasi dari kolom pada basis data. Banyak framework dari berbagai bahasa pemrograman yang telah melakukan implementasi dari konsep ORM ini, diantaranya adalah

- Hibernate (Java)
- Spring Data JPA (Java)
- MyBatis (Java)
- JPA (Java)
- Ebean (Java)
- Waterline (Node jS / Javascript)
- Ruby On Rails (Ruby)
- dan lain - lain

Setelah mengenal konsep ORM, selanjutnya kita akan menggunakan framework hibernate.

##Generate Project Hibernate

Untuk membuat project hibernate, kita menggunakan maven. Buka terminal lalu jalankan perintah berikut untuk membuat project dengan maven

{% highlight bash %}
mvn archetype:generate \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DgroupId=com.rizki.mufrizal.belajarHibernate \
-DartifactId=Belajar-Hibernate
{% endhighlight %}

Buka project tersebut dengan menggunakan Intellij IDEA dengan cara melakukan import terhadap project, Penulis menggunakan Intellij IDEA versi 14.1.4. Kemudian buka file pom.xml. File ini berisi konfigurasi project maven. Karena kita membutuhkan library hibernate, maka kita akan mendeklarasikannya di dalam file pom.xml ini, berikut adalah konfigurasinya.

{% highlight xml %}
<dependencies>

  <!-- junit -->
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
  </dependency>

  <!-- hibernate -->
  <dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>4.3.10.Final</version>
  </dependency>

  <!-- database hsqldb -->
  <dependency>
    <groupId>org.hsqldb</groupId>
    <artifactId>hsqldb</artifactId>
    <version>2.3.3</version>
  </dependency>

  <!--slf4j-->
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
      <version>3.3</version>
      <configuration>
        <source>1.8</source>
        <target>1.8</target>
      </configuration>
    </plugin>
  </plugins>
</build>
{% endhighlight %}

Database yang akan kita gunakan adalah hsqldb, database tersebut bersifat embedded artinya dapat dipindahkan ke komputer lain tanpa perlu melakukan banyak konfigurasi. Buatlah sebuah package baru dengan nama `domain`, pada package ini kita akan memasukkan sebuah class yang berisikan domain - domain yang nantikan akan di mapping oleh hibernate. Pada tutorial kali ini, penulis membuat sebuah class dengan nama Mahasiswa. Jangan lupa untuk membuat getter dan setter dari setiap property.

{% highlight java %}

@Entity
@Table(name = "tb_mahasiswa")
public class Mahasiswa implements Serializable{

    @Id
    @Column(name = "npm", length = 8)
    private String npm;

    @Column(name = "nama", nullable = false, length = 45)
    private String nama;

    @Column(name = "kelas", nullable = false, length = 5)
    private String kelas;

    @Column(name = "jenisKelamin", nullable = false, length = 6)
    @Enumerated(EnumType.STRING)
    private JenisKelamin jenisKelamin;

    @Column(name = "tanggalLahir", nullable = false)
    @Temporal(TemporalType.DATE)
    private Date tanggalLahir;

    //getter setter  

}

{% endhighlight %}

Berikut penjelasan singkat tentang annotation tersebut

- @Entity berfungsi mendeklarasikan bahwa class ini merupakan sebuah class entity yang akan di mapping oleh hibernate.
- @Table berfungsi untuk mendeklarasikan nama tabel.
- @Id berfungsi untuk mendeklarasikan bahwa property tersebut merupakan primary key.
- @Column berfungsi untuk mendeklarasikan definisi dari sebuah kolom.
- @Enumerated berfungsi untuk mendeklarasikan bahwa field tersebut adalah berisi value yang hanya dapat dipilih.
- @Temporal berfungsi untuk mendeklarasikan tanggal.

Untuk jenis kelamin, buat sebuah class enum seperti berikut

{% highlight java %}

public enum JenisKelamin {
    PRIA, WANITA
}

{% endhighlight %}

Tahap selanjutnya adalah membuat konfigurasi hibernate, buatlah sebuah file `hibernate.cfg.xml` pada folder `resources`. Kemudian masukkan konfigurasi seperti dibawah ini

{% highlight xml %}

<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <property name="hibernate.dialect">org.hibernate.dialect.HSQLDialect</property>
        <property name="hibernate.connection.driver_class">org.hsqldb.jdbcDriver</property>
        <property name="hibernate.connection.url">jdbc:hsqldb:databaseHSQLDB/BelajarHibernate</property>
        <property name="hibernate.hbm2ddl.auto">update</property>
        <property name="hibernate.show_sql">true</property>
        <property name="hibernate.format_sql">true</property>

        <mapping class="com.rizki.mufrizal.belajarHibernate.domain.Mahasiswa" />
    </session-factory>
</hibernate-configuration>

{% endhighlight %}

berikut penjelasan tentang konfigurasi diatas

- hibernate.dialect berfungsi untuk mendeklarasikan dialect apa yang akan digunakan, sesuaikan dengan type basis data yang digunakan.
- hibernate.connection.driver_class berfungsi untuk mendeklarasikan driver yang akan digunakan.
- hibernate.connection.url berfungsi untuk mendeklarasikan url ke sebuah basis data, nama databasenya adalah BelajarHibernate yang nanti akan dibuat sebuah folder BelajarHibernate pada root project.
- hibernate.hbm2ddl.auto terdapat beberapa fungsi diantaranya adalah
  - Create : Jika table sudah ada maka semua akan di drop dan dibuat ulang.
  - Update : jika table belum ada maka akan dibuat, jika tabel sudah ada maka hanya dilakukan update.
- hibernate.show_sql berfungsi untuk menampilkan sintak sql yang dilakukan oleh hibernate.
- hibernate.format_sql berfungsi untuk melakukan format sql sehingga mudah dibaca.
- mapping berfungsi untuk melakukan mapping terhadap class yang dituju, class disini berasal dari package entiry yang tadinya kita buat.

Langkah selanjutnya buatlah sebuah package `repository` dan buat sebuah interface di dalamnya dengan nama `MahasiswaRepository`. Fungsi repository adalah untuk melakukan query terhadap basis data. Berikut adalah codingan dari interface MahasiswaRepository.

{% highlight java %}

public interface MahasiswaRepository {
    void save(Mahasiswa mahasiswa);
    void update(Mahasiswa mahasiswa);
    void delete(Mahasiswa mahasiswa);
    Mahasiswa getMahasiswa(String npm);
    List<Mahasiswa> getMahasiswas();
}

{% endhighlight %}

Kemudian buat sebuah class dengan nama `MahasiswaRepositoryImpl` di dalam package `repository` untuk melakukan implementasi interface tersebut. Berikut kodingan untuk class tersebut.

{% highlight java %}

public class MahasiswaRepositoryImpl implements MahasiswaRepository{

    private SessionFactory sessionFactory;
    private static final Logger LOGGER = LoggerFactory.getLogger(MahasiswaRepositoryImpl.class);

    public MahasiswaRepositoryImpl(SessionFactory sessionFactory){
        this.sessionFactory = sessionFactory;
    }

    @Override
    public void save(Mahasiswa mahasiswa){
        Session session = sessionFactory.openSession();

        try {
            session.beginTransaction();
            session.save(mahasiswa);
            session.getTransaction().commit();
            LOGGER.debug("Data Tersimpan");
        }catch (HibernateException e){
            session.getTransaction().rollback();
            LOGGER.error("Error Bung {}", e.getMessage());
        }finally {
            session.close();
            LOGGER.info("Transaction selesai");
        }
    }

    @Override
    public void update(Mahasiswa mahasiswa){
        Session session = sessionFactory.openSession();

        try {
            session.beginTransaction();
            session.update(mahasiswa);
            session.getTransaction().commit();
            LOGGER.debug("Data Di Update");
        }catch (HibernateException e){
            session.getTransaction().rollback();
            LOGGER.error("Error Bung {}", e.getMessage());
        }finally {
            session.close();
            LOGGER.info("Transaction selesai");
        }
    }

    @Override
    public void delete(Mahasiswa mahasiswa){
        Session session = sessionFactory.openSession();

        try {
            session.beginTransaction();
            session.delete(mahasiswa);
            session.getTransaction().commit();
            LOGGER.debug("Data Di Hapus");
        }catch (HibernateException e){
            session.getTransaction().rollback();
            LOGGER.error("Error Bung {}", e.getMessage());
        }finally {
            session.close();
            LOGGER.info("Transaction selesai");
        }
    }

    @Override
    public Mahasiswa getMahasiswa(String npm){
        Session session = sessionFactory.openSession();
        Mahasiswa mahasiswa;

        try {
            session.beginTransaction();
            mahasiswa = (Mahasiswa) session.get(Mahasiswa.class, npm);
            session.getTransaction().commit();
            LOGGER.debug("get mahasiswa");

            return mahasiswa;

        }catch (HibernateException e){
            session.getTransaction().rollback();
            LOGGER.error("Error Bung {}", e.getMessage());
        }finally {
            session.close();
            LOGGER.info("Transaction selesai");
        }

        return null;
    }

    @Override
    public List<Mahasiswa> getMahasiswas(){
        Session session = sessionFactory.openSession();
        List<Mahasiswa> mahasiswas;

        try {
            session.beginTransaction();
            mahasiswas = session.createCriteria(Mahasiswa.class).list();
            session.getTransaction().commit();
            LOGGER.debug("get mahasiswas");

            return mahasiswas;

        }catch (HibernateException e){
            session.getTransaction().rollback();
            LOGGER.error("Error Bung {}", e.getMessage());
        }finally {
            session.close();
            LOGGER.info("Transaction selesai");
        }

        return null;
    }

}

{% endhighlight %}

Pada class diatas terdapat beberapa method CRUD diantaranya adalah save, update, delete, get objek dan get semua data. Masing - masing method mempunyai transaction dan juga dilengkapi dengan try catch. Jika kita menggunakan spring maka fungsi transaction tersebut akan dihandle oleh spring. Selanjutnya agar konfigurasi hibernate dapat di load maka buat sebuah class `HibernateUtil`, berikut adalah konfigurasinya.

{% highlight java %}

public class HibernateUtil {

    private static final SessionFactory sessionFactory;
    private static final MahasiswaRepository MAHASISWA_REPOSITORY;

    static {
        try {
            // Create the SessionFactory from standard (hibernate.cfg.xml)
            // config file.
            sessionFactory = new AnnotationConfiguration().configure().buildSessionFactory();
            MAHASISWA_REPOSITORY = new MahasiswaRepositoryImpl(sessionFactory);
        } catch (Throwable ex) {
            // Log the exception.
            System.err.println("Initial SessionFactory creation failed." + ex);
            throw new ExceptionInInitializerError(ex);
        }
    }

    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }

    public static MahasiswaRepository getMahasiswaRepository() {
        return MAHASISWA_REPOSITORY;
    }
}

{% endhighlight %}

Dan yang terakhir adalah membuat sebuah main class yang akan berfungsi untuk menjalankan project. Dari generate project maven terdapat sebuah class yaitu `App`. Gunakan class tersebut untuk main class, dan masukkan codingan berikut ini.

{% highlight java %}

public class App {
    public static void main( String[] args ) throws ParseException {
        MahasiswaRepository mahasiswaRepository = HibernateUtil.getMahasiswaRepository();
        Logger logger = LoggerFactory.getLogger(App.class);

        Mahasiswa mahasiswa = new Mahasiswa();
        mahasiswa.setNpm("58412085");
        mahasiswa.setNama("Rizki Mufrizal");
        mahasiswa.setKelas("3IA04");
        mahasiswa.setJenisKelamin(JenisKelamin.PRIA);
        mahasiswa.setTanggalLahir(new SimpleDateFormat("dd/MM/yyyy").parse("15/11/1993"));

        mahasiswaRepository.save(mahasiswa);

        List<Mahasiswa> mahasiswas = mahasiswaRepository.getMahasiswas();

        for(Mahasiswa mahasiswaData: mahasiswas){
            logger.info("Npm           : {}", mahasiswaData.getNpm());
            logger.info("Nama          : {}", mahasiswaData.getNama());
            logger.info("Kelas         : {}", mahasiswaData.getKelas());
            logger.info("Jenis Kelamin : {}", mahasiswaData.getJenisKelamin());
            logger.info("Tanggal Lahir : {}", mahasiswaData.getTanggalLahir());
        }

        Mahasiswa mahasiswa1 = mahasiswaRepository.getMahasiswa("58412085");
        mahasiswa1.setNama("rizki");

        mahasiswaRepository.update(mahasiswa1);

        mahasiswaRepository.delete(mahasiswa1);

    }
}

{% endhighlight %}

akhirnya selesai juga :D . Untuk mempermudah logging aplikasi maka kita menggunakan bantuan SLF4J, buat sebuah file properties dengan nama `log4j` pada folder `resources` dengan konfigurasi berikut.

{% highlight properties %}

log4j.rootLogger=DEBUG, file, stdout
log4j.logger.org.hibernate=DEBUG
log4j.logger.org.hibernate.type=ALL

# Direct log messages to a log file
log4j.appender.file=org.apache.log4j.RollingFileAppender
log4j.appender.file.File=logs/LogFile.log
log4j.appender.file.MaxFileSize=5MB
log4j.appender.file.MaxBackupIndex=10
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss:SSS} %C:%L %-5p - %m%n

# Direct log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss:SSS} %C:%L %-5p - %m%n

{% endhighlight %}

Oke untuk logging aplikasi akan dijelaskan pada postingan sekanjutnya :) . Jika sudah maka jalankan aplikasinya dengan perintah berikut.

{% highlight bash %}

mvn compile
mvn exec:java -Dexec.mainClass="com.rizki.mufrizal.belajarHibernate.App"

{% endhighlight %}

Jangan lupa sesuaikan dengan package project anda. Sekian tutorial belajar hibernate dan Terima kasih :). Untuk source code lengkap, penulis publish di [Belajar Hibernate](https://github.com/RizkiMufrizal/Belajar-Hibernate).
