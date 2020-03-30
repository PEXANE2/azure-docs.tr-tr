---
title: REST API'sini kullanarak Azure kaynak durumu olaylarını elde edin | Microsoft Dokümanlar
description: Azure kaynaklarınız için sistem durumu etkinliklerini almak için Azure REST API'lerini kullanın.
ms.topic: conceptual
ms.date: 06/06/2017
ms.openlocfilehash: 6964a6c4e85c38d532b12e730a02c4df73be76e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77654010"
---
# <a name="get-resource-health-using-the-rest-api"></a>REST API'sini kullanarak Kaynak Sağlığı'nı elde edin 

Bu örnek, [Azure REST API'sini](/rest/api/azure/)kullanarak aboneliğinizdeki Azure kaynakları için sistem durumu olaylarının listesini nasıl alınız gösterir.

REST API'si için eksiksiz başvuru belgeleri ve ek örnekler [Azure Monitor REST başvurusunda](/rest/api/monitor)mevcuttur. 

## <a name="build-the-request"></a>İsteği oluşturma

Aboneliğiniz `GET` için sağlık olaylarını aşağıdaki `2018-05-16` `2018-06-20`HTTP isteğini kullanarak.

```http
https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&%24filter=eventTimestamp%20ge%20'2018-05-16T04%3A36%3A37.6407898Z'%20and%20eventTimestamp%20le%20'2018-06-20T04%3A36%3A37.6407898Z'
```

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki üstbilgiler gereklidir: 

|İstek üst bilgisi|Açıklama|  
|--------------------|-----------------|  
|*İçerik Türü:*|Gereklidir. `application/json` olarak ayarlayın.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir `Bearer` [erişim belirtecine](/rest/api/azure/#authorization-code-grant-interactive-clients) ayarlayın. |  

### <a name="uri-parameters"></a>URI parametreleri

| Adı | Açıklama |
| :--- | :---------- |
| subscriptionId | Azure aboneliğini tanımlayan abonelik kimliği. Birden çok aboneliğiniz varsa, [bkz.](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest) |
| api-sürümü | İstek için kullanılacak API sürümü.<br /><br /> Bu belge, yukarıdaki `2015-04-01`URL'de yer alan api sürümünü kapsar.  |
| $filter | Döndürülen sonuç kümesini azaltmak için filtreleme seçeneği. Bu parametre için izin verilebilen [desenler, Etkinlik Günlükleri işlemi için başvuruda](/rest/api/monitor/activitylogs/list#uri-parameters)mevcuttur. Gösterilen örnek, 2018-05-16 ve 2018-06-20 tarihleri arasında tüm olayları yakalar |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>İstek gövdesi

Bu işlem için istek organına gerek yoktur.

## <a name="handle-the-response"></a>Yanıtı işleme

Durum kodu 200, bir sonraki sonuçlar sayfasını almak için bir `nextlink` URI ile birlikte filtre parametresine karşılık gelen sistem durumu olay değerlerinin bir listesiyle birlikte döndürülür.

## <a name="example-response"></a>Örnek yanıt 

```json
{
  "value": [
    {
      "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
      "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
      },
      "id": "/subscriptions/{subscription-id}/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
      "resourceGroupName": "MSSupportGroup",
      "resourceProviderName": {
        "value": "microsoft.support",
        "localizedValue": "microsoft.support"
      },
      "operationName": {
        "value": "microsoft.support/supporttickets/write",
        "localizedValue": "microsoft.support/supporttickets/write"
      },
      "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
      },
      "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
      "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
      "level": "Informational"
    }
  ],
  "nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```
