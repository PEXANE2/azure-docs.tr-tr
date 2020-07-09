---
title: Azure Event Grid olaylar için olay işleyicisi olarak Azure işlevi
description: Azure işlevlerini Event Grid olayları için olay işleyicileri olarak nasıl kullanabileceğinizi açıklar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 8e48949bb5fecdf370fdf23146209ad757ffa062
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105770"
---
# <a name="azure-function-as-an-event-handler-for-event-grid-events"></a>Event Grid olaylar için olay işleyicisi olarak Azure işlevi

Olay işleyicisi, olayın gönderildiği yerdir. İşleyici, olayı işlemek için bir eylem gerçekleştirir. Birkaç Azure hizmeti, olayları işleyecek şekilde otomatik olarak yapılandırılır ve **Azure işlevleri** bunlardan biridir. 

Event Grid olaylara yanıt vermek için sunucusuz bir mimaride **Azure işlevlerini** kullanın. İşleyici olarak bir Azure işlevi kullanırken, genel HTTP tetikleyicisi yerine Event Grid tetikleyicisini kullanın. Event Grid Event Grid Tetikleyicileri otomatik olarak doğrular. Genel HTTP Tetikleyicileri ile [doğrulama yanıtını](webhook-event-delivery.md) kendiniz uygulamanız gerekir.

Daha fazla bilgi için bkz. işlevlerde Event Grid tetikleyicisini kullanmaya genel bakış için [Azure işlevleri Event Grid tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) .

## <a name="tutorials"></a>Öğreticiler

|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: işlev ile olayları Işleme](custom-event-to-function.md) | İşlemek için bir işleve özel bir olay gönderir. |
| [Öğretici: Event Grid kullanarak karşıya yüklenen görüntüleri yeniden boyutlandırmayı otomatikleştirme](resize-images-on-storage-blob-upload-event.md) | Kullanıcılar, görüntüleri Web uygulamasına depolama hesabına yükler. Bir Depolama Blobu oluşturulduğunda, Event Grid işlev uygulamasına bir olay göndererek karşıya yüklenen görüntüyü yeniden boyutlandırır. |
| [Öğretici: veri ambarına büyük veri akışı](event-grid-event-hubs-integration.md) | Event Hubs bir yakalama dosyası oluşturduğunda, Event Grid bir işlev uygulamasına bir olay gönderir. Uygulama yakalama dosyasını alır ve verileri bir veri ambarına geçirir. |
| [Öğretici: Azure Event Grid tümleştirme örneklerine Azure Service Bus](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid, Service Bus konu başlığından bir işlev uygulamasına ve Logic App 'e ileti gönderir. |

## <a name="rest-example-for-put"></a>REST örneği (PUT için)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "AzureFunction",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Web/sites/<FUNCTION APP NAME>/functions/<FUNCTION NAME>",
                "maxEventsPerBatch": 1,
                "preferredBatchSizeInKilobytes": 64
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen olay işleyicilerinin bir listesi için bkz. [olay işleyicileri](event-handlers.md) makalesi. 
