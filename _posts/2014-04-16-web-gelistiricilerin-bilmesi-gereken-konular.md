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

Bu yazımda web geliştiricilerin bilmesi gereken konulardan ve good practice kullanımlardan bahsedeceğim. Yazı boyunca web geliştirme süreçlerinin tamamına değinmek istiyorum. Bu yüzden geliştirme ortamı, veritabanları, backend, frontend, css, otomasyon, deployment, versiyon kontrol vb. konularda bilmeniz gerekenleri anlatmaya çalışacağım. Anlattığım bazı konular son derece temel konular olabileceği gibi, bazıları da üst düzey olabilir. Herşeyi bilmeniz şart değil, ancak biliyor olmanız size birçok konuda avantaj sağlayabilir.

Bu yazı, açık kaynaklı olarak `Github` hesabım üzerinde yayınlanmaktadır. Açık kaynaklı olduğu için kolayca güncel tutmayı planlamaktayım. Eğer herhangi bir yanlışlık görürseniz veya eklemek istedikleriniz olursa `pull request` atabilirsiniz. Merak ettiğiniz konuları da `issue` oluşturarak belirtebilirsiniz.

Bu yazı, [https://github.com/Aristona/aristona.github.io]() üzerindeki `repository` (ambar) üzerinde tutulmaktadır. Format olarak `Markdown (Redcarpet)` kullanılmıştır, bu yüzden `pull request` attığınızda, yazılarınızın bu formata uygun olması gerekmektedir.

Bu yazıdan hiçbir ticari beklentim yoktur, ancak bağış yapmak isterseniz `PayPal` hesabım üzerinden bağış yapabilirsiniz.

---
# Backend (Arka yüz) #
---

Backend için kullanacağımız ana programlama dili `PHP` olmakla beraber, birçok örnek direkt olarak `yazılım mimarileri` ile ilgili olduğu için diğer programlama dillerinde de kullanılabilir.

Bu bölümdeki örneklerin bazıları, temel veya orta düzeyde `PHP` bilgisi gerektirmektedir.

### - Global scopeyi asla kirletmeyin. ###

**a. Değişkenlerinizi global scope içerisinde tanımlamayın.**

`Global scope` (Global alan) içerisinde tanımladığınız değişkenler uygulamanızın heryerinden erişebilir olurlar. `Global scope` içerisindeki değişkenlere ne kadar bağımlı olursanız, uygulamanın farklı bir noktasında hata yapma olasılığınız o kadar artar. Özellikle 3. parti pluginleri, komponentleri veya kütüphaneleri kullandığınız zaman, onların uygulamanızı kötü etkilemeyeceğinden emin olamazsınız.

İsterseniz başlamadan önce `scope` kavramının ne olduğundan ve `PHP`'de nasıl kullanıldığından kısaca bahsedelim.

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

Yukarıdaki örnekte `$global` değişkeni `global scope` içerisinde tanımlanmıştır. Bu yüzden uygulamanın heryerinden erişilebilir olur. Buna karşın, `$fonksiyon` değişkeni `fonksiyon scope` içerisinde tanımlanmıştır ve sadece o fonksiyon içerisinden erişilebilir olur.

Bunu belirttiğimize göre, değişkenlerin `global` olarak tanımlanması neden yanlıştır bunu inceleyelim.

Bir `$veritabani` değişkeninde `MySQL` bağlantısını tuttuğumuzu varsayalım;

```php
<?php
	$veritabani = //bir mysql bağlantısı;
```

Görüldüğü gibi, `$veritabani` değişkeni `global scope` içerisinde `global` olarak tanımlanmıştır. Ancak, uygulamanın herhangi bir yerinde bir başkası;

```php
<?php
	$veritabani = null;
```

yazdığı zaman, artık `$veritabani` değişkeni `NULL` değerine sahip olacağı için hiçbir veritabanı işlemi yapılamaz hale gelecektir. Bu durumda uygulamanız `runtime esnasında` bozulacaktır.

Bu tür hatalardan korunmak için tanımlayacağınız değişkenleri `class scope` (sınıf scope) altında, sınıflarınızı da `namespace` (isim uzayları) altında tanımlamaya çalışın.

**b. Encapsulation (Kapsülleme) yapın, gizli değişkenlere erişimi kesin.**

`Class scope` (sınıf scope) içerisinde tanımladığınız değişkenler de dışarıdan erişime açık olurlar. Bu yüzden yukarıdaki örnek tek başına yeterli olmaz. Siz herşeyi `class scope` içerisinde yazmış olabilirsiniz, ancak sınıflar uygulamanın herhangi biryerinde `instantiate edilebilir` (çalıştırılabilir) ve buradan değişkenlere erişim sağlanabilir.

Bunu önlemek için, `OOP`in (Nesne Yönelimli Programlama) 4 temel ilkesinden biri olan `Encapsulation` (Kapsülleme) özelliğini kullanabiliriz.

Bildiğiniz gibi `PHP`'de `class scope` içerisindeyken `public`, `private` ve `protected` prefixlerini kullanarak değişkenlerin veya fonksiyonların dışarıdan erişilip erişilemeyeceğini belirtebiliyoruz. `Kapsülleme` yapmak için erişimine izin vermek istemediğiniz bir değişkeni `private` veya `protected` prefixlerini kullanarak oluşturduktan sonra, sınıf içerisinde `public` bir fonksiyon oluşturup, oluşturulan fonksiyon üzerinden gizli değişkeni döndürebiliriz.

Bunu hemen bir örnekle anlatalım. Örneğin, `abstract` (soyut) bir veritabanı sınıfınız var ve bu sınıf bir başka sınıfın onu `extend` etmesiyle çalışacak.

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

Dikkat ettiyseniz `$info` değişkeni `public` olarak, `$isConnected` değişkeni ise `private` olarak yazıldı. Yani, `$isConnected` dışarıdan erişilemez hale getirildi.

Şimdi, bir `MySQL` sınıfı oluşturalım. Bu sınıf `Database` sınıfımızı extend ederek başlatıyor olsun.

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

Şuan `MySQL` sınıfı, `Database` sınıfındaki `$isConnected` değişkenine erişemeyecektir, çünkü değişken `private` olarak tanımlandığı için sadece `Database sınıfı scopesi` içerisinden erişilebilir olacaktır. Biz ise `MySQL sınıfı scopesi` içerisine olduğumuza göre bu değişkene erişme hakkımız bulunmamaktadır.

Ancak, `public` olarak bir `isConnected() methodu` yazmıştık ve bu method içerisinde `$info` değişkeninin değerini döndürmüştük. Prefix olarak `public` kullanıldığı için `MySQL` sınıfı, `Database` sınıfındaki `isConnected()` methoduna erişebilecektir.

Bu durumda;
 
1. `MySQL` sınıfı `isConnected()` methoduna erişebilecek mi? Evet. 
2. `isConnected()` methodunun kendi sınıfı içerisindeki `$isConnected` değişkenine erişim hakkı var mı? Evet. 

Artık `MySQL` sınıfı, `Database` sınıfındaki `public` olan `isConnected()` fonksiyonu üzerinden gizli `$isConnected` değişkenine ulaşabilecektir. Buna `encapsulation` (Kapsülleme) denmektedir ve `Nesne Yönelimli Programlama`'nın 4 temel ilkelerinden biridir.

Şimdi, bu durumu size daha dünyevi bir örnekle anlatmaya çalışayım. 

Örneğin, bir `KDV hesaplayıcı` sınıf yazıyorsunuz. `$kdvOrani` adında bir değişken belirlediniz ve değer olarak `0.18` float değerini verdiniz. Eğer siz `$kdvOrani` değişkenini dışarıdan erişilebilir yaparsanız, başka birisi bunu `3.00` olarak değiştirebilir. Değiştirdiğinde ne olur? `100` liralık ürünü alacak kullanıcıdan `%18` yerine `%300` vergi çekmiş olursunuz. (Kendinizi şirketin kapısının önünde bulmak için yeterli bir sebep.)

Ancak, `MySQL` örnediğinde anlattığım gibi, bazı durumlarda `$kdvOrani` değişkenine erişmeniz gerekebilir. Belki bir ürünün KDV'li fiyatının ne olduğunu hesaplattırmak istiyorsunuz. Kim bilir? Bu durumda biraz önce bahsettiğimiz `public fonksiyon`  üzerinden gizli değişkenlere erişme mantığı giriyor. Bu tür erişim sağlayıcı fonksiyonlara `gettler fonksiyonlar` denmekle beraber, yaptıkları iş gizli değişkenin değerini döndürmekten ibarettir.

```php
<?php

class KDVHesaplayici 
{

    private $kdvOrani = 0.18;

    public function getKdvOrani() // bir gettler örneği
    {
        $this->kdvOrani; // 0.18 dönüyor
    }
}
```

Bir de bunun tam tersi mantıkla çalışan `settler fonksiyonlar` vardır. Bu fonksiyonlar, gelen değeri değişkenin değeri ile değiştirmekle yükümlüdürler.

Bir örnekle gösterelim;

```php
<?php

class KDVHesaplayici 
{

    private $kdvOrani = 0.18;

    public function getKdvOrani() // bir gettler örneği
    {
        return $this->kdvOrani;
    }

    public function setKdvOrani($input) // bir settler örneği
    {
        $this->kdvOrani = $input;
    }
}

$kdv = new KDVHesaplayici;
$kdv->setKdvOrani(3);
$kdv->getKdvOrani(); // 3.00
```

`Settler` fonksiyonların iyi yanı, kendi içlerinde bir kontrol mekanizması kurabilmeleridir. Mesela `setKdvOrani()` fonksiyonu içerisinde vergi oranının `0.20`'den fazla olamayacağını kontrol ettirebilirsiniz.

```php
<?php

    public function setKdvOrani($input) // bir settler örneği
    {
        if( (float) $input > 0.20)
        {
           return false;
        }

        $this->kdvOrani = $input;
    }
```

Artık dışarıdan müdahele edilerek bozulamayacak bir sınıf yapısına sahibiz, ve biz istemedikçe kimse uygulamamızı bozamaz.

> Biliyor musunuz?

`Csharp` dilinde `gettler` ve `settler` methodlar kolayca oluşturulabilmektedir.

```php
public class Database
{
    public string info { get; set; } //gettler ve settler oluşturuldu
}
```

> Biliyor musunuz?

`Ruby` dilinde `gettler` ve `settler` oluşturmak çok basittir.

```php
class Database
    attr_accessor :info //gettler ve settler oluşturuldu
end
```

`attr_accessor`, Ruby dilinde `info` değişkeninin gettler ve settler fonksiyonlarını otomatik olarak oluşturur. Malesef `PHP`'de böyle bir kullanım bulunmamaktadır. Biz gettler ve settler fonksiyonlarımızı çoğu zaman elle yazmak zorundayız.

Bilmeniz gereken bir başka konu daha var. `PHP`'de eğer `gettler` ve `settler` methodlar bulunamazsa, `PHP`'nin `sihirli method`larından olan `__get()` ve `__set()` devreye girerler.

// Buraya `__get` ve `__set()` hakkında örnekler gelecek.

> Önemli: Fonksiyonlar global scope içerisinde tanımlanan fonksiyonlardır. Methodlar ise sınıf scope içerisinde tanımlanan fonksiyonlardır.

### - Methodlarınızı ve sınıflarınızı küçük tutun. ###

Bu görüş farklı yazılımcılar tarafından farklı algılanmakla beraber, genel kanı aşağıdaki altın kurala uymanın bize avantaj sağlayacağı yönündedir. 

1. Bir methodda maksimum `10 satır kod` bulunmalıdır.
2. Bir sınıfta maksimum `10 method` bulunmalıdır.
3. Bir sınıfta maksimum `4 dependency` (bağımlılık) bulunmalıdır.
4. Bir pakette/komponentte maksimum `15 sınıf` bulunmalıdır.

Peki, nedir bu bağımlılık?

Bağımlılık, oluşturduğumuz sınıfın çalışabilmesi için gerekli olan diğer sınıfların toplamıdır. Bağımlılıklar `use` kullanılarak, `constructor injection` aracılığıyla veya sınıf scope içerisinde bağımlılık sınıflarının `instantiate` edilmesiyle vb. yöntemlerle eklenebilir.

Aşağıdaki örneği ele alalım;

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

Burada, `HomeController` sınıfının 3 `bağımlılığı` bulunmaktadır.

1. `DatabaseInterface` interfacesine sadık kalmış herhangi bir sınıfa bağımlıdır.
2. `Image` sınıfına bağımlıdır.
3. Global `uzaydaki` (namespace) `Logger` sınıfına bağımlıdır.

Eğer bu sayı `4`'ün üzerine çıkarsa, sınıfınız gereğinden fazla iş yapıyor olabilir. Dolayısıyla hem bu sınıfı yönetmek zorlaşır, hem de `SOLID ilkeleri`nin birincisi olan `Single Responsibility Principle` (Tek amaç ilkesi) ilkesini ihlal etmiş oluruz.

`Tek Amaç İlkesi`'ne uymak için, sınıfımızı ve methodlarımızı fazla şişirmemeli ve küçük tutmalıyız. Ayrıca, sınıfımız ile methodlarımızın ne iş yaptığını anlatırken `ve` kelimesini mümkün olduğunca az kullanmalıyız. 

Örneğin, kullanıcının kayıt olması için hayali bir method oluşturduğumuzu farzedelim ve bu methodun ne iş yaptığını kendimize konuşur gibi anlatalım:

1. Kullanıcıdan gelen `$_POST` verilerini alıyoruz VE
2. Bu verileri oluşturduğumuz `Validation` (Doğrulama) kontrollerinden geçiriyoruz VE
3. Kullanıcı avatar resmi yüklediyse, avatar resmini boyutlandırıyoruz VE
4. Boyutlandırılam avatarı isimlendirip bir klasör içerisine yerleştiriyoruz VE
5. Veritabanına bağlanıp kayıt olacak kullanıcının bilgilerini ekliyoruz VE
6. Eğer veritabanı false döndürmüş veya exception fırlatmışsa bunu yakalıyoruz VE
7. Ekrana başarılı veya başarısız olacak bir sayfa çıktısı veriyoruz.

Gördüğünüz gibi bu methodun ne iş yaptığını açıklarken 6 defa `VE` kullandık. Bu yanlış bir kullanımdır. Bu method gereğinden fazla iş yapmaktadır. Burada gerçekleştirilen işlemlerin bazılarını farklı sınıflara veya methodlara dağıtmamız bizim `Tek Amaç İlkesi`'ne sadık kalmamızı sağlayacaktır.

Şimdi biraz düşünelim. Kayıt sınıfı yazdığımıza göre, `Kayıt` sınıfının amacı, kullanıcıyı başarılı bir şekilde veritabanına kayıt ettirmek olmalıdır. `Doğrulama`'ların, avatar resminin yeniden boyutlandırılmasının, yeniden boyutlandırılan resmin bir klasöre yerleştirilmesinin `Kayıt` sınıfıyla bir ilgisi bulunmamaktadır. Bu işlemler farklı sınıflarda yapılmalıdır.

> Not: Bu maddenin bir kural değil, bir görüş olduğunu hatırlatmalıyım. Sayılarda ufak oynamalar olabileceği gibi, çok büyük oynamalar Tek Amaç İlkesi'nden çıktığınız anlamına gelebilir.

### - Kendinizi == yerine === kullanmaya alıştırın. ###

`==`, `loose comparison` yaptığı için sayı olan `0` ile `false`'ı, sayı olan `1` ile `true`'yu eşit sayar. `Loose comparison` PHP'nin doğasında olmasına rağmen, bazı durumlarda gücü elimize almanız gerekebilir. 

Bu durum için hemen bir örnek verelim;

```php
<?php
    strpos('abcde', 'ab');
```

`strpos` fonksiyonu, ikinci parametredeki stringin, 1. parametredeki string içerisinde kaçıncı sırada geçtiğini bulur. Bu örnekte `strpos` fonksiyonu sayı olan `0` değerini döndürecektir. Çünkü, `ab` yazısı, `abcde` yazısının ilk sırasında geçmektedir.

Siz bu fonksiyondan dönen değeri `==` ile kontrol etmeye çalışırsanız, aslında bir sayı olan `0` değeri `false` olarak algılanacağı için farkında olmadan `bug` çıkarmış olacaksınız.

```php
<?php
    if ( strpos('abcde', 'ab') == false)
        return "ab kelimesi abcde içerisinde geçmiyor."; //hatalı
```

Yukarıdaki örnek hatalıdır. `strpos` fonksiyonu `0` döndürmüş, ama bu `0` değeri if koşulu esnasında yanlışlıkla `false` olarak algılanmıştır.

Bu koşul için mutlaka `===` kullanmamız gerekmekteydi. Böylece `0` değeri `false` olarak algılanmamış olacaktı.

```php
<?php
    if ( strpos('abcde', 'ab') === false)
         return "ab kelimesi abcde içerisinde geçmiyor. Gerçekten.";
```

Artık `0` değeri `false` olmadığı için, yazdığımız ufak scriptimiz doğru çalışacaktır.

Açıkcası, ben biraz disiplinli çalışmayı sevdiğim için daima `strict comparison` operatörünü kullanmaktayım. Özellikle `boolean` türündeki değerleri karşılaştırırken mutlaka `strict comparison` operatörünü kullanın.

PHP'de, `boolean` verileri şunlardır: 

1. Sayı olan `0` ve `1`. (0 false, 1 true)
2. Float olan `0.0` ve `1.0`.
3. Boolean olan `true` ve `false`.
4. Boş string ve string olan 0 `"0"`. (daima false)
5. Dolu string. (daima true)
6. `null` değeri. (false)
7. Boş ve dolu `array`. (boş array false, dolu array true.)
8. `Object` (daima true)
9. `Resources` (daima true)

### - Dependency Injection, Dependency Injection Container, Inversion of Control, Liskov's Substitution Principle ve Dependency inversion principle. ###

**a. Dependency Injection**

Kendisine üst düzey bir `PHP geliştirici` diyen herkesin mutlaka bilgi sahibi olması gereken konular olduğu için bu terimlerin ne olduğunu ve hangi amaçla kullanıldıklarını anlatma ihtiyacı hissediyorum.

Öncelikle, `Dependency Injection` (Bağımlılık Enjeksiyonu), `James Shore`'ın dediği gibi: "5 centlik konsept için 25 dolarlık terim kullanılması." sebebiyle, insanın kulağına sanki çok karışık bir konuymuş gibi geliyor. Bu söze ben de katılıyorum çünkü ben de `bağımlılık enjeksiyonu`'nun mantık olarak son derece basit olduğunu düşünenler arasındayım. Hatta, şuana kadar verdiğim örneklerde birkaç defa kullandığım oldu.

`Dependency Injection` için kuralımız son derece basit. Oluşturduğunuz sınıflarda asla `new` kullanmayacağız. `Bağımlı olduğumuz` sınıflar, dışarıda oluşturulup bizim sınıfımıza enjekte edilecekler.

Aşağıdaki örneği ele alalım;

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

Bu örnekte `Dependency Injection` kullanmadık. Bu aslında büyük bir hata. Bağımlı olduğumuz sınıfları (Mailer sınıfı) bu şekilde oluşturursak, `Deneme` sınıfımız `Mailer` sınıfıyla `tightly coupled` (Sıkı Sıkıya Bağlanmış) olur ve `unit testlerimizi` yazmak çok zor, hatta imkansız bir hale gelir. Ayrıca `Decoupling` (Bağlaşımı koparma) ilkesinden uzaklaşmış oluruz.

Ne demiştik? Sınıf içerisinde `new` kullanmayacağız ve `Dependency Injection` yöntemini kullanarak bağımlılıkları dışarıda oluşturup sınıfımıza enjekte edeceğiz. Aşağıdaki örnekte bunun nasıl yaptıldığını görebilirsiniz;

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

`Dependency Injection` işte bu kadar basit! Sınıfımız `Mailer` sınıfını kendisi oluşturmaktansa, dışarıda oluşturulan `Mailer` sınıfının `constructor injection` (Enjeksiyonu constructor üzerinden yapmak) aracılığıyla sınıfımıza enjekte edilmesi yoluyla `Mailer` sınıfına sahip oluyor. Böylece sınıfımız `Mailer` sınıfına sıkı sıkıya bağlı olmaktan çıkıyor ve test edilebilirliğimiz muazzam düzeyde artıyor. Artık `unit testlerimizi` yazarken, `Mailer` sınıfını kolayca taklit edebiliriz. (Taklit etme olayına `Mocking` denmektedir.)

**b. Liskov's Substitution Principle**

Yukarıdaki örnek doğru bir `Dependency Injection` örneği olmasına rağmen bir eksikliği bulunmaktadır. Yazdığımız sınıf `SOLID İlkeleri`'nden biri olan `Liskov's Substitution Principle`'ı (Liskov'un İkame Kuralı) ihlal etmektedir. `Liskov'un İkame Kuralı` bize der ki; "Sınıflar, başka sınıflar yerine `Abstraction`'lara (Soyutlamalara) bağlı olmalıdır."

Bu kural `PHP`'de şu anlama geliyor. Örnekte kullandığımız `Mailer` sınıfını direkt olarak sınıfa enjekte etmek yerine, `Mailer` sınıfının bir `Interface`'e sadık kalması şart koşulmalı, bu `Interface`'e sadık kalan herhangi bir sınıf `type hinting` (Tür Dayatma) ile kontrol edilmeli ve sınıfımızda çalışabilir olmalıdır.

Hatırlarsanız yukarıdaki örneğimizde tür dayatmayı şu şekilde yapmıştık;

```php
<?php
     public function __construct(Mailer $mailer) // Mailer tür dayatmadır
     {
         $this->mailer = $mailer;
     }
```

Burada tür dayatma `Mailer` olduğu için, sadece `Mailer` sınıfı enjekte edilebilir olacaktır. Ancak, biz `Mailer` sınıfı yerine, belli bir `Interface`'e sadık kalan her sınıfı kullanabilmeliyiz.

Bu konuyu bir örnekle açıklayalım. Öncelikle `Interface`mizi oluşturalım ve bu interfaceye uyacak her sınıfta hangi methodların bulunması gerektiğini belirleyelim.

```php
<?php

interface MailerInterface {
    public function mail();
    public function attachFile();
    public function setSender();
    public function setTo($to);
}

```

Daha sonra `Mailer` sınıfımızın bu interfaceye uymasını sağlayalım.

```php
<?php

class Mailer implements MailerInterface // Dikkat ettiyseniz implements MailerInterface dedik
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

Şuan bu `Mailer` sınıfı, `MailerInterface` interfacesine sadık kaldığı için çalışabilecektir. Ancak, örneğin `Mailer` sınıfında `setTo()` methodu olmasaydı, sınıfımız `MailerInterface` içerisinde şart koşulan `setTo()` methoduna sahip olmadığı için çalışamayacaktı. Kısacası, kullandığımız interface, sınıfımızın sahip olması gereken methodları şart koşmaktadır. Interface içerisinde belirtilen 4 methodun hepsi bulunmayan sınıflar bu `Interface`'ye sadık kalamazlar.

`Liskov'un İkame Kuralı`'na uymak istediğimiz için, öncelikle tür dayatmamızı `Interface` olarak değiştirelim. (`Interface`'ler de birer soyutlama katmanı sayılırlar.)

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

Artık `Deneme` sınıfımız, `MailerInterface` interfacesine sadık kalan herhangi bir sınıfı kabul edecektir.

Anlamadınız mı? Sorun değil, bu durumu hemen dünyevi bir örnekle anlatayım. 

Aracınızla uzun bir yola gittiğinizi farzedelim ve benzininiz bitmek üzere. Benzin almak için bir benzinliğe uğradınız.

1. Eğer benzinliğe (sınıfa) bağlı olsaydınız, Türkiye'deki tek benzinciden benzin alabilirdiniz.
2. Depoya benzin yerine mezot doldurulmadığından emin olamazdınız.

Ne kadar saçma değil mi? O kadar yolu aynı benzinciye gitmek için geri dönmemiz gerekecek. Ama biz Türkiye'deki herhangi bir benzinliğe girip benzin alabilmek istiyoruz. `Interface`lere bağlı kalmak, bizim Türkiye'deki tüm benzincilerden benzin alabiliyor olmamızı sağlar. Çünkü, biz eminiz ki her benzinlikteki benzin pompası bizim aracımızın benzin deposuna uygun. Biz eminiz ki, pompadan çıkan şey (benzin) bizim aracımızın kabul ettiği birşey. (interface) Her benzinlik istasyonu bu tür standartlara sadık kaldığı için istediğimiz benzinliğe girebiliriz.

Şimdi yukarıda verdiğimiz örneği yazılımsal bir örnekle anlatalım.

Mail göndermek için tek bir `Mailer` sınıfı yerine, `SwiftMailer`, `SMTPMailer`, `AWSMailer` veya `MandrillMailer` gibi sınıfları kullanabiliriz. Oluşturduğumuz interfaceye uygun oldukları canımız hangisini isterse o sınıfı kullanabiliriz.

Peki bu bize ne avantaj sağlar? Belki çalıştığımız işyeri artık maillerin `Mandrill API`'si kullanılarak gönderilmesini istedi. Tüm mail gönderme sistemini baştan mı yazacağız? Hayır. Bir `Interface`'e sadık kaldığı sürece her mail sınıfını kullanabiliriz.

Örneğin:

```php
<?php

interface MailerInterface {
    // Şart koşulan methodlar
}

class SwiftMailer implements MailerInterface
{
    // SwiftMailer implementasyonu
}

class MandrillMailer implements MailerInterface
{
    // MandrillMailer implementasyonu
}

class AWSMailer implements MailerInterface
{
    // AWSMailer implementasyonu
}

class Deneme
{
     private $mailer;

     public function __construct(MailerInterface $mailer)
     {
         $this->mailer = $mailer;
     }
}

$mailer = new Mailer(new SwiftMailer); // çalışır
$mailer = new Mailer(new MandrillMailer); // çalışır
$mailer = new Mailer(new AWSMailer); // çalışır
$mailer = new Mailer(new BenzinPompasi); // çalışmaz!!
```

Yukarıdaki örneği inceleyelim.

1. Deneme sınıfı, `SwiftMailer` sınıfını kabul edecek mi? Evet. Çünkü interfacemize sadık kalmış.
2. Deneme sınıfı, `MandrillMailer` sınıfını kabul edecek mi? Evet. Çünkü interfacemize sadık kalmış.
3. Deneme sınıfı, `AWSMailer` sınıfını kabul edecek mi? Evet. Çünkü interfacemize sadık kalmış.
4. Deneme sınıfı, `BenzinPompasi` sınıfını kabul edecek mi? Hayır! Çünkü interfacemize sadık kalmış. Ne olduğu belirsiz birşey bu!

Sonuç olarak, `Liskov'un İkame İlkesi`'ne artık uyabiliyoruz. Artık bir sınıfa bağlı olmak yerine, bir `Interface`e bağlıyız!

Peki bu bize ne avantaj sağlayacak? Artık işyerindeki patronunuz mail göndermek için `Mandrill` kullanalım derse, `MandrillMailer`'i enjekte edebiliriz. `AWSMailer`'e dönelim derse, tek satırı değiştirerek `AWSMailer`'e dönebiliriz.

```php
<?php
     $mailer = new Mailer(new canimizNeIsterse());
```

**c. Dependency Injection konteynerleri**

`Dependency Injection` kavramını öğrendik. `Liskov'un İkame İlkesi`'ni öğrendik. Birçok şey öğrendik ama hala öğrenmemiz gereken konular var.

Şuana kadar herşey güzel, ama yüzlerce `Deneme` sınıfımız varsa ne yapacağız?

```php
<?php
     new Deneme1(new Mandrill());
     new Deneme2(new Mandrill());
     new Deneme3(new Mandrill());
     new Deneme4(new Mandrill());
     new Deneme5(new Mandrill());
     new Deneme6(new Mandrill());
     ...
```

Hepsine tek tek `new Mandrill();` mi diyeceğiz? Hayır. Daha önce ne anlatmıştık? "DRY kuralına uyacağız, ve kendimizi tekrar etmeyeceğiz."

`Dependency Injection Containers` (Dependency Injection Konteynerleri), Dependency Injection (kısaca DI) konusunda bize yardımcı olan konteyner sınıflardır. Hangi sınıfın nereye bağımlı olduğuna, nereye enjekte edileceğine çoğu zaman bu konteyner sınıflar karar verir ve bize yardımcı olur.

Interface kullanırken, şöyle bir sorunla karşı karşıya kalabiliriz. Interfaceler, sınıflar gibi instantiate edilemezler. (Yani `new` kullanarak onları çalıştıramayız.)

Şimdi, çok basit bir algoritma geliştirip Dependency Injection konteynerimizi oluşturacağız. Bu konteyner, `MailerInterface` tür dayatmasını gördüğü yere, bizim belirlediğimiz sınıfı çalıştıracak ve enjekte edecek.

Örneğin, biz konteyner sınıfında `Mandrill`'i seçersek, konteyner sınıfı bundan böyle `MailerInterface` gördüğü heryere `Mandrill` sınıfını enjekte edecek. Eğer `SwiftMailer`'i seçersek, `MailerInterface` gördüğü yere `SwiftMailer` sınıfını enjekte edecek.

Dependency Injection konteynerlerinin, genel olarak aşağıdaki gibi bir kullanımları bulunmaktadır.

```php
<?php

    $container = new DependencyInjectionContainer;

    $container->bind('MailerInterface', function() {
        return new MandrillMailer; //Artık burayı değiştirmemiz yeterli olacak.
    });

    // Yeni bir MandrillMailer instancesi
    $deneme = new Deneme($container->resolve('MailerInterface'));
    
    // Yeni bir MandrillMailer instancesi
    $deneme2 = new Deneme($container->resolve('MailerInterface'));

    // Yeni bir MandrillMailer instancesi
    $deneme3 = new Deneme($container->resolve('MailerInterface'));

    ...
```

Bu örnekte, `return new MandrillMailer` bölümünü değiştirmemiz yeterli olacaktır.

Bazı konteyner sınıflar, `Singleton` kullanımını gibi diğer kullanımları da destekler. (`Singleton` kullanılan sınıflar sadece tek bir instanceye sahip olabilirler, 2. defa instanceleri oluşturulamaz.) Dependency Injection Konteynerlerinin avantajları bunlarla sınırlı olmamakla beraber, detaylı bilgiye sahip olmak isteyen arkadaşlar daha fazla araştırma yapabilirler.

**d. Inversion of Control**

// Yakında

**e. Dependency Inversion Principle**

Yazımız boyunca birçok defa bahsettiğimiz `SOLID İlkeleri`'nin sonuncusudur.

// Yakında

### - mysql_real_escape_string() sizi SQL Injection'dan korumaz. ###

Birçok PHP geliştirici, gelen inputu `mysql_real_escape_string()` ile süzerek `SQL Injection` saldırılarından korunduğunu sanmaktadır. Bu klasik bir yanlıştır ve sizi anca temel düzeydeki `SQL Injection` saldırılarından koruyabilir. Üst düzey ve komplex bir enjeksiyon yapıldığında bu fonksiyon hiçbir işe yaramaz ve sizi koruyamaz.

`SQL Injection`'dan korunmak için, veritabanı `driver`larının `prepared statements` özelliği kullanılmalıdır. Prepared statements özelliği `Mysqli` ve `PDO`'da bulunabilir. `Prepared statements`, escaping işlemini sizin yerinize yapar, bu yüzden kullanımı son derece kolaydır.

```php
<?php

    $sth = $dbh->prepare('SELECT isim, renk, kalori_degeri
    FROM meyveler
    WHERE kalori_degeri < ? AND renk = ?');

    $sth->execute(array($_POST['kalori_degeri'], 'Kırmızı'));
```

Artık herhangi bir süzmeye gerek kalmadan, `$_POST['kalori_degeri']` değerini sorgu içerisinde kullanabilmekteyiz. Ancak dikkat ettiyseniz sorguda `?` kullandık ve `POST` değerini daha sonra sırasıyla `?` olan yerlere bindledik. 

Artık `PDO driver`i, sorguyu oluştururken `?` gördüğü bölümlere bizim verdiğimiz parametreleri ekleyecek ve `SQL Injection` saldırılarının tamamını bizim yerimize önleyecektir.

### - Database Abstraction Layers (DBAL) ve Object Relational Mapping (ORM) ###

Bir önceki örnekte `SQL Injection`'un nasıl önleneceğini öğrenmiştik. Bilmeniz gereken bir konu da `PDO` ve `Mysqli`'nin üzerine `abstraction` (Soyutlama) katmanları çekebileceğinizdir. `Abstraction` (Soyutlama), Nesne Yönelimli Programlama'nın temel 4 ilkesinden biridir. (Hatırlarsanız daha önce `Kapsülleme` ilkesini anlatmıştık.)

Örneğin, bir veritabanı sınıfı yazalım.

```php
<?php

class Database
{

     private $queryCount, $pdo;

     public function __construct($dsn, $user, $password, $host)
     {
          $this->queryCount = 0;
          $this->pdo = new PDO($dsn, $user, $password, $host)
     }

     public function isConnected()
     {
          return $this->pdo->isConnected();
     }

     public function getQueryCount()
     {
          return $this->queryCount();
     }

     public function getResult()
     {
          return $this->pdo->fetch();
     }

     public function query($query, Array $params)
     {
          $this->queryCount++;

          $this->pdo->prepare(); //Sorguyu hazırla
          $this->pdo->execute(); //Sorguyu çalıştır
     }
}

$database = new Database('dsn', 'user', 'password', 'host');

$database->query('SELECT * FROM users WHERE username = ?', array($_POST['kullanici']);

$database->getResult(); // Kullanıcının bilgilerini aldık
$database->getQueryCount(); // 1

```

Buradaki `Database` sınıfı, `PDO` driverinin üzerine çekilmiş bir soyutlama katmanıdır. Biz `Database` sınıfı içerisinde hem kendi methodlarımızı oluşturup, hem de `PDO`'yu kullanabilmekteyiz. Dikkat ettiyseniz `$queryCount` adında bir değişken oluşturduk ve `query methodu` her çağırıldığında bu sayıyı `1` artırdık. Bu tür özellikleri `PDO` size sağlamasa bile, siz bu özellikleri kendiniz ekleyip kullanabilirsiniz. 

Şuana kadar herhangi bir veritabanı sınıfı kullandıysanız, bu sınıflar da `MySQL` üzerine çekilmiş birer soyutlama katmanlarıydı.

Bu konuyu öğrendiğimize göre, artık `DBAL` ve `ORM` konularına girebiliriz.

Veritabanı driverları üzerine çekilen soyutlama katmanları, `Database Abstraction Layers` (DBAL), yani Veritabanı Soyutlama Katmanları adıyla anılmaktadır. Yukarıda vermiş olduğumuz örnek bir `DBAL` örneğidir. Popüler bir `DBAL` örneği olarak `Doctrine DBAL`'ı gösterilebilir.

`Object Relational Mapping` (İlişkisel Obje Eşleme) ise, veritabanı yapınızın objeler şeklinde tutulmasını sağlar. `ORM` araçları olarak `Doctrine ORM`, `Propel ORM`, `ActiveRecord ORM` gösterilebilir. Ben anlaşılması en basit olan, `Laravel`'in (framework) geliştirdiği `Eloquent ORM` ile bir örnek vererek anlatacağım.

`ORM` kullandığımızda, neredeyse hiç `SQL` sorgusu kullanmayız. Biz yapacağımız değişiklikleri, veritabanımızı temsil eden sınıflar üzerinde yaparız.

> Not: Bu bölüm, Eloquent ORM'in, Laravel'de nasıl çalıştığını göstermektedir.

`Eloquent ORM` için, veritabanımızdaki `users` tablosunu temsil eden bir sınıf oluşturalım.

```php
<?php
class User extends Eloquent
{

}
```
Laravel frameworkü, otomatik olarak veritabanındaki `users` tablosunu referans ettiğimizi anlayacaktır, çünkü `user` (kullanıcı) kelimesinin çoğul şekli `users` (kullanıcılar) olacaktır. Siz de veritabanı oluştururken tablo isimlerinizi çoğul oluşturabilirsiniz. (örneğin kullanıcılar, kategoriler, yorumlar vb.) Tablo isimlerini çoğul olarak kullanmak `good practice` (iyi kullanım) sayılmaktadır. Ancak, biz Eloquent örneğimize geri dönelim.

Artık, `User` sınıfını uygulamamızda kullanabiliriz! Hepsi bu kadardı, gerçekten.

```php
<?php

    $user->find(1); // ID'si 1 olan kullanıcıyı al
    $user->find(1)->delete(); // ID'si 1 olan kullanıcıyı sil

    $user->find(15);
        $user->email = "deneme@ornek.com";
    $user->save(); // ID'si 15 olan kullanının email adresini güncelle

    $user->all(); // Tüm kullanıcıları çek

    $user->where('yetki', 'admin')->take(5)->get();
    // Yetkisi admin olan kullanıcılardan 5 tane çek
```

Ne kadar kolay duruyor değil mi? Tek satır SQL sorgusu yazmadan istediğimiz tüm veritabanı işlemlerini gerçekleştirebiliyoruz. `Eloquent` neredeyse bizimle konuşuyor.

Her `ORM` aracı, `Eloquent` kadar kolay olmamakla beraber, bu tür kullanımı desteklemektedir. Projelerinizde kullanmanızı şiddetle tavsiye ederim.

### - Kullanıcı şifrelerini md5() gibi yöntemlerle şifrelemeyin. ###

Kullanıcı şifrelerini `md5()` gibi zayıf ve asıl amacı şifreleme olmayan bir algoritma ile şifrelediğinizde, bu şifreler çok kolay şekilde kırılabilir. Orta seviye bir bilgisayarın bile birkaç dakika içerisinde `md5()` şifrelerini kırabilmektedir.

`md5`'in zayıf olduğu bir konu da, birçok stringe aynı md5 karşılığını verebilmesidir. Genellikle md5 şifreleri kıranlar, bu şifreleri kırmaktan çok, aynı md5 çıktısını türeten veren başka bir şifreyi bulmaya çalışırlar.

Siz kullanıcı girişi gibi bölümlerde md5 şifreyi karşılaştırdığınız için, saldıran kişiler farklı bir şifreyi kullansa bile, md5 çıktısı aynı olacağı için sisteme giriş yapmış olurlar.

Bu yüzden, kullanıcı şifrelerini kullanırken mutlaka;

1. Sağlam bir şifreleme algoritması kullanın.
2. Salt veri oluşturun ve bunu şifrelemede kullanın.

`Salt` veri nedir? `Salt` veri, `Kriptoloji`'de şifrenin çözülmesini zorlaştırmak için şifreye rastgele veri eklenmesidir. Salt verileri şifrenin kırılmasını zorlaştırır.

`PHP`'de şifreleme için `bcrypt` algoritmasını kullanabilirsiniz. `Bcrypt` algoritması, şifrenin güvenli olması için:

1. Salt veri kullanımını mecbur kılar.
2. Şifreyi birçok defa şifrelemeden geçirir ve kırılmasını çok zorlaştırır.
3. Tek taraflı şifreleme algoritmasıdır.

`Bcrypt` ile kriptolanmış şifrelerin çözülebilmesi için, mutlaka kriptolanmış şifrenin, şifrenin bcrypt tarafından kaç defa kriptolandığının ve salt verinin ne olduğunun bilinmesi gerekir. Böylece şifrenin kırılması neredeyse imkansız hale gelir. Bu şifrenin kırılabilmesi için son derece güçlü bir bilgisayar ordusunun çok uzun süre çalışması gerekmektedir.

`Bcrypt` ve diğer şifreleme algoritmaları, PHP'ye `5.5-dev` versiyonu ile eklenmiştir. PHP'nin eski çekirdek geliştiricilerinden biri olan (Ne yazık ki ayrıldı.) Anthony Ferrara, oluşturduğu `password_compat` kütüphanesi ile bu özelliği `PHP 5.3.7`'ye kadar indirmiştir. İsterseniz bu kütüphaneye [https://github.com/ircmaxell/password_compat]() adresinden ulaşabilir ve projelerinizde kullanabilirsiniz.

### - Kullanıcıya güvenmeyin. Aşırı paranoyak olmayın. Input filtrelenir, output escape edilir. ###

Sosyal platformlarda gördüğüm en büyük yanlışlardan biri bu konu. `XSS` veya `SQL Injection` koruması sağlamak için kullanıcıdan gelen veriler `strip_tags`, `htmlspecialchars` ve `htmlentities` gibi birçok fonksiyondan geçirilip veritabanına ekleniyor. Bu son derece yanlıştır ve size yarardan çok zarar verecektir.

Öncelikle, `Input` ve `Output` nedir bunu öğrenelim. `Input` (Girdi), `PHP`'ye gelen verilerin tamamıdır. Örneğin, diskten bir veri okumak, kullanıcının formdan veri yollaması, veritabanının bize veri döndürmesi gibi konuların hepsi `Input` sayılmaktadır. Bazı geliştiriciler bunu sadece kullanıcıların formdan gönderdiği veri ile karıştırmaktadır. `Output` (Çıktı) ise, `PHP`'den çıkan verilerdir. Örneğin, `echo` ile ekrana veri bastırmak, `SQL` sorguları, 3. parti uygulamalara istek göndermek, shell üzerinde komut çalıştırmak vb. `Output` olarak tanımlanır.

Şimdi, neden `Input` verilerinin `htmlspecialchars`, `htmlentities`, `strip_tags` gibi fonksiyonlardan geçirilmelerinin gereksiz olduğundan bahsedelim.

Öncelikle, kullanıcıdan gelen verinin bozulmadan veritabanında saklanması önemlidir. `XSS` ve benzeri saldırılar veritabanında bir zarara yol açmayacağı için veritabanına girmesinin, veya veritabanında tutulmasının bir sakıncası yoktur. Ancak, bu veriler ekrana basılırken mutlaka escape edilmesi gerekmektedir.

**a. Verilerin bozulmadan saklanmasını sağlanmalıdır.**

Kullanıcının gönderdiği veri ham haliyle veritabanında saklanmalıdır, bu yüzden orjinal içeriğe daima ulaşma şansınız olur.

**b. Potansiyel uzunluk hatalarının önüne geçmiş olursunuz.**

Bir ` ` karakteri süzgeç fonksiyonlardan geçtiğinde `&nbsp;` veya  `&#160;` haline dönüşüebilmektedir. Bunlar `6 harf`ten oluşmaktadır ve daha öncesinde uzunluk doğrulaması yapmış olsanız bile, veritabanına girecekleri zaman hücrenin maksimum uzunluğu aşabilirler. Sonuç olarak bu veri ya hücreye eksik şekilde girecektir, ya da veritabanı hata verip sorguyu kesecektir. (Bu tür hatalara genellikle `overflow` hataları denmektedir.)

**c. Zararlı kod bir şekilde veritabanına sızmışsa, çıktı esnasında bu temizlenir.**

Veritabanına tek erişibim yapabilen uygulamanız değildir. Veritabanına direkt olarak bağlanıp zararlı bir kod yazsanız bile, ekrana bastırma esnasında escape işlemi uygulayacağınız için bu sorun olmaktan çıkar.

Ancak, bu bilgiler `SQL Injection` veya diğer saldırılar için geçerli değildir. Bu yüzden her saldırının escape işlemi farklı olmaktadır.

Örneğin, bir `SQL Injection` saldırısını önlemek için uygulamamız gereken escape işlemi farklıdır. `Shell Injection`'ın önlemi farklıdır, `XSS`'in önlemi farklıdır. Bunların hepsi farklı şekilde escape edilmelidir. 

Bu yüzden, altın kuralımız şudur:

**Input (Girdi) esnasında veri filtrelenmelidir.**

Filtremele, genellikle `Validation` (Doğrulama) adıyla da tanınır. Verilerin doğruluğu bu esnada kontrol edilmelidir. Örneğin, veritabanına bir tc kimlik numarası eklenecekse, onun gerçekten bir tc kimlik numarası olduğu doğrulandıktan sonra veritabanına eklenmelidir.

**Output (Çıktı) escape edilmelidir.**

Bunun her saldırı için farklı olacağını söylemiştik, ancak bu bölümde bu konuyu biraz açalım.

`SQL Injection` saldırısını ele alalım. Bu saldırıyı escape etmek için  veriyi `mysql_real_escape_string` (daha önce kötü olduğundan bahsetmiştik, ancak çok kullanıldığı için mantığını anlamanız için örnekte kullanıyorum) gibi fonksiyonlardan geçirebilirsiniz ya da veritabanı driverlarının `prepared statements` özelliğini kullanıp bu işin sizin yerinize yapılmasını sağlarsınız.

`Shell Injection` saldırısını önlemek için, veriyi komut içerisine geçirmeden önce `escapeshellcmd` gibi fonksiyonları kullanarak escape edersiniz.

`XSS` saldırısını önleyecekseniz, veriyi `strip_tags`, `htmlspecialchars` gibi fonksiyonlardan geçirirsiniz. Ancak bu işlem veri, veritabanına girerken yapılmaz, çıktı verirken yapılır. Sebebini yukarıda açıklamıştık.

Bu yüzden, makalelerde bolca gördüğünüz şunun gibi örnekler son derece yanlıştır.

```php
<?php

    $username = mysql_real_escape_string(trim(strip_tags(htmlspecialchars($_POST['username'])));

```

Bunun adı, bana göre `paranoyak`lıktır, ve evin arka kapısı ağzına kadar açık kalmışken, ön kapının tankla, tüfekle, bir yığın askerle korunmasına benzer.

Şimdi gerçek bir dünya örneği ile bu öğrendiğimiz bilgiyi test edeşim. Örneğimiz basit olsun, bir kullanıcı sitemize kayıt olmak istiyor, bu yüzden HTML formunu doldurdu ve gönder butonuna tıkladı. (Ne kadar sıradışı bir fikir, değil mi?)

1. İstek geldi. Doğrulamamızı yapalım. Kullanıcı adı alfa karakterleri mi barındırıyor? Daha önce alınmış mı? Şifre 3-12 karakter arasında mı? Yeni şifre özel karakterleri barındırıyor mu? Seçilen 3-12 karakter arasında mı? TC kimlik no doğru mu? Email adresi düzgün yazılmış mı?
2. Eğer herşeye cevabımız evet ise, `Prepared statements` kullanarak veritabanını güncelleyelim. Daha önce ne demiştik, prepared statements escape işlemini bizim yerimize yapıyor. Ama kullanıcıdan gelen verileri veritabanına eklemeseydik, ve örneğin shell sorgusunda kullansaydık, `escapeshellcmd` kullanarak escape edecektik
3. Kullanıcının verisini veritabanına ekledik. Şimdi, başarı sayfasında bu verilerin bazılarını çekeceğiz ve kullanıcıya "Aristona, üyeliğiniz başarıyla alındı." yazdıracağız. Burada, `XSS` saldırısı yapılabileceği için, veriyi ekrana bastırırken escape edip bastıracağız. Örneğin;

```php
<?php

     // Kullanıcının doğrulama ve kayıt işlemleri

     $username = "Aristona"; //Kullanıcı adı "Aristona" olsun.

     echo 'Başarıyla kayıt oldun,' . $username . '; //Yanlış
     echo 'Başarıyla kayıt oldun,' . strip_tags($username) . '; // Doğru
```

Bu kurallara uyduğunuz zaman;

1. Paranoyaklığı bırakırsınız.
2. Veritabanınızı çöplüğe döndürmemiş olursunuz.
3. Veritabanınızda orjinal içeriği daima tutmuş olursunuz.
4. Güvenli ve kullanışlı bir uygulamanız olur.

Yazılımda paranoyak olmak iyi birşeydir ama azlığı veya fazlalığı hem size, hem de uygulamanıza zarar verir. Siz daima yapmanız gerekeni yapmalısınız. Gece yatarken nasıl pencerelerin kapalığı olduğunu kontrol edip yatıyorsanız, uygulama geliştirirken de böyle olun. Ne penceresiz bir evde yaşayın, ne de penceleri açık bırakıp uyuyun.

### - Kullanıcı giriş işlemleri için alfanumerik verileri kontrol etmeye çalışın. ###

Kullanıcının giriş işlemleri için asla isim/soyisim gibi bilgileri kontrol etmeyin. Kontrol edeceğiniz bilgiler daima alfanümerik harflerden oluşmalı. Aksi takdirde veritabanının karakter seti, kullanılan doğrulama fonksiyonları çok büyük hatalara yol açabilir.

Örneğin, bazı veritabanı karakter setleri, alfanümerik olmayan karakterlere izin vermezler. Bu durumda Türkçe karakterlerden olan Ç, Ş, İ, Ğ, Ü gibi karakterler görünmezler, veya `?` halinde çıkabilirler. Bazı karakter setleri büyük küçük harf duyarlı, bazıları duyarsızdır. Veritabanı karakter setlerine veritabanı bölümünde değineceğiz ama şimdilik bunu bilmeniz yeter.

Şimdi tekrar konumuza dönelim. Neden isim/soyisim gibi bilgiler kontrol edilmemeli? Çünkü birçoğumuzun isminde alfanümerik olmayan harfler var. Mesela benim adım `Anıl ÜNAL`, ve küçük `ı` harfi ile `ü` harfi alfanümerik değil. Benim durumumda olan milyonlarca Türk kullanıcı var. Bizden hariç japonlar, çinliler, koreliler, araplar vb. insanlar ve herkesin kullandığı karakter seti farklı olabiliyor. Ancak, birçoğumuz alfanümerik email adreslerini kullanıyoruz, veya alfanümerik bir kullanıcı almaya zorlanıyoruz. Bu daima iyi birşeydir. Alfanümerik karakterleri ne kadar çok kontrol ederseniz (özellikle giriş işlemlerinde) o kadar avantajlı olursunuz.

Bunun neden önemli olduğunu daha önce tecrübe etmiş olduğum bir örnekle açıklayayım. Son derece büyük bir MMORPG oyununda, başka insanların hesaplarını çalmak için kullanıcı adlarını bilmek yeterli oluyordu. Ama nasıl?

Bir kullanıcının kullanıcı adı `anıl123456` olsun. Bir başka kullanıcı da gidip `anil123456` kullanıcı adını alsın. Uygulama daima alfanümerik karakterlerin olacağını düşünüyordu, ama veritabanı hücresi alfanümerik olmayan karakterleri kabul ediyordu. `latin1_swedish_ci` gibi veritabanı collationları, `anıl123456` ile `anil123456` aynı sayıyordu. Binlerce insanın hesaplarının çalınmasına sebep olan hata buydu. Hesaplarında Türkçe karakter geçen kullanıcıların hesap isimleri, Türkçe karakterler değiştirilerek tekrar alınıyordu. (Anıl ise, Anil. Şener ise Sener gibi.) Bu yapıldığı zaman eski hesaplar giriş yapamıyordu ve yeni hesaplar giriş yaptığında eski hesabın içeriğine ulaşabiliyordu.

Eğer kullanıcınızın isminin görünmesini istiyorsanız (örneğin yorumlarda Anıl ÜNAL yazmasını istiyorsunuz) kullanıcıya alfanümerik bir kullanıcı adı aldırın ve bu kullanıcı adını sadece giriş/çıkış işlemlerinde kullanın. Geri kalan bölümlerde kullanıcının gerçek ismini gösterin.

### - Black list (kara liste) fonksiyonlarına güvenmeyin. ###

Kara liste oluşturan neredeyse tüm fonksiyonlar çöptür. Örneğin, `XSS`'i engellemek için aşağıdaki fonksiyonun kullanılması size hiçbir yarar sağlamaz.

```php
<?php

    function xss_cleaner($input_str) {
        $return_str = str_replace( array('<','>',"'",'"',')','('), array('&lt;','&gt;','&apos;','&#x22;','&#x29;','&#x28;'), $input_str );
        $return_str = str_ireplace( '%3Cscript', '', $return_str );
        return $return_str;
    }
```

Siz burada `script` kelimesini engellediğini düşünebilirsiniz, ama saldırgan `s/**/cript` gibi bir yöntem kullanarak bunu aşabilir. Bu yüzden kara liste oluşturan fonksiyonlar çoğu zaman işe yaramazlar.

Bu yüzden, özellikle konu güvenliğinizse kara liste oluşturan hiçbir fonksiyona güvenmeyin.

### - Veritabanında eksi değerde olmayacak hücreler UNSIGNED olmalıdır. ###

Veritabanında oluşturduğunuz bir `TINYINT` hücre, öntanımlı olarak `negatif` ve `pozitif` değerleri alacaktır. `TINYINT`'in alabileceği değerler `-128` ile `127` arasındaki rakamlardır. Ancak, bu hücre `UNSIGNED` olarak tanımlanırsa, `0` ve `255` arasındaki değerleri kabul edecektir.

Bu yüzden, daima pozitif olacağından emin olduğunuz hücreler için (örneğin `auto increment`) hücrelerinizi `UNSIGNED` olarak tanımlamak size yarar sağlayacaktır.

### - Uygulamanızda mümkün olduğunca Türkçe kullanmamaya çalışın. ###

İngilizce bilmek ve İngilizce kullanmak yazılımcıların hayatını kolaylaştıran en önemli faktörlerdendir. 

İngilizce yazılım dünyasında hemen hemen heryerde karşınıza çıkacaktır. Örneğin, kullandığımız programlama dillerindeki methodlar İngilizce'dir. Takip edebileceğiniz ünlü yazılımcılar hep İngilizce konuşmaktadır. Takip edebileceğiniz bloglar ve websiteleri İngilizce'dir. Github üzerindeki açık kaynaklı projelerin dökümantasyonu İngilizce olarak yazılmıştır. Projelerin kaynak kodlarında kullanılan değişken isimleri, sınıflar, yorum satırları vb. hep İngilizce'dir.

Özellikle Github ve açık kaynaklı projeler içerisinde İngilizce kullanmayan, değişkenlerin ve sınıfların yazılımcının kendi anadilinde tanımlandığı projeler, ne kadar iyi olurlarsa olsunlar başkaları tarafından genellikle kullanılmaz, destek görmez ve yeterince popüler olamazlar. Ayrıca bu durum sizin `amatör` olduğunuz algısı yaratır.

Örneğin, aşağıdaki sınıf Türkçe olarak geliştirilmeye çalışılmıştır.

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
        $stilDosyalari = Klasor::al('*', 'css');
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

Bu sınıf son derece çirkin ve amatörce duruyor. Türkçe desen tam olarak Türkçe değil, İngilizce Türkçe karışımı, ne olduğu belirsiz birşey. Zira PHP unicode desteklemediği için Türkçe karakterleri de kullanamıyoruz, bu yüzden ortaya Türkçe karakterlerin kullanılmadığı bir garip Türkçe çıkıyor.

Sorun sadece bununla kalmıyor. İleri de bir sorun çıktığını farzedelim ve siz saatlerce uğraşmanıza rağmen bunu çözemiyorsunuz. Bu durumda, eğer İngilizce kullandıysanız `Stackoverflow`, kullandığınız dilin `irc kanalları` veya forumlarında hatalı scripti kopyala yapıştır yaparak sorununuzu kolayca dile getirebilirsiniz. Türkçe kullanırsanız kod parçacıklarını koyarken bunları İngilizce'ye çevirmeniz gerekecek aksi takdirde büyük çoğunluğu İngilizce konuşan insanlar sizin ne yapmak istediğinizi anlayamayacaktır.

Buna ek olarak, Türkçe karakterleri kullanmak ayrıca son derece sakıncalıdır. Bunun sebeplerinden bazılarını daha önceki bölümlerde anlatmıştık, ancakk `SSH` üzerinden sunucuya bağlanıp Türkçe karakter yüzünden dosyanın açılmadığı veya komutları giremediğiniz zaman zaten kendinize kızacaksınız. (Bu yazıyı yazarken bile Türkçe karakterler yüzünden birçok sıkıntı çektim ve neredeyse yarım günümü bu hataları çözmek için harcadım.)

Birinin size Ankara İstanbul arası kaç `mil` diye sormasını ister misiniz, veya trafiğin sağdan aktığı ülkelerde ben soldan gideceğim diye inat eder misiniz? Her iyi yazılımcı gibi kendinizi İngilizce kullanmaya alıştırın. Bilmiyorsanız da öğrenmeye çalışın, çünkü İngilizce öğrenmek bile sizi İngilizce bilmeyen yazılımcıların birkaç ışık yılı ötesine taşıyacaktır.

> Not: Tartıştığım yazılımcıların karşı argümanı bazen İngilizce bilmeyen yazılımcılarla çalıştıkları, bu yüzden Türkçe kullanmayı tercih ettikleriydi. Ben şahsen İngilizce bilmeyen yazılımcılara pek güvenemesem de, bu durumda projenin geleceğini düşünmek katı kurallara uymaktan daha önemli olabilir.

### - Kimin yazdığını bilmediğiniz bloglardan ve eğitim setlerinden uzak durun. ###

Kötü eğitim yarar sağlamaktan çok zarar verir. Özellikle Google aramalarında bazen üstlerde çıkan Türkçe bloglar ve bu bloglardaki makaleler ve eğitim setleri; çoğu zaman eksik, demode ve yanlış bilgiler içermektedir. Bu tür bloglardaki içeriklerin çok büyük bir kısmı kaynak belirtilmemiş çeviri, kalanların da birçoğu 2-3 aylık tecrübesiz yazılımcıların `ilk heyecanlarıyla` bloglarına yazdıkları eksik ve yanlış makelelerden oluşmaktadır. (İstisnaları ayrı tutuyorum, ancak ayrı tutacak istisnalar yok denecek kadar az malesef.)

Ben genellikle birinin blog sitesine girdiğim zaman, yazdıkları makelelerin başlıklara bir göz gezdiririm. (Bilmiyorum siz de böylemisiniz?) Yazdıları makelelerin kalitesi bana blogun kalitesi hakkında ipucu verir, ancak bazı bloglar varki gerçekten bir çöp yığınından fazlası değil. Örneğin, bloglarına girip yazdıkları makaleleri okuyunca önce şaşırırsınız. Adam scalability'den girmiş Nginx konfigürasyonlarına kadar, PHP 6 ile gelecek özelliklerden bahsetmiş, yazılım mimarileri falan havada uçuşuyor. Sanırsınız Google'da baş mühendis. Sonra bir kaç blog yazısı daha yazmış "PHP'de echo kullanarak ekrana yazı bastırmak.", "mysql_query() ile veritabanından veri çekmek."

Sihirli kürem yok, bu yüzden gerçekte neler olup bittiğini kanıtlayamıyorum, ancak bu tür bloglardaki makaleler bana biryerden alınmış ve çevirilmeye çalışlışmış gibi geliyor. Kaynak belirterek çeviri yapmalarında bir sorun yok, ama çeviri yaparken birçok konsept ve terim çoğu zaman yanlış çevriliyor. Böylece insanlar yanlış bilgilendiriliyor. Öğrendiğiniz şeylerin yanlış veya yalan olduğunun birgün farkına varsanız tepikiniz ne olurdu?

Özellikle bu durum `PHP blogları` için artık son derece vahim bir hale geldi. Wordpress ile websitesi kurup kendine web geliştirici diyenler, Dreamweaver'de form oluşturup kendini web tasarımcı sananlar, `PHP`'in temelini bile almadan bu işi ticarete dökmek için eğitim seti hazırlayanlar... bu işin ucu gerçekten kaçtı.

PHP bloglarının kötü olmasının bana göre birkaç sebebi var.

1. PHP ile birşeyler geliştirebilmenin diğer dillere göre daha kolay olması, bu yüzden amatör yazılımcılar tarafından sıkça tercih ediliyor olması. (Muhtemelen en büyük amatör/yeni yazılımcı kitlesi bizde.)
2. `WordPress` ve `Joomla` gibi son derece eski ve ilkel yöntemlerle geliştirilen projelerin son derece popüler olması. (malesef)
3. Birkaç yıl öncesine kadar Github ve Composer'in olmayışı, bu yüzden binlerce kötü, eski, güvenli olmayan, demode sınıf ve kod örnekleri internette dolaşıyor ve halen Google arama sonuçlarında çıkıyor olması.
4. PHP'nin 5.3 versiyonuna kadar modernlikten çok uzak bir dil olması.
5. Kurumsallıktan uzak firmaların kullanıyor olması. İşimizi nasıl modernleştirebiliriz yerine nasıl asgari ücrete çalışacak PHP'ci buluruz arayışı.
6. Sosyal Ağ filmini izleyip etkilenmiş, geleceğin `Mark Zuckerberg`'i olmak isteyen, 2 aylık bilgisiyle kendini `Linus Torvalds` sanan kitle.

Mesela bir `Scala` veya `Haskell` blog yazısında makelenin yanlış olma ihtimali son derece düşükken, `PHP` dünyasında bu ihtimal son derece yüksek. `Basic` dünyasında bile bu kadar yanlış ve hatalı bilgi olacağını sanmıyorum.

Bunların dışında, bir de özellikle Türkçe bloglarda göze çarpan genel eksikliklerden bahsetmek istiyorum.

1. Öncelikle bloglar açık kaynaklı değiller. Neredeyse hepsi `Wordpress` üzerine kurulmuş, bu yüzden başkaları düzeltmede bulunamıyor. (Buna çok bilinen `w3schools.com` dahil - Adamlar verdikleri örnekteki SQL Injection açığını tam 6 yıl sonra düzelttiler, ve bunlar bir sertifikasyon veren bir eğitim kurumu.)
2. Yanlış bir bilgi olduğunu söylediğin zaman yorumların siliniyor. Çok az kişi eleştiriyi kabullenebiliyor.
3. Çevirilerde terimler genellikle yanlış çeviriliyor, bu yüzden son derece alakasız sonuçlar çıkabiliyor. (İngilizce öğrenin dememin sebebi bu.)
4. Üst düzey PHP diye yazdıkları makaleler aslında `PHP`'nin temel bilgileri. Ben bunun bir marketing stratejisi olduğunu düşünüyorum, ama emin olun o bloglarda yazanlar hiçbirşey değil.

Gecekondu dikip üst düzey inşaat mühendisliği, geleceğin mimarisi diye anlatan, böbürlenen başka bir topluluk var mı acaba dünyada? PHP blogları hep böyle.

Bir `ASP.NET`'çi veya `C#`'çı, bankalarda veya kurumsal bir firmada çalışırken baş mühendis tarafından ağzının payını alır. Tecrübe kazanana kadar biraz sessiz kalayım der.

Bir `Ruby`'ci, starbucksta elinde kahve kod yazarken bu tür işlere zaten bulaşmaz. Yolunu bulmuştur.

Bir `Haskell`'ci veya `Lisp`'çi zaten blog yazmaz. Çünkü ne yazdıklarını kimse anlamaz.

Bir `Java`'cı, en azından bu işlere okulda aldığı temel mühendislik eğitimiyle başlar. Yazılım mimarilerini, tasarım desenlerini falan kullanamasa da bilir. Arada tek tük söyler.

Bir `C`'ci veya `C++`'cının zaten iyi veya kötü derdi olmaz. İyisi ve kötüsü arasındaki fark çoğu zaman ayırt edilemez.

Bir `Assembly`'cinin zaten blog yazacak bir interneti olmaz.

Bir `NodeJS`'ci, genellikle daha önceden `Javascript`'i tecrübe ettiği için biraz bilgilidir. Yeni başlayan "Soket, asekron falan bişey diyorlar anlamadım ben." der bırakır. 

Ama... `PHP` dünyası böylemi. Bir ev yaparlar, evin pencereleri olmaz, çatısı aşağıda olur, kapıyı açtığında bütün bina çöker ve kapıyı açtığınızda binayı yıktınız diye size kızarlar. Adam bir köpek kulübesi yapar, öyle bir havalara girer ki sanırsın Burj Dubai'yi yapmış. (ama o köpek kulübesini almak isteyen birçok insan çıkar, böyle de bir avantajı var.)

`PHP`'nin böyle garip bir komunitesi var. Bu yüzden konuyu çok dağıtmadan toparlayalım. Kısacası böyle bir durumda olduğumuz için, kendinizi eğitirken yanlış bilgi alıp kafanızı karıştırmayın. Doğru bilgiyi doğru insanlardan, doğru makalelerden, doğru bloglardan ve doğru kitaplardan alın. Bir eğitim setindeki videoları izliyorsanız, o eğitim setini kim yazmış? Ne zaman yazmış? PHP'in hangi sürümü kullanılmış? Yorumları nasıl? gibi konuları araştırın, yoksa bunlar size hiçbirşey kazandırmaz. Youtube'dan 10 yıl öncesinin videolarını izleyip PHP öğrenmeye çalışmayın. "Bu dosyayı al, her projenin başında include et, ne SQL Injection kalır ne birşey, güven sen bana." diyen insanlara gözü kapalı güvenmeyin. Bu konuda son derece dikkatli olmalısınız.

> Not: Bu yazdıklarım genellikle Türkçe bloglar için geçerli. İngilizce bloglar nispeten daha iyi durumda, ama onların da kötüsü çok fazla.

### - ...ya performanslı olmazsa? ...ya çok include uygulamayı yavaşlatırsa? ###

`OOP` (Nesne Yönelimli Programlama) kullanmak istemeyenlerin, frameworklere "Çok hantal çalışıyor." diyenlerin, modern tekniklerin uygulamayı yavaşlatacağını düşünenlerin klasik problemi. `Ya yavaşlarsa?`

Kısa cevap: Hiçbirşey olmaz.

Uzun cevap: Yakında yazarım. Bootleneckler, opcode caching nedir, scalability nedir, mikrooptimizasyonlar niye günü kurtarır, HHVM nedir vs.

### - Kaptan gemiyi terk etmişse, o gemide kalmanın fazla bir anlamı yok. ###

Son birkaç yılda açık kaynak adına son derece büyük adımlar atıldı. Artık neredeyse her türlü açık kaynaklı proje `Github` üzerinden yayınlanmakta. Ancak bu son derece avantajlı olmasına rağmen, bazen dezavantajları da olabiliyor.

Dezavantajlarından biri, `PHP`'in çok fazla açık kaynaklı projeye sahip olması. Bir `Mail` sınıfı arıyorsunuz ve karşınıza binlerce `Github repository`si çıkıyor.

Bir diğer dezavantajı da, insanların haklı olarak en çok gelecek vaadeden projelere yönelmesi. Bazen bir projeyi geliştiren yazılımcı, tekerleği tekrar icat etmektense, farklı bir yazılımcının projesinin daha iyi konumda olduğunu düşünebilir ve o projeye destek olmaya başlayabilir. Kendi projesini ise geliştirmeyi bırakabilir veya başka birinin devralmasını isteyebilir. (Devredilen projelerden çoğu zaman hayır çıkmaz, not edelim.)

Buna örnek olarak `Code Igniter` framework projesi gösterilebilir. Bir zamanlar iyiydi, herkes kullandı, ancak bu projeyi geliştiren çekirdek yazılımcıların birçoğu farklı projelere geçtiler, ve `Code Igniter` projesininden sorumlu olan `EllisLab` firması projeyi devralacak birini aramaya başladı. (Bu yazıyı yazdığım esnada birkaç ay geçmiş olmasına rağmen kimse projeyi devralmak istemedi.)

Kısacası, geminin kaptanı atladıysa, mürettebatı atladıysa, filikalar indirildiyse ve yolcuların birçoğu tahliye edilmeye başlandıysa, o gemide kalmanın mantıklı olduğunu düşünmemelisiniz. Siz de çok geç olmadan atlamalısınız.

Bu durum sadece `Code Igniter` projesiyle ilgili değil. Hertürlü açık kaynaklı ileride bu tür sorunlarla karşılaşılabilir. Bu yüzden, projenizde o günün şartlarındaki en popüler ve en gelecek vaadeden frameworkleri, komponentleri, sınıfları ve kütüphaneleri kullanmaya çalışın.

Terk etmeniz gereken projeler varsa, vakit kaybetmeden terk edin.

### - DRY kuralına uyun ve akıllı çalışın. ###

`DRY (Don't Repeat Yourself)`, Türkçe'siyle `Kendinizi Tekrar Etmeyin` kuralını hem PHP'in temelinde, hem de gerçekten üst düzey konularda kullanabilirsiniz. (Aslında bu bir kural değil, bir tavsiyedir.)

Temel bilgiye sahip bir yazılımcı, aynı şeyleri tekrar etmekten bıkıp o işlem için fonksiyon oluşturuyorsa `DRY` için bir adım atmış olur. Üst düzey bir yazılımcı her projesinde kullanabileceği bir komponent yazmış ve bunu package managerlar tarafından yönetiyorsa `DRY` için bir adım atmış olur.

// Salt PHP ve L4 konusunda DRY örnekleri gelecek buraya.

`DRY`ın sonu yoktur ve sadece programlama dilleriyle ilgili değildir. Proje geliştirirken sık sık yaptığınız işlemleri bilgisayara yaptırmakta bu kural için atılmış adımlar olacaktır. Örneğin, veritabanı yedeği mi alınacak? Veritabanı yönetici paneline gir, veritabanını seç, tabloları seç, exporta tıkla, yol olarak bir path belirle, çıktıyı oluştur, o klasöre gir, çıktıyı zip içerisine koy, sonra ismini "x dbsi yedeği" yap... Bu tür işlemlerle kaybettiğiniz zamanı hesaplayın ve o kaybolan zaman içerisinde kaç tane proje geliştirebileceğinizi düşünün.

Bunun yerine:

    grunt backup:veritabani

yazdığınızda bu işlemlerin sizin yerinize yapılması daha hoş olmaz mı?

Veya:

    grunt backup:veritabani --push

yazdığınızda hem veritabanı yedeğinin alınıp, hem csslerin optimize edilip, hem de sunucuya veritabanını yedeğinin yüklenmesini sağlamak istemez misiniz?

Proje geliştirirken en çok nelere vakit harcadığınızı düşünün ve bilgisayarın yapabileceği herşeyi bilgisayara yaptırın. 

Her seferinde `public function` yazmak zor mu geliyor? Kullandığınız IDE içerisinde `Snippet` oluşturun. `pub` yazınca otomatik tamamlasın. (Daha iyi ihtimalle, zaten birileri oluşturmuş ve Github'da paylaşmıştır, araştırın.)

Uzun terminal komutlarını zor mu geliyor? `Alias` oluşturun, hatta gerekiyorsa bunları script haline getirin.

Özellikle genç arkadaşlar bunun ne kadar önemli olduğunun farkında olmalılar. Günde 8 saat çalışarak 10 birim iş yapacağına 30 birim iş yapabilirsin. Uzun vadede ne kadar çok zaman ve (dolayısıyla para, çünkü zaman === para) kazanabileceğinin farkına varabilirsin. 

Bu yüzden, daha `akıllı çalış`. Daima daha fazlasını öğren. Vakit kaybetme, iyi olmak kadar hızlı olmakta önemli.

> Not: Geliştirici ortamında yapabileceğiniz iyileştirmelerden `Geliştirici Ortamı` bölümünde bahsedeceğim ve nasıl hızlanabileceğinizi açıklayacağım.

### - Daima tutarlı olun. ###

Indenting için tab kullanıyorsanız, tab kullanarak devam edin. Methodları `_` kullanarak ayırıyorsanız, `_` kullanarak devam edin. CSS'lerinizi yazarken ayraç olarak - kullanıyorsanız, heryerde - kullanın. Yaptığınız herşey tutarlı olsun.

Tutarsızlığın en güzel örneği `PHP` çekirdeği. `strpos` ve `str_replace` fonksiyonlarını ele alalım. Niye `str_position` değil de `strpos`? Veya niye tam tersi değil? Niye bazı fonksiyonlar önce diziyi, sonra stringi alırken diğerleri önce stringi, sonra diziyi alıyor? Bu tutarsızlıkların hepsini hatırlamak zorundamıyız? Bu tutarsızlıklar neden var?

Mesela neredeyse tüm programlama dillerinde `reverse()` verilen stringi tersine çevirir. PHP'de bu ne tahmin edin bakalım?

`str_reverse? streverse? strrev? revstr? reverse?`

Hepsi de olabilir, ama doğrusu `strrev`.

`PHP`'nin çekirdek geliştiricileri de bunun farkında ama `backwards compatibility` (Geriye Dönük Uyumluluk) bozulmasın diye şimdilik böyle devam ediyorlar.

Siz kendi projelerinizde bunu asla yapmayın. `TAB` kullanmayı bırakıp `4 boşluk` kullanmaya karar verdiyseniz, ya tüm uygulamayı buna uyarlayın, ya da eski şekil devam edin! Uygulamanın yarısı `TAB`, kalan yarısı `4 boşluk` olmasın. (Yeni öğrendiğim her şey için projeyi düzeltmeye kalksaydım, tek satır kod yazmadan optimize etmeye çalışıyor olurdum.)

Evini sarı renge boyarken yarı yolda fikrini değiştirip evin kalanını yeşil renge boyayan birini gördünüz mü? Ben görmedim, ama projenin yarısını farklı, kalan yarısını farklı geliştiren insanları tanıyorum.

> Not: Tutarsızlığı yüzünden PHP sosyal platformlarda çok ağır eleştirilere maruz kalmakta. (hatta artık kabak tadı verdi)

> Not: Eğer PHP'de `Ruby` ve `Javascript` gibi dillerdeki `reverse()` methodunu kullanmak istiyorsanız, PHP'in `scalar objects` özelliğini kullanabilirsiniz ancak birçok eksikliği/limitasyonu var. Kullanmanızı tavsiye etmem, ama bilginiz olsun diye yazıyorum.

### - Gerekmedikçe else ve uzun if blokları kullanmayın. Daima köşeli parantez kullanın. ###

Bazen, 7-8 satırlık if/else bloklarını tek satırda bile yazabilirsiniz.

Bir değişkenin array olup olmadığını anlamak istediğiniz bir fonksiyon yazıyorsunuz. Aslında bu örnek direkt olarak `is_array` kullanılarak yapılabilir, ancak ben burada soruna değinmek istediğim için fonksiyon oluşturarak yapabileceklerinizi anlatacağım.

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

Ama `return` kullanmak fonksiyonu zaten durduracağı için, `else` kullanmaya gerek kalmadan şu şekilde yazılabilir.

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

Şuanki hali üsttekinden çok daha güzel. Gereksiz `else` bloğunu kaldırmış olduk. Gereksiz `else` bloklarından korunmak çok büyük avantaj sağlıyor. Fonksiyonunuz sağ tarafa doğru uzamamış oluyor.

Devam edelim. `if` bloğu içerisindeki ilk satır daima if bloğu içerisinde sayılacağı için, köşeli parantezleri de silebiliriz.

```php
<?php

    function isArray($input)
    {
        if(is_array($input)
            return true;
  
        return false;
    }
```

Bu örnek düzgün çalışır ama son derece `tehlikelidir.` Neden tehlikeli olduğu nu birazdan `Apple`'ın meşhur `goto fail;` exploiti ile açıklayacağım, ama biz şimdilik devam edelim.

Biz bunu biraz daha geliştirip, `ternary` operatörünü de kullanabiliriz.

```php
<?php

    function isArray($input)
    {
        return is_array($input) ? true : false;
    }
```

Dilerseniz, `ternary` operatörünün ilk parametresi olan `true` yi bile silebilirsiniz.

```php
<?php

    function isArray($input)
    {
        return is_array($input) ?: false;
    }
```

Son olarak, `is_array()` fonksiyonu `true` veya `false` döndüreceği için, fonksiyondan dönen değeri siz de direkt olarak döndürebilirsiniz.

```php
<?php

    function isArray($input)
    {
        return is_array($input);
    }
```

Bu ufak fonksiyon bile birçok şekilde yazılabiliyor, ancak herşeyi kısa yazmak her zaman doğru değildir. Özellikle köşeli parantezleri kaldırmak, farkında olmadan birçok hatanın çıkmasına sebep olabilir.

Köşeli parantezler olmadığındai yanlışlıkla `if` bloğu sonrasına 2. bir satır eklenirse, 2. satır daima çalışacağı için scriptimiz yanlış çalışmaya başlayacaktır.

```php
<?php

    function test()
    {
        if(birşey)
          return "If";
          return "Hata"; // Burası if bloğu içinde değil
        else
          return "Else";
    }
```

Bu örnekte, `else` hiçbir zaman çalışmayacaktır. Çünkü, `birsey` şartı sağlanıyorsa, sadece `return "If";` çalışacak ve fonksiyon `Merhaba` değerini döndürecektir. Diğer her türlü durumda `return "Hata";` çalışacaktır ve fonksiyon `Hata` değerini döndürecektir. Siz uygulamamızı `else` bloğunun çalışacağı durumlarda `Else` değeri dönecek diye programladıysanız, büyük küçük hata birçok bug oluşmasına sebep olacaktır.

Köşeli parantez kullansaydınız bu sorun oluşmayacaktı.

```php
<?php

    function test()
    {
        if(birşey)
        {
            return "Merhaba";
            return "Hata";
        }
        else
        {
            return "Else";
        }
    }
```

Bu durumda, `return "Hata";` asla ve hiçbir koşulda çalışmayacaktı.

Bu neden mi önemli? Apple'ın SSL'de çıkardığı meşhur `goto fail;` hatasının sebebi son derece komik.

```c
    err = true; // ilk başta err true oluyor
    hashOut.data = hashes + SSL_MD5_DIGEST_LEN;
    hashOut.length = SSL_SHA1_DIGEST_LEN;
    if ((err = SSLFreeBuffer(&hashCtx)) != 0)
        goto fail;
    if ((err = ReadyHash(&SSLHashSHA1, &hashCtx)) != 0)
        goto fail;
    if ((err = SSLHashSHA1.update(&hashCtx, &clientRandom)) != 0)
        goto fail;
    if ((err = SSLHashSHA1.update(&hashCtx, &serverRandom)) != 0)
        goto fail;
    if ((err = SSLHashSHA1.update(&hashCtx, &signedParams)) != 0)
        goto fail;
        goto fail;
    if ((err = SSLHashSHA1.final(&hashCtx, &hashOut)) != 0)
        goto fail;

    fail: return err;
```

Sorunu görmediniz mi? Tekrar bakın:

```c
    goto fail;
    goto fail; // Bunun burada ne işi var?
    if ((err = SSLHashSHA1.final(&hashCtx, &hashOut)) != 0)
        goto fail;
```

Yanlışlıkla 2 tane `goto fail;` yazıldığı için `if ((err = SSLHashSHA1.final(&hashCtx, &hashOut)) != 0)` kontrolü hiçbir zaman zaman erişilemiyor. 2. sıradaki `goto fail;` daima çalışacağı için script `fail`'e düşüyor ancak burada hatanın dönmesi yerine `true` değeri dönüyor. Yani en alttaki `if` kontrolü yok sayılıyor, ve bu kontrol yok sayıldığı için bu bölüm üzerinden bir exploit ortaya çıkmış oluyor.

Dünyanın belkide en büyük teknoloji firmasının yaptığı bu hata son derece komik olmakla beraber, sadece düzgün şekilde köşeli parantez kullanılsaydı kendiliğinden çözülmüş olacaktı. Demekki, dünyanın en iyi yazılım mühendisleri bile bu tür hataları gözden kaçırabiliyor. Bu yüzden "Ben dikkatliyim, hiçbirşeyi gözden kaçırmam." demeyin. Bu yüzden mümkün olduğunca köşeli parantezleri doğru kullanmaya çalışın ve gereksiz `else` ifadelerini kullanmaktan kaçının.

### - Kod yaz, tarayıcıya dön, F5'e bas, hata var mı? Yok, devam et. ###

`DRY` (Kendinizi Tekrar Etmeyin) bölümünde anlattığım konuya bir örnekte bu konu. PHP geliştiricilerinin %99'u bu şekilde çalışıyor (ve bu normal) ama yanlış. Neden yanlış olduğunu `DRY` bölümünde anlatmıştım. "Bilgisayarın yapması gereken şeyleri siz yapmamalısınız."

Oluşturduğunuz form submite tıkladığında form verilerini uygulamaya gönderiyor mu? Güzel, ama bunu `acceptance testi` yazarak test edin. Tarayıcıdan bakarak değil.

Formdan veri gönderildiğinde, gelen istek doğru yere yönlendiriliyor mu? Güzel, ama bunu `unit testi` yazarak kontrol edin. Tarayıcıdan bakarak değil.

Formdan gelen tc kimlik no verisini doğrulayan method doğru çıktıları veriyor mu? Güzel, ama bunu `fonksiyonel test` yazarak kontrol edin. Tarayıcıdan bakarak değil.

Bunu yapmadığınız zaman projenin herhangi biryerinde değişiklik yapmaya korkar olursunuz. Proje ne kadar büyürse projeyi manuel test etmek o kadar zorlaşır ve hata oranı büyük ölçüde artar. Benim gibi projeden projeye atlayan biriyseniz, hangi projenin ne durumda olduğunu aklınızda tutamazsınız.

Projeyi geliştirirken her ihtimali tarayıcı üzerinden test etmiş olabilirsiniz. Formlar doğru veriyi gönderiyor, doğrulamalar çalışıyor, özel fonksiyonlar doğru çalışıyor, veriler veritabanına ekleniyor, bilgilendirme sayfası çalışıyor. Evet, herşey çalışıyor ama emin ol o projeye 1 ay ara verdiğinde birdaha neyin çalışıp neyin çalışmadığını hatırlayamayacaksın.

Bu yüzden kendinizi test yazmaya alıştırın. `F5`'e ne zaman basmanız gerekiyorsa parmağınızı geri çekin ve test yazın. Test işlemini kolaylaştıran `PHPUnit`, `Codeception`, `Selenium`, `Behat` gibi araçları inceleyin ve kullanın.

**Continuous Integration için ayrı bir sayfa açalım...**

Continuous Integration, en basit haliyle yazılan testlerin sıklıkla çalıştırılmasını sağlamaya denmektedir. Yazdığınız testleri saatlik olarak, veya versiyon kontrol sistemlerini kullanıyorsanız her commmitte vb. çalıştırılmasını sağlayabilirsiniz. 

Continuous Integration için Travis CI, Hudson CI, Jenkins CI gibi araçları kullanabilirsiniz. Continuous Integration araçlarını kullanmanın size faydası şudur.

Örneğin, projenize bir arkadaşınız destek oluyor. Bu arkadaşınız geliştirme esnasında uygulamanın bir bölümünü bozmuş olabilir. Bu durumda CI sunucusu sizi uyarır. "Hey, 9a8fja numaralı committen sonra artık butonaTıklandığındaFormVerileriSunucuyaGidiyorMu testi çalışmamaya başladı." Bunun sebebini araştırıp kolayca bulabilirsiniz.

Eğer test yazmazsanız ve bu araçları kullanmazsanız, arkadaşınız size şunu diyebilir. "Bugün anasayfadaki formun tasarımını biraz değiştirdim." Aslında değiştirmedi, bozdu. Siz de test yazmadığınız için burada bir hata olduğunu anca müşteriniz telefon açıp sizi azarlayınca anlayabilirsiniz.

Test konusunu çok önemli olup, son zamanlarda kendi başına bir sektör haline dönüşmeye başladı. Kalite kontrol departmanları sistemi test edecek Test Uzmanları/Mühendisleri çalıştırmaya başladılar. Siz de geliştirdiğiniz uygulamanın kaliteli ve çalışır durumda olduğunu kanıtlamak için testlerinizi yazın. Büyük bir proje geliştiriyorsanız proje geliştirmeyi `F5`'e basarak devam ettiremezsiniz.

Test yazmak bazen zaman alıcı olabilir. Çoğu zaman `var_dump()` yazıp `F5`'e basmak geliştiricilere daha kolay gelir. Bu yüzden artık geliştirilmeyeceğinden emin olduğunuz küçük uygulamaları bu şekilde test edip müşteriye verebilirsiniz. Ama o müşterinin 2 yıl sonra sizi arayıp yeni şeyler istemeyeceğinden nasıl emin olacaksınız, orası ayrı konu.

### - Testlerinizi yazarken string kullanmamaya çalışın. ###

İster `unit` test yazıyor olun, ister `acceptance`, ne kadar az string kontrol ederseniz sizin için o kadar sağlıklı olur.

Mesela acceptance testlerinde `<p>Anasayfa</p>` varmı diye kontrol etmektense header kodunun `200 OK` olduğunu veya `homepage.view.php` çalıştırılmış mı diye kontrol edin. Anasayfa yazısını değiştirmek `zararsız` sayılacağı için kolayca değiştirilebilir. Örneğini, anasayfa yazısı yerine `Font Awesome` ile ev resmi koyabilirsiniz, ama `homepage.view.php` dosyasının adı kolay kolay değişmez. Bu dosyanın adı değişirse, controllerdeki methodlarında birçoğu değişecektir.

Bu yüzden testlerinizi yazarken mümkün olduğunda string kullanmaktan kaçının ve değişmeyecek şeyleri kullanarak testlerinizi yazın.

### - HTTP Response kodlarını öğrenmeye ve kullanmaya çalışın. ###

HTTP Response kodları önemlidir. Örneğin, response kodu `404` olmayan bir `404 `sayfası, gerçek bir `404` sayfası değildir. Google gibi arama motorları bu sayfayı 404 sayfası olarak saymazlar, çünkü bu sayfanın aslında 404 ID'sine sahip  bir kullanıcının profili olup olmadığını bilemezler. Daha sonra arama sonuçlarında bu sayfa ekrana yansır ve sizin için hoş olmaz.

Bu yüzden sizin, tarayıcıların, arama motorlarının ve diğer araçların anlayabileceği response kodlarını kullanın. 

> Not: Tüm HTTP Response kodlarına http://en.wikipedia.org/wiki/List_of_HTTP_status_codes adresinden ulaşabilirsiniz.

### - MVC kullanıyorsanız, MVC gibi kullanın. ###

**a. Controller içerisinde echo'yu unutun.**

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

**b. Viewlar, Controllerdan gelen mümkün olan en az bilgiyle çalışmalıdır.**

Bazen `Controller` sınıfları `View`'e gereğinden fazla veri gönderir ve bu sıkıntı çıkarabilir. Genel olarak bir `Controller`, `View` katmanına mümkün olan en az veriyi göndermelidir.

```php
<?php

class Controller
{
  
    public function test()
    {
        // yanlış
        $link = "http://aristona.github.io/" . $_POST['id'];
        $view->bind('link', $link);

        // doğru
        $link = $_POST['id'];
        $view->bind('link', $link); // aristona.github.io adresi, View katmanında tutulmalıydı
    }
}
```

**c. Zayıf controller sınıfları, şişman modeller. **

// Yakında

### - Notice ve Warning'ler birer bugdur ve düzeltilmeleri gerekir. ###

PHP çok katı kurallara sahip değildir bu yüzden ufak çaplı basit hatalar bazen görmezden gelinir. Bu tür hatalar `E_DEPRECATED`, `E_STRICT`, `E_NOTICE` ve `E_WARNING` olarak kayıtlara eklenir. (Hepsine [http://php.net/manual/en/errorfunc.constants.php]() adresinden bakabilirsiniz.)

Ancak, çoğu `PHP` geliştirici bunları fazla umursamıyor. Buradaki hataların düzeltilmesi, projenizin geleceği için son derece önemlidir.

1. `E_DEPRECATED` kullanılan bir `PHP` özelliğinin ileride PHP'den kaldırılacağını belirtir. (örneğin `mysql` ile başlayan fonksiyonlar)

2. `E_STRICT` yazılan scriptin hatalı olabileceğini ve sıkı kurallara uyulmasını gerektiğini belirtir. (örneğin statik olmayan bir methodu statik olarak çağırmak)

3. `E_WARNING` runtime esnasında ortaya çıkan uyarılardır.

4. `E_NOTICE` runtime esnasında ortaya çıkan bildirimlerdir. Warning kadar önemli değildir. (örneğin favicon ikonunun bulunamaması gibi)

Bu yüzden geliştirme ortamınızda hata raporlamanın açık durumda olması ve hertürlü hata seviyesindeki sorunları çözüyor olmanız sizin için bir avantajdır.

### - Hataları analiz etmek için backtrace kullanın. ###

Uygulamanızda bir hata aldınız, ancak neden böyle bir hata aldığınızı bilmiyorsanız PHP'in `backtrace` (geri sarma) özelliğini kullanabilirsiniz.

PHP, siz istemedikçe hatalar hakkında stack trace bilgisi vermez. Sadece `X dosyasının 55. satırında hata oluştu` der.

Stack trace hakkında bilgi sahibi olmak için:

1. `Whoops` adlı paketi (http://filp.github.io/whoops/) uygulamanızda kullanabilirsiniz. (önerilen)

2. `debug_backtrace()` fonksiyonunu kullanarak stack traceyi kendiniz takip edebilirsiniz.

Normalde hiçbir PHP kütüphanesinden bu yazımda bahsetmeyecektim, ama bana kalırsa en yararlı kütüphanelerden biri şuan `Whoops` bu yüzden ondan bahsetmek istedim. Kullanmamanız için hiçbir sebep yok.

### - @ kullanmayın. ###

`@`, PHP'de muhtemel hataların ekrana yansımaması için onları sessizleştirir. Siz istediğiniz kadar hataları sessizleştirin, susturun, onlar oluşmaya devam edecektir. Bu yüzden hataları susturmak yerine çözmeye çalışın.

### - ?> kullanmayın. ###

`PHP` kullanırken açılan tagları `?>` kullanarak kapatmanıza gerek yok. Kapattığınız takdirde `?>` tagından sonra yeni satır veya boşluk gibi karakterler kalabiliyor. Bu karakterler PHP tarafından `output` (çıktı) olarak algılandığı için `Headers already sent` hatası, sessionların oluşturulamaması, header kodlarının değiştirilememesi gibi hatalarla karşılaşabiliyorsunuz.

Bu yüzden, `good practice` (İyi Kullanım), kapanış taglarını kullanmamaktır.

### - Short tags kullanmayın. ###

Bazı yazılımcılar `<?php` yerine `<?` kullanıyor. Bu son derece yanlış. Short tag kullanacaksanız `short_open_tag` mutlaka açık konumda olmalıdır. Kapalı olursa ne olur? Kaynak kodlarınız tarayıcıya çıkabilir.

Dünya çapında birçok web sitesi bu tür basit dikkatsizliklerin kurbanı olabiliyor. Türkiye'de bildiğim kadarıyla `sahibinden.com` var, kaynak kodu tarayıcıya çıkarmayı başarabilen (!) ender firmalardan.

Bu yüzden, `<?php` dışında hiçbir `açılış etiketini` kullanmayın.

> Not: `PHP 5.4` ve üzeri versiyonlarda, kısa ekrana yazdırma etiketi olan `<?=` kullanılabilir, ancak açılış etiketleri yine `<?php` olarak yazılmalıdır.

### - Projelerinizi açık kaynaklı olarak paylaşıyorsanız, .gitignore kullanın! ###

Yanlışlıkla sunucu, ftp, veritabanı veya API bilgilerinizin olduğu dosyaları `Github`'a yüklemeyin. Gizli kalması gereken dosyaları düzgün şekilde `.gitignore` kullanarak gizleyin. Daha sonra silseniz bile commit logları kaldığı için başkaları tarafından görünebiliyorlar. Özellikle veritabanı ve API bilgilerinin tutulduğu konfigürasyon dosyaları, mümkün olduğunca ambar üzerinde tutulmamalıdır.

### - Kullandığınız açık kaynaklı projelerin güvenli olduklarından emin olun. ###

Aşağıda mükemmel yazılımcıların açık kaynaklı projeleri yer alıyor. Bedava `VPS` isteyen var mı?

[https://github.com/search?q=exec+sudo+%24_GET&type=Code]()

Sorunu anlamayanlar varsa anlatmaya çalışayım. Buradaki scriptlerin bazıları, gelen `GET` isteğini kontrol etmeden direkt olarak linux üzerinde yönetici yetkisiyle çalıştırıyorlar. Yani birisi "Hey linux, bana sunucunun şifresini verir misin?" diye sorabilir veya "Hey linux, kendine format atar mısın?" tarzındaki komutları çalıştırabilir.

Açıkcası kullanacağınızı hiç sanmıyorum ama, siz yine de kimin yazdığı belli olmayan açık kaynaklı projeleri kullanmamaya çalışın. Bunların size hackerların sokmaya çalıştığı `shell` dosyalarından hiçbir farkı yok. Hackerların size sokmaya çalıştığı `shell` dosyalarını siz kendiniz sitenize yüklemeyin.

### - Composer kullanın. ###

// Yakında

### - Statik fonksiyon kullanmayın. ###

// Yakında

### - PHP asenkron çalılabilir. ReactPHP'i tanıyın. ###

// PHP nasıl nonblocking IO'ya sahip olabilir ve nasıl çalışır.

### - Ağır sorguları önbellekleyin. ###

// Yakında

### - PSR standartlarına uyun. ###

// PHP-FIG nedir? PSR nedir? Tüm standartlar açıklanacak

---
# Frontend #
---

## CSS ve HTML ##

### CSS Explosiona maruz kalmayın. ###

// CSS explosion nedir? Nasıl korunulur?

### Precompiler kullanın. ###

// Popüler CSS precompilerlar, + SASS örneği

### Semantic HTML yazın. ###

// Neki bu semantic?

### Ayraçları HTML ile yazmayın. ###

// CSS pseudo selectorlere bir giriş

### Duplicate HTML yazmayın. ###

// Kendinizden nefret ettirmenin yolları

### Inline stil ve inline javascriptten kaçının. ###

// Yakında

### Assetleri yüklerden http veya https kullanmayın. ###

// //link şeklinde olmalı.

### YAZILARINIZI BÜYÜK HARFLE yazmayın! ###

// text-transform: uppercase;

### Linkleri doğru şekilde yazın. ###

Linkler yazılırken `www` yazılmamalı ve slash ekli olmalıdır.

Örneğin `http://www.anilunal.com` hatalıyken, doğru olan `http://anilunal.com/`'dur.

### Tarayıcı uyumluluğunu test edin. ###

// Browserstack, caniuse, shim/modernizer vs

### Fazla opacity ve alpha kullanmayın. ###

// GPU

### ID seçicilerini gerekmedikçe kullanmayın. ###

// Sebep

### CSS selectorlerinizi kısa tutun. ###

// Yeah

## Javascript ##

### Tanımlamaları çoğu zaman Javascript Object Literals kullanarak yapın. ###

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

a. Object literallerin keyleri `attribute`dir, JSON'un `string`dir.

```js
var user = { name: "Anıl" } // Object literal
var user = { "name": "Anıl" }  // Object notation (Json)
```

b. Object notation'da method tanımlanamaz, object literal'de tanımlanabilir. Yukarıdaki örnekte `isOld()` bir methoddur.

c. JSON'lar genellikle veri taşımak (API'lerde) ve veri saklamak için kullanılırken, object literaller genellikle OOP amacıyla kullanılır.

### Debug için alert kullanmayın, lütfen! ###

// Rly.

### Hoisting hakkında bilgi sahibi olun. ###

// Nedir?

### Lambda kullanın. ###

// jQuery örneklerinden

### Bir selector kullandığınız zaman onu önbellekte tutun. ###

// Sebep?

## Geliştirici Ortamı ##

### Sublime Text ###

// Kullanmayanlar bu bölümü esgeçsin.

### GruntJS ###

**a. Ayak işlerimizi Grunt'a yaptıralım.**

**b. Kendi Grunt modülümüzü yazalım.**

### Kullanabileceğiniz diğer araçlar. ###

// Yakında

### Kullanmamanız gereken araçlar. ###

// DreamWeaver!!1

### Gözlerinizi koruyun. ###

// Yakında

### Terminale ne kadar yakın, o kadar iyi. ###

// Yakında

### Farenizi değil, klavyenizi hızlandırın. ###

// Yakında

## Veritabanları ##

### Neden `utf8mb4_unicode_ci?` ###

// Yakında

## Deployment ##

### Capistrano ###

// Yakında

### FTP kullanmayın! ###

// Sebepleri ve alternatif kullanımları

## Versiyon kontrol ##

### Sus ve git kullan. ###

// Kaçışın yok

### Commit mesajlarınız açıklayıcı olsun. ###

// Niye?

## Hosting ve sunucu ##

### Shared hostingler artık öldü. ###

// Yakında

### Neden PaaS? ###

// Yakında

> Not: Bu makaleyi vakit buldukça güncelleyeceğim.