---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: f5101233f7995fb58fc530e613ba3235a55c783c
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97628769"
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
            },
            "initialOffsetOptions": {
                "type": "fromStart",
                "enqueuedTime": ""
            }
        }
    }
}  
```

|Özellik  |Varsayılan | Description |
|---------|---------|---------|
|Batchcheckpointflik|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay toplu işlemlerinin sayısı.|
|eventProcessorOptions/maxBatchSize|10|Alma döngüsü başına alınan en yüksek olay sayısı.|
|eventProcessorOptions/prefetchCount|300|Temel tarafından kullanılan varsayılan önceden getirme sayısı `EventProcessorHost` . İzin verilen en düşük değer 10 ' dur.|
|ınitialoffsetoptions/tür|fromStart|Depolamadaki bir denetim noktası mevcut olmadığında işlemenin başlatılacağı olay akışındaki konum. Seçenekler `fromStart` , `fromEnd` veya `fromEnqueuedTime` . `fromEnd` işlev uygulaması çalışmaya başladıktan sonra sıraya alınan yeni olayları işler. Tüm bölümler için geçerlidir.  Daha fazla bilgi için [Eventprocessoroptions belgelerine](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider?view=azure-dotnet)bakın.|
|ınitialoffsetoptions/enqueuedTime|Yok| İşlemin başlatılacağı akışta olayın sıraya alınma süresini belirtir. `initialOffsetOptions/type`Olarak yapılandırıldığında `fromEnqueuedTime` , bu ayar zorunludur. [DateTime. Parse ()](/dotnet/standard/base-types/parsing-datetime)tarafından desteklenen herhangi bir biçimdeki süreyi destekler `2020-10-26T20:31Z` . Açıklık için bir saat dilimi de belirtmeniz gerekir. Saat dilimi belirtilmediğinde Işlevler, Azure üzerinde çalışırken UTC olan işlev uygulamasını çalıştıran makinenin yerel saat dilimini varsayar. Daha fazla bilgi için [Eventprocessoroptions belgelerine](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider?view=azure-dotnet)bakın.|
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

|Özellik  |Varsayılan | Description |
|---------|---------|---------| 
|maxBatchSize|64|Alma döngüsü başına alınan en yüksek olay sayısı.|
|prefetchCount|yok|Temel tarafından kullanılacak varsayılan ön getirme `EventProcessorHost` .| 
|Batchcheckpointflik|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay toplu işlemlerinin sayısı.| 

> [!NOTE]
> Azure Işlevleri 1. x ' teki host.jsbaşvurusu için bkz. [ Azure işlevleri için başvuruhost.js, 1. x](../articles/azure-functions/functions-host-json-v1.md).
