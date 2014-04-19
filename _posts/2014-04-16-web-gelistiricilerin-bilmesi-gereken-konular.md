---
layout: post
title: "Web geliştiricilerin bilmesi gereken konular"
description: "Web geliştiricilerin bilmesi gereken konular"
modified: 2014-04-16
tags: [PHP]
image:
  feature: abstract-12.jpg
comments: true
share: true
---

Merhaba,

Uzun zamandır bloguma yazı yazmıyordum, ancak neredeyse hergün sosyal platformlarda aynı hataların ve yanlış düşüncelerin tekrarlandığını gördüğüm için bu yazıyı yazmaya karar verdim.

Bu yazımda web geliştiricilerin bilmesi gereken konulardan bahsedeceğim. Herşeyi bilmeniz şart değil, ancak biliyor olmanız size birçok konuda avantaj sağlayacak.

Yazı boyunca web geliştirme süreçlerinin tamamına değinmek istiyorum. Bu yüzden geliştirme ortamı, veritabanları, backend, frontend, css, otomasyon, deployment, versiyon kontrol vb. gibi konularda bilmeniz gerekenleri anlatmaya çalışacağım. Bazı konular son derece temel konular olabileceği gibi, bazıları da üst düzey olabilir.

Bu yazıdan hiçbir ticari beklentim yoktur ve olmayacaktır. (Bağış yapmak isteyen arkadaşlar isterlerse yapabilirler.)

Bu yazı açık kaynaklı olarak Github hesabım üzerinde yayınlanmaktadır. Açık kaynaklı olduğu için daima güncel tutmayı planlamaktayım. Eğer herhangi bir yanlışlık görürseniz veya eklemek istedikleriniz olursa `pull request` atabilirsiniz. Merak ettiğiniz konular varsa bu konuları da `issue` oluşturarak belirtebilirsiniz. Kaynak [https://github.com/Aristona/aristona.github.io]() üzerindeki `repository` (ambar) üzerinde tutulmaktadır.

## Backend (Arka yüz) ##

Bu bölümde backend için kullanacağımız ana programlama dili `PHP` olmakla beraber, birçok örnek yazılım mimarisiyle ilgili olduğu için diğer programlama dillerinde de kullanılabilir.

### Global scopeyi asla kirletmeyin. ###

**a. Değişkenlerinizi global scope içerisinde tanımlamayın.**

`Global scope` (Global alan) içerisinde tanımladığınız değişkenler uygulamanızın her tarafından erişebilir olur. Global scope içerisindeki değişkenlere ne kadar bağımlı olursanız uygulamanın biryerinde hata yapma olasılığınız o kadar artar. Özellikle 3. parti pluginleri, komponentleri veya kütüphaneleri kullandığınızda, onların uygulamanızı kötü etkilemeyeceğinden emin olamazsınız.

Öncelikle, PHP'de `scope`'un ne olduğundan kısaca bahsedelim.

```php
<?php
	$global = "Global scope içerisindeki değişken";

    function ()
    {
         $fonksiyon = "Fonksiyon scope içerisindeki değişken";
         echo $fonksiyon; // Çıktı: Fonksiyon scope içerisindeki değişken
    }

    echo $global; // Çıktı: Global scope içerisindeki değişken
    echo $fonksiyon; // Çıktı: Hiçbirşey
```

Yukarıdaki örnekte `$global` değişkeni `global scope` içerisinde tanımlanmıştır ve uygulamanın heryerinden erişilebilir olur. Buna karşın `$fonksiyon` değişkeni `fonksiyon scope` içerisinde tanımlanmıştır ve sadece o fonksiyon içerisinden erişilebilir olur.

Şimdi değişkenleri global olarak tanımlamak neden yanlıştır bunu inceleyelim.

Örneğin, bir `$veritabani` değişkeninde mysql bağlantısını tuttuğumuzu farzedelim.

```php
<?php
	$veritabani = //bir mysql bağlantısı;
```

Bu global scope içerisinde tanımlanmış bir değişken olduğu için, bir başkası:

```php
<?php
	$veritabani = null;
```

yazdığında uygulamanızı runtime esnasında bozabilir. Uygulamanın geri kalanında `$veritabani` değişkeni `NULL` değerine sahip olacağı için hiçbir veritabanı işlemi yapılamaz hale gelecektir.

Bundan korunmak için, tanımlayacağınız değişkenleri `class scope` (sınıf scope) altında tanımlayın. Sınıflarınız da `namespace` altında tanımlanmış olsun.

**b. Kapsüllenecek şeyleri kapsülleyin, açıkta bırakmayın.**

`Class scope` (sınıf scope) içerisinde tanımladığınız değişkenler de dışarıdan erişime açık olurlar, bu yüzden yukarıdaki örnek tek başına yeterli olmaz. 

Siz herşeyi sınıf scope içerisinde yazmış olabilirsiniz, ancak sınıflar uygulamanın herhangi biryerinde instantiate edilebilir (Türkçe'si `başlatılabilir` gibi birşey olmalı?) ve buradan içeriğe direkt müdahele edilebilir.

Bunu önlemek için `OOP`in (Nesne Yönelimli Programlama) temellerinden olan `Encapsulation` (Kapsülleme) özelliğini kullanabilirsiniz.

Kapsülleme son derece basit bir mantıkla çalışır. PHP'de bildiğiniz gibi `public`, `private` ve `protected` keywordleri ile bir değişkenin veya fonksiyonun dışarıdan erişilip erişilemeyeceğini belirtebilirsiniz. Kapsülleme yapmak için, erişimine izin vermek istemediğiniz bir değişkeni `private` veya `protected` ile tanımladıktan sonra, class içerisinde `public` bir fonksiyon tanımlayıp bu fonksiyon üzerinden değişkeni döndürebilirsiniz.

Bunu hemen bir örnekle anlatalım. Örneğin `abstract` (soyut) bir veritabanı sınıfınız var ve bu sınıf bir başka sınıfın onu `extend` etmesiyle çalışacak.

```php
<?php

abstract class Database
{

    public $info;
    private $isConnected;

    public function __construct()
    {
        $this->info = "Son derece gizli bilgi.";
        $this->isConnected = false;
    }

    public function attempt()
    {
        //Veritabanına bağlanmayı dene
    }

    public function connect()
    {
        if( $this->attempt($this->getConnectionString()) );
        {
            $this->info = "Veritabanına bağlandık!";
            $this->isConnected = true;
            return true;
        }

        $this->info "Veritabanına bağlanamadık.";
        $this->isConnected = false;
        return false;
    }

    public function isConnected()
    {
        return $this->isConnected;
    }
}
```

Dikkat ettiyseniz `info` değişkeni `public` olarak, `isConnected` değişkeni `private` olarak yazıldı.

Şimdi, bir `MySQL` sınıfı veritabanı sınıfımızı extend etsin ve onu başlatsın.

```php
<?php

class MySQL extends Database implements DatabaseConnectorInterface
{

    public $queryString;

    public function __construct()
    {
        $this->queryString = "Mysql DB query string";
    }
     
    public function getConnectionString()
    {
        return $this->connectionString;
    }

    public function getInfo()
    {
        return $this->isConnected; // Hata, erişim yasak.
        return $this->isConnected(); //Doğru, fonksiyon üzerinden erişebiliyoruz
    }
}
```

Şuan `MySQL` sınıfı, `Veritabanı` sınıfındaki `isConnected` değişkenini değiştiremez çünkü yetkisi yok. Neden? Değişkeni `private` olarak tanımladığımız için sadece `Database sınıfı scopesi` içerisinden erişilebilir. Ancak biz şuan `MySQL sınıfı scopesi` içerisindeyiz.

Ancak, `public` olarak bir `isConnected()` methodu yazmıştık ve bu method içerisinde `info` değerini döndürmüştük. `MySQL` sınıfı bu methoda erişebilecek mi? Evet. Database sınıfının kendi scopesi içerisindeki `isConnected` değişkenine erişim hakkı var mı? Evet. O zaman bu şekilde gizli değişkenlere dışarıdan erişim verebiliyoruz ancak onları değiştirme hakkı tanımıyoruz. Bunun adına kapsülleme deniyor ve Nesne Yönelimli Programlama'nın 4 temel prensiplerinden biridir.

Şuana kadar kafanız karışmış olabilir ancak bu konuyu size bir dünyevi örnekle anlattığım zaman herşey kafanızda şekillenecek. 

Örneğin, bir `KDV hesaplayıcı` sınıf yazıyorsunuz ve KDV oranını `0.18` olarak belirlediniz. Eğer siz bunu dışarıdan erişilebilir yaparsanız, başka birisi bunu `3.00` olarak değiştirebilir. Değiştirdiğinde ne olur? 100 liralık ürünü alacak kullanıcıdan `%18` yerine `%300` vergi çekmiş olursunuz. (Kendinizi şirketin kapısının önünde bulmak için yeterli bir sebep.)

Buna rağmen, bazı durumlarda `KDV oranı` bilgisine erişmeniz gerekebilir. Belki başka bir yazılımcı ürünün KDV'li fiyatının ne olduğunu hesaplattırmak istiyor ve bu değişkene erişmesi lazım. Burada biraz önce bahsettiğimiz `public fonksiyon` mantığı giriyor. Bu tür fonksiyonlara `gettler fonksiyonlar` denmekle beraber, yaptıkları iş gizli değişkeni döndürmekten ibarettir.

```php
<?php

class KDVCalculator 
{

    private $kdvRatio = 0.18;

    public function getKdvRatio() // bir gettler örneği
    {
        return $this->kdvRatio;
    }
}
```

`settler fonksiyonlar` ise, `gettler fonksiyonların` tam tersi mantıkla çalışır. Onlar, gelen değeri değişkenin değeri olarak atarlar.

Bir örnekle gösterelim:

```php
<?php

class KDVCalculator 
{

    private $kdvRatio = 0.18;

    public function setKdvRatio($input) // bir settler örneği
    {
        return $this->kdvRatio = $input;
    }
}
```

Gördüğünüz gibi `settler` bir fonksiyon üzerinden gizli olan `kdvRatio` değişkeninin değeri güncellenebiliyor.

> Biliyor musunuz?

`Csharp` dilinde gettler ve settler methodlar kolayca oluşturulabilmektedir.

```php
public class Database
{
    public string info { get; set; } //gettler ve settler oluşturuldu
}
```

> Biliyor musunuz?

`Ruby` dilinde gettler ve settler oluşturmak çok basittir.

```ruby
class Database
    attr_accessor :info //gettler ve settler oluşturuldu
end
```

> Biliyor musunuz?

PHP'de eğer `gettler` ve `settler` methodlar bulunamazsa, PHP'nin `sihirli method`larından olan `__get()` ve `__set()` devreye girer.

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

> Not: Geliştirme ortamında yapabileceğiniz iyileştirmelerden Geliştirme Ortamı bölümünde bahsedeceğim.

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

// Sınıf scopesi dışında, uzaklarda biryerlerde
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

class Mailer implements MailerInterface
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

Son olarak, `Mailer` sınıfı yerine, `MailerInterface` interfacesini enjekte edelim.

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
     private $mailer;

     public function __construct(MailerInterface $mailer)
     {
         $this->mailer = $mailer;
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

Dilerseniz, ternary operatörünün ilk çıktısı olan `true` yi bile silebilirsiniz.

```php
<?php

    function isArray($input)
    {
        return is_array($input) ?: false;
    }
```

### Kod yaz, tarayıcıya dön, F5'e bas, hata var mı? Yok, devam et. ###

`DRY` bölümünde anlattığım konuya bir örnekte bu. PHP geliştiricilerinin %99'u bu şekilde çalışıyor (ve bu normal) ama yanlış. Neden yanlış olduğunu `DRY` bölümünde anlatmıştım. Bilgisayarın yapması gereken şeyleri siz yapmayın.

Oluşturduğunuz form submite tıkladığında form verilerini uygulamaya gönderiyor mu? Güzel, ama bunu `acceptance testi` yazarak test edin.

Formdan veri gönderildiğinde, gelen istek doğru yere yönlendiriliyor mu? Güzel, ama bunu `unit testi` yazarak kontrol edin.

Formdan gelen tc kimlik no verisini doğrulayan method doğru çıktıları veriyor mu? Güzel, ama bunu `fonksiyonel test` yazarak kontrol edin.

Bunu yapmadığınız zaman projenin herhangi biryerinde değişiklik yapmaya korkar olursunuz. Proje ne kadar büyürse projeyi manuel test etmek o kadar zorlaşır ve hata oranı büyük ölçüde artar.

Ayrıca, benim gibi projeden projeye atlayan biriyseniz, hangi projenin ne durumda olduğunu aklınızda tutamayacaksınız.

Bu yüzden test yazın, kafanız rahat etsin.

> Not: PHPUnit, Codeception, Selenium, Behat gibi araçları araştırın.

### Testlerinizi yazarken string kullanmamaya çalışın. ###

İster unit test yazıyor olun, ister acceptance, ne kadar az string kontrol ederseniz sizin için o kadar sağlıklı olur.

Mesela acceptance testlerinde `<p>Anasayfa</p>` varmı diye kontrol etmektense header kodunun 200 OK olduğunu kontrol edin. Anasayfa yazısı değişebilir (Anasayfa yerine Font Awesome ile ev resmi koyabilirsiniz örneğin) ama header kodu değişmez.

### HTTP Response kodlarını öğrenmeye ve kullanmaya çalışın. ###

HTTP Response kodları önemlidir. Örneğin, headeri 404 olmayan bir 404 sayfası, gerçek bir 404 sayfası değildir. Google gibi arama motorları bu sayfayı 404 olarak saymazlar çünkü bu sayfanın aslında 404 ID'sine sahip kullanıcının profili olup olmadığını bilemezler.

Tüm HTTP Response kodlarına http://en.wikipedia.org/wiki/List_of_HTTP_status_codes adresinden ulaşabilirsiniz.

### MVC kullanıyorsanız, MVC gibi kullanın. ###

**Controller içerisinde echo'yu unutun**

`Controller` içerisinde asla `echo` veya `print` gibi fonksiyonları kullanmayın. Çıktıyı ekrana bastıracak katman daima `View` katmanıdır.

Örnek olarak aşağıdaki scripti ele alabiliriz.

```php
<?php

class Controller
{
  
    public function test()
    {
        $test = "Birşey";

        echo $test; //yanlış
        print $test; //yanlış

        return $view->bind('test', $test); //doğru
        return $view->render('birsey.php', array('test' => $test)); //doğru
        return $test; //doğru, dönen veri mutlaka view katmanına ulaşacaksa.
    }
}
```

**Viewlar, Controllerdan gelen mümkün olan en az bilgiyle çalışmalıdır.**

Bazen `Controller` sınıfları `View`'e gereğinden fazla veri gönderir ve bu sıkıntı çıkarabilir.

Örneğin, veritabanındaki tablolara göre navigasyon menüsünün linklerini hazırlamakla yükümlü bir `Controller methodu`, View'e mümkün olan en az veriyi göndermelidir.

Örneğin, linki oluştururken kullandığımız site adresi gibi bilgiler, View'de bulunmalı, Controller sadece linkin değişen kısmını göndermelidir.

### Çıkan notice ve warningler birer bugdur ve düzeltilmesi gerekir. ###

PHP çok katı kurallara sahip değildir bu yüzden ufak çaplı basit hatalar bazen görmezden gelinebilir. Bunlar `E_DEPRECATED`, `E_STRICT`, `E_NOTICE` ve `E_WARNING`'dir. (Hepsine http://php.net/manual/en/errorfunc.constants.php adresinden bakabilirsiniz.)

1. `E_DEPRECATED` kullanılan bir özelliğin ileride PHP'den kaldırılacağını belirtir. (örneğin `mysql` ile başlayan fonksiyonlar)

2. `E_STRICT` yazılan scriptin hatalı olabileceğini ve sıkı kurallara uyulmasını gerektiğini belirtir.

3. `E_WARNING` çok tehlikeli olabilecek hataların bırakıldığını belirtir.

4. `E_NOTICE` warning kadar olmasa da, düzeltilmesi gereken hataları belirtir.

Bu yüzden geliştirme ortamınızda hata raporlamanın açık durumda olması ve hertürlü hata seviyesindeki sorunları çözüyor olmanız sizin için bir avantajdır.

### Hataları analiz etmek için backtrace kullanın. ###

Uygulamanızda bir hata aldınız, ancak neden böyle bir hata aldığınızı bilmiyorsanız PHP'in `backtrace` (geri sarma) özelliğini kullanabilirsiniz.

PHP, siz istemedikçe hatalar hakkında stack trace bilgisi vermez. Sadece `X dosyasının 55. satırında hata oluştu` der.

Stack trace hakkında bilgi sahibi olmak için:

1. Whoops adlı paketi (http://filp.github.io/whoops/) uygulamanızda kullanabilirsiniz. (önerilen)

2. `debug_backtrace()` fonksiyonunu kullanarak stack traceyi kendiniz takip edebilirsiniz.

### @ kullanmayın. ###

`@`, PHP'de muhtemel hataların ekrana yansımaması için onları sessizleştirir. Siz istediğiniz hataları sessizleştirin, susturun, onlar oluşmaya devam edecektir.

Açıkcası bu özelliğin PHP'ye neden eklendiğini anlamış biri değilim. Ölümcül hatalar, warningler, noticeler, exceptionlar, trigger_error ve bunların fonksiyonları ile php.ini konfigürasyonları derken zaten yeterince kafa karışıklığı oluşuyor. Birde bunları susturacak (hepsini değil) özellikler var. Neden diye soramıyorum... çünkü PHP kullanıyoruz, bunlara alıştık. ¯\_(ツ)_/¯

### ?> kullanmayın. ###

PHP kullanırken `?>` kullanarak açılan tagları kapatmanıza gerek yok. Kapattığınız takdirde `?>` tagından sonra yeni satır veya boşluk gibi karakterler kalabiliyor. Bu karakterler PHP tarafından `output` (çıktı) olarak algılandığı için `Headers already sent` hatası, sessionların oluşturulamaması, header kodlarının değiştirilememesi gibi hatalara sebep oluyor.

Ben hiçbirşeyi gözden kaçırmam demeyin, kaçabiliyor. 

> Not: Kaçabiliyordan sonra birtane whitespace kaçtı mesela. Farkettin mi?

### Short tags kullanmayın. ###

Bazı yazılımcılar `<?php` yerine `<?` kullanıyor. Bu son derece yanlış. Short tag kullanacaksanız `short_open_tag` mutlaka açık konumda olmalı. Kapalı olursa ne olur? Hiç. Kaynak kodlarınız kabak gibi tarayıcıya çıkar.

Bu hatayı malesef devasa kurumlar bile yapmakta. 2011 yılında `sahibinden.com`'u geliştiren bir yazılımcı `<?php` yerine `a?php` (<'in bir üstündeki tuş) yazdı diye `sahibinden.com`'un tüm kaynak dosyasını tarayıcıda gösterdi ve bu bilgiler arasında duyduğum kadarıyla veritabanı şifresinden ve FTP bilgileri bile vardı.

Sadece bu değil, dünya çapında birçok web sitesi bu tür basit dikkatsizliklerin kurbanı olabiliyor ve oldu da.

Bu yüzden, `<?php` dışında hiçbir açılış tagını kullanmayın.

### Projelerinizi açık kaynaklı olarak paylaşıyorsanız, .gitignore kullanın! ###

Yanlışlıkla sunucu, ftp, veritabanı veya API bilgilerinizin olduğu dosyaları Github'a yüklemeyin. Gizli kalması gereken dosyaları `.gitignore` kullanarak gizleyin.

Commit logları kaldığı için daha sonra silseniz bile başkaları tarafından görünebiliyorlar. Dikkatli ve uyanık olun.

### Projelerinizi açık kaynaklı olarak paylaşıyorsanız, güvenli olduklarından emin olun. ###

Aşağıda mükemmel yazılımcıların... (*öhüöhü*) açık kaynaklı projeleri... (*öhüöhü*) Kusura bakmayın, biraz kötü oldum. Bu kadar kötü kod görünce bünyem kaldırmıyor. 

Durumun vehametini size tek bir linkle göstereceğim.

https://github.com/search?q=exec+sudo+%24_GET&type=Code

Sorunu anlamayanlar varsa anlatmaya çalışayım. Buradaki scriptlerin bazıları, gelen `GET` isteğini kontrol etmeden direkt olarak linux adminin yetkisiyle çalıştırıyorlar. Yani birisi "Hey linux, bana sunucunun şifresini verir misin?" diye sorabilir veya "Hey linux, kendine format atar mısın?" tarzındaki komutları çalıştırabilir.

Bazıları session onları korur diye düşünebilir, ama session spoofing, session hijacking gibi saldırılara maruz kalabilirsin veya cookilerin çalınabilir. Bu durumda ne yapacaksın? Session beni korur demek nasıl olsa kimse beni hacklemez diye SQL Injection açığı bırakmakla aynı şeydir.

Kullanıcıya asla güvenmeyin. Kontrol etmeden hiçbirşeyi shell veya veritabanı sorgusuna sokmayın.

### Ekrana bastırılacak verileri daima filtreleyin. ###

Kullanıcıdan gelen verileri (veritabanına eklenmiş olanlar dahil) ekrana bastırırken daima filtreleyin. Bu filtreleme sizi XSS saldırılarından korur.

Örneğin:

```js
<script>alert("Selam");</script>
```

yazdığım zaman bu input veritabanına girecektir. Daha sonra bu veri ekrana bastırılacaktır ve eğer javascript tarayıcıda açıksa, çalışacaktır.

Bu yüzden ekrana bastırma esnasında filtreden geçirilerek zararsız hale gelmesi gerekmektedir.

### Composer kullanın. ###

// Yakında

### Statik fonksiyon kullanmayın. ###

// Yakında

## Frontend ##

**--- CSS ---**

**CSS Explosiona maruz kalmayın.**

**Precompiler kullanın.**

**Semantic HTML yazın.**

**Ayraçları HTML ile yazmayın.**

**Duplicate HTML yazmayın.**

**Linkleri doğru şekilde yazın.**

Linkler yazılırken `www` yazılmamalı ve slash ekli olmalıdır.

Örneğin `http://www.anilunal.com` hatalıyken, doğru olan `http://anilunal.com/`'dur.

**Tarayıcı uyumluluğunu test edin.**

// Browserstack, caniuse, shim/modernizer vs

**Fazla opacity ve alpha kullanmayın.**

**ID seçicilerini gerekmedikçe kullanmayın.**

**--- Javascript ---**

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

**Debug için alert kullanmayın, lütfen!**

**Hoisting hakkında bilgi sahibi olun.**

**Tanımlamaları lambda altında yapın.**

**Selector kullandığınız zaman onu önbellekleyin.**

## Geliştirici Ortamı ##

**--- Grunt ---**

**Ayak işlerini yaptıralım.**

**Kendi modülümüzü yazalım.**

**Kullanabileceğiniz diğer araçlar.**

**Kullanmamanız gereken araçlar.**

// DW

**Gözlerinizi koruyun.**

// Yakında

**Terminale ne kadar yakın, o kadar iyi.**

**Farenizi değil, klavyenizi hızlandırın.**

## Veritabanları ##

**Neden utf8mb4_unicode_ci?**

// Yakında

## Deployment ##

**Capistrano**

// Yakında

**FTP kullanmayın!**

// Sebepleri ve alternatif kullanımları

## Versiyon kontrol ##

**Sus ve git kullan.**

// Kaçışın yok

**Commit mesajlarınız açıklayıcı olsun.**

## Hosting ve sunucu ##

**Neden PaaS?**

**Shared hostingler artık öldü.**

> Not: Bu makale vakit buldukça güncellenecektir. Eklenmesini istediğiniz konuları issue oluşturarak bildirebilirsiniz.