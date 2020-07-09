---
title: Azure Event Grid olayları için bir olay işleyicisi olarak karma bağlantı
description: Azure Event Grid olayları için Azure Relay karma bağlantıları olay işleyicileri olarak nasıl kullanabileceğinizi açıklar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 38233a2e103600f07837ce9a1ad8d63fe7e4fb99
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105725"
---
# <a name="relay-hybrid-connection-as-an-event-handler-for-azure-event-grid-events"></a>Azure Event Grid olayları için bir olay işleyicisi olarak karma bağlantı
Olay işleyicisi, olayın gönderildiği yerdir. İşleyici, olayı işlemek için başka bir eylem gerçekleştirir. Çeşitli Azure Hizmetleri, olayları işleyecek şekilde otomatik olarak yapılandırılır ve bunlardan biridir **Azure Relay** . 

Kurumsal ağ içinde olan uygulamalara olay göndermek için Azure **geçişi karma bağlantılar** kullanın ve genel olarak erişilebilen bir uç nokta yoktur.

## <a name="tutorials"></a>Öğreticiler
Olay işleyicisi olarak Azure Relay karma bağlantı kullanmanın bir örneği için aşağıdaki öğreticiye bakın. 

|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: olayları karma bağlantıya gönder](custom-event-to-hybrid-connection.md) | Bir dinleyici uygulaması tarafından işlenmek üzere var olan karma bağlantıya özel bir olay gönderir. |

## <a name="rest-example-for-put"></a>REST örneği (PUT için)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "HybridConnection",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Relay/namespaces/<RELAY NAMESPACE NAME>/hybridconnections/<HYBRID CONNECTION NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen olay işleyicilerinin bir listesi için bkz. [olay işleyicileri](event-handlers.md) makalesi. 