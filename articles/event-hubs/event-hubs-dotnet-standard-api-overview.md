---
title: Azure Etkinlik Hub'larına genel bakış .NET Standart API'ler | Microsoft Dokümanlar
description: .NET Standart API genel bakış
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 08/13/2018
ms.author: shvija
ms.openlocfilehash: b09f39f45936a7c43dbc1ef109780315d62c768f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60821900"
---
# <a name="event-hubs-net-standard-api-overview"></a>Olay Hub'ları .NET Standart API'ye genel bakış

Bu makalede, bazı önemli Azure Olay Hub'ları [.NET Standart istemci API'leri](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/)özetlenir. Şu anda Olay Hub'ları için iki .NET Standart istemci kitaplık vardır:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs): Tüm temel çalışma zamanı işlemlerini sağlar.
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor): İşlenen olayları izlemenizi sağlayan ek işlevler ekler ve bir olay merkezinden okunmanın en kolay yoludur.

## <a name="event-hubs-client"></a>Olay Hub'ları istemcisi

[EventHubClient,](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) olayları göndermek, alıcıoluşturmak ve çalışma zamanı bilgileri almak için kullandığınız birincil nesnedir. Bu istemci belirli bir olay hub'ına bağlıdır ve Olay Hub'ları bitiş noktasına yeni bir bağlantı oluşturur.

### <a name="create-an-event-hubs-client"></a>Event Hubs istemcisi oluşturma

Bir [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) nesnesi bağlantı dizesinden oluşturulur. Yeni bir istemciyi anında açmanın en basit yolu aşağıdaki örnekte gösterilmiştir:

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("Event Hubs connection string");
```

Bağlantı dizesini programlı bir şekilde yeniden düzenledik, [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) sınıfını kullanabilir ve bağlantı dizesini [EventHubClient.CreateFromConnectionString'e](/dotnet/api/microsoft.azure.eventhubs.eventhubclient)parametre olarak geçirebilirsiniz.

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("Event Hubs connection string")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a>Olayları gönderme

Olayları bir olay merkezine göndermek için [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) sınıfını kullanın. Gövde bir `byte` dizi veya bir `byte` dizi segmentolmalıdır.

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a>Olayları alma

Olay Hub'larından olay almanın önerilen yolu, olay hub'ı ofset ve bölüm bilgilerini otomatik olarak izlemek için işlevsellik sağlayan [Olay İşlemcisi Ana Bilgisayarını](#event-processor-host-apis)kullanmaktır. Ancak, olayları almak için temel Olay Hub'ları kitaplığı esnekliğini kullanmak isteyebileceğin bazı durumlar vardır.

#### <a name="create-a-receiver"></a>Alıcı oluşturma

Alıcılar belirli bölümlere bağlıdır, bu nedenle bir olay merkezindeki tüm olayları almak için birden çok örnek oluşturmanız gerekir. Bölüm dislerini kodlamak yerine, bölüm bilgilerini programlamak üzere almak iyi bir uygulamadır. Bunu yapmak için [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) yöntemini kullanabilirsiniz.

```csharp
// Create a list to keep track of the receivers
var receivers = new List<PartitionReceiver>();
// Use the eventHubClient created above to get the runtime information
var runTimeInformation = await eventHubClient.GetRuntimeInformationAsync();
// Loop over the resulting partition IDs
foreach (var partitionId in runTimeInformation.PartitionIds)
{
    // Create the receiver
    var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);
    // Add the receiver to the list
    receivers.Add(receiver);
}
```

Olaylar hiçbir zaman bir olay hub'ından kaldırılmadığından (ve yalnızca süresi dolduğundan) uygun başlangıç noktasını belirtmeniz gerekir. Aşağıdaki örnek olası kombinasyonları gösterir:

```csharp
// partitionId is assumed to come from GetRuntimeInformationAsync()

// Using the constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a>Bir olayı tüketin

```csharp
// Receive a maximum of 100 messages in this call to ReceiveAsync
var ehEvents = await receiver.ReceiveAsync(100);
// ReceiveAsync can return null if there are no messages
if (ehEvents != null)
{
    // Since ReceiveAsync can return more than a single event you will need a loop to process
    foreach (var ehEvent in ehEvents)
    {
        // Decode the byte array segment
        var message = UnicodeEncoding.UTF8.GetString(ehEvent.Body.Array);
        // Load the custom property that we set in the send example
        var customType = ehEvent.Properties["Type"];
        // Implement processing logic here
    }
}       
```

## <a name="event-processor-host-apis"></a>Olay İşlemcisi Ana Bilgisayar API'leri

Bu API'ler, kullanılabilir çalışanlar arasında bölümleri dağıtarak kullanılamayan işçi işlemlerine esneklik sağlar:

```csharp
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.

// Read these connection strings from a secure location
var ehConnectionString = "{Event Hubs connection string}";
var ehEntityPath = "{event hub path/name}";
var storageConnectionString = "{Storage connection string}";
var storageContainerName = "{Storage account container name}";

var eventProcessorHost = new EventProcessorHost(
    ehEntityPath,
    PartitionReceiver.DefaultConsumerGroupName,
    ehConnectionString,
    storageConnectionString,
    storageContainerName);

// Start/register an EventProcessorHost
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

// Disposes the Event Processor Host
await eventProcessorHost.UnregisterEventProcessorAsync();
```

Aşağıdaki [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) arabiriminin bir örnek uygulamasıdır:

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    public Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
        return Task.CompletedTask;
    }

    public Task OpenAsync(PartitionContext context)
    {
        Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
        return Task.CompletedTask;
    }

    public Task ProcessErrorAsync(PartitionContext context, Exception error)
    {
        Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
        return Task.CompletedTask;
    }

    public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (var eventData in messages)
        {
            var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
            Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
        }

        return context.CheckpointAsync();
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs senaryoları hakkında daha fazla bilgi almak için aşağıdaki bağlantıları ziyaret edin:

* [Azure Event Hubs nedir?](event-hubs-what-is-event-hubs.md)
* [Kullanılabilir Olay Hub'ları apis](event-hubs-api-overview.md)

.NET API başvuruları burada:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)
