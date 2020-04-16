---
title: Olay Izgara kaynağı olarak Azure aboneliği
description: Azure Olay Ağıtı ile abonelik etkinlikleri için sağlanan özellikleri açıklar
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: reference
ms.date: 04/09/2020
ms.author: spelluru
ms.openlocfilehash: fa88fe4e05ac968588a65d67a2f075bcae48ba7a
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393221"
---
# <a name="azure-subscription-as-an-event-grid-source"></a>Olay Izgara kaynağı olarak Azure aboneliği

Bu makalede, Azure abonelik olayları nın özellikleri ve şeması sağlanmaktadır.Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

Azure abonelikleri ve kaynak grupları aynı etkinlik türlerini yayır. Olay türleri kaynak değişiklikleri veya eylemlerle ilişkilidir. Birincil fark, kaynak gruplarının kaynak grubu içindeki kaynaklar için olaylar yayıyor olması ve Azure abonelikleri abonelik genelinde kaynaklar için olaylar yontmaktadır.

Kaynak olayları PUT, PATCH, POST ve DELETE işlemleri `management.azure.com`için oluşturulur. GET işlemleri etkinlik oluşturmaz. Veri düzlemine gönderilen `myaccount.blob.core.windows.net`işlemler (gibi) olay oluşturmaz. Eylem olayları, bir kaynağın anahtarlarını listeleme gibi işlemler için olay verileri sağlar.

Bir Azure aboneliği için etkinliklere abone olduğunuzda, bitiş noktanız bu abonelik için tüm etkinlikleri alır. Olaylar, sanal bir makineyi güncelleştirme gibi görmek istediğiniz olayları ve dağıtım geçmişinde yeni bir giriş yazma gibi sizin için önemli olmayan olayları da içerebilir. Tüm olayları bitiş noktanızda alabilir ve işlemek istediğiniz olayları işleyen kod yazabilirsiniz. Veya olay aboneliği ni oluştururken bir filtre ayarlayabilirsiniz.

Olayları programlı bir şekilde işlemek `operationName` için, değeri bakarak olayları sıralayabilirsiniz. Örneğin, olay bitiş noktanız yalnızca eşit `Microsoft.Compute/virtualMachines/write` olan veya `Microsoft.Storage/storageAccounts/write`.

Olay konusu, işlemin hedefi olan kaynağın kaynak kimliğidir. Bir kaynak için olayları filtrelemek için, olay aboneliği oluştururken bu kaynak kimliğini sağlayın. Kaynak türüne göre filtre uygulayın, aşağıdaki biçimde bir değer kullanın:`/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`


## <a name="event-grid-event-schema"></a>Olay Izgara olay şeması

### <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Azure abonelikleri, VM oluşturulduğunda veya bir depolama hesabının silindiği zaman gibi Azure Kaynak Yöneticisi'nden yönetim olayları yayar.

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Resources.ResourceActionCancel | Kaynak üzerindeki eylem iptal edildiğinde yükseltilir. |
| Microsoft.Resources.ResourceActionFailure | Kaynak üzerindeki eylem başarısız olduğunda yükseltildi. |
| Microsoft.Resources.ResourceActionBaşarı | Kaynak üzerindeki eylem başarılı olduğunda yükseltilir. |
| Microsoft.Resources.ResourceDeleteCancel | Silme işlemi iptal edildiğinde yükseltilir. Bu olay, şablon dağıtımı iptal edildiğinde gerçekleşir. |
| Microsoft.Resources.ResourceDeleteFailure | Silme işlemi başarısız olduğunda yükseltildi. |
| Microsoft.Resources.ResourceDeleteSuccess | Silme işlemi başarılı olduğunda yükseltilir. |
| Microsoft.Resources.ResourceWriteCancel | Oluşturma veya güncelleştirme işlemi iptal edildiğinde yükseltilir. |
| Microsoft.Resources.ResourceWriteFailure | Oluşturma veya güncelleştirme işlemi başarısız olduğunda yükseltildi. |
| Microsoft.Resources.ResourceWriteSuccess | Oluşturma veya güncelleştirme işlemi başarılı olduğunda yükseltilir. |

### <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, **ResourceWriteSuccess** olayının şeasını gösterir. Aynı şema KaynakYazma Hatası ve **ResourceWriteCancel** için farklı `eventType`değerlere sahip olaylar için kullanılır. **ResourceWriteFailure**

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
}]
```

Aşağıdaki örnek, **ResourceDeleteSuccess** olayının şemasını gösterir. Aynı şema **KaynakDeleteFailure** ve **ResourceDeleteCancel** için farklı değerlere sahip olaylar için `eventType`kullanılır.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
}]
```

Aşağıdaki örnek, **ResourceActionSuccess** olayının şeasını gösterir. Aynı şema **KaynakEylemFailure** ve **ResourceActioniçin** farklı değerlere `eventType`sahip olaylar için kullanılır.

```json
[{   
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
  "eventType": "Microsoft.Resources.ResourceActionSuccess",
  "eventTime": "2018-10-08T22:46:22.6022559Z",
  "id": "{ID}",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
      "action": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
      "evidence": {
        "role": "Contributor",
        "roleAssignmentScope": "/subscriptions/{subscription-id}",
        "roleAssignmentId": "{ID}",
        "roleDefinitionId": "{ID}",
        "principalId": "{ID}",
        "principalType": "ServicePrincipal"
      }     
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "aio": "{token}",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/identityprovider": "{URL}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",       "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "POST",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey/listKeys?api-version=2017-04-01"
    },
    "resourceProvider": "Microsoft.EventHub",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
    "operationName": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}" 
}]
```

### <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Abonelik olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| yetkilendirme | object | İşlem için istenen yetkilendirme. |
| Iddia | object | İddiaların özellikleri. Daha fazla bilgi için [JWT belirtimine](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)bakın. |
| correlationId | string | Sorun giderme için bir işlem kimliği. |
| httpİstek | object | Operasyonun detayları. Bu nesne yalnızca varolan bir kaynağı güncellerken veya bir kaynağı silerken dahil edilir. |
| resourceSağlayıcı | string | İşlem için kaynak sağlayıcısı. |
| resourceUri | string | İşlemdeki kaynağın URI'si. |
| operationName | string | Yapılan operasyon. |
| durum | string | İşlemin durumu. |
| subscriptionId | string | Kaynağın abonelik kimliği. |
| tenantId | string | Kaynağın kiracı kimliği. |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık |Açıklama  |
|---------|---------|
| [Öğretici: Olay Izgarave Microsoft Ekipleri ile Azure Otomasyonu](ensure-tags-exists-on-new-virtual-machines.md) |Bir olay gönderen sanal bir makine oluşturun. Olay, sanal makineyi etiketleyen bir Otomasyon runbook'u tetikler ve microsoft teams kanalına gönderilen bir iletiyi tetikler. |
| [Nasıl: portal üzerinden etkinliklere abone](subscribe-through-portal.md) | Azure aboneliği için etkinliklere abone olmak için portalı kullanın. |
| [Azure CLI: Azure aboneliği için etkinliklere abone olun](./scripts/event-grid-cli-azure-subscription.md) |Azure aboneliği için Olay Izgara aboneliği oluşturan ve olayları Bir WebHook'a gönderen örnek komut dosyası. |
| [PowerShell: Azure aboneliği için etkinliklere abone olun](./scripts/event-grid-powershell-azure-subscription.md)| Azure aboneliği için Olay Izgara aboneliği oluşturan ve olayları Bir WebHook'a gönderen örnek komut dosyası. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için olay [ızgarası nedir'e bakın?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
