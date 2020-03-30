---
title: Azure Olay Izgara olay şeması
description: Tüm olaylar için mevcut olan özellikleri ve şemayı açıklar.Olaylar, gerekli beş dize özelliği ve gerekli bir veri nesnesi kümesinden oluşur.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 01/21/2020
ms.author: babanisa
ms.openlocfilehash: 35cea2e6df311d2f4071686c21c8e4c36477abc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244842"
---
# <a name="azure-event-grid-event-schema"></a>Azure Olay Izgara olay şeması

Bu makalede, tüm olaylar için mevcut özellikleri ve şema açıklanır.Olaylar, gerekli beş dize özelliği ve gerekli bir veri nesnesi kümesinden oluşur. Özellikler, herhangi bir yayımcıdaki tüm olaylar için ortaktür. Veri nesnesi, her yayımcıya özgü özelliklere sahiptir. Sistem konuları için bu özellikler Azure Depolama veya Azure Etkinlik Hub'ları gibi kaynak sağlayıcısına özgüdir.

Olay kaynakları, birden çok olay nesnesi olan bir dizideki olayları Azure Olay Izgarası'na gönderir. Olayları bir olay ızgarası konusuna gönderirken, dizinin toplam boyutu 1 MB'a kadar olabilir. Dizideki her olay 64 KB (Genel Kullanılabilirlik) veya 1 MB (önizleme) ile sınırlıdır. Bir olay veya dizi boyut sınırlamından büyükse, **413 Payload Too Large**yanıtını alırsınız.

> [!NOTE]
> 64 KB'ye kadar boyut landırma olayı Genel Kullanılabilirlik (GA) Hizmet Düzeyi Sözleşmesi (SLA) kapsamındadır. 1 MB'a kadar boyutlandırma etkinliği desteği şu anda önizlemededir. 64 KB üzerindeki olaylar 64-KB artışlarla ücretlendirilir. 

Olay Grid, olayları tek bir olay olan bir dizideki abonelere gönderir. Bu davranış gelecekte değişebilir.

Olay Izgara etkinliği için JSON şemasını ve her Azure yayımcısının veri yükünü [Event Schema deposunda](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/eventgrid/data-plane)bulabilirsiniz.

## <a name="event-schema"></a>Olay şeması

Aşağıdaki örnek, tüm olay yayımcılar tarafından kullanılan özellikleri gösterir:

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

Örneğin, bir Azure Blob depolama olayı için yayınlanan şema:

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```

## <a name="event-properties"></a>Olay özellikleri

Tüm olaylar aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Gerekli | Açıklama |
| -------- | ---- | -------- | ----------- |
| konu başlığı | string | Hayır, ancak dahil edilirse, Olay Izgara konusu Azure Kaynak Yöneticisi Kimliği ile tam olarak eşleşmelidir. Dahil edilmezse, Olay Izgarası etkinliğe damgalanır. | Olay kaynağına tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |
| Konu | string | Evet | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Evet | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Evet | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Evet | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Hayır | Kaynak sağlayıcısına özgü olay verileri. |
| dataVersion | string | Hayır, ama boş bir değerle damgalanır. | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Gerekli değil, ancak dahil edilirse, Olay Izgara Şeması `metadataVersion` tam `1`olarak eşleşmelidir (şu anda, yalnızca). Dahil edilmezse, Olay Izgarası etkinliğe damgalanır. | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesindeki özellikler hakkında bilgi edinmek için olay kaynağına bakın:

* [Azure abonelikleri (yönetim işlemleri)](event-schema-subscriptions.md)
* [Konteyner Kayıt Defteri](event-schema-container-registry.md)
* [Blob depolama](event-schema-blob-storage.md)
* [Etkinlik Hub'ları](event-schema-event-hubs.md)
* [IoT Hub](event-schema-iot-hub.md)
* [Media Services](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json)
* [Kaynak grupları (yönetim işlemleri)](event-schema-resource-groups.md)
* [Service Bus](event-schema-service-bus.md)
* [Azure SignalR](event-schema-azure-signalr.md)
* [Azure Makine Öğrenimi](event-schema-machine-learning.md)

Özel konular için olay yayımcısı veri nesnesini belirler. Üst düzey veriler, standart kaynak tanımlı olaylarla aynı alanlara sahip olmalıdır.

Etkinlikleri özel konulara yayınlarken, etkinliklerinizin etkinlikleriiçin, abonelerin etkinlikle ilgilenip ilgilenmediklerini bilmelerini kolaylaştıran konular oluşturun. Aboneler olayları filtrelemek ve yönlendirmek için konuyu kullanır. Abonelerin bu yolun bölümlerine göre filtre uygulayabilmesi için, olayın gerçekleştiği yer için yol sağlamayı düşünün. Yol, abonelerin olayları dar veya genel olarak filtrelemesini sağlar. Örneğin, konu gibi `/A/B/C` üç segmentli bir yol sağlarsanız, aboneler `/A` geniş bir olay kümesi elde etmek için ilk kesime göre filtre uygulayabilir. Bu aboneler gibi `/A/B/C` konular `/A/D/E`ile olaylar olsun ya da . Diğer aboneler daha `/A/B` dar bir olay kümesi almak için filtre uygulayabilir.

Bazen deneğinin ne olduğu hakkında daha fazla detaya ihtiyacı vardır. Örneğin, **Depolama Hesapları** yayımcısı, `/blobServices/default/containers/<container-name>/blobs/<file>` bir dosya kapsayıcıya eklendiğinde konuyu sağlar. Bir abone, depolama hesabındaki diğer kapsayıcılar için tüm olayları almak için yolu `/blobServices/default/containers/testcontainer` filtreleyebilir. Bir abone, yalnızca metin dosyalarıyla çalışmak `.txt` için sonek tarafından filtreleyebilir veya yönlendirebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
