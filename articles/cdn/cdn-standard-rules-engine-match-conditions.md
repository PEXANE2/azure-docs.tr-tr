---
title: Azure CDN için Standart kurallar altyapısındaki koşulları eşleştirin | Microsoft Dokümanlar
description: Azure İçerik Dağıtım Ağı (Azure CDN) için Standart kurallar altyapısındaki eşleşme koşulları için başvuru belgeleri.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: allensu
ms.openlocfilehash: b8050b973027ac91ede0ba98f4d1c76831da9828
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259944"
---
# <a name="match-conditions-in-the-standard-rules-engine-for-azure-cdn"></a>Azure CDN için Standart kurallar altyapısındaki koşulları eşleştirin

Azure İçerik Teslim Ağı (Azure CDN) için [Standart kurallar altyapısında,](cdn-standard-rules-engine.md) bir kural bir veya daha fazla eşleşme koşulu ve bir eylemden oluşur. Bu makalede, Azure CDN için Standart kurallar altyapısında kullanabileceğiniz eşler koşullarının ayrıntılı açıklamaları yer almaktadır.

Kuralın ilk bölümü bir eşleşme koşulu veya eşleşme koşulları kümesidir. Azure CDN için Standart kurallar altyapısında, her kuralın en fazla dört eşleşme koşulu olabilir. Eşmatch koşulu, tanımlanmış eylemlerin gerçekleştirildiği belirli istek türlerini tanımlar. Birden çok eşleşme koşulu kullanıyorsanız, eşleşme koşulları AND mantığı kullanılarak birlikte gruplandırılır.

Örneğin, bir eşleşme koşulu kullanabilirsiniz:

- İstekleri belirli bir IP adresine, ülkeye veya bölgeye göre filtreleyin.
- Üstbilgi bilgilerine göre istekleri filtreleyin.
- Mobil aygıtlardan veya masaüstü aygıtlardan gelen istekleri filtreleyin.

## <a name="match-conditions"></a>Maç koşulları

Aşağıdaki eşleşme koşulları, Azure CDN için Standart kurallar altyapısında kullanılabilir. 

### <a name="device-type"></a>Cihaz Türü 

Mobil cihazdan veya masaüstü aygıttan gelen istekleri tanımlar.  

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Desteklenen değerler
---------|----------------
Eşittir, Eşit Değil | Mobil, Masaüstü

### <a name="http-version"></a>HTTP sürümü

İstekhttp sürümüne dayalı istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Desteklenen değerler
---------|----------------
Eşittir, Eşit Değil | 2.0, 1.1, 1.0, 0.9, Tümü

### <a name="request-cookies"></a>Çerezleri iste

Gelen istekteki çerez bilgilerine dayalı istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Tanımlama bilgisi adı | İşleç | Çerez değeri | Büyük/küçük harf dönüşümü
------------|----------|--------------|---------------
Dize | [Standart operatör listesi](#standard-operator-list) | Dize, Int | Dönüşüm yok, büyük harfe, küçük harfe

#### <a name="key-information"></a>Önemli bilgiler

- Bir çerez adı belirttiğiniz zaman joker karakter\*değerlerini (yıldız işaretleri dahil ) )) kullanamazsınız; tam bir çerez adı kullanmanız gerekir.
- Bu eşleşme koşulunun örneğine göre yalnızca tek bir çerez adı belirtebilirsiniz.
- Çerez adı karşılaştırmaları büyük/küçük harf duyarsızdır.
- Birden çok çerez değeri belirtmek için, her çerez değeri arasında tek bir boşluk kullanın. 
- Çerez değerleri joker karakter değerlerinden yararlanabilir.
- Joker karakter değeri belirtilmemişse, yalnızca tam bir eşleşme bu eşleşme koşulunu karşılar. Örneğin, "Değer" "Değer" ile eşleşir, ancak "Value1" ile eşleşmez. 

### <a name="post-argument"></a>Sonrası bağımsız değişken

İstekte kullanılan POST isteği yöntemi için tanımlanan bağımsız değişkenleri temel alan istekleri tanımlar. 

#### <a name="required-fields"></a>Gerekli alanlar

Bağımsız değişken adı | İşleç | Bağımsız değişken değeri | Büyük/küçük harf dönüşümü
--------------|----------|----------------|---------------
Dize | [Standart operatör listesi](#standard-operator-list) | Dize, Int | Dönüşüm yok, büyük harfe, küçük harfe

### <a name="query-string"></a>Sorgu dizesi

Belirli bir sorgu dize parametresi içeren istekleri tanımlar. Bu parametre, belirli bir desenle eşleşen bir değere ayarlanır. İstek URL'sinde sorgu dize parametreleri (örneğin, **parametre=değer)** bu koşulun karşılanıp karşılanmadığını belirler. Bu eşleme koşulu, bir sorgu dize parametresini adına göre tanımlar ve parametre değeri için bir veya daha fazla değer kabul eder.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Sorgu dizesi | Büyük/Küçük Harf Dönüşümü
---------|--------------|---------------
[Standart operatör listesi](#standard-operator-list) | Dize, Int | Dönüşüm yok, büyük harfe, küçük harfe

### <a name="remote-address"></a>Uzak adres

İsteklinin konumuna veya IP adresine göre istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Desteklenen değerler
---------|-----------------
Herhangi biri | Yok
Geo Maç | Ülke kodu
IP Maç | IP adresi (boşluk ayrılmış)
Herhangi bir | Yok
Değil Geo Match | Ülke kodu
IP Eşleşmesi Değil | IP adresi (boşluk ayrılmış)

#### <a name="key-information"></a>Önemli bilgiler

- CIDR gösterimini kullanın.
- Birden çok IP adresi ve IP adresi bloğu belirtmek için, değerler arasında tek bir boşluk kullanın:
  - **IPv4 örneği**: *1.2.3.4 10.20.30.40* adresten gelen isteklerle 1.2.3.4 veya 10.20.30.40 ile eşleşir.
  - **IPv6 örneği**: *1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80* her iki adresten gelen isteklerle eşleşir 1:2:3:4:5:6:7:8 veya 10:20:30:40:50:60:70:80.
- IP adresi bloğu sözdizimi, bir ileri eğik çizgi ve önek boyutu ardından temel IP adresidir. Örneğin:
  - **IPv4 örneği**: *5.5.5.64/26, 5.5.5.64* ile 5.5.5.127 adreslerinden gelen tüm isteklerle eşleşir.
  - **IPv6 örneği**: *1:2:3:/48* adreslerinden gelen tüm isteklerle eşleşir 1:2:3:0:0:0:0 0 ile 1:2:3:ffff:ffff:ffff:ffff:ffff.ffff.

### <a name="request-body"></a>İstek gövdesi

İstek gövdesinde görünen belirli metne göre istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | İstek gövdesi | Büyük/küçük harf dönüşümü
---------|--------------|---------------
[Standart operatör listesi](#standard-operator-list) | Dize, Int | Dönüşüm yok, büyük harfe, küçük harfe

### <a name="request-header"></a>İstek üst bilgisi

İstekte belirli bir üstbilgi kullanan istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Üst bilgi adı | İşleç | Üst bilgi değeri | Büyük/küçük harf dönüşümü
------------|----------|--------------|---------------
Dize | [Standart operatör listesi](#standard-operator-list) | Dize, Int | Dönüşüm yok, büyük harfe, küçük harfe

### <a name="request-method"></a>İstek yöntemi

Belirtilen istek yöntemini kullanan istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Desteklenen değerler
---------|----------------
Eşittir, Eşit Değil | GET, POST, PUT, DELETE, HEAD, OPTIONS, TRACE

#### <a name="key-information"></a>Önemli bilgiler

- Azure CDN'de önbelleğe alınmış içeriği yalnızca GET isteği yöntemi oluşturabilir. Diğer tüm istek yöntemleri ağ üzerinden tamamlanır. 

### <a name="request-protocol"></a>İstek protokolü

Kullanılan belirtilen protokolü kullanan istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Desteklenen değerler
---------|----------------
Eşittir, Eşit Değil | HTTP, HTTPS

### <a name="request-url"></a>İstek URL'si

Belirtilen URL ile eşleşen istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | İstek URL'si | Büyük/küçük harf dönüşümü
---------|-------------|---------------
[Standart operatör listesi](#standard-operator-list) | Dize, Int | Dönüşüm yok, büyük harfe, küçük harfe

#### <a name="key-information"></a>Önemli bilgiler

- Bu kural koşulunu kullandığınızda, protokol bilgilerini içerdiğinden emin olun. Örneğin: *https://www.\<yourdomain\>.com*.

### <a name="url-file-extension"></a>URL dosya uzantısı

İstenen URL'de dosya adında belirtilen dosya uzantısını içeren istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Dahili numara | Büyük/küçük harf dönüşümü
---------|-----------|---------------
[Standart operatör listesi](#standard-operator-list) | Dize, Int | Dönüşüm yok, büyük harfe, küçük harfe

#### <a name="key-information"></a>Önemli bilgiler

- Uzatma için, bir satır aralığı içermez; örneğin, *.html*yerine *html* kullanın.

### <a name="url-file-name"></a>URL dosya adı

İstenen URL'de belirtilen dosya adını içeren istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Dosya adı | Büyük/küçük harf dönüşümü
---------|-----------|---------------
[Standart operatör listesi](#standard-operator-list) | Dize, Int | Dönüşüm yok, büyük harfe, küçük harfe

#### <a name="key-information"></a>Önemli bilgiler

- Birden çok dosya adı belirtmek için, her dosya adını tek bir boşlukla ayırın. 

### <a name="url-path"></a>URL yolu

İstenen URL'de belirtilen yolu içeren istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Değer | Büyük/Küçük Harf Dönüşümü
---------|-------|---------------
[Standart operatör listesi](#standard-operator-list) | Dize, Int | Dönüşüm yok, büyük harfe, küçük harfe

#### <a name="key-information"></a>Önemli bilgiler

- Dosya adı değeri joker karakter değerlerinden yararlanabilir. Örneğin, her dosya adı deseni, her yıldız işaretinin bir veya daha fazla karakterden oluşan bir diziyle eşleştiği bir veya daha fazla yıldız işaretinden (*) oluşabilir.

## <a name="reference-for-rules-engine-match-conditions"></a>Kurallar motoru eşleştirme koşulları için başvuru

### <a name="standard-operator-list"></a>Standart operatör listesi

Standart operatör listesinden değerleri kabul eden kurallar için aşağıdaki işleçler geçerlidir:

- Herhangi biri
- Eşittir 
- Contains 
- Ile başlar 
- Ile biter 
- Küçüktür
- Daha az veya eşittir
- Büyüktür
- Büyük veya eşittir
- Herhangi bir
- İçermez
- Ile başlamaz 
- Ile bitmez 
- En az
- En az veya eşit
- Daha büyük değil
- Büyük veya eşit olmayan

*Daha Az* ve Büyük veya eşittir gibi sayısal işleçler *için*kullanılan karşılaştırma uzunluğuna dayanır. Bu durumda, eşleşme durumundaki değer, karşılaştırmak istediğiniz uzunluğa eşit bir tamsayı olmalıdır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CDN'ye genel bakış](cdn-overview.md)
- [Standart kural altyapısı başvurusu](cdn-standard-rules-engine-reference.md)
- [Standart kurallar altyapısındaki eylemler](cdn-standard-rules-engine-actions.md)
- [Standart kural altyapısını kullanarak HTTPS'yi zorlama](cdn-standard-rules-engine.md)
