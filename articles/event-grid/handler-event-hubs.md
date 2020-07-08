---
title: Azure Event Grid olaylar için olay işleyicisi olarak olay hub 'ı
description: Olay Hub 'larını Azure Event Grid olayları için olay işleyicileri olarak nasıl kullanabileceğinizi açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: f46b17fdffc870e6afc5f3b0711169db8270a540
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83800433"
---
# <a name="event-hub-as-an-event-handler-for-azure-event-grid-events"></a>Azure Event Grid olaylar için olay işleyicisi olarak olay hub 'ı
Olay işleyicisi, olayın gönderildiği yerdir. İşleyici, olayı işlemek için bir eylem gerçekleştirir. Çeşitli Azure Hizmetleri, olayları işleyecek şekilde otomatik olarak yapılandırılır ve **azure Event Hubs** bunlardan biridir. 

Çözümünüz olayları işleyebileceğinden daha hızlı Event Grid olaylar aldığında **Event Hubs** kullanın. Olaylar bir olay hub 'ında olduktan sonra, uygulamanız olay hub 'ından olayları kendi zamanlamaya göre işleyebilir. Gelen olayları işlemek için olay işlemenizi ölçeklendirebilirsiniz.

## <a name="tutorials"></a>Öğreticiler
Aşağıdaki örneklere bakın: 

|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Azure CLı ile özel olayları Azure Event Hubs yönlendirme](custom-event-to-eventhub.md) | Bir uygulama tarafından işlenmek üzere bir olay hub 'ına özel bir olay gönderir. |
| [Kaynak Yöneticisi şablonu: bir Event Grid özel konu oluşturma ve Olay Hub 'ına olay gönderme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Özel konu için abonelik oluşturan Kaynak Yöneticisi şablonu. Olayları bir Azure Event Hubs gönderir. |

## <a name="message-properties"></a>İleti özellikleri
Event Grid olaylar için olay işleyicisi olarak bir **Olay Hub** 'ı kullanıyorsanız, aşağıdaki ileti üst bilgilerini ayarlayın: 

| Özellik adı | Açıklama |
| ------------- | ----------- | 
| AEG-abonelik-adı | Olay aboneliğinin adı. |
| AEG-Delivery-Count | <p>Olay için yapılan deneme sayısı.</p> <p>Örnek: "1"</p> |
| AEG-Event-Type | <p>Etkinliğin türü.</p><p> Örnek: "Microsoft. Storage. blobCreated"</p> | 
| AEG-Metadata-Version | <p>Etkinliğin meta veri sürümü.</p> <p>Örnek: "1".</p><p> **Event Grid olay şeması**için, bu özellik meta veri sürümünü ve **bulut olay şeması**için, **Belirtim sürümünü**temsil eder. </p>|
| AEG-veri sürümü | <p>Etkinliğin veri sürümü.</p><p>Örnek: "1".</p><p>**Event Grid olay şeması**için, bu özellik veri sürümünü ve **bulut olay şeması**için, uygulanmaz.</p> |
| AEG-çıkış-olay kimliği | Event Grid olayının KIMLIĞI. |

## <a name="rest-examples-for-put"></a>REST örnekleri (PUT için)


### <a name="event-hub"></a>Olay hub'ı

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "EventHub",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUBS NAMESPACE NAME>/eventhubs/<EVENT HUB NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

### <a name="event-hub---delivery-with-managed-identity"></a>Olay Hub 'ı-yönetilen kimlikle teslim

```json
{
    "properties": {
        "deliveryWithResourceIdentity": 
        {
            "identity": 
            {
                "type": "SystemAssigned"
            },
            "destination": 
            {
                "endpointType": "EventHub",
                "properties": 
                {
                    "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUBS NAMESPACE NAME>/eventhubs/<EVENT HUB NAME>"
                }
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen olay işleyicilerinin bir listesi için bkz. [olay işleyicileri](event-handlers.md) makalesi. 
