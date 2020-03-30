---
title: Azure Olay Izgara olay hub'ları olay şeması
description: Azure Olay Ağıtı ile olay hub'ları etkinlikleri için sağlanan özellikleri açıklar
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: reference
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 9c0113687d27bf43375f298057129a5594ec0a06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60561837"
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Etkinlik hub'ları için Azure Olay Izgara olay şeması

Bu makalede, olay hub'ları olaylar için özellikleri ve şema sağlar.Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

Örnek komut dosyaları ve öğreticilerin listesi için [Olay Hub'ları etkinlik kaynağına](event-sources.md#event-hubs)bakın.

### <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Bir yakalama dosyası oluşturulduğunda Olay Hub'ları **Microsoft.EventHub.CaptureFileCreated** olay türünü yayar.

## <a name="example-event"></a>Örnek olay

Bu örnek olay, yakalama özelliği bir dosyayı depoladığında ortaya çıkan bir olay hub'larının şemasını gösterir: 

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        },
        "dataVersion": "",
        "metadataVersion": "1"
    }
]
```

## <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Olay merkezi olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| fileUrl | string | Yakalama dosyasına giden yol. |
| Filetype | string | Yakalama dosyasının dosya türü. |
| Partitionıd | string | Kırık kimlik. |
| sizeBytes | integer | Dosya boyutu. |
| olaySay | integer | Dosyadaki olayların sayısı. |
| ilkSequenceNumber | integer | Kuyruktaki en küçük sıra numarası. |
| lastSequenceNumber | integer | Kuyruktaki son sıra numarası. |
| ilkEnqueueTime | string | Kuyruktan ilk kez. |
| lastEnqueueTime | string | Kuyruktan son kez. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
* Olay hub'ları olaylarını işleme hakkında bilgi için [bkz.](event-grid-event-hubs-integration.md)