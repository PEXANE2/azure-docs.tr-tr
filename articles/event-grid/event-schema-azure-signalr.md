---
title: Olay Izgara kaynağı olarak Azure SingnalR
description: Azure Event Grid ile Azure SinyalR etkinlikleri için sağlanan özellikleri açıklar
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: babanisa
ms.openlocfilehash: 730d1a7a053ab636c45313dd0c35a537434eb782
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393386"
---
# <a name="azure-event-grid-event-schema-for-signalr-service"></a>SignalR Hizmeti için Azure Olay Izgara etkinliği şeması

Bu makalede, SignalR Service olayları için özellikleri ve şema sağlar.Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın. Ayrıca, Azure SignalR'ı etkinlik kaynağı olarak kullanmanız için hızlı başlangıçların ve öğreticilerin bir listesini de verir.

## <a name="event-grid-event-schema"></a>Olay Izgara olay şeması

### <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

SignalR Hizmeti aşağıdaki olay türlerini yayır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.SignalRService.ClientConnectionConnected | İstemci bağlantısı bağlandığında yükseltildi. |
| Microsoft.SignalRService.ClientConnectionBağlantısı kesildi | İstemci bağlantısı kesildiğinde yükseltildi. |

### <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, istemci bağlantısına bağlı bir olayın şemasını gösterir: 

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

İstemci bağlantısı bağlantısı kesilen bir olayın şeması benzer: 

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

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | SignalR Service olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| timestamp | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| hubName | string | İstemci bağlantısının ait olduğu hub. |
| Connectionıd | string | İstemci bağlantısı için benzersiz tanımlayıcı. |
| userId | string | Talepte tanımlanan kullanıcı tanımlayıcısı. |
| errorMessage | string | Bağlantının kesilmesine neden olan hata. |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık | Açıklama |
|---------|---------|
| [Olay Kılavuz'u kullanarak Azure SignalR Hizmeti etkinliklerine tepki verme](../azure-signalr/signalr-concept-event-grid-integration.md) | Azure SignalR Hizmetini Olay Ağıt'ı ile tümleştirmeye genel bakış. |
| [Azure SignalR Hizmeti etkinlikleri Etkinlik Ağılasına nasıl gönderilir?](../azure-signalr/signalr-howto-event-grid-integration.md) | Azure SignalR Hizmeti etkinliklerinin Bir uygulamaya Nasıl Gönderilebildiğini Event Grid aracılığıyla gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
