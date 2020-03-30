---
title: Pazar yeri ölçüm hizmeti API'leri | Azure Marketi
description: Azure Marketi'ndeki SaaS teklifleri için kullanım etkinliği.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: 315f36e5aed9dee0a89e1f9f504b18a6bed806e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275756"
---
# <a name="marketplace-metering-service-apis"></a>Market ölçüm hizmeti API’leri

Kullanım olayı API belirli bir satın alınan varlık için kullanım olayları yayıltmak için izin verir. Kullanım olayı isteği, teklifi yayımlarken yayımcı tarafından tanımlanan ölçüm hizmetleri boyutuna başvurur.

## <a name="usage-event"></a>Kullanım olayı

**MESAJ**:`https://marketplaceapi.microsoft.com/api/usageEvent?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|            |          |
| ---------- | ---------------------- |
| `ApiVersion` | Bu istek için kullanılacak işlemin sürümü. Son API sürümü 2018-08-31 olduğunu. |

*İstek üstleri:*

| İçerik türü       | `application/json`    |
| ------------------ | ---------------------------- |
| `x-ms-requestid`     | İstemciden gelen isteği izlemek için benzersiz dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
| `x-ms-correlationid` | İstemci üzerinde işlem için benzersiz dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
| `authorization`   | [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) Not: HTTP isteği yaparken, `Bearer` başvurulan bağlantıdan elde edilen belirteç önek. |

*Istek:*

```json
{
  "resourceId": "Identifier of the resource against which usage is emitted",
  "quantity": 5.0,
  "dimension": "Dimension identifier",
  "effectiveStartTime": "Time in UTC when the usage event occurred",
  "planId": "Plan associated with the purchased offer"
}
```

### <a name="responses"></a>Yanıtlar

Ürün Kodu: 200<br>
Tamam 

```json
{
  "usageEventId": "Unique identifier associated with the usage event",
  "status": "Accepted",
  "messageTime": "Time this message was created in UTC",
  "resourceId": "Identifier of the resource against which usage is emitted",
  "quantity": 5.0,
  "dimension": "Dimension identifier",
  "effectiveStartTime": "Time in UTC when the usage event occurred",
  "planId": "Plan associated with the purchased offer"
}
```

Ürün Kodu: 400 <br>
Kötü istek, eksik veya geçersiz veri sağlanan veya süresi dolmuş

```json
{
  "message": "One or more errors have occurred.",
  "target": "usageEventRequest",
  "details": [
    {
      "message": "The resourceId is required.",
      "target": "ResourceId",
      "code": "BadArgument"
    }
  ],
  "code": "BadArgument"
}
```

Ürün Kodu: 403<br>
Kötü istek, eksik veya geçersiz veri sağlanan veya süresi dolmuş

```json
{
  "code": "Forbidden",
  "message": "User is not allowed authorized to call this"
}
```

Ürün Kodu: 409<br>
Çakışma, kullanım kaynağı kimliği için kullanım çağrısı aldığımızda ve zaten var olan etkili kullanım. Yanıt, kabul `additionalInfo` edilen ileti hakkında bilgi içeren alan içerir.

```json
{
  "code": "Conflict",
  "additionalInfo": {
    "usageEventId": "Unique identifier associated with the usage event",
    "status": "Accepted|NotProcessed|Expired",
    "messageTime": "Time this message was created in UTC",
    "resourceId": "Identifier of the resource against which usage is emitted",
    "quantity": 5.0,
    "dimension": "Dimension identifier",
    "effectiveStartTime": "Time in UTC when the usage event occurred",
    "planId": "Plan associated with the purchased offer"
  }
}
```

## <a name="batch-usage-event"></a>Toplu kullanım olayı

Toplu kullanım olayı API'si, aynı anda birden fazla satın alınan varlık için kullanım olayları yayıltmaya olanak tanır. Toplu kullanım olayı isteği, teklifi yayımlarken yayımcı tarafından tanımlanan ölçüm hizmetleri boyutuna başvurur.

>[!Note]
>Microsoft'un ticari pazara birden fazla SaaS teklifi kaydedebilirsiniz. Her kayıtlı SaaS teklifi, kimlik doğrulama ve yetkilendirme amacıyla kaydedilmiş benzersiz bir Azure AD uygulamasına sahiptir. Toplu olarak yayılan etkinlikler, teklifi kaydederken aynı Azure AD uygulamasına sahip tekliflere ait olmalıdır.

**POSTA:**`https://marketplaceapi.microsoft.com/api/batchUsageEvent?api-version=<ApiVersion>`

*Sorgu parametreleri:*

|            |     |
| ---------- | -------------------- |
| `ApiVersion` | Bu istek için kullanılacak işlemin sürümü. Son API sürümü 2018-08-31 olduğunu. |

*İstek üstleri:*

| İçerik türü       | `application/json`       |
| ------------------ | ------ |
| `x-ms-requestid`     | İstemciden gelen isteği izlemek için benzersiz dize değeri, tercihen bir GUID. Bu değer sağlanmazsa, bir oluşturulur ve yanıt üstbilgisinde sağlanır. |
| `x-ms-correlationid` | İstemci üzerinde işlem için benzersiz dize değeri. Bu parametre, istemci çalışmasından gelen tüm olayları sunucu tarafındaki olaylarla ilişkilendirer. Bu değer sağlanmazsa, bir tanesi oluşturulur ve yanıt üstbilgilerinde sağlanır. |
| `authorization`      | [JSON web belirteci (JWT) taşıyıcı belirteci alın.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) Not: HTTP isteği yaparken, `Bearer` başvurulan bağlantıdan elde edilen belirteç önek.  |

*Istek:*
```json
{
  "request": [
    {
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer"
    },
    {
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer"
    }
  ]
}
```
### <a name="responses"></a>Yanıtlar

Ürün Kodu: 200<br>
Tamam

```json
{
  "count": 2,
  "result": [
    {
      "usageEventId": "Unique identifier associated with the usage event",
      "status": "Accepted|Expired|Duplicate|Error|ResourceNotFound|ResourceNotAuthorized|InvalidDimension|BadArgument",
      "messageTime": "Time this message was created in UTC",
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer",
      "error": "Error object (optional)"
    },
    {
      "usageEventId": "Unique identifier associated with the usage event",
      "status": "Accepted|Expired|Duplicate|Error|ResourceNotFound|ResourceNotAuthorized|InvalidDimension|BadArgument",
      "messageTime": "Time this message was created in UTC",
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer",
      "error": "Error object (optional)"
    }
  ]
}
```

API yanıtında `BatchUsageEvent` başvurulan durum kodunun açıklaması:

| Durum kodu  | Açıklama |
| ---------- | -------------------- |
| `Accepted` | Kabul edilen kod. |
| `Expired` | Süresi dolmuş kullanım. |
| `Duplicate` | Yinelenen kullanım sağlandı. |
| `Error` | Hata kodu. |
| `ResourceNotFound` | Sağlanan kullanım kaynağı geçersizdir. |
| `ResourceNotAuthorized` | Bu kaynağın kullanımını sağlama yetkiniz yok. |
| `InvalidDimension` | Bu teklif/plan için kullanım dan geçirilen boyut geçersizdir. |
| `InvalidQuantity` | Geçirilen miktar 0'<. |
| `BadArgument` | Giriş eksik veya biçimsiz. |

Ürün Kodu: 400<br>
Kötü istek, eksik veya geçersiz veri sağlanan veya Süresi Doldu

```json
{
  "message": "One or more errors have occurred.",
  "target": "usageEventRequest",
  "details": [
    {
      "message": "Invalid data format.",
      "target": "usageEventRequest",
      "code": "BadArgument"
    }
  ],
  "code": "BadArgument"
}
```
Ürün Kodu: 403<br>
Kullanıcı bu aramayı yapmak için yetkisiz

```json
{
  "code": "Forbidden",
  "message": "User is not allowed to call this"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için Bkz. [SaaS tarifeli faturalandırma.](./saas-metered-billing.md)
