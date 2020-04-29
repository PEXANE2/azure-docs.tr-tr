---
title: REST API kullanarak Azure Kaynak durumu olaylarını alın | Microsoft Docs
description: Azure kaynaklarınızın sistem durumu olaylarını almak için Azure REST API 'Lerini kullanın.
ms.topic: conceptual
ms.date: 06/06/2017
ms.openlocfilehash: 6964a6c4e85c38d532b12e730a02c4df73be76e5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77654010"
---
# <a name="get-resource-health-using-the-rest-api"></a>REST API kullanarak Kaynak Durumu alın 

Bu örnek makalede, [azure REST API](/rest/api/azure/)kullanarak aboneliğinizdeki Azure kaynakları için bir sistem durumu olaylarının listesinin nasıl alınacağını gösterilmektedir.

Tüm başvuru belgelerini ve REST API için ek örnekleri [Azure IZLEYICI Rest başvurusunda](/rest/api/monitor)bulabilirsiniz. 

## <a name="build-the-request"></a>İsteği oluşturma

Ve `GET` `2018-06-20`arasındaki `2018-05-16` zaman aralığı için aboneliğinizin sistem durumu olaylarını listelemek üzere aşağıdaki HTTP isteğini kullanın.

```http
https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&%24filter=eventTimestamp%20ge%20'2018-05-16T04%3A36%3A37.6407898Z'%20and%20eventTimestamp%20le%20'2018-06-20T04%3A36%3A37.6407898Z'
```

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki üstbilgiler gereklidir: 

|İstek üst bilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. `application/json` olarak ayarlayın.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir `Bearer` [erişim belirtecine](/rest/api/azure/#authorization-code-grant-interactive-clients) ayarlayın. |  

### <a name="uri-parameters"></a>URI parametreleri

| Adı | Açıklama |
| :--- | :---------- |
| subscriptionId | Bir Azure aboneliğini tanımlayan abonelik KIMLIĞI. Birden çok aboneliğiniz varsa bkz. [birden çok abonelikle çalışma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| api-sürümü | İstek için kullanılacak API sürümü.<br /><br /> Bu belge, yukarıdaki URL 'ye `2015-04-01`dahil edilen api sürümünü içerir.  |
| $filter | Döndürülen sonuç kümesini azaltmak için filtreleme seçeneği. Bu parametre için izin verilen desenler, [etkinlik günlükleri işleminin başvurusunda](/rest/api/monitor/activitylogs/list#uri-parameters)bulunabilir. Gösterilen örnek, 2018-05-16 ile 2018-06-20 arasında bir zaman aralığındaki tüm olayları yakalar |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>İstek gövdesi

Bu işlem için istek gövdesi gerekli değil.

## <a name="handle-the-response"></a>Yanıtı işleme

Durum kodu 200, filtre parametresine karşılık gelen bir sistem durumu olay değerleri listesiyle birlikte döndürülür ve sonraki sonuç sayfasını almak `nextlink` için URI ile birlikte.

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
