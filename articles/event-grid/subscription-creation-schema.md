---
title: Azure Olay Izgarası abonelik şeması
description: Bu makalede, Azure Olay Ağıt'ı olan bir etkinliğe abone olma özellikleri açıklanmaktadır. Olay Grid abonelik şeması.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: reference
ms.date: 01/23/2020
ms.author: babanisa
ms.openlocfilehash: 4bb04d22b762f31a02515549b698030a5267e4cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720767"
---
# <a name="event-grid-subscription-schema"></a>Olay Izgara abonelik şeması

Olay Izgara aboneliği oluşturmak için Olay Oluştur aboneliği işlemine istek gönderirsiniz. Şu biçimi kullanın:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Örneğin, adlı `examplestorage` `examplegroup`kaynak grubunda adı geçen bir depolama hesabı için olay aboneliği oluşturmak için aşağıdaki biçimi kullanın:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Olay Aboneliği adı 3-64 karakter uzunluğunda olmalı ve yalnızca a-z, A-Z, 0-9 ve "-" içerebilir. Makalede, isteğin gövdesi için özellikleri ve şema açıklanmaktadır.
 
## <a name="event-subscription-properties"></a>Olay abonelik özellikleri

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| hedef | object | Bitiş noktasını tanımlayan nesne. |
| filtre | object | Olay türlerini filtreleme için isteğe bağlı bir alan. |

### <a name="destination-object"></a>hedef nesne

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| endpointType | string | Aboneliğin bitiş noktası türü (webhook/HTTP, Event Hub veya kuyruk). | 
| endpointUrl | string | Bu etkinlik aboneliğindeki etkinliklerin hedef URL'si. | 

### <a name="filter-object"></a>filtre nesnesi

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| dahilEventTypes | array | Olay iletisindeki olay türü, bu olay türü adlarından biriyle tam olarak eşleşince eşleştirin. Olay adı, olay kaynağıiçin kayıtlı olay türü adlarıyla eşleşmediğinde hata kaldırır. Varsayılan tüm olay türleri eşleşir. |
| subjectBeginsWith | string | Olay iletisinde konu alanına bir önek eşleştirme filtresi. Varsayılan veya boş dize tümünü eşleşir. | 
| subjectEndsWith | string | Olay iletisinde konu alanına sonek eşleme filtresi. Varsayılan veya boş dize tümünü eşleşir. |
| isSubjectCaseHassas | string | Filtreler için büyük/küçük harf duyarlı eşleştirmeyi denetler. |


## <a name="example-subscription-schema"></a>Örnek abonelik şeması

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "Microsoft.Storage.BlobCreated", "Microsoft.Storage.BlobDeleted" ],
      "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "isSubjectCaseSensitive ": "true"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay Izgarasına giriş için [olay ızgarası nedir'e bakın?](overview.md)
