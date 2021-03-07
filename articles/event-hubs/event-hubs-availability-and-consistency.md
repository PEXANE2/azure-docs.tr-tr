---
title: Kullanılabilirlik ve tutarlılık-Azure Event Hubs | Microsoft Docs
description: Bölümler kullanılarak Azure Event Hubs maksimum kullanılabilirlik ve tutarlılık sağlama.
ms.topic: article
ms.date: 01/25/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 325cc80daba2a44dedbd5e09ac4858ae2815c1cd
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102425932"
---
# <a name="availability-and-consistency-in-event-hubs"></a>Event Hubs’da kullanılabilirlik ve tutarlılık
Bu makalede, Azure Event Hubs tarafından desteklenen kullanılabilirlik ve tutarlılık hakkında bilgi sağlanır. 

## <a name="availability"></a>Kullanılabilirlik
Azure Event Hubs, bir veri merkezinde birden çok hata etki alanına yayılan kümeler genelinde tek tek makinelerin veya hatta tamamen oluşan tüm hatalardan oluşan ciddi arızaların riskini yayar. Hizmetin, garanti edilen hizmet düzeylerinde çalışmaya devam edebilmesi ve genellikle bu tür bir hata oluştuğunda dikkat çekici kesintiler olmadan, saydam hata algılama ve yük devretme mekanizmaları uygular. 

[Kullanılabilirlik alanları](../availability-zones/az-overview.md) etkin olan bir Event Hubs ad alanı oluşturulduysa, kesinti riski üç fiziksel olarak ayrılmış tesislere göre daha fazla yayılır ve hizmette, tüm tesis için eksiksiz ve çok zararlı bir kayıpla hemen başa çıkabilecek kapasite ayırmalar vardır. Daha fazla bilgi için bkz. [Azure Event Hubs-coğrafi olağanüstü durum kurtarma](event-hubs-geo-dr.md).

Bir istemci uygulaması, bir bölümü belirtmeden olayları bir olay hub 'ına gönderdiğinde, olaylar Olay Hub 'ınızdaki bölümler arasında otomatik olarak dağıtılır. Bir bölüm bir nedenle kullanılamıyorsa, olaylar kalan bölümler arasında dağıtılır. Bu davranış, en fazla çalışma süresi sağlar. En uzun süre gerektiren kullanım örnekleri için, bu model belirli bir bölüme olay göndermek yerine tercih edilir. 

### <a name="availability-considerations-when-using-a-partition-id-or-key"></a>Bölüm KIMLIĞI veya anahtarı kullanırken kullanılabilirlik konuları
Bölüm KIMLIĞI veya bölüm anahtarı kullanmak isteğe bağlıdır. Bir tane kullanıp kullanmayacağınızı dikkatle düşünün. Bir olayı yayımlarken bir bölüm KIMLIĞI/anahtar belirtmezseniz Event Hubs, yükü bölümler arasında dengeler. Bölüm KIMLIĞI/anahtarı kullandığınızda, bu bölümler tek bir düğümde kullanılabilirlik gerektirir ve zaman içinde kesintiler meydana gelebilir. Örneğin, işlem düğümlerinin yeniden başlatılması veya düzeltme eki uygulanması gerekebilir. Bu nedenle, bir bölüm KIMLIĞI/anahtarı ayarlarsanız ve bu bölüm bazı nedenlerle kullanılamaz hale gelirse, bu bölümdeki verilere erişme girişimi başarısız olur. Yüksek kullanılabilirlik en önemse, bölüm KIMLIĞI/anahtar belirtmeyin. Bu durumda, olaylar iç yük dengeleme algoritması kullanılarak bölümlere gönderilir. Bu senaryoda, kullanılabilirlik (bölüm KIMLIĞI/anahtar yok) ve tutarlılık (olayları belirli bir bölüme sabitleme) arasında açık bir seçim yapabilirsiniz. Bölüm KIMLIĞI/anahtar kullanımı, bir olay hub 'ının bölüm düzeyinde kullanılabilirliğini daha eski bir şekilde açar. 

### <a name="availability-considerations-when-handling-delays-in-processing-events"></a>Olayları işlemede gecikme yaparken kullanılabilirlik konuları
Diğer bir deyişle, olayları işlerken bir tüketici uygulaması ile ilgili gecikmelerden bahsede olursunuz. Bazı durumlarda, daha fazla aşağı akış işleme gecikmesine neden olabilecek şekilde, tüketici uygulamasının verileri bırakması ve işleme devam etmek yerine yeniden denemesi daha iyi olabilir. Örneğin, bir stok şeridi sayesinde, güncel verilerin tamamlanmasını beklemek daha iyidir, ancak canlı sohbet veya VOıP senaryosunda, tamamlanmamış olsa bile verilere hızlıca sahip olmanız önerilir.

Bu kullanılabilirlik konuları verildiğinde, tüketici uygulaması aşağıdaki hata işleme stratejilerinden birini seçebilir:

- Durdur (sorunlar düzeltilinceye kadar olay hub 'ından okumayı Durdur)
- Bırak (iletiler önemli değildir, bunları bırakın)
- Yeniden dene (iletileri uygun gördüğünüz şekilde yeniden deneyin)


## <a name="consistency"></a>Tutarlılık
Bazı senaryolarda, olayların sıralaması önemli olabilir. Örneğin, arka uç sisteminizin silme komutundan önce bir update komutunu işlemesini isteyebilirsiniz. Bu senaryoda, bir istemci uygulaması sıralama korunabilmesi için olayları belirli bir bölüme gönderir. Bir tüketici uygulaması bölümden bu olayları tükettiği zaman sırayla okunabilir. 

Bu yapılandırmayla, gönderdiğiniz belirli bir bölüm kullanılamaz durumdaysa, bir hata yanıtı alacağını aklınızda bulundurun. Karşılaştırma noktası olarak, tek bir bölüme yönelik benzeşim yoksa Event Hubs hizmeti, olaylarınızı bir sonraki kullanılabilir bölüme gönderir.


## <a name="appendix"></a>Ek

### <a name="send-events-to-a-specific-partition"></a>Olayları belirli bir bölüme gönder
Bu bölümde, C#, Java, Python ve JavaScript kullanarak belirli bir bölüme nasıl olay göndereceğiniz gösterilmektedir. 

### <a name="net"></a>[.NET](#tab/dotnet)
Bir olay hub 'ına bir olay toplu işinin nasıl gönderileceğini gösteren tam örnek kod için (bölüm KIMLIĞINI/anahtarını ayarlamadan) bkz. [azure Event Hubs (Azure. Messaging. EventHubs) ile olayları gönderme ve alma](event-hubs-dotnet-standard-getstarted-send.md).

Olayları belirli bir bölüme göndermek için, [](/dotnet/api/azure.messaging.eventhubs.producer.eventhubproducerclient.createbatchasync#Azure_Messaging_EventHubs_Producer_EventHubProducerClient_CreateBatchAsync_Azure_Messaging_EventHubs_Producer_CreateBatchOptions_System_Threading_CancellationToken_) `PartitionId` `PartitionKey` [createbatchoptions](//dotnet/api/azure.messaging.eventhubs.producer.createbatchoptions)içinde veya ' i belirterek EventHubProducerClient. createbatchasync metodunu kullanarak Batch oluşturun. Aşağıdaki kod, bir bölüm anahtarı belirterek belirli bir bölüme olay toplu işi gönderir. 

```csharp
var batchOptions = new CreateBatchOptions { PartitionKey = "cities" };
using var eventBatch = await producer.CreateBatchAsync(batchOptions);
```

Ayrıca, [SendEventOptions](/dotnet/api/azure.messaging.eventhubs.producer.sendeventoptions)içinde **PartitionID** veya **Partitionkey** belirterek [EventHubProducerClient. sendadsync](/dotnet/api/azure.messaging.eventhubs.producer.eventhubproducerclient.sendasync#Azure_Messaging_EventHubs_Producer_EventHubProducerClient_SendAsync_System_Collections_Generic_IEnumerable_Azure_Messaging_EventHubs_EventData__Azure_Messaging_EventHubs_Producer_SendEventOptions_System_Threading_CancellationToken_) yöntemini de kullanabilirsiniz.

```csharp
var sendEventOptions  = new SendEventOptions { PartitionKey = "cities" };
// create the events array
producer.SendAsync(events, sendOptions)
```

### <a name="java"></a>[Java](#tab/java)
Bir olay hub 'ına bir olay toplu işinin nasıl gönderileceğini gösteren tam örnek kod için (bölüm KIMLIĞI/anahtar ayarlamadan), bkz. [azure Event Hubs (Azure-Messaging-eventhubs) olayları göndermek veya almak Için Java kullanma](event-hubs-java-get-started-send.md).

Olayları belirli bir bölüme göndermek için [Createbatchoptions](/java/api/com.azure.messaging.eventhubs.models.createbatchoptions)IÇINDE **bölüm kimliğini** veya **bölüm anahtarını** belirterek [CreateBatch](/java/api/com.azure.messaging.eventhubs.eventhubproducerclient.createbatch) yöntemini kullanarak Batch oluşturun. Aşağıdaki kod, bir bölüm anahtarı belirterek belirli bir bölüme olay toplu işi gönderir. 

```java
CreateBatchOptions batchOptions = new CreateBatchOptions();
batchOptions.setPartitionKey("cities");
```

Ayrıca, [Sendoleyde](/java/api/com.azure.messaging.eventhubs.models.sendoptions) **bölüm kimliği** veya **bölüm anahtarı** belirterek [EventHubProducerClient. Send](/java/api/com.azure.messaging.eventhubs.eventhubproducerclient.send#com_azure_messaging_eventhubs_EventHubProducerClient_send_java_lang_Iterable_com_azure_messaging_eventhubs_EventData__com_azure_messaging_eventhubs_models_SendOptions_) yöntemini kullanabilirsiniz.

```java
List<EventData> events = Arrays.asList(new EventData("Melbourne"), new EventData("London"), new EventData("New York"));
SendOptions sendOptions = new SendOptions();
sendOptions.setPartitionKey("cities");
producer.send(events, sendOptions);
```

### <a name="python"></a>[Python](#tab/python) 
Bir olay hub 'ına bir olay toplu işinin nasıl gönderileceğini gösteren tam örnek kod için (bölüm KIMLIĞINI/anahtarını ayarlamadan) bkz. [Python (Azure-eventhub) kullanarak Olay Hub 'larından olayları gönderme veya olayları alma](event-hubs-python-get-started-send.md).

Olayları belirli bir bölüme göndermek için yöntemini kullanarak bir Batch oluştururken, [`EventHubProducerClient.create_batch`](/python/api/azure-eventhub/azure.eventhub.eventhubproducerclient#create-batch---kwargs-) veya öğesini belirtin `partition_id` `partition_key` . Ardından, [`EventHubProducerClient.send_batch`](/python/api/azure-eventhub/azure.eventhub.aio.eventhubproducerclient#send-batch-event-data-batch--typing-union-azure-eventhub--common-eventdatabatch--typing-list-azure-eventhub-) toplu işi Olay Hub 'ının bölümüne göndermek için yöntemini kullanın. 

```python
event_data_batch = await producer.create_batch(partition_key='cities')
```

Veya parametreleri için değerler belirterek [EventHubProducerClient.send_batch](/python/api/azure-eventhub/azure.eventhub.eventhubproducerclient#send-batch-event-data-batch----kwargs-) yöntemini de kullanabilirsiniz `partition_id` `partition_key` .

```python
producer.send_batch(event_data_batch, partition_key="cities")
```


### <a name="javascript"></a>[JavaScript](#tab/javascript)
Bir olay hub 'ına bir olay toplu işinin nasıl gönderileceğini gösteren tam örnek kod için (bölüm KIMLIĞINI/anahtarını ayarlamadan) bkz. [JavaScript (Azure/Event-hub) kullanarak Olay Hub 'larından olayları gönderme veya alma](event-hubs-node-get-started-send.md).

Olayları belirli bir bölüme göndermek için, veya belirterek [EventHubProducerClient. CreateBatchOptions](/javascript/api/@azure/event-hubs/eventhubproducerclient#createBatch_CreateBatchOptions_) nesnesini kullanarak [bir Batch oluşturun](/javascript/api/@azure/event-hubs/eventhubproducerclient#createBatch_CreateBatchOptions_) `partitionId` `partitionKey` . Ardından, [EventHubProducerClient. sendbatch](/javascript/api/@azure/event-hubs/eventhubproducerclient#sendBatch_EventDataBatch__OperationOptions_) yöntemini kullanarak toplu işi Olay Hub 'ına gönderin. 

Aşağıdaki örneğe bakın.

```javascript
const batchOptions = { partitionKey = "cities"; };
const batch = await producer.createBatch(batchOptions);
```

Ayrıca, [Sendbatchoptions](/javascript/api/@azure/event-hubs/sendbatchoptions)IÇINDE **bölüm kimliğini** veya **bölüm anahtarını** belirterek [EventHubProducerClient. sendbatch](/javascript/api/@azure/event-hubs/eventhubproducerclient#sendBatch_EventData____SendBatchOptions_) yöntemini de kullanabilirsiniz.

```javascript
const sendBatchOptions = { partitionKey = "cities"; };
// prepare events
producer.sendBatch(events, sendBatchOptions);
```

---


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs hizmete genel bakış](./event-hubs-about.md)
* [Olay Hub 'ı oluşturma](event-hubs-create.md)
