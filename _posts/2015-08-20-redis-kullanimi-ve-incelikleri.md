---
layout: post
title: "Redis kullanımı ve incelikleri"
description: "Redis kullanımı ve incelikleri"
modified: 2015-08-20
tags: [PHP]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

#Redis kullanımı ve incelikleri#

Uzun zamandır Redis hakkında bir yazı yazmak istiyordum çünkü Redis son zamanlarda öğrendiğim en yararlı araçlardan birisi. Fazla Türkçe kaynak bulamadım bu yüzden Redis'i yine aynı tarzda, yeni başlayanların da anlayabileceği bir şekilde anlatmayı planlıyorum.

###Redis nedir?###

Redis; no-sql veritabanı olarak, önbellekleme için ve mesaj sunucusu amacıyla kullanılan, in-memory bir veri-yapıları deposudur. Redis'in kendisini veri-yapıları deposu olarak adlandırmasındaki sebep diğer alternatiflerine göre daha fazla veri türünü desteklemesidir.

###Avantajları###

- Senkron çalıştığı için son derece hızlıdır.
- Birçok veri türünü destekler.
- Veriyi hem RAM üzerine hem de ayarlandığınız konfigürasyona göre disk üzerine kaydedebilir.
- Disk üzerine kayıt yaptığı için restart sonrasında aynı verilerle çalışmaya devam eder.
- Son derece aktif bir kullanıcı kitlesine sahiptir.
- Sharding, Cluster, Sentinel, Replication gibi birçok enterprise özelliklere sahiptir.

###Dezavantajları###

- Asenkron çalışmadığı için tek instance üzerinde, asenkron alternatiflerin eriştiği performansa erişemeyebilirsiniz.
- Veri boyutunuza göre RAM'e ihtiyacınız olur.
- Relational veritabanlarında olduğu gibi komplex sorguları desteklemez.
- Komplex sorgular için sorgu yapacaksanız, Redis yapısını düzgün kurgulamalısınız.
- Bir transaction hata alırsa geri dönüşü yoktur.

###Ne kadar hızlı?###

İnternette Redis için yapılmış birçok benchmark bulabilirsiniz. Kendi bilgisayarımda denediğimde saniyede 100.000 SET/GET, pipe özelliğini açtığımda saniyede 400.000 SET/GET komutu desteklediğini görmüştüm. Ancak göz önünde bulundurmamız gereken birkaç nokta var. Bu sadece bir tane Redis instancesinin değerleri. Birden fazla Redis instancesi oluşturup her birini işlemcinizin bir çekirdeğine ve farklı bir porta atayabilirsiniz. Eğer bu yetmiyorsa kolayca bir master-slave replikasyonu oluşturabilirsiniz. Bu da yetmiyorsa bir Redis cluster kullanabilirsiniz. Bu noktaya geleceğinizi hiç sanmıyorum ama bu da yetmiyorsa, consistent hashing özelliklerini kullanarak verilerinizi birden fazla parçaya bölüp shardlar veya partitionlar üzerinde tutabilirsiniz.

###Kısaca: Desteklenen veri türleri###

Redis `STRING`, `HASH`, `SET`, `SORTED SET` ve `LIST` gibi veri türlerini destekler.

###Özellikler###

Yukarıda yazdıklarım benim için çok sıkıcıydı. Eminim sizin için de sıkıcı gelmiştir. Gereksiz terimler, anlaması güç kelimeler... Merak etmeyin, bundan sonra herşeyi "Anneye anlatır gibi", basit, kısa ve anlaşılabilir şekilde tutmaya çalışacağım.

**Pipelining**

Bir restorantta olduğunuzu hayal edin. Garsonluk yapıyorsunuz. Kocaman bir masanız var ve 1000 kişi aynı masaya rezervasyon yaptırmış. Müşterilerinizin herbirini siparişini veriyor. "Ben tavuk sote istiyorum." "Ben köfte istiyorum." Hepsini bir kağıda not edip mutfağın yolunu tutuyorsunuz. Redis asenkron çalışmadığı için tek garson var o da sizsiniz.

1000 kişiye yemeklerini en hızlı nasıl servis edersiniz? Mutfakta bir tane yemek servis arabasına koyabildiğiniz kadar yemek koyar, onu masaya iterek götürür, hepsini dağıtır ve geri gelirsiniz. Her defasında bir tane tabağı elinizde taşıyıp, masaya bırakıp, yenisini almak için mutfağa geri dönmezsiniz. Değil mi?

Pipeling tamamen bu işe yarar. Redis komutlarını pipe içerisinde biriktir ve tek seferde hepsini çalıştırırsınız.

Aşağıdaki örneği ele alalım.

    $siparisler = ["Köfte", "Tavuk Sote", ...];

    foreach ($siparisler as $siparis) {
        $redis->get("siparis:{$siparis}");
    }

Şuan pipeline kullanmadığımız için, bu kod parçacığı her defasında Redis'e bağlanmak zorunda. Yaptığı işi şu şekilde hayal edebilirsiniz:

    Redis'e bağlan -> 1. siparişin değerini al -> PHP'ye geri dön ->
    Redis'e bağlan -> 2. siparişin değerini al -> PHP'ye geri dön ->
    ...

Redis'e bağlanmak ve PHP'ye dönmek arasında geçen süreye round trip deniyor. Yani garsonumuz, yemeği eline hemen alabiliyor, masaya da bırakabiliyor, ama mutfaktan masaya kadar gitmek ve geri dönmek her defasında biraz vakit alıyor. Pipeline kullansak garsona bütün yemekleri tek seferde yükler ve öyle gönderirdik. Masaya vardığında yemekleri dağıtırdı. Böylece mutfak ve masa arasındaki mesafe sadece bir defa kat edilirdi. Buradan çok büyük bir performans artışı sağlardık.

Yazacağımız kod aşağı yukarı şuna benzerdi. (Örnek hangi Redis client kütüphanesini kullandığınıza göre farklılık gösterebilir ama PHP için en kapsamlı kütüphane Predis ve ben örneklerimi onun üzerinden vereceğim.)

    $siparisler = ["Köfte", "Tavuk Sote", ...];

    $redis->pipeline(function (Redis $redis) use (&$siparisler) {
        foreach ($siparisler as $siparis) {
            $redis->get("siparis:{$siparis}");
        }
    });

Bu örnek komutları pipeline içine doldurur, Redis'e bunu tek seferde işlettirir ve hepsinin cevabını tek seferde döndürürdü. İlk başta pipeline 400.000 IOPS (saniye başına istek) desteklerken, pipeline olmadığında bu sayının 100.000 IOPS'a düştüğünü belirtmiştim. Uygulamanızı geliştirirken bazı kod parçalarının birden fazla Redis komutu çalıştırdığını görüyorsanız kendinize "Bunu pipeline içerisinde kullanabilir miyim?" diye sorun.

**LUA Desteği**

Redis çok akıllı değil. Ona relational veritabanlarında olduğu gibi, "Mutfağa git. Müşterilerin 3 gün önce, saat 08:00 ile 12:00 arasında verdikleri siparişler arasından çorba türünde olanları grupla ve bana çorba başına ne kadar kazanç sağladığımı göster." gibi komplex sorgular soramazsınız. Ona sorabileceğiniz şeyler hep basit olmak zorundadır.

- Bana müşterileri ver.
- Bana bu müşterilerin verdiği siparişleri ver.
- Bana bu siparişler arasından çorba olanları ver.
- Bana bu çorbalardan edindiğim kazanç bilgilerini ver.

Bu tür basit scriptleri LUA ile yapabilirsiniz. Bu genellikle verileri alıp kullandığınız programlama diliyle yapmaktan daha hızlı olmaktadır ancak LUA bilmiyorsanız verileri çekip kendi programlama dilinizde oynayabilirsiniz.

***Son derece önemli bir complexity notu...***

Bunu üstüne basarak, altını çizerek, görebileceğiniz en parlak renk ve en büyük puntolarla yazdığımı farzedin.

> Redis'i içine koyduğunuz verileri "en düşük time complexity" ile nasıl okurum diye düşünerek kurgulamalısınız.

Bu son derece önemlidir. Yoksa verileriniz büyüdükçe time complexity artar ve Redis çok daha uzun sürelerle çalışır.

Öncelikle size time complexitynin ne olduğundan bahsedeyim. Bilgisayar Bilimi (ülkemizde var mı bilmiyorum, Bilgisayar Mühendisliği veri yapıları dersinde işleniyordur belki) eğitimi alanlar bunları ders olarak görüyorlar ama kendi kendine programlamayı öğrenen insanlar genellikle bunu bilmiyor.

Restorant örneğimize geri dönelim. Farzedelim ki müşterilerimizin hepsi diyet yapıyor ama hepsinin canı specialimiz olan en ağır kalorili yemeği denemek istiyor. Ancak, masadaki diğer müşterilerden çekindikleri için bunu garsona sesli olarak söyleyemiyorlar. Bir karar alıyorlar: herkes yemek istediği yemeği oturduğu sandalyenin numarasıyla beraber bir kağıda yazacak ve bunu kapatıp bir sepetin içine atacak. (Oy kullanır gibi.) Hemen herkes kağıtlarını sepete atıyor ve garson sepeti alıp mutfağa gidiyor.

Aşçımız önce garsona kızıyor çünkü şimdi tek tek bütün kağıtları sepetten çıkarıp okumak zorunda ancak garsonun yapabileceği birşey yok. Sistem bu şekilde `kurgulandı`. Ama iş keşke bununla bitse. Aşçı'ya yardım eden bir mutfak görevlisi, 500 numaralı sandalyede oturan kadını tanıdığını, onun rahatsızlığı olduğunu ve eğer şekerli bir yemek sipariş etmişse şeker yerine tatlandırıcı kullanması gerektiğini söylüyor. Garsonumuzun onun kim olduğunu ve ne istediğini bilmiyor. Elinde kağıtların olduğu bir sepet var...

Bu durumda yapabileceğimiz tek şey sepetten kağıtları tek tek çekip 500. sandalyenin kağıdı bulunana kadar hepsini tek tek okumak olacaktır.

Sepetten çektiğimiz kaçıncı kağıt 500. sandalyeye ait olabilir? Birinci - olabilir. Sonuncu - olabilir. Peki müşteri sayısı kadar çekim yaparsak kesinlikle o kadınının kağıdını da çekmiş oluruz diyebilir miyiz? Evet.

Buna bilgisayar biliminde time complexity denmektedir. Burada müşteri numarası dinamik bir sayı olduğu için buna genelde `n` denir ve complexitysi `o(n)` olarak gösterilir. Peki `n` değeri nedir? Müşteri sayısı. 2 tane müşterimiz olsa, 2 tane kağıt olacak ve 2 tane çekim yapsak bütün bilgilere ulaşmış olacağız. Çok zor değil. Peki, 100.000.000 tane müşterimiz olsa? O zaman 100.000.000 tane çekim yapmamız gerekebilir. Muhtemelen garson çekimleri bitirene kadar restorant iflas etmiş olur.

> Bu durumda Redis'e kızamazsınız! Oraya o kağıtları siz gelişigüzel attınız. Kurgunuz bu. 1-100 arasını bir sepete, 101-200 arasını diğer sepete, 201-300 arasını başka bir sepete atabilirdiniz. Bu durumda sadece 401-500 sepetini baktığınızda 500. sandalyede oturan kadının yemek bilgilerine daha çabuk ulaşabilirdiniz çünkü 100 tane kağıt var. "Verileri nasıl okuyacaksanız Redis'i ona göre kurgulayın" diyerek kastettiğim şey bu.

Time Complexity (veya sadece complexity, ikisi aynı şey) daima mümkün olan `en kötü ihtimal` üzerinden hesaplanırlar. Eğer 2 tane müşterin varsa:

    o(n) === o(müşteri sayısı) === o(2)

Yani complexity 2'dir. Yani Redis senin istediğin veriyi, "en kötü ihtimalle" 2 adımda bulacaktır.

Eğer 100.000.000 tane müşterin varsa:

    o(n) === o(müşteri sayısı) === o(100.000.000)

Burada complexity 100.000.000 çıkacaktır ve bu çok yüksek bir rakam.

Peki 2. bir sepet olsaydı ve istedikleri `tatlıları`'da aynı mantıkla o sepete atsalardı? Bu durumda yazacağın koda göre complexity çok fazla olabilirdi.

    // Yemek sepetinden bir kağıt çek. Çektiğin kağıttaki sandalye sayısını, Tatlı sepetinde bulmaya çalış.

Kaç farklı kombinasyon var? 100.000.000 tane müşterimiz olduğunu varsayarsak:

    o(n * n)
    = o(yemek sepetindeki kağıt sayısı ^ tatlı sepetindeki kağıt sayısı)
    = (100.000.000 * 100.000.000)
    = o(100.000.000.000.000.000)

Bu sayının nasıl söylendiğini bile bilmiyorum. Peki biz bunu daha az complexity ile nasıl tutabilirdik? Müşterilere sepete kağıt atmalarını söylemek yerine, 2 dakika izin isteyebilir ve yazıcıdan telefon rehberine benzer ufak bir kağıt çıkartabilirdik. (Veri yapılarında bu `HASH` diye geçer)

Elinizde şöyle bir kağıt parçası olduğunu düşünün:

    Masa numarası --- İstediği Yemek --- İstediği Tatlı
    1                 [             ]    [            ]
    2                 [             ]    [            ]
    ...

Garson herkesin istediği yemek ve tatlıyı boş alanlara yazdırıp mutfağa öyle gidebilir. Bu durumda 500. sandalyenin hangi yemeği ve hangi tatlıyı istediğini `o(1)` şekilde görebilirsiniz.

> o(1) complexity daima en hızlı complexity değeridir.

Bu yapının bize çıkaracağı zorluklar yok mu? Var. Patronumuz mutfağa gelip bize şunu sorabilirdi: "Bana en çok para kazandıran yemeği göster."

Yemekleri gruplayıp hepsinin kaçar defa sipariş edildiğini bulduğumuzda `o(n)` (n = rehberdeki satır sayısı) kadar complexity olurdu çünkü her satırı tek tek okuyup, hangi yemeğin sipariş edildiğini bir artıracağız. Daha sonra yemeklerin fiyatını çektirecektik. Bunun da complexitysi `o(n)` (n = sipariş edilen yemek türü sayısı) olacaktı çünkü herkes aynı yemeği sipariş etmiş olabilir. Herkes farklı yemeği de sipariş etmiş olabilir. Biz her zaman en kötü ihtimalle gideceğiz. Son complexity şuna benzeyecekti.

    o(N + S)
    N = Müşteri sayısı
    S = Müşterilerin sipariş ettiği yemek türü sayısı)

500 müşteri 100 farklı yemek söyleseydi: `600`, hepsi aynı yemeği söyleseydi `501` complexity olacaktı. (500 müşteri ve 1 yemek)

Bunu yapmak zorunda mıyız? Hayır. Şöyle yapsaydık:

    // Bu kağıt aşçı için [HASH]
    Masa numarası --- İstediği Yemek --- İstediği Tatlı
    1                 [             ]    [            ]
    2                 [             ]    [            ]
    ...

    // Bu kağıt patron için [HASH]
    Sipariş Edilen Yemek --- Sipariş Sayısı
    [                  ]     [            ]
    [                  ]     [            ]
    ...

Kadınlar siparişlerini yazdıklarında istedikleri yemeği başka bir kağıta not edebilirdik ve o yemeğin sipariş sayısını 1 artırabilirdik.

    Sipariş Edilen Yemek --- Sipariş Sayısı --- Birim Fiyat
    [Köfte             ]     [13          ]     [10       ]
    [Çorba             ]     [2           ]     [3        ]
    ...

Patronumuz bize "Bana en çok para kazandıran yemeği göster." bunu `o(n)` n = sipariş edilen yemek türü sayısı değerine indirebilirdik. Yazarken biraz yorulurduk ama Redis kurgusunu daima `okurken nasıl hızlı okurum` sorusunu düşünerek yapmalısınız.

Peki bu complexityi daha da indiremez miyiz? İndiririz! Redis'in desteklediği `SORTED SET` (sıralanmış küme) veri türünü kullanabiliriz. Bunlar bir skora göre (skor, yemeğin birim fiyatı olabilir) sıralanacağı için; Redis'e "Bana sıraladığın kümenin 1. elemanını döndür" dediğimizde daha düşük bir complexity ile bu sonuca ulaşabilirdik.

> Her Redis komutunun time complexity değerleri http://redis.io/commands adresinde yazmaktadır.

Aslında Redis'in sevdiğim bir yanı da bu. Beni düşünmeye ve veri yapılarının time complexitylerini hesaplamaya itiyor. Geliştirdiğim algoritmaları zamanla daha da hızlandıracak yöntemler buluyorum ve bu çok işime yarıyor.

Complexity hesaplama aslında önemli bir konu. Birçok büyük firma yazılımcı alırken mülakatlarında genellikle bu tür soruları soruyorlar. Time complexity (https://en.wikipedia.org/wiki/Time_complexity) Big-o Notation (https://tr.wikipedia.org/wiki/B%C3%BCy%C3%BCk_O_g%C3%B6sterimi) gibi sorular daima karşınıza çıkar. Öğrenin, uygulayın, ne kadar düşük complexity ile çözüme ulaşabiliyorsunuz görün. :)

**Veritabanı desteği**

@todo

**Redis Cluster ve Sentinel**

@todo

**Master-Slave Replikasyonu**

@todo

**Partitioning**

@todo

**Mass Insert**

@todo

**Expiration**

@todo

**Redundancy & Persistence**

@todo

**Failover, Quorum Anlaşmaları ve Master Seçimleri**

@todo

**Konfigürasyon Desteği**

@todo

**PUB/SUB**

@todo

**Konfigürasyon Desteği**

@todo

**Yardımcı Araçlar**

- Redsmin
- Twemproxy
- try.redis.io
- redis-cli

**Sonuç**

Redis son derece güzel ve aktif bir proje. Büyük küçük birçok firma kullanılıyor. Alet çantanıza eklemeniz size çok büyük bir avantaj sağlayabilir.

Merak ettiğiniz soruları yorum yaparak sorabilirsiniz.

Bir sonraki yazımda görüşmek üzere!

Ps. Yazımda hatalar veya devrik cümleler olabilir. Üzerinden geçtiğim zamanlarda gözüme çarpan hataları düzeltiyorum. Hata görürseniz yorum olarak bana bildirebilir veya yazılarım açık kaynaklı olduğu için Github üzerinden Pull Request gönderebilirsiniz.