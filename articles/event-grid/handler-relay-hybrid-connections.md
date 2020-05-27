---
title: Azure Event Grid olayları için bir olay işleyicisi olarak karma bağlantı
description: Azure Event Grid olayları için Azure Relay karma bağlantıları olay işleyicileri olarak nasıl kullanabileceğinizi açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: 04fbc7b739fa8ea7b08a3341c2f78244c445e721
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83800877"
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