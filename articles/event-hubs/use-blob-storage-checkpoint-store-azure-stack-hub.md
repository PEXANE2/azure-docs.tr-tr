---
title: Azure Stack hub 'da (Önizleme) denetim noktası deposu olarak blob depolamayı kullanma
description: Bu makalede, Azure Stack hub 'ında (Önizleme) Event Hubs bir denetim noktası deposu olarak blob Storage 'ın nasıl kullanılacağı açıklanır.
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: 1c876ed36be6aec9395a1acba3a1deb25a47de2c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87039234"
---
# <a name="use-blob-storage-as-checkpoint-store---event-hubs-on-azure-stack-hub-preview"></a>Blob depolamayı denetim noktası deposu olarak kullanma-Azure Stack hub 'da Event Hubs (Önizleme)
Azure Blob depolama 'Yı, genellikle Azure 'da bulunan farklı bir Storage blob SDK sürümünü destekleyen bir ortamda denetim noktası deposu olarak kullanıyorsanız, depolama hizmeti API sürümünü bu ortam tarafından desteklenen belirli bir sürümle değiştirmek için kodu kullanmanız gerekir. Örneğin, [Event Hubs bir Azure Stack hub sürümü 2002](/azure-stack/user/event-hubs-overview)çalıştırıyorsanız, depolama hizmeti için en yüksek sürüm 2017-11-09 ' dir. Bu durumda, Storage Service API sürümünü 2017-11-09 'e hedeflemek için kodu kullanmanız gerekir. Belirli bir depolama API sürümünün nasıl hedeflenecek hakkında bir örnek için GitHub 'da şu örneklere bakın: 

- [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/Sample10_RunningWithDifferentStorageVersion.cs)
- [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithCustomStorageVersion.java). 
- [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/javascript/receiveEventsWithApiSpecificStorage.js) veya [TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/typescript/src/receiveEventsWithApiSpecificStorage.ts) 
- Python- [zaman uyumlu](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob/samples/receive_events_using_checkpoint_store_storage_api_version.py), [zaman uyumsuz](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/receive_events_using_checkpoint_store_storage_api_version_async.py)

> [!IMPORTANT]
> Azure Stack hub Event Hubs Şu anda [Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) aşamasındadır ve ücretsizdir. 

Blob depolamayı kullanan Event Hubs alıcıyı, Azure Stack hub 'ın desteklediği sürümü hedeflemeden denetim noktası deposu olarak çalıştırırsanız, aşağıdaki hata iletisini alırsınız:

```
The value for one of the HTTP headers is not in the correct format
```


## <a name="sample-error-message-in-python"></a>Python 'da örnek hata iletisi
Python için bir hata, `azure.core.exceptions.HttpResponseError` öğesinin hata işleyicisine geçirilir `on_error(partition_context, error)` `EventHubConsumerClient.receive()` . Ancak, yöntemi `receive()` özel durum oluşturmaz. `print(error)`Aşağıdaki özel durum bilgilerini yazdırır:

```bash
The value for one of the HTTP headers is not in the correct format.

RequestId:f048aee8-a90c-08ba-4ce1-e69dba759297
Time:2020-03-17T22:04:13.3559296Z
ErrorCode:InvalidHeaderValue
Error:None
HeaderName:x-ms-version
HeaderValue:2019-07-07
```

Günlükçü aşağıdaki gibi iki uyarıyı günlüğe kaydeder:

```bash
WARNING:azure.eventhub.extensions.checkpointstoreblobaio._blobstoragecsaio: 
An exception occurred during list_ownership for namespace '<namespace-name>.eventhub.<region>.azurestack.corp.microsoft.com' eventhub 'python-eh-test' consumer group '$Default'. 

Exception is HttpResponseError('The value for one of the HTTP headers is not in the correct format.\nRequestId:f048aee8-a90c-08ba-4ce1-e69dba759297\nTime:2020-03-17T22:04:13.3559296Z\nErrorCode:InvalidHeaderValue\nError:None\nHeaderName:x-ms-version\nHeaderValue:2019-07-07')

WARNING:azure.eventhub.aio._eventprocessor.event_processor:EventProcessor instance '26d84102-45b2-48a9-b7f4-da8916f68214' of eventhub 'python-eh-test' consumer group '$Default'. An error occurred while load-balancing and claiming ownership. 

The exception is HttpResponseError('The value for one of the HTTP headers is not in the correct format.\nRequestId:f048aee8-a90c-08ba-4ce1-e69dba759297\nTime:2020-03-17T22:04:13.3559296Z\nErrorCode:InvalidHeaderValue\nError:None\nHeaderName:x-ms-version\nHeaderValue:2019-07-07'). Retrying after 71.45254944090853 seconds
```



## <a name="next-steps"></a>Sonraki adımlar

Bölümlendirme ve sağlama hakkında bilgi edinmek için aşağıdaki makaleye bakın: [uygulamanızın birden çok örneğinde bölüm yükünü dengeleme](event-processor-balance-partition-load.md)
