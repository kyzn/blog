---
title: "GitHub'da Varsayılan Dal İsmini Değiştirmek"
date: 2020-06-15T20:00:00Z
tags:
  - git
  - github
---

Her git deposunun varsayılan bir dalı vardır. Aşağıda GitHub depolarınızın varsayılan dal ismini nasıl değiştirebileceğinizi görebilirsiniz.

## 1) Haber verin

Varsayılan dal isminin değiştirilmesi takımlar için büyük bir değişim olabilir. Eğer birçok katkıcısı olan bir deponuz varsa önce onlara haber vermelisiniz. Bir zaman dilimi önerip bekleyebilirsiniz.

## 2) Bazı özelliklerin çalışmama ihtimali olduğunu unutmayın

Uzunca bir süre master hem git hem de GitHub için varsayılan isimdi. Dolayısıyla bazı GitHub özelliklerinin çalışmadığını görebilirsiniz.

Mesela GitHub Pages size şu anda iki farklı kaynak seçeneği sunuyor. Her iki seçenek de master dalından veri alıyor. Bunun yakın zamanda değişeceğini tahmin ediyorum. Bu değişiklik olana kadar siz GitHub Pages depolarınızın master dallarını silmemeyi düşünebilirsiniz.

![](/images/github-pages-master.png)

## 3) ingydotnet/git-hub'ı indirin

GitHub'a bağlanan bir komut satırı aracı olan [git-hub](https://github.com/ingydotnet/git-hub/)'ı kullanacağız. Bu betiği yüklemek için üç yol bulunuyor. Ben burada önerilen yolu anlatacağım.

### 3.1) git-hub deposunu klonlayın

İlk adım depoyu klonlamak. Bu betik klonlandığı klasörden çalışacak. Ben  `/home/kivanc/.git-hub`'ı seçtim. Yani kısaca `~/.git-hub`.

    git clone https://github.com/ingydotnet/git-hub ~/.git-hub

### 3.2) Başlatma betiğini çalıştırın

Depoyu klonladık fakat `git hub` komutları henüz gelmedi. Bunun için başlatma betiğini `source` ile çalıştırmamız gerekiyor.

    source ~/.git-hub/.rc

### 3.3) Başlatma betiğini terminal başlangıcında çalıştırın

`git hub` komutlarına her ihtiyacımız olduğunda `source` kullanmak yerine bunu başlangıca da ekleyebiliriz. Ben `~/.bash_aliases`'a ekledim. Eğer sizde bu yoksa  `~/.bashrc`, `~/.bash_profile` veya `~/.profile` dosyalarını deneyebilirsiniz.

Dosyaya ekleme yapmak için iki tane büyüktür işaretine ihtiyacınız olduğunu unutmayın. Bir tane büyüktür işareti "üzerine yaz" anlamına geliyor.

    echo 'source ~/.git-hub/.rc' >> ~/.bash_aliases

### 3.4) git hub setup'ı çalıştırın

Bazı git-hub komutlarının çalışması için önce aşağıdaki komutu çalıştırmanız gerekiyor. Burada size GitHub kullanıcı adınız, şifreniz ve varsa 2 faktörlü kimlik doğrulama kodunuz sorulacak.

    git hub setup

Bu kurulum aşağıdaki gibi görünecek.

<center><script id="asciicast-339891" src="https://asciinema.org/a/339891.js" async></script></center>

## 4) ditch-master.sh'ı indirin

fREW bu süreci otomatikleştiren bir betik yazdı. Bu sayede depoları tek tek gezmenize gerek kalmadı. Bu betiği https://gist.github.com/frioux/6c259c63dda705f9ef9d030ddb880347 adresinde görebilirsiniz.

Bu betiği yukarıdan indirebileceğiniz gibi, sanki bir git deposuymuş gibi klonlayabilirsiniz de.

    git clone https://gist.github.com/6c259c63dda705f9ef9d030ddb880347.git ~/ditch-master

İndirdiğiniz betiğin yürütülebilir olması gerekiyor.

    cd ~/ditch-master
    chmod a+x ditch-master.sh

Bu betik yeni varsayılan dal ismi olarak "main" sözcüğünü kullanıyor. Eğer farklı bir kelime kullanmak isterseniz betikteki "main" kelimelerini istediğiniz bir kelimeyle değiştirebilirsiniz. Mesela "trunk" kelimesini kullanmak isterseniz, şu komutu kullanabilirsiniz.

    perl -i -pe 's/main/trunk/g' ditch-master.sh

Geriye sadece betiği çalıştırmak kalıyor.

    ./ditch-master.sh

Bu betik şu şekilde çalışacak.

<center><script id="asciicast-EqW4uYlihVExmzb0OiIDA4NUY" src="https://asciinema.org/a/EqW4uYlihVExmzb0OiIDA4NUY.js" async></script></center>

İşte bu kadar!
