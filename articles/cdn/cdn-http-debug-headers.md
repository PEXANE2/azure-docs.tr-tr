---
title: X-EC-Azure CDN kuralları altyapısı için HTTP üstbilgilerini hata ayıkla | Microsoft Docs
description: X-EC-Debug hata ayıklama önbelleği istek üst bilgisi, istenen varlığa uygulanan önbellek ilkesi hakkında ek bilgiler sağlar. Bu üstbilgiler Verizon 'e özgüdür.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: allensu
ms.openlocfilehash: 52aae3bdd2fe82eea6cbd500723192c88c293a1e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81260505"
---
# <a name="x-ec-debug-http-headers-for-azure-cdn-rules-engine"></a>X-EC-Azure CDN kuralları altyapısı için HTTP üstbilgilerini hata ayıkla
Hata ayıklama önbelleği istek üst bilgisi `X-EC-Debug`, istenen varlığa uygulanan önbellek ilkesi hakkında ek bilgiler sağlar. Bu üst bilgiler Verizon ürünlerden **Azure CDN Premium** 'a özgüdür.

## <a name="usage"></a>Kullanım
POP sunucularından bir kullanıcıya gönderilen yanıt yalnızca aşağıdaki koşullar karşılandığında `X-EC-Debug` üstbilgiyi içerir:

- Belirtilen istek için kural altyapısında [hata ayıklama önbelleği yanıt üstbilgileri özelliği](cdn-verizon-premium-rules-engine-reference-features.md#debug-cache-response-headers) etkinleştirildi.
- Belirtilen istek, yanıta dahil edilecek hata ayıklama önbelleği yanıt üst bilgileri kümesini tanımlar.

## <a name="requesting-debug-cache-information"></a>Hata ayıklama önbelleği bilgilerini isteme
Yanıta dahil edilecek hata ayıklama önbelleği bilgilerini tanımlamak için belirtilen istekteki aşağıdaki yönergeleri kullanın:

İstek üst bilgisi | Açıklama |
---------------|-------------|
X-EC-hata ayıklama: x-EC-önbellek | [Önbellek durum kodu](#cache-status-code-information)
X-EC-hata ayıklama: x-EC-Cache-Remote | [Önbellek durum kodu](#cache-status-code-information)
X-EC-hata ayıkla: x-EC-Check-önbelleklenebilir | [Önbelleğe alınabilir öğeleri](#cacheable-response-header)
X-EC-hata ayıkla: x-EC-Cache-Key | [Cache-Key](#cache-key-response-header)
X-EC-hata ayıklama: x-EC-Cache-State | [Önbellek durumu](#cache-state-response-header)

### <a name="syntax"></a>Sözdizimi

Aşağıdaki üst bilgi ve istekte belirtilen yönergeler eklenerek hata ayıklama önbelleği yanıt üstbilgileri istenebilir:

`X-EC-Debug: Directive1,Directive2,DirectiveN`

### <a name="sample-x-ec-debug-header"></a>Örnek X-EC-hata ayıklama üstbilgisi

`X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state`

## <a name="cache-status-code-information"></a>Önbellek durum kodu bilgileri
X-EC-Debug yanıt üst bilgisi, bir sunucuyu ve aşağıdaki yönergeler aracılığıyla yanıtı nasıl ele alındığını tanımlayabilir:

Üst bilgi | Açıklama
-------|------------
X-EC-hata ayıklama: x-EC-önbellek | Bu üst bilgi, içerik CDN aracılığıyla her yönlendirildiğinde raporlanır. İsteği yerine getirdi POP sunucusunu tanımlar.
X-EC-hata ayıklama: x-EC-Cache-Remote | Bu üst bilgi, yalnızca istenen içerik bir kaynak kalkan sunucusunda veya bir ADN Ağ Geçidi sunucusunda önbelleğe alındığında bildirilir.

### <a name="response-header-format"></a>Yanıt üst bilgisi biçimi

X-EC-Debug üstbilgisi, önbellek durum kodu bilgilerini aşağıdaki biçimde bildirir:

- `X-EC-Debug: x-ec-cache: <StatusCode from Platform (POP/ID)>`

- `X-EC-Debug: x-ec-cache-remote: <StatusCode from Platform (POP/ID)>`

Yukarıdaki yanıt üst bilgisi sözdiziminde kullanılan terimler aşağıdaki gibi tanımlanır:
- StatusCode: istenen içeriğin, bir önbellek durum kodu ile temsil edilen CDN tarafından nasıl işlendiğini gösterir.
    
    Belirteç tabanlı kimlik doğrulaması nedeniyle yetkisiz bir istek reddedildiğinde, TCP_DENIED durum kodu NONE yerine bildirilebilir. Ancak, önbellek durum raporları veya ham günlük verileri görüntülenirken, NONE durum kodu çalışmaya devam edecektir.

- Platform: içeriğin istendiği platformu gösterir. Bu alan için aşağıdaki kodlar geçerlidir:

    Kod  | Platform
    ------| --------
    ECAcc | HTTP büyük
    ECS   | HTTP küçük
    ECD   | Uygulama Teslim Ağı (ADN)

- POP: isteği işlenen [pop 'U](cdn-pop-abbreviations.md) gösterir. 

### <a name="sample-response-headers"></a>Örnek yanıt üstbilgileri

Aşağıdaki örnek üst bilgiler bir istek için önbellek durum kodu bilgilerini sağlar:

- `X-EC-Debug: x-ec-cache: TCP_HIT from ECD (lga/0FE8)`

- `X-EC-Debug: x-ec-cache-remote: TCP_HIT from ECD (dca/EF00)`

## <a name="cacheable-response-header"></a>Önbelleklenebilir yanıt üst bilgisi
`X-EC-Debug: x-ec-check-cacheable` Yanıt üst bilgisi, istenen içeriğin önbelleğe alınıp alınmayacağını belirtir.

Bu yanıt üst bilgisi, önbelleğe almanın yapılıp yapılmayacağını göstermez. Bunun yerine, isteğin önbelleğe alma için uygun olup olmadığını gösterir.

### <a name="response-header-format"></a>Yanıt üst bilgisi biçimi

`X-EC-Debug` Yanıt üst bilgisi, bir isteğin önbelleğe alınıp alınmayacağını bildiren aşağıdaki biçimdedir:

`X-EC-Debug: x-ec-check-cacheable: <cacheable status>`

Yukarıdaki yanıt üst bilgisi sözdiziminde kullanılan terim aşağıdaki gibi tanımlanır:

Değer  | Açıklama
-------| --------
EVET    | İstenen içeriğin önbelleğe alma için uygun olduğunu gösterir.
NO     | İstenen içeriğin önbelleğe alma için uygun olmadığını gösterir. Bu durumun nedeni aşağıdakilerden biri olabilir: <br /> -Müşteriye özgü yapılandırma: hesabınıza özgü bir yapılandırma, pop sunucularının bir varlığı önbelleğe almasını engelleyebilir. Örneğin, kural altyapısı, uygun istekler için önbelleği atlama özelliğini etkinleştirerek bir varlığın önbelleğe alınmasını önleyebilir.<br /> -Önbellek yanıtı üstbilgileri: istenen varlığın Cache-Control ve Expires üstbilgileri, POP sunucularının onu önbelleğe almasını önleyebilir.
BILINMEYEN | Sunucuların istenen varlığın önbelleklenebilir olup olmadığını değerlendiremediğini belirtir. Bu durum genellikle, belirteç tabanlı kimlik doğrulaması nedeniyle istek reddedildiğinde oluşur.

### <a name="sample-response-header"></a>Örnek yanıt üst bilgisi

Aşağıdaki örnek yanıt üst bilgisi, istenen içeriğin önbelleğe alınıp alınmayacağını gösterir:

`X-EC-Debug: x-ec-check-cacheable: YES`

## <a name="cache-key-response-header"></a>Cache-Key yanıt üst bilgisi
`X-EC-Debug: x-ec-cache-key` Yanıt üst bilgisi, istenen içerikle ilişkili fiziksel önbellek anahtarını gösterir. Fiziksel önbellek anahtarı, önbelleğe alma amacıyla bir varlığı tanımlayan bir yoldan oluşur. Diğer bir deyişle, sunucular, bir varlığın önbelleğe alınmış bir sürümünü kendi önbelleğine göre kendi yoluna göre kontrol eder.

Bu fiziksel önbellek anahtarı, bir çift eğik çizgi (//) ve ardından içerik istemek için kullanılan protokol (HTTP veya HTTPS) ile başlar. Bu protokolün ardından, içerik erişim noktasıyla (örneğin, _/000001/_) başlayan istenen varlığın göreli yolu gelir.

Varsayılan olarak, HTTP platformları *Standart önbellek*kullanacak şekilde yapılandırılmıştır. Bu, sorgu dizelerinin önbelleğe alma mekanizması tarafından yoksayılacağı anlamına gelir. Bu tür bir yapılandırma, Cache-Key ' in sorgu dizesi verilerini dahil etmesini engeller.

Bir sorgu dizesi önbellek anahtarına kaydedilirse, bu, karma eşdeğerine dönüştürülür ve ardından istenen varlığın adı ve dosya uzantısı arasına eklenir (örneğin, varlık&lt;karma değeri&gt;. html).

### <a name="response-header-format"></a>Yanıt üst bilgisi biçimi

Yanıt `X-EC-Debug` üst bilgisi, fiziksel önbellek anahtarı bilgilerini aşağıdaki biçimde bildirir:

`X-EC-Debug: x-ec-cache-key: CacheKey`

### <a name="sample-response-header"></a>Örnek yanıt üst bilgisi

Aşağıdaki örnek yanıt üst bilgisi, istenen içerik için fiziksel önbellek anahtarını gösterir:

`X-EC-Debug: x-ec-cache-key: //http/800001/origin/images/foo.jpg`

## <a name="cache-state-response-header"></a>Önbellek durumu yanıt üst bilgisi
`X-EC-Debug: x-ec-cache-state` Yanıt üst bilgisi, istenen içeriğin istek yaptığı sırada önbellek durumunu gösterir.

### <a name="response-header-format"></a>Yanıt üst bilgisi biçimi

`X-EC-Debug` Yanıt üst bilgisi, önbellek durumu bilgilerini aşağıdaki biçimde bildirir:

`X-EC-Debug: x-ec-cache-state: max-age=MASeconds (MATimePeriod); cache-ts=UnixTime (ddd, dd MMM yyyy HH:mm:ss GMT); cache-age=CASeconds (CATimePeriod); remaining-ttl=RTSeconds (RTTimePeriod); expires-delta=ExpiresSeconds`

Yukarıdaki yanıt üst bilgisi sözdiziminde kullanılan terimler aşağıdaki gibi tanımlanır:

- MASeconds: istenen içeriğin Cache-Control üstbilgileri tarafından tanımlanan maksimum yaşı (saniye cinsinden) belirtir.

- MATimePeriod: maksimum yaş değerini (yani, Masaniye) daha büyük bir birimin (örneğin, günler) yaklaşık eşdeğerine dönüştürür. 

- UnixTime: istenen içeriğin (POSIX saati veya UNIX dönemi olarak da bilinir) önbellekte zaman damgasını gösterir. Önbellek zaman damgası, bir varlığın TTL hesaplanacak başlangıç tarihini/saati gösterir. 

    Kaynak sunucu bir üçüncü taraf HTTP önbelleğe alma sunucusu kullanmıyorsa veya bu sunucu yaş yanıt üst bilgisini döndürmezse, bu durumda önbellek zaman damgası her zaman varlığın alındığı veya yeniden doğrulandığı tarih/saat olur. Aksi halde, POP sunucuları varlığın TTL 'sini şu şekilde hesaplamak için Age alanını kullanır: alma/RevalidateDateTime-Age.

- ddd, gg MMM yyyy HH: mm: ss GMT: istenen içeriğin önbellek zaman damgasını gösterir. Daha fazla bilgi için lütfen yukarıdaki UnixTime terimine bakın.

- CASeconds: önbellek zaman damgasından bu yana geçen saniye sayısını belirtir.

- RTSeconds: önbelleğe alınan içeriğin yeni kabul edileceği saniye sayısını gösterir. Bu değer şu şekilde hesaplanır: RTSeconds = maksimum-yaş-önbellek yaşı.

- RTTimePeriod: kalan TTL değerini (yani RTSeconds) daha büyük bir birimin yaklaşık eşdeğerine (örneğin, günler) dönüştürür.

- ExpiresSeconds: `Expires` yanıt üstbilgisinde belirtilen tarih/saatten önce kalan saniye sayısını gösterir. `Expires` Yanıt üst bilgisi yanıta dahil edilmediği takdirde bu terimin değeri *none*olur.

### <a name="sample-response-header"></a>Örnek yanıt üst bilgisi

Aşağıdaki örnek yanıt üst bilgisi, istenen içeriğin istek yaptığı sırada önbellek durumunu gösterir:

```X-EC-Debug: x-ec-cache-state: max-age=604800 (7d); cache-ts=1341802519 (Mon, 09 Jul 2012 02:55:19 GMT); cache-age=0 (0s); remaining-ttl=604800 (7d); expires-delta=none```

