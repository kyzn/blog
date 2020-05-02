---
title: "Hoş Geldin Hugo"
date: 2019-08-26T20:09:05-07:00
tags:
  - duyuru
---

Blogumu bir statik site üreticisinden başkasına taşıdım. Bugüne kadar [Plerd](https://github.com/jmacdotorg/plerd)’ün [sadeleştirilmiş bir kopyası](https://github.com/kyzn/mini-plerd) ile html dosyalarını üretiyordum. Doğruyu söylemek gerekirse bu aslında güzel bir sistemdi. Şimdi aynı markdown dosyalarından statik içerik üretmek için [Hugo](https://gohugo.io)’yu kullanıyorum. 

mini-plerd’ü (yukarıda bahsettiğim sadeleştirilmiş Plerd) ne kadar istesem de güncel tutamadım. Orijinal Plerd’ün bazı güncellemelerini kendi kopyama dahil edemedim. Çok yakın bir zamana kadar markdown tablolarını bile destekleyemiyordum. Belki de Plerd’ün kendisini kullansam daha iyi olurdu.

Hugo [çok sayıda tema](https://themes.gohugo.io/) ile geliyor. Bunlardan bir tane seçmeniz gerekiyor. Temayı kurmak ve değiştirmek gayet kolay.

Ayrıca blogu internete yükleme şeklimi de değiştirdim. Önceden dosyalarımı DigitalOcean’a yüklüyor, oradan da nginx aracılığıyla yayınlıyordum. Bunun maliyeti ayda 5 dolardı. Şimdi düşünüyorum da AWS S3 veya DigitalOcean Spaces gibi bir obje deposu da kullanabilirdim. Artık [GitHub pages](https://pages.github.com) kullanıyorum. Burada dosyalarınızı bir depoya yüklüyorsunuz ve GitHub sizin için ücretsiz bir şekilde yayınlıyor. Kendi alan adınızla da kullanabilmeniz güzel bir detay.

Şimdilik Hugo’yu çok sevdim. Yerelde yeni yazılar yazmak çok kolay. Yakında Hugo ile ilgili bir kurulum blogu yazacağım. Yüklerken karşılaştığım hata mesajlarını da içerecek.