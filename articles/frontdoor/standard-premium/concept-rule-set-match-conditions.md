---
title: Azure ön kapısının Standart/Premium kural kümesi eşleştirme koşullarını yapılandırma
description: Bu makalede, Azure ön kapısı Standart/Premium kural kümesi ile kullanılabilen çeşitli eşleşme koşullarının bir listesi sunulmaktadır.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: 4c65d0e7f80fab59ca7df4849df7117d482352c1
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101100515"
---
# <a name="azure-front-door-standardpremium-preview-rule-set-match-conditions"></a>Azure ön kapı Standart/Premium (Önizleme) kural kümesi eşleşme koşulları

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Bu öğreticide, Azure portal ilk kural kümesiyle nasıl bir kural kümesi oluşturacağınız gösterilmektedir. Azure ön kapısının Standart/Premium [kural kümesinde](concept-rule-set.md), bir kural sıfır veya daha fazla eşleşme koşulu ve bir eylemden oluşur. Bu makalede, Azure ön kapısı Standart/Premium kural kümesinde kullanabileceğiniz eşleştirme koşullarına ilişkin ayrıntılı açıklamalar sağlanmaktadır.

Bir kuralın ilk bölümü eşleşme koşullarıdır veya eşleştirme koşulları kümesidir. Bir kural, en fazla 10 eşleşme koşullarından oluşabilir. Bir eşleşme koşulu, tanımlı eylemlerin yapıldığı belirli istek türlerini tanımlar. Birden çok eşleşme koşulu kullanırsanız, eşleşme koşulları ve Logic kullanılarak birlikte gruplandırılır. Birden çok değeri ("boşlukla ayrılmış" olarak belirtilen) destekleyen tüm eşleşme koşulları için "OR" işleci kabul edilir.

Örneğin, bir eşleşme koşulunu şu şekilde kullanabilirsiniz:

* İstekleri belirli bir IP adresine, ülkeye veya bölgeye göre filtreleyin.
* İstekleri üst bilgi bilgisine göre filtreleyin.
* Mobil cihazlardan veya masaüstü cihazlardan gelen istekleri filtreleyin.
* İstek dosya adı ve dosya uzantısından gelen istekleri filtreleyin.
* İstek URL 'SI, protokol, yol, sorgu dizesi, post args, vb. için istekleri filtreleyin.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Aşağıdaki eşleşme koşulları, Azure ön kapı Standart/Premium kuralları kümesi için kullanılabilir:

## <a name="device-type"></a>Cihaz Türü

Bir mobil cihazdan veya masaüstü cihazdan yapılan istekleri tanımlar.  

#### <a name="required-fields"></a>Gerekli alanlar

Operatör | Desteklenen değerler
---------|----------------
Eşittir, eşit değildir | Mobil, masaüstü

## <a name="post-argument"></a>Post bağımsız değişkeni

İstekte kullanılan POST isteği yöntemi için tanımlanan bağımsız değişkenlere göre istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Bağımsız değişken adı | Operatör | Bağımsız değişken değeri | Örnek dönüştürme
--------------|----------|----------------|---------------
Dize | [İşleç listesi](#operator-list) | String, Int | Küçük harf, büyük harf

## <a name="query-string"></a>Sorgu dizesi

Belirli bir sorgu dizesi parametresi içeren istekleri tanımlar. Bu parametre, belirli bir düzenle eşleşen bir değere ayarlanır. İstek URL 'sindeki sorgu dizesi parametreleri (örneğin, **Parameter = değer**) bu koşulun karşılanıp karşılanmadığını belirtir. Bu eşleştirme koşulu, bir sorgu dizesi parametresini adına göre tanımlar ve parametre değeri için bir veya daha fazla değeri kabul eder.

#### <a name="required-fields"></a>Gerekli alanlar

Operatör | Sorgu dizesi | Örnek dönüştürme
---------|--------------|---------------
[İşleç listesi](#operator-list) | String, Int | Küçük harf, büyük harf

## <a name="remote-address"></a>Uzak adres

İstekleri isteyanın konumuna veya IP adresine göre tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Operatör | Desteklenen değerler
---------|-----------------
Coğrafi eşleşme | Ülke kodu
IP eşleşmesi | IP adresi (boşlukla ayrılmış)
Coğrafi eşleşme değil | Ülke kodu
IP eşleşmesi değil | IP adresi (boşlukla ayrılmış)

#### <a name="key-information"></a>Önemli bilgiler

* CıDR gösterimini kullanın.
* Birden çok IP adresi ve IP adresi blokları için, ' veya ' mantığı çalıştırılır.
    * **IPv4 örneği**: *1.2.3.4* ve *10.20.30.40* iki IP adresini eklerseniz, bu durum, 1.2.3.4 veya 10.20.30.40 adreslerinden gelen herhangi bir istek varsa, koşul eşleştirilir.
    * **IPv6 örneği**: *1:2:3:4:5:6:7:8* ve *10:20:30:40:50:60:70:80* olan iki IP adresini eklerseniz, bu koşul, 1:2:3:4:5:6:7:8 veya 10:20:30:40:50:60:70:80 adresinden gelen istekler varsa eşleştirilir.
* Bir IP adres bloğunun sözdizimi, ardından bir eğik çizgi ve ön ek boyutu gelen temel IP adresidir. Örneğin:
    * **IPv4 örneği**: *5.5.5.64/26* , 5.5.5.64 ile 5.5.5.127 arası adreslerden gelen isteklerle eşleşir.
    * **IPv6 örneği**: *1:2:3:/48* , 1:2:3: ffff: ffff: ffff: ffff: ffff arasındaki 1:2:3:0:0:0:0:0 adresten gelen isteklerden eşleşir.

## <a name="request-body"></a>İstek gövdesi

İstek gövdesinde görüntülenen belirli bir metne göre istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Operatör | İstek gövdesi | Örnek dönüştürme
---------|--------------|---------------
[İşleç listesi](#operator-list) | String, Int | Küçük harf, büyük harf

## <a name="request-header"></a>İstek üst bilgisi

İstekte belirli bir üst bilgiyi kullanan istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Üst bilgi adı | Operatör | Üst bilgi değeri | Örnek dönüştürme
------------|----------|--------------|---------------
Dize | [İşleç listesi](#operator-list) | String, Int | Küçük harf, büyük harf

## <a name="request-method"></a>İstek yöntemi

Belirtilen istek yöntemini kullanan istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Operatör | Desteklenen değerler
---------|----------------
Eşittir, eşit değildir | AL, POSTALA, KOY, SIL, HEAD, SEÇENEKLER, IZLE

#### <a name="key-information"></a>Önemli bilgiler

Yalnızca GET isteği yöntemi Azure ön kapıda önbelleğe alınmış içerik oluşturabilir. Diğer tüm istek yöntemleri ağ üzerinden proxy olarak kullanılır.

## <a name="request-protocol"></a>İstek Protokolü

Kullanılan belirtilen protokolü kullanan istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Operatör | Desteklenen değerler
---------|----------------
Eşittir, eşit değildir | HTTP, HTTPS

## <a name="request-url"></a>İstek URL’si

Belirtilen URL ile eşleşen istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Operatör | İstek URL’si | Örnek dönüştürme
---------|-------------|---------------
[İşleç listesi](#operator-list) | String, Int | Küçük harf, büyük harf

#### <a name="key-information"></a>Önemli bilgiler

Bu kural koşulunu kullandığınızda protokol bilgilerini eklediğinizden emin olun. Örneğin: *https://www . \<yourdomain\> . com*.

## <a name="request-file-extension"></a>İstek dosyası uzantısı

İstenen URL 'deki dosya adında belirtilen dosya uzantısını içeren istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Operatör | Dahili numara | Örnek dönüştürme
---------|-----------|---------------
[İşleç listesi](#operator-list)  | String, Int | Küçük harf, büyük harf

#### <a name="key-information"></a>Önemli bilgiler

Uzantı için, önde gelen bir nokta eklemeyin; Örneğin, *. html* yerine *HTML* kullanın.

## <a name="request-file-name"></a>İstek dosyası adı

İstenen URL 'de belirtilen dosya adını içeren istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

Operatör | Dosya adı | Örnek dönüştürme
---------|-----------|---------------
[İşleç listesi](#operator-list)| String, Int | Küçük harf, büyük harf

## <a name="request-path"></a>İstek yolu

İstek URL 'sinde belirtilen yolu içeren istekleri tanımlar.

#### <a name="required-fields"></a>Gerekli alanlar

İşleç | Değer | Örnek dönüştürme
---------|-------|---------------
[İşleç listesi](#operator-list) | String, Int | Küçük harf, büyük harf

## <a name="operator-list"></a><a name = "operator-list"></a>İşleç listesi

Standart işleç listesinden değerleri kabul eden kurallar için aşağıdaki işleçler geçerlidir:

* Herhangi bir
* Eşittir
* Contains
* Şununla başlar
* Şununla biter
* Küçüktür
* Küçüktür veya eşittir
* Büyüktür
* Büyük veya eşittir
* Hiçbirini değil
* İçermez
* Şununla başlamaktadır
* Şununla bitmiyor
* Küçüktür
* Küçüktür veya eşittir
* Şundan büyük değil
* Büyük veya eşit değil
* Normal ifade

*Küçüktür ve* *büyüktür* gibi sayısal işleçler için, kullanılan karşılaştırma uzunluğa göre belirlenir. Match koşulunun değeri, karşılaştırmak istediğiniz uzunluğa eşit bir tamsayı olmalıdır.

## <a name="regular-expression"></a>Normal ifade

Regex aşağıdaki işlemleri desteklemez:

* Geri başvurular ve yakalama alt ifadeleri
* Rastgele sıfır Genişlik onaylamaları
* Alt yordam başvuruları ve özyinelemeli desenler
* Koşullu desenler
* Geri izleme denetim fiilleri
* \C Single-Byte yönergesi
* \R yeni satır eşleştirme yönergesi
* Eşleşme sıfırlama yönergesinin \K başlangıcı
* Belirtme çizgileri ve gömülü kod
* Atomik gruplandırma ve sahip nicelik belirteçleri

## <a name="next-steps"></a>Sonraki adımlar

* [Kural kümesi](concept-rule-set.md)hakkında daha fazla bilgi edinin.
* [Ilk kurallar kümesini nasıl yapılandıracağınızı](how-to-configure-rule-set.md)öğrenin.
* [Kural kümesi eylemleri](concept-rule-set-actions.md)hakkında daha fazla bilgi edinin.
