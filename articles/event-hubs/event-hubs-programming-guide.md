---
title: .NET programlama kılavuzu-Azure Event Hubs (eski) | Microsoft Docs
description: Bu makalede, Azure .NET SDK kullanarak Azure Event Hubs için kod yazma hakkında bilgi verilmektedir.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: de731d591c367e386fe8ef1eef03f1b90e0fa126
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85314550"
---
# <a name="net-programming-guide-for-azure-event-hubs-legacy-microsoftazureeventhubs-package"></a>Azure Event Hubs için .NET Programlama Kılavuzu (eski Microsoft. Azure. EventHubs paketi)
Bu makalede, Azure Event Hubs kullanarak kod yazma konusunda bazı yaygın senaryolar ele alınmaktadır. Burada Event Hubs’ın önceden bilindiği varsayılır. Event Hubs’a kavramsal genel bakış için bkz. [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md).

> [!WARNING]
> Bu kılavuz, eski **Microsoft. Azure. EventHubs** paketi içindir. En son [Azure. Messaging. EventHubs](get-started-dotnet-standard-send-v2.md) paketini kullanmak için kodunuzu [geçirmeniz](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md) önerilir.  


## <a name="event-publishers"></a>Olay yayımcıları

Olayları HTTP POST kullanarak veya AMQP 1,0 bağlantısı aracılığıyla bir olay hub 'ına gönderirsiniz. Hangi seçeneğin kullanılacağı ve ne zaman, değinilmesi gereken senaryoya bağlıdır. AMQP 1.0 bağlantıları Service Bus içinde aracılı bağlantılar olarak ölçülür ve sıklıkla daha yüksek ileti hacimlerine ve düşük gecikme gereksinimlerine sahip senaryolar kalıcı bir mesajlaşma kanalı sağladığından bu senaryolarda daha uygundur.

.NET ile yönetilen API’ler kullanılırken Event Hubs’a veri yayımlamaya yönelik birincil yapılar [EventHubClient][] ve [EventData][] sınıflarıdır. [Eventhubclient][] , olayların olay hub 'ına gönderildiği AMQP iletişim kanalını sağlar. [Eventdata][] sınıfı bir olayı temsil eder ve bir olay hub 'ına ileti yayımlamak için kullanılır. Bu sınıf, olayla ilgili olarak gövde, bazı meta veriler (Özellikler) ve başlık bilgilerini (SystemProperties) içerir. Diğer özellikler bir olay hub 'ından geçerken [eventdata][] nesnesine eklenir.

## <a name="get-started"></a>başlarken
Event Hubs destekleyen .NET sınıfları [Microsoft. Azure. EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) NuGet paketinde sunulmaktadır. Visual Studio Çözüm Gezgini 'ni veya Visual Studio 'da [Paket Yöneticisi konsolunu](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) kullanarak yükleyebilirsiniz. Bunu yapmak için [Paket Yöneticisi Konsolu](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) penceresinde aşağıdaki komutu yürütün:

```shell
Install-Package Microsoft.Azure.EventHubs
```

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Event Hubs oluşturmak için Azure portal, Azure PowerShell veya Azure CLı kullanabilirsiniz. Ayrıntılar için bkz. [Azure Portal kullanarak bir Event Hubs ad alanı ve bir olay hub 'ı oluşturma](event-hubs-create.md).

## <a name="create-an-event-hubs-client"></a>Event Hubs istemcisi oluşturma

Event Hubs etkileşimde bulunmak için birincil sınıf [Microsoft. Azure. EventHubs. eventhubclient][eventhubclient]' dır. Aşağıdaki örnekte gösterildiği gibi [Createfromconnectionstring](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createfromconnectionstring) yöntemini kullanarak bu sınıfın örneğini oluşturabilirsiniz:

```csharp
private const string EventHubConnectionString = "Event Hubs namespace connection string";
private const string EventHubName = "event hub name";

var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
{
    EntityPath = EventHubName

};
eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

## <a name="send-events-to-an-event-hub"></a>Olayları bir olay hub 'ına gönderme

Bir [Eventhubclient][] örneği oluşturarak ve [sendadsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync) yöntemi aracılığıyla zaman uyumsuz olarak göndererek olayları bir olay hub 'ına gönderirsiniz. Bu yöntem tek bir [eventdata][] örnek parametresini alır ve zaman uyumsuz olarak bir olay hub 'ına gönderir.

## <a name="event-serialization"></a>Olayı seri hale getirme

[Eventdata][] sınıfında, olay veri yükünü temsil eden çeşitli parametreleri, baytları veya bayt dizisini alan [iki aşırı yüklenmiş Oluşturucu](/dotnet/api/microsoft.azure.eventhubs.eventdata.-ctor) vardır. JSON’u [EventData][] ile kullanırken JSON ile kodlanmış bir dize için bayt dizisini almak üzere **Encoding.UTF8.GetBytes()** kullanabilirsiniz. Örneğin:

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
> Bölümler hakkında bilginiz yoksa, [Bu makaleye](event-hubs-features.md#partitions)bakın. 

Olay verilerini gönderirken, Bölüm ataması oluşturmak için karma hale getirilmiş bir değer belirtebilirsiniz. Bölüm, [Partitionsender. PartitionId](/dotnet/api/microsoft.azure.eventhubs.partitionsender.partitionid) özelliğini kullanarak belirtirsiniz. Ancak, bölümleri kullanma kararı kullanılabilirlik ve tutarlılık arasında seçim gösterir. 

### <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Bölüm anahtarı kullanmak isteğe bağlıdır ve bir tane kullanıp kullanmayacağınızı dikkatle düşünün. Bir olayı yayımlarken bölüm anahtarı belirtmezseniz hepsini bir kez deneme ataması kullanılır. Çoğu durumda, olay sıralaması önemliyse, Bölüm anahtarının kullanılması iyi bir seçimdir. Bir bölüm anahtarı kullandığınızda, bu bölümler tek bir düğümde kullanılabilirlik gerektirir ve zaman içinde kesintiler meydana gelebilir; Örneğin, işlem düğümleri yeniden başlatıldığında ve düzeltme ekiyle. Bu nedenle, bir bölüm KIMLIĞI ayarlarsanız ve bu bölüm bazı nedenlerle kullanılamaz hale gelirse, bu bölümdeki verilere erişme girişimi başarısız olur. Yüksek kullanılabilirlik en önemse, bölüm anahtarı belirtmeyin; Bu durumda olaylar, daha önce açıklanan hepsini bir kez deneme modeli kullanılarak bölümlere gönderilir. Bu senaryoda, kullanılabilirlik (bölüm KIMLIĞI yok) ve tutarlılık (olayları bir bölüm KIMLIĞINE sabitleme) arasında açık bir seçim yapabilirsiniz.

Diğer bir nokta, olayları işlerken gecikmelerin işlenmesine neden olur. Bazı durumlarda, verileri bırakıp işleme devam etmek denenmeye çalışmak daha iyi olabilir ve bu da daha fazla aşağı akış işleme gecikmesine neden olabilir. Örneğin, bir stok şeridi sayesinde, güncel verilerin tamamlanmasını beklemek daha iyidir, ancak canlı sohbet veya VOıP senaryosunda, tamamlanmamış olsa bile verilere hızlıca sahip olmanız önerilir.

Bu kullanılabilirlik konuları verildiğinde, bu senaryolarda aşağıdaki hata işleme stratejilerinden birini seçebilirsiniz:

- Durdur (işlemler düzeltilene kadar Event Hubs okumayı Durdur)
- Bırak (iletiler önemli değildir, bunları bırakın)
- Yeniden dene (iletileri uygun gördüğünüz şekilde yeniden deneyin)

Kullanılabilirlik ve tutarlılık arasındaki denge hakkında daha fazla bilgi ve bir tartışma için bkz. [Event Hubs kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md). 

## <a name="batch-event-send-operations"></a>Toplu olay gönderme işlemleri

Olayları toplu halde göndermek, aktarım hızını artırmaya yardımcı olabilir. Bir [Sendadsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync) çağrısı için veri nesnelerinin daha sonra eklenebileceği bir toplu iş oluşturmak Için [CreateBatch](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createbatch) API 'sini kullanabilirsiniz.

Tek bir toplu işlem, bir olayın 1 MB sınırını aşmamalıdır. Ayrıca, toplu işteki her bir ileti aynı yayımcı kimliğini kullanır. Toplu işin en büyük olay boyutu aşmamasını sağlamak gönderenin sorumluluğundadır. Aşması durumunda bir istemci **Gönderme** hatası oluşturulur. Batch 'in 1 MB 'ı aşmadığından emin olmak için [Eventhubclient. CreateBatch](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createbatch) yardımcı yöntemini kullanabilirsiniz. [CreateBatch](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createbatch) API 'sinden boş bir [eventdatabatch](/dotnet/api/microsoft.azure.eventhubs.eventdatabatch) alırsınız ve sonra toplu işi oluşturmak Için olayları eklemek üzere [TryAdd](/dotnet/api/microsoft.azure.eventhubs.eventdatabatch.tryadd) 'yi kullanın. 

## <a name="send-asynchronously-and-send-at-scale"></a>Zaman uyumsuz olarak gönderme ve ölçekli gönderme

Olayları bir olay hub 'ına zaman uyumsuz olarak gönderirsiniz. Zaman uyumsuz olarak gönderilmesi, bir istemcinin olayları gönderme hızını arttırır. [Sendadsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync) bir [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) nesnesi döndürüyor. İstemci yeniden deneme seçeneklerini denetlemek için istemci üzerindeki [Retrypolicy](/dotnet/api/microsoft.servicebus.retrypolicy) sınıfını kullanabilirsiniz.

## <a name="event-consumers"></a>Olay tüketicileri
[EventProcessorHost][] sınıfı Event Hubs verilerini işler. .NET platformu üzerinde olay okuyucuları oluştururken bu uygulamayı kullanmanız gerekir. [EventProcessorHost][] aynı zamanda denetim noktası oluşturma ve bölüm kiralama yönetimi sağlayan olay işlemcisi uygulamaları için iş parçacığı güvenli, çok işlemli, güvenli bir çalışma zamanı ortamı sağlar.

[EventProcessorHost][] sınıfını kullanmak için [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulayabilirsiniz. Bu arabirim dört yöntem içerir:

* [OpenAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.openasync)
* [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.closeasync)
* [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync)
* [ProcessErrorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processerrorasync)

Olay işlemeyi başlatmak için, Olay Hub 'ınız için uygun parametreleri sağlayan [Eventprocessorhost][]örneğini oluşturun. Örneğin:

> [!NOTE]
> EventProcessorHost ve ilgili sınıfları **Microsoft. Azure. EventHubs. Processor** paketinde sunulmaktadır. [Bu makaledeki](event-hubs-dotnet-framework-getstarted-send.md#add-the-event-hubs-nuget-package) yönergeleri Izleyerek veya [Paket Yöneticisi konsolu](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) penceresinde aşağıdaki komutu vererek, paketi Visual Studio projenize ekleyin: `Install-Package Microsoft.Azure.EventHubs.Processor` .

```csharp
var eventProcessorHost = new EventProcessorHost(
        EventHubName,
        PartitionReceiver.DefaultConsumerGroupName,
        EventHubConnectionString,
        StorageConnectionString,
        StorageContainerName);
```

Ardından, [ıeventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulamanızı çalışma zamanına kaydetmek Için [Registereventprocessorasync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync) çağrısı yapın:

```csharp
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();
```

Bu noktada, ana bilgisayar bir "Greedy" algoritması kullanarak Olay Hub 'ındaki her bölümde bir kira edinmeye çalışır. Bu kiralamalar belirli bir zaman çerçevesi için sonuncu ve sonra yenilenmelidir. Bu örnekte çalışan örnekleri olan yeni düğümler çevrimiçi oldukça kiralama ayırmaları yapar ve zaman içerisinde yük daha fazla kira elde etmeye çalıştığından düğümler arasında kayar.

![Olay İşleyicisi Konağı](./media/event-hubs-programming-guide/IC759863.png)

Zaman içerisinde bir denge sağlanır. Bu dinamik özellik hem ölçek artırma hem de ölçek azaltma için tüketicilere CPU tabanlı otomatik ölçeklendirmenin uygulanmasını sağlar. Event Hubs doğrudan bir ileti sayıları kavramı olmadığından, ortalama CPU kullanımı genellikle arka ucu veya tüketici ölçeğini ölçmek için en iyi mekanizmadır. Yayımcılar tüketicilerin işleyebileceğinden daha fazla olay yayımlamaya başlarsa, tüketiciler üzerindeki CPU artışı çalışan örnek sayısı üzerinde otomatik ölçeklendirmeye neden olmak için kullanılabilir.

[EventProcessorHost][] sınıfı ayrıca bir Azure depolama tabanlı denetim noktası oluşturma mekanizması kullanır. Bu mekanizma uzaklığı bölüm başına temelinde depolar, böylece her tüketici önceki tüketicinin son denetim noktasının ne olduğunu belirleyebilir. Bölümler kiralamalar aracılığıyla düğümler arasında geçiş yaptığında yük kaymasını kolaylaştıran eşitleme mekanizması budur.

## <a name="publisher-revocation"></a>Yayımcı iptali

Olay Işleyicisi ana bilgisayarının gelişmiş çalışma zamanı özelliklerine ek olarak Event Hubs hizmeti, belirli yayımcıların Olay Hub 'ına olay göndermesini engellemek için [Yayımcı iptalinin](/rest/api/eventhub/revoke-publisher) yapılmasını sağlar. Bu özellikler, bir yayımcı belirtecinin tehlikeye düşmesi durumunda veya bir yazılım güncelleştirmesinin uygun şekilde davranmasına neden olduğu durumlarda faydalıdır. Bu durumlarda SAS belirtecinin bir parçası olan yayımcı kimliğinin olayları yayımlaması engellenebilir.

> [!NOTE]
> Şu anda, bu özelliği yalnızca REST API destekler ([Yayımcı iptali](/rest/api/eventhub/revoke-publisher)).

Yayımcı iptali ve yayımcı olarak Event Hubs’a gönderme hakkında daha fazla bilgi için [Event Hubs Büyük Ölçekli Güvenli Yayımlama](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) örneğine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs senaryoları hakkında daha fazla bilgi almak için aşağıdaki bağlantıları ziyaret edin:

* [Event Hubs API’sine genel bakış](event-hubs-api-overview.md)
* [Event Hubs nedir?](event-hubs-what-is-event-hubs.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Olay işlemcisi konak API başvurusu](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)

[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[EventHubClient]: /dotnet/api/microsoft.azure.eventhubs.eventhubclient
[EventData]: /dotnet/api/microsoft.azure.eventhubs.eventdata
[CreateEventHubIfNotExists]: /dotnet/api/microsoft.servicebus.namespacemanager.createeventhubifnotexists
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.eventdata#Microsoft_ServiceBus_Messaging_EventData_PartitionKey
[EventProcessorHost]: /dotnet/api/microsoft.azure.eventhubs.processor
