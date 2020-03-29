---
title: Azure Olay Izgarası Azure SignalR olay şeması
description: Azure Event Grid ile Azure SinyalR etkinlikleri için sağlanan özellikleri açıklar
services: event-grid
author: chenyl
ms.service: event-grid
ms.topic: reference
ms.date: 06/11/2019
ms.author: chenyl
ms.openlocfilehash: 3b072ff2b680ad6d144c7441190ab2df9870f5d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67789078"
---
# <a name="azure-event-grid-event-schema-for-signalr-service"></a>SignalR Hizmeti için Azure Olay Izgara etkinliği şeması

Bu makalede, SignalR Service olayları için özellikleri ve şema sağlar.Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.


## <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

SignalR Hizmeti aşağıdaki olay türlerini yayır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.SignalRService.ClientConnectionConnected | İstemci bağlantısı bağlandığında yükseltildi. |
| Microsoft.SignalRService.ClientConnectionBağlantısı kesildi | İstemci bağlantısı kesildiğinde yükseltildi. |

## <a name="example-event"></a>Örnek olay

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

## <a name="event-properties"></a>Olay özellikleri

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

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
