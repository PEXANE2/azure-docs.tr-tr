---
title: Microsoft standart kuralları altyapısından eşleşme koşullarından Azure CDN | Microsoft Docs
description: Microsoft standart kuralları altyapısı eşleştirme koşullarından Azure Content Delivery Network için başvuru belgeleri.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: a72452d37b152a9463a5aee0e199fd42ea852236
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615968"
---
# <a name="azure-cdn-from-microsoft-standard-rules-engine-match-conditions"></a>Microsoft standart kuralları altyapısından eşleşme koşullarından Azure CDN

Bu makalede, Microsoft [Standart kuralları altyapısından](cdn-standard-rules-engine.md)Azure Content DELIVERY Network (CDN) için kullanılabilen eşleşme koşullarının ayrıntılı açıklamaları listelenmektedir.

Kuralın ilk bölümü bir eşleşme koşulları kümesidir. Her kuralda en fazla 4 eşleşme koşulu olabilir. Bir eşleşme koşulu, kuralda tanımlanan eylemlerin gerçekleştirileceği belirli türde istekleri belirler. Birden çok eşleşme koşulu kullanırsanız, ve mantığı kullanılarak birlikte gruplandırılır.

Örneğin, bir eşleşme koşulunu şu şekilde kullanabilirsiniz:

- Belirli bir IP adresi veya ülke/bölge tarafından oluşturulan filtre istekleri.
- İstekleri üst bilgi bilgisine göre filtreleyin.
- Mobil veya masaüstü cihazlarındaki istekleri filtreleyin.

## <a name="match-conditions"></a>Koşulları Eşleştir

Aşağıdaki eşleşme koşulları kullanılabilir. 

### <a name="device-type"></a>Cihaz türü 

Cihaz türü eşleştirme koşulu, bir mobil veya masaüstü cihazdan yapılan istekleri özelliklerine göre tanımlar.  

**Gerekli alanlar**

İşleç | Desteklenen değer
---------|----------------
Eşittir, eşit değildir | Mobil, masaüstü


### <a name="http-version"></a>HTTP sürümü

HTTP sürümü eşleştirme koşulu, isteğin ulaştığı HTTP sürümüne göre istekleri tanımlar.

**Gerekli alanlar**

İşleç | Desteklenen değer
---------|----------------
Eşittir, eşit değildir | 2,0, 1,1, 1,0, 0,9, All


### <a name="request-cookies"></a>İstek tanımlama bilgileri

Istek tanımlama bilgileri eşleştirme koşulu, gelen istekteki tanımlama bilgisi bilgilerine göre istekleri tanımlar.

**Gerekli alanlar**

Tanımlama bilgisi adı | İşleç | Tanımlama bilgisi değeri | Örnek dönüştürme
------------|----------|--------------|---------------
Dize | [Standart işleç listesi](#standard-operator-list) | String, Int | Dönüştürme yok, büyük harf, küçük harf

Anahtar bilgileri
- Bir tanımlama bilgisi adı belirtirken, yıldız işareti (*) de dahil olmak üzere joker karakter değerleri desteklenmez; yalnızca tam tanımlama bilgisi adı eşleşmeleri karşılaştırma için uygundur.
- Bu eşleşme koşulunun örneği başına yalnızca tek bir tanımlama bilgisi adı belirtilebilir.
- Tanımlama bilgisi adı karşılaştırmaları büyük/küçük harfe duyarlıdır.
- Her birini tek bir boşlukla ayırarak birden fazla tanımlama bilgisi değeri belirtin. 
- Tanımlama bilgisi değerleri joker değerlerden faydalanabilir.
- Joker karakter değeri belirtilmediyse, bu eşleştirme koşulunu yalnızca tam eşleşme karşılar. Örneğin, "Value" belirtildiğinde "Value" eşleşir, ancak "değer1" olarak kullanılamaz. 

### <a name="post-argument"></a>Post bağımsız değişkeni

**Gerekli alanlar**

Bağımsız değişken adı | İşleç | Bağımsız değişken değeri | Örnek dönüştürme
--------------|----------|----------------|---------------
Dize | [Standart işleç listesi](#standard-operator-list) | String, Int | Dönüştürme yok, büyük harf, küçük harf

### <a name="query-string"></a>Sorgu Dizesi

Sorgu dizesi eşleştirme koşulları, belirtilen sorgu dizesi parametresini içeren istekleri tanımlar. Bu parametre, belirtilen bir düzenle eşleşen bir değere ayarlanır. İstek URL 'sindeki sorgu dizesi parametreleri (örneğin, Parameter = değer) bu koşulun karşılanıp karşılanmadığını belirtir. Bu eşleştirme koşulu, bir sorgu dizesi parametresini adına göre tanımlar ve parametre değeri için bir veya daha fazla değeri kabul eder.

**Gerekli alanlar**

İşleç | Sorgu Dizesi | Örnek dönüştürme
---------|--------------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Dönüştürme yok, büyük harf, küçük harf

### <a name="remote-address"></a>Uzak adres

Uzak adres eşleştirme koşulu, istekleri istek sahibinin konumuna veya IP adresine göre belirler.

**Gerekli alanlar**

İşleç | Desteklenen değerler
---------|-----------------
Herhangi biri | Yok
Coğrafi eşleşme | Ülke kodları
IP eşleşmesi | IP adresleri (boşluk ayrılmış)
Hiçbirini değil | Yok
Coğrafi eşleşme değil | Ülke kodları
IP eşleşmesi değil | IP adresleri (boşluk ayrılmış)

Anahtar bilgileri:

- CıDR gösterimini kullanın.
- Her birini tek bir boşlukla ayırarak birden çok IP adresi ve/veya IP adresi bloğu belirtin. Örneğin:
  - **IPv4 örneği**: 1.2.3.4 10.20.30.40, 1.2.3.4 veya 10.20.30.40 adresinden gelen isteklerle eşleşir.
  - **IPv6 örneği**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80, 1:2:3:4:5:6:7:8 ya da 10:20:30:40:50:60:70:80 adresinden gelen isteklerle eşleşir.
- Bir IP adres bloğunun sözdizimi, ardından bir eğik çizgi ve ön ek boyutu gelen temel IP adresidir. Örneğin:
  - **IPv4 örneği**: 5.5.5.64/26, 5.5.5.64 ile 5.5.5.127 arası adreslerden gelen isteklerle eşleşir.
  - **IPv6 örneği**: 1:2:3:/48, 1:2:3:0:0:0:0:0 ile 1:2: 3: ffff: ffff: ffff: ffff: ffff adresinden gelen tüm isteklerle eşleşir.

### <a name="request-body"></a>İstek Gövdesi

**Gerekli alanlar**

İşleç | İstek Gövdesi | Örnek dönüştürme
---------|--------------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Dönüştürme yok, büyük harf, küçük harf

### <a name="request-header"></a>İstek üst bilgisi

**Gerekli alanlar**
Üst bilgi adı | İşleç | Üst bilgi değeri | Örnek dönüştürme
------------|----------|--------------|---------------
Dize | [Standart işleç listesi](#standard-operator-list) | String, Int | Dönüştürme yok, büyük harf, küçük harf

### <a name="request-method"></a>İstek yöntemi

**Gerekli alanlar**

İşleç | Desteklenen değer
---------|----------------
Eşittir, eşit değildir | AL, POSTALA, KOY, SIL, HEAD, SEÇENEKLER, IZLE

Anahtar bilgileri:

- Yalnızca GET isteği yöntemi CDN 'de önbelleğe alınmış içerik oluşturabilir. Diğer tüm istek yöntemleri ağ üzerinden proxy olarak kullanılır. 

### <a name="request-protocol"></a>İstek Protokolü

**Gerekli alanlar**

İşleç | Desteklenen değer
---------|----------------
Eşittir, eşit değildir | HTTP, HTTPS

### <a name="request-url"></a>İstek URL'si

**Gerekli alanlar**

İşleç | İstek URL'si | Örnek dönüştürme
---------|-------------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Dönüştürme yok, büyük harf, küçük harf

Anahtar bilgileri:

- Istek URL 'sini girerken protokol bilgilerini dahil edin. Örneğin, "https://www. [yourdomain]. com "

### <a name="url-file-extension"></a>URL dosya uzantısı

**Gerekli alanlar**

İşleç | Dahili numara | Örnek dönüştürme
---------|-----------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Dönüştürme yok, büyük harf, küçük harf

Anahtar bilgileri:

- Uzantı için, önde gelen bir nokta eklemeyin; Örneğin,. html yerine HTML kullanın.

### <a name="url-file-name"></a>URL dosyası adı

**Gerekli alanlar**

İşleç | Dosya adı | Örnek dönüştürme
---------|-----------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Dönüştürme yok, büyük harf, küçük harf

Anahtar bilgileri:

- Birden çok dosya adı belirtmek için, her dosya adını tek bir boşlukla ayırın. 

### <a name="url-path"></a>URL Yolu

**Gerekli alanlar**

İşleç | Değer | Örnek dönüştürme
---------|-------|---------------
[Standart işleç listesi](#standard-operator-list) | String, Int | Dönüştürme yok, büyük harf, küçük harf

Anahtar bilgileri:

- Dosya adı değeri joker değerlerden faydalanabilir. Örneğin her bir dosya adı deseninin bir veya daha fazla yıldız işareti (*), her yıldız işareti bir veya daha fazla karakterle eşleşen bir dizi olabilir.

## <a name="reference-for-rules-engine-match-conditions"></a>Kural altyapısı eşleştirme koşulları başvurusu

### <a name="standard-operator-list"></a>Standart Işleç listesi

Standart işleç listesini içeren kurallar için aşağıdaki işleçler geçerlidir:

- Herhangi biri
- Eşittir 
- Contains 
- Şununla başlar 
- Şununla biter 
- Şu değerden az:
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

"Küçüktür" veya "büyüktür veya eşittir" gibi sayısal işleçler için, kullanılan karşılaştırma uzunluğa göre değişir. Bu durumda, eşleşme koşulunun değeri, karşılaştırmak istediğiniz uzunluğa eşit bir tamsayı olmalıdır. 

---

[Başa dön](#match-conditions)

</br>

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Content Delivery Network genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-standard-rules-engine-reference.md)
- [Kural altyapısı eylemleri](cdn-standard-rules-engine-actions.md)
- [Standart kurallar altyapısını kullanarak HTTPS 'yi zorla](cdn-standard-rules-engine.md)
