---
title: Azure ön kapısı | Microsoft Docs
description: Bu makalede Azure Front Door’a genel bir bakış sağlanır. Uygulamanız için Yük Dengeleme Kullanıcı trafiği için doğru seçim olup olmadığını öğrenin.
services: frontdoor
documentationcenter: ''
author: megan-beatty
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/30/2020
ms.author: mebeatty
ms.openlocfilehash: 77c0d68f507e09b315c912d1d91fdf9cf63db6fa
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82515534"
---
# <a name="azure-front-door-rules-engine-match-conditions"></a>Azure ön kapı kuralları altyapısı eşleştirme koşulları

[AFD kural altyapısında](front-door-rules-engine.md) bir kural sıfır veya daha fazla eşleşme koşulu ve bir eylemden oluşur. Bu makalede, AFD kural altyapısında kullanabileceğiniz eşleştirme koşullarına ilişkin ayrıntılı açıklamalar sağlanmaktadır. 

Bir kuralın ilk bölümü eşleşme koşullarıdır veya eşleştirme koşulları kümesidir. Bir kural, en fazla 10 eşleşme koşullarından oluşabilir. Bir eşleşme koşulu, tanımlı eylemlerin gerçekleştirildiği belirli istek türlerini tanımlar. Birden çok eşleşme koşulu kullanırsanız, eşleşme koşulları ve Logic kullanılarak birlikte gruplandırılır. Birden çok değeri destekleyen (aşağıda "boşlukla ayrılmış" olarak belirtilen) tüm eşleşme koşulları için "OR" işleci kabul edilir. 

Örneğin, bir eşleşme koşulunu şu şekilde kullanabilirsiniz:

- İstekleri belirli bir IP adresine, ülkeye veya bölgeye göre filtreleyin.
- İstekleri üst bilgi bilgisine göre filtreleyin.
- Mobil cihazlardan veya masaüstü cihazlardan gelen istekleri filtreleyin.

Aşağıdaki eşleşme koşulları, Azure ön kapı kuralları altyapısında kullanılabilir.  

## <a name="device-type"></a>Cihaz Türü 

Bir mobil cihazdan veya masaüstü cihazdan yapılan istekleri tanımlar.  

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Desteklenen değerler
---------|----------------
Eşittir, eşit değildir | Mobil, masaüstü

## <a name="post-argument"></a>Post bağımsız değişkeni

İstekte kullanılan POST isteği yöntemi için tanımlanan bağımsız değişkenlere göre istekleri tanımlar. 

#### <a name="required-fields"></a>Gerekli alanlar

Bağımsız değişken adı | İşleç | Bağımsız değişken değeri | Örnek dönüştürme
--------------|----------|----------------|---------------
Dize | [Standart işleç listesi](#standard-operator-list) | String, Int | Küçük harf, büyük harf, kesme, boşluk kaldırma, URL kodlama, URL kod çözme

## <a name="query-string"></a>Sorgu dizesi

Belirli bir sorgu dizesi parametresi içeren istekleri tanımlar. Bu parametre, belirli bir düzenle eşleşen bir değere ayarlanır. İstek URL 'sindeki sorgu dizesi parametreleri (örneğin, **Parameter = değer**) bu koşulun karşılanıp karşılanmadığını belirtir. Bu eşleştirme koşulu, bir sorgu dizesi parametresini adına göre tanımlar ve parametre değeri için bir veya daha fazla değeri kabul eder.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Sorgu dizesi | Örnek dönüştürme
---------|--------------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Küçük harf, büyük harf, kesme, boşluk kaldırma, URL kodlama, URL kod çözme

## <a name="remote-address"></a>Uzak adres

İstekleri isteyanın konumuna veya IP adresine göre tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Desteklenen değerler
---------|-----------------
Coğrafi eşleşme | Ülke kodu
IP eşleşmesi | IP adresi (boşlukla ayrılmış)
Coğrafi eşleşme değil | Ülke kodu
IP eşleşmesi değil | IP adresi (boşlukla ayrılmış)

#### <a name="key-information"></a>Önemli bilgiler

- CıDR gösterimini kullanın.
- Birden çok IP adresi ve IP adres bloğu belirtmek için, değerler arasında tek bir boşluk kullanın:
  - **IPv4 örneği**: *1.2.3.4 10.20.30.40* , 1.2.3.4 veya 10.20.30.40 adresinden gelen isteklerle eşleşir.
  - **IPv6 örneği**: *1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80* , 1:2:3:4:5:6:7:8 ya da 10:20:30:40:50:60:70:80 adresinden gelen isteklerle eşleşir.
- Bir IP adres bloğunun sözdizimi, ardından bir eğik çizgi ve ön ek boyutu gelen temel IP adresidir. Örneğin:
  - **IPv4 örneği**: *5.5.5.64/26* , 5.5.5.64 ile 5.5.5.127 arası adreslerden gelen isteklerle eşleşir.
  - **IPv6 örneği**: *1:2:3:/48* , 1:2:3:0:0:0:0:0 ile 1:2: 3: ffff: ffff: ffff: ffff: ffff adresinden gelen tüm isteklerle eşleşir.

## <a name="request-body"></a>İstek gövdesi

İstek gövdesinde görüntülenen belirli bir metne göre istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | İstek gövdesi | Örnek dönüştürme
---------|--------------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Küçük harf, büyük harf, kesme, boşluk kaldırma, URL kodlama, URL kod çözme

## <a name="request-header"></a>İstek üst bilgisi

İstekte belirli bir üst bilgiyi kullanan istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Üst bilgi adı | İşleç | Üst bilgi değeri | Örnek dönüştürme
------------|----------|--------------|---------------
Dize | [Standart işleç listesi](#standard-operator-list) | String, Int | Küçük harf, büyük harf, kesme, boşluk kaldırma, URL kodlama, URL kod çözme

## <a name="request-method"></a>İstek yöntemi

Belirtilen istek yöntemini kullanan istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Desteklenen değerler
---------|----------------
Eşittir, eşit değildir | AL, POSTALA, KOY, SIL, HEAD, SEÇENEKLER, IZLE

#### <a name="key-information"></a>Önemli bilgiler

- Yalnızca GET isteği yöntemi Azure ön kapıda önbelleğe alınmış içerik oluşturabilir. Diğer tüm istek yöntemleri ağ üzerinden proxy olarak kullanılır. 

## <a name="request-protocol"></a>İstek Protokolü

Kullanılan belirtilen protokolü kullanan istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Desteklenen değerler
---------|----------------
Eşittir, eşit değildir | HTTP, HTTPS

## <a name="request-url"></a>İstek URL'si

Belirtilen URL ile eşleşen istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | İstek URL'si | Örnek dönüştürme
---------|-------------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Küçük harf, büyük harf, kesme, boşluk kaldırma, URL kodlama, URL kod çözme

#### <a name="key-information"></a>Önemli bilgiler

- Bu kural koşulunu kullandığınızda protokol bilgilerini eklediğinizden emin olun. Örneğin: *https://www.\<yourdomain\>.com*.

## <a name="request-file-extension"></a>İstek dosyası uzantısı

İstenen URL 'deki dosya adında belirtilen dosya uzantısını içeren istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Dahili numara | Örnek dönüştürme
---------|-----------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Küçük harf, büyük harf, kesme, boşluk kaldırma, URL kodlama, URL kod çözme

#### <a name="key-information"></a>Önemli bilgiler

- Uzantı için, önde gelen bir nokta eklemeyin; Örneğin, *. html*yerine *HTML* kullanın.

## <a name="request-file-name"></a>İstek dosyası adı

İstenen URL 'de belirtilen dosya adını içeren istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Dosya adı | Örnek dönüştürme
---------|-----------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Küçük harf, büyük harf, kesme, boşluk kaldırma, URL kodlama, URL kod çözme

#### <a name="key-information"></a>Önemli bilgiler

- Birden çok dosya adı belirtmek için, her dosya adını ENTER tuşuna basarak ayırın. 

## <a name="request-path"></a>İstek yolu

İstek URL 'sinde belirtilen yolu içeren istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Değer | Örnek dönüştürme
---------|-------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Küçük harf, büyük harf, kesme, boşluk kaldırma, URL kodlama, URL kod çözme

## <a name="standard-operator-list"></a>Standart işleç listesi

Standart işleç listesinden değerleri kabul eden kurallar için aşağıdaki işleçler geçerlidir:

- Herhangi biri
- Eşittir 
- Contains 
- Şununla başlar 
- Şununla biter 
- Küçüktür
- Küçüktür veya eşittir
- Büyüktür
- Büyük veya eşittir
- Hiçbirini değil
- İçermez
- Şununla başlamaktadır 
- Şununla bitmiyor 
- Küçüktür
- Küçüktür veya eşittir
- Şundan büyük değil
- Büyük veya eşit değil

*Küçüktür ve* *büyüktür*gibi sayısal işleçler için, kullanılan karşılaştırma uzunluğa göre belirlenir. Bu durumda, Match koşulunun değeri, karşılaştırmak istediğiniz uzunluğa eşit bir tamsayı olmalıdır. 


## <a name="next-steps"></a>Sonraki adımlar

- İlk [kural altyapısı yapılandırmanızı](front-door-tutorial-rules-engine.md)ayarlamayı öğrenin. 
- [Kural altyapısı eylemleri](front-door-rules-engine-actions.md) hakkında daha fazla bilgi edinin
- [Azure ön kapı kuralları altyapısı](front-door-rules-engine.md) hakkında daha fazla bilgi edinin
