---
title: Azure API Yönetimi önbelleğe alma ilkeleri | Microsoft Dokümanlar
description: Azure API Yönetimi'nde kullanılmak üzere kullanılabilir önbelleğe alma ilkeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/27/2018
ms.author: apimpm
ms.openlocfilehash: 06c4ede12f939e48973d3e0b502d90b848d199bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280306"
---
# <a name="api-management-caching-policies"></a>API Management önbelleğe alma ilkeleri
Bu konu, aşağıdaki API Yönetimi ilkeleri için bir başvuru sağlar. İlkeler ekleme ve yapılandırma hakkında bilgi için [API Yönetimi'ndeki İlkeler'e](https://go.microsoft.com/fwlink/?LinkID=398186)bakın.

## <a name="caching-policies"></a><a name="CachingPolicies"></a>Önbelleğe alma ilkeleri

- Yanıt önbelleğe alma ilkeleri
    - [Önbellekten alın](api-management-caching-policies.md#GetFromCache) - Önbellek görünümünü gerçekleştirin ve kullanılabilir olduğunda geçerli önbelleğe alınmış yanıtları döndürün.
    - [Depodan önbelleğe](api-management-caching-policies.md#StoreToCache) depolayın - Belirtilen önbellek denetim yapılandırmasına göre yanıtları önbelleğe alan.
- Değer önbelleğe alma ilkeleri
    - [Önbellekten değer alın](#GetFromCacheByKey) - Önbelleğe alınmış bir öğeyi anahtarla alın.
    - [Önbellekte depo değeri](#StoreToCacheByKey) - Bir öğeyi önbellekte anahtarla saklayın.
    - [Önbellekten değeri kaldırın](#RemoveCacheByKey) - Önbellekteki bir öğeyi anahtarla kaldırın.

## <a name="get-from-cache"></a><a name="GetFromCache"></a>Önbellekten alın
Önbellek `cache-lookup` görünümünü gerçekleştirmek ve kullanılabilir olduğunda geçerli bir önbelleğe alınmış yanıtı döndürmek için ilkeyi kullanın. Bu ilke, yanıt içeriğinin belirli bir süre boyunca sabit kaldığı durumlarda uygulanabilir. Yanıt önbelleğe alma, arka uç web sunucusuna uygulanan bant genişliği ve işleme gereksinimlerini azaltır ve API tüketicileri tarafından algılanan gecikme süresini azaltır.

> [!NOTE]
> Bu ilke, [önbellek ilkesine](api-management-caching-policies.md#StoreToCache) karşılık gelen bir Depo olmalıdır.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" caching-type="prefer-external | external | internal" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">
  <vary-by-header>Accept</vary-by-header>
  <!-- should be present in most cases -->
  <vary-by-header>Accept-Charset</vary-by-header>
  <!-- should be present in most cases -->
  <vary-by-header>Authorization</vary-by-header>
  <!-- should be present when allow-private-response-caching is "true"-->
  <vary-by-header>header name</vary-by-header>
  <!-- optional, can repeated several times -->
  <vary-by-query-parameter>parameter name</vary-by-query-parameter>
  <!-- optional, can repeated several times -->
</cache-lookup>
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true" caching-type="internal" >
            <vary-by-query-parameter>version</vary-by-query-parameter>
        </cache-lookup>
    </inbound>
    <outbound>
        <cache-store duration="seconds" />
        <base />
    </outbound>
</policies>
```

#### <a name="example-using-policy-expressions"></a>İlke ifadelerini kullanma örneği
Bu örnek, desteklenen hizmetin `Cache-Control` yönergesi tarafından belirtildiği gibi arka uç hizmetinin yanıt önbelleğe alma yla eşleşen API Yönetimi yanıt önbelleğe alma süresini nasıl yapılandırılabildiğini gösterir. Bu politikayı yapılandırma ve kullanma nın bir gösterimi için, [Vlad Vinogradsky ile Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 25:25'e hızlı ileri sarma bölümüne bakın.

```xml
<!-- The following cache policy snippets demonstrate how to control API Management response cache duration with Cache-Control headers sent by the backend service. -->

<!-- Copy this snippet into the inbound section -->
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >
  <vary-by-header>Accept</vary-by-header>
  <vary-by-header>Accept-Charset</vary-by-header>
</cache-lookup>

<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the default value of 5 min if none is found  -->
<cache-store duration="@{
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;
  }"
 />
```

Daha fazla bilgi için [İlke ifadeleri](api-management-policy-expressions.md) ve [Bağlam değişkenine](api-management-policy-expressions.md#ContextVariables)bakın.

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|önbellek arama|Kök öğesi.|Evet|
|kafaya göre değişir|Kabul Et, Kabul Et-Charset, Accept-Coding, Accept-Language, Authorization, Expect, From, Host, If-Match gibi belirtilen üstbilginin değeri başına yanıtları önbelleğe almaya başlayın.|Hayır|
|değişken sorgu-parametresi|Belirtilen sorgu parametrelerinin değeri başına yanıtları önbelleğe almaya başlayın. Tek veya birden çok parametre girin. Ayırıcı olarak yarı kolon kullanın. Hiçbiri belirtilmemişse, tüm sorgu parametreleri kullanılır.|Hayır|

### <a name="attributes"></a>Öznitelikler

| Adı                           | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| izin-özel-yanıt-önbelleğe alma | `true`Ayarlandığında, Yetkilendirme üstbilgisi içeren isteklerin önbelleğe aldamasına izin verir.                                                                                                                                                                                                                                                                        | Hayır       | yanlış             |
| önbelleğe alma tipi               | Öznitelik aşağıdaki değerler arasında seçim yapın:<br />- `internal`yerleşik API Yönetimi önbelleğini kullanmak,<br />- `external`[Azure API Yönetimi'nde Redis için harici bir Azure Önbelleği kullanın'da](api-management-howto-cache-external.md)açıklandığı gibi harici önbelleği kullanmak,<br />- `prefer-external`aksi takdirde yapılandırılmış veya dahili önbellek kullanmak için. | Hayır       | `prefer-external` |
| aşağı akım önbelleğe alma türü        | Bu öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> - yok - downstream önbelleğe izin verilmez.<br />- özel - downstream özel önbelleğe izin verilir.<br />- genel - özel ve paylaşılan downstream önbelleğe izin verilir.                                                                                                          | Hayır       | yok              |
| yeniden onaylamalı                | Akış aşağı önbelleği etkinleştirildiğinde, bu öznitelik ağ geçidi yanıtlarında önbellek denetim yönergesini `must-revalidate` açar veya kapatır.                                                                                                                                                                                                                      | Hayır       | true              |
| geliştiriciye göre değişir              | Abonelik `true` [anahtarı](https://docs.microsoft.com/azure/api-management/api-management-subscriptions)başına yanıtları önbelleğe almak için ayarlayın.                                                                                                                                                                                                                                                                                                         | Evet      |         False          |
| geliştirici gruplarına göre değişir       | Kullanıcı `true` grubu başına yanıtları önbelleğe almak [için](https://docs.microsoft.com/azure/api-management/api-management-howto-create-groups)ayarlayın.                                                                                                                                                                                                                                                                                                             | Evet      |       False            |

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

- **İlke bölümleri:** gelen
- **İlke kapsamları:** tüm kapsamlar

## <a name="store-to-cache"></a><a name="StoreToCache"></a>Önbelleğe depola
İlke, `cache-store` yanıtları belirtilen önbellek ayarlarına göre önbelleğe getirir. Bu ilke, yanıt içeriğinin belirli bir süre boyunca sabit kaldığı durumlarda uygulanabilir. Yanıt önbelleğe alma, arka uç web sunucusuna uygulanan bant genişliği ve işleme gereksinimlerini azaltır ve API tüketicileri tarafından algılanan gecikme süresini azaltır.

> [!NOTE]
> Bu ilke, ilgili bir [önbellek ten Al](api-management-caching-policies.md#GetFromCache) ilkesi olmalıdır.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cache-store duration="seconds" />
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false">
            <vary-by-query-parameter>parameter name</vary-by-query-parameter> <!-- optional, can repeated several times -->
        </cache-lookup>
    </inbound>
    <outbound>
        <base />
        <cache-store duration="3600" />
    </outbound>
</policies>
```

#### <a name="example-using-policy-expressions"></a>İlke ifadelerini kullanma örneği
Bu örnek, desteklenen hizmetin `Cache-Control` yönergesi tarafından belirtildiği gibi arka uç hizmetinin yanıt önbelleğe alma yla eşleşen API Yönetimi yanıt önbelleğe alma süresini nasıl yapılandırılabildiğini gösterir. Bu politikayı yapılandırma ve kullanma nın bir gösterimi için, [Vlad Vinogradsky ile Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 25:25'e hızlı ileri sarma bölümüne bakın.

```xml
<!-- The following cache policy snippets demonstrate how to control API Management response cache duration with Cache-Control headers sent by the backend service. -->

<!-- Copy this snippet into the inbound section -->
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >
  <vary-by-header>Accept</vary-by-header>
  <vary-by-header>Accept-Charset</vary-by-header>
</cache-lookup>

<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the default value of 5 min if none is found  -->
<cache-store duration="@{
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;
  }"
 />
```

Daha fazla bilgi için [İlke ifadeleri](api-management-policy-expressions.md) ve [Bağlam değişkenine](api-management-policy-expressions.md#ContextVariables)bakın.

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|önbellek deposu|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

| Adı             | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| süre         | Saniyeler içinde belirtilen önbelleğe alınmış girişlerin canlı yayılacak zamanı.                                                                                                                                                                                                                                                                                                   | Evet      | Yok               |

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

- **İlke bölümleri:** giden
- **İlke kapsamları:** tüm kapsamlar

## <a name="get-value-from-cache"></a><a name="GetFromCacheByKey"></a>Önbellekten değer alma
Önbellek `cache-lookup-value` aramasını anahtarla gerçekleştirmek ve önbelleğe alınmış bir değeri döndürmek için ilkeyi kullanın. Anahtar rasgele bir dize değerine sahip olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır.

> [!NOTE]
> Bu ilke, [önbellek ilkesinde](#StoreToCacheByKey) karşılık gelen bir Mağaza değerine sahip olmalıdır.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cache-lookup-value key="cache key value"
    default-value="value to use if cache lookup resulted in a miss"
    variable-name="name of a variable looked up value is assigned to"
    caching-type="prefer-external | external | internal" />
```

### <a name="example"></a>Örnek
Daha fazla bilgi ve bu iiden örnekler için Azure [API Yönetimi'nde Özel önbelleğe alma bölümüne](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/)bakın.

```xml
<cache-lookup-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    variable-name="userprofile" />

```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|önbellek arama değeri|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

| Adı             | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| önbelleğe alma tipi | Öznitelik aşağıdaki değerler arasında seçim yapın:<br />- `internal`yerleşik API Yönetimi önbelleğini kullanmak,<br />- `external`[Azure API Yönetimi'nde Redis için harici bir Azure Önbelleği kullanın'da](api-management-howto-cache-external.md)açıklandığı gibi harici önbelleği kullanmak,<br />- `prefer-external`aksi takdirde yapılandırılmış veya dahili önbellek kullanmak için. | Hayır       | `prefer-external` |
| varsayılan değer    | Önbellek anahtarı araması kaçırılmayla sonuçlanırsa değişkene atanacak değer. Bu öznitelik belirtilmemişse, `null` atanır.                                                                                                                                                                                                           | Hayır       | `null`            |
| anahtar              | Aramada kullanılacak önbellek anahtar değeri.                                                                                                                                                                                                                                                                                                                       | Evet      | Yok               |
| değişken adı    | Arama başarılı [context variable](api-management-policy-expressions.md#ContextVariables) olursa, görünüm değişkeninin adı, yukarı ya yıla atanan değere atanacaktır. Arama da bir hata yla sonuçlanırsa, değişkene `default-value` öznitelik `null`değeri `default-value` veya öznitelik atlanırsa atanır.                                       | Evet      | Yok               |

### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

- **İlke bölümleri:** gelen, giden, arka uç, on-error
- **İlke kapsamları:** tüm kapsamlar

## <a name="store-value-in-cache"></a><a name="StoreToCacheByKey"></a>Önbellekte depo değeri
Önbellek depolamasını `cache-store-value` anahtarla gerçekleştirir. Anahtar rasgele bir dize değerine sahip olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır.

> [!NOTE]
> Bu ilke, [önbellek ilkesinden](#GetFromCacheByKey) karşılık gelen bir Get değerine sahip olmalıdır.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<cache-store-value key="cache key value" value="value to cache" duration="seconds" caching-type="prefer-external | external | internal" />
```

### <a name="example"></a>Örnek
Daha fazla bilgi ve bu iiden örnekler için Azure [API Yönetimi'nde Özel önbelleğe alma bölümüne](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/)bakın.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />

```

### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|önbellek-mağaza değeri|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

| Adı             | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| önbelleğe alma tipi | Öznitelik aşağıdaki değerler arasında seçim yapın:<br />- `internal`yerleşik API Yönetimi önbelleğini kullanmak,<br />- `external`[Azure API Yönetimi'nde Redis için harici bir Azure Önbelleği kullanın'da](api-management-howto-cache-external.md)açıklandığı gibi harici önbelleği kullanmak,<br />- `prefer-external`aksi takdirde yapılandırılmış veya dahili önbellek kullanmak için. | Hayır       | `prefer-external` |
| süre         | Değer, saniye cinsinden belirtilen sağlanan süre değeri için önbelleğe alınır.                                                                                                                                                                                                                                                                                 | Evet      | Yok               |
| anahtar              | Önbellek anahtarı değeri altında depolanır.                                                                                                                                                                                                                                                                                                                   | Evet      | Yok               |
| value            | Önbelleğe alınacak değer.                                                                                                                                                                                                                                                                                                                                     | Evet      | Yok               |
### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

- **İlke bölümleri:** gelen, giden, arka uç, on-error
- **İlke kapsamları:** tüm kapsamlar

### <a name="remove-value-from-cache"></a><a name="RemoveCacheByKey"></a>Önbellekten değeri kaldırma
Önbelleğe `cache-remove-value` alınmış bir öğeyi anahtarıyla tanımlanır. Anahtar rasgele bir dize değerine sahip olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır.

#### <a name="policy-statement"></a>İlke bildirimi

```xml

<cache-remove-value key="cache key value" caching-type="prefer-external | external | internal"  />

```

#### <a name="example"></a>Örnek

```xml

<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>

```

#### <a name="elements"></a>Öğeler

|Adı|Açıklama|Gerekli|
|----------|-----------------|--------------|
|önbellek kaldırma değeri|Kök öğesi.|Evet|

#### <a name="attributes"></a>Öznitelikler

| Adı             | Açıklama                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| önbelleğe alma tipi | Öznitelik aşağıdaki değerler arasında seçim yapın:<br />- `internal`yerleşik API Yönetimi önbelleğini kullanmak,<br />- `external`[Azure API Yönetimi'nde Redis için harici bir Azure Önbelleği kullanın'da](api-management-howto-cache-external.md)açıklandığı gibi harici önbelleği kullanmak,<br />- `prefer-external`aksi takdirde yapılandırılmış veya dahili önbellek kullanmak için. | Hayır       | `prefer-external` |
| anahtar              | Önbellekten kaldırılacak daha önce önbelleğe alınmış değerin anahtarı.                                                                                                                                                                                                                                                                                        | Evet      | Yok               |

#### <a name="usage"></a>Kullanım
Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) kullanılabilir.

- **İlke bölümleri:** gelen, giden, arka uç, on-error
- **İlke kapsamları:** tüm kapsamlar

## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışan daha fazla bilgi için bkz:

+ [API Yönetiminde İlkeler](api-management-howto-policies.md)
+ [API'leri Dönüştür](transform-api.md)
+ [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
+ [İlke örnekleri](policy-samples.md)
