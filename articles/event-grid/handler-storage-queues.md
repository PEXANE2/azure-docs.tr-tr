---
title: Azure Event Grid olaylar için olay işleyicisi olarak depolama kuyruğu
description: Azure depolama kuyruklarını Azure Event Grid olayları için olay işleyicileri olarak nasıl kullanabileceğinizi açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: f62f2b5bc01518af29bd1deb17a38e9fe105a4ed
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83800566"
---
# <a name="storage-queue-as-an-event-handler-for-azure-event-grid-events"></a>Azure Event Grid olaylar için olay işleyicisi olarak depolama kuyruğu
Olay işleyicisi, olayın gönderildiği yerdir. İşleyici, olayı işlemek için başka bir eylem gerçekleştirir. Birkaç Azure hizmeti, olayları işleyecek şekilde otomatik olarak yapılandırılır ve **Azure kuyruk depolaması** bunlardan biridir. 

Çekilmesi gereken olayları almak için **kuyruk depolama alanını** kullanın. Yanıt vermeyi çok uzun süren uzun süre çalışan bir işleminiz varsa kuyruk depolama alanını kullanabilirsiniz. Uygulama kuyruk depolamaya olay göndererek olayları kendi zamanlamaya göre çekebilir ve işleyebilir.

## <a name="tutorials"></a>Öğreticiler
Kuyruk depolama alanını olay işleyicisi olarak kullanmanın bir örneği için aşağıdaki öğreticiye bakın. 

|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Azure CLı ve Event Grid ile Azure kuyruk depolama 'ya özel olayları yönlendirme](custom-event-to-queue-storage.md) | Bir kuyruk depolamasına özel olayların nasıl gönderileceğini açıklar. |

## <a name="rest-examples-for-put"></a>REST örnekleri (PUT için)

### <a name="storage-queue-as-the-event-handler"></a>Olay işleyicisi olarak depolama kuyruğu

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "StorageQueue",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>",
                "queueName": "<QUEUE NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

### <a name="storage-queue-as-the-event-handler---delivery-with-managed-identity"></a>Olay işleyicisi olarak depolama kuyruğu-yönetilen kimlikle teslim

```json
{
    "properties": 
    {
        "deliveryWithResourceIdentity": 
        {
            "identity": 
            {
                "type": "SystemAssigned"
            },
            "destination": 
            {
                "endpointType": "StorageQueue",
                "properties": 
                {
                    "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>",
                    "queueName": "<QUEUE NAME>"
                }
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

### <a name="storage-queue-as-a-deadletter-destination"></a>Depolama kuyruğu, sahipsiz bir hedef olarak

```json
{
    "name": "",
    "properties": 
    {
        "destination": 
        {
            "endpointType": "StorageQueue",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<DESTINATION STORAGE>",
                "queueName": "queue1"
            }
        },
        "eventDeliverySchema": "EventGridSchema",
        "deadLetterDestination": 
        {
            "endpointType": "StorageBlob",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<DEADLETTER STORAGE>",
                "blobContainerName": "test"
            }
        }
    }
}
```

### <a name="storage-queue-as-a-deadletter-destination---managed-identity"></a>Hedef tarafından yönetilen bir kimlik olarak depolama kuyruğu

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "StorageQueue",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<DESTINATION STORAGE>",
                "queueName": "queue1"
            }
        },
        "eventDeliverySchema": "EventGridSchema",
        "deadLetterWithResourceIdentity": 
        {
            "identity": 
            {
                "type": "SystemAssigned"
            },
            "deadLetterDestination": 
            {
                "endpointType": "StorageBlob",
                "properties": 
                {
                    "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<DEADLETTER STORAGE>",
                    "blobContainerName": "test"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen olay işleyicilerinin bir listesi için bkz. [olay işleyicileri](event-handlers.md) makalesi. 
