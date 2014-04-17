---
layout: post
title: "Web geliştiricilerin bilmesi gereken temel konular"
description: "Web geliştiricilerin bilmesi gereken temel konular"
modified: 2014-04-16
tags: [PHP]
image:
  feature: abstract-12.jpg
comments: true
share: true
---

Merhaba,

Zamanım olmadığı için uzun zamandır bloguma yazı yazmıyordum. Ancak neredeyse hergün sosyal platformlarda aynı hataları ve yanlış düşüncelerin olduğunu gördüğüm için bu yazıyı yazmaya karar verdim.

Bu yazımda web geliştiricilerinin bilmesi gereken temel konulardan bahsedeceğim. Aslında bahsedeceğim şeyler çok üst düzey konular değil ama bilinmesi ve uyulması size büyük bir avantaj sağlayacak.

Yazı boyunca web geliştirme süreçlerinin tamamına değinmek istiyorum, bu yüzden geliştirme ortamı, veritabanları, backend, frontend, css, otomasyon, deployment, versiyon kontrol vb. konularda bilmeniz gerekenleri anlatacağım.

Hazırsanız başlayalım.

> Not: Bu yazı açık kaynaklı olarak Github hesabım üzerinde yayınlanmaktadır. Eğer herhangi bir yanlışlık görürseniz veya eklemek istedikleriniz olursa pull request atabilirsiniz.

## Backend ##

### Global scopeyi asla kirletmeyin. ###

**Değişkenlerinizi global scope içerisinde tanımlamayın.**

Global scope içerisinde tanımladığınız şeyler uygulamanızın her tarafından erişebilir olur. Global scope içerisindeki verilere ne kadar bağımlı olursanız uygulamanın biryerinde hata yapma olasılığınız o kadar artar. Özellikle 3. parti pluginleri, komponentleri veya kütüphaneleri kullandığınızda, onların uygulamanızı kötü etkilemeyeceğinden emin olamazsınız. 

Örneğin:

```php
<?php
	$veritabani = //pdo bağlantısı
```

şeklinde global scope içerisinde bir değişken oluşturup buna bağlı kalırsanız, bir başkası istemeyerekte olsa

```php
<?php
	$veritabani = null;
```

yazarak uygulamanızı runtime esnasında bozabilir. Uygulamanın geri kalanında `veritabani` değişkeni `NULL` değerine sahip olacağı için hiçbir veritabanı işlemi yapılamaz hale gelecektir.

Bundan korunmak için, tanımlayacağınız değişkenleri `class scope` (sınıf scope) altında tanımlayın. Sınıflarınız da `namespace` altında tanımlansın.

**Kapsüllenecek şeyleri kapsülleyin, açıkta bırakmayın.**

Yukarıdaki örnek tek başına yeterli olmaz. Siz herşeyi sınıf scope içerisinde yazmış olabilirsiniz, ancak sınıflar uygulamanın herhangi biryerinde instantiate edilebilir (Türkçe'ye çevirebilen var mı?) ve buradan içeriğe direkt müdahele edilebilir.

Bunu önlemek için `OOP` (Nesne Yönelimli Programlama) temellerinden olan `Encapsulation` (Kapsülleme) özelliğini kullanabilirsiniz.

Örneğin `abstract` (soyut) bir veritabanı sınıfınız var ve bu sınıf bir başka sınıfın onu extend etmesiyle çalışacak.

```php
<?php

abstract class Database
{

    public $veritabani;
    private $info;

    public function __construct()
    {
        $this->info = new stdClass();
        $this->connect();
    }

    public function attempt()
    {
        //Veritabanına bağlanmayı dene
    }

    public function connect()
    {
        $this->attempt($this->getConnectionString());
        $this->info()->isConnected = true;
    }
}
```

Dikkat ettiyseniz `info` değişkenini `private` olarak yazdım.

```php
<?php

//Yukarıdaki sınıfa ek olarak

class MySQL extends Database implements DatabaseConnectorInterface
{

    public $queryString;

    public function __construct()
    {
        $this->queryString = //Mysql DB query string
    }
     
    public function getConnectionString()
    {
        return $this->connectionString;
    }
}

```

Şuan `MySQL` sınıfı, `Veritabanı` sınıfındaki `info` objesini değiştiremez, çünkü yetkisi yok. Neden? Çünkü değişken `private` olarak tanımladık ve sadece `Database` sınıfı scopesi içerisinden erişilebilir.

Size bu konuyu daha dünyevi bir örnekle anlatmaya çalışayım. 

Mesela KDV hesaplayıcı bir sınıf yazıyorsunuz ve KDV oranını `0,18` olarak belirlediniz. Eğer siz bunu dışarıdan erişilebilir yaparsanız, başka birisi bunu `3.00` olarak değiştirdiğinde ne olur? 100 liralık ürünü alacak kullanıcıdan 300 lira vergi çekmiş olursunuz. Kendinizi şirketin kapısının önünde bulmak için yeterli bir sebep.

Buna rağmen bazı durumlarda KDV oranı bilgisine erişmeniz gerekebilir. Örneğin ürünün KDV fiyatını hesaplattırmak için KDV Hesaplayıcı sınıfında KDV oranı kaç olarak belirlenmiş bunu öğrenmek isteyebilirsiniz. Burada `gettlers` devreye giriyor.

```php
<?php

class KDVCalculator 
{

    private $kdvRatio //0.18

    public function getKdvRatio()
    {
        return $this->kdvRatio;
    }
}
```

Bilmeyenler için, `gettlers` ve `settlers` aslında çok basit iki tanım. Gettler, alacağımız değişkeni bir fonksiyon üzerinden almak, settler ise değiştireceğimiz değişkenin değerini fonksiyon üzerinden değiştirmektir.

**Javascript kullanıyorsanız tanımlamaları çoğu zaman Javascript Object Literals kullanarak yapın.**

Javascript Object Literalleri PHP'nin sınıf yapısına benzer, ancak aynı şey değildir.

Aşağıdaki gibi bir javascript object literal tanımlayabiliriz.

```js

//Javascript object literaller PHP'deki sınıflara benzer, ancak aynı şey değildir.
var user = {
  
   name           : "Anıl",
   age            : 25,
   isAuthenticated: false,

   isOld: function() {
       return (this.age >= 30) true : false; 
   }

}
```

Uygulamanın herhangi biryerinde `if ( user.isOld() === true )` şeklinde kullanabilirsiniz.

> Not: `Javascript Object Literals` ile `Javascript Object Notation (JSON)` farklı şeylerdir. Karıştırmayın.

`Javascript Object Literals` ile `JSON` farkı:

a. Object literallerin keyleri stringdir, JSON'un attribute dir.

```js
var user = { name: "Anıl" } //Object literal
var user = { "name": "Anıl" }  //Object notation
```

b. JSON'da method tanımlanamaz. Yukarıdaki örnekte `isOld()` bir methoddur.

c. JSON'lar genellikle veri taşımak (API'lerde) ve veri saklamak için kullanılırken, object literaller genellikle OOP amacıyla kullanılır.

### Methodlarınızı ve sınıflarınızı küçük tutun. ###

Altın kuralımız şu: 

1. Bir methodda maksimum 10 satır
2. Bir sınıfta maksimum 10 method
3. Bir sınıfta maksimum 4 dependency (bağımlılık)
4. Bir pakette/komponentte 15 sınıf.

Nedir bu bağımlılık? 

Bağımlılık, oluşturduğumuz sınıfın çalışabilmesi için kullandığı diğer sınıfların bütünüdür. Bağımlılıklar `use` kullanılarak, `constructor injection` aracılığıyla veya scope içerisinde sınıflar `instantiate edilerek` eklenebilir.

Örneğin:

```php
<?php namespace Controllers;

use Baska\Bir\Uzaydaki\ImageSinifi as Image;

class HomeController
{
    private $database, $imageResizer, $logger;

    /* 
    | Constructor enjeksiyonu
    | Ayrıca bu bağımlılık enjeksiyonudur ve bunu bilmek
    | unit testler yazmak için mecburdur.
    */ 
    public function __construct(DatabaseInterface $database) //bağımlılık
    {
        $this->database = $database;
        $this->imageResizer = new Image; //bağımlılık
        $this->logger = new \Logger; //bağımlılık
    }

```

Burada, `HomeController` sınıfı, 3 şeye bağlıdır.

1. `DatabaseInterface` interfacesine sadık kalmış herhangi bir sınıfa.
2. `Image` sınıfına.
3. `Logger` sınıfına.

Eğer bu sayı 4'ün üzerine çıkarsa, o zaman sınıfınız gereğinden fazla iş yapmış olur. Hem yönetmek zorlaşır, hem de SOLID ilkelerinden biri olan Single Responsibility Principle (Tek amaç ilkesi) ilkesini ihlal etmiş oluruz.

> Not: Maksimum 4 bağımlılık bir kural değil, görüştür. Uymak zorunda değilsiniz ancak uymaya çalışırsanız 100kb'lık sınıflar içerisinde boğulmazsınız.

### mysql_real_escape_string() sizi SQL Injection'dan korumaz. ###

Gelen inputu `mysql_real_escape_string()` ile süzerek SQL Injection'dan korunduğunuzu sanıyorsanız yanılıyorsunuz. Bu klasik bir yanlıştır.

Süzmeyle hiç uğraşmadan `prepared statements` özelliğini kullanın. Prepared statements `Mysqli` ve `PDO`'da bulunabilir, ancak benim tavsiyem bir `ORM` aracı kullanmanızdır.

> Not: Bu bölüm ilerleyen günlerde daha detaylı anlatılacak.

### Kullanıcı şifrelerini md5() gibi yöntemlerle şifrelemeyin, vakit kaybı. ###

> Not: Bu bölüm ilerleyen günlerde daha detaylı anlatılacak.

### Uygulamanızda mümkün olduğunca Türkçe kullanmamaya çalışın. ###

Bu sektörde tek temel ihtiyaç var, o da İngilizce. Kullandığımız programlama dillerindeki methodlar İngilizce, takip edebileceğiniz ünlü yazılımcılar hep İngilizce konuşuyor, takip edebileceğiniz bloglar ve websiteleri İngilizce, Github üzerindeki açık kaynaklı projeler İngilizce, onların dökümantasyonları İngilizce.

Özellikle uygulama içerisindeki değişkenleri, sınıfları, methodları vb. Türkçe kullanmak son derece amatörce. Zaten PHP'de tam unicode desteği olmadığı için Türkçe karakterleri de kullanamıyorsunuz ve ortaya saçma sapan birşey çıkıyor.

Örneğin:

```php
<?php
    
class AssetYukleyici
{
    private $dosyalar;

    public function __construct()
    {
        $this->dosyalar = array();
    }

    public function style_ekle()
    {
        $stilDosyalari = Directory::get('*', 'css');
        if( !empty($stilDosyalari) return $stilDosyalari;
    }

    public function css_ciktisi()
    {
        return array_walk($this->dosyalar, function($value, $key) {
            return "<link href=\"assets/admin/css/{$value}.css'\" rel=\"stylesheet\">";
            });
        );
    }
```

Bana kalırsa son derece çirkin ve amatörce duruyor. Türkçe desen Türkçe değil, İngilizce Türkçe karışımı, ne olduğu belirsiz birşey.

Birde ileride bug çıktığını farzedelim, Stackoverflow'da sordunuz. Kimse sizin `asset yükleyici` değişken adınızdan birşey anlamayacak, açıklamak zorunda kalacaksınız. Muhtemelen kimse cevap vermeyecek.

Hem sadece bununla kalmıyor. Türkçe karakterleri kullanmak son derece sakıncalı. SSH üzerinden sunucuya bağlanıp Türkçe karakter yüzünden dosyanın açılmadığı veya komutları giremediğiniz zaman zaten kendinize kızacaksınız. (Mesela şuan bile bu yazıyı Türkçe karakterler yüzünden `Jekyll`'e compile ettireceğim diye kaç takla attım, biliyor musunuz?)

> Not: Tartıştığım yazılımcıların buna karşı argümanı Türk bir şirkette Türk yazılımcılarla ve bazen İngilizce bilmeyen yazılımcılarla çalıştığı bu yüzden Türkçe tercih ettikleriydi. Ben şahsen İngilizce bilmeyen yazılımcılara pek güvenemesem de, bu durumda projenin geleceğini düşünmek katı kurallara uymaktan daha önemlidir.

### Türkçe veya kimin yazdığını bilmediğiniz bloglardan, eğitim setlerinden uzak durun. ###

Bu tür bloglarda yazılan yazıların %90'ı kaynak belirtilmemiş çeviri, kalanların da birçoğu 2-3 aylık yazılımcıların `ilk heyecanlarıyla` bloglarına yazdıkları eksik ve yanlış makelelerden oluşuyor. (İstisnaları ayrı tutuyorum ancak ayrı tutacak istisnaya denk gelmedim şuana kadar.)

Bloglarına girip yazdıkları makaleleri okuyunca önce şaşırıyorsun. Adam scalability'den girmiş Nginx konfigürasyonlarına kadar, PHP 6 ile gelecek özelliklerden bile bahsetmiş. Sonra bir kaç blog yazısı daha yazmış `PHP'de echo kullanarak ekrana yazı bastırmak.`, `mysql_query() ile veritabanından veri çekmek.`

Neredeyse hepsi böyle. Gözlemlediğim kadarıyla Türkçe bloglardaki genel eksiklikler:

1. Açık kaynaklı değiller. Başkaları düzeltmede bulunamıyor. (Buna çok bilinen w3schools.com dahil)
2. Yanlış bir bilgi olduğunu söylediğin zaman yorumların siliniyor, çok az kişi eleştiriyi kabullenebiliyor.
3. Çevirilerde terimler genellikle yanlış çeviriliyor. Son derece alakasız sonuçlar çıkabiliyor.
4. Üst düzey PHP diye yazdıkları makaleler aslında PHP'nin temel bilgileri.

Lütfen kendinizi eğitirken yanlış bilgi alıp kafanızı karıştırmayın. Doğru bilgiyi doğru insanlardan, doğru makalelerden ve doğru kitaplardan alın.

> Not: Bu blog da Türkçe ve bunun bir ironi olduğunun farkındayım.

### ...ya performanslı olmazsa? ...ya çok include uygulamayı yavaşlatırsa? ###

OOP kullanmak istemeyenlerin, frameworklere çok hantal çalışıyor diyenlerin, modern tekniklerin uygulamayı yavaşlatacağını düşünenlerin klasik problemi. `Ya yavaşlarsa.`

Kısa cevap: Hiçbirşey olmaz.

Uzun cevap: Yakında yazarım. Bootleneckler, opcode caching nedir, scalability nedir, mikrooptimizasyonlar niye günü kurtarır vs.

### Kendinizi == yerine === kullanmaya alıştırın. ###

`==`, `loose comparison` yaptığı için `0` ile `false`'yi, `1` ile `true`'yi eşit sayar. Ancak bazı durumlarda gücü elinize almanız gerekir. 

Örneğin:

```php
<?php
    strpos('abcde', 'ab');
```

`strpos` fonksiyonu, ikinci parametredeki stringin, 1. parametredeki string içerisinde kaçıncı sırada geçtiğini bulur. Bu fonksiyon, bu şekilde kullanıldığında integer olan `0` değerini döndürecektir. Yani `ab` stringi ilk sırada geçiyor anlamına gelmekte.

Ancak, sen bunu `==` ile kontrol etmeye çalışırsan, `0` değeri `false` olarak algılanacak ve farketmeden bug çıkarmış olacaksın.

```php
<?php
    if ( strpos('abcde', 'ab') == false)
        return "ab kelimesi abcde içerisinde geçmiyor."; //hatalı
```

Yanlış. Doğrusu `===` kullanmak olmalıydı

```php
<?php
    if ( strpos('abcde', 'ab') === false)
         return "ab kelimesi abcde içerisinde geçmiyor. Gerçekten.";
```

PHP'nin doğasında loose comparison (==) ve gerektiğinde strict comparison (===) kullanmak var, ancak ben biraz disiplinli çalışmayı sevdiğimden daima strict comparison (===) kullanıyorum.

Özellikle `boolean` verileri karşılaştırırken mutlaka strict comparison operatörünü kullanın.

`Boolean` verileri şunlardır: 

1. Sayı olan 0 ve 1
2. Float olan 0.0 ve 1.0
3. Boolean olan true ve false
4. Boş string veya string olan 0 ("0")
5. null
6. Boş array (boş array false, dolu array true)
7. Object (daima true)
8. Resources (daima true, http://www.php.net/manual/en/resource.php)

### DRY kuralına uyun. Kendinizi tekrar etmeyin. ###

Yakında...

### Daima tutarlı olun. ###

Yakında...

### Dependency Injection, DI Container ve Inversion of Control ###

Kısaca...

### If içerisinde if içerisinde if son derece yanlış. ###

Yakında... (art of readable code)

### MVC kullanıyorsanız, MVC gibi kullanın. ###

**echo'yu unutun**

Yakında.

## Frontend ##

**CSS**

// CSS Explosiondan korunmak

**Sass**

**Browser compatibility**

**Tips&Tricks**

**Ne kadar az, o kadar şık.**

**Javascript**

Yakında

## Dev Environment ##

**Grunt**

**Tips & tricks**

**Terminale ne kadar yakın, o kadar iyi**

## Veritabanları ##

**Neden utf8mb4_unicode_ci?**

Yakında

## Deployment ##

**Capistrano**

Yakında

**FTP kullanmayın! (asla)!**

Yakında

## Versiyon kontrol ##

**Sus ve git kullan**

Yakında

## Aklıma gelenler ##

Yakında.

> Not: Bu makale vakit buldukça güncellenecektir. Eklenmesini istediğiniz konuları issue oluşturarak bildirebilirsiniz.