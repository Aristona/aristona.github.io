---
layout: post
title: "OOP'in ötesinde..."
description: "OOP'in ötesinde..."
modified: 2015-08-21
tags: [PHP]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

#OOP'in ötesinde...#

Bugünlerde birçoğumuz öğrendiği OOP tekniklerini kod yazarken uyguluyor. Methodlar, sınıflar, kalıtımlar, prototipler... Ancak, yaptığımız işlerde doğru olmayan birşeyler var. Genellikle bir veri bir methoda gidiyor, o methodda işlenip başka birşey olarak geri dönüyor, dönen veri başka bir sınıfa gidiyor, o sınıf o veriyi tekrar işliyor ve başka birşey döndürüyor.

Klasik OOP ile, kendi başına bir anlamı olmayan anlamsız (aptal) verileri anlamlı hale getirmek için, akıllı bir sınıf oluşturma ihtiyacı duyuyoruz. Bu bana göre çok mantıklıca bir yöntem değil.

Bir düşünün, yolda yürürken yerden bir elma bulduğumuzu farzedelim. Biz o elmayı elimize aldığımızda "Bu bir elmadır." diyebiliyoruz, ancak kod yazdığımızda ise, "Bu [objeyi] bir elma analiz fabrikasına sok. Eğer gerçekten bir elma ise, bana doğru veya yanlış de. Eğer obje analiz edilemezse, bilinmeyen obje exceptionu fırlat ve evrenin yok olmasını önle." tarzında kod yazıyoruz. Elma, kendisinin bir elma olduğunu neden bilemiyor? Biz neden onun elma olduğunu bilemiyoruz? Yazdığımız kodlarda neden en çekirdek objelerimiz bu kadar aptal oluyor?

Bazen kod yazarken aklıma bu tür sorular takılır. "Acaba bunu böyle yapsak nasıl olurdu?" sorusunu sık sık sorarım ve araştırmaya başlarım. Sonuç olarak, genellikle birileri bu soruların cevabını yıllar öncesinden vermiş olur.

İşin mühendislik kısmını yapmak sıkıcı tarafı, ama yaptığımız şeyi sanatımızla süslemek bizim elimizde.

// Yakında

// Value Objects

// Functional Programming Paradigms

// Reflective

// ADR

// Domain Driven Design

// AOP

// Event Driven Design

// Testability

// PHP ve OOP'in az kullanılan yönleri