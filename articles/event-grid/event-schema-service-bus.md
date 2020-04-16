---
title: Olay Izgara kaynağı olarak Azure Hizmet Veri Tos
description: Azure Olay Ağıtı ile Hizmet Veri Veri Mes'leri etkinlikleri için sağlanan özellikleri açıklar
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: babanisa
ms.openlocfilehash: 141a0e96071014dc3705d30f72b1a9257737298a
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393234"
---
# <a name="azure-service-bus-as-an-event-grid-source"></a>Olay Izgara kaynağı olarak Azure Hizmet Veri Tos

Bu makalede, Servis Veri Servisi etkinlikleri için özellikler ve şema sağlar.Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

## <a name="event-grid-event-schema"></a>Olay Izgara olay şeması

### <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Servis Veri Servisi aşağıdaki olay türlerini yayır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners | Bir Kuyruk veya Abonelik'te etkin iletiler olduğunda ve alıcı lar dinlemediğinde yükseltilir. |
| Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener | Ölü Harf Kuyruğu'nda etkin iletiler olduğunda ve etkin dinleyici olmadığında yükseltilir. |

### <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, dinleyici olayı olmayan etkin iletilerin şemasını gösterir:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Ölü harf sırası olayının şeması benzer:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Blob depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Namespacename | string | Hizmet Veri Servisi ad alanı kaynağı var. |
| Requesturi | string | Uri belirli sıraya veya abonelik olay yayan. |
| Entitytype | string | Hizmet Veri Mesuliyeti kuruluş türü yayan olaylar (sıra veya abonelik). |
| queueName | string | Bir kuyruğa abone olan etkin iletilerin yer alan sırası. Konular / abonelikler kullanıyorsanız null değeri. |
| topicName | string | Etkin iletilerin yer alan Servis Veri Servisi aboneliğinin ait olduğu konu. Sıra kullanıyorsanız null değeri. |
| subscriptionName | string | Etkin iletilerle Hizmet Veri Servisi aboneliği. Sıra kullanıyorsanız null değeri. |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: Azure Hizmet Veri Servisi'ni Azure Olay Izgara tümleştirme örneklerine](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Olay Grid, Service Bus konusundan işlev uygulamasına ve mantık uygulamasına ileti gönderir. |
| [Azure Servis Veri Servisi'ndan Olay Ağı tümleştirmesine](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Hizmet Veri Veri Tos'u Olay Izgarasıyla tümleştirmeye genel bakış. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
* Hizmet Veri Yolunda Azure Olay Izgarasını kullanma hakkında ayrıntılı bilgi [için, Olay Ağı tümleştirmesine genel bakış](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md)alameti olan Servis Veri Yolunda'na bakın.
* [Fonksiyonlar veya Logic Apps ile Servis Veri Servisi etkinlikleri almayı](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json)deneyin.
