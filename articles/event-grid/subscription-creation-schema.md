---
title: Azure Event Grid abonelik şeması
description: Bu makalede, Azure Event Grid bir olaya abone olmak için özellikler açıklanmaktadır. Event Grid abonelik şeması.
ms.topic: reference
ms.date: 07/07/2020
ms.openlocfilehash: f6e33171cbba65cfeaca49ab6a8954be8bb89acb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102199960"
---
# <a name="event-grid-subscription-schema"></a>Event Grid abonelik şeması

Event Grid bir abonelik oluşturmak için, olay aboneliği oluşturma işlemine bir istek gönderirsiniz. Şu biçimi kullanın:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Örneğin, adlı bir kaynak grubunda adlı bir depolama hesabı için bir olay aboneliği oluşturmak için `examplestorage` `examplegroup` aşağıdaki biçimi kullanın:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Olay abonelik adı 3-64 karakter uzunluğunda olmalıdır ve yalnızca a-z, A-Z, 0-9 ve "-" karakterlerini içerebilir. Makalede, isteğin gövdesi için özellikler ve şema açıklanmaktadır.
 
## <a name="event-subscription-properties"></a>Olay aboneliği özellikleri

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| hedef | object | Uç noktasını tanımlayan nesne. |
| filtre | object | Olay türlerini filtrelemek için isteğe bağlı bir alan. |

### <a name="destination-object"></a>hedef nesne

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| endpointType | string | Abonelik için uç nokta türü (Web kancası/HTTP, Olay Hub 'ı veya kuyruğu). | 
| endpointUrl | string | Bu olay aboneliğindeki olaylar için hedef URL. | 

### <a name="filter-object"></a>filtre nesnesi

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| includedEventTypes | array | Olay iletisindeki olay türü, bu olay türü adlarından biriyle tam olarak eşleşiyorsa eşleşir. Olay adı olay kaynağı için kayıtlı olay türü adlarıyla eşleşmediği zaman bir hata oluşturur. Varsayılan değer tüm olay türleriyle eşleşir. |
| subjectBeginsWith | string | Olay iletisindeki Konu alanı için bir önek eşleşmesi filtresi. Varsayılan veya boş dize tümü ile eşleşir. | 
| subjectEndsWith | string | Olay iletisindeki Konu alanına bir sonek eşleşmesi filtresi. Varsayılan veya boş dize tümü ile eşleşir. |
| isSubjectCaseSensitive | string | Filtreler için büyük/küçük harfe duyarlı eşleştirmeyi denetler. |
| enableAdvancedFilteringOnArrays | boolean | Gelişmiş filtrelemede anahtarlar için diziler kullanılmasına izin vermez. Daha fazla bilgi için bkz. [Gelişmiş filtreleme](event-filtering.md#advanced-filtering). |


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
