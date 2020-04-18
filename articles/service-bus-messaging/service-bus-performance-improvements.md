---
title: Azure Hizmet Veri Servisi'ni kullanarak performansı artırmak için en iyi uygulamalar
description: Aracılı iletialışverişi yaparken performansı en iyi duruma getirmek için Hizmet Veri Servisi'nin nasıl kullanılacağını açıklar.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 03/12/2020
ms.author: aschhab
ms.openlocfilehash: 267965ee41280a677050d1676285dda8734bc044
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606051"
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Service Bus Mesajlaşması kullanarak performans geliştirmek için en iyi yöntemler

Bu makalede, aracılı iletialışverişi yaparken performansı en iyi duruma getirmek için Azure Hizmet Veri Servisi'nin nasıl kullanılacağı açıklanmaktadır. Bu makalenin ilk bölümünde, performansı artırmaya yardımcı olmak için sunulan farklı mekanizmalar açıklanmaktadır. İkinci bölüm, Servis Veri Yolu'nun belirli bir senaryoda en iyi performansı sunabilecek şekilde nasıl kullanılacağı konusunda rehberlik sağlar.

Bu makale boyunca, "istemci" terimi Hizmet Veri Tos'una erişen herhangi bir varlığı ifade eder. İstemci gönderen veya alıcı rolünü alabilir. "Gönderen" terimi, Hizmet Veri Servisi sırasına veya konu aboneliğine ileti gönderen bir Hizmet Veri Servisi sırası veya konu istemcisi için kullanılır. "Alıcı" terimi, Servis Veri Servisi kuyruğundan veya abonelikten ileti alan Bir Servis Veri Servisi Sırası veya abonelik istemcisi anlamına gelir.

Bu bölümler, Service Bus'Un performansı artırmaya yardımcı olmak için kullandığı çeşitli kavramları tanıtır.

## <a name="protocols"></a>Protokoller

Service Bus, istemcilerin üç protokolden biri aracılığıyla ileti gönderip almalarını sağlar:

1. Gelişmiş İleti Sıraya Alma Protokolü (AMQP)
2. Servis Veri Servisi Mesajlaşma Protokolü (SBMP)
3. Köprü Metni Aktarım Protokolü (HTTP)

Hizmet Veri Servisi bağlantısını koruduğu ndan, AMQP en verimli yöntemdir. Ayrıca toplu işleme ve ön getirme uygular. Açıkça belirtilmediği sürece, bu makaledeki tüm içerik AMQP veya SBMP kullanımını varsayar.

> [!IMPORTANT]
> SBMP yalnızca .NET Framework için kullanılabilir. AMQP ,NET Standard için varsayılan değerdir.

## <a name="choosing-the-appropriate-service-bus-net-sdk"></a>Uygun Servis Veri Mes 'u .NET SDK'nın seçilmesi

Desteklenen iki Azure Hizmet Veri Kurumu .NET SDK'sı vardır. Api'leri çok benzerdir ve hangisini seçeceğinizi kafa karıştırıcı olabilir. Kararınızı yönlendirmenize yardımcı olmak için aşağıdaki tabloya bakın. Microsoft.Azure.ServiceBus SDK'yı daha modern, performanslı ve platformlar arası uyumlu olduğundan öneririz. Ayrıca, WebSockets üzerinden AMQP'yi destekler ve açık kaynak projeleri Azure .NET SDK koleksiyonunun bir parçasıdır.

| NuGet Paketi | Birincil Ad Alanı(lar) | Minimum Platform(lar) | Protokoller |
|---------------|----------------------|---------------------|-------------|
| <a href="https://www.nuget.org/packages/Microsoft.Azure.ServiceBus" target="_blank">Microsoft.Azure.ServiceBus<span class="docon docon-navigate-external x-hidden-focus"></span></a> | `Microsoft.Azure.ServiceBus`<br>`Microsoft.Azure.ServiceBus.Management` | .NET Core 2.0<br>.NET Framework 4.6.1<br>Mono 5.4<br>Xamarin.iOS 10.14<br>Xamarin.Mac 3.8<br>Xamarin.Android 8.0<br>Evrensel Windows Platformu 10.0.16299 | AMQP<br>HTTP |
| <a href="https://www.nuget.org/packages/WindowsAzure.ServiceBus" target="_blank">WindowsAzure.ServiceBus<span class="docon docon-navigate-external x-hidden-focus"></span></a> | `Microsoft.ServiceBus`<br>`Microsoft.ServiceBus.Messaging` | .NET Framework 4.6.1 | AMQP<br>SBMP<br>HTTP |

Minimum .NET Standart platform desteği hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support)

## <a name="reusing-factories-and-clients"></a>Fabrikaları ve müşterileri yeniden kullanma

# <a name="microsoftazureservicebus-sdk"></a>[Microsoft.Azure.ServiceBus SDK](#tab/net-standard-sdk)

Hizmet Veri Servisi istemci nesneleri, uygulamaları [`IQueueClient`][QueueClient] [`IMessageSender`][MessageSender]gibi veya , bağımlılık enjeksiyonu için singletons olarak kaydedilmelidir (veya bir kez anlık ve paylaşılan). İleti gönderdikten sonra ileti fabrikalarını veya sıra, konu ve abonelik istemcilerini kapatmamanız ve bir sonraki iletiyi gönderdiğinde bunları yeniden oluşturmamanız önerilir. İleti fabrikasının kapatılması Servis Veri Servisi'ne olan bağlantıyı siler ve fabrikayı yeniden oluştururken yeni bir bağlantı kurulur. Bağlantı kurmak, aynı fabrika ve istemci nesnelerini birden çok işlem için yeniden kullanarak önkullanabileceğiniz pahalı bir işlemdir. Bu istemci nesnelerini eşzamanlı eşzamanlı işlemler için ve birden çok iş parçacığı için güvenle kullanabilirsiniz.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure.ServiceBus SDK](#tab/net-framework-sdk)

Service Bus istemci nesneleri `QueueClient` gibi `MessageSender`veya , aynı zamanda bağlantıların iç yönetimini sağlayan bir [MessagingFactory][MessagingFactory] nesnesi aracılığıyla oluşturulur. İleti gönderdikten sonra ileti fabrikalarını veya sıra, konu ve abonelik istemcilerini kapatmamanız ve bir sonraki iletiyi gönderdiğinde bunları yeniden oluşturmamanız önerilir. İleti fabrikasının kapatılması Servis Veri Servisi'ne olan bağlantıyı siler ve fabrikayı yeniden oluştururken yeni bir bağlantı kurulur. Bağlantı kurmak, aynı fabrika ve istemci nesnelerini birden çok işlem için yeniden kullanarak önkullanabileceğiniz pahalı bir işlemdir. Bu istemci nesnelerini eşzamanlı eşzamanlı işlemler için ve birden çok iş parçacığı için güvenle kullanabilirsiniz.

---

## <a name="concurrent-operations"></a>Eşzamanlı işlemler

Bir işlemi gerçekleştirmek (gönderme, alma, silme, vb.) biraz zaman alır. Bu süre, isteğin ve yanıtın gecikmesine ek olarak Servis Veri Servisi tarafından işlemin işlenmesini içerir. Zaman başına işlem sayısını artırmak için, işlemlerin aynı anda yürütülmesi gerekir.

İstemci eşzamanlı işlemleri eşzamanlı işlemler gerçekleştirerek zamanlar. Sonraki istek önceki istek tamamlanmadan önce başlatılır. Aşağıdaki kod snippet bir eşzamanlı gönderme işlemi örneğidir:

# <a name="microsoftazureservicebus-sdk"></a>[Microsoft.Azure.ServiceBus SDK](#tab/net-standard-sdk)

```csharp
var messageOne = new Message(body);
var messageTwo = new Message(body);

var sendFirstMessageTask =
    queueClient.SendAsync(messageOne).ContinueWith(_ =>
    {
        Console.WriteLine("Sent message #1");
    });
var sendSecondMessageTask =
    queueClient.SendAsync(messageTwo).ContinueWith(_ =>
    {
        Console.WriteLine("Sent message #2");
    });

await Task.WhenAll(sendFirstMessageTask, sendSecondMessageTask);
Console.WriteLine("All messages sent");
```

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure.ServiceBus SDK](#tab/net-framework-sdk)

```csharp
var messageOne = new BrokeredMessage(body);
var messageTwo = new BrokeredMessage(body);

var sendFirstMessageTask =
    queueClient.SendAsync(messageOne).ContinueWith(_ =>
    {
        Console.WriteLine("Sent message #1");
    });
var sendSecondMessageTask =
    queueClient.SendAsync(messageTwo).ContinueWith(_ =>
    {
        Console.WriteLine("Sent message #2");
    });

await Task.WhenAll(sendFirstMessageTask, sendSecondMessageTask);
Console.WriteLine("All messages sent");
```

---

Aşağıdaki kod, bir eşzamanlı alma işlemine örnektir.

# <a name="microsoftazureservicebus-sdk"></a>[Microsoft.Azure.ServiceBus SDK](#tab/net-standard-sdk)

Tam <a href="https://github.com/Azure/azure-service-bus/blob/master/samples/DotNet/Microsoft.Azure.ServiceBus/SendersReceiversWithQueues" target="_blank">kaynak kodu örnekleri <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için GitHub deposuna bakın:

```csharp
var receiver = new MessageReceiver(connectionString, queueName, ReceiveMode.PeekLock);

static Task LogErrorAsync(Exception exception)
{
    Console.WriteLine(exception);
    return Task.CompletedTask;
};

receiver.RegisterMessageHandler(
    async (message, cancellationToken) =>
    {
        Console.WriteLine("Handle message");
        await receiver.CompleteAsync(message.SystemProperties.LockToken);
    },
    new MessageHandlerOptions(e => LogErrorAsync(e.Exception))
    {
        AutoComplete = false,
        MaxConcurrentCalls = 20
    });
```

Nesne, `MessageReceiver` bağlantı dizesi, sıra adı ve bir göz bakışı alma modu yla anında doldurulır. Ardından, `receiver` örnek ileti işleyicisini kaydetmek için kullanılır.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure.ServiceBus SDK](#tab/net-framework-sdk)

Tam <a href="https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/SendersReceiversWithQueues" target="_blank">kaynak kodu örnekleri <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için GitHub deposuna bakın:

```csharp
var factory = MessagingFactory.CreateFromConnectionString(connectionString);
var receiver = await factory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);

// Register the handler to receive messages asynchronously
receiver.OnMessageAsync(
    async message =>
    {
        Console.WriteLine("Handle message");
        await message.CompleteAsync();
    },
    new OnMessageOptions
    {
        AutoComplete = false,
        MaxConcurrentCalls = 20
    });
```

Bağlantı `MessagingFactory` dizesinden bir `factory` nesne oluşturur. Örnekte, `factory` bir `MessageReceiver` anlık olarak. Ardından, `receiver` örnek, ileti işleyicisini kaydetmek için kullanılır.

---

## <a name="receive-mode"></a>Alma modu

Bir kuyruk veya abonelik istemcisi oluştururken, bir alma modu belirtebilirsiniz: *Peek-lock* veya *Al ve Sil*. Varsayılan alma `PeekLock`modu. Varsayılan modda çalışırken, istemci Servis Veri Servisi'nden bir ileti almak için bir istek gönderir. İstemci iletiyi aldıktan sonra, iletiyi tamamlamak için bir istek gönderir.

Alma modunu `ReceiveAndDelete`ayarlarken, her iki adım da tek bir istekte birleştirilir. Bu adımlar, genel işlem sayısını azaltır ve genel ileti çıktısını artırabilir. Bu performans artışı iletileri kaybetme riskine girer.

Servis Veri Servisi, alma ve silme işlemlerine yönelik hareketleri desteklemez. Buna ek olarak, istemcinin bir iletiyi ertelemek veya [ölü harfle yazdığı](service-bus-dead-letter-queues.md) senaryolar için gözetleme kilidi semantikleri gereklidir.

## <a name="client-side-batching"></a>İstemci tarafı toplu işleme

İstemci tarafı toplu işleme, bir sıranın veya konu istemcisinin ileti göndermesini belirli bir süre geciktirmesini sağlar. İstemci bu süre içinde başka iletiler gönderirse, iletileri tek bir toplu iş olarak gönderir. İstemci tarafı toplu işlemi, bir kuyruğa veya abonelik istemcisine birden çok **Tam** isteği tek bir istekte toplu olarak ayırmasına da neden olur. Toplu İşlem yalnızca eşzamanlı **Gönder** ve **Tamamişlemleri** için kullanılabilir. Eşzamanlı işlemler hemen Servis Veri Servisi'ne gönderilir. Toplu işlem, gözetleme veya alma işlemleri için oluşmaz ve istemciler arasında toplu işlem gerçekleşmez.

# <a name="microsoftazureservicebus-sdk"></a>[Microsoft.Azure.ServiceBus SDK](#tab/net-standard-sdk)

.NET Standart SDK için toplu işlevsellik, henüz bir özelliği işlemek için ortaya çıkarmaz.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure.ServiceBus SDK](#tab/net-framework-sdk)

Varsayılan olarak, istemci 20 ms'lik bir toplu iş aralığı kullanır. İleti fabrikasını oluşturmadan önce [BatchFlushInterval][BatchFlushInterval] özelliğini ayarlayarak toplu iş aralığını değiştirebilirsiniz. Bu ayar, bu fabrika tarafından oluşturulan tüm istemcileri etkiler.

Toplu işlemi devre dışı atmak için [BatchFlushInterval][BatchFlushInterval] özelliğini **TimeSpan.Zero**olarak ayarlayın. Örneğin:

```csharp
var settings = new MessagingFactorySettings
{
    NetMessagingTransportSettings =
    {
        BatchFlushInterval = TimeSpan.Zero
    }
};
var factory = MessagingFactory.Create(namespaceUri, settings);
```

Toplu oluşturma faturalandırılabilir ileti işlemlerinin sayısını etkilemez ve yalnızca [Microsoft.ServiceBus.Messaging](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) kitaplığını kullanarak Service Bus istemci protokolü için kullanılabilir. HTTP protokolü toplu işlemi desteklemez.

> [!NOTE]
> Ayar, `BatchFlushInterval` toplu işlemenin uygulamanın perspektifinden örtülü olmasını sağlar. yani; uygulama yapar `SendAsync` `CompleteAsync` ve aramalar ve belirli Toplu aramalar yapmaz.
>
> Açık istemci tarafı toplu işlenme aşağıdaki yöntem çağrısı kullanılarak uygulanabilir:
> ```csharp
> Task SendBatchAsync(IEnumerable<BrokeredMessage> messages);
> ```
> Burada iletilerin birleşik boyutu, fiyatlandırma katmanı tarafından desteklenen maksimum boyuttan daha az olmalıdır.

---

## <a name="batching-store-access"></a>Toplu mağaza erişimi

Bir sıra, konu veya abonelik veri alım verime artış için, Servis Veri Servisi kendi iç deposuna yazarken birden çok ileti toplu. Bir sırada veya konu üzerinde etkinleştirilmişse, mağazaya ileti yazmak toplu olarak alınır. Bir kuyrukveya abonelikte etkinleştirilirse, mağazadan iletileri silme toplu olarak alınır. Bir varlık için toplu depo erişimi etkinse, Servis Veri Servisi bu varlıkla ilgili bir mağaza yazma işlemini 20 m'ye kadar geciktirir.

> [!NOTE]
> 20ms toplu işlem aralığının sonunda Servis Veri Servisi hatası olsa bile, toplu iş lerle iletileri kaybetme riski yoktur.

Bu aralıkta oluşan ek mağaza işlemleri toplu iş ekine eklenir. Toplu depo erişimi yalnızca **Gönder** ve **Tamamla** işlemlerini etkiler; alma işlemleri etkilenmez. Toplu depo erişimi, bir varlık üzerindeki bir özelliktir. Toplu işlem, toplu depo erişimini sağlayan tüm varlıklarda gerçekleşir.

Yeni bir kuyruk, konu veya abonelik oluştururken, toplu mağaza erişimi varsayılan olarak etkinleştirilir.

# <a name="microsoftazureservicebus-sdk"></a>[Microsoft.Azure.ServiceBus SDK](#tab/net-standard-sdk)

Toplu depo erişimini devre dışı kılmış olmak için `ManagementClient`bir . Özelliği `false`' ye ayarlayan bir `EnableBatchedOperations` sıra açıklamasından bir kuyruk oluşturun

```csharp
var queueDescription = new QueueDescription(path)
{
    EnableBatchedOperations = false
};
var queue = await managementClient.CreateQueueAsync(queueDescription);
```

Daha fazla bilgi için, aşağıdakilere bakın:
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.management.queuedescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.Management.QueueDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.management.subscriptiondescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.Management.SubscriptionDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.management.topicdescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.Management.TopicDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure.ServiceBus SDK](#tab/net-framework-sdk)

Toplu depo erişimini devre dışı kılmış olmak için `NamespaceManager`bir . Özelliği `false`' ye ayarlayan bir `EnableBatchedOperations` sıra açıklamasından bir kuyruk oluşturun

```csharp
var queueDescription = new QueueDescription(path)
{
    EnableBatchedOperations = false
};
var queue = namespaceManager.CreateQueue(queueDescription);
```

Daha fazla bilgi için, aşağıdakilere bakın:
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.QueueDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.SubscriptionDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.topicdescription.enablebatchedoperations?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.TopicDescription.EnableBatchedOperations` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

---

Toplu depo erişimi faturalandırılabilir ileti işlemlerinin sayısını etkilemez ve bir sıra, konu veya abonelik özelliğidir. Alma modundan ve istemci ile Servis Veri Servisi hizmeti arasında kullanılan protokolden bağımsızdır.

## <a name="prefetching"></a>Ön getirme

[Ön alma,](service-bus-prefetch.md) bir alma işlemi gerçekleştirirken hizmetten ek iletiler yüklemek için kuyruk veya abonelik istemcisi sağlar. İstemci bu iletileri yerel bir önbellekte depolar. Önbelleğin boyutu veya `QueueClient.PrefetchCount` `SubscriptionClient.PrefetchCount` özellikleri tarafından belirlenir. Ön alma sağlayan her istemci kendi önbelleğini korur. Önbellek istemciler arasında paylaşılmaz. İstemci bir alma işlemi başlatır ve önbelleği boşsa, hizmet bir dizi ileti iletir. Toplu iş boyutu önbelleğin boyutuna veya hangisi daha küçükse 256 KB'ye eşittir. İstemci bir alma işlemi başlatır ve önbellek bir ileti içeriyorsa, ileti önbellekten alınır.

İleti önceden alındığı zaman, hizmet önceden getirilen iletiyi kilitler. Kilitle, önceden getirilen ileti farklı bir alıcı tarafından alınamaz. Kilit sona ermeden önce alıcı iletiyi tamamlayamazsa, ileti diğer alıcılar tarafından kullanılabilir hale gelir. İletinin önceden getirilen kopyası önbellekte kalır. Süresi dolan önbelleğe alınan kopyayı tüketen alıcı, bu iletiyi tamamlamaya çalıştığında bir özel durum alır. Varsayılan olarak, ileti kilidi 60 saniye sonra sona erer. Bu değer 5 dakikaya kadar uzatılabilir. Süresi dolmuş iletilerin tüketimini önlemek için önbellek boyutu her zaman kilit zaman aşımı aralığında bir istemci tarafından tüketilebilen ileti sayısından daha küçük olmalıdır.

Varsayılan kilit süresi 60 saniye kullanırken, `PrefetchCount` iyi bir değer fabrikanın tüm alıcılarının maksimum işlem hızlarının 20 katıdır. Örneğin, bir fabrika üç alıcı oluşturur ve her alıcı saniyede en fazla 10 ileti işleyebilir. Prefetch sayısı 20 X 3 X 10 = 600'ü geçmemelidir. Varsayılan olarak, `PrefetchCount` 0 olarak ayarlanır, bu da hizmetten ek ileti getirilmediği anlamına gelir.

İletilerin önceden alınması, genel ileti işlemleri veya tur lar sayısını azalttığı için bir kuyruk veya abonelik için genel iş bilgisini artırır. Ancak, ilk iletiyi alma daha uzun sürer (artan ileti boyutu nedeniyle). Bu iletiler istemci tarafından zaten karşıdan yüklenmiş olduğundan, önceden alınmış iletileri almak daha hızlı olacaktır.

İletinin zaman-canlı (TTL) özelliği, sunucunun iletiyi istemciye gönderdiği sırada sunucu tarafından denetlenir. İleti alındığı zaman istemci iletinin TTL özelliğini denetlemez. Bunun yerine, ileti istemci tarafından önbelleğe alınırken iletinin TTL'si geçmiş olsa bile ileti alınabilir.

Ön alma, faturalandırılabilir ileti işlemlerinin sayısını etkilemez ve yalnızca Servis Veri Servisi istemci protokolü için kullanılabilir. HTTP protokolü ön getirmeyi desteklemez. Prefetching hem senkron hem de eşzamanlı alma işlemleri için kullanılabilir.

# <a name="microsoftazureservicebus-sdk"></a>[Microsoft.Azure.ServiceBus SDK](#tab/net-standard-sdk)

Daha fazla bilgi için `PrefetchCount` aşağıdaki özelliklere bakın:

* <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.QueueClient.PrefetchCount` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.servicebus.subscriptionclient.prefetchcount?view=azure-dotnet" target="_blank">`Microsoft.Azure.ServiceBus.SubscriptionClient.PrefetchCount` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

# <a name="windowsazureservicebus-sdk"></a>[WindowsAzure.ServiceBus SDK](#tab/net-framework-sdk)

Daha fazla bilgi için `PrefetchCount` aşağıdaki özelliklere bakın:

* <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.queueclient.prefetchcount?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.QueueClient.PrefetchCount` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
* <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.prefetchcount?view=azure-dotnet" target="_blank">`Microsoft.ServiceBus.Messaging.SubscriptionClient.PrefetchCount` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

---

## <a name="prefetching-and-receivebatch"></a>Prefetching ve ReceiveBatch

> [!NOTE]
> Microsoft.Azure.ServiceBus SDK toplu iş işlevlerini ortaya çıkarmadığı için bu bölüm yalnızca WindowsAzure.ServiceBus SDK için geçerlidir.

Birden çok iletiyi bir araya getirme kavramları, iletileri toplu olarak`ReceiveBatch`işlemeye benzer anlambilime sahip olsa da ( ), bunları birlikte kullanırken akılda tutulması gereken bazı küçük farklılıklar vardır.

Prefetch istemci (ve)`QueueClient` `SubscriptionClient`üzerinde bir yapılandırma (veya mod) ve `ReceiveBatch` bir işlemdir (istek-yanıt semantiği vardır).

Bunları birlikte kullanırken, aşağıdaki durumları göz önünde bulundurun -

* Prefetch, almayı beklediğiniz ileti sayısından daha büyük veya `ReceiveBatch`eşit olmalıdır.
* Prefetch, n'nin varsayılan kilit süresi olduğu saniyede işlenen ileti sayısının n/3 katına kadar olabilir.

Bu ileti belirli bir alıcıya kilitli olduğunu ima eder, çünkü açgözlü bir yaklaşım (yani prefetch sayısı çok yüksek tutmak) sahip bazı zorluklar vardır. Öneri, yukarıda belirtilen eşikler arasındaki prefetch değerlerini denemek ve neyin uyduğunu ampirik olarak belirlemektir.

## <a name="multiple-queues"></a>Birden çok kuyruk

Beklenen yük tek bir kuyruk veya konu tarafından işlenemiyorsa, birden çok ileti varlığı kullanmanız gerekir. Tüm varlıklar için aynı istemciyi kullanmak yerine, birden çok varlık kullanırken, her varlık için özel bir istemci oluşturun.

## <a name="development-and-testing-features"></a>Geliştirme ve test özellikleri

> [!NOTE]
> Microsoft.Azure.ServiceBus SDK bu işlevselliği ortaya çıkarmadığı için bu bölüm yalnızca WindowsAzure.ServiceBus SDK için geçerlidir.

Servis Veri Servisi'nin, **üretim yapılandırmalarında asla kullanılmaması gereken**geliştirme [`TopicDescription.EnableFilteringMessagesBeforePublishing`][TopicDescription.EnableFiltering]için özel olarak kullanılan bir özelliği vardır: .

Konuya yeni kurallar veya filtreler eklendiğinde, yeni [`TopicDescription.EnableFilteringMessagesBeforePublishing`][TopicDescription.EnableFiltering] filtre ifadesinin beklendiği gibi çalıştığını doğrulamak için kullanabilirsiniz.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki bölümlerde tipik ileti senaryoları açıklanVe tercih edilen Servis Veri Servisi veri günü ayarlarını ana hatlarını açıklar. İş mesuliyet oranları küçük (1 ileti/saniyeden az), orta (1 ileti/saniye veya daha büyük ancak 100 ileti/saniyeden az) ve yüksek (100 ileti/saniye veya daha büyük) olarak sınıflandırılır. İstemci sayısı küçük (5 veya daha az), orta (5'ten fazla ama 20'den az veya 20'ye eşit) ve büyük (20'den fazla) olarak sınıflandırılır.

### <a name="high-throughput-queue"></a>Yüksek iş tonu sırası

Amaç: Tek bir kuyruğun iş boyunun en üst düzeye çıkar. Gönderenlerin ve alıcıların sayısı azdır.

* Sıraya genel gönderme oranını artırmak için gönderenler oluşturmak için birden çok ileti fabrikası kullanın. Her gönderen için, eşzamanlı işlemler veya birden çok iş parçacığı kullanın.
* Kuyruktaki genel alma oranını artırmak için alıcı oluşturmak için birden çok ileti fabrikası kullanın.
* İstemci tarafı toplu işlerinden yararlanmak için eşzamanlı işlemleri kullanın.
* Service Bus istemci iletişim iletimlerinin sayısını azaltmak için toplu işlem aralığını 50 ms olarak ayarlayın. Birden çok gönderen kullanılırsa, toplu işlem aralığını 100 ms'e yükseltin.
* Toplu depo erişimini etkin bırakın. Bu erişim, iletilerin sıraya yazılabilir genel oranını artırır.
* Prefetch sayısını, bir fabrikanın tüm alıcılarının maksimum işlem oranlarının 20 katına ayarlayın. Bu sayı, Servis Veri Mes'ü istemci iletişim iletimlerinin sayısını azaltır.

### <a name="multiple-high-throughput-queues"></a>Birden çok yüksek iş tonu kuyruğu

Hedef: Birden çok sıranın genel iş alım ını en üst düzeye çıkarın. Tek bir sıranın iş bölümü orta veya yüksektir.

Birden çok kuyrukta maksimum verim elde etmek için, tek bir kuyruğun iş bürünme kısmını en üst düzeye çıkarmak için ana hatlarıyla özetlenen ayarları kullanın. Ayrıca, farklı kuyruklardan gönderen veya alan istemciler oluşturmak için farklı fabrikalar kullanın.

### <a name="low-latency-queue"></a>Düşük gecikme sırası

Hedef: Bir sıranın veya konunun uçtan uca gecikmesini en aza indirin. Gönderenlerin ve alıcıların sayısı azdır. Sıranın iş bölümü küçük veya orta düzeydedir.

* İstemci tarafı toplu işlemeyi devre dışı. İstemci hemen bir ileti gönderir.
* Toplu depo erişimini devre dışı kılmış. Hizmet hemen mağazaya ileti yazar.
* Tek bir istemci kullanıyorsanız, prefetch sayısını alıcının işlem hızının 20 katına ayarlayın. Sıraya aynı anda birden çok ileti gelirse, Servis Veri Servisi istemci protokolü hepsini aynı anda iletir. İstemci sonraki iletiyi aldığında, bu ileti zaten yerel önbellektedir. Önbellek küçük olmalıdır.
* Birden çok istemci kullanıyorsanız, prefetch sayısını 0 olarak ayarlayın. İlk istemci ilk iletiyi işlemeye devam ederken, ikinci istemci ikinci iletiyi ayarı olarak alabilir.

### <a name="queue-with-a-large-number-of-senders"></a>Çok sayıda gönderenle sıra

Hedef: Çok sayıda gönderenle bir sıranın veya konunun iş büründürülme sini en üst düzeye çıkarın. Her gönderen orta oranda ileti gönderir. Alıcı sayısı azdır.

Service Bus, bir ileti kuruluşuna (veya AMQP kullanarak 5000) en fazla 1000 eşzamanlı bağlantı sağlar. Bu sınır ad alanı düzeyinde uygulanır ve kuyruklar/konular/abonelikler ad alanı başına eşzamanlı bağlantı sınırıyla kapatılır. Kuyruklar için bu sayı gönderenler ve alıcılar arasında paylaşılır. Gönderenler için 1000 bağlantının tümü gerekiyorsa, sırayı bir konu ve tek bir abonelikle değiştirin. Bir konu gönderenlerden en fazla 1000 eşzamanlı bağlantıyı kabul ederken, abonelik alıcılardan gelen ek 1000 eşzamanlı bağlantıyı kabul eder. 1000'den fazla eşzamanlı gönderen gerekiyorsa, gönderenler HTTP üzerinden Servis Veri Servisi protokolüne ileti göndermelidir.

Iş verme kullanımını en üst düzeye çıkarmak için aşağıdaki adımları gerçekleştirin:

* Her gönderen farklı bir işlemde yaşıyorsa, işlem başına yalnızca tek bir fabrika kullanın.
* İstemci tarafı toplu işlerinden yararlanmak için eşzamanlı işlemleri kullanın.
* Service Bus istemci iletişim iletimlerinin sayısını azaltmak için varsayılan toplu işlem aralığını 20 ms kullanın.
* Toplu depo erişimini etkin bırakın. Bu erişim, iletilerin sıraya veya konuya yazılabilir genel oranını artırır.
* Prefetch sayısını, bir fabrikanın tüm alıcılarının maksimum işlem oranlarının 20 katına ayarlayın. Bu sayı, Servis Veri Mes'ü istemci iletişim iletimlerinin sayısını azaltır.

### <a name="queue-with-a-large-number-of-receivers"></a>Çok sayıda alıcıyla sıra

Hedef: Çok sayıda alıcıyla bir sıranın veya aboneliğin alma oranını en üst düzeye çıkarın. Her alıcı iletileri orta hızda alır. Gönderenlerin sayısı azdır.

Service Bus, bir varlığa en fazla 1000 eşzamanlı bağlantı sağlar. Bir kuyruk 1000'den fazla alıcı gerektiriyorsa, sırayı bir konu ve birden çok abonelikle değiştirin. Her abonelik en fazla 1000 eşzamanlı bağlantıyı destekleyebilir. Alternatif olarak, alıcılar http protokolü aracılığıyla kuyruğa erişebilir.

Iş verme kullanımını en üst düzeye çıkarmak için aşağıdakileri yapın:

* Her alıcı farklı bir işlemde yaşıyorsa, işlem başına yalnızca tek bir fabrika kullanın.
* Alıcılar senkron veya eşzamanlı işlemleri kullanabilir. Tek bir alıcının orta alma oranı göz önüne alındığında, Tam isteğiistemci tarafı toplu işleme alıcı iş bilgili etkilemez.
* Toplu depo erişimini etkin bırakın. Bu erişim, varlığın toplam yükünü azaltır. Ayrıca, iletilerin kuyruğa veya konuya yazılabilir genel oranını azaltır.
* Prefetch sayısını küçük bir değere ayarlayın (örneğin, PrefetchCount = 10). Bu sayı, diğer alıcıların önbelleğe alınmış çok sayıda iletisi varken alıcıların boşta olmasını önler.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Az sayıda abonelik içeren konu

Hedef: Az sayıda abonelikle bir konunun iş kısmını en üst düzeye çıkarın. İleti birçok abonelik tarafından alınır, bu da tüm abonelikler üzerindeki birleşik alma oranının gönderme oranından daha büyük olduğu anlamına gelir. Gönderenlerin sayısı azdır. Abonelik başına alıcı sayısı azdır.

Iş verme kullanımını en üst düzeye çıkarmak için aşağıdakileri yapın:

* Konuya genel gönderme oranını artırmak için gönderenler oluşturmak için birden çok ileti fabrikası kullanın. Her gönderen için, eşzamanlı işlemler veya birden çok iş parçacığı kullanın.
* Abonelikten genel alma oranını artırmak için alıcı oluşturmak için birden çok ileti fabrikası kullanın. Her alıcı için, eşzamanlı işlemler veya birden çok iş parçacığı kullanın.
* İstemci tarafı toplu işlerinden yararlanmak için eşzamanlı işlemleri kullanın.
* Service Bus istemci iletişim iletimlerinin sayısını azaltmak için varsayılan toplu işlem aralığını 20 ms kullanın.
* Toplu depo erişimini etkin bırakın. Bu erişim, iletilerin konuya yazılabilir genel oranını artırır.
* Prefetch sayısını, bir fabrikanın tüm alıcılarının maksimum işlem oranlarının 20 katına ayarlayın. Bu sayı, Servis Veri Mes'ü istemci iletişim iletimlerinin sayısını azaltır.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Çok sayıda abonelik içeren konu

Hedef: Çok sayıda abonelikiçeren bir konunun iş sayısını en üst düzeye çıkarın. İleti birçok abonelik tarafından alınır, bu da tüm abonelikler üzerindeki birleşik alma oranının gönderme oranından çok daha büyük olduğu anlamına gelir. Gönderenlerin sayısı azdır. Abonelik başına alıcı sayısı azdır.

Çok sayıda aboneliği olan konular, tüm iletiler tüm aboneliklere yönlendirilmişse genellikle düşük genel iş çıkışını ortaya çıkarır. Bu düşük iş elde etmesi, her iletinin birçok kez alınmasından ve bir konuda bulunan tüm iletilerin ve tüm aboneliklerinin aynı mağazada depolanmış olmasından kaynaklanır. Gönderen sayısının ve abonelik başına alıcı sayısının az olduğu varsayılır. Service Bus, konu başına 2.000'e kadar aboneliği destekler.

Iş verme kullanımını en üst düzeye çıkarmak için aşağıdaki adımları deneyin:

* İstemci tarafı toplu işlerinden yararlanmak için eşzamanlı işlemleri kullanın.
* Service Bus istemci iletişim iletimlerinin sayısını azaltmak için varsayılan toplu işlem aralığını 20 ms kullanın.
* Toplu depo erişimini etkin bırakın. Bu erişim, iletilerin konuya yazılabilir genel oranını artırır.
* Prefetch sayısını beklenen alma oranının 20 katına saniyeler içinde ayarlayın. Bu sayı, Servis Veri Mes'ü istemci iletişim iletimlerinin sayısını azaltır.

<!-- .NET Standard SDK, Microsoft.Azure.ServiceBus -->
[QueueClient]: /dotnet/api/microsoft.azure.servicebus.queueclient
[MessageSender]: /dotnet/api/microsoft.azure.servicebus.core.messagesender

<!-- .NET Framework SDK, Microsoft.Azure.ServiceBus -->
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[BatchFlushInterval]: /dotnet/api/microsoft.servicebus.messaging.messagesender.batchflushinterval
[ForcePersistence]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence
[EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnableFiltering]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing

<!-- Local links -->
[Partitioned messaging entities]: service-bus-partitioning.md