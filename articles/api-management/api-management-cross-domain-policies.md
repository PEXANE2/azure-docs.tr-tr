---
title: Azure API Management etki alanları arası ilkeler | Microsoft Docs
description: Azure API Management 'de kullanıma sunulan etki alanları arası ilkeler hakkında bilgi edinin.
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
ms.openlocfilehash: 6512edd26b59dac11f046e82940db4877728943c
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86243601"
---
# <a name="api-management-cross-domain-policies"></a>API Management etki alanları arası ilkeler
Bu konu, aşağıdaki API Management ilkelerine yönelik bir başvuru sağlar. İlke ekleme ve yapılandırma hakkında daha fazla bilgi için bkz. [API Management ilkeleri](https://go.microsoft.com/fwlink/?LinkID=398186).

## <a name="cross-domain-policies"></a><a name="CrossDomainPolicies"></a>Etki alanları arası ilkeler

- [Etki alanları arası çağrılara Izin ver](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -API 'Yi Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirir.
- [CORS](api-management-cross-domain-policies.md#CORS) -tarayıcı tabanlı istemcilerden etki alanları arası çağrılara izin vermek için bir işleme veya bir API 'ye çıkış noktaları arası kaynak PAYLAŞıMı (CORS) desteği ekler.
- [JSONP](api-management-cross-domain-policies.md#JSONP) -JavaScript tarayıcı tabanlı istemcilerden etki alanları arası çağrılara izin vermek için, doldurma (JSONP) desteğiyle birlikte bir Işleme veya API 'yi ekler.

## <a name="allow-cross-domain-calls"></a><a name="AllowCrossDomainCalls"></a>Etki alanları arası çağrılara izin ver
API 'yi `cross-domain` Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirmek için ilkeyi kullanın.

### <a name="policy-statement"></a>İlke ekstresi

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

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|etki alanları arası|Kök öğe. Alt öğeler, [Adobe etki alanları arası ilke dosyası belirtimine](https://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html)uymalıdır.|Evet|

### <a name="usage"></a>Kullanım
Bu ilke, aşağıdaki ilke [bölümlerinde](./api-management-howto-policies.md#sections) ve [kapsamlarda](./api-management-howto-policies.md#scopes)kullanılabilir.

- **İlke bölümleri:** gelen
- **İlke kapsamları:** tüm kapsamlar

## <a name="cors"></a><a name="CORS"></a>CORS
`cors`İlke, tarayıcı tabanlı istemcilerden etki alanları arası çağrılara izin vermek için bir işleme veya BIR API 'ye çıkış noktaları arası kaynak paylaşımı (CORS) desteği ekler.

CORS, bir tarayıcı ve sunucunun etkileşime geçmesini ve belirli çapraz kaynak isteklerine izin verip vermeyeceğinizi (yani, bir Web sayfasındaki JavaScript 'ten diğer etki alanlarına yapılan XMLHttpRequests çağrılarını) belirlemenizi sağlar. Bu, yalnızca aynı kaynaklı isteklere izin verilenden daha fazla esneklik sağlar, ancak tüm çapraz kaynak isteklerine izin verilmesinin daha güvenlidir.

### <a name="policy-statement"></a>İlke ekstresi

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
Bu örnek, alma ve GÖNDERI dışında özel üst bilgileri veya yöntemleri olan yöntemler gibi, ön uçuş isteklerinin nasıl destekleyeceğinizi gösterir. Özel üst bilgileri ve ek HTTP fiillerini desteklemek için `allowed-methods` `allowed-headers` Aşağıdaki örnekte gösterildiği gibi ve bölümlerini kullanın.

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

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|CORS|Kök öğe.|Evet|Yok|
|izin verilen-kaynaklar|`origin`Etki alanları arası istekler için izin verilen kaynakları tanımlayan öğeleri içerir. `allowed-origins``origin`herhangi bir kaynağa izin vermek için belirten tek bir öğe `*` veya bir URI içeren bir ya da daha fazla öğe içerebilir `origin` .|Evet|Yok|
|başlangıç|Değer `*` tüm kaynakları veya tek bir kaynağı belirten BIR URI 'yi izin verebilir. URI bir düzen, konak ve bağlantı noktası içermelidir.|Evet|Bağlantı noktası bir URI 'de yoksayılırsa, HTTP için bağlantı noktası 80 ve HTTPS için bağlantı noktası 443 kullanılır.|
|izin verilen-Yöntemler|GET veya POST dışındaki yöntemlere izin veriliyorsa bu öğe gereklidir. `method`Desteklenen HTTP fiillerini belirten öğeleri içerir. Değer `*` tüm yöntemleri gösterir.|Hayır|Bu bölüm yoksa GET ve POST desteklenir.|
|method|Bir HTTP fiilini belirtir.|Bölüm mevcutsa en az bir `method` öğe gereklidir `allowed-methods` .|Yok|
|izin verilen-üstbilgiler|Bu öğe `header` , isteğe dahil edilebilir üst bilgilerin adlarını belirten öğeleri içerir.|Hayır|Yok|
|kullanıma yönelik üst bilgiler|Bu öğe `header` , istemci tarafından erişilebilen üst bilgilerin adlarını belirten öğeleri içerir.|Hayır|YOK|
|üst bilgi|Bir üst bilgi adı belirtir.|`header` `allowed-headers` Bölümünde veya bölümü varsa en az bir öğe gereklidir `expose-headers` .|Yok|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|izin ver-kimlik bilgileri|`Access-Control-Allow-Credentials`Ön kontrol yanıtındaki üst bilgi bu özniteliğin değerine ayarlanır ve istemcinin etki alanları arası isteklerde kimlik bilgilerini gönderme yeteneğini etkiler.|Hayır|yanlış|
|ön kontrol-sonuç-en yüksek yaş|`Access-Control-Max-Age`Ön kontrol yanıtında üst bilgi bu özniteliğin değerine ayarlanır ve Kullanıcı aracısının ön uçuş yanıtını önbelleğe alma yeteneğini etkiler.|Hayır|0|

### <a name="usage"></a>Kullanım
Bu ilke, aşağıdaki ilke [bölümlerinde](./api-management-howto-policies.md#sections) ve [kapsamlarda](./api-management-howto-policies.md#scopes)kullanılabilir.

- **İlke bölümleri:** gelen
- **İlke kapsamları:** tüm kapsamlar

## <a name="jsonp"></a><a name="JSONP"></a>JSONP
`jsonp`İlke, JavaScript tarayıcı tabanlı istemcilerden etki alanları arası çağrılara izin vermek için bir işleme veya BIR API Ile JSON (JSONP) desteği ekler. JSONP, farklı bir etki alanındaki bir sunucudan veri istemek için JavaScript programlarında kullanılan bir yöntemdir. JSONP, Web sayfalarına erişimin aynı etki alanında olması gereken çoğu Web tarayıcısı tarafından zorlanan sınırlandırmayı atlar.

### <a name="policy-statement"></a>İlke ekstresi

```xml
<jsonp callback-parameter-name="callback function name" />
```

### <a name="example"></a>Örnek

```xml
<jsonp callback-parameter-name="cb" />
```

Yöntemi geri çağırma parametresi olmadan çağırırsanız? CB = XXX, düz JSON döndürür (işlev çağrısı sarmalayıcısı olmadan).

Geri çağırma parametresini eklerseniz `?cb=XXX` , BIR JSONP sonucu döndürür ve özgün JSON sonuçlarını, şunun gibi geri çağırma işlevi etrafında sarmalama`XYZ('<json result goes here>');`

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|JSONP|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|geri arama-parametre-adı|Etki alanları arası JavaScript işlev çağrısı, işlevin bulunduğu tam etki alanı adıyla önekli.|Evet|Yok|

### <a name="usage"></a>Kullanım
Bu ilke, aşağıdaki ilke [bölümlerinde](./api-management-howto-policies.md#sections) ve [kapsamlarda](./api-management-howto-policies.md#scopes)kullanılabilir.

- **İlke bölümleri:** giden
- **İlke kapsamları:** tüm kapsamlar

## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışma hakkında daha fazla bilgi için bkz.

+ [API Management ilkeler](api-management-howto-policies.md)
+ [API dönüştürme](transform-api.md)
+ İlke deyimlerinin ve ayarlarının tam listesi için [Ilke başvurusu](./api-management-policies.md)
+ [İlke örnekleri](policy-samples.md)
