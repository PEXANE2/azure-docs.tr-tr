---
title: Kullanılabilirlik ve tutarlılık-Azure Event Hubs | Microsoft Docs
description: Bölümler kullanılarak Azure Event Hubs maksimum kullanılabilirlik ve tutarlılık sağlama.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 497a6e7430c4e6f8e29f903294ca94a4cb23012b
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85315770"
---
# <a name="availability-and-consistency-in-event-hubs"></a>Event Hubs’da kullanılabilirlik ve tutarlılık

## <a name="overview"></a>Genel Bakış
Azure Event Hubs, tek bir olay hub 'ında kullanılabilirliği ve paralelleştirme geliştirmek için bir [bölümlendirme modeli](event-hubs-scalability.md#partitions) kullanır. Örneğin, bir olay hub 'ının dört bölümü varsa ve bir yük dengeleme işleminde bu bölümlerden biri bir sunucudan diğerine taşınırsa, yine de üç bölümden birini gönderebilir ve alabilirsiniz. Ayrıca, daha fazla bölüm olması, verilerinizi işletirecek daha fazla eşzamanlı okuyucu sağlamanıza olanak sağlar ve böylece toplam aktarım hızını geliştirir. Dağıtılmış bir sistemde bölümlemenin ve sıralamanın etkilerini anlamak çözüm tasarımının önemli bir yönüdür.

Sipariş ve kullanılabilirlik arasındaki ticaretin açıklanmasına yardımcı olmak için, Brewer 'in Theokal olarak da bilinen [Cap](https://en.wikipedia.org/wiki/CAP_theorem)'ler için bkz.. Bu, tutarlılık, kullanılabilirlik ve bölüm toleransı arasındaki seçimi anlatmaktadır. Ağ tarafından bölümlenmiş sistemler için tutarlılık ve kullanılabilirlik arasında her zaman zorunluluğunu getirir olduğunu belirtir.

Brewer 'in bu, tutarlılığı ve kullanılabilirliği aşağıdaki şekilde tanımlar:
* Bölüm toleransı: bir bölüm hatası oluşması durumunda bile veri işleme sisteminin veri işlemeye devam etmesi özelliği.
* Kullanılabilirlik: başarısız olmayan bir düğüm makul bir süre içinde makul bir yanıt döndürür (hata veya zaman aşımları olmadan).
* Tutarlılık: belirli bir istemci için en son yazmayı döndürecek bir okuma garantisi vardır.

## <a name="partition-tolerance"></a>Bölüm toleransı
Event Hubs, bölümlenmiş bir veri modelinin üzerine kurulmuştur. Kurulum sırasında olay hub 'ınızdaki bölüm sayısını yapılandırabilirsiniz, ancak daha sonra bu değeri değiştiremezsiniz. Event Hubs olan bölümleri kullanmanız gerektiğinden, uygulamanız için kullanılabilirlik ve tutarlılık hakkında bir karar vermeniz gerekir.

## <a name="availability"></a>Kullanılabilirlik
Event Hubs kullanmaya başlamanın en kolay yolu, varsayılan davranışı kullanmaktır. 

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure. Messaging. EventHubs (5.0.0 veya üzeri)](#tab/latest)
Yeni bir **[EventHubProducerClient](/dotnet/api/azure.messaging.eventhubs.producer.eventhubproducerclient?view=azure-dotnet)** nesnesi oluşturur ve **[sendadsync](/dotnet/api/azure.messaging.eventhubs.producer.eventhubproducerclient.sendasync?view=azure-dotnet)** yöntemini kullanırsanız, olaylarınız Olay Hub 'ınızdaki bölümler arasında otomatik olarak dağıtılır. Bu davranış, en fazla çalışma süresi sağlar.

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft. Azure. EventHubs (4.1.0 veya önceki sürümler)](#tab/old)
Yeni bir **[Eventhubclient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient)** nesnesi oluşturur ve **[Send](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync?view=azure-dotnet#Microsoft_Azure_EventHubs_EventHubClient_SendAsync_Microsoft_Azure_EventHubs_EventData_)** yöntemini kullanırsanız, olaylarınız Olay Hub 'ınızdaki bölümler arasında otomatik olarak dağıtılır. Bu davranış, en fazla çalışma süresi sağlar.

---

En uzun süre gerektiren kullanım örnekleri için bu model tercih edilir.

## <a name="consistency"></a>Tutarlılık
Bazı senaryolarda, olayların sıralaması önemli olabilir. Örneğin, arka uç sisteminizin silme komutundan önce bir update komutunu işlemesini isteyebilirsiniz. Bu örnekte, bir olayda bölüm anahtarını ayarlayabilir veya `PartitionSender` yalnızca belirli bir bölüme olayları göndermek için bir nesne (eski Microsoft. Azure. Messaging kitaplığını kullanıyorsanız) kullanabilirsiniz. Bunun yapılması, bu olaylar bölümden okunduklarında, bunların sırayla okunmasını sağlar. **Azure. Messaging. EventHubs** kitaplığını kullanıyorsanız ve daha fazla bilgi için bkz. [olayları bir bölüme yayımlamak için Partitionsender 'Dan EventHubProducerClient 'ye geçirme](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md#migrating-code-from-partitionsender-to-eventhubproducerclient-for-publishing-events-to-a-partition).

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

Bu yapılandırmayla, gönderdiğiniz belirli bir bölüm kullanılamaz durumdaysa, bir hata yanıtı alacağını aklınızda bulundurun. Bir karşılaştırma noktası olarak, tek bir bölüme yönelik benzeşim yoksa Event Hubs hizmeti, olaylarınızı bir sonraki kullanılabilir bölüme gönderir.

Sıralamayı güvence altına almak için olası bir çözüm, aynı zamanda süreyi de en üst düzeye çıkararak olay işleme uygulamanızın bir parçası olarak olayları toplar. Bunu yapmanın en kolay yolu, olaylarınızı özel bir sıra numarası özelliğiyle damgakullanmaktır. Aşağıdaki kodda bir örnek gösterilir:

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

* [Event Hubs hizmete genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay hub’ı oluşturma](event-hubs-create.md)
