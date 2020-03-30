---
title: Azure API Yönetimi erişim kısıtlama ilkeleri | Microsoft Dokümanlar
description: Azure API Yönetimi'nde kullanılmak üzere kullanılabilen erişim kısıtlama ilkeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/10/2020
ms.author: apimpm
ms.openlocfilehash: 3ba620d66b84e6724751b2024059e8ecd66888cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266123"
---
# <a name="api-management-access-restriction-policies"></a>API Management erişim kısıtlama ilkeleri

Bu konu, aşağıdaki API Yönetimi ilkeleri için bir başvuru sağlar. İlkeler ekleme ve yapılandırma hakkında bilgi için [API Yönetimi'ndeki İlkeler'e](https://go.microsoft.com/fwlink/?LinkID=398186)bakın.

## <a name="access-restriction-policies"></a><a name="AccessRestrictionPolicies"></a>Erişim kısıtlama ilkeleri

-   [HTTP üstbilgisini denetleyin](api-management-access-restriction-policies.md#CheckHTTPHeader) - BIR HTTP Üstbilginin varlığını ve/veya değerini zorlar.
-   [Çağrı oranını aboneye göre sınırlandırın](api-management-access-restriction-policies.md#LimitCallRate) - Abonelik başına çağrı oranını sınırlayarak API kullanım artışlarını önler.
-   [Arama oranını anahtara göre sınırlandırın](#LimitCallRateByKey) - Her anahtar bazında çağrı hızını sınırlayarak API kullanım artışlarını önler.
-   [Arayan IP'lerini kısıtlayın](api-management-access-restriction-policies.md#RestrictCallerIPs) - Belirli IP adreslerinden ve/veya adres aralıklarından gelen filtreler (izin verir/reddeder).
-   [Kullanım kotasını aboneliğe göre ayarlama](api-management-access-restriction-policies.md#SetUsageQuota) - Abonelik başına yenilenebilir veya ömür boyu çağrı hacmi ve/veya bant genişliği kotası uygulamanızı sağlar.
-   [Kullanım kotasını anahtara göre ayarlayın](#SetUsageQuotaByKey) - Yenilenebilir veya ömür boyu çağrı hacmini ve/veya bant genişliği kotasını anahtar bazında uygulamanızı sağlar.
-   [JWT'yi doğrulayın](api-management-access-restriction-policies.md#ValidateJWT) - Belirli bir HTTP Üstbilgisinden veya belirli bir sorgu parametresinden çıkarılan bir JWT'nin varlığını ve geçerliliğini zorlar.

> [!TIP]
> Farklı kapsamlarda erişim kısıtlama ilkeleri kullanabilirsiniz. Örneğin, `validate-jwt` ilkeyi API düzeyinde uygulayarak Tüm API'yi AAD kimlik doğrulaması yla güvence altına alabilir `claims` veya API işlem düzeyine uygulayabilir ve daha ayrıntılı denetim için kullanabilirsiniz.

## <a name="check-http-header"></a><a name="CheckHTTPHeader"></a>HTTP üstbilgikontrol edin

Bir `check-header` isteğin belirli bir HTTP üstbilgisi olduğunu zorlamak için ilkeyi kullanın. Üstbilginin belirli bir değeri olup olmadığını isteğe bağlı olarak denetleyebilir veya izin verilen bir değer aralığını denetleyebilirsiniz. Denetim başarısız olursa, ilke istek işlemeyi sonlandırır ve ilke tarafından belirtilen HTTP durum kodu ve hata iletisini döndürür.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="true">
    <value>Value1</value>
    <value>Value2</value>
</check-header>
```

### <a name="example"></a>Örnek

```xml
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>
</check-header>
```

### <a name="elements"></a>Öğeler

| Adı         | Açıklama                                                                                                                                   | Gerekli |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| kontrol üstbilgi | Kök öğesi.                                                                                                                                 | Evet      |
| value        | İzin verilen HTTP üstbilgi değeri. Birden çok değer öğesi belirtildiğinde, değerlerden herhangi biri eşleşmiyorsa, denetim başarılı olarak kabul edilir. | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Adı                       | Açıklama                                                                                                                                                            | Gerekli | Varsayılan |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| başarısız-denetim-hata-ileti | Üstbilgi yoksa veya geçersiz bir değeri varsa, HTTP yanıt gövdesinde döndürülecek hata iletisi. Bu iletide herhangi bir özel karakter düzgün kaçtı olmalıdır. | Evet      | Yok     |
| başarısız-check-httpcode      | Üstbilgi yoksa veya geçersiz bir değeri varsa, http durum kodu döndürülecek.                                                                                        | Evet      | Yok     |
| üstbilgi adı                | Kontrol etmek için HTTP Üstbilgi adı.                                                                                                                                  | Evet      | Yok     |
| yoksayma-durum                | True veya False olarak ayarlanabilir. Üstbilgi değeri kabul edilebilir değerler kümesiyle karşılaştırıldığında True case'e ayarlanırsa, bu durum göz ardı edilir.                                    | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen, giden

-   **İlke kapsamları:** tüm kapsamlar

## <a name="limit-call-rate-by-subscription"></a><a name="LimitCallRate"></a>Aboneye göre arama oranını sınırlandırın

İlke, `rate-limit` çağrı oranını belirli bir süre başına belirli bir sayıyla sınırlandırarak abonelik bazında API kullanımının artmasını önler. Bu ilke tetiklendiğinde arayan `429 Too Many Requests` bir yanıt durum kodu alır.

> [!IMPORTANT]
> Bu ilke, ilke belgesi başına yalnızca bir kez kullanılabilir.
>
> [İlke ifadeleri](api-management-policy-expressions.md) bu ilke özniteliklerinin hiçbirinde kullanılamaz.

> [!CAUTION]
> Daraltma mimarisinin dağıtılmış doğası nedeniyle, oran sınırlaması hiçbir zaman tam olarak doğru değildir. Yapılandırılmış ve izin verilen isteklerin gerçek sayısı arasındaki fark istek hacmi ve hızı, arka uç gecikmesi ve diğer etkenlere bağlı olarak değişir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<rate-limit calls="number" renewal-period="seconds">
    <api name="API name" id="API id" calls="number" renewal-period="seconds" />
        <operation name="operation name" id="operation id" calls="number" renewal-period="seconds" />
    </api>
</rate-limit>
```

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
        <rate-limit calls="20" renewal-period="90" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```

### <a name="elements"></a>Öğeler

| Adı       | Açıklama                                                                                                                                                                                                                                                                                              | Gerekli |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| oran sınırı | Kök öğesi.                                                                                                                                                                                                                                                                                            | Evet      |
| API        | Ürün içindeki API'lere çağrı oranı sınırı uygulamak için bu öğelerden birini veya birkaçını ekleyin. Ürün ve API çağrı oranı sınırları bağımsız olarak uygulanır. API üzerinden `name` veya `id`. Her iki öznitelik `id` sağlanırsa, `name` kullanılır ve yoksayılır.                    | Hayır       |
| Işlem  | API içindeki işlemlere çağrı hızı sınırı uygulamak için bu öğelerden birini veya birkaçını ekleyin. Ürün, API ve operasyon çağrı oranı sınırları bağımsız olarak uygulanır. İşlem ya `name` da `id`. Her iki öznitelik `id` sağlanırsa, `name` kullanılır ve yoksayılır. | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Adı           | Açıklama                                                                                           | Gerekli | Varsayılan |
| -------------- | ----------------------------------------------------------------------------------------------------- | -------- | ------- |
| ad           | Oran sınırını uygulamak için API adı.                                                | Evet      | Yok     |
| Aramalar          | 'de belirtilen zaman aralığında izin verilen maksimum `renewal-period`toplam çağrı sayısı. | Evet      | Yok     |
| yenileme dönemi | Kotanın sıfırlandığı saniye cinsinden zaman dilimi.                                              | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen

-   **İlke kapsamları:** ürün, api, operasyon

## <a name="limit-call-rate-by-key"></a><a name="LimitCallRateByKey"></a>Arama oranını anahtara göre sınırlandırın

> [!IMPORTANT]
> Bu özellik API Yönetimi'nin **Tüketim** katmanında kullanılamaz.

İlke, `rate-limit-by-key` çağrı oranını belirli bir zaman dilimi başına belirli bir sayıyla sınırlandırarak API kullanımının önemli bazda artışlarını önler. Anahtar rasgele bir dize değerine sahip olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır. Hangi isteklerin sınıra doğru sayılması gerektiğini belirtmek için isteğe bağlı artış koşulu eklenebilir. Bu ilke tetiklendiğinde arayan `429 Too Many Requests` bir yanıt durum kodu alır.

Daha fazla bilgi ve bu iiden örnekler için Azure [API Yönetimi ile Gelişmiş istek azaltma](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/)bölümüne bakın.

> [!CAUTION]
> Daraltma mimarisinin dağıtılmış doğası nedeniyle, oran sınırlaması hiçbir zaman tam olarak doğru değildir. Yapılandırılmış ve izin verilen isteklerin gerçek sayısı arasındaki fark istek hacmi ve hızı, arka uç gecikmesi ve diğer etkenlere bağlı olarak değişir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<rate-limit-by-key calls="number"
                   renewal-period="seconds"
                   increment-condition="condition"
                   counter-key="key value" />

```

### <a name="example"></a>Örnek

Aşağıdaki örnekte, oran sınırı arayan IP adresi tarafından anahtarlanır.

```xml
<policies>
    <inbound>
        <base />
        <rate-limit-by-key  calls="10"
              renewal-period="60"
              increment-condition="@(context.Response.StatusCode == 200)"
              counter-key="@(context.Request.IpAddress)"/>
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```

### <a name="elements"></a>Öğeler

| Adı              | Açıklama   | Gerekli |
| ----------------- | ------------- | -------- |
| fiyat-limit-by-key | Kök öğesi. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Adı                | Açıklama                                                                                           | Gerekli | Varsayılan |
| ------------------- | ----------------------------------------------------------------------------------------------------- | -------- | ------- |
| Aramalar               | 'de belirtilen zaman aralığında izin verilen maksimum `renewal-period`toplam çağrı sayısı. | Evet      | Yok     |
| karşı anahtar         | Fiyat sınırı ilkesi için kullanılacak anahtar.                                                             | Evet      | Yok     |
| artış durumu | İsteğin kotaya doğru sayılması gerektiğini belirten`true`boolean ifadesi ( ).        | Hayır       | Yok     |
| yenileme dönemi      | Kotanın sıfırlandığı saniye cinsinden zaman dilimi.                                              | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen

-   **İlke kapsamları:** tüm kapsamlar

## <a name="restrict-caller-ips"></a><a name="RestrictCallerIPs"></a>Arayan IP'leri kısıtlama

İlke, `ip-filter` belirli IP adreslerinden ve/veya adres aralıklarından gelen aramaları filtreler (izin verir/reddeder).

### <a name="policy-statement"></a>İlke bildirimi

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address" />
</ip-filter>
```

### <a name="example"></a>Örnek

Aşağıdaki örnekte, ilke yalnızca tek IP adresinden veya belirtilen IP adresleri aralığından gelen isteklere izin verir

```xml
<ip-filter action="allow">
    <address>13.66.201.169</address>
    <address-range from="13.66.140.128" to="13.66.140.143" />
</ip-filter>
```

### <a name="elements"></a>Öğeler

| Adı                                      | Açıklama                                         | Gerekli                                                       |
| ----------------------------------------- | --------------------------------------------------- | -------------------------------------------------------------- |
| ip filtresi                                 | Kök öğesi.                                       | Evet                                                            |
| adres                                   | Filtre uygulanacak tek bir IP adresi belirtir.   | En az `address` `address-range` bir veya öğe gereklidir. |
| adres aralığı="adres" to="adres" | Filtre uygulanacak bir ip adresi aralığı belirtir. | En az `address` `address-range` bir veya öğe gereklidir. |

### <a name="attributes"></a>Öznitelikler

| Adı                                      | Açıklama                                                                                 | Gerekli                                           | Varsayılan |
| ----------------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------- | ------- |
| adres aralığı="adres" to="adres" | Erişime izin vermek veya reddetmek için çeşitli IP adresleri.                                        | `address-range` Öğe kullanıldığında gereklidir. | Yok     |
| ip-filtre action="&#124; yasaklamaizin"    | Belirtilen IP adresleri ve aralıkları için aramalara izin verilip verilmeyeceğini belirtir. | Evet                                                | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen
-   **İlke kapsamları:** tüm kapsamlar

## <a name="set-usage-quota-by-subscription"></a><a name="SetUsageQuota"></a>Kullanım kotalarını abonelme göre ayarlama

İlke, `quota` abonelik bazında yenilenebilir veya ömür boyu çağrı hacmi ve/veya bant genişliği kotası uygular.

> [!IMPORTANT]
> Bu ilke, ilke belgesi başına yalnızca bir kez kullanılabilir.
>
> [İlke ifadeleri](api-management-policy-expressions.md) bu ilke özniteliklerinin hiçbirinde kullanılamaz.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="API name" id="API id" calls="number" renewal-period="seconds" />
        <operation name="operation name" id="operation id" calls="number" renewal-period="seconds" />
    </api>
</quota>
```

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```

### <a name="elements"></a>Öğeler

| Adı      | Açıklama                                                                                                                                                                                                                                                                                  | Gerekli |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| kota     | Kök öğesi.                                                                                                                                                                                                                                                                                | Evet      |
| API       | Ürün içindeki API'lere çağrı kotası uygulamak için bu öğelerden birini veya birkaçını ekleyin. Ürün ve API çağrı kotaları bağımsız olarak uygulanır. API üzerinden `name` veya `id`. Her iki öznitelik `id` sağlanırsa, `name` kullanılır ve yoksayılır.                    | Hayır       |
| Işlem | API içindeki işlemlere çağrı kotası uygulamak için bu öğelerden birini veya birkaçını ekleyin. Ürün, API ve işletme çağrı kotaları bağımsız olarak uygulanır. İşlem ya `name` da `id`. Her iki öznitelik `id` sağlanırsa, `name` kullanılır ve yoksayılır. | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Adı           | Açıklama                                                                                               | Gerekli                                                         | Varsayılan |
| -------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------- |
| ad           | ApI'nin adı veya kotanın uygulandığı işlem.                                             | Evet                                                              | Yok     |
| Bant genişliği      | Belirtilen zaman aralığında izin verilen maksimum toplam kilobayt `renewal-period`sayısı. | Ya `calls` `bandwidth`, veya her ikisi birlikte belirtilmelidir. | Yok     |
| Aramalar          | 'de belirtilen zaman aralığında izin verilen maksimum `renewal-period`toplam çağrı sayısı.     | Ya `calls` `bandwidth`, veya her ikisi birlikte belirtilmelidir. | Yok     |
| yenileme dönemi | Kotanın sıfırlandığı saniye cinsinden zaman dilimi.                                                  | Evet                                                              | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen
-   **İlke kapsamları:** ürün

## <a name="set-usage-quota-by-key"></a><a name="SetUsageQuotaByKey"></a>Kullanım kotalarını anahtara göre ayarlama

> [!IMPORTANT]
> Bu özellik API Yönetimi'nin **Tüketim** katmanında kullanılamaz.

İlke, `quota-by-key` her temel olarak yenilenebilir veya ömür boyu çağrı hacmi ve/veya bant genişliği kotası uygular. Anahtar rasgele bir dize değerine sahip olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır. Hangi isteklerin kotaya doğru sayılması gerektiğini belirtmek için isteğe bağlı artış koşulu eklenebilir. Birden çok ilke aynı anahtar değerini artımlı ise, istek başına yalnızca bir kez artımlanır. Arama sınırına ulaşıldığında, arayan `403 Forbidden` bir yanıt durum kodu alır.

Daha fazla bilgi ve bu iiden örnekler için Azure [API Yönetimi ile Gelişmiş istek azaltma](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/)bölümüne bakın.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<quota-by-key calls="number"
              bandwidth="kilobytes"
              renewal-period="seconds"
              increment-condition="condition"
              counter-key="key value" />

```

### <a name="example"></a>Örnek

Aşağıdaki örnekte, kota arayan IP adresi tarafından anahtarlanır.

```xml
<policies>
    <inbound>
        <base />
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"
                      counter-key="@(context.Request.IpAddress)" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```

### <a name="elements"></a>Öğeler

| Adı  | Açıklama   | Gerekli |
| ----- | ------------- | -------- |
| kota | Kök öğesi. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Adı                | Açıklama                                                                                               | Gerekli                                                         | Varsayılan |
| ------------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------- |
| Bant genişliği           | Belirtilen zaman aralığında izin verilen maksimum toplam kilobayt `renewal-period`sayısı. | Ya `calls` `bandwidth`, veya her ikisi birlikte belirtilmelidir. | Yok     |
| Aramalar               | 'de belirtilen zaman aralığında izin verilen maksimum `renewal-period`toplam çağrı sayısı.     | Ya `calls` `bandwidth`, veya her ikisi birlikte belirtilmelidir. | Yok     |
| karşı anahtar         | Kota ilkesi için kullanılacak anahtar.                                                                      | Evet                                                              | Yok     |
| artış durumu | İsteğin kotaya doğru sayılıp sayılmaması`true`gerektiğini belirten boolean ifadesi ( )             | Hayır                                                               | Yok     |
| yenileme dönemi      | Kotanın sıfırlandığı saniye cinsinden zaman dilimi.                                                  | Evet                                                              | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen
-   **İlke kapsamları:** tüm kapsamlar

## <a name="validate-jwt"></a><a name="ValidateJWT"></a>JWT'yi doğrulayın

İlke, `validate-jwt` belirtilen bir HTTP Üstbilgisinden veya belirli bir sorgu parametresinden çıkarılan bir JWT'nin varlığını ve geçerliliğini zorlar.

> [!IMPORTANT]
> İlke, `validate-jwt` öznitelik belirtilmedikçe ve `exp` `require-expiration-time` `false`'' olarak ayarlanmıyorsa, kayıtlı talebin JWT belirtecine dahil olmasını gerektirir.
> İlke `validate-jwt` HS256 ve RS256 imzalama algoritmalarını destekler. HS256 için anahtar base64 kodlanmış formda ilke içinde satır içi sağlanmalıdır. RS256 için anahtarın Açık Kimlik yapılandırma bitiş noktası üzerinden sağlanması gerekir.
> İlke, `validate-jwt` aşağıdaki şifreleme algoritmaları A128CBC-HS256, A192CBC-HS384, A256CBC-HS512 kullanarak simetrik anahtarlarla şifrelenmiş belirteçleri destekler.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<validate-jwt
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"
    failed-validation-httpcode="http status code to return on failure"
    failed-validation-error-message="error message to return on failure"
    token-value="expression returning JWT token as a string"
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"
    clock-skew="allowed clock skew in seconds"
    output-token-variable-name="name of a variable to receive a JWT object representing successfully validated token">
  <openid-config url="full URL of the configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />
  <issuer-signing-keys>
    <key>base64 encoded signing key</key>
    <!-- if there are multiple keys, then add additional key elements -->
  </issuer-signing-keys>
  <decryption-keys>
    <key>base64 encoded signing key</key>
    <!-- if there are multiple keys, then add additional key elements -->
  </decryption-keys>
  <audiences>
    <audience>audience string</audience>
    <!-- if there are multiple possible audiences, then add additional audience elements -->
  </audiences>
  <issuers>
    <issuer>issuer string</issuer>
    <!-- if there are multiple possible issuers, then add additional issuer elements -->
  </issuers>
  <required-claims>
    <claim name="name of the claim as it appears in the token" match="all|any" separator="separator character in a multi-valued claim">
      <value>claim value as it is expected to appear in the token</value>
      <!-- if there is more than one allowed values, then add additional value elements -->
    </claim>
    <!-- if there are multiple possible allowed values, then add additional value elements -->
  </required-claims>
</validate-jwt>

```

### <a name="examples"></a>Örnekler

#### <a name="simple-token-validation"></a>Basit belirteç doğrulaması

```xml
<validate-jwt header-name="Authorization" require-scheme="Bearer">
    <issuer-signing-keys>
        <key>{{jwt-signing-key}}</key>  <!-- signing key specified as a named value -->
    </issuer-signing-keys>
    <audiences>
        <audience>@(context.Request.OriginalUrl.Host)</audience>  <!-- audience is set to API Management host name -->
    </audiences>
    <issuers>
        <issuer>http://contoso.com/</issuer>
    </issuers>
</validate-jwt>
```

#### <a name="azure-active-directory-token-validation"></a>Azure Etkin Dizin belirteç doğrulaması

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>
        <claim name="id" match="all">
            <value>insert claim here</value>
        </claim>
    </required-claims>
</validate-jwt>
```

#### <a name="azure-active-directory-b2c-token-validation"></a>Azure Active Directory B2C belirteç doğrulaması

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>
        <claim name="id" match="all">
            <value>insert claim here</value>
        </claim>
    </required-claims>
</validate-jwt>
```

#### <a name="authorize-access-to-operations-based-on-token-claims"></a>Belirteç taleplerine dayalı işlemlere erişim yetkisi verme

Bu örnek, belirteç alacakları değerine dayalı işlemlere erişimi yetkilendirmek için [JWT doğrula](api-management-access-restriction-policies.md#ValidateJWT) ilkesinin nasıl kullanılacağını gösterir.

```xml
<validate-jwt header-name="Authorization" require-scheme="Bearer" output-token-variable-name="jwt">
    <issuer-signing-keys>
        <key>{{jwt-signing-key}}</key> <!-- signing key is stored in a named value -->
    </issuer-signing-keys>
    <audiences>
        <audience>@(context.Request.OriginalUrl.Host)</audience>
    </audiences>
    <issuers>
        <issuer>contoso.com</issuer>
    </issuers>
    <required-claims>
        <claim name="group" match="any">
            <value>finance</value>
            <value>logistics</value>
        </claim>
    </required-claims>
</validate-jwt>
<choose>
    <when condition="@(context.Request.Method == "POST" && !((Jwt)context.Variables["jwt"]).Claims["group"].Contains("finance"))">
        <return-response>
            <set-status code="403" reason="Forbidden" />
        </return-response>
    </when>
</choose>
```

### <a name="elements"></a>Öğeler

| Öğe             | Açıklama                                                                                                                                                                                                                                                                                                                                           | Gerekli |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| doğrulamak-jwt        | Kök öğesi.                                                                                                                                                                                                                                                                                                                                         | Evet      |
| Kitle -lere           | Belirteç üzerinde bulunabilecek kabul edilebilir hedef kitle taleplerinin listesini içerir. Birden çok hedef kitle değeri varsa, her değer tüm tükenmiş (bu durumda doğrulama başarısız olana kadar) veya başarılı olana kadar denenir. En az bir hedef kitle belirtilmelidir.                                                                     | Hayır       |
| veren-imza-anahtarları | İmzalı jetonları doğrulamak için kullanılan Base64 kodlanmış güvenlik anahtarlarının listesi. Birden çok güvenlik anahtarı varsa, her anahtar tüm tükenmiş (bu durumda doğrulama başarısız olana kadar) veya başarılı olana kadar (belirteç devrilme için yararlı) denenir. Anahtar öğeler, `id` iddiaya karşı `kid` eşleşen isteğe bağlı bir özelliğe sahiptir.               | Hayır       |
| şifre çözme anahtarları     | Jetonların şifresini çözmek için kullanılan Base64 kodlanmış anahtarların listesi. Birden çok güvenlik anahtarı varsa, tüm anahtarlar tükenene (bu durumda doğrulama başarısız olana) veya bir anahtar başarılı olana kadar her anahtar denenir. Anahtar öğeler, `id` iddiaya karşı `kid` eşleşen isteğe bağlı bir özelliğe sahiptir.                                                 | Hayır       |
| Ihraççılar             | Belirteci veren kabul edilebilir ilkelerin listesi. Birden çok veren değeri varsa, her değer tüm tükenmiş kadar (bu durumda doğrulama başarısız olur) veya başarılı olana kadar denenir.                                                                                                                                         | Hayır       |
| openid-config       | İmza anahtarları ve verenin alınabileceği uyumlu bir Açık Kimlik yapılandırma bitiş noktası belirtmek için kullanılan öğe.                                                                                                                                                                                                                        | Hayır       |
| gerekli talepler     | Geçerli kabul edilebilmek için belirteçte bulunması beklenen taleplerin listesini içerir. `match` Öznitelik, ilkedeki `all` her talep değerine ayarlandığında doğrulamanın başarılı olması için belirteçte bulunması gerekir. `match` Öznitelik en `any` az bir talep için ayarlandığında, doğrulamanın başarılı olabilmesi için belirteçte bulunması gerekir. | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Adı                            | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                                                            | Gerekli                                                                         | Varsayılan                                                                           |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| saat çarpıtma                      | Timespan. Belirteç verenin sistem saatleri ile API Yönetimi örneği arasında beklenen maksimum zaman farkını belirtmek için kullanın.                                                                                                                                                                                                                                                                                                               | Hayır                                                                               | 0 saniye                                                                         |
| başarısız doğrulama-hata-ileti | JWT doğrulama geçmiyorsa HTTP yanıt gövdesinde dönmek için hata iletisi. Bu iletide herhangi bir özel karakter düzgün kaçtı olmalıdır.                                                                                                                                                                                                                                                                                                 | Hayır                                                                               | Varsayılan hata iletisi doğrulama sorununa bağlıdır, örneğin "JWT yok." |
| başarısız doğrulama-httpcode      | JWT doğrulamageçmezse http durum kodu dönmek için.                                                                                                                                                                                                                                                                                                                                                                                         | Hayır                                                                               | 401                                                                               |
| üstbilgi adı                     | Belirteci tutan HTTP üstbilgisinin adı.                                                                                                                                                                                                                                                                                                                                                                                                         | biri `header-name`belirtilmelidir. `query-parameter-name` `token-value` | Yok                                                                               |
| sorgu-parametre-adı            | Belirteci tutan sorgu parametresinin adı.                                                                                                                                                                                                                                                                                                                                                                                                     | biri `header-name`belirtilmelidir. `query-parameter-name` `token-value` | Yok                                                                               |
| belirteç değeri                     | JWT belirteci içeren bir dize döndüren ifade                                                                                                                                                                                                                                                                                                                                                                                                     | biri `header-name`belirtilmelidir. `query-parameter-name` `token-value` | Yok                                                                               |
| id                              | Öğedeki `id` öznitelik, imza doğrulaması için kullanılacak uygun `kid` anahtarı bulmak için belirtecindeki (varsa) taleple eşleşen dizeyi belirtmenize olanak tanır. `key`                                                                                                                                                                                                                                           | Hayır                                                                               | Yok                                                                               |
| match                           | Öğedeki öznitelik, `match` doğrulamanın `claim` başarılı olması için ilkedeki her talep değerinin belirteçte bulunması gerekip gerekmediğini belirtir. Olası değerler şunlardır:<br /><br /> - `all`- Doğrulamanın başarılı olabilmesi için poliçedeki her talep değerinin belirtecinde bulunması gerekir.<br /><br /> - `any`- Doğrulamanın başarılı olabilmesi için belirteçte en az bir talep değeri bulunması gerekir.                                                       | Hayır                                                                               | tümü                                                                               |
| gerektiren-son kullanma-zaman         | Boolean. Belirteçte bir son kullanma talebinin gerekli olup olmadığını belirtir.                                                                                                                                                                                                                                                                                                                                                                               | Hayır                                                                               | true                                                                              |
| gerektiren şema                  | Belirteç şemasının adı, örneğin "Taşıyıcı". Bu öznitelik ayarlandığında, ilke belirtilen şemanın Yetkilendirme üstbilgi değerinde bulunmasını sağlar.                                                                                                                                                                                                                                                                                    | Hayır                                                                               | Yok                                                                               |
| imzalı belirteçler gerektirir           | Boolean. Belirteç imzalanması gerekip gerekmediğini belirtir.                                                                                                                                                                                                                                                                                                                                                                                           | Hayır                                                                               | true                                                                              |
| Ayırıcı                       | Dize. Çok değerli bir talepten bir değer kümesi ayıklamak için kullanılacak bir ayırıcıyı (örneğin"," olarak) belirtir.                                                                                                                                                                                                                                                                                                                                          | Hayır                                                                               | Yok                                                                               |
| url                             | Açık Kimlik yapılandırma meta verilerinin alınabileceği yerden Kimlik yapılandırma uç noktası URL'si açın. Yanıt, URL'de tanımlandığı gibi özelliklere`https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderMetadata`göre olmalıdır: . Azure Etkin Dizin için aşağıdaki `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` URL'yi kullanın: dizin kiracı adınızı `contoso.onmicrosoft.com`n yerine | Evet                                                                              | Yok                                                                               |
| çıkış-belirteç-değişken-adı      | Dize. Başarılı belirteç doğrulaması üzerine bir tür [`Jwt`](api-management-policy-expressions.md) nesnesi olarak belirteç değeri alacak bağlam değişkeninin adı                                                                                                                                                                                                                                                                                     | Hayır                                                                               | Yok                                                                               |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilke [bölümlerinde](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamlarında](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes)kullanılabilir.

-   **İlke bölümleri:** gelen
-   **İlke kapsamları:** tüm kapsamlar

## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışan daha fazla bilgi için bkz:

-   [API Yönetiminde İlkeler](api-management-howto-policies.md)
-   [API'leri Dönüştür](transform-api.md)
-   [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
-   [İlke örnekleri](policy-samples.md)
