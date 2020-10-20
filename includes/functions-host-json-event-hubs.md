---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 8f3a58d3a7470867ab23249bbd645289e010ad89
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223169"
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
|prefetchCount|300|Temel tarafından kullanılan varsayılan önceden getirme sayısı `EventProcessorHost` . İzin verilen en düşük değer 10 ' dur.|
|Batchcheckpointflik|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay toplu işlemlerinin sayısı.|

> [!NOTE]
> Azure Işlevleri 2. x ve ötesi 'daki host.jsbaşvurusu için bkz. [ Azure işlevleri için başvuruhost.js](../articles/azure-functions/functions-host-json.md).

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
> Azure Işlevleri 1. x ' teki host.jsbaşvurusu için bkz. [ Azure işlevleri için başvuruhost.js, 1. x](../articles/azure-functions/functions-host-json-v1.md).
