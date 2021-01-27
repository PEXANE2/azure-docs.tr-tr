---
title: Kullanılabilirlik ve tutarlılık-Azure Event Hubs | Microsoft Docs
description: Bölümler kullanılarak Azure Event Hubs maksimum kullanılabilirlik ve tutarlılık sağlama.
ms.topic: article
ms.date: 01/25/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 2fdb62e953230a38a26d22e136789fea52c8ee8c
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98882204"
---
# <a name="availability-and-consistency-in-event-hubs"></a>Event Hubs’da kullanılabilirlik ve tutarlılık
Bu makalede, Azure Event Hubs tarafından desteklenen kullanılabilirlik ve tutarlılık hakkında bilgi sağlanır. 

## <a name="availability"></a>Kullanılabilirlik
Azure Event Hubs, bir veri merkezinde birden çok hata etki alanına yayılan kümeler genelinde tek tek makinelerin veya hatta tamamen oluşan tüm hatalardan oluşan ciddi arızaların riskini yayar. Hizmetin, garanti edilen hizmet düzeylerinde çalışmaya devam edebilmesi ve genellikle bu tür bir hata oluştuğunda dikkat çekici kesintiler olmadan, saydam hata algılama ve yük devretme mekanizmaları uygular. [Kullanılabilirlik alanları](../availability-zones/az-overview.md)için etkin seçeneğiyle bir Event Hubs ad alanı oluşturulduysa, kesinti riski üç fiziksel olarak ayrılmış tesislere göre daha fazla yayılır ve hizmette tüm tesis için eksiksiz ve çok zararlı kayıplarla anında kapasiteye yetecek kapasite ayırmalar vardır. Daha fazla bilgi için bkz. [Azure Event Hubs-coğrafi olağanüstü durum kurtarma](event-hubs-geo-dr.md).

Bir istemci uygulaması olayları bir olay hub 'ına gönderdiğinde, olaylar Olay Hub 'ınızdaki bölümler arasında otomatik olarak dağıtılır. Bir bölüm bir nedenle kullanılamıyorsa, olaylar kalan bölümler arasında dağıtılır. Bu davranış, en fazla çalışma süresi sağlar. En uzun süre gerektiren kullanım örnekleri için, bu model belirli bir bölüme olay göndermek yerine tercih edilir. Daha fazla bilgi için bkz. [bölümler](event-hubs-scalability.md#partitions).

## <a name="consistency"></a>Tutarlılık
Bazı senaryolarda, olayların sıralaması önemli olabilir. Örneğin, arka uç sisteminizin silme komutundan önce bir update komutunu işlemesini isteyebilirsiniz. Bu senaryoda, bir istemci uygulaması sıralama korunabilmesi için olayları belirli bir bölüme gönderir. Bir tüketici uygulaması bölümden bu olayları tükettiği zaman sırayla okunabilir. 

Bu yapılandırmayla, gönderdiğiniz belirli bir bölüm kullanılamaz durumdaysa, bir hata yanıtı alacağını aklınızda bulundurun. Karşılaştırma noktası olarak, tek bir bölüme yönelik benzeşim yoksa Event Hubs hizmeti, olaylarınızı bir sonraki kullanılabilir bölüme gönderir.

Sıralamayı güvence altına almak için olası bir çözüm, aynı zamanda süreyi de en üst düzeye çıkararak olay işleme uygulamanızın bir parçası olarak olayları toplar. Bunu yapmanın en kolay yolu, olaylarınızı özel bir sıra numarası özelliğiyle damgakullanmaktır.

Bu senaryoda, üretici istemcisi Olay Hub 'ınızdaki kullanılabilir bölümlerden birine olaylar gönderir ve karşılık gelen sıra numarasını uygulamanızdan ayarlar. Bu çözüm, durumun işlem uygulamanız tarafından tutulmasını gerektirir, ancak göndermenize daha büyük olabilecek bir uç nokta verir.

## <a name="appendix"></a>Ek

### <a name="net-examples"></a>.NET örnekleri

#### <a name="send-events-to-a-specific-partition"></a>Olayları belirli bir bölüme gönder
Olayları yalnızca belirli bir bölüme göndermek için bir olayda bölüm anahtarını ayarlayın veya bir `PartitionSender` nesne (eski Microsoft. Azure. Messaging kitaplığını kullanıyorsanız) kullanın. Bunun yapılması, bu olaylar bölümden okunduklarında, bunların sırayla okunmasını sağlar. 

Daha yeni **Azure. Messaging. EventHubs** kitaplığını kullanıyorsanız, [bir bölüme olay yayımlamak Için bkz. partitionsender 'dan kodu EventHubProducerClient 'a geçirme](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md#migrating-code-from-partitionsender-to-eventhubproducerclient-for-publishing-events-to-a-partition).

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure. Messaging. EventHubs (5.0.0 veya üzeri)](#tab/latest)

```csharp
var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
var eventHubName = "<< NAME OF THE EVENT HUB >>";

await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
{
    var batchOptions = new CreateBatchOptions() { PartitionId = "my-partition-id" };
    using EventDataBatch eventBatch = await producerClient.CreateBatchAsync(batchOptions);
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First")));
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second")));
    
    await producerClient.SendAsync(eventBatch);
}
```

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft. Azure. EventHubs (4.1.0 veya önceki sürümler)](#tab/old)

```csharp
var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
var eventHubName = "<< NAME OF THE EVENT HUB >>";

var connectionStringBuilder = new EventHubsConnectionStringBuilder(connectionString){ EntityPath = eventHubName }; 
var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
PartitionSender partitionSender = eventHubClient.CreatePartitionSender("my-partition-id");
try
{
    EventDataBatch eventBatch = partitionSender.CreateBatch();
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First")));
    eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second")));

    await partitionSender.SendAsync(eventBatch);
}
finally
{
    await partitionSender.CloseAsync();
    await eventHubClient.CloseAsync();
}
```

---

### <a name="set-a-sequence-number"></a>Sıra numarası ayarla
Aşağıdaki örnek, olaylarınızı özel bir sıra numarası özelliği ile damgalar. 

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure. Messaging. EventHubs (5.0.0 veya üzeri)](#tab/latest)

```csharp
// create a producer client that you can use to send events to an event hub
await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
{
    // get the latest sequence number from your application
    var sequenceNumber = GetNextSequenceNumber();

    // create a batch of events 
    using EventDataBatch eventBatch = await producerClient.CreateBatchAsync();

    // create a new EventData object by encoding a string as a byte array
    var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));

    // set a custom sequence number property
    data.Properties.Add("SequenceNumber", sequenceNumber);

    // add events to the batch. An event is a represented by a collection of bytes and metadata. 
    eventBatch.TryAdd(data);

    // use the producer client to send the batch of events to the event hub
    await producerClient.SendAsync(eventBatch);
}
```

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft. Azure. EventHubs (4.1.0 veya önceki sürümler)](#tab/old)
```csharp
// Create an Event Hubs client
var client = new EventHubClient(connectionString, eventHubName);

//Create a producer to produce events
EventHubProducer producer = client.CreateProducer();

// Get the latest sequence number from your application 
var sequenceNumber = GetNextSequenceNumber();

// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));

// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);

// Send single message async
await producer.SendAsync(data);
```
---

Bu örnek, olaylarınızı Olay Hub 'ınızdaki kullanılabilir bölümlerden birine gönderir ve karşılık gelen sıra numarasını uygulamanızdan ayarlar. Bu çözüm, durumun işlem uygulamanız tarafından tutulmasını gerektirir, ancak göndermenize daha büyük olabilecek bir uç nokta verir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs hizmete genel bakış](./event-hubs-about.md)
* [Olay Hub 'ı oluşturma](event-hubs-create.md)
