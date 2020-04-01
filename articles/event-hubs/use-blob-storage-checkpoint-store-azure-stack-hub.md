---
title: Azure Yığını Hub'ında denetim noktası deposu olarak Blob Depolama'yı kullanma (önizleme)
description: Bu makalede, Azure Yığını Hub'ındaki Olay Hub'larında Blob Depolama'nın denetim noktası deposu olarak nasıl kullanılacağı açıklanmaktadır (önizleme).
services: event-hubs
documentationcenter: na
author: spelluru
ms.service: event-hubs
ms.topic: how-to
ms.date: 03/18/2020
ms.author: spelluru
ms.openlocfilehash: 1f0e4dea44007ef82cb4b700ff0be4a5579541d8
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398928"
---
# <a name="use-blob-storage-as-checkpoint-store---event-hubs-on-azure-stack-hub-preview"></a>Blob Depolama'yı denetim noktası deposu olarak kullanma - Azure Stack Hub'ındaki Etkinlik Hub'ları (önizleme)
Azure Blob Depolama'yı, Depolama Blob SDK'nın azure'da genellikle kullanılabilir sürümünden farklı bir sürümünü destekleyen bir ortamda denetim noktası deposu olarak kullanıyorsanız, Depolama hizmeti API sürümünü bu ortam tarafından desteklenen belirli sürümle değiştirmek için kodu kullanmanız gerekir. Örneğin, [Bir Azure Yığını Hub sürümü 2002'de Etkinlik Hub'ları](https://docs.microsoft.com/azure-stack/user/event-hubs-overview)çalıştırıyorsanız, Depolama hizmeti için kullanılabilir en yüksek sürüm 2017-11-09 sürümüdür. Bu durumda, Depolama hizmeti API sürümünü 2017-11-09'a hedeflemek için kod kullanmanız gerekir. Belirli bir Depolama API sürümünü niçin hedefleneceksiniz hakkında bir örnek için, GitHub'daki şu örneklere bakın: 

- [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/Sample10_RunningWithDifferentStorageVersion.cs)
- [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithOlderStorageVersion.java). 
- [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsWithDownleveledStorage.js) veya [TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsWithDownleveledStorage.ts), 
- Python - [Senkron](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob/samples/event_processor_blob_storage_example_with_storage_api_version.py), [Asynchronous](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/event_processor_blob_storage_example_with_storage_api_version.py)

> [!IMPORTANT]
> Azure Yığını Hub'ındaki Olay Hub'ları şu anda [önizlemede](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) ve ücretsiz. 

Azure Stack Hub'ın desteklediği sürümü hedeflemeden Blob Depolama'yı denetim noktası deposu olarak kullanan Event Hub'ları alıcısı çalıştırırsanız, aşağıdaki hata iletisini alırsınız:

```
The value for one of the HTTP headers is not in the correct format
```


## <a name="sample-error-message-in-python"></a>Python'da örnek hata iletisi
Python için hata, `azure.core.exceptions.HttpResponseError` hata işleyicisine `on_error(partition_context, error)` `EventHubConsumerClient.receive()`aktarılır. Ancak, yöntem `receive()` bir istisna yükseltmez. `print(error)`aşağıdaki özel durum bilgilerini yazdırır:

```bash
The value for one of the HTTP headers is not in the correct format.

RequestId:f048aee8-a90c-08ba-4ce1-e69dba759297
Time:2020-03-17T22:04:13.3559296Z
ErrorCode:InvalidHeaderValue
Error:None
HeaderName:x-ms-version
HeaderValue:2019-07-07
```

Kaydedici aşağıdakiler gibi iki uyarı yı zedler:

```bash
WARNING:azure.eventhub.extensions.checkpointstoreblobaio._blobstoragecsaio: 
An exception occurred during list_ownership for namespace '<namespace-name>.eventhub.<region>.azurestack.corp.microsoft.com' eventhub 'python-eh-test' consumer group '$Default'. 

Exception is HttpResponseError('The value for one of the HTTP headers is not in the correct format.\nRequestId:f048aee8-a90c-08ba-4ce1-e69dba759297\nTime:2020-03-17T22:04:13.3559296Z\nErrorCode:InvalidHeaderValue\nError:None\nHeaderName:x-ms-version\nHeaderValue:2019-07-07')

WARNING:azure.eventhub.aio._eventprocessor.event_processor:EventProcessor instance '26d84102-45b2-48a9-b7f4-da8916f68214' of eventhub 'python-eh-test' consumer group '$Default'. An error occurred while load-balancing and claiming ownership. 

The exception is HttpResponseError('The value for one of the HTTP headers is not in the correct format.\nRequestId:f048aee8-a90c-08ba-4ce1-e69dba759297\nTime:2020-03-17T22:04:13.3559296Z\nErrorCode:InvalidHeaderValue\nError:None\nHeaderName:x-ms-version\nHeaderValue:2019-07-07'). Retrying after 71.45254944090853 seconds
```



## <a name="next-steps"></a>Sonraki adımlar

Bölümleme ve denetim noktası hakkında aşağıdaki makaleye bakın: [Uygulamanızın birden çok örneğinde bölüm yükünü dengeleyin](event-processor-balance-partition-load.md)
