---
title: CloudEvents şemasındaki etkinliklerle Azure Etkinlik Ağıt'ı kullanma
description: Azure Etkinlik Ağıt'taki etkinlikler için CloudEvents şemasını nasıl kullanacağımı açıklar. Hizmet, Bulut Etkinlikleri'nin JSON uygulamasındaki olayları destekler.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: babanisa
ms.openlocfilehash: 127095bef2c67a93097bf90bea54ca1b44b16c58
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394390"
---
# <a name="cloudevents-v10-schema-with-event-grid"></a>Olay Izgara ile CloudEvents v1.0 şema

Varsayılan olay [şemasına](event-schema.md)ek olarak, Azure Event [Grid, CloudEvents v1.0](https://github.com/cloudevents/spec/blob/v1.0/json-format.md) ve [HTTP protokol bağlamanın](https://github.com/cloudevents/spec/blob/v1.0/http-protocol-binding.md)JSON uygulamasındaki olayları doğal olarak destekler. [CloudEvents](https://cloudevents.io/) olay verilerini açıklamak için açık bir [belirtimdir.](https://github.com/cloudevents/spec/blob/v1.0/spec.md)

CloudEvents, yayımlama ve bulut tabanlı etkinlikler tüketmek için ortak bir olay şeması sağlayarak birlikte çalışabilirliği kolaylaştırır. Bu şema, tek tip takımlama, olayları işleme & yönlendirmenin standart yolları ve dış olay şemasını deserialize etmenin evrensel yolları sağlar. Ortak bir şema ile, daha kolay platformlar arasında iş entegre edebilirsiniz.

CloudEvents, Microsoft da dahil olmak üzere birçok [ortak çalışan](https://github.com/cloudevents/spec/blob/master/community/contributors.md)tarafından Cloud Native [Computing Foundation](https://www.cncf.io/)aracılığıyla oluşturulmaktadır. Şu anda sürüm 1.0 olarak kullanılabilir.

Bu makalede, Olay Grid ile CloudEvents şeması açıklanmaktadır.

## <a name="sample-event-using-cloudevents-schema"></a>CloudEvents şemakullanarak örnek olay

Aşağıda CloudEvents biçiminde bir Azure Blob Depolama olayı örneği verilmiştir:

``` JSON
{
    "specversion": "1.0",
    "type": "Microsoft.Storage.BlobCreated",  
    "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}",
    "id": "9aeb0fdf-c01e-0131-0922-9eb54906e209",
    "time": "2019-11-18T15:13:39.4589254Z",
    "subject": "blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "dataschema": "#",
    "data": {
        "api": "PutBlockList",
        "clientRequestId": "4c5dd7fb-2c48-4a27-bb30-5361b5de920a",
        "requestId": "9aeb0fdf-c01e-0131-0922-9eb549000000",
        "eTag": "0x8D76C39E4407333",
        "contentType": "image/png",
        "contentLength": 30699,
        "blobType": "BlockBlob",
        "url": "https://gridtesting.blob.core.windows.net/testcontainer/{new-file}",
        "sequencer": "000000000000000000000000000099240000000000c41c18",
        "storageDiagnostics": {
            "batchId": "681fe319-3006-00a8-0022-9e7cde000000"
        }
    }
}
```

CloudEvents v1.0'daki kullanılabilir alanların, türlerinin ve tanımlarının ayrıntılı bir [açıklamasına buradan ulaşabilirsiniz.](https://github.com/cloudevents/spec/blob/v1.0/spec.md#required-attributes)

CloudEvents şemasında ve Olay Izgara şemasında teslim edilen olaylar için üstbilgi değerleri `content-type`. CloudEvents şeması için bu üstbilgi `"content-type":"application/cloudevents+json; charset=utf-8"`değeri . Olay Izgara şeması için bu `"content-type":"application/json; charset=utf-8"`üstbilgi değeri .

## <a name="event-grid-for-cloudevents"></a>CloudEvents için Olay Izgara

CloudEvents şemasındaki olayların hem giriş hem de çıktısı için Olay Grid'i kullanabilirsiniz. CloudEvents'i Blob Depolama etkinlikleri, IoT Hub etkinlikleri ve özel olaylar gibi sistem etkinlikleri için kullanabilirsiniz. Ayrıca tel üzerinde ileri geri bu olayları dönüştürebilirsiniz.


| Giriş şeması       | Çıktı şeması
|--------------------|---------------------
| CloudEvents biçimi | CloudEvents biçimi
| Olay Izgara biçimi  | CloudEvents biçimi
| Olay Izgara biçimi  | Olay Izgara biçimi

Tüm olay şemaları için Olay Izgarası, bir olay ızgarası konusuna yayımlarken ve bir olay aboneliği oluştururken doğrulama gerektirir. Daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulaması'na](security-authentication.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
[CloudEvents v1.0 şemasını Event Grid ile nasıl kullanacağız.](cloudevents-schema.md)  
