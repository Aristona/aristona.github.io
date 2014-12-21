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

---
# Giriş #
---

### Çıkış noktası ###

Neredeyse her gün sosyal platformlarda aynı hataların ve yanlış düşüncelerin tekrarlandığını gördüğüm ve hep aynı şeyleri söylemekten sıkıldığım için bu yazıyı yazmaya karar verdim.

### Amaç ###

Bu yazıyı yazmaktaki amacım yazılım konusunda insanlara farklı bir bakış açısı kazandırmaktır.

### Kapsam ###

Bu yazımda web geliştiricilerin bilmesi gereken konuları, yapılan yanlışları, bu yanlışların nasıl düzeltebileceğini ve genel olarak yazılım dünyasında kabul görmüş `good practice` (İyi kullanım) durumlardan bahsedeceğim.

Yazı boyunca web geliştirme süreçlerinin tamamına değineceğim. Bu yüzden geliştirme ortamı, veritabanları, back end, front end, css, otomasyon, deployment, versiyon kontrol vb. konularda bilmeniz gerekenleri anlatmaya çalışacağım. Anlattığım bazı konular son derece temel konular olabileceği gibi, bazıları da üst düzey olabilir. Her şeyi bilmeniz şart değil, ancak biliyor olmanız size birçok konuda avantaj sağlayabilir.

### Kaynak ###

Bu yazı açık kaynaklı olarak `GitHub` hesabım üzerinde yayınlanmaktadır. Açık kaynaklı olduğu için kolayca güncel tutmayı planlamaktayım. Eğer herhangi bir yanlışlık görürseniz veya eklemek istedikleriniz olursa `pull request` atabilirsiniz. Merak ettiğiniz konuları da `issue` oluşturarak belirtebilir veya bu sayfaya yorum bırakabilirsiniz.

Bu yazı, [https://github.com/Aristona/aristona.github.io](https://github.com/Aristona/aristona.github.io) üzerindeki `repository` (ambar) üzerinde tutulmaktadır. Format olarak `Markdown (Redcarpet)` kullanılmıştır, bu yüzden `pull request` attığınızda, yazılarınızın bu formata uygun olması gerekmektedir.

### Anlaşma ###

1. Bu yazıda hatalar bulunabilir. Yazım hataları bulunabilir. Çalışmayan örnekler olabilir. Benim de doğru bildiğim yanlışlar olabilir. Bu durumda yapmanız gereken, "Bu yanlış! Ne kötü bir yazı bu!" demek yerine, eğer yapabiliyorsanız, gerekli düzeltmeleri yapıp `pull request` atmanızdır. Eğer yapamıyorsanız, araştırmam için gerekli kaynakları ve hatalı bölümü `issue` oluşturarak bana bildirebilirsiniz.

2. Bu yazı bir kitap veya akademik bir makale değildir. Sadece, yıllardır tecrübe ettiğim bilgilerin ve tuttuğum notların, insanların kolayca ulaşabileceği, okuyabileceği ve anlayabileceği bir formatta yayınlanmasından ibarettir.

3. Birçok konuyu "5 yaşındaki birine anlatır gibi" anlatıp, genel olarak fazla detaya inmeden yüzeysel bir anlatım yapacağım. Örneğin, ben `GruntJS` isimli uygulamayı ve alternatiflerini tanıtacağım ve size nasıl bir yarar sağlayacağını birkaç basit örnekle anlatacağım; fakat size özel bir Grunt modülü yazmayacağım. Amacım sizlere balık vermek değil, balık tutmayı öğretmek. Burada yüzeysel olarak öğrendiklerinizi araştırıp kendinizi geliştirebilirsiniz.

### Destek ve Beklenti ###

Bu yazıdan hiçbir ticari beklentim yoktur. Kendiniz bir yazı göndermek isterseniz (web geliştirme ile ilgili olmalı ve mutlaka kabul görmüş bir good practice olmalı) `pull request` atarak yazınızı gönderebilirsiniz.

---
# Değişiklikler #
---

Bazı okuyucular yorumlarında yazıda nelerin değiştiğini takip edemediklerini söylemişlerdi. Aslında versiyon kontrol bu sorunu çözüyor olsa da, ben yine de okuma kolaylığı olması açısından bir güncelleme yaptığımda nelerin güncellendiğini bu bölümde belirteceğim. Buradaki değişiklikler 24.07.2014 tarihi sonrasını kapsayacaktır. (Ufak değişiklikler ve yazım hatalarının düzeltilmesi burada belirtilmeyecektir.)

**09.09.2014**

- CSS bölümüne **Temel attributeler dışında diğer HTML attributelerini kullanmayın.** alanı eklendi.
- CSS bölümüne **Inline stil yazmaktan kaçının.** alanı eklendi.
- CSS bölümüne **CSS Inheritence** alanı eklendi.
- CSS bölümüne **height propertyini kullanmayın.** alanı eklendi.
- CSS bölümüne **`<br>` kullanmayın.** alanı eklendi.
- CSS bölümüne **`&nbsp;` kullanmayın.** alanı eklendi.
- CSS bölümüne **Açıkta element bırakmayın.** alanı eklendi.
- CSS bölümüne **Negatif piksel margini çok kullanmayın.** alanı eklendi.
- CSS bölümüne **Ayraçları HTML ile yazmayın.** alanı eklendi.

**27.08.2014**

- Genel bir imla ve hata denetimi yapıldı.

**09.08.2014**

- Giriş bölümü düzenlendi.
- Deployment bölümüne **FTP kullanmayın** alanı eklendi.
- Deployment bölümüne **DeployHQ ve türevi servisler** alanı eklendi.
- Deployment bölümüne **Capistrano** alanı eklendi.
- JavaScript bölümüne **Debug için alert kullanmayın, lütfen!** alanı eklendi.

**25.07.2014**

- JavaScript bölümüne **; prefixini kullanın.** alanı eklendi.
- JavaScript bölümündeki **Assetleri yüklerden http veya https kullanmayın.** alanı geliştirildi.
- JavaScript bölümüne **Daima 'use strict'; kullanın.** alanı eklendi.
- JavaScript bölümüne **JavaScript hataları izlenebilir. İzleyin.** alanı eklendi.
- JavaScript bölümüne **Asenkron, deferred ve DOM eventlerinden bağımsız yüklemeler yapın.** alanı eklendi.

---
# Back end (Arka yüz) #
---

Back end için kullanacağımız ana programlama dili `PHP` olmakla beraber, birçok örnek direkt olarak `yazılım mimarileri` ile ilgili olduğu için diğer programlama dillerinde de kullanılabilir.

> Not: Bu bölümdeki örneklerin bazıları, temel veya orta düzeyde `PHP` bilgisi ile Nesne Yönelimli Programlama (OOP) bilgisi gerektirmektedir.

### - Global scope'u asla kirletmeyin. ###

**a. Değişkenlerinizi global scope içerisinde tanımlamayın.**

`Global scope` (global etki alanı) içerisinde tanımladığınız değişkenler uygulamanızın her yerinden erişilebilir olurlar. `Global scope` içerisindeki değişkenlere ne kadar bağımlı olursanız, uygulamanın farklı bir noktasında hata yapma olasılığınız o kadar artar. Özellikle 3. parti pluginleri, komponentleri veya kütüphaneleri kullandığınız zaman, onların uygulamanızı kötü etkilemeyeceğinden emin olamazsınız.

İsterseniz başlamadan önce `scope` kavramının ne olduğundan ve `PHP`'de nasıl kullanıldığından kısaca bahsedelim.

```php
<?php
    $global = "Global scope içerisindeki değişken";

    function ()
    {
        $fonksiyon = "Function scope içerisindeki değişken";
        echo $fonksiyon; // Çıktı: Function scope içerisindeki değişken
    }

    echo $global; // Çıktı: Global scope içerisindeki değişken
    echo $fonksiyon; // Çıktı: Undefined variable (tanımlanmamış değişken) hatası
```

Yukarıdaki örnekte `$global` değişkeni `global scope` içerisinde tanımlanmıştır. Bu yüzden uygulamanın her yerinden erişilebilir olur. Buna karşın, `$fonksiyon` değişkeni `function scope` içerisinde tanımlanmıştır ve sadece o fonksiyon içerisinden erişilebilir olur.

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

yazdığı zaman, artık `$veritabani` değişkeni `NULL` değerine sahip olacağı için, hiçbir veritabanı işlemi yapılamaz hale gelecektir. Bu durumda uygulamanız `runtime esnasında` bozulacaktır.

Bu tür hatalardan korunmak için tanımlayacağınız değişkenleri `class scope` (sınıf etki alanı) altında, sınıflarınızı da `namespace` (isim uzayları) altında tanımlamaya çalışın.

**b. Encapsulation (Kapsülleme) yapın, gizli değişkenlere erişimi kesin.**

`Class scope` içerisinde tanımladığınız değişkenler de, eğer `visibility` (PHP Görünürlük olarak çevirmiş ama bana kalırsa erişilebilirlik olmalı) belirtilmemişse dışarıdan erişime açık olurlar. Siz her şeyi `class scope` içerisinde yazmış olabilirsiniz, ancak uygulamanın herhangi bir yerinde `new` keywordü kullanılarak sınıfların instancesi oluşturulabilir ve buradan değişkenlere erişim sağlanabilir. Özellikle `singleton` (sınıfların sadece bir kopyasının oluşmasına izin veren tasarım deseni) gibi bir `antipattern` (sık sık karşılaşılan bir problemi ileriye dönük zararlı olacak ve inefektif şekilde çözmeye çalışmak) kullanıyorsanız, uzun vadede sorunlarla karşılaşmanız muhtemeldir.

Biliyorum, aklınızda "Nasıl 5 yaşındakilere anlatıyormuş bu makaleyi?" sorusu var, ama terimleri açıklamam gerekiyor. Yukarıda bahsettiğim terimlerin Türkçe çevirilerini bulamadım (sürpriz) bu yüzden elimden geldiğince yazılımsal terimleri İngilizce tutup çevirilerini parantez içinde vermeye çalışacağım.

Konumuza dönelim. Oluşturulan bir değişkenin veya nesnenin içeriğinin beklenmedik bir şekilde değiştirilmesi uzun zamandır problem oluşturdu. Siz `$veritabani` diye bir değişken oluşturabilirdiniz, ancak başka birisi onu `NULL` diye değiştirebilirdi, çünkü erişimleri vardı, çünkü siz o değişkeni global scope altında tanımlanmıştınız. Kullandığınız bir yardımcı PHP scripti `$veritabani` adında bir değişken oluşturmuş ve o değişken üzerinden işlem yapıyor olabilir. Bu yüzden global scope üzerinde değişken tanımlamak kullanmak daima kötü bir kullanım sayılır.

Sınıf yapısı bu sorunu biraz olsun çözüyor. Sınıf içerisinde oluşturulan bir değişkenin `görünürlüğünü` ayarlamak için, `OOP`ın (Nesne Yönelimli Programlama) 4 temel ilkesinden biri olan `Encapsulation` (Kapsülleme) özelliğini kullanabiliriz.

Bildiğiniz gibi `PHP`'de `class scope` içerisindeyken `public`, `private` ve `protected` keywordlerini kullanarak değişkenlerin ve/veya fonksiyonların dışarıdan erişilip erişilemeyeceğini belirtebiliyoruz. `Kapsülleme` yapmak için erişimine izin vermek istemediğiniz bir değişkeni `private` veya `protected` keywordlerini kullanarak oluşturduktan sonra, sınıf içerisinde `public` bir fonksiyon oluşturup, oluşturulan fonksiyon üzerinden gizli değişkeni döndürebiliriz. Örnek olarak basit bir KDV hesaplayıcı sınıf yazalım. 1500 liralık bir ürün ve 0.18 KDV değeri olsun.

```php
<?php namespace KDV;

class Calculator
{

    public $ratio = 0.18;
    public $price = 1500;

    public function calculate()
    {
       return $this->price + ($this->price * $this->ratio);
    }
}
```

Şimdi, bu sınıfımızın bir instancesini oluşturup hesaplamayı yaptıralım.

```
$calculator = new KDV\Calculator;
$calculator->calculate(); // Çıktı 1770
```

Güzel. İstediğimiz sonucu alabiliyoruz. Ama farzedelim, bir şekilde `$ratio` değişkenine erişim sağlandı ve değer değiştirildi. Bu haliyle erişim sağlanacaktır, çünkü `$ratio` değişkeni `public`, yani herkesin erişebileceği şekilde tanımlanmıştır.

```
$calculator = new KDV\Calculator;
$calculator->ratio = 10; // değere erişim yetkimiz var
$calculator->calculate(); // Çıktı 16500
```

Tebrikler. 1500 liralık ürün için müşteriye fiyatı 16500 lira olarak gösterdiniz. Her zaman bu olay bu kadar açık şekilde olmayacak, ama sorunu görebiliyor musunuz? Belki yanınıza yeni katılan yazılımcıya "haftasonları kdv değerini düşüren" bir sınıf yaz dediniz. O da haftasonları `$ratio` değerini değiştiren bir sınıf yazdı.

Kendimize soralım. Buradaki `$ratio` değişkeni bizim için önemli mi? Evet! Kimse kafasına göre o veriye erişip değiştirememeli. En azından belli bir kontrolden geçmeli. Bu kontrolü de siz kendiniz oluşturacaksınız. %1000 vergi nerede görülmüş?

Şimdi `$ratio` değişkeninin görünebilirlik değerini `public` yerine `private`, yani sadece kendi sınıfımızdan erişilebilecek şekilde düzenleyelim.

```
private $ratio = 0.18;
```

Bu durumda

```
$calculator = new KDV\Calculator;
$calculator->ratio = 10; // Exception fırlatacaktır çünkü erişilemez bir veriye ulaşmaya çalışıyoruz
$calculator->calculate();
```

yazılımcı arkadaşımız bu değere ulaşmak isterken hata alacaktır. Siz böyle yaparak onun bu değere ulaşmasını engellemiş oldunuz. Bunun bir çözümü olmalı. Şansızlıyız ki var! Şöyle yapsak... değişkeni gizli tutsak, ama o değişkenin içeriğine erişebilen bir `public` fonksiyon oluştursak?

```php
<?php namespace KDV;

class Calculator
{

    private $ratio = 0.18;

    public function getRatio()
    {
        return $this->ratio;
    }

}
```

Şimdi, public olan fonksiyon üzerinden private olan değişkene erişmeyi deneyelim.

```
$calculator = new KDV\Calculator;
$calculator->ratio = 10; // Erişemez
$calculator->getRatio(); // 0.18
```

Method üzerinden erişebiliyoruz! Bu tür methodlara `getter` methodlar denmektedir. Daha iyi anlamak adına bu durumu inceleyelim:

1. Global scope içerisinden, sınıf içerisindeki `getRatio()` methoduna erişebilecekmiyiz? Evet, çünkü `public` olarak tanımlanmış.
2. Sınıf scope içerisindeki `getRatio()` methoudu `private` olan `$ratio` değişkenine erişebilecek mi? Evet, `getRatio()` o sınıfın bir üyesi.

Peki, yazılımcı arkadaşımız bu veriyi değiştirmek isterse ne yapacak? Tekrar bir `public` oluşturacağız ve bu kez, veriyi döndürmek yerine, verinin içeriğini değiştireceğiz.

```php
<?php namespace KDV;

class Calculator
{

    private $ratio = 0.18;

    public function setRatio(float $ratio)
    {
        if( $ratio > 0.20) // 0.20'den fazla veri olamasın
        {
            throw new \RatioException(sprintf("Ratio cannot be higher than 0.20, %s given.", $ratio))
        }

        $this->ratio = $ratio;
    }

}
```

Artık bu method üzerinden istediğimiz değişiklikleri yapabileceğiz. Bu tür fonksiyonlara `setter` fonksiyonlar denmektedir.

```
$calculator = new KDV\Calculator;
$calculator->setRatio(0.20); // Erişemez
$calculator->getRatio(); // 0.20
$calculator->calculate(); // 15800
```

Eğer `0.20`'den fazla bir oran girilirse, yazdığımız sınıf exception fırlatıp muhtemel hataların önüne geçecektir. Buna `Encapsulation` (Kapsülleme) denmektedir ve `Nesne Yönelimli Programlama`'nın 4 temel ilkelerinden biridir.

> Biliyor musunuz?

`C#` dilinde `getter` ve `setter` methodlar kolayca oluşturulabilmektedir.

```php
public class Database
{
    public string info { get; set; } //getter ve setter oluşturuldu
}
```

> Biliyor musunuz?

`Ruby` dilinde `getter` ve `setter` oluşturmak çok basittir.

```php
class Database
    attr_accessor :info //getter ve setter oluşturuldu
end
```

`attr_accessor`, Ruby dilinde `info` değişkeninin getter ve setter fonksiyonlarını otomatik olarak oluşturur. Maalesef `PHP`'de böyle bir kullanım bulunmamaktadır. Biz getter ve setter fonksiyonlarımızı çoğu zaman elle yazmak zorundayız.

Bilmeniz gereken bir başka konu daha var. `PHP`'de eğer `getter` ve `setter` methodlar bulunamazsa, `PHP`'nin `sihirli method`larından olan `__get()` ve `__set()` devreye girerler.

// Buraya `__get` ve `__set()` hakkında örnekler gelecek.

> Önemli: Fonksiyonlar global scope içerisinde tanımlanan fonksiyonlara denir. Methodlar ise sınıf scope içerisinde tanımlanan fonksiyonlara denir.

### - Methodlarınızı ve sınıflarınızı küçük tutun. ###

Bu görüş farklı yazılımcılar tarafından farklı algılanmakla beraber, genel kanı aşağıdaki altın kurala uymanın bize avantaj sağlayacağı yönündedir.

1. Bir methodda maksimum `10 satır kod` bulunmalıdır.
2. Bir sınıfta maksimum `10 method` bulunmalıdır.
3. Bir sınıfta maksimum `4 dependency` (bağımlılık) bulunmalıdır.
4. Bir pakette/komponentte maksimum `15 sınıf` bulunmalıdır.

Peki, nedir bu bağımlılık?

Bağımlılık, oluşturduğumuz sınıfın çalışabilmesi için gerekli olan diğer sınıfların toplamıdır. Bağımlılıklar `use` kullanılarak, `constructor injection` aracılığıyla veya class scope içerisinde bağımlılık sınıflarının `instantiate` edilmesiyle vb. yöntemlerle eklenebilir.

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

1. `DatabaseInterface` `interface`'sine sadık kalmış herhangi bir sınıfa bağımlıdır.
2. `Image` sınıfına bağımlıdır.
3. Global `uzaydaki` (namespace) `Logger` sınıfına bağımlıdır.

Eğer bu sayı `4`'ün üzerine çıkarsa, sınıfınız gereğinden fazla iş yapıyor olabilir. Dolayısıyla hem bu sınıfı yönetmek zorlaşır, hem de `SOLID ilkeleri`nin birincisi olan `Single Responsibility Principle` (Tek amaç ilkesi) ilkesini ihlal etmiş oluruz.

`Tek Amaç İlkesi`'ne uymak için, sınıfımızı ve methodlarımızı fazla şişirmemeli ve küçük tutmalıyız. Ayrıca, sınıfımız ile methodlarımızın ne iş yaptığını anlatırken `ve` kelimesini mümkün olduğunca az kullanmalıyız. 

Örneğin, kullanıcının kayıt olması için hayali bir method oluşturduğumuzu farzedelim ve bu methodun ne iş yaptığını kendimize konuşur gibi anlatalım:

1. Kullanıcıdan gelen `$_POST` verilerini alıyoruz VE
2. Bu verileri oluşturduğumuz `Validation` (Doğrulama) kontrollerinden geçiriyoruz VE
3. Kullanıcı avatar resmi yüklediyse, avatar resmini boyutlandırıyoruz VE
4. Boyutlandırılan avatarı isimlendirip bir klasör içerisine yerleştiriyoruz VE
5. Veritabanına bağlanıp kayıt olacak kullanıcının bilgilerini ekliyoruz VE
6. Eğer veritabanı false döndürmüş veya exception fırlatmışsa bunu yakalıyoruz VE
7. Ekrana başarılı veya başarısız olacak bir sayfa çıktısı veriyoruz.

Gördüğünüz gibi bu methodun ne iş yaptığını açıklarken 6 defa `VE` kullandık. Bu yanlış bir kullanımdır. Bu method gereğinden fazla iş yapmaktadır. Burada gerçekleştirilen işlemlerin bazılarını farklı sınıflara veya methodlara dağıtmamız bizim `Tek Amaç İlkesi`'ne sadık kalmamızı sağlayacaktır.

> Not: Bu maddenin bir kural değil, bir görüş olduğunu hatırlatmalıyım. Sayılarda ufak oynamalar olabileceği gibi, çok büyük oynamalar Tek Amaç İlkesi'nden çıktığınız anlamına gelebilir.

Şimdi biraz düşünelim. Kayıt sınıfı yazdığımıza göre, `Kayıt` sınıfının amacı, kullanıcıyı başarılı bir şekilde veritabanına kayıt ettirmek olmalıdır. `Doğrulama`'ların, avatar resminin yeniden boyutlandırılmasının, yeniden boyutlandırılan resmin bir klasöre yerleştirilmesinin `Kayıt` sınıfıyla bir ilgisi bulunmamaktadır. Bu işlemler farklı sınıflarda yapılmalıdır.

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
         return "ab kelimesi abcde içerisinde geçmiyor. Gerçekten."; //doğru
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

**a. Dependency Injection (Bağımlılık Enjeksiyonu)**

Kendisine üst düzey bir `PHP Geliştirici` diyen herkesin mutlaka bilgi sahibi olması gereken konular olduğu için bu terimlerin ne olduğunu ve hangi amaçla kullanıldıklarını anlatma ihtiyacı hissediyorum.

Öncelikle, `Dependency Injection` (Bağımlılık Enjeksiyonu), `James Shore`'un dediği gibi: "5 centlik konsept için 25 dolarlık terim kullanılması." sebebiyle, insanın kulağına sanki çok karışık bir konuymuş gibi geliyor. Bu söze ben de katılıyorum; çünkü ben de `bağımlılık enjeksiyonu`'nun mantık olarak son derece basit olduğunu düşünenler arasındayım. Hatta, şuana kadar verdiğim örneklerde birkaç defa kullandığım oldu.

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

// class scope'un dışında, uzaklarda bir yerlerde
new Deneme(new Mailer);

```

`Dependency Injection` işte bu kadar basit! Sınıfımız `Mailer` sınıfını kendisi oluşturmaktansa, dışarıda oluşturulan `Mailer` sınıfının `constructor injection` (Enjeksiyonu constructor üzerinden yapmak) aracılığıyla sınıfımıza enjekte edilmesi yoluyla `Mailer` sınıfına sahip oluyor. Böylece sınıfımız `Mailer` sınıfına sıkı sıkıya bağlı olmaktan çıkıyor ve test edilebilirliğimiz muazzam düzeyde artıyor. Artık `unit testlerimizi` yazarken, `Mailer` sınıfını kolayca taklit edebiliriz (taklit etme olayına `Mocking` denmektedir).

**b. Liskov's Substitution Principle**

Yukarıdaki örnek doğru bir `Dependency Injection` örneği olmasına rağmen bir eksikliği bulunmaktadır: Yazdığımız sınıf, `SOLID İlkeleri`'nden biri olan `Liskov's Substitution Principle`'ı (Liskov'un İkame Kuralı) ihlal etmektedir. `Liskov'un İkame Kuralı` bize der ki; "Sınıflar, başka sınıflar yerine `Abstraction`'lara (Soyutlamalara) bağlı olmalıdır."

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

Bu konuyu bir örnekle açıklayalım. Öncelikle `Interface`'mizi oluşturalım ve bu `interface`'e uyacak her sınıfta hangi methodların bulunması gerektiğini belirleyelim.

```php
<?php

interface MailerInterface {
    public function mail();
    public function attachFile();
    public function setSender();
    public function setTo($to);
}

```

Daha sonra `Mailer` sınıfımızın bu `interface`'e uymasını sağlayalım.

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

Şu an bu `Mailer` sınıfı, `MailerInterface` `interface`'ine sadık kaldığı için çalışabilecektir. Ancak, örneğin `Mailer` sınıfında `setTo()` methodu olmasaydı, sınıfımız `MailerInterface` içerisinde şart koşulan `setTo()` methoduna sahip olmadığı için çalışamayacaktı. Kısacası, kullandığımız `interface`, sınıfımızın sahip olması gereken methodları şart koşmaktadır. `Interface` içerisinde belirtilen 4 methodun hepsi bulunmayan sınıflar bu `interface`'e sadık kalamazlar.

`Liskov'un İkame Kuralı`'na uymak istediğimiz için, öncelikle tür dayatmamızı `interface` olarak değiştirelim `interface`'ler de birer soyutlama katmanı sayılırlar).

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

Artık `Deneme` sınıfımız, `MailerInterface` `interface`'ine sadık kalan herhangi bir sınıfı kabul edecektir.

Anlamadınız mı? Sorun değil, bu durumu hemen dünyevi bir örnekle anlatayım.

Aracınızla uzun bir yola gittiğinizi farzedelim ve benzininiz bitmek üzere. Benzin almak için bir benzinliğe uğradınız.

1. Eğer benzinliğe (sınıfa) bağlı olsaydınız, Türkiye'deki tek benzinciden benzin alabilirdiniz.
2. Depoya benzin yerine mazot doldurulmadığından emin olamazdınız.

Ne kadar saçma değil mi? O kadar yolu aynı benzinciye gitmek için geri dönmemiz gerekecek. Ama biz Türkiye'deki herhangi bir benzinliğe girip benzin alabilmek istiyoruz. `Interface`lere bağlı kalmak, bizim Türkiye'deki tüm benzincilerden benzin alabiliyor olmamızı sağlar. Çünkü, biz eminiz ki her benzinlikteki benzin pompası bizim aracımızın benzin deposuna uygun. Biz eminiz ki, pompadan çıkan şey (benzin) bizim aracımızın kabul ettiği bir şey. Her benzin istasyonu bu tür standartlara sadık kaldığı için istediğimiz benzinliğe girebiliriz.

Şimdi yukarıda verdiğimiz örneği yazılımsal bir örnekle anlatalım.

Mail göndermek için tek bir `Mailer` sınıfı yerine, `SwiftMailer`, `SMTPMailer`, `AWSMailer` veya `MandrillMailer` gibi sınıfları kullanabiliriz. Oluşturduğumuz `interface`'e uygun oldukları sürece canımız hangisini isterse o sınıfı kullanabiliriz.

Peki bu bize ne avantaj sağlar? Belki çalıştığımız işyeri artık maillerin `Mandrill API`'si kullanılarak gönderilmesini istedi. Tüm mail gönderme sistemini baştan mı yazacağız? Hayır. Bir `interface`'e sadık kaldığı sürece her mail sınıfını kullanabiliriz.

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

1. Deneme sınıfı, `SwiftMailer` sınıfını kabul edecek mi? Evet. Çünkü `interface`'mize sadık kalmış.
2. Deneme sınıfı, `MandrillMailer` sınıfını kabul edecek mi? Evet. Çünkü `interface`'mize sadık kalmış.
3. Deneme sınıfı, `AWSMailer` sınıfını kabul edecek mi? Evet. Çünkü `interface`'mize sadık kalmış.
4. Deneme sınıfı, `BenzinPompasi` sınıfını kabul edecek mi? Muhtemelen hayır! Çünkü `interface`'mize sadık kalıp kalmadığını bilmiyoruz; fakat isminden yola çıkarak kalmadığını söyleyebiliriz.

Sonuç olarak, `Liskov'un İkame İlkesi`'ne artık uyabiliyoruz. Artık bir sınıfa bağlı olmak yerine, bir `interface`'e bağlıyız!

Peki bu bize ne avantaj sağlayacak? Artık işyerindeki patronunuz mail göndermek için `Mandrill` kullanalım derse, `MandrillMailer`'i enjekte edebiliriz. `AWSMailer`'e dönelim derse, tek satırı değiştirerek `AWSMailer`'e dönebiliriz.

```php
<?php
     $mailer = new Mailer(new canimizNeIsterse());
```

**c. Dependency Injection konteynerleri**

`Dependency Injection` kavramını öğrendik. `Liskov'un İkame İlkesi`'ni öğrendik. Birçok şey öğrendik ama hala öğrenmemiz gereken konular var.

Şuana kadar her şey güzel, ama yüzlerce `Deneme` sınıfımız varsa ne yapacağız?

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

Hepsine tek tek `new Mandrill();` mi diyeceğiz? Hayır. Daha önce ne anlatmıştık? "DRY kuralına uyacağız ve kendimizi tekrar etmeyeceğiz."

`Dependency Injection Containers` (Dependency Injection Konteynerleri), Dependency Injection (kısaca DI) konusunda bize yardımcı olan konteyner sınıflardır. Hangi sınıfın nereye bağımlı olduğuna, nereye enjekte edileceğine çoğu zaman bu konteyner sınıflar karar verir ve bize yardımcı olur.

`Interface` kullanırken, şöyle bir sorunla karşı karşıya kalabiliriz: `Interface`'ler, sınıflar gibi instantiate edilemezler, yani `new` kullanarak onları çalıştıramayız.

Şimdi, çok basit bir algoritma geliştirip Dependency Injection konteynerimizi oluşturacağız. Bu konteyner, `MailerInterface` tür dayatmasını gördüğü yerde, bizim belirlediğimiz sınıfı çalıştıracak ve enjekte edecek.

Örneğin, biz konteyner sınıfında `Mandrill`'i seçersek, konteyner sınıfı bundan böyle `MailerInterface` gördüğü her yere `Mandrill` sınıfını enjekte edecek. Eğer `SwiftMailer`'i seçersek, `SwiftMailer` sınıfını enjekte edecek.

Dependency Injection konteynerlerinin, genel olarak aşağıdaki gibi bir kullanımları bulunmaktadır:

```php
<?php

    $container = new DependencyInjectionContainer;

    $container->bind('MailerInterface', function() {
        return new MandrillMailer; // Artık burayı değiştirmemiz yeterli olacak.
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

Bazı konteyner sınıflar, `Singleton` kullanımı gibi diğer kullanımları da destekler (`Singleton` kullanılan sınıflar sadece tek bir instance'a sahip olabilirler, başka bir instance'ları oluşturulamaz). Dependency Injection Konteynerlerinin avantajları bunlarla sınırlı olmamakla beraber, detaylı bilgiye sahip olmak isteyen arkadaşlar daha fazla araştırma yapabilirler.

**d. Inversion of Control**

// Yakında

**e. Dependency Inversion Principle**

Yazımız boyunca birçok defa bahsettiğimiz `SOLID İlkeleri`'nin sonuncusudur.

// Yakında

### - mysql_real_escape_string() sizi SQL Injection'dan korumaz. ###

Birçok PHP geliştirici, gelen inputu `mysql_real_escape_string()` ile süzerek `SQL Injection` saldırılarından korunduğunu sanmaktadır. Bu klasik bir yanlıştır ve sizi ancak temel düzeydeki `SQL Injection` saldırılarından koruyabilir. Üst düzey ve komplex bir enjeksiyon yapıldığında bu fonksiyon hiçbir işe yaramaz ve sizi koruyamaz.

`SQL Injection`'dan korunmak için, veritabanı `driver`larının `prepared statements` özelliği kullanılmalıdır. `Prepared statements` özelliği `Mysqli` ve `PDO`'da bulunabilir. `Prepared statements`, escaping işlemini sizin yerinize yapar, bu yüzden kullanımı son derece kolaydır.

```php
<?php

    $sth = $dbh->prepare('SELECT isim, renk, kalori_degeri
    FROM meyveler
    WHERE kalori_degeri < ? AND renk = ?');

    $sth->execute(array($_POST['kalori_degeri'], 'Kırmızı'));
```

Artık herhangi bir süzmeye gerek kalmadan, `$_POST['kalori_degeri']` değerini sorgu içerisinde kullanabilmekteyiz. Ancak dikkat ettiyseniz sorguda `?` kullandık ve `POST` değerini daha sonra sırasıyla `?` olan yerlere bind ettik.

Artık `PDO driver`'ı, sorguyu oluştururken `?` gördüğü bölümleri bizim verdiğimiz parametrelerle değiştirecek ve `SQL Injection` saldırılarının tamamını bizim yerimize önleyecektir.

### - Database Abstraction Layers (DBAL) ve Object Relational Mapping (ORM) ###

Bir önceki örnekte `SQL Injection`'un nasıl önleneceğini öğrenmiştik. Bilmeniz gereken bir konu da `PDO` ve `Mysqli`'nin üzerine `abstraction` (Soyutlama) katmanları çekebileceğinizdir. `Abstraction` (Soyutlama), Nesne Yönelimli Programlama'nın temel 4 ilkesinden biridir (hatırlarsanız daha önce `Kapsülleme` ilkesini anlatmıştık).

Örneğin, bir veritabanı sınıfı yazalım:

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

Şu ana kadar herhangi bir veritabanı sınıfı kullandıysanız, bu sınıflar `MySQL` üzerine çekilmiş birer soyutlama katmanıydı.

Bu konuyu öğrendiğimize göre, artık `DBAL` ve `ORM` konularına girebiliriz.

Veritabanı driverları üzerine çekilen soyutlama katmanları, `Database Abstraction Layers` (DBAL), yani Veritabanı Soyutlama Katmanları adıyla anılmaktadır. Yukarıda vermiş olduğumuz örnek bir `DBAL` örneğidir. Popüler bir `DBAL` örneği olarak `Doctrine DBAL` gösterilebilir.

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
Laravel frameworkü, otomatik olarak veritabanındaki `users` tablosunu referans ettiğimizi anlayacaktır, çünkü `user` (kullanıcı) kelimesinin çoğul şekli `users` (kullanıcılar) olacaktır. Siz de veritabanı oluştururken tablo isimlerinizi çoğul oluşturabilirsiniz (örneğin kullanıcılar, kategoriler, yorumlar, vb.). Tablo isimlerini çoğul olarak kullanmak `good practice` (iyi kullanım) sayılmaktadır. Ancak, biz Eloquent örneğimize geri dönelim.

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

Kullanıcı şifrelerini `md5()` gibi zayıf ve asıl amacı şifreleme olmayan bir algoritma ile şifrelediğinizde, bu şifreler çok kolay şekilde kırılabilir. Orta seviye bir bilgisayar bile birkaç dakika içerisinde `md5()` şifrelerini kırabilmektedir.

`md5`'in zayıf olduğu bir konu da, birçok stringe aynı md5 karşılığını verebilmesidir. Genellikle md5 şifreleri kıranlar, bu şifreleri kırmaktan çok, aynı md5 çıktısını veren başka bir şifreyi bulmaya çalışırlar.

Siz kullanıcı girişi gibi bölümlerde md5 şifreyi karşılaştırdığınız için, saldıran kişiler farklı bir şifreyi kullansa bile, md5 çıktısı aynı olacağı için sisteme giriş yapmış olurlar.

Bu yüzden, kullanıcı şifrelerini kullanırken mutlaka;

1. Sağlam bir şifreleme algoritması kullanın.
2. Salt veri oluşturun ve bunu şifrelemede kullanın.

`Salt` veri nedir? `Salt` veri, `Kriptoloji`'de şifrenin çözülmesini zorlaştırmak için şifreye rastgele veri eklenmesidir. Salt verileri şifrenin kırılmasını zorlaştırır.

`PHP`'de şifreleme için `bcrypt` algoritmasını kullanabilirsiniz. `bcrypt` algoritması, şifrenin güvenli olması için:

1. Salt veri kullanımını mecbur kılar.
2. Şifreyi birçok defa şifrelemeden geçirir ve kırılmasını çok zorlaştırır.
3. Tek taraflı şifreleme algoritmasıdır.

`bcrypt` ile kriptolanmış şifrelerin çözülebilmesi için, mutlaka kriptolanmış şifrenin, bcrypt tarafından kaç defa kriptolandığının ve salt verinin ne olduğunun bilinmesi gerekir. Böylece şifrenin kırılması neredeyse imkansız hale gelir. Bu şifrenin kırılabilmesi için son derece güçlü bir bilgisayar ordusunun çok uzun süre çalışması gerekmektedir.

`bcrypt` ve diğer şifreleme algoritmaları, PHP'ye `5.5-dev` versiyonu ile eklenmiştir. PHP'nin eski çekirdek geliştiricilerinden biri olan (ne yazık ki ayrıldı) Anthony Ferrara, oluşturduğu `password_compat` kütüphanesi ile bu özelliği `PHP 5.3.7`'ye kadar indirmiştir. İsterseniz bu kütüphaneye [https://github.com/ircmaxell/password_compat](https://github.com/ircmaxell/password_compat) adresinden ulaşabilir ve projelerinizde kullanabilirsiniz.

### - Kullanıcıya güvenmeyin. Aşırı paranoyak olmayın. Input filtrelenir, output escape edilir. ###

Sosyal platformlarda gördüğüm en büyük yanlışlardan biri bu konu. `XSS` veya `SQL Injection` koruması sağlamak için kullanıcıdan gelen veriler `strip_tags`, `htmlspecialchars` ve `htmlentities` gibi birçok fonksiyondan geçirilip veritabanına ekleniyor. Bu son derece yanlıştır ve size yarardan çok zarar verecektir.

Öncelikle, `Input` ve `Output` nedir bunu öğrenelim. `Input` (Girdi), `PHP`'ye gelen verilerin tamamıdır. Örneğin, diskten bir veri okumak, kullanıcının formdan veri yollaması, veritabanının bize veri döndürmesi gibi konuların hepsi `Input` sayılmaktadır. Bazı geliştiriciler bunu sadece kullanıcıların formdan gönderdiği veri ile karıştırmaktadır. `Output` (Çıktı) ise, `PHP`'den çıkan verilerdir. Örneğin, `echo` ile ekrana veri bastırmak, `SQL` sorguları, 3. parti uygulamalara istek göndermek, shell üzerinde komut çalıştırmak vb. `Output` olarak tanımlanır.

Şimdi, neden `Input` verilerinin `htmlspecialchars`, `htmlentities`, `strip_tags` gibi fonksiyonlardan geçirilmelerinin gereksiz olduğundan bahsedelim.

Öncelikle, kullanıcıdan gelen verinin bozulmadan veritabanında saklanması önemlidir. `XSS` ve benzeri saldırılar veritabanında bir zarara yol açmayacağı için veritabanına girmesinin, veya veritabanında tutulmasının bir sakıncası yoktur. Ancak, bu veriler ekrana basılırken mutlaka escape edilmesi gerekmektedir.

**a. Verilerin bozulmadan saklanmasını sağlanmalıdır.**

Kullanıcının gönderdiği veri ham haliyle veritabanında saklanmalıdır, bu yüzden orjinal içeriğe daima ulaşma şansınız olur.

**b. Potansiyel uzunluk hatalarının önüne geçmiş olursunuz.**

Bir ` ` karakteri süzgeç fonksiyonlardan geçtiğinde `&nbsp;` veya  `&#160;` haline dönüşüebilmektedir. Bunlar `6 harf`ten oluşmaktadır ve daha öncesinde uzunluk doğrulaması yapmış olsanız bile, veritabanına girecekleri zaman hücrenin maksimum uzunluğu aşabilirler. Sonuç olarak bu veri ya hücreye eksik şekilde girecektir, ya da veritabanı hata verip sorguyu kesecektir (bu tür hatalara genellikle `overflow` hataları denmektedir).

**c. Zararlı kod bir şekilde veritabanına sızmışsa, çıktı esnasında bu temizlenir.**

Veritabanına tek erişibim yapabilen uygulamanız değildir. Veritabanına direkt olarak bağlanıp zararlı bir kod yazsanız bile, ekrana bastırma esnasında escape işlemi uygulayacağınız için bu sorun olmaktan çıkar.

Ancak, bu bilgiler `SQL Injection` veya diğer saldırılar için geçerli değildir. Bu yüzden her saldırının escape işlemi farklı olmaktadır.

Örneğin, bir `SQL Injection` saldırısını önlemek için uygulamamız gereken escape işlemi farklıdır. `Shell Injection`'ın önlemi farklıdır, `XSS`'in önlemi farklıdır. Bunların hepsi farklı şekilde escape edilmelidir.

Bu yüzden, altın kuralımız şudur:

**Input (Girdi) esnasında veri filtrelenmelidir.**

Filtremele, genellikle `Validation` (Doğrulama) adıyla da tanınır. Verilerin doğruluğu bu esnada kontrol edilmelidir. Örneğin, veritabanına bir T.C. kimlik numarası eklenecekse, onun gerçekten bir T.C. kimlik numarası olduğu doğrulandıktan sonra veritabanına eklenmelidir.

**Output (Çıktı) escape edilmelidir.**

Bunun her saldırı için farklı olacağını söylemiştik, ancak bu bölümde bu konuyu biraz açalım.

`SQL Injection` saldırısını ele alalım. Bu saldırıyı escape etmek için veriyi `mysql_real_escape_string` (daha önce kötü olduğundan bahsetmiştik, ancak çok kullanıldığı için mantığını anlamanız için örnekte kullanıyorum) gibi fonksiyonlardan geçirebilirsiniz ya da veritabanı driverlarının `prepared statements` özelliğini kullanıp bu işin sizin yerinize yapılmasını sağlarsınız.

`Shell Injection` saldırısını önlemek için, veriyi komut içerisine geçirmeden önce `escapeshellcmd` gibi fonksiyonları kullanarak escape edersiniz.

`XSS` saldırısını önleyecekseniz, veriyi `strip_tags`, `htmlspecialchars` gibi fonksiyonlardan geçirirsiniz. Ancak bu işlem veri, veritabanına girerken yapılmaz, çıktı verirken yapılır. Sebebini yukarıda açıklamıştık.

Bu yüzden, makalelerde bolca gördüğünüz şunun gibi örnekler son derece yanlıştır.

```php
<?php

    $username = mysql_real_escape_string(trim(strip_tags(htmlspecialchars($_POST['username'])));

```

Bunun adı, bana göre `paranoyak`lıktır, ve evin arka kapısı ağzına kadar açık kalmışken, ön kapının tankla, tüfekle, bir yığın askerle korunmasına benzer.

Şimdi gerçek bir dünya örneği ile bu öğrendiğimiz bilgiyi test edelim. Örneğimiz basit olsun, bir kullanıcı sitemize kayıt olmak istiyor, bu yüzden HTML formunu doldurdu ve gönder butonuna tıkladı. Ne kadar sıradışı bir fikir, değil mi?

1. İstek geldi. Doğrulamamızı yapalım. Kullanıcı adı alfa karakterleri mi barındırıyor? Daha önce alınmış mı? Şifre 3-12 karakter arasında mı? Yeni şifre özel karakterleri barındırıyor mu? Seçilen 3-12 karakter arasında mı? T.C. kimlik no doğru mu? E-mail adresi düzgün yazılmış mı?
2. Eğer her şeye cevabımız evet ise, `prepared statements` kullanarak veritabanını güncelleyelim. Daha önce ne demiştik? `prepared statements`, escape işlemini bizim yerimize yapıyor. Ama kullanıcıdan gelen verileri veritabanına eklemeseydik ve örneğin shell sorgusunda kullansaydık, `escapeshellcmd` kullanarak escape edecektik
3. Kullanıcının verisini veritabanına ekledik. Şimdi, başarı sayfasında bu verilerin bazılarını çekeceğiz ve kullanıcıya "Aristona, üyeliğiniz başarıyla alındı." gibi bir cevap yazdıracağız. Burada, `XSS` saldırısı yapılabileceği için, veriyi ekrana bastırırken escape edip bastıracağız. Örneğin;

```php
<?php

     // Kullanıcının doğrulama ve kayıt işlemleri

     $username = "Aristona"; // Kullanıcı adı "Aristona" olsun.

     echo 'Başarıyla kayıt oldun, ' . $username; // Yanlış
     echo 'Başarıyla kayıt oldun, ' . strip_tags($username); // Doğru
```

Bu kurallara uyduğunuz zaman;

1. Paranoyaklığı bırakırsınız.
2. Veritabanınızı çöplüğe döndürmemiş olursunuz.
3. Veritabanınızda daima orjinal içeriği tutmuş olursunuz.
4. Güvenli ve kullanışlı bir uygulamanız olur.

Yazılımda paranoyak olmak iyi bir şeydir; ama azlığı veya fazlalığı hem size, hem de uygulamanıza zarar verir. Siz daima yapmanız gerekeni yapmalısınız. Gece yatarken nasıl pencerelerin kapalı olduğunu kontrol edip yatıyorsanız, uygulama geliştirirken de böyle olun. Ne penceresiz bir evde yaşayın ne de penceleri açık bırakıp uyuyun.

### - Kullanıcı giriş işlemleri için alfanumerik verileri kontrol etmeye çalışın. ###

Kullanıcının giriş işlemleri için asla isim/soyisim gibi bilgileri kontrol etmeyin. Kontrol edeceğiniz bilgiler daima alfanümerik harflerden oluşmalı. Aksi takdirde veritabanının karakter seti, kullanılan doğrulama fonksiyonları çok büyük hatalara yol açabilir.

Örneğin, bazı veritabanı karakter setleri, alfanümerik olmayan karakterlere izin vermezler. Bu durumda Türkçe karakterlerden olan Ç, Ş, İ, Ğ, Ü gibi karakterler görünmezler, veya `?` halinde çıkabilirler. Bazı karakter setleri büyük küçük harf duyarlı, bazıları duyarsızdır. Veritabanı karakter setlerine veritabanı bölümünde değineceğiz, şimdilik bunu bilmeniz yeter.

Şimdi tekrar konumuza dönelim. Neden isim/soyisim gibi bilgiler kontrol edilmemeli? Çünkü birçoğumuzun isminde alfanümerik olmayan harfler var. Mesela benim adım `Anıl ÜNAL`, ve küçük `ı` harfi ile `ü` harfi alfanümerik değil. Benim durumumda olan milyonlarca Türk kullanıcı var. Bizden hariç japonlar, çinliler, koreliler, araplar vb. insanlar ve herkesin kullandığı karakter seti farklı olabiliyor. Ancak, birçoğumuz alfanümerik e-mail adreslerini kullanıyoruz, veya alfanümerik bir kullanıcı adı almaya zorlanıyoruz. Bu daima iyi bir şeydir. Alfanümerik karakterleri ne kadar çok kontrol ederseniz (özellikle giriş işlemlerinde) o kadar avantajlı olursunuz.

Bunun neden önemli olduğunu daha önce tecrübe etmiş olduğum bir örnekle açıklayayım: Son derece büyük bir MMORPG oyununda, başka insanların hesaplarını çalmak için kullanıcı adlarını bilmek yeterli oluyordu. Ama nasıl?

Bir kullanıcının kullanıcı adı `anıl123456` olsun. Bir başka kullanıcı da gidip `anil123456` kullanıcı adını alsın. Uygulama daima alfanümerik karakterlerin olacağını düşünüyordu, ama veritabanı hücresi alfanümerik olmayan karakterleri kabul ediyordu. `latin1_swedish_ci` gibi veritabanı collation'ları, `anıl123456` ile `anil123456` aynı sayıyordu. Binlerce insanın hesaplarının çalınmasına sebep olan hata buydu. Hesaplarında Türkçe karakter geçen kullanıcıların hesap isimleri, Türkçe karakterler değiştirilerek tekrar alınıyordu (Anıl ise, Anil; Şener ise Sener gibi). Bu yapıldığı zaman eski hesaplar giriş yapamıyordu ve yeni hesaplar giriş yaptığında eski hesabın içeriğine ulaşabiliyordu.

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

### - Uygulamanızda mümkün olduğunca Türkçe kullanmamaya çalışın. ###

İngilizce bilmek ve İngilizce kullanmak yazılımcıların hayatını kolaylaştıran en önemli faktörlerdendir. 

İngilizce yazılım dünyasında hemen hemen her yerde karşınıza çıkacaktır. Örneğin, kullandığımız programlama dillerindeki methodlar İngilizce'dir. Takip edebileceğiniz ünlü yazılımcılar hep İngilizce konuşmaktadır. Takip edebileceğiniz bloglar ve websiteleri İngilizce'dir. GitHub üzerindeki açık kaynaklı projelerin dökümantasyonu İngilizce olarak yazılmıştır. Projelerin kaynak kodlarında kullanılan değişken isimleri, sınıflar, yorum satırları vb. hep İngilizce'dir.

Özellikle GitHub ve açık kaynaklı projeler içerisinde İngilizce kullanmayan, değişkenlerin ve sınıfların yazılımcının kendi anadilinde tanımlandığı projeler, ne kadar iyi olurlarsa olsunlar başkaları tarafından genellikle kullanılmaz, destek görmez ve yeterince popüler olamazlar. Ayrıca bu durum sizin `amatör` olduğunuz algısı yaratır.

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

Bu sınıf son derece çirkin ve amatörce duruyor. Türkçe desen tam olarak Türkçe değil, İngilizce Türkçe karışımı, ne olduğu belirsiz bir şey. Zira PHP unicode desteklemediği için Türkçe karakterleri de kullanamıyoruz, bu yüzden ortaya Türkçe karakterlerin kullanılmadığı bir garip Türkçe çıkıyor.

Sorun sadece bununla kalmıyor. İleride bir sorun çıktığını farzedelim ve siz saatlerce uğraşmanıza rağmen bunu çözemiyorsunuz. Bu durumda, eğer İngilizce kullandıysanız `StackOverflow` gibi platformlarda, kullandığınız dilin `irc kanalları` veya forumlarında hatalı scripti kopyala/yapıştır yaparak sorununuzu kolayca dile getirebilirsiniz. Türkçe kullanırsanız kod parçacıklarını koyarken bunları İngilizce'ye çevirmeniz gerekecek aksi takdirde büyük çoğunluğu İngilizce konuşan insanlar sizin ne yapmak istediğinizi anlayamayacaktır.

Buna ek olarak, Türkçe karakterleri kullanmak son derece sakıncalıdır. Bunun sebeplerinden bazılarını daha önceki bölümlerde anlatmıştık, ancak `SSH` üzerinden sunucuya bağlanıp Türkçe karakter yüzünden dosya açılmadığı veya komutları giremediğiniz zaman zaten kendinize kızacaksınız. Bu yazıyı yazarken bile Türkçe karakterler yüzünden birçok sıkıntı çektim ve neredeyse yarım günümü bu hataları çözmek için harcadım.

Birinin size Ankara-İstanbul arası kaç `mil` diye sormasını ister misiniz? Veya trafiğin sağdan aktığı ülkelerde ben soldan gideceğim diye inat eder misiniz? Her iyi yazılımcı gibi kendinizi İngilizce kullanmaya alıştırın. Bilmiyorsanız da öğrenmeye çalışın; çünkü İngilizce öğrenmek bile sizi İngilizce bilmeyen yazılımcıların birkaç ışık yılı ötesine taşıyacaktır.

> Not: Tartıştığım yazılımcıların karşı argümanı bazen İngilizce bilmeyen yazılımcılarla çalıştıkları, bu yüzden Türkçe kullanmayı tercih ettikleriydi. Ben şahsen İngilizce bilmeyen yazılımcılara pek güvenemesem de, bu durumda projenin geleceğini düşünmek katı kurallara uymaktan daha önemli olabilir.

### - Kimin yazdığını bilmediğiniz bloglardan ve eğitim setlerinden uzak durun. ###

Kötü eğitim, yarar sağlamaktan çok zarar verir. Özellikle Google aramalarında bazen üstlerde çıkan Türkçe bloglar ve bu bloglardaki makaleler ve eğitim setleri çoğu zaman eksik, demode ve yanlış bilgiler içermektedir. Bu tür bloglardaki içeriklerin çok büyük bir kısmı kaynak belirtilmemiş çeviri, kalanların da birçoğu 2-3 aylık tecrübesiz yazılımcıların `ilk heyecanlarıyla` bloglarına yazdıkları eksik ve yanlış makelelerden oluşmaktadır. İstisnaları ayrı tutuyorum; ancak ayrı tutacak istisnalar yok denecek kadar az malesef.

Ben genellikle birinin blog sitesine girdiğim zaman, yazdıkları makelelerin başlıklara bir göz gezdiririm. Bilmiyorum siz de böyle misiniz? Yazdıkları makelelerin kalitesi, bana blogun kalitesi hakkında ipucu verir; ancak bazı bloglar var ki gerçekten bir çöp yığınından fazlası değil. Örneğin, bloglarına girip yazdıkları makaleleri okuyunca önce şaşırırsınız. Adam scalability'den girmiş Nginx konfigürasyonlarına kadar, PHP 6 ile gelecek özelliklerden bahsetmiş, yazılım mimarileri falan havada uçuşuyor. Sanırsınız Google'da baş mühendis. Sonra bir kaç blog yazısı daha yazmış: "PHP'de echo kullanarak ekrana yazı bastırmak", "mysql_query() ile veritabanından veri çekmek".

Sihirli kürem yok, bu yüzden gerçekte neler olup bittiğini kanıtlayamıyorum; ancak bu tür bloglardaki makaleler bana bir yerden alınmış ve çevirilmeye çalışılmış gibi geliyor. Kaynak belirterek çeviri yapmalarında bir sorun yok; ama çeviri yaparken birçok konsept ve terim çoğu zaman yanlış çevriliyor. Böylece insanlar yanlış bilgilendiriliyor. Öğrendiğiniz şeylerin yanlış veya yalan olduğunun bir gün farkına varsanız tepikiniz ne olurdu?

Bu durum, özellikle `PHP blogları` için, artık son derece vahim bir hale geldi. WordPress ile websitesi kurup kendine web geliştirici diyenler, Dreamweaver'da form oluşturup kendini web tasarımcı sananlar, `PHP`'nin temelini bile almadan bu işi ticarete dökmek için eğitim seti hazırlayanlar... Bu işin ucu gerçekten kaçtı.

PHP bloglarının kötü olmasının bana göre birkaç sebebi var:

1. PHP ile bir şeyler geliştirebilmenin diğer dillere göre daha kolay olması, bu yüzden amatör yazılımcılar tarafından sıkça tercih ediliyor olması (muhtemelen en büyük amatör/yeni yazılımcı kitlesi bizde).
2. `WordPress` ve `Joomla` gibi son derece eski ve ilkel yöntemlerle geliştirilen projelerin son derece popüler olması.
3. Birkaç yıl öncesine kadar GitHub (2008) ve Composer (2012)'ın olmayışı. Bu yüzden, binlerce kötü, eski, güvenli olmayan, demode sınıf ve kod örnekleri internette dolaşıyor ve halen Google arama sonuçlarında çıkıyor olması.
4. PHP'nin 5.3 versiyonuna kadar modernlikten çok uzak bir dil olması.
5. Kurumsallıktan uzak firmaların kullanıyor olması. İşimizi nasıl modernleştirebiliriz yerine nasıl asgari ücrete çalışacak PHP'ci buluruz arayışı.
6. Sosyal Ağ filmini izleyip etkilenmiş, geleceğin `Mark Zuckerberg`'i olmak isteyen, 2 aylık bilgisiyle kendini `Linus Torvalds` sanan kitle.

Mesela bir `Scala` veya `Haskell` blog yazısında makelenin yanlış olma ihtimali son derece düşükken, `PHP` dünyasında bu ihtimal son derece yüksek. `Basic` dünyasında bile bu kadar yanlış ve hatalı bilgi olacağını sanmıyorum.

Bunların dışında, bir de özellikle Türkçe bloglarda göze çarpan genel eksikliklerden bahsetmek istiyorum.

1. Öncelikle bloglar açık kaynaklı değiller. Neredeyse hepsi `WordPress` üzerine kurulmuş, bu yüzden başkaları düzeltmede bulunamıyor. Buna çok bilinen `w3schools.com` dahil - Adamlar verdikleri örnekteki SQL Injection açığını tam 6 yıl sonra düzelttiler ve bunlar sertifika veren bir eğitim kurumu.
2. Yanlış bir bilgi olduğunu söylediğin zaman yorumların siliniyor. Çok az kişi eleştiriyi kabullenebiliyor.
3. Çevirilerde terimler genellikle yanlış çeviriliyor, bu yüzden son derece alakasız sonuçlar çıkabiliyor (İngilizce öğrenin dememin sebebi bu).
4. Üst düzey PHP diye yazdıkları makaleler aslında `PHP`'nin temel bilgileri. Ben bunun bir marketing stratejisi olduğunu düşünüyorum, ama emin olun o bloglarda yazanlar hiç birşey değil.

Gecekondu dikip üst düzey inşaat mühendisliği, geleceğin mimarisi diye anlatan, böbürlenen başka bir topluluk var mı acaba dünyada? PHP blogları hep böyle.

Bir `ASP.NET`'ci veya `C#`'cı, bankalarda veya kurumsal bir firmada çalışırken baş mühendis tarafından ağzının payını alır. Tecrübe kazanana kadar biraz sessiz kalayım der.

Bir `Ruby`'ci, starbucksta elinde kahve kod yazarken bu tür işlere zaten bulaşmaz. Yolunu bulmuştur.

Bir `Haskell`'ci veya `Lisp`'çi zaten blog yazmaz; çünkü ne yazdıklarını kimse anlamaz.

Bir `Java`'cı, en azından bu işlere okulda aldığı temel mühendislik eğitimiyle başlar. Yazılım mimarilerini, tasarım desenlerini falan kullanamasa da bilir. Arada tek tük söyler.

Bir `C`'ci veya `C++`'cının zaten iyi veya kötü derdi olmaz. İyisi ve kötüsü arasındaki fark çoğu zaman ayırt edilemez.

Bir `Assembly`'cinin zaten blog yazacak bir interneti olmaz.

Bir `NodeJS`'ci, genellikle daha önceden `JavaScript`'i tecrübe ettiği için biraz bilgilidir. Yeni başlayan "Soket, asenkron falan bişey diyorlar anlamadım ben." der bırakır.

Ama... `PHP` dünyası böyle mi? Bir ev yaparlar, evin pencereleri olmaz, çatısı aşağıda olur, kapıyı açtığında bütün bina çöker ve kapıyı açtığınızda binayı yıktınız diye size kızarlar. Adam bir köpek kulübesi yapar, öyle bir havalara girer ki sanırsın Burj Dubai'yi yapmış (ama o köpek kulübesini almak isteyen birçok insan çıkar, böyle de bir avantajı var).

`PHP`'nin böyle garip bir komünitesi var. Bu yüzden konuyu çok dağıtmadan toparlayalım: Kısacası böyle bir durumda olduğumuz için, kendinizi eğitirken yanlış bilgi alıp kafanızı karıştırmayın. Doğru bilgiyi doğru insanlardan, doğru makalelerden, doğru bloglardan ve doğru kitaplardan alın. Bir eğitim setindeki videoları izliyorsanız, o eğitim setini kim yazmış, ne zaman yazmış, PHP'nin hangi sürümü kullanılmış, yorumları nasıl gibi konuları araştırın, yoksa bunlar size hiçbir şey kazandırmaz. YouTube'dan 10 yıl öncesinin videolarını izleyip PHP öğrenmeye çalışmayın. "Bu dosyayı al, her projenin başında include et. Ne SQL Injection kalır ne bir şey, güven sen bana." diyen insanlara gözü kapalı güvenmeyin. Bu konuda son derece dikkatli olmalısınız.

> Not: Bu yazdıklarım genellikle Türkçe bloglar için geçerli. İngilizce bloglar nispeten daha iyi durumda; ama onların da kötüsü çok fazla.

### - ...ya performanslı olmazsa? ...ya çok include uygulamayı yavaşlatırsa? ###

`OOP` (Nesne Yönelimli Programlama) kullanmak istemeyenlerin, framework'lere "Çok hantal çalışıyor." diyenlerin, modern tekniklerin uygulamayı yavaşlatacağını düşünenlerin klasik problemi: `Ya yavaşlarsa?`

Kısa cevap: Hiçbir şey olmaz.

Uzun cevap: Yakında yazarım. Bootle-neck'ler, opcode caching nedir, scalability nedir, mikrooptimizasyonlar niye günü kurtarır, HHVM nedir vs.

### - Kaptan gemiyi terk etmişse, o gemide kalmanın fazla bir anlamı yok. ###

Son birkaç yılda açık kaynak adına son derece büyük adımlar atıldı. Artık neredeyse her türlü açık kaynaklı proje `GitHub` üzerinden yayınlanmakta. Ancak bu son derece avantajlı olmasına rağmen, bazen dezavantajları da olabiliyor.

Dezavantajlarından biri, `PHP`'nin çok fazla açık kaynaklı projeye sahip olması. Bir `Mail` sınıfı arıyorsunuz ve karşınıza binlerce `GitHub repository`si çıkıyor.

Bir diğer dezavantajı da, insanların haklı olarak en çok gelecek vaadeden projelere yönelmesi. Bazen bir projeyi geliştiren yazılımcı, tekerleği tekrar icat etmektense, farklı bir yazılımcının projesinin daha iyi konumda olduğunu düşünebilir ve o projeye destek olmaya başlayabilir. Kendi projesini ise geliştirmeyi bırakabilir veya başka birinin devralmasını isteyebilir (devredilen projelerden çoğu zaman hayır çıkmaz, not edelim).

Buna örnek olarak, başka bir framework olan `CodeIgniter` projesi gösterilebilir. Bir zamanlar iyiydi, herkes kullandı, ancak bu projeyi geliştiren çekirdek yazılımcıların birçoğu farklı projelere geçtiler ve `CodeIgniter` projesininden sorumlu olan `EllisLab` firması projeyi devralacak birini aramaya başladı (bu yazıyı yazdığım esnada birkaç ay geçmiş olmasına rağmen kimse projeyi devralmak istemedi).

Kısacası, geminin kaptanı atladıysa, mürettebatı atladıysa, filikalar indirildiyse ve yolcuların birçoğu tahliye edilmeye başlandıysa, o gemide kalmanın mantıklı olduğunu düşünmemelisiniz. Siz de çok geç olmadan atlamalısınız.

Bu durum sadece `CodeIgniter` projesiyle ilgili değil. Her türlü proje ileride bu tür sorunlarla karşılaşılabilir. Bu yüzden, projenizde o günün şartlarındaki en popüler ve en gelecek vaadeden framework'leri, komponentleri, sınıfları ve kütüphaneleri kullanmaya çalışın.

Terk etmeniz gereken projeler varsa, vakit kaybetmeden terk edin.

### - DRY kuralına uyun ve akıllı çalışın. ###

`DRY (Don't Repeat Yourself)`, Türkçe'siyle `Kendinizi Tekrar Etmeyin` kuralını hem PHP'in temelinde, hem de gerçekten üst düzey konularda kullanabilirsiniz (aslında bu bir kural değil, bir tavsiyedir).

Temel bilgiye sahip bir yazılımcı, aynı şeyleri tekrar etmekten bıkıp o işlem için fonksiyon oluşturuyorsa `DRY` için bir adım atmış olur. Üst düzey bir yazılımcı her projesinde kullanabileceği bir komponent yazmış ve bunu package managerlar tarafından yönetiyorsa `DRY` için bir adım atmış olur.

// Salt PHP ve L4 konusunda DRY örnekleri gelecek buraya.

`DRY`ın sonu yoktur ve sadece programlama dilleriyle ilgili değildir. Proje geliştirirken sık sık yaptığınız işlemleri bilgisayara yaptırmakta bu kural için atılmış adımlar olacaktır. Örneğin, veritabanı yedeği mi alınacak? Veritabanı yönetici paneline gir, veritabanını seç, tabloları seç, exporta tıkla, yol olarak bir path belirle, çıktıyı oluştur, o klasöre gir, çıktıyı zip içerisine koy, sonra ismini "x dbsi yedeği" yap... Bu tür işlemlerle kaybettiğiniz zamanı hesaplayın ve o kaybolan zaman içerisinde kaç tane proje geliştirebileceğinizi düşünün.

Bunun yerine:

    grunt backup:veritabani_adi

yazdığınızda bu işlemlerin sizin yerinize yapılması daha hoş olmaz mı?

Veya:

    cap production deploy

yazdığınızda sunucuya veritabanını yedeğinin yüklenmesini sağlayıp, Apache/MySQL (artık ne kullanıyorsanız) restart atılmasını istemez misiniz?

Proje geliştirirken en çok nelere vakit harcadığınızı düşünün ve bilgisayarın yapabileceği her şeyi bilgisayara yaptırın.

Her seferinde `public function` yazmak zor mu geliyor? Kullandığınız IDE içerisinde `Snippet` oluşturun. `pub` yazınca otomatik tamamlasın. Daha iyi ihtimalle, zaten birileri oluşturmuş ve GitHub'da paylaşmıştır, araştırın.

Uzun terminal komutlarını zor mu geliyor? `Alias` oluşturun, hatta gerekiyorsa bunları script haline getirin.

Özellikle genç arkadaşlar bunun ne kadar önemli olduğunun farkında olmalılar. Günde 8 saat çalışarak 10 birim iş yapacağına 30 birim iş yapabilirsin. Uzun vadede ne kadar çok zaman ve (dolayısıyla para, çünkü zaman === para) kazanabileceğinin farkına varabilirsin.

Bu yüzden, daha `akıllı çalış`. Daima daha fazlasını öğren. Vakit kaybetme, iyi olmak kadar hızlı olmakta önemli.

> Not: Geliştirici ortamında yapabileceğiniz iyileştirmelerden `Geliştirici Ortamı` bölümünde bahsedeceğim ve nasıl hızlanabileceğinizi açıklayacağım.

### - Daima tutarlı olun. ###

Indentation için tab kullanıyorsanız, tab kullanarak devam edin. Methodları `_` kullanarak ayırıyorsanız, `_` kullanarak devam edin. CSS'lerinizi yazarken ayraç olarak - kullanıyorsanız, her yerde - kullanın. Yaptığınız her şey tutarlı olsun.

Tutarsızlığın en güzel örneği `PHP` çekirdeği. `strpos` ve `str_replace` fonksiyonlarını ele alalım. Niye `str_position` değil de `strpos`? Veya niye tam tersi değil? Niye bazı fonksiyonlar önce diziyi, sonra stringi alırken diğerleri önce stringi, sonra diziyi alıyor? Bu tutarsızlıkların hepsini hatırlamak zorunda mıyız? Bu tutarsızlıklar neden var?

Mesela neredeyse tüm programlama dillerinde `reverse()` verilen stringi tersine çevirir. PHP'de bu ne tahmin edin bakalım?

`str_reverse? streverse? strrev? revstr? reverse?`

Herhangi biri olabilir; ama PHP için doğrusu `strrev`.

`PHP`'nin çekirdek geliştiricileri de bunun farkında; ama `backwards compatibility` (Geriye Dönük Uyumluluk) bozulmasın diye şimdilik böyle devam ediyorlar.

Siz kendi projelerinizde bunu asla yapmayın. `TAB` kullanmayı bırakıp `4 boşluk` kullanmaya karar verdiyseniz, ya tüm uygulamayı buna uyarlayın, ya da eski şekil devam edin! Uygulamanın yarısı `TAB`, kalan yarısı `4 boşluk` olmasın. Yeni öğrendiğim her şey için projeyi düzeltmeye kalksaydım, tek satır kod yazmadan optimize etmeye çalışıyor olurdum.

Evini sarı renge boyarken yarı yolda fikrini değiştirip evin kalanını yeşil renge boyayan birini gördünüz mü? Ben görmedim, ama projenin yarısını farklı, kalan yarısını farklı geliştiren insanları tanıyorum. Onlardan olmayın.

> Not: Tutarsızlığı yüzünden PHP sosyal platformlarda çok ağır eleştirilere maruz kalmakta -ve eleştirenler hep aynı şeyleri söylediği için artık kabak tadı vermeye başladı.

Eğer PHP'de `Ruby` ve `JavaScript` gibi dillerdeki `reverse()` methodunu kullanmak istiyorsanız, PHP'in `scalar objects` extensionunu kullanabilirsiniz ancak birçok eksikliği/limitasyonu var. 3. parti extension olduğu için kullanmanızı tavsiye etmem (çünkü shared hostinglerde çalışmaz) ama bilginiz olsun diye yazıyorum, böyle bir kullanım var.

Örneğin:

```
<?php

    class StringHandler {
        public function reverse() {
            return strrev($this);
        }
    }

    register_primitive_type_handler('string', 'StringHandler');

    $ornek = "Merhaba!";
    echo $ornek->reverse(); // Çıktı: "!abahreM"
```

İleride `PHP` çekirdeğine eklenir mi bilinmez, ama ben şahsen bu özelliğin eklenmesini çok isterdim.

### - Gerekmedikçe else ve uzun if blokları kullanmayın. Daima kıvırcık parantez kullanın. Yazdığınız kodları sağa yaklaştırmayın. ###

Bazen 7-8 satırlık `if/else` bloklarını tek satırda bile yazabilirsiniz, ve çoğu zaman gereksiz `else` bloklarını kaldırabilirsiniz. Gereksiz `else` bloklarını silmemiz kodlarımızın sağa doğru yaklaşmaması konusunda bize yardımcı olur.

Yazdığımız kodlar, bazen sağa doğru yaklaşırlar. Bu genellikle iç içe `if` blokları kullandığımız zaman ortaya çıkar. Aslında konu başlığı "İç içe if blokları açmak genellikle hatadır." olabilecekken, ben "Kodlarınızı sağa doğru yaklaştırmayın." demeyi tercih ediyorum; çünkü hem daha genel bir alanı kapsıyor, hem de daha akılda kalıcı.

Öncelikle, kodların sağa yaklaşması ne demek önce hemen bunu açıklayalım.

```php
<?php

     public function deneme()
     {
          // 1. seviye
          if(birsey)
          {
               // 2. seviye
               if(baska_birsey)
               {
                    // 3. seviye
                    if(baska_birsey_daha)
                    {
                        //...
                    }
                    else
                    {
                        //...
                    }
               }
               else
               {

               }
          }
          else
          {

          }
     }
```

Bu örnekte gördüğünüz gibi iç içe `3` tane `if bloğu` açılmış. Dikkat ettiyseniz her `if` bloğu sağ tarafa biraz daha yaklaşmış. Eğer böyle yaparsanız yazdığınız kodlar okunaklı olmaktan çıkar. Bunun içine `else` blokları da girdiğinde hangi kıvırcık parantezin hangi bloğu kapattığını veya açtığını anlamanız güç olur. Fonksiyon içerisinde genellikle `1` veya `2` seviye if bloğu oluşturmalısınız. `3` ve üzeri çok fazla iş yapıldığına ve kodun okunamaz olacağına işarettir.

İyi bir kod, bakıldığında aşağıya doğru düz bir çizgiyi andırmalı ve dalgalanmalar az olmalıdır. Burada bizim oluşturduğumuz fonksiyon düz bir çizgi yerine, yassı bir `C` harfini (tersten) andırıyor. Oluşturduğunuz sınıfta bu fonksiyon gibi 10 fonksiyon daha olduğunu hayal ederseniz, dalgalı bir çizgi görüntüsü gözünüzde canlanacaktır.

"Kodlarınızı sağa doğru yaklaştırmayın." dememin bir sebebi daha var: Kodlarınızdaki her satır, ortalama maksimum 80 ile 120 karakter uzunluğunda olmalıdır. Bu bir kural değildir ancak `good practice` (İyi Kullanım) sayılmaktadır. IDE veya iyi bir tex editörü kullanıyorsanız, siz de `wrapping` ayarını 80 veya 120 karakter uzunluğa getirebilirsiniz.

Kodun okunabilir olması için bir bilgiyi öğrendik, ancak daha geliştirilebilecek noktalar var. Örneğin, bir değişkenin array olup olmadığını anlamak istediğiniz bir fonksiyon yazalım. Aslında bu örnek direkt olarak `is_array` kullanılarak yapılabilir, ancak ben burada soruna değinmek istediğim için fonksiyon oluşturarak yapabileceklerinizi anlatmak istiyorum.

Bu fonksiyon aşağıdaki şekilde yazılabilir:

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

Şuanki hali üsttekinden çok daha güzel. Gereksiz `else` bloğunu kaldırmış olduk. Gereksiz `else` bloklarını kaldırmak bize çok büyük avantaj sağlıyor. Hem fonksiyonunuz sağ tarafa doğru uzamamış oluyor, hem de gereksiz kod yükünden kurtuluyoruz (`return`'un sadece fonksiyon içerisinde kullanılabileceğini unutmayın).

Devam edelim: `if bloğu` içerisindeki ilk satır daima `if bloğu` içerisinde sayılacağı için, kıvırcık parantezleri de silebiliriz.

```php
<?php

    function isArray($input)
    {
        if(is_array($input)
            return true;
        return false;
    }
```

Bu örnek düzgün çalışır; ama son derece `tehlikelidir.` Neden tehlikeli olduğunu birazdan `Apple`'ın meşhur `goto fail;` exploiti ile açıklayacağım, biz şimdilik devam edelim.

Biz bunu biraz daha geliştirip, `ternary` operatörünü de kullanabiliriz.

```php
<?php

    function isArray($input)
    {
        return is_array($input) ? true : false;
    }
```

Dilerseniz, `ternary` operatörünün ilk parametresi olan `true`'yu bile silebilirsiniz; ancak bu da `tehlikelidir`.

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

Bu ufacık fonksiyon bile birçok şekilde yazılabilmekte; ancak herşeyi kısa yazmak her zaman doğru değildir. Özellikle kıvırcık parantezleri kaldırmak, farkında olmadan birçok hatanın çıkmasına sebep olabilir.

Kıvırcık parantezler olmadığında, yanlışlıkla `if` bloğu sonrasına 2. bir satır eklenirse, 2. satır daima çalışacağı için scriptimiz yanlış çalışmaya başlayacaktır.

```php
<?php

    function test()
    {
        if (kosul)
          return "If";
          return "Hata"; // Burası if bloğu içinde değil
        else
          return "Else";
    }
```

Bu örnekte, `else` hiçbir zaman çalışmayacaktır. Çünkü, `kosul` şartı sağlanıyorsa, sadece `return "If";` çalışacak ve fonksiyon `If` değerini döndürecektir. Diğer her türlü durumda `return "Hata";` çalışacaktır ve fonksiyon `Hata` değerini döndürecektir. Kıvırcık parantez olmadığında 2. satır `if bloğu` içerisinde sayılmaz.

> Önemli Not: Normal şartlarda if-elseif-else bloklarının arasına başka bir statement ekleyemezsiniz. Bu durumda PHP size `unexpected 'else'` şeklinde bir syntax hatası döndürür. if olmayan bir yerde else if ya da else bloğundan söz edilemez. Arka arkada yazılan iki if bloğu için bu durum geçerli değildir; çünkü ikisi birbirinden bağımsız işlenir.

Siz uygulamamızı `else` bloğunun çalışacağı durumlarda `Else` değeri dönecek diye programladıysanız; ama bu durum gözden kaçmışsa ve fonksiyonunuz `Hata` değerini döndürüyorsa, birçok `bug` oluşmasına davetiye çıkarmışsınız demektir.

Bu durumdan kurtulmak için kıvırcık parantez kullansaydınız, bu durum sorun olmaktan kendiliğinden çıkacaktı.

```php
<?php

    function test()
    {
        if (kosul)
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

Bu örnekte, `kosul` koşulu sağlanıyorsa `if` bloğunun ilk `return` ifadesini çalışacaktır, koşul sağlanmıyorsa fonksiyon `else` bloğunu çalıştıracak ve oradaki `return` ifadesini çalıştıracaktır. `return "Hata";` ifadesi asla ve hiçbir koşulda çalışmayacaktır.

Kıvırcık parantez kullanmak bu yüzden son derece önemlidir. `Apple`'ın `SSL`'de çıkardığı meşhur `goto fail;` güvenlik açığının sebebi budur.

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

Yanlışlıkla 2 tane `goto fail;` yazıldığı için `if ((err = SSLHashSHA1.final(&hashCtx, &hashOut)) != 0)` if bloğuna hiçbir zaman zaman erişilemiyor. 2. sıradaki `goto fail;` daima çalışacağı için script `fail`'e düşüyor ancak burada hatanın dönmesi yerine `true` değeri dönüyor. Yani en alttaki `if` kontrolü yok sayılıyor ve bu kontrol yok sayıldığı için bu bölüm üzerinden bir exploit ortaya çıkmış oluyor.

Dünyanın belki de en büyük teknoloji firmasının yaptığı bu hata son derece komik olmakla beraber, sadece düzgün şekilde kıvırcık parantez kullanılsaydı kendiliğinden çözülmüş olacaktı. Demek ki, dünyanın en iyi yazılım mühendisleri bile bu tür hataları gözden kaçırabiliyor.

Toparlarsak, bu bölümde şu konulara değindik:

1. Kodlarımızı sağa doğru uzatmamalıyız.
2. Kıvırcık parantezleri doğru kullanmalıyız.
3. Gereksiz `else` ifadelerini silebiliriz.

### - Kod yaz, tarayıcıya dön, F5'e bas, hata var mı? Yok, devam et. ###

`DRY` (Kendinizi Tekrar Etmeyin) bölümünde anlattığım konuya bir örnekte bu konu. PHP geliştiricilerinin %99'u bu şekilde çalışıyor (ve bu normal) ama yanlış. Neden yanlış olduğunu `DRY` bölümünde anlatmıştım: "Bilgisayarın yapması gereken şeyleri siz yapmamalısınız."

Oluşturduğunuz form, submit'e tıklandığında form verilerini uygulamaya gönderiyor mu? Güzel; ama bunu `acceptance test` yazarak test edin, tarayıcıdan bakarak değil.

Formdan veri gönderildiğinde, gelen istek doğru yere yönlendiriliyor mu? Güzel; ama bunu `unit test` yazarak kontrol edin, tarayıcıdan bakarak değil.

Formdan gelen T.C. kimlik no verisini doğrulayan method doğru çıktıları veriyor mu? Güzel; ama bunu `fonksiyonel test` yazarak kontrol edin, tarayıcıdan bakarak değil.

Bunu yapmadığınız zaman projenin herhangi bir yerinde değişiklik yapmaya korkar olursunuz. Proje ne kadar büyürse projeyi manuel test etmek o kadar zorlaşır ve hata oranı büyük ölçüde artar. Benim gibi projeden projeye atlayan biriyseniz, hangi projenin ne durumda olduğunu aklınızda tutamazsınız.

Projeyi geliştirirken her ihtimali tarayıcı üzerinden test etmiş olabilirsiniz. Formlar doğru veriyi gönderiyor, doğrulamalar çalışıyor, özel fonksiyonlar doğru çalışıyor, veriler veritabanına ekleniyor, bilgilendirme sayfası çalışıyor. Evet, her şey çalışıyor ama emin olun o projeye 1 ay ara verdiğinde bir daha neyin çalışıp neyin çalışmadığını hatırlayamayacaksınız.

Bu yüzden kendinizi test yazmaya alıştırın. `F5`'e ne zaman basmanız gerekiyorsa parmağınızı geri çekin ve test yazın. Test işlemini kolaylaştıran `PHPUnit`, `Codeception`, `Selenium`, `Behat` gibi araçları inceleyin ve kullanın.

**Continuous Integration için ayrı bir sayfa açalım...**

Continuous Integration, en basit haliyle yazılan testlerin sıklıkla çalıştırılmasını sağlamaya denmektedir. Yazdığınız testleri saatlik olarak veya versiyon kontrol sistemlerini kullanıyorsanız her commmit sonrası çalıştırılmasını sağlayabilirsiniz.

Continuous Integration için `Travis CI`, `Hudson CI`, `Jenkins CI` gibi araçları kullanabilirsiniz. Continuous Integration araçlarını kullanmanın size faydası şudur:

Örneğin, projenize bir arkadaşınız destek oluyor. Bu arkadaşınız geliştirme esnasında uygulamanın bir bölümünü bozmuş olabilir. Bu durumda CI sunucusu sizi uyarır. "Hey, 9a8fja numaralı committen sonra artık butonaTıklandığındaFormVerileriSunucuyaGidiyorMu testi çalışmamaya başladı." Bunun sebebini araştırıp kolayca bulabilirsiniz.

Eğer test yazmazsanız ve bu araçları kullanmazsanız, arkadaşınız size şunu diyebilir. "Bugün anasayfadaki formun tasarımını biraz değiştirdim." Aslında değiştirmedi, bozdu. Siz de test yazmadığınız için burada bir hata olduğunu anca müşteriniz telefon açıp sizi azarlayınca anlayabilirsiniz.

Test konusunu çok önemli olup, son zamanlarda kendi başına bir sektör haline dönüşmeye başladı. Kalite kontrol departmanları sistemi test edecek Test Uzmanları/Mühendisleri çalıştırmaya başladılar. Siz de geliştirdiğiniz uygulamanın kaliteli ve çalışır durumda olduğunu kanıtlamak için testlerinizi yazın. Büyük bir proje geliştiriyorsanız proje geliştirmeyi `F5`'e basarak devam ettiremezsiniz.

Test yazmak bazen zaman alıcı olabilir. Çoğu zaman `var_dump()` yazıp `F5`'e basmak geliştiricilere daha kolay gelir. Bu yüzden artık geliştirilmeyeceğinden emin olduğunuz küçük uygulamaları bu şekilde test edip müşteriye verebilirsiniz. Ama o müşterinin 2 yıl sonra sizi arayıp yeni şeyler istemeyeceğinden nasıl emin olacaksınız, orası ayrı konu.

### - Testlerinizi yazarken string kullanmamaya çalışın. ###

İster `unit test` yazıyor olun, ister `acceptance test`, ne kadar az string kontrol ederseniz sizin için o kadar sağlıklı olur.

Mesela acceptance testlerinde `<p>Anasayfa</p>` varmı diye kontrol etmektense header kodunun `200 OK` olduğunu veya `homepage.view.php` çalıştırılmış mı diye kontrol edin. Anasayfa yazısını değiştirmek `zararsız` sayılacağı için kolayca değiştirilebilir. Örneğin, anasayfa yazısı yerine `Font Awesome` ile ev resmi koyabilirsiniz, ama `homepage.view.php` dosyasının adı kolay kolay değişmez. Bu dosyanın adı değişirse, controller'daki methodlarında birçoğu değişecektir.

Bu yüzden testlerinizi yazarken mümkün olduğunda string kullanmaktan kaçının ve değişmeyecek şeyleri kullanarak testlerinizi yazın.

### - HTTP Response kodlarını öğrenmeye ve kullanmaya çalışın. ###

HTTP Response kodları önemlidir. Örneğin, response kodu `404` olmayan bir `404 `sayfası, gerçek bir `404` sayfası değildir. Google gibi arama motorları bu sayfayı 404 sayfası olarak saymazlar; çünkü bu sayfanın aslında 404 ID'sine sahip  bir kullanıcının profili olup olmadığını bilemezler. Daha sonra arama sonuçlarında bu sayfa ekrana yansır ve sizin için hoş olmaz.

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
        $test = "Bir şey";

        echo $test; // yanlış
        print $test; // yanlış

        return $view->bind('test', $test); // doğru
        return $view->render('birsey.php', array('test' => $test)); // doğru
        return $test; // doğru, dönen veri mutlaka view katmanına ulaşacaksa.
    }
}
```

**b. View'lar, Controller'dan gelen mümkün olan en az bilgiyle çalışmalıdır.**

Bazen `Controller` sınıfları `View`'a gereğinden fazla veri gönderir ve bu sıkıntı çıkarabilir. Genel olarak bir `Controller`, `View` katmanına mümkün olan en az veriyi göndermelidir.

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

**c. Zayıf controller sınıfları, şişman modeller.**

// Yakında

### - Notice ve Warning'ler birer bugdur ve düzeltilmeleri gerekir. ###

PHP çok katı kurallara sahip değildir. Bu yüzden ufak çaplı, basit hatalar bazen görmezden gelinir. Bu tür hatalar `E_DEPRECATED`, `E_STRICT`, `E_NOTICE` ve `E_WARNING` olarak kayıtlara eklenir (hepsine [http://php.net/manual/en/errorfunc.constants.php](http://php.net/manual/en/errorfunc.constants.php) adresinden bakabilirsiniz).

Ancak, çoğu `PHP` geliştirici bunları fazla umursamıyor. Buradaki hataların düzeltilmesi, projenizin geleceği için son derece önemlidir.

1. `E_DEPRECATED` kullanılan bir `PHP` özelliğinin ileride PHP'den kaldırılacağını belirtir (örneğin `mysql` ile başlayan fonksiyonlar).
2. `E_STRICT` yazılan script'in hatalı olabileceğini ve sıkı kurallara uyulmasını gerektiğini belirtir (örneğin statik olmayan bir methodu statik olarak çağırmak).
3. `E_WARNING` runtime esnasında ortaya çıkan uyarılardır.
4. `E_NOTICE` runtime esnasında ortaya çıkan bildirimlerdir. Warning kadar önemli değildir (örneğin favicon ikonunun bulunamaması gibi).

Bu yüzden geliştirme ortamınızda hata raporlamanın açık durumda olması ve her türlü hata seviyesindeki sorunları çözüyor olmanız sizin için bir avantajdır.

### - Hataları analiz etmek için backtrace kullanın. ###

Uygulamanızda bir hata aldınız, ancak neden böyle bir hata aldığınızı bilmiyorsanız PHP'in `backtrace` (geri sarma) özelliğini kullanabilirsiniz.

PHP, siz istemedikçe hatalar hakkında stack trace bilgisi vermez. Sadece `X dosyasının 55. satırında hata oluştu` der.

Stack trace hakkında bilgi sahibi olmak için:

1. `Whoops` adlı paketi (http://filp.github.io/whoops/) uygulamanızda kullanabilirsiniz (önerilen).
2. `debug_backtrace()` fonksiyonunu kullanarak stack trace'i kendiniz takip edebilirsiniz.

Normalde hiçbir PHP kütüphanesinden bu yazımda bahsetmeyecektim, ama bana kalırsa en yararlı kütüphanelerden biri şuan `Whoops` bu yüzden ondan bahsetmek istedim. Kullanmamanız için hiçbir sebep yok.

### - @ kullanmayın. ###

`@`, PHP'de muhtemel hataların ekrana yansımaması için onları sessizleştirir. Siz istediğiniz kadar hataları sessizleştirin, susturun, onlar oluşmaya devam edecektir. Bu yüzden hataları susturmak yerine çözmeye çalışın.

### - ?> kullanmayın. ###

`PHP` kullanırken açılan tagları `?>` kullanarak kapatmanıza gerek yok. Kapattığınız takdirde `?>` tagından sonra yeni satır veya boşluk gibi karakterler kalabiliyor. Bu karakterler PHP tarafından `output` (çıktı) olarak algılandığı için `Headers already sent` hatası, sessionların oluşturulamaması, header kodlarının değiştirilememesi gibi hatalarla karşılaşabiliyorsunuz.

Bu yüzden, `good practice` (İyi Kullanım), dosya sonundaki kapanış taglarını kullanmamaktır.

### - Short tags kullanmayın. ###

Bazı yazılımcılar `<?php` yerine `<?` kullanıyor. Bu son derece yanlış. Short tag kullanacaksanız `short_open_tag` mutlaka açık konumda olmalıdır. Kapalı olursa ne olur? Kaynak kodlarınız tarayıcıya çıkabilir.

Dünya çapında birçok web sitesi bu tür basit dikkatsizliklerin kurbanı olabiliyor. Türkiye'de bildiğim kadarıyla `sahibinden.com` var, kaynak kodu tarayıcıya çıkarmayı başarabilen (!) ender firmalardan.

Bu yüzden, `<?php` dışında hiçbir `açılış etiketini` kullanmayın.

> Not: `PHP 5.4` ve üzeri versiyonlarda, `echo` yerine kısa ekrana yazdırma etiketi olan `<?=` kullanılabilir, ancak açılış etiketleri yine `<?php` olarak yazılmalıdır.

### - Projelerinizi açık kaynaklı olarak paylaşıyorsanız, .gitignore kullanın! ###

Yanlışlıkla sunucu, FTP, veritabanı veya API bilgilerinizin olduğu dosyaları `GitHub`'a yüklemeyin. Gizli kalması gereken dosyaları düzgün şekilde `.gitignore` kullanarak gizleyin. Daha sonra silseniz bile commit logları kaldığı için başkaları tarafından görünebiliyorlar. Özellikle veritabanı ve API bilgilerinin tutulduğu konfigürasyon dosyaları, mümkün olduğunca ambar üzerinde tutulmamalıdır.

### - Kullandığınız açık kaynaklı projelerin güvenli olduklarından emin olun. ###

Aşağıda mükemmel yazılımcıların açık kaynaklı projeleri yer alıyor. Bedava `VPS` isteyen var mı?

[https://github.com/search?q=exec+sudo+%24_GET&type=Code](https://github.com/search?q=exec+sudo+%24_GET&type=Code)

Sorunu anlamayanlar varsa anlatmaya çalışayım. Buradaki scriptlerin bazıları, gelen `GET` isteğini kontrol etmeden direkt olarak linux üzerinde yönetici yetkisiyle çalıştırıyorlar. Yani birisi "Hey linux, bana sunucunun şifresini verir misin?" diye sorabilir veya "Hey linux, kendine format atar mısın?" tarzındaki komutları çalıştırabilir.

Açıkcası kullanacağınızı hiç sanmıyorum ama, siz yine de kimin yazdığı belli olmayan açık kaynaklı projeleri kullanmamaya çalışın. Bunların, hackerların size sokmaya çalıştığı `shell` dosyalarından hiçbir farkı yok. Bu dosyaları, kendiniz sitenize yüklemeyin.

### - Composer kullanın. ###

// Yakında

### - Statik fonksiyonları kullanırken dikkatli olun. ###

// Aslında kullanılması gereken durumlar var. Bu yüzden hem neden iyi, hem neden kötü ikisini de anlatalım.

### - PHP asenkron çalılabilir. ReactPHP'i tanıyın. ###

// PHP nasıl non-blocking IO'ya sahip olabilir ve nasıl çalışır.

### - Ağır sorguları önbellekleyin. ###

// Yakında

### - PSR standartlarına uyun. ###

`PSR` standartları, `PHP-FIG` ekibinin, `framework`ler (Çatılar) arası kod düzenini ve uyumluluğu sağlamak için oluşturduğu yazılım geliştirme standartlardır. `PHP-FIG`'in açılımı, `Framework Interop Group` olmakla beraber, frameworkler arasında birlikte çalışabilirliği desteklemek için kurulmuş bir oluşumdur. Bu oluşum sonrasında birçok popüler PHP projesi `PSR` standartlarına uyma kararı almıştır. Günümüzde son derece popüler olan `PSR` standartlarına uymak sizin için büyük avantaj sağlayacaktır.

`PSR` standartları [https://github.com/php-fig/fig-standards](https://github.com/php-fig/fig-standards) reposu üzerinde tutulmaktadır. Kabul edilen standartlar `accepted` klasörü içerisinde bulunabilir.

Bu yazıyı yazdığım sırada kabul edilmiş `5` tane `PSR standardı` bulunmaktadır. Bunlar `PSR-0`, `PSR-1`, `PSR-2`, `PSR-3` ve `PSR-4`'tür.

**PSR'a girmeden...**

`PSR` standartları oluşturulurken bazı terimler kullanılmıştır. Standartları Türkçe'ye çevirirken kuralların yanına parantez içerisinde terimlerini yazacağım için önce bu terimlerin ne anlama geldiğini inceleyelim.

* "MUST", "SHALL" ve "REQUIRED": Mutlaka yapılması gereken maddeleri belirten kurallardır.
* "MUST NOT" ve "SHALL NOT": Asla yapılmaması gereken maddeleri belirten kurallardır.
* "SHOULD" ve "RECOMMENDED": Yapılması tavsiye edilen, ama bazı durumlarda şart koşulmayan kurallardır.
* "SHOULD NOT": Yapılması tavsiye edilmeyen, ama bazı durumlarda göz yumulabileceği belirtilen kurallardır.
* "OPTIONAL" ve "MAY": Tamamen geliştiricinin opsiyonuna sunulan kurallardır.

Örneğin, bir kuralın başında `(MUST)` yazıyorsa, o kurala uyma mecburiyetiniz bulunmaktadır. `(MAY)` yazıyorsa, o kurala uyup uymama seçimi size bırakılmıştır. Diğerlerini yukarıda bulabilirsiniz.

**PSR-0 - Autoloading (Otomatik Yükleme) Standardı**

Bu standart, sınıflardan otomatik yüklenmesi amacıyla oluşturulmuştur.

Maddelerimiz:

- `(MUST)`: Her sınıf ve namespace `\<Sağlayıcı Adı>\(<Namespace>\)*<Sınıf Adı>` yapısına uymalıdır.
- `(MUST)`: En üst seviye namespace daima `Sağlayıcı Adı` olmalıdır.
- `(OPTIONAL)`: Her namespace istediği kadar alt namespace'e sahip olabilir.
- `(Bilgi)`: Her namespace ayırıcı (`\`), `DIRECTORY_SEPARATOR` (Klasör Ayırıcı) anlamına gelmektedir.
- `(Bilgi)`: Sınıf adındaki her `_` karakteri, `DIRECTORY_SEPARATOR` (Klasör Ayırıcı) anlamına gelmektedir. `_` karakterinin namespace içerisinde bir anlamı yoktur.
- `(Bilgi)`: Tam namespace ve sınıf yapısı, yükleme esnasında sonuna `.php` eklenerek yüklenmektedir.
- `(OPTIONAL)`: Sağlayıcı adı, namespace ve sınıflardaki alfabetik karakterler, istenilen her türlü küçük veya büyük harf kombinasyonuyla yazılabilir.

Örnekler:

* \Doctrine\Common\IsolatedClassLoader => /proje/klasoru/lib/vendor/Doctrine/Common/IsolatedClassLoader.php
* \Symfony\Core\Request =>/proje/klasoru/lib/vendor/Symfony/Core/Request.php
* \Zend\Acl => /proje/klasoru/lib/vendor/Zend/Acl.php
* \Zend\Mail\Message => /proje/klasoru/lib/vendor/Zend/Mail/Message.php
* \namespace\package\Class_Name => /path/to/project/lib/vendor/namespace/package/Class/Name.php
* \namespace\package_name\Class_Name => /path/to/project/lib/vendor/namespace/package_name/Class/Name.php

**PSR-1 - Basic Coding (Temel Kodlama) Standardı**

**a. Genel bakış**

* `(MUST)`: Dosyalar sadece `<?php` ve `<?=` taglarını kullanabilir.
* `(MUST)`: Dosyalar, PHP kodu için sadece `UTF-8 (BOMSUZ)` seçeneğini kullanabilir.
* `(SHOULD)`: Dosyalar, ya sembol oluşturabilir (örn. sınıflar, fonksiyonlar, constantlar vb.) veya yan etki yaratabilirler (örn. çıktı vermek, .ini ayarlarını değiştirmek, vb.) ancak ikisini birden YAPAMAZLAR.
* `(MUST)`: Namespaceler ve sınıflar mutlaka bir autoloading standardına uymak zorundadır. (Örn. PSR-0 veya PSR-4).
* `(MUST)`: Sınıf isimleri mutlaka `StudlyCaps` şeklinde yazılmak zorundadır. (örn. KdvHesaplayici)
* `(MUST)`: Sınıf constantları mutlaka büyük harflerle ve ayraç olarak `_` şeklinde yazlmak zorundadır. (örn. const BIR_CONSTANT)
* `(MUST)`: Method isimleri mutlaka `camelCase` şeklinde yazılmak zorundadır. (örn birFonksiyonAdi())

**b. Dosyalar**

* `(MUST)`: PHP kodları ya uzun olan `<?php ?>` yapısını, ya da `<?= ?>` olan kısa echo yapısını kullanabilir. Başka her türlü varyasyon yasaklanmıştır.

**c. Karakter encoding**

* `(MUST)`: Dosyalar, PHP kodu için sadece `UTF-8 (BOMSUZ)` seçeneğini kullanabilir.

**d. Yan Etkiler**

// Yakında

**e. Constantlar**

// Yakında

**f. Propertyler**

// Yakında

**g. Methodlar**

// Yakında

**PSR-2 - Coding style (Kodlama Stilleri) Standardı**

`PSR-2`, `PSR-1`'in devamı niteliğindedir.

// Yakında

## API Geliştirme ##

// API geliştirme hakkında kısa bir yazı, niye kullanıyoruz.

### Kullanılabilir extensionlar ve araçlar. ###

// Yakında

### Versiyon prefixi kullanın. ###

// Yakında

### API anahtarı, URI'e eklenmemeli ve HTTP Auth ile gönderilmeli. ###

// Eskiden nasıl yapılıyor, yeni şekilde nasıl yapılmalı.

### URI düzenine uyun. ###

// Resourceful? type/identifier/subtype/identifier kuralına uyulmalı

### Doğru status kodları kullanılmalı ve response mesajı 2. planda olmalı. ###

// { message } ek bilgiye sahip olmalı. Hatayı asıl anlatacak kısım header kodudur.

### Hit izinleri ###

// Yakında

---
# Frontend #
---

## CSS ve HTML ##

### CSS Explosiona maruz kalmayın. ###

// CSS explosion nedir? Nasıl korunulur?

### Temel attributeler dışında diğer HTML attributelerini kullanmayın. ###

Bildiğiniz gibi HTML inline attibuteleri destekler, örneğin:

```html
<div height="100" width="100"></div>
```

yazarak `100x100 pixel` boyutunda bir div oluşturabiliriz, ancak HTML attributeleri yerine CSS propertylerini kullanmak daha sağlıklıdır.

```html
<div style="height: 100px; width: 100px;"></div>
```

HTML sadece markup için kullanılmalı ve CSS ile şekillendirilmelidir. Kullanmanız gereken HTML attributeleri class, id ve style olmalıdır. Ancak, class harici tüm attributeler, eninde sonunda bir kötü kullanıma denk gelecektir. Aşağıda bunları detaylıca inceleyeceğiz.

### Inline stil yazmaktan kaçının. ###

Yukarıdaki örnekte kullandığımız CSS `inline CSS` (yani bir CSS dosyasına değil de, direkt HTML içinde style olarak yazmak) olarak geçiyor ve bu son derece yanlış bir kullanım. Inline kullanıma csslerimizi `<style>` içerisinde yazmakta dahildir.

Şimdi bu neden sıkıntılıdır onu inceleyelim. Aslında burada birkaç sıkıntı var, bu yüzden hepsini örneklerle anlatacağım.

Birincisiyle başlayalım. Örneğin, 50x50 boyutunda 3 tane div oluşturalım ve bunlara arkaplan olarak transparan bir renk verelim.

```html
<div style="height: 50px; width: 50px; background-color: rgba(0,0,0,.5);"></div>
<div style="height: 50px; width: 50px; background-color: rgba(0,0,0,.5);"></div>
<div style="height: 50px; width: 50px; background-color: rgba(0,0,0,.5);"></div>
```

Şuana kadar bu yazıyı okuduysanız sorunu anında anlamışsınızdır. DRY kuralını bozmuş oluyoruz! Arkaplanı yeşil yapmak istesek ne olacaktı? 3 farklı yeri tek tek değiştirmemiz gerekecekti.

İkincisi sıkıntı, peki bu divler çok farklı sayfalarda olsaydı? Hangi sayfalara bunu eklediğini hatırlamak için uğraşacaktın. Sen anasayfa.html üzerindeki divleri elle değiştirdin, ama kayit.html'deki div'i değiştirmeyi unuttun. Bu durumda "Aaaa, unutmuşum onu." diyemezsin. Her zaman, tek yeri değiştirdiğinde tüm uygulamanın değişiyor olması gerekir. Bunu yapmazsan, projeyi yönetmek çok zor olur.

Üçüncü sıkıntı, eğer ileride doğru kullanım olan CSS'ye geçiş yaparsan ve kutucuklarını `classlar` üzerinden yönetirsen, inline `style` attributesi sorun çıkaracaktır çünkü `style` ile belirtilen css propertyleri `daha baskındır`, bu yüzden `class` ile tanımlanan propertyleri override (üzerine yazmak) ederler.

Bunu örnekle anlatayım. Yukarıda bahsettiğimiz yeşil dive "kutucuk" adını verelim ve onun css'ini yazalım.

```css
.kutucuk {
    height: 50px;
    width: 50px;
    background: green; //Arkaplanı yeşil yaptık
}
```

Daha sonra, `class` attributesiyle, oluşturduğumuz divlerin `kutucuk` attributesine sahip olmasını sağlayalım.

```html
<div class="kutucuk" style="height: 50px; width: 50px; background-color: rgba(0,0,0,.5);"></div>
<div class="kutucuk" style="height: 50px; width: 50px; background-color: rgba(0,0,0,.5);"></div>
<div class="kutucuk" style="height: 50px; width: 50px; background-color: rgba(0,0,0,.5);"></div>
```

Şuan, siz arkaplan olarak yeşil bekliyor olabilirsiniz, ancak arkaplan transparan kalmaya devam edecektir. Sebebini 3. maddede söylemiştim. `style` ile eklenen css propertyleri, daha baskındır. Öncelik daima onlarındır.

Bunu önlemek için `!important` kullanabilirsiniz. Örneğin:

```css
.kutucuk {
    background: green !important; //Arkaplanı yeşil yaptık ve baskın hale getirdik
}
```

yazsaydık, bu kez öncelik bizde olacaktı ve arkaplan yeşil olacaktı. Ancak, `!important` kullanmak, bazı istisnalar haricinde kötü sayılmaktadır.

Şimdi, doğru olanı yapalım ve inline cssleri silelim.

```css
.kutucuk {
    height: 50px;
    width: 50px;
    background: green;
}
```

```html
<div class="kutucuk"></div>
<div class="kutucuk"></div>
<div class="kutucuk"></div>
```

Daha güzel, ancak geliştirebileceğimiz noktalar var.

> Not: CSS'de öncelik konusunu biraz açalım.

Ben CSS'i ilk öğrendiğim zamanlarda bu konuyu anlamakta güçlük çekmiştim, ama CSS çok basit bir mantıkla çalışıyor. `!important` ile belirtilen propertyler, daima öncelik kazanıyor. Daha sonra `style` attributesi içerisindekiler geliyor (ama style kullanmak iyi birşey değil demiştik), en son olarak, propertyler belirtiliş derinliğine göre ekleniyor.

Aşağıdaki örneği ele alalım.

```css
/* Height ve width değerlerinin olduğunu varsayın. Yoksa bu kutucuklar 0 boyutunda olduğu için
tarayıcıda görünmeyecekler. :) */
div {
    background: #111;
}

.kutucuk {
    background: #222;
}

div.baba-kutucuk .kutucuk {
    background: #333;
}

.onemli-kutucuk {
    background: #444 !important;
}
```

Sırayla aşağıdaki divlerin ne renk alacağına bakalım.

**<div></div>**

Bu, `#111` rengini alacaktır. Çünkü tüm divler, global olarak #111 rengini alması için ayarlanmış.

**<div class="kutucuk"></div>**

Bu, `#222` rengini alacaktır. Çünkü, üstteki örnekte olmayan biçimde, class değeri eklenmiş.

**<div class="baba-kutucuk"><div class="kutucuk"></div></div>**

Derinlik konusu burada başlıyor. Burada kutucuğun alacağı renk `#333` olacaktır. Çünkü "baba kutucuğun altındaki kutucuk" diye belirtilmiş ve bu ona bir derinlik katmış. Herşey kutucuk olabilir, ama her kutucuğun babası, baba kutucuk olmayabilir. :)

Eğer baba kutucuğun üstüne dede kutucuk ekleseydik, o zaman `.dede-kutucuk .baba-kutucuk .kutucuk { }` içerisine yazdığımız css rengi çalışacaktı. Bir elementi, ne kadar derin belirtirseniz, o kadar önem kazanır ve orada yazılan renk değeri öncelik kazanır.

**<div class="baba-kutucuk"><div class="kutucuk" style="background: #555"></div></div>**

Bu, `#555` rengini alacaktır. Çünkü `style` ile eklenen değerler, soyağacından daha önemlidir.

**<div class="baba-kutucuk"><div class="onemli-kutucuk" style="background: #555;"></div></div>**

Bu, `#444` değerini alır, çünkü önemli kutucuğun önemi `!important` kullanarak belirtilmiş.

Peki, tüm divlere global olarak `!important` ekleseydik, sonra `onemli-kutucuk` içine de `!important` ekleseydik? O zaman global olarak `!important` ekleyen tasarımcının kulağını çınlatabilirdiniz. :)

### CSS Inheritence ###

Kutucuk örneğine bir flashback yapalım.

```css
.kutucuk {
    height: 50px;
    width: 50px;
    background: green;
}
```

Peki biz başka arkaplana sahip kutucuklar istersek? Mesela, siyah ve beyaz renklerine sahip kutucuklar oluşturalım.

```css
.kutucuk-yesil {
    height: 50px;
    width: 50px;
    background: green;
}

.kutucuk-beyaz {
    height: 50px;
    width: 50px;
    background: white;
}

.kutucuk-siyah {
    height: 50px;
    width: 50px;
    background: black;
}
```

Ding! DRY kuralını yine bozduk. Kutucukları 30x30 boyutuna getirmek istersek ne yapacağız? 3 farklı yeri düzenleyeceğiz. Bunun yerine, `CSS inheritence` kullanıp, ortak kullanılan propertyleri paylaştırsak nasıl olurdu?

```css
.kutucuk  {
    height: 50px;
    width: 50px;
}

.kutucuk-yesil {
    background: green;
}

.kutucuk-beyaz {
    background: white;
}

.kutucuk-siyah {
    background: black;
}
```

Artık `<div class="kutucuk kutucuk-yesil"></div>` yazdığımızda, hem kutucuğun boyutları, hem de arkaplan rengi alınmış olacak.

### height propertyini kullanmayın. ###

Yukarıdaki örneklerimi bilerek önce kötü verip daha sonra iyi hale getirmeye çalışıyorum ki anlamak kolay olsun.

Eğer bir objeye `height` kullanarak yükseklik ekliyorsan, o obje daima o yükseklikte kalacaktır. Neredeyse hiçbir zaman (çok ufak istisnalar dışında) `height` değeri verilmez ve elementlerin boyutlandırması otomatik olarak yapılır.

Sen kutucuğun altına, 1 paragraf yazı da yazabilirsin, 10 paragraf yazı da. Haliyle, 10 paragraf yazı daha uzun olacaktır. Bu yüzden, çok zorda kalmadıkça `height` kullanmayın.

### `<br>` kullanmayın. ###

Hiçbir istisnası yok. `<br>` kullanma.

### `&nbsp;` kullanmayın. ###

`&nbsp;`, tarayıcılar tarafından parselenirken ufak bir boşluk oluşturur. Şunun gibi (&nbsp;)

Siz asla `&nbsp;` kullanmayın. Eğer bir elementi kaydırmak istiyorsanız, `margin` veya `padding` kullanarak kaydırın.

### Açıkta element bırakmayın. ###

Aşağıdaki örneğe bakalım.

```html
<div class="ornek">
    Merhaba dünya!
</div>
```

Merhaba dünya yazısını seçmekte zorlanırsınız. En kötü ihtimalle, yazıyı açıkta bırakmaktansa `<span>` içerisine alın.

```html
<div class="ornek">
    <span>Merhaba dünya!</span>
</div>
```

### Negatif piksel margini çok kullanmayın. ###

Bugün çok satan bir premium Themeforest teması üzerinde oynama yapmam gerekiyordu ve inanın bu yazıyı yazma sebebim de bu oldu.

Temanın heryeri, ama heryeri negatif margin dolu. `margin: -3px;`, `margin-top: -15px;`

Bu kişinin yaptığı tema şuna benziyordu.

Önce, navigasyon elementine `margin-bottom: 30px` propertysini eklemiş ve içerik elementini biraz aşağıya ittirmiş. (resmini çizmeye çalışırsak, şöyle)

```
| Navigasyon |
--------------
--------------
--------------
|   İçerik   |
```

Daha sonra, içerik elementine `margin-top: -30px;` vermiş ve geri almış.

```
| Navigasyon |
|   İçerik   |
```

...Niye?

### Semantic HTML yazın. ###

// Neki bu semantic?

### Ayraçları HTML ile yazmayın. ###

İnanılmaz derecede sık karşılaşıyorum bu durumla.

```html
<a href="/anasayfa.html">Anasayfa</a>
<span class="divider"> | </span>
<a href="/hakkimizda.html">Hakkımızda</a>
<span class="divider"> | </span>
<a href="/iletisim.html">İletişim</a>
```

Yapmaya çalıştığı şey çizersek şu:

```
Anasayfa | Hakkımızda | İletişim
```

1999'a hoşgeldiniz.

Daha düzgün hale getirelim. CSS pseudo selectorleri destekleyeli yıllar oldu.

```html
<ul>
    <li>
         <a href="/anasayfa.html">Anasayfa</a>
    </li>
    <li>
         <a href="/hakkimizda.html">Hakkımızda</a>
    </li>
    <li>
         <a href="/iletisim.html">İletişim</a>
    </li>
</ul>
```

```sass
li {
    border-right: 1px solid #fff;

    &:last-child {
        border-right: 0;
    }
}
```

Alacağın sonuç:

```
Anasayfa | Hakkımızda | İletişim
```

### Assetleri yüklerden http veya https kullanmayın. ###

Uygulamanızda kullanacağınız assetleri yüklerken hangi protokolün kullanılacağına tarayıcının karar vermesini sağlamak iyi bir kullanım sayılmaktadır.

Bu yüzden belirli bir protokol belirlemek yerine, assetlerinizin hangi protokol üzerinden yüklenmesi gerektiğini tarayıcılara bırakabilirsiniz. Protokol belirtmemek için, sadece `//` yazmanız yeterlidir.

```html

<script type="text/javascript" src="//assets/app.js">

```

### Yazılarınızı BÜYÜK HARFLE yazmayın! ###

```
<p>
    EVET, HERKES YAZILARINI BÜYÜK HARFLERLE YAZMALI.
    ÇOK ŞİRİN GÖRÜNÜYORLAR.
</p>
```

Asla yazıların HTML'ye yazdığınız şekilde görüneceğini beklemeyin. Ne zaman gerekiyorsa, gerekli CSS attributelerini kullanarak yazılarınızı şekillendirin. Eğer bir yazının büyük harflerle yazılması gerekiyorsa, bunu CSS propertyleri ile belirtin.

```
p {
    text-transform: uppercase;
}
```

### Linkleri doğru şekilde yazın. ###

Linkler yazılırken `www` yazılmamalı ve slash ekli olmalıdır. Örneğin `http://www.anilunal.com` hatalıyken, doğru olan `http://anilunal.com/`'dur.

### Tarayıcı uyumluluğunu test edin. ###

// Browserstack, caniuse, shim/modernizer vs

### Fazla opacity ve alpha kullanmayın. ###

// GPU

### ID seçicilerini gerekmedikçe kullanmayın. ###

// Sebep

### CSS selectorlerinizi kısa tutun. ###

// Yeah

### Auto prefixing ###

// Auto prefix grunt modülü, PrefixR API vs.

## JavaScript ##

### Tanımlamaları çoğu zaman JavaScript Object Literals kullanarak yapın. ###

JavaScript Object Literalleri PHP'nin sınıf yapısına benzer, ancak aynı şey değildir.

Aşağıdaki gibi bir JavaScript object literal tanımlayabiliriz.

```js

//JavaScript object literaller PHP'deki sınıflara benzer, ancak aynı şey değildir.
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

> Not: `JavaScript Object Literals` ile `JavaScript Object Notation (JSON)` farklı şeylerdir. Karıştırmayın.

`JavaScript Object Literals` ile `JSON` farkı:

a. Object literallerin keyleri `attribute`dir, JSON'un `string`dir.

```js
var user = { name: "Anıl" } // Object literal
var user = { "name": "Anıl" }  // Object notation (Json)
```

b. Object notation'da method tanımlanamaz, object literal'de tanımlanabilir. Yukarıdaki örnekte `isOld()` bir methoddur.

c. JSON'lar genellikle veri taşımak (API'lerde) ve veri saklamak için kullanılırken, object literaller genellikle OOP amacıyla kullanılır.

### ; prefixini kullanın. ###

Oluşturduğunuz ilk anonim fonksiyondan önce, `;` prefixini kullanmak daima iyi bir kullanımdır. Bunun sebebi ise, JavaScript dilinde `semicolon`ların (noktalı virgül) çoğu zaman önemsenmemesinden kaynaklanmaktadır.

Örneğin, siz noktalı virgül kullanmadığınızda kodlarınız çalışmaya devam edecektir. Ancak, sizden önceki anonim fonksiyon noktalı virgül kullanmadıysa, uygulamanız scope sorunları yaşayabileceği için hatalı çalışacaktır.

Bunun neden kaynaklandığını örnek vererek hemen açıklayayım. Örneğin, çok basit bir anonim fonksiyon oluşturdunuz ve bunu `2.js` olarak kaydettiniz.

```js
// 2.js
(function() {
    alert("2");
})();
```

Bunu sayfanıza eklediniz ve test ettiniz:

```html
<script type="text/JavaScript" src="//2.js"></script>
```

Şuan sayfaya girdiğinizde, ekranda `2` yazısını göreceksiniz. Şimdi, `1.js` dosyanızı oluşturun ve bu dosyada noktalı virgül kullanmayın:

```js
// 1.js
(function() {
    alert("1");
})() // <-- Burada noktalı virgül olmalıydı.
```

Aynı şekilde, `1.js`'yi sayfanıza dahil edin ve çalıştırın:

```html
<script type="text/JavaScript" src="//1.js"></script>
<script type="text/JavaScript" src="//2.js"></script>
```

Ekrana sadece `1` yazısı geldi, değil mi? Ancak siz muhtemelen `1 ve 2` bekliyordunuz. Bunun sebebi, anonim fonksiyon düzgün kapanmadığı için, JavaScript'in çalışırken scriptimizi şu şekilde algılamasından kaynaklandı.

```js
// 1.js
(function() {
    alert("1");
})()(function() { // <-- Aslında 1. scope içerisinde sayılırız halen.
    alert("2");
});
```

Sorun şu ki, daha 1. anonim fonksiyon kapanmadan 2.si başlatılmaya çalışılıyor.

Şunu diyebilirsiniz: "Ama ben hep noktalı virgül kullanıyorum?". Evet, kullanıyor olabilirsiniz. Ama sizden önce sayfaya yüklenen bir kütüphane kullanmıyorsa? Siteye yüklediğiniz slider pluginini yazan kişi noktalı virgül alışkanlığı olan birisi değilse?

Sonuç olarak, ilk başa yazdığınız `;` prefixi, aslında sizden önce gelen bir anonim fonksiyonun düzgün olarak kapatılmasını sağlıyor.

```js
// 1.js
(function() {
    alert("1");
})()

;(function() { // <-- Buradaki ;, aslında üstteki anonim fonksiyonu kapatıyor düzgünce.
    alert("2");
});
```

Ne kadar hoş ve mantıklı bir kullanım, değil mi?

### Daima 'use strict'; kullanın. ###

`'use strict';`, JavaScript dilinde, yazdığınız scope içerisinde sıkı kurallara uyulacağını belirtmektedir. Örneğin, oluşturduğunuz anonim fonksiyon içerisinde yazabilirsiniz:

```js
;(function() {

    'use strict'; // Artık bu scope içerisinde sıkı kurallara uyuyoruz.

})();

```

veya sadece kullandığınız ufak fonksiyonu kapsamasını da sağlayabilirsiniz:

```js
function merhaba() {
    'use strict'; // Artık bu fonksiyon içerisinde sıkı kurallara uyuyoruz.
}
```

Peki, nedir bu sıkı kurallar? Sıkı kurallar, `Ecmascript5` ile gelen özelliklerden birisidir. Sizin yapabileceğiniz bazı yanlışlıkları önleyerek sizi uyaran ve bu yüzden daha fazla exception fırlatılmasına sebep olan bir ibaredir. Bu ibare, `güvensiz` sayılabilecek bazı işlemleri engeller (örneğin, global objeye erişim sağlanması) veya yapılabilecek bazı basit kod hatalarını (örneğin, bir değişkeni declare etmeden değer atamaya kalkmak) algılayarak exception fırlatır.

Benim tavsiyem, daima tüm kodlarınızı kapsayacak şekilde (ilk örnekteki gibi anonim fonksiyon açıldığı anda olabilir) `'use strict';` ibaresini yazın. Bu sizi biraz daha sıkı çalışmanıza ve dolayısıyla daha az hata yapmanıza yardımcı olur.

Unutmadan, birçok `linter` (kod analizi yapan uygulamalar), bu ibarenin tanımlanmasını mecburi kılmaktadır. Size büyük avantajlar sağladığı için, `'use strict';` ibaresini kullanmak good practice (iyi kullanım) sayılmaktadır ve kullanmanız şiddetle tavsiye edilir.

### JavaScript hataları izlenebilir. İzleyin. ###

Birçok geliştirici, yazdığı JavaScriptlerin ne durumda olduğunu izlemiyor. Geliştirme ortamında kendi testlerini yapıyor ve production sunucusuna atıyor. Bu doğru, ama scriptin ne durumda nasıl çalıştığını nasıl anlayabilirsin ki? Belki belirli iPhone ve iOS versiyonlarında çalışmıyor? Belki belirli jQuery versiyonlarında veya sayfada belirli bir plugin varken veya belli bir tarayıcıda çalışmıyor? Belki kullanıcının tarayıcısında bir eklenti varken bile çalışmıyor olabilir. Özellikle, JavaScript pluginleri geliştiriyor ve diğer sitelerde kullanıma sunuyorsanız, yazdığınız koddaki exceptionları veya hataları izleyen bir monitoring sistemi geliştirmek ve kullanmak size büyük avantaj sağlayacaktır.

Bunu yapmaktaki amacımız çok basit. Client tarafında alınan hataları, daha sonra incelemek üzere kaydedeceğiz. Örneğin, JavaScript'te bir exception fırlatıldığında, bu exceptionu bir API üzerinden kendi veritabanımıza kaydedebiliriz. Daha sonra bu hataların neden kaynaklandığını analiz edip çözüm üretebiliriz. Eğer yazdığımız JavaScript'i izlemiyorsak, bir hata olduğundan nasıl emin olabiliriz?

Eminim bu işlemi yapan birçok açık kaynaklı kütüphane vardır ancak ben şahsen `Bugsnag`'ın JavaScript özelliğini kullanıyorum. Kullanımı çok basit. Size verdiği JavaScript dosyasını sitenize koyuyorsunuz ve herhangi bir hata oluştuğunda, Bugsnag dashboard ekranınızda bu hata çıkıyor. `Bugsnag`, dilerseniz `GitHub` üzerinde otomatik issue oluşturabiliyor veya `Hipchat` üzerinden size bildirim gönderebiliyor veya SMS atabiliyor. [https://bugsnag.com/platforms/JavaScript](https://bugsnag.com/platforms/JavaScript) adresinden inceleyebilirsiniz.

Geliştirme yapmak kadar, geliştirdiğiniz projeleri izlemek de önemli. İzleyin, hataları algılayın, çözüm üretin.

### Asenkron, deferred ve DOM eventlerinden bağımsız yüklemeler yapın. ###

Yazıya başlamadan önce, başlıktaki terimlerin ne olduğunu anlatmak istiyorum. Kendimi en rahat ifade edebildiğim anlatım şekli olan örneklendirme ile anlatmaya devam edeceğim.

Farzedelim ki, sayfamızda 10 tane JavaScript plugini olsun.

```html
<script type="text/JavaScript" src="//jquery.js">
<script type="text/JavaScript" src="//1.js">
<script type="text/JavaScript" src="//2.js">
<script type="text/JavaScript" src="//3.js">
<!-- ... -->
<script type="text/JavaScript" src="//10.js">
```

Bu örnekte, tüm dosyalar, blocking/senkron olarak yükleniyor. Blocking dediğimiz olay, bir şey yüklenirken, diğer(ler)inin bekliyor olması Bu programcılıkta daima aynıdır. Bir şey olurken bir şeyin onu beklemesine `blocking` denir. Mesela PHP blocking bir dildir; çünkü bir satırdaki işlem yapılırken alt satır yukarıdaki işlemin bitmesini bekler. Tarayıcı her seferinde tek bir dosyayı yüklüyor. Daha sonra sıradakine geçiyor. Buna CSS ve diğer assetler de dahil, ancak bazı akıllı tarayıcılar resimler gibi önemsiz şeyleri otomatik olarak `asenkron `indirebiliyorlar.

Burada, şu avantajımız var. Birincisi, `1.js` ve diğer JavaScript dosyaları, `jQuery`'nin yüklenmiş olduğundan emin olabiliyorlar, bu yüzden kendi içlerinde direkt olarak `jQuery` özelliklerini kullanabiliyorlar. Ancak şu dezavantajımız da var. Birincisi, sayfa açılış süresi uzuyor, ikincisi `DOM Eventlerine` bağlı kalıyoruz.

`DOM Eventleri` dediğimiz olay, `sayfanın` (Document Object Model) duruma göre olay ateşlemesidir. Örneğin, her şey yüklendiğinde otomatik olarak `DOMContentLoaded` (İçerik Yüklendi) olayını ateşler. Bu ateşleme yapıldığında, artık sayfanın tamamen yüklendiğini anlarsınız.

Şöyle bir `pseudo` örnek verebiliriz:

```js
on('DOMContentLoaded', function() {
    alert("Her şey yüklendi.");
}
```

Burada bir sorun yok, ancak en çok bilinen, `jQuery`'nin `ready` eventi, direkt olarak `DOM`'a bağlıdır. Yani, aşağıdaki gibi bir kod yazdıysanız: (bahse girerim yazdınız)

```js
$(document).on('ready', function() {

    $('.birsey').html('<span>Merhaba dünya!</span>');

});

```

bunun anlamı şudur: "Önce sayfanın yüklenmesini bekle. Sayfa yüklenmesi tamamlandığında Merhaba Dünya yazdır."

> Bilgi: jQuery kullanıyorsanız, "$(document).on('ready', function() { });" yerine daha kısa olarak "$(function() { });" yazabilirsiniz. İkisi aynı şeydir.

Bu işlem için sayfa yüklemesini beklemek bence mantıklı değil. İşte burada devreye `asenkron` yüklemeler giriyor.

Yukarıdaki verdiğimiz örneğe, aşağıdaki gibi `async` ibaresini eklediğimizde, dosyalarımız `asenkron` yüklenmeye başlayacaktır.

```html
<script async type="text/JavaScript" src="//jquery.js">
<script async type="text/JavaScript" src="//1.js">
<script async type="text/JavaScript" src="//2.js">
<script async type="text/JavaScript" src="//3.js">
<!-- ... -->
<script async type="text/JavaScript" src="//10.js">
```

`Asenkron` yükleme ne işe yarar? Artık yüklemelerimiz `non-blocking` olur, yani birini yüklemek için, tarayıcı diğerlerinin yüklenmesini beklemez. Tarayıcı birçok bağlantı açar ve hepsini birden yüklemeye başlar. Dolayısıyla, sayfa açılış süresi muazzam ölçüde artar.

Ama hemen sevinmeyin, bunun getirdiği bir sıkıntı var. Sıkıntı şu: uygulamanızdaki dosyaların yükleme sırası karışır. Yani `10.js` ilk başta, `jQuery` ise en sonda yüklenebilir ve sen `10.js` içerisinde `jQuery` özelliklerini kullandıysan, geçmiş olsun, kendini `undefined object` hatalarına hazırla.

> Not: Maksimum anlık bağlantı sayısı, tarayıcıdan tarayıcıya değişmektedir. Bu sayı, tarayıcılarda genellikle öntanımlı olarak 4-6 arasındadır. İsterseniz tarayıcı ayarlarından bunu değiştirebilirsiniz.

Bu sorunu `deferred` yüklemeler çözmektedir. `async` yerine `defer` yazılarak `deferred` yüklemeler yapılabilir. `Deferred` yüklemeler de, aynı `asenkron` yüklemeler gibi, `non-blocking` olarak çalışır. Tek farkı, `asenkron` yüklenen dosyalar, mümkün olan ilk anda çalışırlar, ancak `deferred` yüklenen dosyalar, yükleme sırasına sadık kalırlar.

Ancak, ben size Hem asenkron yükleme yapmayı, hem yüklemelerin birbirinden haberdar olmasını, hem de yükleyiciye tam hükmetmenin yolunu anlatmak istiyorum.

> Not: Aslında bunu sağlayan birçok kütüphane var, ancak ben kendim `$script.js` kullandığım için mantığı onun üzerinden anlatacağım.

Biz, `DOM` gibi, birşeyler yüklendiğinde kendi `eventlerimizi` ateşleyebiliriz. Daha sonra, ateşlenen `eventlere` göre işlemlerimizi yapabiliriz. Örneğin, `$script.js` ile, aşağıdaki gibi yüklemeler yapabiliriz.

```js
;(function() {

    // Bir $script.js asenkron yükleme ve event örneği

    $script('//jquery.js', 'jquery');
    $script(['//plugin1.js', '//plugin2.js'], 'plugins')

    $script.ready('jquery', function() {
        // jQuery yüklendiği anda çalıştırılacak callback
        alert("jQuery yüklendi!");
    });

    $script.ready('plugins', function() {
        // 2 plugin de yüklendiği anda çalıştırılacak callback
        alert("2 plugin de yüklendi.");
    });

})();
```

Bu durumda, neyin ne zaman yüklendiğinden `eventlar` aracılığıyla haberdar olabildiğimiz için, yükleme sırasını düşünmeden kodlarımızı yazabiliriz. Mesela, pluginleri çalıştırmadan önce, `jQuery`'nin yüklenmiş olduğu olayını bekleyebiliriz. Böylece, `DOM`'un yüklenmesini gerektiren işlemler için onu beklerken, `DOM` ile ilgisi olmayan (örneğin, bir üst makalede yazdığımız Bugsnag izleyicisi) sayfa yüklenmeden çalışmaya başlayabilir.

Bu bölümdeki en güzel kullanımlardan biri, `<script>` tagı içerisinde tek bir `loader.js` scripti tutmak ve bu script içerisinde `asenkron` olarak diğer scriptlerin yüklenmesini sağlamaktır.

Not: `$script.js` kütüphanesi [https://github.com/ded/script.js/](https://github.com/ded/script.js/) adresindeki repoda bulunmaktadır.

### Debug için alert kullanmayın, lütfen! ###

Eğer kullanıyorsanız da, işiniz bittiğinde silmeyi unutmayın! Bazı geliştiriciler `alert();` kullanarak debug yapıyor ve işi bittiğinde silmeyi unuturup sunucuya atıyorlar.

`alert` yerine, `console.log` veya Firebug/Web Inspector gibi araçların debugging özelliklerini kullanmaya çalışın.

### Hoisting hakkında bilgi sahibi olun. ###

JavaScript, dolayısıyla jQuery'de çok az bilinen (sadece uzun süre vakit geçiren geliştiriciler biliyor gözlemlediğim kadarıyla) bir olay hoisting. Hoistingi anlatmadan önce JavaScript'in temeline inip nasıl çalıştığını anlatmak istiyorum.

> Not: Eğer JavaScript'i gerçek anlamda öğrenmek istiyorsanız, tam bir JavaScript Ninja olan Stoyan Stefanov'ın kitaplarını tavsiye ederim.

// JavaScript, nasıl çalışıyor?

// JavaScriptin nasıl çalıştığını anladıysak, o zaman hoistingin gerekliliğini de anlamışızdır.

// Nelere dikkat etmeliyiz, good practice kullanımlar nedir.

### Object.observe ve Obverver pattern. ###

// Ecmascript 7 ile gelen object.observe özelliği ve Observer pattern

### var x || {} ###

// Yakında

### js- prefix on class selectors ###

// Yakında

### Global window objesini kirletmeyin ###

// Yakında

### Bir selector kullandığınız zaman onu önbellekte tutun. ###

// Sebep?

## Geliştirici Ortamı ##

### Sublime Text ###

// Kullanmayanlar bu bölümü esgeçsin.

### GruntJS ###

// Nedir?

// Kendi Grunt modülümüzü yazalım.

### Terminale ne kadar yakın, o kadar iyi. ###

// Yakında

### Farenizi değil, klavyenizi hızlandırın. ###

// Yakında

## Veritabanları ##

### Neden `utf8mb4_unicode_ci?` ###

// Yakında

### - Veritabanında eksi değerde olmayacak hücreler UNSIGNED olmalıdır. ###

Veritabanında oluşturduğunuz bir `TINYINT` hücre, öntanımlı olarak `negatif` ve `pozitif` değerleri alacaktır. `TINYINT`'in alabileceği değerler `-128` ile `127` arasındaki rakamlardır. Ancak, bu hücre `UNSIGNED` olarak tanımlanırsa, `0` ve `255` arasındaki değerleri kabul edecektir.

Bu yüzden, daima pozitif olacağından emin olduğunuz hücreler için (örneğin `auto increment`) hücrelerinizi `UNSIGNED` olarak tanımlamak size yarar sağlayacaktır.

## Deployment ##

Deployment, yazdığınız kodların sizin bilgisayarınzdan çıkıp sunucuya ulaşması ve çalışmasının sağlanması arasında geçen süreçtir (FTP'den dosyaları upload edip phpmyadmin'den güncel db yedeğini import ediyorsun ya, o deployment işte). Biz bu bölümde deployment konusunda bize yarar sağlayacak araçları tanıyacağız.

> Bu bölümdeki yazıları okumadan önce Versiyon Kontrol hakkında bilgi sahibi olunması büyük avantaj sağlayacaktır.

### FTP kullanmayın ###

FTP kullanmamanızı gerektiren aslında birçok sebep var. Örneğin:

1. FTP kullandığınız zaman dosyalarınız ve FTP bilgileriniz şifrelenmemiş bir protokol üzerinden iletilir. Çalınma ihtimali vardır.
2. Reponuzla sunucunuzu senkron halde tutmak zorlaşır, çoğu zaman versiyon kontrolü bozarsınız.
3. Şifrelerle çalışmak çok ilkelcedir.
4. Sadece FTP bağlantısına izin veren sunucularda çalışmak zordur.

Şimdi bunları biraz açalım: Öncelikle şifrelenmeme konusuna değinelim. Bildiğiniz gibi, FTP protokolü şifresiz bir protokoldür, yani gönderdiğiniz dosyalar sizin bilgisayarınızla sunucu arasındaki herhangi bir noktada erişilebilir. Daha da ötesi, FTP şifreniz de `plain text` (Düz yazı) olarak gönderilir. Bu bilgiler başka birinin eline geçerse uygulamanız çok büyük zarar görebilir. Ayrıca, FTP üzerinde direkt düzenleme yaparsanız uygulamanız versiyon kontrolü bozacağı için (bu aşamada versiyon control FTP üzerinde çalışan kodlarda değişiklik yapıldığını algılayacaktır) kesinlikle önerilmemektedir. Son olarak, FTP şifresi gibi gereksiz işlerle vakit kaybedersiniz.

Shared hosting, yapıları gereği çoğu zaman SFTP/SSH yetkisi vermezler. Bu durumda, sunucuda yapmanız gereken en temel işlemleri de yapamazsınız (örneğin bir servise restart atmak gibi). Böyle bir durumda ya o firmaya email gönderirsiniz ya da kontrol panellerinde böyle özellikler olsun diye dua edersiniz.

FTP'ye daha güvenli alternatif olarak SSH/SFTP kullanmanızı tavsiye ederim. Bu tür protokoller FTP'ye göre çok güvenlidirler. Bilgisayarınız ve hedef sunucu arasındaki veri akışı şifrelenmiş bir protokol üzerinden yapılacağı için asla izlenemezler. Ayrıca, parmak izini andıran bir şekilde çalıştıkları için (şifre de kullanabilirsiniz dilerseniz ama ne gereği var?) sunucu sizin bilgisayarınızı tanıyacaktır. Çoğu zaman sadece `ssh sunucu` yazarak giriş yapabilirsiniz.

Bu durumda, isterseniz kullandığınız FTP uygulamasını (FileZilla gibi) SFTP üzerinden bağlatıp aynı işlemleri gerçekleştirebilirsiniz veya terminal üzerinden SSH bağlantısı gerçekleştirip, sunucu tarafında `git pull` komutunu çalıştırabilirsiniz. Size kalmış... Ama bu işlemleri otomatik olarak yaptıramaz mıyız? Tabi ki yaptırırız. Yazının başından beri hep ne diyoruz? Bilgisayarın yapabileceği işlerle sen neden uğraşasın ki? Şansızlıyız, deployment için de işimizi kolaylaştıracak araçlar mevcut.

### DeployHQ ve türevi servisler ###

Deployment işlemi için kullanabileceğiniz bazı 3. parti servisler bulunmaktadır. Bu tür servisler genellikle aylık bir ücret karşılığı çalışırlar. Bu servislere örnek olarak DeployHQ ve Dploy.io verilebilir.

Peki ne işe yarar bu servisler? Aslında çalışma mantıkları çok basit. Senin FTP'den yükleyeceğin dosyaları, onlar analiz edip otomatik olarak yükler. Bunu da senin versiyon kontrol repository'ne okuma yetkisiyle bağlanarak yaparlar. Sen versiyon kontrol uygulamanla bir commit'i repo üzerine push'ladığında, otomatik olarak değişen dosyalar sunucuna aktarılır. Böyle bir kullamın birçok avantajı var, mesela:

1. İki iş yapmış olmazsın.
2. Deployment öncesi için komut girebilirsin (örn, bakım modunu aç).
3. Deployment sonrası için komut girebilirsin (veritabanını güncelle, composer update komutunu çalışır, bakım modunu kapat gibi).
4. Deployment işlemini birden çok sunucuya yapabilirsin.
5. İşlemlerin durumuyla ilgili otomatik bildirimler alabilirsin.

Bu servisleri kullanmak çok basit çünkü sana bir GUI sunuyorlar ve sen işlemlerini bir websitesi kullanırmış gibi yapıyorsun. Ancak aylık ücretleri olduğu için bazı geliştiriciler tarafından tercih edilmeyebilir. Bu durumda, yardımımıza Capistrano koşuyor. Capistrano, bu tür servislerin bize sağladığı özellikleri (ve daha fazlasını) bize sağlayan yardımcı bir araç. Ne olduğunu bir sonraki bölümde öğrenelim.

### Capistrano ###

Capistrano'nun ne olduğunu, sitesindeki kısa açıklama ile özetlemeye çalışalım: "A remote server automation and deployment tool written in Ruby." yani Türkçesiyle, "Capistrano, Ruby ile yazılmış  bir sunucu otomasyon ve deployment aracıdır." (Yeri gelmişken, şu deployment lafını Türkçe'ye birtürlü çeviremedim, böyle idare edin).

Çalışma mantığı basit, ancak DeployHQ gibi bir servisi kullanmak kadar kolay değil. Capistrano ile scriptlerinizi kendiniz yazacaksınız. Şimdi biraz flashback yapalım, `DRY` ile ilgili makalede ne demiştik?

> bir komut yazdığınızda hem veritabanı yedeğinin alınıp, hem csslerin optimize edilip, hem de sunucuya veritabanını yedeğinin yüklenmesini sağlamak istemez misiniz? Proje geliştirirken en çok nelere vakit harcadığınızı düşünün ve bilgisayarın yapabileceği her şeyi bilgisayara yaptırın.

İşte orada kastettiğim araçlardan biri `Capistrano`'ydu. Capistrano'ya [https://github.com/capistrano/capistrano](https://github.com/capistrano/capistrano) adresinden ulaşabilirsiniz.

// Yakında buraya Capistrano ile ilgili örnekler gelecek

## Versiyon kontrol ##

// Versiyon kontrol nedir, onun tanımı yapılacak.

### Git kullanın. ###

// Git hangi konularda avantajlı SVN gibi alternatiflere göre?

### Commit mesajlarınız açıklayıcı olsun. ###

// Niye?

## Hosting ve Sunucu ##

### Shared hostingleri unutun. ###

// Shared hosting bad practice kullanımları - FTP gibi ilkel yöntemler anlatılacak

### PaaS? ###

// Fortrabbit - Appfog - Heroku gibi servislerin ve avantajları

### Cloud sunucular ve provisionerlar ###

// Laravel Forge - Serverpilot - Aylık 5 dolara nasıl taş gibi sunuculara sahip olabileceğiniz

## Scalability ##

// Uygulama tanıtımları: Varnish - Redis - HAProxy - Resque - Iron - CDN vsvs.

> Not: Bu makaleyi vakit buldukça güncelleyeceğim.
