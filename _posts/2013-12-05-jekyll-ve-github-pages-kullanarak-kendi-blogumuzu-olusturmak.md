---
layout: post
title: "Jekyll ve Github Pages kullanarak kendi blogumuzu oluşturmak"
description: "Jekyll ve Github Pages kullanarak kendi blogumuzu oluşturmak"
modified: 2013-12-05
tags: [Jekyll, Github, GruntJS, Git]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

# Bölüm 1: Giriş #

Merhaba,

Size bu yazımda Jekyll ve Github Pages kullanarak nasıl bir web sayfası oluşturabileceğinizi anlatacağım. Öncelikle isterseniz kısaca `Jekyll` ve `Github Pages`'in ne olduklarından bahsedelim.

**Jekyll nedir?**

`Jekyll`, kendi basit ancak güçlü bir altyapıya sahip statik site oluşturucudur. (Static site generator) Mantığı çok basittir. Siz yazılarınızı `Markdown` formatında yazarsınız ve `Jekyll` bunu statik bir web sitesine çevirir. 

**Avantajları neler?**

Birçok insan sadece blog yazılarını paylaşmak için `Wordpress` gibi alternatifleri kullanıyor ancak bir süre sonra vaktinizi blog yazıları yazmaktan çok `Wordpress`'i yönetmekle geçirdiğinizi anlıyorsunuz. Yönetmeniz ve kurmanız gereken eklentiler, yönetmeniz gereken bir MySQL veritabanı, almak zorunda olduğunuz bir hosting hizmeti, Wordpress'i güncel ve güvenli tutmak için sık sık update yapma zorunluluğu, yapmanız gereken birçok ayar ve daha birçok şey.

`Jekyll` ise kullanımı son derece basit bir uygulama. Veritabanı olmadığı için çokta performanslı. `Jekyll` kullanırken veritabanı yönetimi veya hosting (`Github Pages` sayesinde) gibi şeylerle uğraşmanız gerekmiyor. Yazılarınızı `Markdown` gibi bir formatta yazın, `jekyll build` komutunu girin ve `Jekyll` size sitenizin statik HTML çıktısını versin. Bu kadar basit.

**Github Pages nedir?**

Github, size projeleriniz için web sayfası oluşturabileceğiniz ve projenizi tanıtabileceğiniz bir hosting hizmeti veriyor. Kullanımı bedava. Birçok açık kaynaklı proje ve yazılımcı, projelerini ve kişisel sayfalarını Github pages üzerinde tutuyor.

**Peki hiç dezavantajı yok mu?**

Jekyll'in çalışması için gerekli bazı dependencyler var. İlk başta `Jekyll`'i yüklemek için `Ruby` ve `Ruby Gem` kurulmalı. `Github Pages` kullanmak istiyorsanız `Git`, syntax highlighting özelliğinden yararlanmak istiyorsanız `Python 2.7.5`, klasik işlemleri halletmek için de `GruntJS` (dolayısıyla `NodeJS` ve `Node Package Manager`) sisteminizde kurulu olmalı.

İlk etapta, "Uhh, ben en iyisi Wordpress ile devam edeyim." gibi bir düşüncede olabilirsiniz ancak bunların bilgisayarınızda bulunması daima yararınıza. (veya daha da iyisi - Vagrant kullanarak sanal sunucunuzda bulunması, ancak bu konuya daha sonraki bir yazımda değineceğim.)

`Jekyll`'in çalışmak için bazı uygulamalara ihtiyaç duyduğunu bilmeniz yeterli.

## Bölüm 2: Gerekli dependencylerin kurulması ##

Bilgisayarınıza `Ruby` ve `Ruby Gem`'i kurup gerekli ayarlamaları yapın. Daha sonra:

~~~ruby
gem install jekyll
~~~

yazarak `Jekyll` uygulamasını kurun. Aynı şekilde bahsettiğim diğer gereksinimleri de bilgisayarınıza kurun. Uygulamaların kurulum aşaması gerçekten çok kolay ve internette çok fazla döküman olduğu için burada tekrar bahsederek vakit kaybetmek istemiyorum.

## Bölüm 3: Jekyll ile ilk sitemizi oluşturalım ##

Herşey hazırsa, `Jekyll` ile ilk sitemizi oluşturalım.

~~~ruby
jekyll new site_klasoru
~~~

yazarak `Jekyll`'in `site_klasoru` adlı klasörde oluşturulmasını sağlayın. Daha sonra:

~~~ruby
cd site_klasorum
jekyll serve
~~~

komutlarını girerek `Jekyll`'i çalıştırın. Eğer herşeyi doğru yaptıysanız `http://localhost:4000` adresine girdiğimizde `Jekyll` sizi karşılıyor olacak.

## Bölüm 4: Jekyll için tema indirelim ##

[http://jekyllthemes.org/](http://jekyllthemes.org/) adresinde açık kaynaklı `Jekyll` temaları bulabilirsiniz. Bu blog, Michael Rose'ın geliştirdiği `HPSTR` teması üzerine kurulu. İndirmek için:

~~~ruby
cd site_klasoru
git clone https://github.com/mmistakes/hpstr-jekyll-theme.git
~~~

komutunu girerek kendi bilgisayarınıza indirebilirsiniz.

## Bölüm 5: Scaffolding konusuna değinelim ##

Öncelikle, `_site` klasörü, `Jekyll` tarafından generate edilen statik dosyaların tutulduğu klasördür. Bu klasörü root klasör olarak düşünebilirsiniz. İçerisinde değişiklik yapmamanız gerekiyor çünkü `jekyll build` yazdığınızda bunlar override edilmiş olacak.

`_posts` klasörü `Markdown` formatında yazdığınız yazıları tutacağınız klasör. `Markdown` syntaxı çok kolay ve yüksek ihtimalle daha önce farkında olmadan kullandınız. Popüler siteler (stackoverflow, Reddit, bazı forumlar gibi) `Markdown` formatını kullanıyor. Detaylı bilgiye [http://daringfireball.net/projects/markdown/syntax](http://daringfireball.net/projects/markdown/syntax) adresinden ulaşabilirsiniz.

Normalde `Markdown` syntaxında yazı yazarken bir yazının kod olduğunu göstermek için soldan 4 boşluk koymak gerekiyor, böylece harfler arasındaki mesafe eşitleniyor ve yazdığınız kod okunabilir oluyor. (HTML'deki `<pre>` mantığı) Ancak, `HPSTR` teması code highlighting için `Kramdown` formatını kullanıyor. Bu yüzden yazdığınız kodların hangi dilde olduğunu belirtip syntax highlighting işlemini kolayca yapabiliyorsunuz. 

Benim kişisel tavsiyem `Kramdown` yerine `Redcarpet` formatını kullanmanızdır. `Redcarped` formatını kullanmak için `config.yml` dosyanızı aşağıdaki hale getirin.

~~~ruby
markdown:    redcarpet

redcarpet:
  extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "strikethrough", "superscript"]
~~~

`Config.yml` dosyası üzerinden diğer düzenlemeleri de yapabilirsiniz. Örneğin, `Pygments` kullanmak istemiyorsanız bunu devredışı bırakabilirsiniz. Config dosyasındaki tüm ayarlar son derece anlaşılabilir olacak şekilde isimlendirildiği için üzerinde daha fazla durmayacağım.

## Bölüm 6: Jekyll build ##

Unutmamanız gereken konu, site üzerinde herhangi bir değişiklik yaptığınızda `jekyll build` yazarak `_site` klasörü altındaki dosyaların tekrar oluşturulmasını sağlamak. Örneğin `_posts` klasörüne yeni bir yazı yazdığınız zaman bunun görünür olması için `jekyll build` komutunu girmeniz gerekiyor, ki `Jekyll` statik sitenizi generate edebilsin.

Bunu local sunucunuzda kullanıp deployment öncesinde hata varmı diye bakabilirsiniz. Eğer bununla uğraşmak istemiyorsanız şanslısınız, çünkü `Github Pages` build işlemini bizim yerimize yapabiliyor.

**Güncelleme:** [Jekyll 2.4.0](http://jekyllrb.com/news/2014/09/10/jekyll-2-4-0-released/) yayınlandıktan sonra proje üzerinde çalışırken `jekyll serve` komutu anlık olarak değişiklikleri built ediyor. Bu yüzden `jekyll build` koşmaya gerek kalmıyor. Kısaca eskiden `jekyll serve --watch` ile yaptığımız iş artık default oldu.

## Bölüm 7: Github pages üzerinde sayfamızı oluşturalım ##

Github üzerinde sayfa açmak için birkaç çeşit yol var ancak ben size en kolay yoldan kişisel sayfanızı nasıl oluşturabileceğinizi anlatacağım. İşlem son derece basit.

Öncelikle, Github'a kayıtlı değilseniz kayıt olun ve daha sonra `Repositories` sekmesine gelip `New` butonuna tıklayın. Repository adı `githubHesapAdınız.github.io` formatında olmalı. Bu şekilde Github, bu reponun sizin kişisel sayfanız olacağını anlıyor.

## Bölüm 8: Deployment işlemi ##

`config.yml` içerisindeki `url` değerini `http://githubHesapAdınız.github.io` olarak değiştirin. (Çünkü assetler artık localhost üzerinden sunulmayacak) Daha sonra:

~~~ruby
	  cd site_klasoru
    git init
    git remote add origin https://github.com/githubHesapAdınız/githubHesapAdınız.github.io.git
    git add -A
    git commit -m "Initial deployment"
    git push origin master
    // SSH key kullanmıyorsanız github kullanıcı adı ve şifrenizi isteyecek
~~~

komutlarını girerek dosyaları remote reponuza pushlayın. Birkaç dakika içerisinde [http://githubHesapAdınız.github.io](http://githubHesapAdınız.github.io)  adresine giriş yapın ve siteniz sizi karşılıyor olacaktır.

## Bölüm 9: Grunt için ayrı bir paragraf. ##

Öncelikle `GruntJS` bir javascript task runner. Kendisi bir `NPM` paketi ve modülleri de `NPM ` aracılığıyla yükleniyor. Tabiri caizse yazılım geliştirirken yaptığınız `ayak işlerini` bırakın ben yapayım mantığıyla çalışıyor. Örneğin; resimlerin küçültülmesi ve optimizasyonu, `grunt-contrib-watch` modülü ile unit testlerin otomatik çalıştırılması, lintlerin çalıştırılması, css ve javascript dosyalarının uglify/minify/combine işlemlerinin yapılması gibi vakit alıcı işlemleri kendisi yapıyor. `GruntJS`'ın birçok modülü var ve yapabilecekleri bunlarla sınırlı değil.

Kullandığım `HPSTR` teması bir `Gruntfile` dosyasıyla beraber geliyor. `npm install` komutunu girdiğiniz zaman `GruntJS`'ın dependencyleri `node_modules` isimli bir klasöre indirilmiş olacak. Bunu doğru yaptırsanız terminalinize `grunt` komutunu girdiğinizde gerekli işlemlerin yapıldığını göreceksiniz.

Ben deployment yapmadan önce en azından bir defa `Grunt`'un çalıştırılması gerektiğini düşünüyorum. Zaten bir defa nasıl çalıştığını görünce bağımlısı olacaksınız ve benim gibi siz de kendi `Gruntfile` scriptlerinizi yazmaya başlayacaksınız. :)

## Bölüm 10: Sonuç ##

Yazımız boyunca `Jekyll`'in ne olduğunu, `Github Pages` üzerinde kişisel blogumuzu nasıl host edebileceğimizi, `GruntJS`'ın nasıl çalıştığını, `Git` kullanarak nasıl remote repo oluşturup nasıl deployment yapabileceğimizi ve `Package Manager`ların nasıl kullanıldığını öğrenmiş olduk.

Kurulum gibi birçok konuya değinmediğim için kurulum esnasında biraz sıkıntı çekebilirsiniz. Ben aldığım hatalardan kısaca bahsedeyim:

- Bilgisayarımda Python 2.7.5 kurulu değildi, bu yüzden `Pygments` sorun çıkartıyordu. Python 2.7.5 kurarak sorunu çözdüm. Bunu kurmak istemiyorsanız `Pygments` i devredışı bırakın ancak o zaman syntax highlighting özelliğinden yararlanamazsınız.

- Jekyll komutlarını girince `Liquid Exception UTF-8 / Special Characters` hatası alabiliyorsunuz. Çözümü terminalde Jekyll komutlarını girmeden önce Windows için `chcp 65001` komutunu, linux tabanlı işletim sistemleri ve Git Bash için `export LANG=en_US.UTF-8` yazarak terminalin dilini runtime esnasında değiştirmek.

- Grunt'un Imagemin modülü windows tabanlı işletim sistemlerinde sıkıntılı çalışıyor. Ben Gruntfile dosyamdan onu kaldırmak zorunda kaldım. Bu konu hakkında bir issue açık durumda: [https://github.com/gruntjs/grunt-contrib-imagemin/issues/109](https://github.com/gruntjs/grunt-contrib-imagemin/issues/109) Zaten küçük bir site, resimleri optimize etmek bize fazla birşey kazandırmayacak.

Başka yazılarda tekrar görüşmek üzere!

> Yazım yanlışları olabilir. Bu tür yanlışlıkları düzeltip [https://github.com/Aristona/aristona.github.io](https://github.com/Aristona/aristona.github.io) adresine `Pull Request` atabilirsiniz.
