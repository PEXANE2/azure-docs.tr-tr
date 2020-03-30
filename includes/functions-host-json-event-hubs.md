---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: b5d8f67a70961aab21312b6f241081dcb33f66fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188165"
---
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
|maxBatchSize|64|Alma döngüsü başına alınan maksimum olay sayısı.|
|prefetchCount|yok|Altta yatan EventProcessorHost tarafından kullanılacak varsayılan PrefetchCount.| 
|topluCheckpointFrequency|1|EventHub imleç denetim noktası oluşturmadan önce işlenmesi gereken olay toplu işlerinin sayısı.| 
