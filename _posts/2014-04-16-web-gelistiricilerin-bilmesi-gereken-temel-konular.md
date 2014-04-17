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

// Not: Bu bölüm ilerleyen günlerde daha detaylı anlatılacak.

### Kullanıcı şifrelerini md5() gibi yöntemlerle şifrelemeyin, vakit kaybı. ###

// Bu bölüm ilerleyen günlerde daha detaylı anlatılacak.

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

### DRY kuralına uyun ve akıllı çalışın. ###

`DRY (Don't Repeat Yourself)`, Türkçe'siyle `Kendinizi Tekrar Etmeyin` kuralını hem PHP'in temelinde, hem de gerçekten üst düzey konularda kullanabilirsiniz.

Temel bilgiye sahip bir yazılımcı, aynı şeyleri tekrar etmekten bıkıp o işlem için fonksiyon oluşturuyorsa DRY için bir adım atmış olur. Üst düzey bir yazılımcı her projesinde kullanabileceği bir komponent yazmış ve bunu package managerlar tarafından yönetiyorsa DRY için bir adım atmış olur.

// Salt PHP ve L4 konusunda DRY örnekleri gelecek buraya.

`DRY`ın sonu yoktur ve sadece programlama dilleriyle ilgili değildir. Proje geliştirirken sık sık yaptığınız işlemleri bilgisayara yaptırmakta bu kural için atılmış adımlar olacaktır.

Örneğin, veritabanı yedeği mi alınacak? Veritabanı yönetici paneline gir, veritabanını seç, tabloları seç, exporta tıkla, yol olarak bir path belirle, çıktıyı oluştur, o klasöre gir, çıktıyı zip içerisine koy, sonra ismini "x dbsi yedeği" yap... Bu tür işlemlerle kaybettiğiniz zamanı hesaplayın ve o kaybolan zaman içerisinde kaç tane proje geliştirebileceğinizi düşünün.

Bunun yerine:

    grunt backup:veritabani

yazdığınızda bu işlemlerin sizin yerinize yapılması daha hoş olmaz mı?

Veya:

    grunt backup:veritabani --push

yazdığınızda hem veritabanı yedeğinin alınıp, hem csslerin optimize edilip, hem sunucuya veritabanını yedeğinin yüklenmesini sağlamak istemez misiniz?

Proje geliştirirken en çok nelere vakit harcadığınızı düşünün ve bilgisayarın yapabileceği herşeyi bilgisayara yaptırın. 

Her seferinde public function yazmak zor mu geliyor? Kullandığınız IDE içerisinde Snippet oluşturun. (ya da daha iyi ihtimalle, zaten birileri oluşturmuş ve Github'da paylaşmıştır, araştırın.)

Uzun terminal komutları zor mu geliyor? Alias oluşturun.

Özellikle genç olan arkadaşlar bunun ne kadar önemli olduğunun farkında olmalılar. Günde 8 saat çalışarak 10 birim iş yapacağına 30 birim iş yapabilirsin. Uzun vadede ne kadar çok zaman ve (dolayısıyla para) kazanabileceğinin farkına var. Daha `akıllı çalış`. Daima daha fazlasını öğren. İyi olmak kadar hızlı olmakta önemli.

> Not: Geliştirme ortamında yapabileceklerinzden iyileştirmelerden Geliştirme Ortamı bölümünde bahsedeceğim.

### Daima tutarlı olun. ###

Indenting için tab kullanıyorsanız, tab kullanarak devam edin. Methodları _ kullanarak ayırıyorsanız, böyle devam edin. Yaptığınız herşey tutarlı olsun. CSS'lerinizi yazarken ayraç olarak - kullanıyorsanız, heryerde - kullanın.

Tutarsızlığın en güzel örneği PHP çekirdeği. `strpos` ve `str_replace` fonksiyonlarını ele alalım. Niye `str_position` değil de `strpos`?

Neden bazı fonksiyonlar önce diziyi, sonra stringi alırken diğerleri önce stringi, sonra diziyi alıyor? Bu tutarsızlıkların hepsini hatırlamak zorundamıyız? Neden bir standart yok?

Mesela neredeyse tüm programlama dillerinde `reverse()` verilen stringi tersine çevirir. PHP'de bu ne tahmin edin bakalım?

`str_reverse? streverse? strrev? revstr? reverse?`

Kazanan `strrev`.

Siz bunu asla yapmayın. TAB kullanmayı bırakıp 4 space kullanmaya başlamaya karar verdiyseniz, ya tüm uygulamayı buna uyarlayın, ya da eski şekil devam edin.

> Not: Sırf bu yüzden PHP'in yediği lafları hayal edemezsiniz, ancak PHP ekibininde yapabileceği fazla birşey yok çünkü geliştirilmiş onca uygulamayı bozmak istemiyorlar.

> Not 2: Eğer PHP'de `Ruby` ve `Javascript` gibi dillerdeki `reverse()` methodunu kullanmak istiyorsanız, PHP'in `scalar objects` özelliğini kullanabilirsiniz ancak birçok eksikliği/limitasyonu var.

### Dependency Injection, Dependency Injection Container ve Inversion of Control ###

Bunların ne olduğunu bilmek artık her geliştirici için şart olduğu için ne olduklarını yazma ihtiyacı hissediyorum.

`Dependency Injection` (Bağımlılık Enjeksiyonu), James Shore'ın dediği gibi: "5 centlik konsept için 25 dolarlık terim kullanılması."

Bağımlılık Enjeksiyonu mantık olarak son derece basit. Hatta, şuana kadar verdiğim örneklerde birkaç defa kullandım. Kuralımız basit. Oluşturduğunuz sınıflarda `new` kullanmayacaksınız, kullanmamız gereken bağımlılık sınıfları bizim sınıfımıza verilecek.

Örneğin:

```php
<?php

class Deneme
{
     private $mailer;

     public function __construct()
     {
         $this->mailer = new Mailer;
     }
}
```

bunu yapmak büyük bir hata. Böyle yaptığımız zaman unit testlerimizi yapmak çok zor, hatta imkansız duruma gelir. Bunun yerine dependency injection kullanmalıyız.

```php
<?php

class Deneme
{
     private $mailer;

     public function __construct(Mailer $mailer)
     {
         $this->mailer = $mailer;
     }
}

//Uzaklarda biryerlerde

<?php
    new Deneme(new Mailer);

```

Dependency Injection bu kadar basit. Sınıfımız kendisi `Mailer` sınıfını oluşturmaktansa, `constructor injection` (Enjeksiyonu constructor üzerinden eklemek) aracılığıyla `Mailer` sınıfına sahip oluyor.

Ancak, bu yazdığımız kod `SOLID İlkeleri`'ni ihlal ediyor çünkü sınıfı direkt olarak enjekte etmiş oluyoruz. (L) Liskov's Substitution Principle kuralı der ki; sınıflar, başka sınıflar yerine abstractionlara bağımlı olmalı.

Bu PHP'de şu anlama geliyor. `Mailer` sınıfı bir interfaceyi implement etmeli, ve bu interfaceye uyan herhangi bir sınıf `Deneme` sınıfında çalışabilir olmalıdır.

Bir örnekle açıklayalım. Öncelikle interfacemizi oluşturalım ve hangi methodların bulunması gerekiyor bunları belirtelim.

```php
<?php

interface MailerInterface {
    public function mail();
    public function attachFile();
    public function setSender();
    public function setTo();
}

```

Daha sonra `Mailer` sınıfımızın bu interfaceye uymasını sağlayalım.

```php
<?php

class Mailer implements ImageInterface
{
    public function mail()
    {
        // mail fonksiyonu
    }
    public function attachFile()
    {
        // attachFile fonksiyonu
    }
    public function setSender()
    {
        // setSender fonksiyonu
    }
    public function setTo($to)
    {
        // setTo fonksiyonu
    }
}
```

Son olarak, `Mailer` sınıfı yerine, `ImageInterface` interfacesini enjekte edelim.

```php
<?php

class Deneme
{
     private $mailer;

     public function __construct(MailerInterface $mailer)
     {
         $this->mailer = $mailer;
     }
}
```

Şuan `Deneme` sınıfımız `Mailer` sınıfı yerine, `MailerInterface` interfacesine uyan herhangi bir sınıfı kabul edecek.

```php
<?php

class Deneme
{
     private $imageResizer;

     public function __construct(ImageInterface $image)
     {
         $this->imageResizer = $image;
     }
}
```

Bu olayı dünyevi bir örnekle anlatalım. Aracınızla gidiyorsunuz ve benzininiz bitmek üzere. Benzinliğe girip benzin almak için bir benzinliğe uğradınız.

1. Eğer benzinliğe (sınıf) bağlı olsaydınız, Türkiye'deki tek benzinciden benzin alabilirdiniz.
2. Benzin yerine mezot doldurulmadığından emin olamazdınız. Aracınızı çalıştırdığınızda neden motordan garip sesler geliyor diye düşünürdünüz.

Interfacelere bağlı kalmak, bizim Türkiye'deki tüm benzincilerden benzin almamıza yardımcı olur. Çünkü eminiz benzin pompası bizim aracımızın benzin deposuna uygun. Pompadan çıkan şey (benzin) bizim aracımızın kabul ettiği birşey. (interface)

Şimdi yukarıda verdiğimiz örneği yazılımsal bir örnekle anlatalım.

Mail göndermek için `SwiftMailer`, `SMTPMailer`, `AWSMailer` veya `MandrillMailer` kullanabiliriz. Interfacemize uyduğu sürece istediğimiz sınıfı kullanabiliriz.

**Houston, bir sorunumuz var....**

Sorunumuz şu. Interface'ler sınıflar gibi instantiate edilemezler. (Yani `new` kullanarak onları çalıştıramayız.)

Peki bu durumda ne olacak?

`Dependency Injection Konteynerleri` bu durumda bizim yardımımıza koşuyor. Bu konteynerler, interfaceleri gördüklerinde belirttiğimiz sınıfı oluşturup bizim yerimize döndürecekler.

// Yakında burayı anlatacağım, anlaşılabilir şekilde örnekler yazmam lazım.

> Not: Dependency Injection Konteynerlerinin avantajları bunlarla sınırlı değil. Detaylı bilgiye sahip olmak isteyen arkadaşlar Google üzerinde araştırma yapabilir.

### Gerekmedikçe else, uzun if blokları ve köşeli parantez kullanmayın. ###

Bazen, 7-8 satırlık if/else bloklarını tek satırda bile yazabilirsiniz. Bunun için bir örnek verelim. 

Bir değişkenin array olup olmadığını anlamak istediğiniz bir fonksiyon yazıyorsunuz. (Aslında bu örnek is_array ile yapılabilir ancak ben burada soruna değinmek istediğim için fonksiyon oluşturarak anlatacağım.)

Bu fonksiyon aşağıdaki şekilde yazılabilir.

```php
<?php

    function isArray($input)
    {
        if(is_array($input)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
```

Ama `return` kullanmak fonksiyonu zaten durduracağı için `else` kullanmaya gerek kalmadan şu şekilde yazılabilir.

```php
<?php

    function isArray($input)
    {
        if(is_array($input)
        {
            return true;
        }
  
        return false;
    }
```

If bloğu içerisindeki ilk satır daima if bloğu içerisinde sayılacağı için, köşeli parantezleri de silebiliriz.

```php
<?php

    function isArray($input)
    {
        if(is_array($input)
            return true;
  
        return false;
    }
```

Bunu biraz daha geliştirip, ternary yapısını da kullanabiliriz.

```php
<?php

    function isArray($input)
    {
        return is_array($input) ? true : false;
    }
```

Çok daha kısa, okunaklı ve şık duruyor.

Dilerseniz, ternary operatörünün ilk çıktısı olan `true` yi de silebilirsiniz.

```php
<?php

    function isArray($input)
    {
        return is_array($input) ?: false;
    }
```

### Kod yaz, tarayıcıya dön, F5'e bas, hata var mı? Yok, devam et. ###

`DRY` bölümünde anlattığım konuya bir örnekte bu. PHP geliştiricilerinin %99'u bu şekilde çalışıyor (ve bu normal) ama yanlış. Neden yanlış olduğunu `DRY` bölümünde anlatmıştım. Bilgisayarın yapması gereken şeyleri siz yapmayın.

### Testlerinizi yazarken string kullanmamaya çalışın. ###

// Yakında

### Headerler hakkında bilgi sahibi olun. ###

// Headeri 404 olmayan 404 sayfası, 404 sayfası değildir.

### MVC kullanıyorsanız, MVC gibi kullanın. ###

**echo'yu unutun**

Yakında.

**Viewlar, Controllerdan gelen mümkün olan en az bilgiyle çalışmalıdır.**

Yakında.

## Frontend ##

**CSS**

// CSS Explosiondan korunmak

**Semantic HTML yazmak**

// Divider kullananlar falan var ya, hepsi yanlış

**Sass**

**Browser compatibility**

// Browserstack, caniuse, shim/modernizer vs

**Tips&Tricks**

// Opacity, alpha, js prefixleri, reuseable css, SVG, 3D animasyonlar

**Ne kadar az, o kadar şık.**

**Javascript**

// Yakında

## Dev Environment ##

**Grunt**

// Ayak işlerini yaptıralım

// Kendi modülümüzü yazalım

**Kullanabileceğiniz araçlar**

// Yakında

**Kullanmamanız gereken şeyler**

// DW

**Gözlerinizi koruyun**

// Yakında

**Terminale ne kadar yakın, o kadar iyi**

// Farenizi değil, klavyenizi hızlandırın

## Veritabanları ##

**Neden utf8mb4_unicode_ci?**

// Yakında

## Deployment ##

**Capistrano**

// Yakında

**FTP kullanmayın!**

// Sebepleri ve alternatif kullanımları

## Versiyon kontrol ##

**Sus ve git kullan**

// Kaçışın yok

> Not: Bu makale vakit buldukça güncellenecektir. Eklenmesini istediğiniz konuları issue oluşturarak bildirebilirsiniz.

