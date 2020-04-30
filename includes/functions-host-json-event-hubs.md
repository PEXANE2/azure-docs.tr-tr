---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 2604a1608f21d7239db755027e15b8198fb3f9f2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81791653"
---
### <a name="functions-2x-and-higher"></a>İşlevler 2. x ve üzeri

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
|prefetchCount|300|Temel `EventProcessorHost`tarafından kullanılan varsayılan önceden getirme sayısı.|
|Batchcheckpointflik|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay toplu işlemlerinin sayısı.|

> [!NOTE]
> Azure Işlevleri 2. x ve ötesinde Host. JSON başvurusu için bkz. [Azure işlevleri için Host. JSON başvurusu](../articles/azure-functions/functions-host-json.md).

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
|prefetchCount|yok|Temel `EventProcessorHost`tarafından kullanılacak varsayılan ön getirme.| 
|Batchcheckpointflik|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay toplu işlemlerinin sayısı.| 

> [!NOTE]
> Azure Işlevleri 1. x içindeki Host. JSON başvurusu için bkz. [Azure işlevleri için Host. JSON başvurusu 1. x](../articles/azure-functions/functions-host-json-v1.md).

