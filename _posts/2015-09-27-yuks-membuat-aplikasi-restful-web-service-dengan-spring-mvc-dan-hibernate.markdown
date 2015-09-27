---
layout: post
title: Yuks Membuat Aplikasi RESTful Web Service Dengan Spring MVC Dan Hibernate
modified:
categories:
excerpt:
tags: []
image:
  feature:
date: 2015-09-27T21:48:49+07:00
---

Kali ini penulis ingin mengajak anda untuk membuat sebuah aplikasi RESTful Web Service dengan menggunakan teknologi Spring MVC dan Hibernate.

>>RESTful merupakan salah satu dari jenis web service, dimana RESTful ini sendiri menggunakan pertukaran data antar state dengan menggunakan protokol HTTP (Hypertext Transfer Protocol). RESTful merupakan suatu gaya arsitektur perangkat lunak untuk pendistribusian sistem hipermedia seperti WWW. Istilah ini diperkenalkan pertama kalu pada tahun 2000 dengan disertai doktoral Roy Fielding yang merupakan salah seorang penulis utama spesifikasi HTTP.

Oke...teknologi RESTful ini sendiri dapat menggunakan beberapa format data sebagai pertukaran data yaitu JSON, XML dan lain - lain. Pada tutorial kali ini, penulis menggunakan format JSON.

Kita mulai dengan membuat project melalui terminal seperti berikut.

{% highlight bash %}
mvn archetype:generate \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DgroupId=com.rizki.mufrizal.belajarSpringMVCHibernate \
-DartifactId=Belajar-SpringMVC-Hibernate
{% endhighlight %}

Silahkan buka project dengan IDE anda, disini saya menggunakan netbeans. Struktur Aplikasi penulis seperti ini.
![Screenshot from 2015-09-27 19:21:30.png](../images/Screenshot from 2015-09-27 19:21:30.png)

Kita mulai dari class domain, disini saya membuat sebuah class dengan nama `Makanan` berikut adalah kodingannya.

{% highlight java %}
package com.rizki.mufrizal.belajarSpringMVCHibernate.domain;

import java.io.Serializable;
import java.math.BigDecimal;
import java.util.Date;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.Temporal;
import org.hibernate.annotations.GenericGenerator;

/**
 *
 * @author rizki
 * @since Sep 27, 2015
 */

@Entity
@Table(name = "tb_makanan")
public class Makanan implements Serializable {

    @Id
    @Column(name = "idMakanan", length = 150)
    @GeneratedValue(generator = "system-uuid")
    @GenericGenerator(name = "system-uuid", strategy = "uuid")
    private String idMakanan;

    @Column(name = "namaMakanan", length = 40)
    private String namaMakanan;

    @Column(name = "hargaMakanan")
    private BigDecimal hargaMakanan;

    @JsonFormat(pattern = "dd/MM/yyyy", shape = JsonFormat.Shape.STRING)
    @Column(name = "tanggalKadaluarsa")
    @Temporal(javax.persistence.TemporalType.DATE)
    private Date tanggalKadaluarsa;

    //getter
    //setter

}
{% endhighlight %}

Class `Makanan` merupakan class domain yang akan kita gunakan sebagai model di dalam konsep MVC. Class ini nantinya akan dimapping oleh hibernate. Terdapat 4 properti yaitu id makanan, nama makanan, harga makanan dan tanggal kadaluarsa. Kemudian kita beralih ke package repository, buat 2 class yaitu `MakananRepository` dan `MakananRepositoryImpl`. Class `MakananRepository` nantinya akan kita inject ke class `MakananServiceImpl`. Baiklah berikut adalah codingan dari class `MakananRepository`.

{% highlight java %}
package com.rizki.mufrizal.belajarSpringMVCHibernate.repository;

import com.rizki.mufrizal.belajarSpringMVCHibernate.domain.Makanan;
import java.util.List;

/**
 *
 * @author rizki
 * @since Sep 27, 2015
 */

public interface MakananRepository {

    public void save(Makanan makanan);

    public void update(Makanan makanan);

    public void delete(Makanan makanan);

    public Makanan getMakanan(String idMakanan);

    public List<Makanan> getMakanans();
}
{% endhighlight %}

Class `MakananRepositoryImpl` akan melakukan implementasi terhadap class `MakananRepository`. Semua yang berurusan dengan `query` kita satukan di dalam class repository. Berikut adalah codingannya.
{% highlight java %}
package com.rizki.mufrizal.belajarSpringMVCHibernate.repository;

import com.rizki.mufrizal.belajarSpringMVCHibernate.domain.Makanan;
import java.util.List;
import org.hibernate.SessionFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

/**
 *
 * @author rizki
 * @since Sep 27, 2015
 */

@Repository
public class MakananRepositoryImpl implements MakananRepository {

    @Autowired
    private SessionFactory sessionFactory;

    @Override
    public void save(Makanan makanan) {
        sessionFactory.getCurrentSession().save(makanan);
    }

    @Override
    public void update(Makanan makanan) {
        sessionFactory.getCurrentSession().update(makanan);
    }

    @Override
    public void delete(Makanan makanan) {
        sessionFactory.getCurrentSession().delete(makanan);
    }

    @Override
    public Makanan getMakanan(String idMakanan) {
        return sessionFactory.getCurrentSession().get(Makanan.class, idMakanan);
    }

    @Override
    public List<Makanan> getMakanans() {
        return sessionFactory.getCurrentSession().createCriteria(Makanan.class).list();
    }

}
{% endhighlight %}

Bisa dilihat kita menggunakan annotation `@Repository`, annotation ini berfungsi untuk memanage bean - bean, sedangkan disana juga terdapat `@Autowired` yang berfungsi untuk melakukan inject ke bean `sessionFactory`. Kemudian dari `SessionFactory` kita dapat melakukan perintah CRUD. Tahap selanjutnya kita membuat lagi 2 class yaitu `MakananService` dan `MakananServiceImpl`. Untuk class `MakananService` sama seperti class `MakananRepository`. Sedangkan class `MakananServiceImpl` berikut codingannya.

{% highlight java %}
package com.rizki.mufrizal.belajarSpringMVCHibernate.service;

import com.rizki.mufrizal.belajarSpringMVCHibernate.domain.Makanan;
import com.rizki.mufrizal.belajarSpringMVCHibernate.repository.MakananRepository;
import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

/**
 *
 * @author rizki
 * @since Sep 27, 2015
 */

@Service
@Transactional(readOnly = true)
public class MakananServiceImpl implements MakananService {

    @Autowired
    private MakananRepository makananRepository;

    @Transactional
    @Override
    public void save(Makanan makanan) {
        makananRepository.save(makanan);
    }

    @Transactional
    @Override
    public void update(Makanan makanan) {
        makananRepository.update(makanan);
    }

    @Transactional
    @Override
    public void delete(Makanan makanan) {
        makananRepository.delete(makanan);
    }

    @Override
    public Makanan getMakanan(String idMakanan) {
        return makananRepository.getMakanan(idMakanan);
    }

    @Override
    public List<Makanan> getMakanans() {
        return makananRepository.getMakanans();
    }

}

{% endhighlight %}

Mirip dengan class `MakananRepositoryImpl` hanya saja disini kita menggunakan annotation `@Service` karena class ini merupakan bagian dari pada bisnis coding. Disana kita menggunakan annotation `@Transactional` yang berfungsi untuk menghandle transaction pada database, konsep ini dikenal dengan 'AOP (Aspect Oriented Programming)'. Setelah selesai, kita buat lagi class `MakananController`, class ini yang akan berfungsi sebagai penerima request dan mengembalikan request, Berikut codingannya.

{% highlight java %}
package com.rizki.mufrizal.belajarSpringMVCHibernate.controller;

import com.rizki.mufrizal.belajarSpringMVCHibernate.domain.Makanan;
import com.rizki.mufrizal.belajarSpringMVCHibernate.service.MakananService;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;

/**
 *
 * @author rizki
 * @since Sep 27, 2015
 */

@RestController
@RequestMapping(value = "/api")
public class MakananController {

    @Autowired
    private MakananService makananService;

    @ResponseStatus(HttpStatus.OK)
    @RequestMapping(value = "/makanan", method = RequestMethod.GET, produces = {MediaType.APPLICATION_JSON_VALUE})
    public List<Makanan> getMakanans() {
        return makananService.getMakanans();
    }

    @ResponseStatus(HttpStatus.CREATED)
    @RequestMapping(value = "/makanan", method = RequestMethod.POST, produces = {MediaType.APPLICATION_JSON_VALUE}, consumes = {MediaType.APPLICATION_JSON_VALUE})
    public Map<String, Object> saveMakanan(@RequestBody Makanan makanan) {
        makananService.save(makanan);

        Map<String, Object> m = new HashMap<>();
        m.put("Success", Boolean.TRUE);
        m.put("Info", "Data Tersimpan");

        return m;
    }

    @ResponseStatus(HttpStatus.OK)
    @RequestMapping(value = "/makanan", method = RequestMethod.PUT, produces = {MediaType.APPLICATION_JSON_VALUE}, consumes = {MediaType.APPLICATION_JSON_VALUE})
    public Map<String, Object> updateMakanan(@RequestBody Makanan makanan) {
        makananService.update(makanan);

        Map<String, Object> m = new HashMap<>();
        m.put("Success", Boolean.TRUE);
        m.put("Info", "Data Berhasil di update");

        return m;
    }

    @ResponseStatus(HttpStatus.OK)
    @RequestMapping(value = "/makanan/{idMakanan}", method = RequestMethod.DELETE, produces = {MediaType.APPLICATION_JSON_VALUE})
    public Map<String, Object> deleteMakanan(@PathVariable("idMakanan") String idMakanan) {
        makananService.delete(makananService.getMakanan(idMakanan));

        Map<String, Object> m = new HashMap<>();
        m.put("Success", Boolean.TRUE);
        m.put("Info", "Data Berhasil di hapus");

        return m;
    }

}
{% endhighlight %}

Wah makin banyak annotationnya :D ,Berikut adalah penjelasan singkatnya.

- @RestController : Merupakan annotaion dari spring mvc yang khusus untuk RESTful sehingga dia akan langsung mengambalikan data yang di return dari sebuah method.
- @RequestMapping : berfungsi sebagai mendeklarasikan URI yang akan kita gunakan.
- @ResponseStatus : berfungsi untuk membuat response status yang kita inginkan.
- @RequestBody : berfungsi untuk menerima request berupa JSON dari client.
- @PathVariable : berfungsi untuk mengambil data yang ada pada path URI

Oke untuk class java nya telah selesai, kita beralih ke konfigurasi XML untuk sprng mvc dan hibernate. Buat folder `webapp` sejajar dengan folder `java`, dan `resources`, kemudian di dalamnya buat sebuah folder dengan nama `WEB-INF`. Di dalam folder ini tambahkan sebuah file `jdbc.properties` untuk konfigurasi database, berikut codingannya.

{% highlight properties %}
#
# @author rizki
# @since Sep 27, 2015
#

jdbc.driverClassName = com.mysql.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/belajar
jdbc.username = root
jdbc.password = root
{% endhighlight %}

Kemudian buat sebuah file `hibernate.cfg.xml` untuk konfigurasi hibernate, berikut adalah codingannya.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN" "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        <property name="hibernate.hbm2ddl.auto">update</property>
        <property name="hibernate.show_sql">true</property>
        <property name="hibernate.format_sql">true</property>

        <mapping class="com.rizki.mufrizal.belajarSpringMVCHibernate.domain.Makanan" />

    </session-factory>
</hibernate-configuration>
{% endhighlight %}

Kodingan diatas telah dibahas pada tutorial [Belajar Hibernate](http://rizkimufrizal.github.io/belajar-hibernate/). Selanjutnya adalah kita membuat sebuah file lagi yaitu `mvc-dispatcher-servlet.xml` untuk konfigurasi spring mvc. Berikut kodingannya.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <context:annotation-config/>
    <mvc:annotation-driven/>
    <tx:annotation-driven transaction-manager="transactionManager"/>
    <context:property-placeholder location="/WEB-INF/jdbc.properties"/>

    <context:component-scan base-package="com.rizki.mufrizal.belajarSpringMVCHibernate"/>

    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="${jdbc.driverClassName}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="/WEB-INF/hibernate.cfg.xml"/>
    </bean>

    <bean id="transactionManager" class="org.springframework.orm.hibernate5.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory"/>
    </bean>

</beans>
{% endhighlight %}

Berikut adalah penjelasan singkatnya.

- `tx:annotation-driven` memberitahukan kepada spring bahwa kita mengaktifkan transaction dengan menggunakan annotation `@Transactional`.
- `context:property-placeholder` untuk meload file konfigurasi database.
- `context:component-scan` berfungsi untuk mendefinisikan class - class mana saja yang akan menggunakan annotation seperti `@Repository`, `@Service`, `@Controller` dan `@RestController`.
- `bean id="dataSource"` merupakan bean yang berfungsi sebagai konfigurasi database.
- `bean id="sessionFactory"` merupakan bean yang berfungsi untuk meload konfigurasi hibernate.
- `bean id="transactionManager"` merupakan bean yang berfungsi sebagai transaction.

Untuk meload semua konfigurasi tersebut, buatlah sebuah file `web.xml`, kemudian masukkan codingan berikut.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <servlet>
        <servlet-name>mvc-dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>mvc-dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
            /WEB-INF/mvc-dispatcher-servlet.xml
        </param-value>
    </context-param>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

</web-app>
{% endhighlight %}

Yang terakhir adalah buatlah sebuah file `log4j.properties` di dalam folder `resources`, berikut adalah codingannya.

{% highlight properties %}
#
# @author rizki
# @since Sep 27, 2015
#

log4j.rootLogger=DEBUG, file, stdout

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

Akhirnya selesai juga :D, mari kita lakukan uji coba dengan menggunakan [Postman](https://www.getpostman.com/). Buka postman lalu lakukan uji coba dengan method `POST` untuk menyimpan data seperti ini.

![Screenshot from 2015-09-27 21:30:25.png](../images/Screenshot from 2015-09-27 21:30:25.png)

Kemudian uji coba dengan menggunakan method GET untuk mengambil data

![Screenshot from 2015-09-27 21:30:36.png](../images/Screenshot from 2015-09-27 21:30:36.png)

Kemudian uji coba dengan menggunakan method PUT untuk merubah data

![Screenshot from 2015-09-27 21:31:11.png](../images/Screenshot from 2015-09-27 21:31:11.png)

Dan yang adalah uji coba dengan menggunakan method DELETE untuk menghapus data

![Screenshot from 2015-09-27 21:31:28.png](../images/Screenshot from 2015-09-27 21:31:28.png)

Sekian tutorial membuat aplikasi resftful web service dengan teknologi spring mvc, hibernate dan Terima kasih :). Untuk source code lengkap, penulis publish di [Belajar Spring MVC dan Hibernate](https://github.com/RizkiMufrizal/Belajar-SpringMVC-Hibernate).
