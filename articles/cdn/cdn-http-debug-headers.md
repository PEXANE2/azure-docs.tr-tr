---
title: Azure CDN kuralları altyapısı için X-EC Hata Ayıklama HTTP üstbilgisi | Microsoft Dokümanlar
description: X-EC hata ayıklama önbelleği önbilgisi üstbilgisi, istenen varlığa uygulanan önbellek ilkesi hakkında ek bilgiler sağlar. Bu başlıklar Verizon'a özgü.
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
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260505"
---
# <a name="x-ec-debug-http-headers-for-azure-cdn-rules-engine"></a>Azure CDN kuralları altyapısı için X-EC Hata Ayıklama HTTP üstbilgisi
Hata ayıklama önbellek istek `X-EC-Debug`üstbilgisi, istenen varlığa uygulanan önbellek ilkesi hakkında ek bilgiler sağlar. Bu başlıklar, **Verizon ürünlerinden Azure CDN Premium'a** özeldir.

## <a name="usage"></a>Kullanım
POP sunucularından bir kullanıcıya gönderilen `X-EC-Debug` yanıt, üstbilgiyi yalnızca aşağıdaki koşullar yerine getirildiğinde içerir:

- Hata [Ayıklama Önbellek Yanıt Üstbilgi özelliği,](cdn-verizon-premium-rules-engine-reference-features.md#debug-cache-response-headers) belirtilen istek için kurallar altyapısında etkinleştirildi.
- Belirtilen istek, yanıta dahil edilecek hata ayıklama önbelleğe alım sayıltıcı üstbilgi kümesini tanımlar.

## <a name="requesting-debug-cache-information"></a>Hata ayıklama önbellek bilgilerini isteme
Yanıta dahil edilecek hata ayıklama önbelleğe alım sayıltıcı bilgilerini tanımlamak için belirtilen istekte aşağıdaki yönergeleri kullanın:

İstek üst bilgisi | Açıklama |
---------------|-------------|
X-EC-Hata Ayıklama: x-ec-önbellek | [Önbellek durum kodu](#cache-status-code-information)
X-EC-Hata Ayıklama: x-ec-önbellek-uzaktan | [Önbellek durum kodu](#cache-status-code-information)
X-EC-Hata Ayıklama: x-ec-check-önbelleğe | [Önbelleğe uygun](#cacheable-response-header)
X-EC-Hata Ayıklama: x-ec-önbellek anahtarı | [Önbellek anahtarı](#cache-key-response-header)
X-EC-Hata Ayıklama: x-ec-cache-state | [Önbellek durumu](#cache-state-response-header)

### <a name="syntax"></a>Sözdizimi

Hata ayıklama önbellek yanıt üstbilgiaşağıdaki üstbilgi ve istek te belirtilen yönergeleri ekleyerek istenebilir:

`X-EC-Debug: Directive1,Directive2,DirectiveN`

### <a name="sample-x-ec-debug-header"></a>Örnek X-EC-Hata Ayıklama üstbilgisi

`X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state`

## <a name="cache-status-code-information"></a>Önbellek durum kodu bilgileri
X-EC Hata Ayıklama yanıt üstbilgisi bir sunucuyu ve yanıtı aşağıdaki yönergeler aracılığıyla nasıl işleyebilir:

Üst bilgi | Açıklama
-------|------------
X-EC-Hata Ayıklama: x-ec-önbellek | Bu üstbilgi, içerik CDN üzerinden yönlendirildiğinde bildirilir. İsteği yerine getiren POP sunucusunu tanımlar.
X-EC-Hata Ayıklama: x-ec-önbellek-uzaktan | Bu üstbilgi yalnızca istenen içeriğin bir başlangıç kalkanı sunucusunda veya ADN ağ geçidi sunucusunda önbelleğe alındığunda bildirilir.

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

X-EC-Hata Ayıklama üstbilgisi önbellek durum kodu bilgilerini aşağıdaki biçimde bildirir:

- `X-EC-Debug: x-ec-cache: <StatusCode from Platform (POP/ID)>`

- `X-EC-Debug: x-ec-cache-remote: <StatusCode from Platform (POP/ID)>`

Yukarıdaki yanıt üstbilgi sözdiziminde kullanılan terimler aşağıdaki gibi tanımlanır:
- StatusCode: İstenen içeriğin önbellek durum kodu yla temsil edilen CDN tarafından nasıl işleneceğini gösterir.
    
    Token Tabanlı Kimlik Doğrulama nedeniyle yetkisiz bir istek reddedildiğinde, TCP_DENIED durum kodu YOK yerine raporlanabilir. Ancak, ÖNbellek Durum raporları veya ham günlük verileri görüntülendiğinde NONE durum kodu kullanılmaya devam eder.

- Platform: İçeriğin istendiği platformu gösterir. Aşağıdaki kodlar bu alan için geçerlidir:

    Kod  | Platform
    ------| --------
    ECAcc | HTTP Büyük
    Ecs   | HTTP Küçük
    Eçg   | Uygulama Dağıtım Ağı (ADN)

- POP: İsteği işleyen [POP'u](cdn-pop-abbreviations.md) gösterir. 

### <a name="sample-response-headers"></a>Örnek yanıt üstbilgi

Aşağıdaki örnek üstbilgiler, bir istek için önbellek durum kodu bilgilerini sağlar:

- `X-EC-Debug: x-ec-cache: TCP_HIT from ECD (lga/0FE8)`

- `X-EC-Debug: x-ec-cache-remote: TCP_HIT from ECD (dca/EF00)`

## <a name="cacheable-response-header"></a>Önbelleğe uygun yanıt üstbilgi
Yanıt `X-EC-Debug: x-ec-check-cacheable` üstbilgi, istenen içeriğin önbelleğe alınmış olup olmadığını gösterir.

Bu yanıt üstbilgi önbellek gerçekleşti olup olmadığını göstermez. Bunun yerine, isteğin önbelleğe almaya uygun olup olmadığını gösterir.

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

Bir `X-EC-Debug` isteğin önbelleğe alınmış olup olmadığını bildiren yanıt üstbilgisi aşağıdaki biçimdedir:

`X-EC-Debug: x-ec-check-cacheable: <cacheable status>`

Yukarıdaki yanıt üstbilgi sözdiziminde kullanılan terim aşağıdaki gibi tanımlanır:

Değer  | Açıklama
-------| --------
EVET    | İstenen içeriğin önbelleğe almaya uygun olduğunu gösterir.
NO     | İstenen içeriğin önbelleğe alma için uygun olmadığını gösterir. Bu durum, aşağıdaki nedenlerden biri nedeniyle olabilir: <br /> - Müşteriye Özel Yapılandırma: Hesabınıza özgü bir yapılandırma, pop sunucuların bir varlığı önbelleğe almalarını engelleyebilir. Örneğin, Kurallar Motoru, geçerli istekleri için Bypass Önbelleği özelliğini etkinleştirerek bir varlığın önbelleğe alınmasını engelleyebilir.<br /> - Önbellek Yanıt Üstbilgileri: İstenen varlığın Önbellek Denetimi ve Süresi Dolan üstbilgileri POP sunucularının önbelleğe almalarını engelleyebilir.
Bilinmeyen | Sunucuların istenen varlığın önbelleğe alınıp alınamayacağını değerlendiremediğini gösterir. Bu durum genellikle belirteç tabanlı kimlik doğrulaması nedeniyle istek reddedildiğinde oluşur.

### <a name="sample-response-header"></a>Örnek yanıt üstbilgi

Aşağıdaki örnek yanıt üstbilgi, istenen içeriğin önbelleğe alınıp alınmadığını gösterir:

`X-EC-Debug: x-ec-check-cacheable: YES`

## <a name="cache-key-response-header"></a>Önbellek Anahtarı yanıt üstbilgi
Yanıt `X-EC-Debug: x-ec-cache-key` üstbilgisi, istenen içerikle ilişkili fiziksel önbellek anahtarını gösterir. Fiziksel önbellek anahtarı önbelleğe alma amacıyla bir varlığı tanımlayan bir yol oluşur. Başka bir deyişle, sunucular önbellek anahtarı tarafından tanımlanan yoluna göre bir varlığın önbelleğe alınmış sürümünü denetler.

Bu fiziksel önbellek anahtarı, içeriği istemek için kullanılan protokolün (HTTP veya HTTPS) ardından çift ileri eğik çizgiyle (//) başlar. Bu protokolü, içerik erişim noktasıyla başlayan istenen varlığa giden göreli yol (örneğin, _/000001/ )._

Varsayılan olarak, HTTP platformları *standart önbellek*kullanacak şekilde yapılandırılır, bu da sorgu dizelerinin önbelleğe alma mekanizması tarafından yoksayıldığı anlamına gelir. Bu tür yapılandırma, önbellek anahtarının sorgu dize verileri eklemesini engeller.

Bir sorgu dizesi önbellek anahtarına kaydedilirse, karma eşdeğerine dönüştürülür ve sonra istenen varlığın adı ile dosya uzantısı arasına&lt;eklenir&gt;(örneğin, kıymet karma değeri .html).

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

`X-EC-Debug` Yanıt üstbilgisi fiziksel önbellek anahtarı bilgilerini aşağıdaki biçimde bildirir:

`X-EC-Debug: x-ec-cache-key: CacheKey`

### <a name="sample-response-header"></a>Örnek yanıt üstbilgi

Aşağıdaki örnek yanıt üstbilgisi istenen içeriğin fiziksel önbellek anahtarını gösterir:

`X-EC-Debug: x-ec-cache-key: //http/800001/origin/images/foo.jpg`

## <a name="cache-state-response-header"></a>Önbellek durumu yanıt üstbilgi
Yanıt `X-EC-Debug: x-ec-cache-state` üstbilgi, istenen içeriğin istendiği anda önbellek durumunu gösterir.

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

Yanıt `X-EC-Debug` üstbilgisi önbellek durumu bilgilerini aşağıdaki biçimde bildirir:

`X-EC-Debug: x-ec-cache-state: max-age=MASeconds (MATimePeriod); cache-ts=UnixTime (ddd, dd MMM yyyy HH:mm:ss GMT); cache-age=CASeconds (CATimePeriod); remaining-ttl=RTSeconds (RTTimePeriod); expires-delta=ExpiresSeconds`

Yukarıdaki yanıt üstbilgi sözdiziminde kullanılan terimler aşağıdaki gibi tanımlanır:

- MASeconds: İstenen içeriğin Önbellek Denetimi üstbilgilerinde tanımlandığı maksimum yaşı (saniye cinsinden) gösterir.

- MATimePeriod: Maksimum yaş değerini (örneğin, MASeconds) daha büyük bir birimin yaklaşık eşdeğerine (örneğin, günler) dönüştürür. 

- UnixTime: Unix zamanında istenen içeriğin önbellek zaman damgasını gösterir (POSIX zamanı veya Unix çağı olarak da bilinir). Önbellek zaman damgası, bir varlığın TTL'sinin hesaplanacağı başlangıç tarihini/saatini gösterir. 

    Kaynak sunucu üçüncü taraf bir HTTP önbelleğe alma sunucusu kullanmıyorsa veya bu sunucu Yaş yanıt üstbilgisini döndürmüyorsa, önbellek zaman damgası her zaman varlığın alındığı veya yeniden geçersiz kılındığı tarih/saat olacaktır. Aksi takdirde, POP sunucuları varlığın TTL'sini aşağıdaki gibi hesaplamak için Yaş alanını kullanır: Alma/Yeniden DoğrulamaDateTime - Yaş.

- ddd, dd MMM yyyy HH:mm:ss GMT: İstenen içeriğin önbellek zaman damgasını gösterir. Daha fazla bilgi için lütfen yukarıdaki UnixTime terimine bakın.

- CASeconds: Önbellek zaman damgası bu yana geçen saniye sayısını gösterir.

- RTSeconds: Önbelleğe alınmış içeriğin taze olarak kabul edileceği kalan saniye sayısını gösterir. Bu değer aşağıdaki gibi hesaplanır: RTSeconds = max-age - önbellek yaş.

- RTTimePeriod: Kalan TTL değerini (örneğin, RTSeconds) daha büyük bir birimin yaklaşık eşdeğerine (örneğin, günler) dönüştürür.

- Sona ErenSaniye: `Expires` Yanıt üstbilgisinde belirtilen tarih/saatten önce kalan saniye sayısını gösterir. `Expires` Yanıt üstbilgiyanıta dahil edilmediyse, bu terimin değeri *yok.*

### <a name="sample-response-header"></a>Örnek yanıt üstbilgi

Aşağıdaki örnek yanıt üstbilgi, istendiği anda istenen içeriğin önbellek durumunu gösterir:

```X-EC-Debug: x-ec-cache-state: max-age=604800 (7d); cache-ts=1341802519 (Mon, 09 Jul 2012 02:55:19 GMT); cache-age=0 (0s); remaining-ttl=604800 (7d); expires-delta=none```

