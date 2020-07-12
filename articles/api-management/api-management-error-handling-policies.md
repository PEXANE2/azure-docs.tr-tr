---
title: Azure API Management ilkelerinde hata işleme | Microsoft Docs
description: Azure API Management isteklerin işlenmesi sırasında oluşabilecek hata koşullarına nasıl yanıt verileceğini öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/10/2020
ms.author: apimpm
ms.openlocfilehash: bddb4ea3759d19d1e122739fb69cf9bf96c66635
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86243554"
---
# <a name="error-handling-in-api-management-policies"></a>API Management ilkelerinde hata işleme

`ProxyError`Azure API Management, bir nesne sağlayarak, yayımcıların isteklerin işlenmesi sırasında oluşabilecek hata koşullarına yanıt vermesini sağlar. `ProxyError`Nesnesine bağlam üzerinden erişilir [. LastError](api-management-policy-expressions.md#ContextVariables) özelliği ve ilke bölümünde ilkeler tarafından kullanılabilir `on-error` . Bu makalede, Azure API Management 'daki hata işleme özelliklerine yönelik bir başvuru sağlanmaktadır.

## <a name="error-handling-in-api-management"></a>API Management işlenirken hata oluştu

Azure API Management ilkeleri `inbound` , `backend` `outbound` `on-error` Aşağıdaki örnekte gösterildiği gibi,, ve bölümlerine bölünür.

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is
         forwarded to the backend service go here -->
    </backend>
    <outbound>
      <!-- statements to be applied to the response go here -->
    </outbound>
    <on-error>
        <!-- statements to be applied if there is an error
             condition go here -->
  </on-error>
</policies>
```

Bir isteğin işlenmesi sırasında, yerleşik adımlar, istek kapsamında olan herhangi bir ilke ile birlikte yürütülür. Bir hata oluşursa, işleme hemen `on-error` ilke bölümüne atlar.
`on-error`İlke bölümü herhangi bir kapsamda kullanılabilir. API yayımcıları, hatayı olay hub 'larına kaydetme veya arayana dönmek için yeni bir yanıt oluşturma gibi özel davranışları yapılandırabilir.

> [!NOTE]
> `on-error`Bölümü varsayılan olarak ilkelerde yer almıyor. `on-error`Bölümü bir ilkeye eklemek için, ilke düzenleyicisinde istenen ilkeye gidin ve ekleyin. İlkeleri yapılandırma hakkında daha fazla bilgi için bkz. [API Management ilkeleri](./api-management-howto-policies.md).
>
> `on-error`Bölüm yoksa, arayanlar bir hata durumu oluşursa 400 veya 500 http yanıt iletisi alır.

### <a name="policies-allowed-in-on-error"></a>Hatada izin verilen ilkeler

İlke bölümünde aşağıdaki ilkeler kullanılabilir `on-error` .

-   ['yu](api-management-advanced-policies.md#choose)
-   [Set değişkeni](api-management-advanced-policies.md#set-variable)
-   [bul ve Değiştir](api-management-transformation-policies.md#Findandreplacestringinbody)
-   [dönüş-yanıt](api-management-advanced-policies.md#ReturnResponse)
-   [Set-Header](api-management-transformation-policies.md#SetHTTPheader)
-   [Set-yöntemi](api-management-advanced-policies.md#SetRequestMethod)
-   [durum ayarla](api-management-advanced-policies.md#SetStatus)
-   [gönderme isteği](api-management-advanced-policies.md#SendRequest)
-   [tek yönlü istek gönder](api-management-advanced-policies.md#SendOneWayRequest)
-   [-eventhub arası](api-management-advanced-policies.md#log-to-eventhub)
-   [JSON-XML](api-management-transformation-policies.md#ConvertJSONtoXML)
-   [XML-JSON](api-management-transformation-policies.md#ConvertXMLtoJSON)
-   [limit-eşzamanlılık](api-management-advanced-policies.md#LimitConcurrency)
-   [sahte yanıt](api-management-advanced-policies.md#mock-response)
-   [retry](api-management-advanced-policies.md#Retry)
-   [izlemesinin](api-management-advanced-policies.md#Trace)

## <a name="lasterror"></a>LastError

Bir hata oluştuğunda ve denetim ilke bölümüne atdığında `on-error` , hata [bağlamda saklanır. LastError](api-management-policy-expressions.md#ContextVariables) özelliği, bölümünde ilkeler tarafından erişilebilecek `on-error` . LastError aşağıdaki özelliklere sahiptir.

| Ad       | Tür   | Açıklama                                                                                               | Gerekli |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------- | -------- |
| `Source`   | string | Hatanın oluştuğu öğeyi adlandırır. İlke ya da yerleşik bir ardışık düzen adımı adı olabilir.      | Evet      |
| `Reason`   | string | Hata işlemede kullanılabilecek makine dostu hata kodu.                                       | Hayır       |
| `Message`  | string | İnsan tarafından okunabilen hata açıklaması.                                                                         | Evet      |
| `Scope`    | string | Hatanın oluştuğu kapsamın adı ve "genel", "ürün", "API" veya "işlem" olabilir | Hayır       |
| `Section`  | string | Hatanın oluştuğu bölüm adı. Olası değerler: "gelen", "arka uç", "giden" veya "hata üzerinde".      | Hayır       |
| `Path`     | string | İç içe geçmiş ilkeyi belirtir, örneğin "SELECT [3]/, [2]".                                                 | Hayır       |
| `PolicyId` | string | `id`Hatanın oluştuğu ilkede, müşteri tarafından belirtilmişse özniteliğin değeri             | Hayır       |

> [!TIP]
> Durum koduna bağlam aracılığıyla erişebilirsiniz. Response. StatusCode.

> [!NOTE]
> Tüm ilkelerin `id` , ilkenin kök öğesine eklenebilen bir isteğe bağlı özniteliği vardır. Bir hata durumu oluştuğunda bu öznitelik bir ilkede mevcutsa özniteliğin değeri özelliği kullanılarak alınabilir `context.LastError.PolicyId` .

## <a name="predefined-errors-for-built-in-steps"></a>Yerleşik adımlar için önceden tanımlanmış hatalar

Aşağıdaki hatalar, yerleşik işleme adımlarının değerlendirmesi sırasında oluşabilecek hata koşulları için önceden tanımlanmıştır.

| Kaynak        | Koşul                                 | Neden                  | İleti                                                                                                                |
| ------------- | ----------------------------------------- | ----------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| yapılandırma | URI, herhangi bir API veya Işlemle eşleşmiyor | OperationNotFound       | Gelen istek bir işleme dönüştürülemiyor.                                                                      |
| yetkilendirme | Abonelik anahtarı sağlanmadı             | SubscriptionKeyNotFound | Eksik abonelik anahtarı nedeniyle erişim reddedildi. Bu API 'ye istek yaparken abonelik anahtarını eklediğinizden emin olun. |
| yetkilendirme | Abonelik anahtarı değeri geçersiz         | Subscriptionkeygeçersiz  | Geçersiz abonelik anahtarı nedeniyle erişim reddedildi. Etkin bir abonelik için geçerli bir anahtar sağladığınızdan emin olun.            |
| birden çok | İstek beklemede iken, bir istemciden bir API Management ağ geçidine olan akış bağlantısı, istemci tarafından iptal edildi | ClientConnectionFailure | birden çok |
| birden çok | Yukarı akış bağlantısı (bir API Management ağ geçidinden arka uç hizmetine) kurulmadı veya arka uç tarafından iptal edildi | BackendConnectionFailure | birden çok |
| birden çok | Belirli bir ifadenin değerlendirmesi sırasında çalışma zamanı özel durumu oluştu | ExpressionValueEvaluationFailure | birden çok |

## <a name="predefined-errors-for-policies"></a>İlkeler için önceden tanımlanmış hatalar

Aşağıdaki hatalar, ilke değerlendirmesi sırasında oluşabilecek hata koşulları için önceden tanımlanmıştır.

| Kaynak       | Koşul                                                       | Neden                    | İleti                                                                                                                              |
| ------------ | --------------------------------------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| hız limiti   | Hız sınırı aşıldı                                             | Ratelimitexcebaşında         | Hız sınırı aşıldı                                                                                                               |
| kota        | Kota aşıldı                                                  | Quotaaştı             | Çağrı hacmi kotası aşıldı. Kota xx: xx: xx içinde replenished olacaktır. -veya-bant genişliği kotası. Kota xx: xx: xx içinde replenished olacaktır. |
| JSONP        | Geri çağırma parametre değeri geçersiz (yanlış karakterler içeriyor) | Callbackparametergeçersiz  | {Callback-Parameter-Name} geri çağırma parametresinin değeri geçerli bir JavaScript tanımlayıcısı değil.                                          |
| IP filtresi    | Çağıran IP, istekten ayrıştırılamadı                          | Failedtoparsecallerıp     | Çağıran için IP adresi oluşturulamadı. Erişim reddedildi.                                                                        |
| IP filtresi    | Arayan IP 'si izin verilenler listesinde değil                                | Callerıpnotallowed        | {IP-Address} çağıran IP adresine izin verilmez. Erişim reddedildi.                                                                        |
| IP filtresi    | Çağıran IP engellenen listede                                    | Calleripengellenen           | Çağıran IP adresi engellendi. Erişim reddedildi.                                                                                         |
| onay üst bilgisi | Gerekli üst bilgi sunulmadı veya değer eksik               | HeaderNotFound            | İstekte {Header-name} üst bilgisi bulunamadı. Erişim reddedildi.                                                                    |
| onay üst bilgisi | Gerekli üst bilgi sunulmadı veya değer eksik               | HeaderValueNotAllowed     | {Header-value} öğesinin üst bilgi {Header-Name} değerine izin verilmez. Erişim reddedildi.                                                          |
| Validate-JWT | İstekte JWT belirteci eksik                                 | TokenNotFound             | İstekte JWT bulunamadı. Erişim reddedildi.                                                                                         |
| Validate-JWT | İmza doğrulama başarısız oldu                                     | Tokensignaturegeçersiz     | JWT kitaplığından ileti <\> . Erişim reddedildi.                                                                                          |
| Validate-JWT | Geçersiz hedef kitle                                                | TokenAudienceNotAllowed   | JWT kitaplığından ileti <\> . Erişim reddedildi.                                                                                          |
| Validate-JWT | Geçersiz veren                                                  | Tokenıssuernotallowed     | JWT kitaplığından ileti <\> . Erişim reddedildi.                                                                                          |
| Validate-JWT | Belirtecin süresi doldu                                                   | TokenExpired              | JWT kitaplığından ileti <\> . Erişim reddedildi.                                                                                          |
| Validate-JWT | İmza anahtarı ID tarafından çözümlenemedi                            | TokenSignatureKeyNotFound | JWT kitaplığından ileti <\> . Erişim reddedildi.                                                                                          |
| Validate-JWT | Gerekli talepler belirteçte eksik                          | TokenClaimNotFound        | JWT belirtecinde şu talepler eksik: <C1 \> , <C2 \> ,... Erişim reddedildi.                                                            |
| Validate-JWT | Talep değerleri uyumsuzluğu                                           | TokenClaimValueNotAllowed | {Claim-Value} talebinin {Claim-Name} değerine izin verilmez. Erişim reddedildi.                                                             |
| Validate-JWT | Diğer doğrulama sorunları                                       | Jwtgeçersiz                | JWT kitaplığından ileti <\>                                                                                                          |
| ilet-istek veya gönderme isteği | Yapılandırılmış zaman aşımı içinde arka uçta HTTP yanıt durum kodu ve üst bilgileri alınmadı | Zaman aşımı | birden çok |

## <a name="example"></a>Örnek

API ilkesini şu şekilde ayarlama:

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
    <on-error>
        <set-header name="ErrorSource" exists-action="override">
            <value>@(context.LastError.Source)</value>
        </set-header>
        <set-header name="ErrorReason" exists-action="override">
            <value>@(context.LastError.Reason)</value>
        </set-header>
        <set-header name="ErrorMessage" exists-action="override">
            <value>@(context.LastError.Message)</value>
        </set-header>
        <set-header name="ErrorScope" exists-action="override">
            <value>@(context.LastError.Scope)</value>
        </set-header>
        <set-header name="ErrorSection" exists-action="override">
            <value>@(context.LastError.Section)</value>
        </set-header>
        <set-header name="ErrorPath" exists-action="override">
            <value>@(context.LastError.Path)</value>
        </set-header>
        <set-header name="ErrorPolicyId" exists-action="override">
            <value>@(context.LastError.PolicyId)</value>
        </set-header>
        <set-header name="ErrorStatusCode" exists-action="override">
            <value>@(context.Response.StatusCode.ToString())</value>
        </set-header>
        <base />
    </on-error>
</policies>
```

ve yetkisiz bir istek göndermek aşağıdaki yanıta neden olur:

![Yetkisiz hata yanıtı](media/api-management-error-handling-policies/error-response.png)

## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışma hakkında daha fazla bilgi için bkz.

-   [API Management ilkeler](api-management-howto-policies.md)
-   [API dönüştürme](transform-api.md)
-   İlke deyimlerinin ve ayarlarının tam listesi için [Ilke başvurusu](./api-management-policies.md)
-   [İlke örnekleri](policy-samples.md)
