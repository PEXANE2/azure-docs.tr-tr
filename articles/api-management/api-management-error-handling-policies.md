---
title: Azure API Yönetimi ilkelerinde hata işleme | Microsoft Dokümanlar
description: Azure API Yönetimi'nde isteklerin işlenmesi sırasında oluşabilecek hata koşullarına nasıl yanıt verebileceğini öğrenin.
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
ms.openlocfilehash: 2c021a6d10c95b58ac444de8ea895ca01371a2b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75902451"
---
# <a name="error-handling-in-api-management-policies"></a>API Management ilkelerinde hata işleme

Azure API `ProxyError` Yönetimi, bir nesne sağlayarak yayıncıların isteklerin işlenmesi sırasında oluşabilecek hata koşullarına yanıt vermesine olanak tanır. Nesneye `ProxyError` bağlam üzerinden [erişilir. LastError](api-management-policy-expressions.md#ContextVariables) özelliği ve `on-error` ilke bölümündeki ilkeler tarafından kullanılabilir. Bu makalede, Azure API Yönetimi'ndeki hata işleme yetenekleri için bir başvuru sağlanmaktadır.

## <a name="error-handling-in-api-management"></a>API Yönetiminde hata işleme

Azure API Yönetimi'ndeki ilkeler, `outbound`aşağıdaki `on-error` örnekte gösterildiği gibi `inbound`, , `backend`ve bölümlere ayrılır.

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

Bir isteğin işlenmesi sırasında, yerleşik adımlar, istek kapsamında olan tüm ilkelerle birlikte yürütülür. Bir hata oluşursa, işleme hemen `on-error` ilke bölümüne atlar.
İlke `on-error` bölümü herhangi bir kapsamda kullanılabilir. API yayımcılar, hatayı olay hub'larına günlüğe kaydetme veya arayanın geri dönmesi için yeni bir yanıt oluşturma gibi özel davranışları yapılandırabilir.

> [!NOTE]
> Bölüm `on-error` varsayılan olarak ilkelerde bulunmaz. `on-error` Bölümü bir ilkeye eklemek için, ilke düzenleyicisinde istenen ilkeye göz atın ve ekleyin. İlkeleri yapılandırma hakkında daha fazla bilgi için [API Yönetimi'ndeki İlkeler'e](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/)bakın.
>
> Bölüm yoksa, `on-error` bir hata koşulu oluşursa arayanlar 400 veya 500 HTTP yanıt iletisi alır.

### <a name="policies-allowed-in-on-error"></a>Hata yapmasına izin verilen ilkeler

İlke bölümünde aşağıdaki ilkeler `on-error` kullanılabilir.

-   [Seçin](api-management-advanced-policies.md#choose)
-   [set değişkeni](api-management-advanced-policies.md#set-variable)
-   [bul-ve-değiştir](api-management-transformation-policies.md#Findandreplacestringinbody)
-   [return-response](api-management-advanced-policies.md#ReturnResponse)
-   [set-üstbilgi](api-management-transformation-policies.md#SetHTTPheader)
-   [set yöntemi](api-management-advanced-policies.md#SetRequestMethod)
-   [set durumu](api-management-advanced-policies.md#SetStatus)
-   [gönderme-istek](api-management-advanced-policies.md#SendRequest)
-   [gönderme-tek yönlü-istek](api-management-advanced-policies.md#SendOneWayRequest)
-   [log-to-eventhub](api-management-advanced-policies.md#log-to-eventhub)
-   [json-to-xml](api-management-transformation-policies.md#ConvertJSONtoXML)
-   [xml-to-json](api-management-transformation-policies.md#ConvertXMLtoJSON)

## <a name="lasterror"></a>LastError

Bir hata oluşur ve denetim `on-error` ilke bölümüne atlar, hata bağlamında depolanır. [ Bölümdeki](api-management-policy-expressions.md#ContextVariables) ilkeler tarafından erişilebilen LastError özelliği. `on-error` LastError aşağıdaki özelliklere sahiptir.

| Adı       | Tür   | Açıklama                                                                                               | Gerekli |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------- | -------- |
| `Source`   | string | Hatanın oluştuğu öğeyi adlandırır. İlke veya yerleşik bir boru hattı adım adı olabilir.      | Evet      |
| `Reason`   | string | Hata işlemede kullanılabilecek makine dostu hata kodu.                                       | Hayır       |
| `Message`  | string | İnsan tarafından okunabilir hata açıklaması.                                                                         | Evet      |
| `Scope`    | string | Hatanın oluştuğu ve "global", "ürün", "api" veya "işlem" olabilecek kapsamın adı | Hayır       |
| `Section`  | string | Hatanın oluştuğu bölüm adı. Olası değerler: "gelen", "arka uç", "giden"veya "on-error".      | Hayır       |
| `Path`     | string | İç içe ilke belirtir, örneğin "seç[3]/when[2]".                                                 | Hayır       |
| `PolicyId` | string | Hatanın `id` oluştuğu ilke de müşteri tarafından belirtilirse özniteliğin değeri             | Hayır       |

> [!TIP]
> Durum koduna bağlam üzerinden erişebilirsiniz. Response.StatusCode.

> [!NOTE]
> Tüm ilkeler, ilkenin temel öğesine eklenebilecek isteğe bağlı `id` bir özniteliğe sahiptir. Bir hata koşulu oluştuğunda bu öznitelik bir ilke içinde varsa, öznitelik değeri `context.LastError.PolicyId` özelliği kullanılarak alınabilir.

## <a name="predefined-errors-for-built-in-steps"></a>Yerleşik adımlar için önceden tanımlanmış hatalar

Aşağıdaki hatalar, yerleşik işleme adımlarının değerlendirilmesi sırasında oluşabilecek hata koşulları için önceden tanımlanır.

| Kaynak        | Koşul                                 | Neden                  | İleti                                                                                                                |
| ------------- | ----------------------------------------- | ----------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| yapılandırma | Uri herhangi bir API veya Operasyon ile eşleşmiyor | İşlem Bulunamadı       | Gelen isteği bir işlemle eşleştireme.                                                                      |
| yetkilendirme | Abonelik anahtarı sağlanmaz             | AbonelikKeyNotFound | Abonelik anahtarının eksik olması nedeniyle erişim reddedildi. Bu API'ye istekte bulunmada abonelik anahtarını eklediğinizden emin olun. |
| yetkilendirme | Abonelik anahtar değeri geçersiz         | AbonelikKeyGeçersiz  | Geçersiz abonelik anahtarı nedeniyle erişim reddedildi. Etkin bir abonelik için geçerli bir anahtar sağladığından emin olun.            |
| birden çok | Alt akış bağlantısı (istemciden API Management ağ geçidine) istek beklemedeyken istemci tarafından iptal edildi | İstemci Bağlantı Hatası | birden çok |
| birden çok | Yukarı bağlantı (bir API Yönetimi ağ geçidinden arka uç hizmetine) kurulmamada veya arka uç tarafından iptal edildi | BackendConnectionFailure | birden çok |
| birden çok | Belirli bir ifadenin değerlendirilmesi sırasında çalışma zamanı özel durumu oluşmuştu | İfadeDeğer Değerlendirme Hatası | birden çok |

## <a name="predefined-errors-for-policies"></a>İlkeler için önceden tanımlanmış hatalar

Aşağıdaki hatalar, ilke değerlendirmesi sırasında oluşabilecek hata koşulları için önceden tanımlanmıştır.

| Kaynak       | Koşul                                                       | Neden                    | İleti                                                                                                                              |
| ------------ | --------------------------------------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| oran sınırı   | Oran sınırı aşıldı                                             | RateLimitAşıldı         | Oran sınırı aşıldı                                                                                                               |
| kota        | Kota aşıldı                                                  | Kota Aşıldı             | Çağrı hacmi kotası aşıldı. Kota xx:xx:xx olarak doldurulacaktır. -veya- Bant genişliği kotası dışında. Kota xx:xx:xx olarak doldurulacaktır. |
| Jsonp        | Geri arama parametre değeri geçersizdir (yanlış karakterler içerir) | Geri AramaParametreGeçersiz  | Geri arama parametresi {callback-parameter-name} değeri geçerli bir JavaScript tanımlayıcısı değildir.                                          |
| ip filtresi    | Arayan IP'yi istekten ayrışdırmak için başarısız olunamadı                          | BaşarısızToParseCallerIP     | Arayan için IP adresi oluşturulamadı. Erişim reddedildi.                                                                        |
| ip filtresi    | Arayan IP izin verilen listede değil                                | CalleripNotAllowed        | Arayan IP adresi {ip-address} giremez. Erişim reddedildi.                                                                        |
| ip filtresi    | Arayan IP engellenen listede                                    | CallerIpBlocked           | Arayan IP adresi engellendi. Erişim reddedildi.                                                                                         |
| kontrol üstbilgi | Gerekli üstbilgi sunulmadı veya değer eksik               | ÜstbilgiNotFound            | Üstbilgi {üstbilgi adı} istekte bulunamadı. Erişim reddedildi.                                                                    |
| kontrol üstbilgi | Gerekli üstbilgi sunulmadı veya değer eksik               | ÜstbilgiValueNotAllowed     | Üstbilgi {üstbilgi adı} değeri {üstbilgi değeri} izin verilmez. Erişim reddedildi.                                                          |
| doğrulamak-jwt | Jwt belirteci istek eksik                                 | TokenNotFound             | JWT istek bulunamadı. Erişim reddedildi.                                                                                         |
| doğrulamak-jwt | İmza doğrulama başarısız oldu                                     | TokenSignatureGeçersiz     | jwt kütüphaneden\><mesaj . Erişim reddedildi.                                                                                          |
| doğrulamak-jwt | Geçersiz hedef kitle                                                | TokenAudienceNotAllowed   | jwt kütüphaneden\><mesaj . Erişim reddedildi.                                                                                          |
| doğrulamak-jwt | Geçersiz veren kuruluş                                                  | TokenIssuerNotAllowed     | jwt kütüphaneden\><mesaj . Erişim reddedildi.                                                                                          |
| doğrulamak-jwt | Belirtecin süresi doldu                                                   | TokenExpired              | jwt kütüphaneden\><mesaj . Erişim reddedildi.                                                                                          |
| doğrulamak-jwt | İmza anahtarı kimlikle çözülmedi                            | TokenSignatureKeyNotFound | jwt kütüphaneden\><mesaj . Erişim reddedildi.                                                                                          |
| doğrulamak-jwt | Belirteçten gerekli talepler eksik                          | TokenClaimNotFound        | JWT belirteci aşağıdaki iddiaları eksik:\><c1 , <c2\>, ... Erişim reddedildi.                                                            |
| doğrulamak-jwt | Talep değerleri uyuşmazlığı                                           | TokenClaimValueNotAllowed | {claim-value} talebine {claim-name} değerine izin verilmez. Erişim reddedildi.                                                             |
| doğrulamak-jwt | Diğer doğrulama hataları                                       | JwtGeçersiz                | jwt kütüphanesinden <mesaj\>                                                                                                          |
| iletme isteği veya gönderme-istek | HTTP yanıt durum kodu ve üstbilgi yapılandırılan zaman ası içinde arka uçtan alınmadı | Zaman aşımı | birden çok |

## <a name="example"></a>Örnek

Bir API ilkesini şu şekilde ayarlama:

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

ve yetkisiz bir istek gönderme aşağıdaki yanıtla sonuçlanır:

![Yetkisiz hata yanıtı](media/api-management-error-handling-policies/error-response.png)

## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışan daha fazla bilgi için bkz:

-   [API Yönetiminde İlkeler](api-management-howto-policies.md)
-   [API'leri Dönüştür](transform-api.md)
-   [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
-   [İlke örnekleri](policy-samples.md)
