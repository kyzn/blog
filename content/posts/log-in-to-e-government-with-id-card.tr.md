---
title: "E-Devlet'e Kimlik Kartı ile Giriş Yapmak"
date: 2020-05-16T15:30:00+03:00
tags:
  - ubuntu
  - akıllı-kart
---

![](/images/edevlet-menu.png)

E-Devlet çok sayıda kurumun çok sayıda hizmeti sunduğu bir web uygulaması olarak artık birçok kişi tarafından kullanılıyor. Bu uygulamaya kimlik kartınızı kullanarak da giriş yapabiliyorsunuz. Estonya E-Oturumu başvurusundan kalan kart okuyucumu kullanarak giriş yapmayı denedim.

Malzemeler:
- Ubuntu 19.10
- Estonia E-Oturumu USB kart okuyucu
- T.C. Kimlik Kartı
- ve kartın pin kodu

# 1. Adım

Önce kart okuyucunun `lsusb` komutu tarafından gösterilip gösterilmediğine bakalım. Kart okuyucuyu bilgisayara bağlamadan önce `lsusb` komutunu çalıştırın. Daha sonra kart okuyucuyu bağlayıp tekrar çalıştırın. Komut çıktısında fazladan bir satır görmeniz gerekiyor.

    Advanced Card Systems, Ltd ACR38 SmartCard Reader

Bunu yapmanın bir yolu daha var. `watch lsusb` komutunu çalıştırın ve kart okuyucuyu takıp çıkardığınızda ekranı izleyin.

# 2. Adım

E-Kimlik uygulamasını https://cdn.e-devlet.gov.tr/downloads/e-kimlik/edevlet-ekimlik.jnlp adresinden indirin. Bu dosyayı Ubuntu işletim sisteminde açabilmek için `icedtea-netx` kütüphanesini yüklemelisiniz.

    $ sudo apt-get install icedtea-netx


# 3. Adım

Kart okuyucunuzun `lsusb` komutunda görünmesi E-Kimlik uygulamasının kartınızı okuyabileceği anlamına gelmiyor. Bunun için birkaç kütüphaneye daha ihtiyacımız var.

    $ sudo apt-get install pcscd pcsc-tools

Bunu yüklediğimizde `pcsc_scan` isminde çok kullanışlı bir aracımız oluyor. Bu komutu kullanarak kart okuyucumuzun çalışıp çalışmadığını kontrol edebiliyoruz. Kart okuyucuyu bilgisayarınızdan çıkarın ve aşağıdaki komutu çalıştırın. 3. adımın buradan sonrası zorunlu değil, **isterseniz [4. adıma geçebilirsiniz](#4-adım).**

    $ pcsc_scan

Bu komut siz `^C` ile durduruna kadar çalışacak. Önce ekranda şöyle bir şey göreceksiniz:

    Scanning present readers...
    Waiting for the first reader...
<center><h6>"Mevcut okuyucular taranıyor... İlk okuyucu için bekleniyor..."</h6></center>

Daha sonra kart okuyucu taktığınızda burada küçük bir güncelleme olacak:

    Waiting for the first reader...found one
<center><h6>"İlk okuyucu için bekleniyor...Bir tane buldum"</h6></center>

Eğer kart okuyucuda kart yoksa şunu göreceksiniz:

    Card state: Card removed,
<center><h6>"Kart durumu: Kart çıkarıldı,"</h6></center>

Ve kartı taktığınızda da şunu göreceksiniz:

    Card state: Card inserted,
    ATR: 01 23 45 67 89 AB CD EF .. .. .. ..
<center><h6>"Kart durumu: Kart takıldı, ATR kodu: ...."</h6></center>

Aslına bakarsanız ben kartı taktığımda bir hata mesajı aldım.

    Can't locate Chipcard/PCSC/Card.pm in @INC

Sistemin kendi Perl uygulaması yerine `perlbrew` kullandığım için bu hata oluştu. Yani `libpcsc-perl` kütüphanesi yüklenmişti, ama benim mevcut Perl kurulumumun bundan pek de haberi yoktu. Dolayısıyla `perlbrew switch-off` komutuyla sistemin kendi Perl uygulamasına dönmem gerekti. Ondan sonra `pcsc_scan` komutunu tekrar çalıştırdım ve sorun çıkmadı.

    $ which perl
    /home/kivanc/perl5/perlbrew/perls/perl-5.30.2/bin/perl
    $ perlbrew switch-off
    perlbrew is switched off.
    $ which perl
    /usr/bin/perl

Eğer bunu yaparsanız işiniz bittiğinde `perlbrew switch` komutuyla eski duruma geri dönebilirsiniz.

# 4. Adım

https://giris.turkiye.gov.tr/Giris/T-C-Kimlik-Karti adresine gidin ve T.C. Kimlik Numaranızı yazın. Ekranda bir işlem kodu görünecek. Bu kodu kopyalayın ve tarayıcı sekmesini açık tutun.

![](/images/edevlet-giris.png)

# 5. Adım

Kartınızı kart okuyucuya, kart okuyucuyu da bilgisayara takın. JNLP dosyasını çift tıklayarak veya aşağıdaki komutu kullanarak çalıştırın.

    $ javaws edevlet-ekimlik.jnlp

Eğer bu uygulamayı ilk kez çalıştırıyorsanız size izinler ve kısayollar ile ilgili birkaç soru sorabilir. Ben masaüstü kısayolu oluşturmamayı tercih ettim.

# 6. Adım

4. adımdaki işlem kodunu uygulamaya girin. "Standart kart okuyucu ile giriş" butonunu tıklayın.

![](/images/edevlet-jnlp-menu.png)

# 7. Adım

Şimdi uygulama size kartınızın pin kodunu soracak. Yeni kimlik kartı için başvurduğunuzda gelen zarfı hatırlıyor musunuz? Bu pin kodu o zarfın içinde olmalı.

![](/images/edevlet-jnlp-pin.png)

# 8. Adım

İşleminizin tamamlandığı mesajını göreceksiniz. Tarayıcınıza geri dönün ve giriş yapıp yapmadığınızı kontrol edin. Artık uygulamayı kapatıp kart okuyucuyu çıkarabilirsiniz.

![](/images/edevlet-jnlp-done.png)
