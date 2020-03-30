---
title: .NET Programlama kılavuzu - Azure Etkinlik Hub'ları (eski) | Microsoft Dokümanlar
description: Bu makalede, Azure .NET SDK'yı kullanarak Azure Etkinlik Hub'ları için kod yazma hakkında bilgi verilmektedir.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
ms.service: event-hubs
ms.custom: seodec18
ms.topic: article
ms.date: 01/15/2020
ms.author: shvija
ms.openlocfilehash: d958c2d32c16874676f46bb216067fe2d7bbe784
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280982"
---
# <a name="net-programming-guide-for-azure-event-hubs-legacy-microsoftazureeventhubs-package"></a>.Azure Etkinlik Hub'ları için .NET Programlama kılavuzu (eski Microsoft.Azure.EventHubs paketi)
Bu makalede, Azure Olay Hub'larını kullanarak kod yazarken karşılaşılan bazı yaygın senaryolar açıklanmaktadır. Burada Event Hubs’ın önceden bilindiği varsayılır. Event Hubs’a kavramsal genel bakış için bkz. [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md).

> [!WARNING]
> Bu kılavuz, eski **Microsoft.Azure.EventHubs** paketi içindir. En son [Azure.Messaging.EventHubs](get-started-dotnet-standard-send-v2.md) paketini kullanmak için kodunuzu [geçirmenizi](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md) öneririz.  


## <a name="event-publishers"></a>Olay yayımcıları

Olayları HTTP POST'u kullanarak veya AMQP 1.0 bağlantısı üzerinden bir etkinlik merkezine gönderirsiniz. Hangi seçeneğin kullanılacağı ve ne zaman ele alınabilen belirli senaryoya bağlıdır. AMQP 1.0 bağlantıları Service Bus içinde aracılı bağlantılar olarak ölçülür ve sıklıkla daha yüksek ileti hacimlerine ve düşük gecikme gereksinimlerine sahip senaryolar kalıcı bir mesajlaşma kanalı sağladığından bu senaryolarda daha uygundur.

.NET ile yönetilen API’ler kullanılırken Event Hubs’a veri yayımlamaya yönelik birincil yapılar [EventHubClient][] ve [EventData][] sınıflarıdır. [EventHubClient,][] olayların olay merkezine gönderildiği AMQP iletişim kanalını sağlar. [EventData][] sınıfı bir olayı temsil eder ve iletileri bir olay hub'ına yayımlamak için kullanılır. Bu sınıf, olay la ilgili gövdeyi, bazı meta verileri (Özellikler) ve üstbilgi bilgilerini (SystemProperties) içerir. Olay hub'ından geçerken [EventData][] nesnesine diğer özellikler eklenir.

## <a name="get-started"></a>Kullanmaya başlayın
Olay Hub'larını destekleyen .NET sınıfları [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) NuGet paketinde sağlanır. Visual Studio Solution explorer'ı veya Visual Studio'daki [Paket Yöneticisi Konsolu'nu](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) kullanarak yükleyebilirsiniz. Bunu yapmak için [Paket Yöneticisi Konsolu](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) penceresinde aşağıdaki komutu yürütün:

```shell
Install-Package Microsoft.Azure.EventHubs
```

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Etkinlik Hub'ları oluşturmak için Azure portalını, Azure PowerShell'i veya Azure CLI'yi kullanabilirsiniz. Ayrıntılar için, [Azure portalını kullanarak bir Olay Hub'ları ad alanı ve etkinlik merkezi oluşturun'a](event-hubs-create.md)bakın.

## <a name="create-an-event-hubs-client"></a>Event Hubs istemcisi oluşturma

Olay Hub'ları ile etkileşim kurmak için birincil sınıf [Microsoft.Azure.EventHubs.EventHubClient][EventHubClient]olduğunu. Aşağıdaki örnekte gösterildiği gibi [CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createfromconnectionstring) yöntemini kullanarak bu sınıfı anında oluşturabilirsiniz:

```csharp
private const string EventHubConnectionString = "Event Hubs namespace connection string";
private const string EventHubName = "event hub name";

var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
{
    EntityPath = EventHubName

};
eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

## <a name="send-events-to-an-event-hub"></a>Olayları bir etkinlik merkezine gönderme

[Bir EventHubClient][] örneği oluşturarak ve [SendAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync) yöntemi yle eşzamanlı olarak göndererek olayları bir olay hub'ına gönderirsiniz. Bu yöntem tek bir [EventData][] örnek parametresini alır ve eşzamanlı olarak bir olay hub'ına gönderir.

## <a name="event-serialization"></a>Olayı seri hale getirme

[EventData][] sınıfı, olay veri yükünü temsil eden çeşitli parametreleri, baytları veya bayt dizisini alan [iki aşırı yüklü oluşturucuya](/dotnet/api/microsoft.azure.eventhubs.eventdata.-ctor) sahiptir. JSON’u [EventData][] ile kullanırken JSON ile kodlanmış bir dize için bayt dizisini almak üzere **Encoding.UTF8.GetBytes()** kullanabilirsiniz. Örnek:

```csharp
for (var i = 0; i < numMessagesToSend; i++)
{
    var message = $"Message {i}";
    Console.WriteLine($"Sending message: {message}");
    await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
}
```

## <a name="partition-key"></a>Bölüm anahtarı

> [!NOTE]
> Bölümlere aşina değilseniz, bu [makaleye](event-hubs-features.md#partitions)bakın. 

Olay verileri gönderirken, bir bölüm ataması oluşturmak için hashed bir değer belirtebilirsiniz. [PartitionSender.PartitionID](/dotnet/api/microsoft.azure.eventhubs.partitionsender.partitionid) özelliğini kullanarak bölümü belirtirsiniz. Ancak, bölümleri kullanma kararı kullanılabilirlik ve tutarlılık arasında bir seçim anlamına gelir. 

### <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Bölüm anahtarı kullanmak isteğe bağlıdır ve bir tane kullanıp kullanmayacağını dikkatlice düşünmelisiniz. Bir olayı yayımlarken bölüm anahtarı belirtmezseniz hepsini bir kez deneme ataması kullanılır. Çoğu durumda, olay sıralama önemliyse, bölüm anahtarı kullanmak iyi bir seçimdir. Bir bölüm anahtarı kullandığınızda, bu bölümler tek bir düğümüzerinde kullanılabilirlik gerektirir ve kesintiler zaman içinde oluşabilir; örneğin, işlem düğümleri yeniden başlatıldığında ve yama. Bu nedenle, bir bölüm kimliği ayarlarsanız ve bu bölüm herhangi bir nedenle kullanılamaz hale gelirse, bu bölümdeki verilere erişme girişimi başarısız olur. Yüksek kullanılabilirlik en önemliyse, bir bölüm anahtarı belirtmeyin; bu durumda olaylar daha önce açıklanan round-robin modeli kullanılarak bölümlere gönderilir. Bu senaryoda, kullanılabilirlik (bölüm kimliği yok) ve tutarlılık (olayları bir bölüm kimliğine sabitleme) arasında açık bir seçim yapıyorsunuz.

Bir diğer husus da, olayların işlenmesindeki gecikmeleri işlemektir. Bazı durumlarda, veri bırakmak ve yeniden denemek, işleme devam etmeye çalışmadan daha iyi olabilir, bu da daha fazla akış aşağı işlem gecikmesine neden olabilir. Örneğin, bir stok işaretçisi ile tam güncel verileri beklemek daha iyidir, ancak canlı bir sohbet veya VOIP senaryosunda, tam olmasa bile verileri hızlı bir şekilde elde etmeyi tercih edeyim.

Bu kullanılabilirlik hususları göz önüne alındığında, bu senaryolarda aşağıdaki hata işleme stratejilerinden birini seçebilirsiniz:

- Dur (Olaylar Hub'larından okumayı dur, her şey düzeltilene kadar)
- Bırak (iletiler önemli değildir, bırakın)
- Yeniden deneyin (iletileri uygun gördüğünüz şekilde yeniden deneyin)

Daha fazla bilgi ve kullanılabilirlik ve tutarlılık arasındaki dengeler hakkında bir tartışma için, [Olay Hub'larında Kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)konusuna bakın. 

## <a name="batch-event-send-operations"></a>Toplu olay gönderme işlemleri

Olayları toplu olarak göndermek, iş bilginin artmasına yardımcı olabilir. [CreateBatch](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createbatch) API'sını, daha sonra [SendAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync) çağrısı için veri nesnelerinin eklenebileceği bir toplu iş partisi oluşturmak için kullanabilirsiniz.

Tek bir toplu iş, bir olayın 1 MB sınırını aşmamalıdır. Ayrıca, toplu işteki her bir ileti aynı yayımcı kimliğini kullanır. Toplu işin en büyük olay boyutu aşmamasını sağlamak gönderenin sorumluluğundadır. Aşması durumunda bir istemci **Gönderme** hatası oluşturulur. Toplu iş 1 MB'ı aşmadığından emin olmak için yardımcı yöntem [EventHubClient.CreateBatch'ı](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createbatch) kullanabilirsiniz. [CreateBatch](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createbatch) API'den boş bir [EventDataBatch](/dotnet/api/microsoft.azure.eventhubs.eventdatabatch) alırsınız ve toplu iş yapmak için olay eklemek için [TryAdd'i](/dotnet/api/microsoft.azure.eventhubs.eventdatabatch.tryadd) kullanırsınız. 

## <a name="send-asynchronously-and-send-at-scale"></a>Zaman uyumsuz olarak gönderme ve ölçekli gönderme

Olayları eşit bir şekilde bir olay merkezine gönderirsiniz. Eşsenkronize gönderme, istemcinin olayları gönderme hızını artırır. [SendAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync) bir [Görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) nesnesini döndürür. İstemci yeniden deneme seçeneklerini denetlemek için istemcideki [RetryPolicy](/dotnet/api/microsoft.servicebus.retrypolicy) sınıfını kullanabilirsiniz.

## <a name="event-consumers"></a>Olay tüketicileri
[EventProcessorHost][] sınıfı Event Hubs verilerini işler. .NET platformu üzerinde olay okuyucuları oluştururken bu uygulamayı kullanmanız gerekir. [EventProcessorHost][] aynı zamanda denetim noktası oluşturma ve bölüm kiralama yönetimi sağlayan olay işlemcisi uygulamaları için iş parçacığı güvenli, çok işlemli, güvenli bir çalışma zamanı ortamı sağlar.

[EventProcessorHost][] sınıfını kullanmak için [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulayabilirsiniz. Bu arabirim dört yöntem içerir:

* [OpenAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.openasync)
* [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.closeasync)
* [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync)
* [ProcessErrorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processerrorasync)

Olay işlemeyi başlatmak için [EventProcessorHost'u][]anlık olarak başlatın ve olay göbeğiniz için uygun parametreleri sağlayın. Örnek:

> [!NOTE]
> EventProcessorHost ve ilgili sınıfları **Microsoft.Azure.EventHubs.Processor** paketinde sağlanır. [Bu makaledeki](event-hubs-dotnet-framework-getstarted-send.md#add-the-event-hubs-nuget-package) talimatları izleyerek veya [Paket Yöneticisi Konsol](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) penceresinde aşağıdaki komutu vererek paketi`Install-Package Microsoft.Azure.EventHubs.Processor`Visual Studio projenize ekleyin: .

```csharp
var eventProcessorHost = new EventProcessorHost(
        EventHubName,
        PartitionReceiver.DefaultConsumerGroupName,
        EventHubConnectionString,
        StorageConnectionString,
        StorageContainerName);
```

Ardından, [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulamanızı çalışma süresine kaydetmek için [RegisterEventProcessorAsync'i](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync) arayın:

```csharp
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();
```

Bu noktada, ana bilgisayar "açgözlü" algoritması kullanarak olay merkezindeki her bölüm için bir kira elde etmeye çalışır. Bu kiralamalar belirli bir zaman dilimi için son ve daha sonra yenilenmesi gerekir. Bu örnekte çalışan örnekleri olan yeni düğümler çevrimiçi oldukça kiralama ayırmaları yapar ve zaman içerisinde yük daha fazla kira elde etmeye çalıştığından düğümler arasında kayar.

![Olay İşleyicisi Konağı](./media/event-hubs-programming-guide/IC759863.png)

Zaman içerisinde bir denge sağlanır. Bu dinamik özellik hem ölçek artırma hem de ölçek azaltma için tüketicilere CPU tabanlı otomatik ölçeklendirmenin uygulanmasını sağlar. Olay Hub'larının doğrudan ileti sayıları kavramı olmadığından, ortalama CPU kullanımı genellikle arka uç veya tüketici ölçeğini ölçmek için en iyi mekanizmadır. Yayımcılar tüketicilerin işleyebileceğinden daha fazla olay yayımlamaya başlarsa, tüketiciler üzerindeki CPU artışı çalışan örnek sayısı üzerinde otomatik ölçeklendirmeye neden olmak için kullanılabilir.

[EventProcessorHost][] sınıfı ayrıca bir Azure depolama tabanlı denetim noktası oluşturma mekanizması kullanır. Bu mekanizma uzaklığı bölüm başına temelinde depolar, böylece her tüketici önceki tüketicinin son denetim noktasının ne olduğunu belirleyebilir. Bölümler kiralamalar aracılığıyla düğümler arasında geçiş yaptığında yük kaymasını kolaylaştıran eşitleme mekanizması budur.

## <a name="publisher-revocation"></a>Yayımcı iptali

Olay Hub'ları hizmeti, Olay İşlemcisi Host'un gelişmiş çalışma zamanı özelliklerine ek olarak, belirli yayıncıların olay hub'ına olay göndermesini engellemek için [yayımcının iptalini](/rest/api/eventhub/revoke-publisher) sağlar. Bu özellikler, yayımcı belirteci tehlikeye girdiyse veya bir yazılım güncelleştirmesi uygunsuz şekilde tavır vermelerine neden oluyorsa yararlıdır. Bu durumlarda SAS belirtecinin bir parçası olan yayımcı kimliğinin olayları yayımlaması engellenebilir.

> [!NOTE]
> Şu anda, yalnızca REST API bu özelliği destekler[(yayımcı iptal).](/rest/api/eventhub/revoke-publisher)

Yayımcı iptali ve yayımcı olarak Event Hubs’a gönderme hakkında daha fazla bilgi için [Event Hubs Büyük Ölçekli Güvenli Yayımlama](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) örneğine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs senaryoları hakkında daha fazla bilgi almak için aşağıdaki bağlantıları ziyaret edin:

* [Event Hubs API’sine genel bakış](event-hubs-api-overview.md)
* [Etkinlik Hub'ları Nedir](event-hubs-what-is-event-hubs.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Olay işlemci ana bilgisayar API başvurusu](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)

[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[EventHubClient]: /dotnet/api/microsoft.azure.eventhubs.eventhubclient
[Eventdata]: /dotnet/api/microsoft.azure.eventhubs.eventdata
[CreateEventHubIfNotExists]: /dotnet/api/microsoft.servicebus.namespacemanager.createeventhubifnotexists
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.eventdata#Microsoft_ServiceBus_Messaging_EventData_PartitionKey
[EventProcessorHost]: /dotnet/api/microsoft.azure.eventhubs.processor
