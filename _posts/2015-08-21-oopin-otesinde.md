---
layout: post
title: "OOP'in ötesinde..."
description: "OOP'in ötesinde..."
modified: 2015-08-21
tags: [PHP, OOP, SOLID]
image:
  feature: abstract-7.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

# OOP'in ötesinde... #

Bugünlerde birçoğumuz öğrendiği klasik OOP tekniklerini kod yazarken uyguluyor. Methodlar, sınıflar, kalıtımlar, prototipler... Bu güzel birşey! Ancak, yazdığımız kodlarda geliştirebileceğimiz bazı noktalar var.

Bazen kod yazarken aklıma bu tür sorular takılır. "Acaba bunu böyle yapsak nasıl olurdu?" sorusunu sık sık kendime sorarım ve araştırmaya başlarım. Sonuç olarak, genellikle birileri bu soruların cevabını yıllar öncesinden vermiş olur. Bu yazımda, bu tür kullanımların size gerçek dünya örnekleriyle neden önemli olduklarını anlatmaya çalışacağım.

İşin mühendislik kısmını yapmak sıkıcı tarafı, ama yaptığımız şeyi sanatımızla süslemek bizim elimizde.

![](https://cdn.tutsplus.com/net/authors/jeffreyway/1278414923_coraline-vs-puppeteer.gif)

Başlayalım!

### Value Objects (Değer Objeleri) ###

Klasik OOP yaklaşımıyla kod yazarken, genellikle bir veri bir methoda gidiyor, o methodda işlenip bir primitive olarak geri dönüyor, dönen primitive kontrollerden geçiyor ve başka bir sınıfa gidiyor, o sınıf o primitive değerini tekrar işliyor ve başka bir primitive döndürüyor.

> Boolean, integer, string, object, array, null, void gibi değerler genel olarak Primitive adıyla isimlendirilmiştir. Yukarıdaki örnekte, bir methoda string (primitive) gönderip, boolean (primitive) döndürülmesini hayal edebilirsiniz.

Kısacası, klasik yaklaşımla kendi başına bir anlamı olmayan anlamsız (aptal) verileri anlamlı hale getirmek için, akıllı bir sınıf oluşturma ihtiyacı duyuyoruz. Bu bana göre çok mantıklıca bir yöntem değil.

Bir düşünün, yolda yürürken yerden bir elma bulduğumuzu farzedelim. Biz o elmayı elimize aldığımızda "Bu bir elmadır." diyebiliyoruz, ancak kod yazdığımızda ise, "Bu [primitiveyi] bir elma analiz fabrikasına (sınıfına) sok. Eğer gerçekten bir "Elma" türünde bir primitive ise, bana doğru veya yanlış değerini döndür. Eğer obje analiz edilemezse "BilinmeyenObje" exceptionu fırlat ve evrenin yok olmasını önle." tarzında kod yazıyoruz.

Elma, kendisinin bir elma olduğunu neden bilemiyor? Biz neden onun elma olduğunu bilemiyoruz? Yazdığımız kodlarda neden en çekirdek objelerimiz bu kadar aptal oluyor?

Value Objects bu soruna çözüm getirmek için düşünülmüş bir konsept. Verilerimizi daima objeler olarak tutup, karşılaştırmalarımızı ise kimlikleri (instanceleri) yerine değerleri ile yapıyoruz.

> Bu yüzden "Değer Objeleri" deniyor.

Daha iyi anlamak için, aşağıdaki örneği ele alalım.

    "http://aristona.github.io" (string)
    "deneme@deneme.com" (string)
    [ (array)
        15, (integer)
        "$" (string)
    ]

Yukarıdaki verilerin tek başlarına hiçbir anlamı yok. Ancak biz bunları kullanırken kendi türlerine çevirip kullanabiliriz.

    ValueObjects\URL (object)
    ValueObjects\Email (object)
    ValueObjects\Money (object)

Karşılaştırmalarımızı da, bunların "değerleri" ile yapabiliriz. Örneğin:

    $a = new ValueObjects\Email("deneme@deneme.com");
    $b = new ValueObjects\Email("deneme@deneme.com");
    $a == $b // true

> Yukarıdaki örnekte __toString() methodu içerisinde email propertysi return edilmekte.

Bu şekilde para değerlerini de karşılaştırabiliriz.

    $usd = new Money(new USD(1.50));
    $try = new Money(new TRY(15.00));

    $try > $usd // true

> Yukarıdaki örnekte __toString() methodu içerisinde, "Para" objesinin değeri döndürülmekte. USD, neredeyse TRY'nin 3 katı an itibariyle, ancak aradaki fark çok fazla olduğu için $try değeri daha yüksek.

    $a = new ValueObjects\URL("http://deneme.com");
    $b = new ValueObjects\URL("ftp://deneme.com");
    $a->value() === $b->value() // false (urlleri olduğu gibi kontrol ettik)
    $a->domain() === $b->domain() // true (domainleri kontrol ettik)

Bu yöntemin iyi yanı, uygulamamızın heryerinde aynı objeleri kullanabilmemiz, ve gerektiğinde objelerimizi akıllı hale getirebilmemiz.

    // Değer objemizi oluşturalım.
    $value = new Money(new USD(1.50))->value();

    // Veritabanına kaydedelim.
    $money = new App\Money;
        $money->amount = $value;
    $money->save();

Veritabanından veriyi çektiğimizde ise, çektiğimizde o veriyi `Money` türüne çevirerek kullanmamız gerekiyor. Bunun için pseudo olarak bir accessor yazdığımızı farzedelim.

    // Veritabanından parayı çekelim.
    $money = App\Money::first();

    // Pseudo olarak bir accessor yazdığımızı farzedelim.
    public function getAmountAttribute($attr)
    {
         return new ValueObjects\Money($attr);
    }

    // O veriye erişelim.
    $money->amount // ValueObjects\Money (object)

    // Başka bir türe çevirelim.
    $money->amount->convert(function ($converter) {
        return $converter->toUSD();
    });

> Single Reponsibility Principle bize sınıfın sadece tek bir işi olması gerektiğini söyler. Bu yüzden, "Para" yı başka bir sınıfta, "Converter" i ise başka bir sınıfta tuttum, ve convert lambdası içerisine apply ettim. Sınıf oluşturmaktan asla korkmayın.

Daha da ileri gidelim. Paramızı USD'ye çevirdikten sonra biraz TRY ekleyelim.

    $money->amount->convert(function ($converter) {
        return $converter->toUSD();
    })->add(function () {
        return new ValueObjects\Money(new TRY(15.00));
    });

Dikkat etmeniz gereken nokta şu. Aslında biz buraya TRY eklemiyoruz, TRY'nin "Para" dediğimiz objedeki değerini ekliyoruz. TRY'nin değeri 1.00 olabilir, USD 3.00 olabilir, Zimbabwe doları 0.0001 olabilir, ama hepsinin bir değeri var ve hepsi bir "Para" objesi. 3.00 değerinin kendisinin bir anlamı yok (aptal), ama Para(USD(3.00))'ın birçok anlamı var.

Bunu yaptığımızda, uygulamamızda akıllı objeleri kullanmaya başlıyoruz, ve o objeleri istediğimiz kadar akıllı yapmak bizim elimizde.

Örneğin, elimizdeki para değerini, o anki kur değerinde en çok kazanç sağlayabileceğimiz para cinsine dönüştürebiliriz. Bunun mantığını "Converter" sınıfımıza yazabiliriz.

    $money->amount->convert(function ($converter) {
        return $converter->highest();
        // en çok para kazandıracak para birimini return et
    });

Veya, o anki para biriminin sembolünü aldırabiliriz.

    $money->amount->convert(function ($converter) {
        return $converter->toUSD();
    })->format(function ($signature, $value) {
        return sprintf("{$signature} {$value} param var");
    });
    // $ 1.50 param var

Uygulamamızın geri kalanında da, bu objeleri typehintler ile kullanabiliriz. Bir "Ürün" objesi oluşturalım, ve buna fiyat olarak "Para" değer objesini gönderelim.

    <?php

    use ValueObjects\Money;

    class Product
    {
        protected $price;

        public function __construct(Money $money)
        {
            $this->price = $money;
        }

        public function price()
        {
            return $this->price;
        }
    }

    ---

    $product = new Product(new Money(new USD(15.00)));
    $product ->price // (object) ValueObjects\Money

    $price = $product ->price->convert(function ($converter) {
        return $converter->toUSD();
    });

    $price->show(); // $ 15.00

Aynı şekilde, kullanıcımızın da sahip olduğu "Para" değerini tutup, "değerleri" kontrol ettirebiliriz. Hatta ileri gidip, `affords` adında bir method oluşturalım.

    $user = User::current();
    $user->funds // (object) ValueObjects\Money

    $product = new Product(new Money(new USD(15.00)));
    $product->price // (object) ValueObjects\Money

    $user->affords($product, function ($user, $product) {
        echo sprintf("%s can afford %s!", $user->name, $product->name);
    });

    // Jane can afford Green Shoe.

İşte bu kadar! Artık ürün fiyatını 15.00 gibi hardcoded tutmak yerine, kendi içerisinde bir anlamı olan "Para" değer objesi olarak tutuyoruz. Bu objemiz, başka sınıflara bağlı kalmadan, kendi değerini hesaplayacak kadar akıllı. Başka para türleriyle kendini karşılaştıracak kadar akıllı. Kendini başka türlere çevirecek kadar akıllı.

En başa dönelim. Yerde bir Elma bulmuştuk. Biz onun Elma olduğunu nasıl biliyoruz? Çünkü o bir Elma değil, o bir ValueObjects\Elma. :) Geriye, onu klasik OOP ile dekorate etmek kalıyor.

> ValueObjects konusu bu kadar, ama biz aşağıda biraz çılgınlık yapacağız. Mühendislik burada bitiyor, artık sanat başlayacak. Artık parmaklarımızdan sıkıcı mühendislik konseptleri yerine hayalgücümüz akacak!

Hey sen, `ValueObjects\Elma`. Çok tatlı görünüyorsun. Seni yemem lazım!

Elma bir Meyve'dir, Meyve bir yiyecektir. Öyleyse inheritencemizi bu şekilde kurgulayabiliriz. (OOP 101, yay!)

    <?php namespace ValueObjects;

    class Elma extends Meyve extends Yiyecek
    {

    }

Aslında inheritence daha düzgün kurgulanabilir (Böceklerin yediği şeyler de yiyecektir, ama insanlar onları yemez. Aynı şekilde ben bir böceğin tarhana çorbası içtiğini görmedim, ya siz?) ama konuyu saptırmayalım şimdi. Amacımız o elmayı yemek!

...ama yemeden önce onu yıkayalım. Değil mi?

    $musluk = new Musluk;
    $musluk->yıka($elma);

    $ben->ye($elma); // Yummy (true)
    $ben->ye($çürükElma); // Iyy, ben bunu yemem (false)

    // Aslında, bizler de birer ValueObject'iz evrende...
    $ben // (object) ValueObjects\Creatures\HomoSapiens
    $ben->alive // true
    $ben->hungry? // true
    $ben->ye(YiyecekInterface $yiyecek);

Bir dakika... herkes neden durdu? Araçlar neden ilerlemiyor? Kaldırımda yürüyenler neden dondu? Çünkü;

    $musluk->yıka($elma);

Bu işlem 5-10 saniye sürüyor ve o esnada sistem blocklanıyor. Elma yiyeceğiz diye bütün evreni dondurduk. Onu asenkron yapalım. Elma yeterince yıkandığında `yeterinceYikandi` callbackini çalıştırsın.

Bu esnada dikkatli olmamız lazım. Evreni blocklamayacağız diye sonsuza kadar yıkamayalım elmayı. (Paralel programlama zor zanaat...)

    $musluk->yıka($elma)->yeterinceYikandi(function ($ben, $elma)) {
        $ben->ye($elma);
    });

Daha güzel. Ama Elma'ya ne oldu? Elma öldü :( Belki ölmeden önce son sözleri vardır, ama biz onları duyamadık.

İyi de, İnsan ne duyar? Ses. Sesi ne çıkarır? Elmanın ses çıkaracak bir özelliği yokki. Ağzı yok, ses telleri yok... ama onun hisleri var, ben inanıyorum!  Keşke zamanında bir `Observer` ekleseydik Elma objemize. Zamanı geri alalım, ve ekleyelim.

![](http://aristona.github.io/images/backtothefuture.png)

    $universe->pause()->setDate(function ($date)) {
        // 1 saat yeter!
        return $date->subHours(1);
    })->continue();

    // Ah, ne güzel bir ValueObjects\Elma buldum yerde?

`Observer`lar, adından da anlaşılacağı üzere `İzleyici`lerdir. Bir objeyi izlerler. Objede herhangi bir değişiklik olduğunda bu sınıflar çalışmaya başlar. Bu sınıfların amacı tamamen hedef objeyi izlemekle alakalıdır. 

Aslında Observer'leri insan beynine benzetebilirsiniz. Beyin midemizi izliyor, ve beynimiz bize "Bak acıkıyorsun. Midedeki yiyecek propertysi gittikçe azalıyor." diye sinyal gönderiyor, ancak bizi zorla sofraya oturtup yemek yedirtmiyor, sadece izliyor ve uyarıyor. (Biyoloji bilgim bu kadar malesef.)

Biz de, Elma objemize bir izleyici sınıfı ekleyebiliriz. Onun hislerini hook edip onları dış evrene taşıyabiliriz. Burada tüm implementasyonu yazmayacağım, ama aşağıdaki örneği kafanızda canlandırın.

     $elma = new İzleyici(new ValueObjects\Elma));

Veya onu Trait olarak ta ekleyebiliriz. Traitlerin kullanım amaçları bu, ama ben yukarıdaki yöntemi tercih ediyorum.

     class Elma
     {
          use İzleyici;
     }

İzleyici sınıfımızda, bazı eventler olduğunu farzedelim.

     class İzleyici
     {
         public function created($obje)
         {
             // Obje yaratıldı
             // Evrenin loglarına "{obje} yaratıldı" yaz
         }

         public function onÖlümNotuChanged($obje)
         {
             // Eğer değişen property "$ölümNotu" ise
             // Ekrana "Ölüm notu değişti. Ölüm notu: {$this->ölümNotu} yaz"
         }

         public function died($obje)
         {
             // Obje öldü
             // Evrenin loglarına objenin son sözlerini yaz
         }
     }

Bu durumda:

    $elma = new İzleyici(new ValueObjects\Elma));

dediğimde, evrenin loglarına şu bilgiler gelecek.

    [Evren][Log] {Elma} yaratıldı.
    [Evren][Log] Ölüm notu değişti. Ölüm notu:
        "Beni yiyen insan 2 dakika içinde ölecektir."

çünkü ne zaman Elma objesinde bir değişiklik olsa, Observer sınıfı çalışacak. Observerlerin işi bu.

Zamanı geri almıştık. Şimdi tekrar o Elma'yı yiyelim.

    $musluk->yıka($elma)->yeterinceYikandi(function ($ben, $elma)) {
        $ben->ye($elma);
    });

    // ye methodu
    public function ye(YiyecekInterface $yiyecek)
    {
         // Yiyeceği mideye indir.
         // ...

         // Yiyeceği yoket.
         $yiyecek->die();
    }

    // yiyecek->die() methodu
    public function die()
    {
         // Güle güle zalim dünya!
         // Sesim çıkmıyor ama en azından evren
         // loglarımı tuttu!

         return;
    }

Sonunda, zor oldu ama yiyebildik elmayı.

    [Evren][Log] {Elma} öldü. Son propertyleri:
         {object}.ölümNotu: "Beni yiyen insan 2 dakika içinde ölecektir."

Bak sen şu elmaya, blöfte yaparmış. O kadar küçük bir elma nasıl bir insanı (pardon ValueObjects\HomoSapiens'i) öldürebilir ki, bunun bir mantığı yok.

    [Evren][Log] {İnsan} objesinin kalp atışları yavaşlıyor.

Nasıl yani? Bende de mi Observer vardı? Baştan beri izleniyormuydum????

    [Evren][Log] {İnsan} objesi ölmek üzere.

Lanet olsun, vaktim gerçekten azalıyor. En iyisi ölüm notumu yazayım!

    $ben->ölümNotu("Lanet olası elma!");

    [Evren][Log] Ölüm notu değişti. Ölüm notu:
        "Lanet olası elma!"

     ...
     ...
     ...
    [Evren][Log] {İnsan} öldü. Son propertyleri:
         {object}.ölümNotu: "Lanet olası elma!"
         {object}.sonYediği: {ValueObjects\Elma}

Hikayemiz mutlu sonla bitmedi, ama birçok şey öğrenmiş olduk. Nasıl akıllıca bir OOP modeli kurabileceğimizi, değer objelerini ve observerleri hangi amaçlarla kullanabileceğimizi, gerçek dünyadaki olayları bile OOP ile nasıl anlamlı hale getirebileceğimizi.

Ama benim aklımda hala böceğin tarhana çorbası içip içmeyeği var. İçer mi? İçerse bizim gibi kaşık kullanarak mı içer? Yoksa inheritencemizi ve interfacelerimizi değiştirmek zorunda mıyız? Sizce?

// Functional Programming Paradigms

// Reflective

// ADR

// Domain Driven Design

// AOP

// Event Driven Design

// Testability

// OOP'in az kullanılan yönleri