---
title: Azure SignalR Hizmeti etkinliklerine tepki verme
description: Azure SinyalR Hizmeti etkinliklerine abone olmak için Azure Olay Ağıt'ı kullanın. Diğer downstream hizmetleri bu olaylar tarafından tetiklenebilir.
services: azure-signalr,event-grid
author: chenyl
ms.author: chenyl
ms.reviewer: zhshang
ms.date: 11/13/2019
ms.topic: conceptual
ms.service: signalr
ms.openlocfilehash: a8e25907b40b910f2b91884d355b6ac85eeaa250
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74158200"
---
# <a name="reacting-to-azure-signalr-service-events"></a>Azure SignalR Hizmeti olaylarına yanıt verme

Azure SignalR Service olayları, uygulamaların modern sunucusuz mimariler kullanarak bağlı veya bağlantısı kesilen istemci bağlantılarına tepki göstermesine olanak tanır. Bunu karmaşık kod veya pahalı ve verimsiz yoklama hizmetlerine gerek kalmadan yapar.  Bunun yerine, etkinlikler [Azure İşlevleri](https://azure.microsoft.com/services/functions/), [Azure Mantık Uygulamaları](https://azure.microsoft.com/services/logic-apps/)ve hatta kendi özel http dinleyiciniz gibi abonelere Azure Olay [Izgarası](https://azure.microsoft.com/services/event-grid/) üzerinden itilir ve yalnızca kullandığınız kadar ını ödler.

Azure SignalR Hizmeti etkinlikleri, zengin yeniden deneme ilkeleri ve ölü harf teslimatı yoluyla uygulamalarınız için güvenilir teslimat hizmetleri sağlayan Event Grid hizmetine güvenilir bir şekilde gönderilir. Daha fazla bilgi için [Olay Izgara iletisi teslimi ve yeniden deneyin.](https://docs.microsoft.com/azure/event-grid/delivery-and-retry)

![Olay Izgara Modeli](https://docs.microsoft.com/azure/event-grid/media/overview/functional-model.png)

## <a name="serverless-state"></a>Sunucusuz durum
Azure SignalR Service olayları yalnızca istemci bağlantıları sunucusuz durumdaolduğunda etkindir. Genel olarak konuşursak, istemci hub sunucusuna yönlendirmezse, sunucusuz duruma geçer. Klasik mod yalnızca istemci bağlantılarının bağlandığı hub'ın hub sunucusu olmadığında çalışır. Ancak, sunucusuz modu bazı sorunları önlemek için önerilir. Hizmet modu hakkında daha fazla bilgi edinmek [için Hizmet Modu'nu nasıl seçeceğinizi](https://github.com/Azure/azure-signalr/blob/dev/docs/faq.md#what-is-the-meaning-of-service-mode-defaultserverlessclassic-how-can-i-choose)öğrenin.

## <a name="available-azure-signalr-service-events"></a>Kullanılabilir Azure SignalR Hizmeti etkinlikleri
Olay [ızgarası,](../event-grid/concepts.md#event-subscriptions) olay iletilerini abonelere yönlendirmek için olay aboneliklerini kullanır. Azure SignalR Service etkinlik abonelikleri iki tür olayı destekler:  

|Olay Adı|Açıklama|
|----------|-----------|
|`Microsoft.SignalRService.ClientConnectionConnected`|İstemci bağlantısı bağlandığında yükseltilir.|
|`Microsoft.SignalRService.ClientConnectionDisconnected`|İstemci bağlantısı kesildiğinde yükseltilir.|

## <a name="event-schema"></a>Olay şeması
Azure SignalR Hizmeti etkinlikleri, verilerinizdeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. "Microsoft.SignalRService" ile başlayan eventType özelliğiyle bir Azure SignalR Hizmeti olayını tanımlayabilirsiniz. Olay Izgara olay özelliklerinin kullanımı hakkında ek bilgiler [Olay Grid olay şemasında](../event-grid/event-schema.md)belgelenmiştir.  

İstemci bağlantısına bağlı bir olaya örnek aşağıda verilmiştir:
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

Daha fazla bilgi için [SignalR Service etkinlik şemasına](../event-grid/event-schema-azure-signalr.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Olay Ağı'sı hakkında daha fazla bilgi edinin ve Azure SignalR Hizmeti etkinliklerini deneyin:

> [!div class="nextstepaction"]
> [Try a sample Event Grid integration with Azure SignalR Service](./signalr-howto-event-grid-integration.md)
> [Olay Izgarahakkında](../event-grid/overview.md) Azure SignalR Hizmeti ile örnek bir Olay Ağı tümleştirmesi deneyin
