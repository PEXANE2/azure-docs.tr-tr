---
title: Event Grid kaynağı olarak Azure SignalR
description: Azure Event Grid ile Azure SignalR olayları için sunulan özellikleri açıklar
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 2ac391f366c4b9a82741a1b6b3135f5d7b5fe331
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86106660"
---
# <a name="azure-event-grid-event-schema-for-signalr-service"></a>SignalR hizmeti için Azure Event Grid olay şeması

Bu makalede, SignalR hizmeti olaylarına yönelik özellikler ve şema sağlanmaktadır.Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](event-schema.md). Ayrıca, Azure SignalR 'yi bir olay kaynağı olarak kullanmak için hızlı başlayan ve öğreticilerin bir listesini sağlar.

## <a name="event-grid-event-schema"></a>Event Grid olay şeması

### <a name="available-event-types"></a>Kullanılabilir olay türleri

SignalR hizmeti aşağıdaki olay türlerini yayar:

| Olay türü | Description |
| ---------- | ----------- |
| Microsoft. SignalRService. ClientConnectionConnected | İstemci bağlantısı bağlandığında tetiklenir. |
| Microsoft. SignalRService. Clientconnectionconnected | İstemci bağlantısı kesildiğinde tetiklenir. |

### <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, istemci bağlantısı bağlantılı olayının şemasını gösterir: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionConnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

İstemci bağlantısı kesik bağlantısı olayının şeması benzerdir: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionDisconnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23",
    "errorMessage": "Internal server error."
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki en üst düzey verilere sahiptir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağının tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Türü | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Etkinliğin UTC saatine göre oluşturulduğu zaman. |
| kimlik | string | Etkinliğin benzersiz tanımlayıcısı. |
| veriler | nesne | SignalR hizmeti olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| timestamp | string | Etkinliğin UTC saatine göre oluşturulduğu zaman. |
| hubName | string | İstemci bağlantısının ait olduğu merkez. |
| ConnectionID | string | İstemci bağlantısı için benzersiz tanımlayıcı. |
| userId | string | Talep içinde tanımlanan kullanıcı tanımlayıcısı. |
| Hata | string | Bağlantının bağlantısının kesilmesine neden olan hata. |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık | Açıklama |
|---------|---------|
| [Event Grid kullanarak Azure SignalR hizmeti olaylarına tepki verme](../azure-signalr/signalr-concept-event-grid-integration.md) | Event Grid ile Azure SignalR hizmeti tümleştirilmesine genel bakış. |
| [Event Grid için Azure SignalR hizmeti olaylarını gönderme](../azure-signalr/signalr-howto-event-grid-integration.md) | Event Grid aracılığıyla Azure SignalR hizmeti olaylarının bir uygulamaya nasıl gönderileceğini gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
