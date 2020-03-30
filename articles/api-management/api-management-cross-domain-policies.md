---
title: Azure API Yönetimi etki alanları arası ilkeler | Microsoft Dokümanlar
description: Azure API Yönetimi'nde kullanılmak üzere kullanılabilen çapraz etki alanı ilkeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 7689d277-8abe-472a-a78c-e6d4bd43455d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: b72abf4e208c57987375a105865046f194460058
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265993"
---
# <a name="api-management-cross-domain-policies"></a>API Management etki alanları arası ilkeler
Bu konu, aşağıdaki API Yönetimi ilkeleri için bir başvuru sağlar. İlkeler ekleme ve yapılandırma hakkında bilgi için [API Yönetimi'ndeki İlkeler'e](https://go.microsoft.com/fwlink/?LinkID=398186)bakın.

## <a name="cross-domain-policies"></a><a name="CrossDomainPolicies"></a>Etki alanı ilkeleri arası

- [Etki alanları arası çağrılara izin ver](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - API'yi Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirir.
- [CORS](api-management-cross-domain-policies.md#CORS) - Tarayıcı tabanlı istemcilerden gelen etki alanları arası çağrılara izin vermek için bir operasyona veya API'ye başlangıç lar arası kaynak paylaşımı (CORS) desteği ekler.
- [JSONP](api-management-cross-domain-policies.md#JSONP) - JavaScript tarayıcı tabanlı istemcilerden çapraz etki alanı çağrılarına izin vermek için bir operasyona veya API'ye dolgu (JSONP) desteği yle JSON ekler.

## <a name="allow-cross-domain-calls"></a><a name="AllowCrossDomainCalls"></a>Etki alanları arası çağrılara izin verme
API'yi Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirmek için `cross-domain` ilkeyi kullanın.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cross-domain>
    <!-Policy configuration is in the Adobe cross-domain policy file format,
        see https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->
</cross-domain>
```

### <a name="example"></a>Örnek

```xml
<cross-domain>
    <cross-domain>
        <allow-http-request-headers-from domain='*' headers='*' />
    </cross-domain>
</cross-domain>
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|çapraz etki alanı|Kök öğesi. Alt öğeler Adobe [etki alanı ilkesi dosya belirtimine](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html)uygun olmalıdır.|Evet|

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

- **İlke bölümleri:** gelen
- **İlke kapsamları:** tüm kapsamlar

## <a name="cors"></a><a name="CORS"></a>Cors
İlke, `cors` tarayıcı tabanlı istemcilerden gelen etki alanları arası çağrılara izin vermek için bir operasyona veya API'ye orijinler arası kaynak paylaşımı (CORS) desteği ekler.

CORS, bir tarayıcının ve sunucunun etkileşimkurmasına ve belirli çapraz orijin isteklerine izin verip vermemeye izin vermesini sağlar (örn. Bir web sayfasından diğer etki alanlarına JavaScript'ten yapılan XMLHttpRequests aramaları). Bu, yalnızca aynı başlangıç isteklerine izin vermekten daha fazla esneklik sağlar, ancak tüm çapraz orijin isteklerine izin vermekten daha güvenlidir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cors allow-credentials="false|true">
    <allowed-origins>
        <origin>origin uri</origin>
    </allowed-origins>
    <allowed-methods preflight-result-max-age="number of seconds">
        <method>http verb</method>
    </allowed-methods>
    <allowed-headers>
        <header>header name</header>
    </allowed-headers>
    <expose-headers>
        <header>header name</header>
    </expose-headers>
</cors>
```

### <a name="example"></a>Örnek
Bu örnek, GET ve POST dışındaki özel üstbilgi veya yöntemleri olanlar gibi uçuş öncesi isteklerinasıl destekleyeceğini gösterir. Özel üstbilgi ve ek HTTP fiillerini `allowed-methods` `allowed-headers` desteklemek için aşağıdaki örnekte gösterildiği gibi bölümleri ve bölümleri kullanın.

```xml
<cors allow-credentials="true">
    <allowed-origins>
        <!-- Localhost useful for development -->
        <origin>http://localhost:8080/</origin>
        <origin>http://example.com/</origin>
    </allowed-origins>
    <allowed-methods preflight-result-max-age="300">
        <method>GET</method>
        <method>POST</method>
        <method>PATCH</method>
        <method>DELETE</method>
    </allowed-methods>
    <allowed-headers>
        <!-- Examples below show Azure Mobile Services headers -->
        <header>x-zumo-installation-id</header>
        <header>x-zumo-application</header>
        <header>x-zumo-version</header>
        <header>x-zumo-auth</header>
        <header>content-type</header>
        <header>accept</header>
    </allowed-headers>
    <expose-headers>
        <!-- Examples below show Azure Mobile Services headers -->
        <header>x-zumo-installation-id</header>
        <header>x-zumo-application</header>
    </expose-headers>
</cors>
```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|Cors|Kök öğesi.|Evet|Yok|
|izin verilen kökenler|Etki `origin` alanları arası istekler için izin verilen kökenleri açıklayan öğeler içerir. `allowed-origins`herhangi bir menşee izin `origin` `*` verecek tek bir öğe veya `origin` URI içeren bir veya daha fazla öğe içerebilir.|Evet|Yok|
|başlangıç|Değer, tüm `*` kökenlere izin vermek için veya tek bir kaynağı belirten bir URI olabilir. URI bir düzen, ana bilgisayar ve bağlantı noktası içermelidir.|Evet|Bağlantı noktası BIR URI'de atlanırsa, http için bağlantı noktası 80 kullanılır ve 443 bağlantı noktası HTTPS için kullanılır.|
|izin verilen yöntemler|GET veya POST dışındaki yöntemlere izin veriliyorsa bu öğe gereklidir. Desteklenen `method` HTTP fiillerini belirten öğeler içerir. Değer `*` tüm yöntemleri gösterir.|Hayır|Bu bölüm yoksa, GET ve POST desteklenir.|
|method|Bir HTTP fiili belirtir.|Bölüm varsa `method` en az bir öğe gereklidir. `allowed-methods`|Yok|
|izin verilen üstbilgi|Bu öğe, isteke dahil edilebilen üstbilgiadlarının adlarını belirten öğeler içerir. `header`|Hayır|Yok|
|pozlama başlıkları|Bu öğe, istemci tarafından erişilen üstbilgi adlarını belirten öğeler içerir. `header`|Hayır|Yok|
|üst bilgi|Üstbilgi adını belirtir.|Bölüm içinde `header` `allowed-headers` veya `expose-headers` varsa en az bir öğe gereklidir.|Yok|

### <a name="attributes"></a>Öznitelikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|izin-kimlik bilgileri|Ön `Access-Control-Allow-Credentials` kontrol yanıtındaki üstbilgi bu özniteliğin değerine göre ayarlanır ve istemcinin etki alanı arası isteklerde kimlik bilgileri gönderme yeteneğini etkiler.|Hayır|yanlış|
|ön kontrol-sonuç-max-age|Ön `Access-Control-Max-Age` uçuş yanıtındaki üstbilgi bu özniteliğin değerine göre ayarlanır ve kullanıcı aracısının uçuş öncesi yanıtı önbelleğe verme yeteneğini etkiler.|Hayır|0|

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

- **İlke bölümleri:** gelen
- **İlke kapsamları:** tüm kapsamlar

## <a name="jsonp"></a><a name="JSONP"></a>Jsonp
İlke, `jsonp` JavaScript tarayıcı tabanlı istemcilerden gelen etki alanları arası çağrılara izin vermek için bir operasyona veya API'ye dolgu (JSONP) desteği yle JSON ekler. JSONP, JavaScript programlarında farklı bir etki alanında sunucudan veri istemek için kullanılan bir yöntemdir. JSONP, web sayfalarına erişimin aynı etki alanında olması gereken çoğu web tarayıcısı tarafından uygulanan sınırlamayı atlar.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<jsonp callback-parameter-name="callback function name" />
```

### <a name="example"></a>Örnek

```xml
<jsonp callback-parameter-name="cb" />
```

Eğer geri arama parametresi olmadan yöntem çağırırsanız ?cb=XXX düz JSON (bir işlev çağrı sarıcı olmadan) döndürecektir.

Geri arama parametresini `?cb=XXX` eklerseniz, orijinal JSON sonuçlarını geri çağırma işlevi etrafında saran bir JSONP sonucu döndürecek`XYZ('<json result goes here>');`

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|Jsonp|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Adı|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|geri arama-parametre-adı|Etki alanı arası JavaScript işlevi, işlevin bulunduğu tam nitelikli alan adı ile önceden belirlenmiş arama.|Evet|Yok|

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

- **İlke bölümleri:** giden
- **İlke kapsamları:** tüm kapsamlar

## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışan daha fazla bilgi için bkz:

+ [API Yönetiminde İlkeler](api-management-howto-policies.md)
+ [API'leri Dönüştür](transform-api.md)
+ [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
+ [İlke örnekleri](policy-samples.md)
