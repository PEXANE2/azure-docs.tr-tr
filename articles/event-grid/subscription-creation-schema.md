---
title: Azure Event Grid abonelik şeması
description: Bu makalede, Azure Event Grid bir olaya abone olmak için özellikler açıklanmaktadır. Event Grid abonelik şeması.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: reference
ms.date: 01/23/2020
ms.author: babanisa
ms.openlocfilehash: 4bb04d22b762f31a02515549b698030a5267e4cd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76720767"
---
# <a name="event-grid-subscription-schema"></a>Event Grid abonelik şeması

Event Grid bir abonelik oluşturmak için, olay aboneliği oluşturma işlemine bir istek gönderirsiniz. Şu biçimi kullanın:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Örneğin, adlı `examplestorage` `examplegroup`bir kaynak grubunda adlı bir depolama hesabı için bir olay aboneliği oluşturmak için aşağıdaki biçimi kullanın:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Olay abonelik adı 3-64 karakter uzunluğunda olmalıdır ve yalnızca a-z, A-Z, 0-9 ve "-" karakterlerini içerebilir. Makalede, isteğin gövdesi için özellikler ve şema açıklanmaktadır.
 
## <a name="event-subscription-properties"></a>Olay aboneliği özellikleri

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| hedef | object | Uç noktasını tanımlayan nesne. |
| filtre | object | Olay türlerini filtrelemek için isteğe bağlı bir alan. |

### <a name="destination-object"></a>hedef nesne

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| endpointType | string | Abonelik için uç nokta türü (Web kancası/HTTP, Olay Hub 'ı veya kuyruğu). | 
| endpointUrl | string | Bu olay aboneliğindeki olaylar için hedef URL. | 

### <a name="filter-object"></a>filtre nesnesi

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| includedEventTypes | array | Olay iletisindeki olay türü, bu olay türü adlarından biriyle tam olarak eşleşiyorsa eşleşir. Olay adı olay kaynağı için kayıtlı olay türü adlarıyla eşleşmediği zaman bir hata oluşturur. Varsayılan değer tüm olay türleriyle eşleşir. |
| subjectBeginsWith | string | Olay iletisindeki Konu alanı için bir önek eşleşmesi filtresi. Varsayılan veya boş dize tümü ile eşleşir. | 
| subjectEndsWith | string | Olay iletisindeki Konu alanına bir sonek eşleşmesi filtresi. Varsayılan veya boş dize tümü ile eşleşir. |
| isSubjectCaseSensitive | string | Filtreler için büyük/küçük harfe duyarlı eşleştirmeyi denetler. |


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

* Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
