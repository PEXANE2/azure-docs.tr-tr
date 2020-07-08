---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 2604a1608f21d7239db755027e15b8198fb3f9f2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81791653"
---
### <a name="functions-2x-and-higher"></a>İşlevler 2.x ve üzeri

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|maxBatchSize|10|Alma döngüsü başına alınan en yüksek olay sayısı.|
|prefetchCount|300|Temel tarafından kullanılan varsayılan önceden getirme sayısı `EventProcessorHost` .|
|Batchcheckpointflik|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay toplu işlemlerinin sayısı.|

> [!NOTE]
> Azure Işlevleri 2. x ve ötesi 'daki host.jsbaşvurusu için bkz. [Azure işlevleri için başvuruhost.js](../articles/azure-functions/functions-host-json.md).

### <a name="functions-1x"></a>İşlevler 1.x

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxBatchSize|64|Alma döngüsü başına alınan en yüksek olay sayısı.|
|prefetchCount|yok|Temel tarafından kullanılacak varsayılan ön getirme `EventProcessorHost` .| 
|Batchcheckpointflik|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay toplu işlemlerinin sayısı.| 

> [!NOTE]
> Azure Işlevleri 1. x ' teki host.jsbaşvurusu için bkz. [Azure işlevleri için başvuruhost.js, 1. x](../articles/azure-functions/functions-host-json-v1.md).

