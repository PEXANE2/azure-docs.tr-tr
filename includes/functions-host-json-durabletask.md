---
title: include dosyası
description: include dosyası
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/14/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 6bb59db4c1b31033b1e116742dedc94621b1c60d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80117138"
---
[Dayanıklı Fonksiyonlar](../articles/azure-functions/durable-functions-overview.md)için yapılandırma ayarları.

### <a name="durable-functions-1x"></a>Dayanıklı Fonksiyonlar 1.x

```json
{
  "durableTask": {
    "hubName": "MyTaskHub",
    "controlQueueBatchSize": 32,
    "partitionCount": 4,
    "controlQueueVisibilityTimeout": "00:05:00",
    "workItemQueueVisibilityTimeout": "00:05:00",
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
    "maxQueuePollingInterval": "00:00:30",
    "azureStorageConnectionStringName": "AzureWebJobsStorage",
    "trackingStoreConnectionStringName": "TrackingStorage",
    "trackingStoreNamePrefix": "DurableTask",
    "traceInputsAndOutputs": false,
    "logReplayEvents": false,
    "eventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "eventGridKeySettingName":  "EventGridKey",
    "eventGridPublishRetryCount": 3,
    "eventGridPublishRetryInterval": "00:00:30",
    "eventGridPublishEventTypes": ["Started", "Completed", "Failed", "Terminated"]
  }
}
```

### <a name="durable-functions-2x"></a><a name="durable-functions-2-0-host-json"></a>Dayanıklı Fonksiyonlar 2.x

```json
{
 "extensions": {
  "durableTask": {
    "hubName": "MyTaskHub",
    "storageProvider": {
      "connectionStringName": "AzureWebJobsStorage",
      "controlQueueBatchSize": 32,
      "controlQueueBufferThreshold": 256,
      "controlQueueVisibilityTimeout": "00:05:00",
      "maxQueuePollingInterval": "00:00:30",
      "partitionCount": 4,
      "trackingStoreConnectionStringName": "TrackingStorage",
      "trackingStoreNamePrefix": "DurableTask",
      "workItemQueueVisibilityTimeout": "00:05:00",
    },
    "tracing": {
      "traceInputsAndOutputs": false,
      "traceReplayEvents": false,
    },
    "notifications": {
      "eventGrid": {
        "topicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
        "keySettingName": "EventGridKey",
        "publishRetryCount": 3,
        "publishRetryInterval": "00:00:30",
        "publishEventTypes": [
          "Started",
          "Pending",
          "Failed",
          "Terminated"
        ]
      }
    },
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
    "extendedSessionsEnabled": false,
    "extendedSessionIdleTimeoutInSeconds": 30,
    "useGracefulShutdown": false
  }
  }
}

```

Görev hub adları bir harfle başlamalı ve yalnızca harf ve sayılardan oluşmalıdır. Belirtilmemişse, bir işlev uygulamasının varsayılan görev hub adı **DurableFunctionsHub'dır.** Daha fazla bilgi için [Görev hub'larına](../articles/azure-functions/durable-functions-task-hubs.md)bakın.

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|hubName|Dayanıklı FonksiyonlarHub|Alternatif [görev hub](../articles/azure-functions/durable-functions-task-hubs.md) adları, aynı depolama arka ucunu kullanıyor olsalar bile, birden çok Dayanıklı İşlev uygulamasını birbirinden ayırmak için kullanılabilir.|
|controlQueueBatchSize|32|Bir seferde denetim kuyruğundan çekilecek ileti sayısı.|
|controlQueueBufferThreshold|256|Bir defada bellekte arabelleğe alınabilecek denetim sırası iletilerinin sayısı, bu noktada gönderici ek iletileri sıralamadan önce bekler.|
|partitionCount |4|Denetim sırası için bölüm sayısı. 1 ile 16 arasında pozitif bir tamsayı olabilir.|
|controlQueueVisibilityTimeout |5 dakika|Sırayı yikama denetim sırası iletilerinin görünürlük zaman ları.|
|workItemQueueVisibilityTimeout |5 dakika|Sıradan çıkmış iş öğesi sıra iletilerinin görünürlük zaman süresi.|
|maxConcurrentAktiviteFonksiyonları |Geçerli makinedeki işlemci sayısı 10 kat|Tek bir ana bilgisayar örneğinde aynı anda işlenebilen maksimum etkinlik işlevi sayısı.|
|maxConcurrentOrchestratorFunctions |Geçerli makinedeki işlemci sayısı 10 kat|Tek bir ana bilgisayar örneğinde aynı anda işlenebilen maksimum orchestrator işlevi sayısı.|
|maxQueuePollingInterval|30 saniye|*Hh:mm:ss* formatındaki maksimum denetim ve iş öğesi sıra yoklama aralığı. Daha yüksek değerler, ileti işleme gecikmelerinin artmasına neden olabilir. Daha düşük değerler, artan depolama işlemleri nedeniyle daha yüksek depolama maliyetlerine neden olabilir.|
|azureStorageConnectionStringName |AzureWebJobsStorage|Temel Azure Depolama kaynaklarını yönetmek için kullanılan Azure Depolama bağlantı dizesine sahip uygulama ayarı adı.|
|izlemeStoreConnectionStringName||Geçmiş ve Örnekler tabloları için kullanılacak bir bağlantı dizesinin adı. Belirtilmemişse, `azureStorageConnectionStringName` bağlantı kullanılır.|
|izlemeStoreNamePrefix||Belirtildiğinde `trackingStoreConnectionStringName` Geçmiş ve Örnekler tabloları için kullanılacak önek. Ayarlanmazsa, varsayılan önek değeri `DurableTask`. `trackingStoreConnectionStringName` Belirtilmemişse, Geçmiş ve Örnekler tabloları `hubName` değeri önekleri olarak kullanır ve `trackingStoreNamePrefix` herhangi bir ayar yoksayılır.|
|traceInputsAndOutputs |yanlış|İşlev çağrılarının girdi ve çıktılarının izlenip izlenmeyeceğini gösteren bir değer. İşlev yürütme olaylarını takip ederken varsayılan davranış, işlev çağrıları için serileştirilmiş giriş ve çıktılara bayt sayısını eklemektir. Bu davranış, günlükleri şişirme veya yanlışlıkla hassas bilgileri açığa çıkarmadan giriş ve çıkışların nasıl göründüğü hakkında en az bilgi sağlar. Bu özelliği niçin doğru olarak ayarlamak, varsayılan işlev günlüğe kaydetmenin işlev giriş ve çıktılarının tüm içeriğini günlüğe kaydetmesini sağlar.|
|logReplayEtkinlikler|yanlış|Uygulama Öngörüleri'ne düzenleme tekrar etkinlikleri yazıp yazılmayacağını gösteren bir değer.|
|olayGridTopicEndpoint ||Azure Olay Izgarası özel konu bitiş noktasının URL'si. Bu özellik ayarlandığında, orkestrasyon yaşam döngüsü bildirim olayları bu bitiş noktasına yayımlanır. Bu özellik Uygulama Ayarları çözünürlüğünü destekler.|
|olayGridKeySettingName ||Azure Olay Izgarası özel konusuyla kimlik doğrulaması için kullanılan `EventGridTopicEndpoint`anahtarı içeren uygulama ayarı adı.|
|olayGridPublishRetryCount|0|Olay Izgara Konusu'na yayımlama başarısız olursa yeniden deneme sayısı.|
|olayGridPublishRetryInterval|5 dakika|Olay *Izgarahh:mm:ss* biçiminde yeniden deneme aralığı yayımlar.|
|olayGridPublishEventTypes||Olay Izgara'da yayımlanacak olay türlerinin listesi. Belirtilmemişse, tüm olay türleri yayımlanacaktır. İzin verilen `Started` `Completed`değerler `Failed` `Terminated`, , , .|
|useGracefulShutdown|yanlış|(Önizleme) Ana bilgisayar kapatmalarının işlem içi işlev yürütmelerinde başarısız olma olasılığını azaltmak için incelikle kapatmayı etkinleştirin.|

Bu ayarların çoğu performansı optimize etmek içindir. Daha fazla bilgi için [Performans ve ölçek'e](../articles/azure-functions/durable-functions-perf-and-scale.md)bakın.
