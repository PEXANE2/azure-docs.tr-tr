---
title: CloudEvents şemasında olaylarla Azure Event Grid kullanma
description: Azure Event Grid olaylar için CloudEvents şemasının nasıl kullanılacağını açıklar. Hizmet, bulut olaylarının JSON uygulamasındaki olayları destekler.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: babanisa
ms.openlocfilehash: 127095bef2c67a93097bf90bea54ca1b44b16c58
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81394390"
---
# <a name="cloudevents-v10-schema-with-event-grid"></a>Event Grid ile CloudEvents v 1.0 şeması

Azure Event Grid, [varsayılan olay şemasına](event-schema.md)ek olarak, [cloudevents v 1.0](https://github.com/cloudevents/spec/blob/v1.0/json-format.md) ve [http protokol bağlamasının](https://github.com/cloudevents/spec/blob/v1.0/http-protocol-binding.md)JSON uygulamasındaki olayları yerel olarak destekler. [Cloudevents](https://cloudevents.io/) , olay verilerini tanımlamaya yönelik [açık bir belirtimdir](https://github.com/cloudevents/spec/blob/v1.0/spec.md) .

CloudEvents, yayımlama için ortak bir olay şeması sağlayarak ve bulut tabanlı olayları tüketerek birlikte çalışabilirliği basitleştirir. Bu şema, tek düzen araçları, & işleme olaylarını yönlendirme için standart yolları ve dış olay şemasının serisini kaldırma evrensel yollarını sağlar. Ortak bir şema ile, iş platformları arasında daha kolay bir şekilde tümleşebilir.

CloudEvents, [bulut Yerel Bilgi Işlem altyapısı](https://www.cncf.io/)aracılığıyla Microsoft gibi birkaç [ortak](https://github.com/cloudevents/spec/blob/master/community/contributors.md)şekilde oluşturulmuştur. Şu anda sürüm 1,0 olarak sunulmaktadır.

Bu makalede, Event Grid ile CloudEvents şeması açıklanmaktadır.

## <a name="sample-event-using-cloudevents-schema"></a>CloudEvents şemasını kullanan örnek olay

CloudEvents biçiminde Azure Blob depolama olayına bir örnek aşağıda verilmiştir:

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

Kullanılabilir alanlar, bunların türleri ve CloudEvents v 1.0 içindeki tanımlara ilişkin ayrıntılı bir açıklama [burada bulunabilir](https://github.com/cloudevents/spec/blob/v1.0/spec.md#required-attributes).

CloudEvents şemasında teslim edilen olaylar ve Event Grid şeması için üst bilgi değerleri, hariç aynıdır `content-type`. CloudEvents şeması için bu üst bilgi değeri olur `"content-type":"application/cloudevents+json; charset=utf-8"`. Event Grid şeması için bu üst bilgi değeri olur `"content-type":"application/json; charset=utf-8"`.

## <a name="event-grid-for-cloudevents"></a>CloudEvents için Event Grid

CloudEvents şemasında olay girişi ve çıktısı için Event Grid kullanabilirsiniz. BLOB depolama olayları ve IoT Hub olayları ve özel olaylar gibi sistem olayları için CloudEvents kullanabilirsiniz. Ayrıca, bu olayları tel ve ileri doğru şekilde dönüştürebilir.


| Giriş şeması       | Çıktı şeması
|--------------------|---------------------
| CloudEvents biçimi | CloudEvents biçimi
| Event Grid biçimi  | CloudEvents biçimi
| Event Grid biçimi  | Event Grid biçimi

Tüm olay şemaları için Event Grid, bir olay kılavuzu konusuna yayımlarken ve olay aboneliği oluştururken doğrulama gerektirir. Daha fazla bilgi için bkz. [güvenlik ve kimlik doğrulaması Event Grid](security-authentication.md).

## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Event Grid Ile CloudEvents v 1.0 şeması kullanma](cloudevents-schema.md).  
