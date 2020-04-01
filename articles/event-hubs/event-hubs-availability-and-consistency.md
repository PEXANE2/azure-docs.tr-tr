---
title: Kullanılabilirlik ve tutarlılık - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bölümleri kullanarak Azure Etkinlik Hub'ları ile maksimum kullanılabilirlik ve tutarlılık miktarı nasıl sağlanabilir?
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
editor: ''
ms.assetid: 8f3637a1-bbd7-481e-be49-b3adf9510ba1
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2020
ms.author: shvija
ms.openlocfilehash: 0546adb6131479a8f5d2e7e31819483200586839
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397331"
---
# <a name="availability-and-consistency-in-event-hubs"></a>Event Hubs’da kullanılabilirlik ve tutarlılık

## <a name="overview"></a>Genel Bakış
Azure Olay Hub'ları, tek bir olay hub'ında kullanılabilirliği ve paralelleştirmeyi geliştirmek için bir [bölümleme modeli](event-hubs-scalability.md#partitions) kullanır. Örneğin, bir olay hub'ının dört bölümü varsa ve bu bölümlerden biri yük dengeleme işleminde bir sunucudan diğerine taşınırsa, diğer üç bölümden gönderip alabilirsiniz. Ayrıca, daha fazla bölüme sahip olmak, verilerinizi işleyen daha fazla eşzamanlı okuyucuya sahip olmanızı ve toplam iş lerinizi geliştirmenizi sağlar. Dağıtılmış bir sistemde bölümleme ve sıralamanın sonuçlarını anlamak çözüm tasarımının kritik bir yönüdür.

Sipariş verme ve kullanılabilirlik arasındaki değiş tokuşu açıklamaya yardımcı olmak için Brewer teoremi olarak da bilinen [CAP teoremi'ne](https://en.wikipedia.org/wiki/CAP_theorem)bakın. Bu teorem tutarlılık, kullanılabilirlik ve bölüm toleransı arasındaki seçimi açıklar. Bu ağ tarafından bölümlenmiş sistemler için her zaman tutarlılık ve kullanılabilirlik arasında bir denge olduğunu belirtir.

Brewer teoremi tutarlılığı ve kullanılabilirliği aşağıdaki gibi tanımlar:
* Bölüm toleransı: Bir veri işleme sisteminin bir bölüm hatası oluşsa bile verileri işlemeye devam etme yeteneği.
* Kullanılabilirlik: başarısız olmayan bir düğüm makul bir süre içinde makul bir yanıt verir (hiçbir hata veya zaman ayarı olmadan).
* Tutarlılık: bir okuma belirli bir istemci için en son yazmak dönmek için garanti edilir.

## <a name="partition-tolerance"></a>Bölüm toleransı
Olay Hub'ları, bölümlenmiş bir veri modelinin üzerine inşa edilmiştir. Kurulum sırasında olay hub'ınızdaki bölüm sayısını yapılandırabilirsiniz, ancak bu değeri daha sonra değiştiremezsiniz. Olay Hub'ları ile bölümleri kullanmanız gerektiğinden, uygulamanızın kullanılabilirliği ve tutarlılığı hakkında bir karar vermeniz gerekir.

## <a name="availability"></a>Kullanılabilirlik
Olay Hub'ları kullanmaya başlamanın en basit yolu varsayılan davranışı kullanmaktır. 

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure.Messaging.EventHubs (5.0.0 veya sonrası)](#tab/latest)
Yeni bir **[EventHubProducerClient](/dotnet/api/azure.messaging.eventhubs.producer.eventhubproducerclient?view=azure-dotnet)** nesnesi oluşturur ve **[SendAsync](/dotnet/api/azure.messaging.eventhubs.producer.eventhubproducerclient.sendasync?view=azure-dotnet)** yöntemini kullanırsanız, olaylarınız olay merkezinizdeki bölümler arasında otomatik olarak dağıtılır. Bu davranış, en fazla çalışma süresisağlar.

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft.Azure.EventHubs (4.1.0 veya önceki)](#tab/old)
Yeni bir **[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient)** nesnesi oluşturur ve **[Gönder](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync?view=azure-dotnet#Microsoft_Azure_EventHubs_EventHubClient_SendAsync_Microsoft_Azure_EventHubs_EventData_)** yöntemini kullanırsanız, olaylarınız olay hub'ınızdaki bölümler arasında otomatik olarak dağıtılır. Bu davranış, en fazla çalışma süresisağlar.

---

Maksimum çalışma süresi gerektiren kullanım örnekleri için bu model tercih edilir.

## <a name="consistency"></a>Tutarlılık
Bazı senaryolarda, olayların sıralanması önemli olabilir. Örneğin, arka uç sisteminizin bir silme komutundan önce bir güncelleştirme komutunu işlemesini isteyebilirsiniz. Bu durumda, bir olay üzerinde bölüm anahtarını ayarlayabilir `PartitionSender` veya olayları yalnızca belirli bir bölüme göndermek için bir nesne (eski Microsoft.Azure.Messaging kitaplığını kullanıyorsanız) kullanabilirsiniz. Bunu yapmak, bu olaylar bölümden okunduğunda, sırayla okunmasını sağlar. **Azure.Messaging.EventHubs** kitaplığını kullanıyorsanız ve daha fazla bilgi [için, olayları bir bölüme yayımlamak için PartitionSender'dan EventHubProducerClient'a kodu geçirme bölümüne](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md#migrating-code-from-partitionsender-to-eventhubproducerclient-for-publishing-events-to-a-partition)bakın.

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure.Messaging.EventHubs (5.0.0 veya sonrası)](#tab/latest)

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

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft.Azure.EventHubs (4.1.0 veya önceki)](#tab/old)

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

Bu yapılandırmayla, gönderdiğiniz belirli bölüm kullanılamıyorsa bir hata yanıtı alacağınızı unutmayın. Karşılaştırma noktası olarak, tek bir bölüme yakınlığınız yoksa, Olay Hub'ları hizmeti etkinliğinizi kullanılabilir bir sonraki bölüme gönderir.

Sipariş vermenin yanı sıra zamanı en üst düzeye çıkarmak için olası bir çözüm, olay işleme uygulamanızın bir parçası olarak olayları toplamaktır. Bunu başarmanın en kolay yolu, etkinliğinizi özel bir sıra numarası özelliğiyle damgalamaktır. Aşağıdaki kodda bir örnek gösterilir:

#### <a name="azuremessagingeventhubs-500-or-later"></a>[Azure.Messaging.EventHubs (5.0.0 veya sonrası)](#tab/latest)

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

#### <a name="microsoftazureeventhubs-410-or-earlier"></a>[Microsoft.Azure.EventHubs (4.1.0 veya önceki)](#tab/old)
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

Bu örnek, olay merkezinizdeki kullanılabilir bölümlerden birine gönderir ve uygulamanızdan karşılık gelen sıra numarasını ayarlar. Bu çözüm, durum işleme uygulaması tarafından tutulmasını gerektirir, ancak gönderenler kullanılabilir olması daha olasıdır bir bitiş noktası verir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olay Hub'ları hizmetine genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay hub’ı oluşturma](event-hubs-create.md)
