---
title: Bölümlenmiş Azure Hizmet Veri Günü kuyrukları ve konuları oluşturma | Microsoft Dokümanlar
description: Birden çok ileti aracısı kullanarak Servis Veri Aracı sı ve konularını nasıl bölümlere ayırır.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 02/06/2020
ms.author: aschhab
ms.openlocfilehash: 671368993acb43c0d55eca73119effa934e3cff8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260949"
---
# <a name="partitioned-queues-and-topics"></a>Bölümlenmiş kuyruklar ve konular

Azure Servis Veri Servisi, iletileri işlemek için birden çok ileti aracısı ve ileti depolamak için birden çok mesajlaşma mağazası kullanır. Geleneksel bir sıra veya konu tek bir ileti aracısı tarafından işlenir ve tek bir ileti deposunda depolanır. Service Bus *bölümleri,* kuyrukların ve konuların veya *ileti kuruluşlarının*birden çok ileti aracısı ve mesajlaşma mağazaları arasında bölümlenmesini sağlar. Bölümleme, bölümlenmiş bir varlığın genel iş ortasının artık tek bir ileti aracıcısının veya ileti deposunun performansıyla sınırlı olmadığı anlamına gelir. Ayrıca, bir ileti deposunun geçici kesintisi, bölümlenmiş bir sırayı veya konuyu kullanılamaz hale getirmez. Bölümlenmiş kuyruklar ve konular, hareketler ve oturumlar için destek gibi tüm gelişmiş Hizmet Veri Yolundan özellikleri içerebilir.

> [!NOTE]
> Bölümleme, Temel veya Standart SKU'larda tüm kuyruklar ve konular için varlık oluşturmada kullanılabilir. Premium mesajlaşma SKU için kullanılamaz, ancak Premium ad alanlarında önceden varolan bölümlenmiş varlıklar beklendiği gibi çalışmaya devam eder.
 
Varolan herhangi bir sırada veya konu üzerinde bölümleme seçeneğini değiştirmek mümkün değildir; seçeneği yalnızca varlığı oluşturduğunuzda ayarlayabilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Bölümlenmiş her sıra veya konu birden çok bölümden oluşur. Her bölüm farklı bir ileti deposunda depolanır ve farklı bir ileti aracısı tarafından işlenir. Bir ileti bölümlenmiş bir kuyruğa veya konuya gönderildiğinde, Hizmet Veri Servisi iletiyi bölümlerden birine atar. Seçim, Service Bus tarafından veya gönderenin belirtebileceği bir bölüm anahtarı kullanılarak rasgele yapılır.

İstemci, bölümlenmiş bir kuyruktan veya bir abonelikten bölümlenmiş bir konuya ileti almak istediğinde, Hizmet Veri Gönderi iletilerin tüm bölümlerini sorgular ve ardından ileti mağazalarından herhangi birinden alınan ilk iletiyi alıcıya döndürür. Servis Veri Servisi diğer iletileri önbelleğe alır ve ek alma istekleri aldığında iade eder. Alıcı istemci bölümleme farkında değildir; bölümlenmiş bir sıranın veya konunun istemciye dönük davranışı (örneğin, okuma, tamamlama, erteleme, deadletter, prefetching) normal bir varlığın davranışıyla aynıdır.

Bölümlenmiş bir kuyruğa veya konuya ileti gönderirken veya ileti alırken ek bir maliyet yoktur.

## <a name="enable-partitioning"></a>Bölümleme etkinleştirme

Azure Hizmet Veri Servisi ile bölümlenmiş kuyrukları ve konuları kullanmak için Azure SDK `api-version=2013-10` sürüm 2.2 veya sonraki sürümlerini kullanın veya HTTP isteklerinizi belirtin veya daha sonra belirtin.

### <a name="standard"></a>Standart

Standart ileti katmanında, 1, 2, 3, 4 veya 5 GB boyutlarında (varsayılan değer 1 GB'dır) Servis Veri Servisi kuyrukları ve konuları oluşturabilirsiniz. Bölümleme etkinken, Service Bus varlığın her biri belirtilen aynı boyutta 16 kopya (16 bölüm) oluşturur. Bu nedenle, 16 bölümden sonra 5 GB boyutunda bir kuyruk oluşturursanız, \* en yüksek sıra boyutu (5 16) = 80 GB olur. Azure [portalındaki][Azure portal]girişine bakarak bölümlenmiş sıranızın veya konunuzun en büyük boyutunu, bu varlık için **Genel Bakış** bıçakta görebilirsiniz.

### <a name="premium"></a>Premium

Premium katman ad alanında, bölümleme varlıkları desteklenmez. Ancak, 1, 2, 3, 4, 5, 10, 20, 40 veya 80 GB boyutlarında (varsayılan değer 1 GB'dır) Hizmet Veri Servisi kuyrukları ve konuları oluşturmaya devam edebilirsiniz. [Azure portalındaki][Azure portal]girişine bakarak kuyruğuzun veya konunuzun boyutunu, bu varlık için **Genel Bakış** bıçakta görebilirsiniz.

### <a name="create-a-partitioned-entity"></a>Bölümlenmiş varlık oluşturma

Bölümlenmiş bir sıra veya konu oluşturmanın birkaç yolu vardır. Uygulamanızdan sıra yı veya konuyu oluşturduğunuzda, [sırasıyla QueueDescription.EnablePartitioning][QueueDescription.EnablePartitioning] veya [TopicDescription.EnablePartitioning][TopicDescription.EnablePartitioning] özelliğini **doğru**olarak ayarlayarak sıra veya konu için bölümleme özelliğini etkinleştirebilirsiniz. Bu özellikler, sıra nın veya konunun oluşturulduğu anda ayarlanmalıdır ve yalnızca eski [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) kitaplığında kullanılabilir. Daha önce belirtildiği gibi, varolan bir sırada veya konu üzerinde bu özellikleri değiştirmek mümkün değildir. Örnek:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternatif olarak, [Azure portalında][Azure portal]bölümlenmiş bir sıra veya konu oluşturabilirsiniz. Portalda bir kuyruk veya konu oluşturduğunuzda, kuyruktaki veya konu **Oluştur** iletişim kutusundaki **Bölümleme Etkinleştir** seçeneği varsayılan olarak işaretlenir. Bu seçeneği yalnızca Standart katman varlığında devre dışı kullanabilirsiniz; Premium katman bölümleme desteklenmez ve onay kutusunun hiçbir etkisi yoktur. 

## <a name="use-of-partition-keys"></a>Bölüm tuşlarının kullanımı

İleti bölümlenmiş bir kuyruğa veya konuya kenredildiğinde, Servis Veri Servisi bir bölüm anahtarının varlığını denetler. Bir tane bulursa, bu tuşu temel alan bölümü seçer. Bir bölüm anahtarı bulamazsa, bir iç algoritmadayalı bölüm seçer.

### <a name="using-a-partition-key"></a>Bölüm anahtarı kullanma

Oturumlar veya hareketler gibi bazı senaryolar iletilerin belirli bir bölümde depolanmasını gerektirir. Tüm bu senaryolar bir bölüm anahtarı kullanımını gerektirir. Aynı bölüm anahtarını kullanan tüm iletiler aynı bölüme atanır. Bölüm geçici olarak kullanılamıyorsa, Servis Veri Servisi bir hata döndürür.

Senaryoya bağlı olarak, farklı ileti özellikleri bir bölüm anahtarı olarak kullanılır:

**SessionId**: İletide [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özellik kümesi varsa, Hizmet Veri Servisi oturum anahtarı olarak **SessionID'yi** kullanır. Bu şekilde, aynı oturuma ait tüm iletiler aynı ileti aracısı tarafından işlenir. Oturumlar, Hizmet Veri Servisi'nin ileti siparişini ve oturum durumlarının tutarlılığını garanti etmesini sağlar.

**PartitionKey**: Bir [iletide SessionId](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özelliği [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) kümesi değil de PartitionKey özelliği varsa, Hizmet Veri İşlemi bölüm anahtarı olarak [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özellik değerini kullanır. İletide [hem SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) hem de [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özellikleri kümesi varsa, her iki özelliğin de aynı olması gerekir. [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özelliği [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özelliğinden farklı bir değere ayarlanmışsa, Hizmet Veri Servisi geçersiz bir işlem özel durumu döndürür. Bir gönderen oturum farkında işlem iletileri gönderirse [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özelliği kullanılmalıdır. Bölüm anahtarı, bir hareket içinde gönderilen tüm iletilerin aynı ileti aracısı tarafından işlenmesini sağlar.

**MessageId**: Sıra veya konu [nun RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) özelliği **gerçek** olarak ayarlanmışsa ve SessionId veya [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özellikleri ayarlanmıyorsa, [MessageId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özellik değeri bölüm anahtarı olarak hizmet vermektedir. [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) (Microsoft .NET ve AMQP kitaplıkları, gönderen uygulama atamazsa otomatik olarak bir ileti kimliği atar.) Bu durumda, aynı iletinin tüm kopyaları aynı ileti aracısı tarafından işlenir. Bu kimlik, Service Bus'Un yinelenen iletileri algılamasını ve ortadan kaldırmasını sağlar. [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) özelliği **doğru**ayarlanmazsa, Hizmet Veri Servisi [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) özelliğini bir bölüm anahtarı olarak kabul etmez.

### <a name="not-using-a-partition-key"></a>Bölüm anahtarı nı kullanmama

Bir bölüm anahtarı yokluğunda, Servis Veri Servisi iletileri bölümlenmiş sıranın veya konunun tüm bölümlerine round-robin şeklinde dağıtır. Seçilen bölüm kullanılamıyorsa, Servis Veri Servisi iletiyi farklı bir bölüme atar. Bu şekilde, bir ileti deposunun geçici olarak kullanılamamasına rağmen gönderme işlemi başarılı ol. Ancak, bir bölüm anahtarı sağladığı garantili sipariş elde etmez.

Kullanılabilirlik (bölme anahtarı yok) ve tutarlılık (bölüm anahtarı kullanarak) arasındaki dengenin daha ayrıntılı bir şekilde tartışılması için [bu makaleye](../event-hubs/event-hubs-availability-and-consistency.md)bakın. Bu bilgiler, bölümlenmiş Hizmet Veri Hizmeti veri hizmeti varlıkları için eşit olarak geçerlidir.

Hizmet Veri Servisi'ne iletiyi farklı bir bölüme eklemek için yeterli zaman vermek için, iletiyi gönderen istemci tarafından belirtilen [İşlem Zaman Ödeme](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) değeri 15 saniyeden büyük olmalıdır. [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) özelliğini varsayılan değer olan 60 saniye olarak ayarlamanız önerilir.

Bir bölüm anahtarı belirli bir bölüme bir iletiyi "sabitler". Bu bölümü tutan ileti deposu kullanılamıyorsa, Servis Veri Servisi bir hata döndürür. Bir bölüm anahtarı yokluğunda, Servis Veri Servisi farklı bir bölüm seçebilir ve işlem başarılı. Bu nedenle, gerekli olmadıkça bir bölüm anahtarı sağlamamanız önerilir.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Gelişmiş konular: bölümlenmiş varlıklarla hareketleri kullanma

Bir işlem kapsamında gönderilen iletilerin bölüm anahtarını belirtmesi gerekir. Anahtar aşağıdaki özelliklerden biri olabilir: [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), veya [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid). Aynı işlemin parçası olarak gönderilen tüm iletiler aynı bölüm anahtarını belirtmelidir. Bir hareket içinde bölüm anahtarı olmayan bir ileti göndermeye çalışırsanız, Servis Veri Servisi geçersiz bir işlem özel durumu döndürür. Aynı hareket içinde farklı bölüm anahtarları olan birden çok ileti göndermeye çalışırsanız, Servis Veri Servisi geçersiz bir işlem özel durumu döndürür. Örnek:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

Bölüm anahtarı olarak hizmet veren özelliklerden herhangi biri ayarlanırsa, Servis Veri Mesuli iletiyi belirli bir bölüme sabitler. Bu davranış, bir hareket kullanılıp kullanılmadığı oluşur. Gerekli değilse bir bölüm anahtarı belirtmemeniz önerilir.

## <a name="using-sessions-with-partitioned-entities"></a>Bölümlenmiş varlıklarla oturumları kullanma

Oturuma duyarlı bir konuya veya kuyruğa işlem iletisi göndermek için iletinin [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özelliğinin ayarlamış olması gerekir. [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özelliği de belirtilmişse, [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özelliğiyle aynı olmalıdır. Farklıysa, Servis Veri Servisi geçersiz bir işlem özel durumu döndürür.

Normal (bölümlenmemiş) kuyrukların veya konuların aksine, farklı oturumlara birden çok ileti göndermek için tek bir işlem kullanmak mümkün değildir. Denenmişse, Servis Veri Servisi geçersiz bir işlem özel durum verir. Örnek:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.SessionId = "mySession";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Bölümlenmiş varlıklarla otomatik ileti iletme

Service Bus, bölümlenmiş varlıklardan, bölümlere veya bunlar arasında otomatik ileti iletmesini destekler. Otomatik ileti iletmesini etkinleştirmek için, kaynak kuyruğunda veya abonelikte [QueueDescription.ForwardTo][QueueDescription.ForwardTo] özelliğini ayarlayın. İletibir bölüm anahtarı[(SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey)veya [MessageId)](/dotnet/api/microsoft.azure.servicebus.message.messageid)belirtirse, bu bölüm anahtarı hedef varlık için kullanılır.

## <a name="considerations-and-guidelines"></a>Dikkat edilecek hususlar ve yönergeler
* **Yüksek tutarlılık özellikleri**: Bir varlık oturumlar, yinelenen algılama veya bölümleme anahtarının açık denetimi gibi özellikler kullanıyorsa, ileti işlemleri her zaman belirli bölüme yönlendirilir. Bölümlerden herhangi biri yüksek trafikle karşılaşırsa veya temel depo sağlıksızsa, bu işlemler başarısız olur ve kullanılabilirlik azalır. Genel olarak, tutarlılık hala bölümlenmemiş varlıklarçok daha yüksektir; tüm trafiğin aksine, yalnızca bir trafik alt kümesinde sorunlar yaşanıyor. Daha fazla bilgi için, [kullanılabilirlik ve tutarlılık](../event-hubs/event-hubs-availability-and-consistency.md)bu tartışmaya bakın.
* **Yönetim**: Oluştur, Güncelleştir ve Sil gibi işlemler varlığın tüm bölümleriüzerinde gerçekleştirilmelidir. Herhangi bir bölüm sağlıksızsa, bu işlemler için hatalara neden olabilir. Get işlemi için, ileti sayıları gibi bilgilerin tüm bölümlerden toplanmış olması gerekir. Herhangi bir bölüm sağlıksızsa, varlık kullanılabilirlik durumu sınırlı olarak bildirilir.
* **Düşük toplu ileti senaryoları**: Özellikle HTTP protokolünü kullanırken, bu tür senaryolar için tüm iletileri almak için birden çok alma işlemi gerçekleştirmeniz gerekebilir. İstek leri almak için, ön uç tüm bölümlerde bir alma gerçekleştirir ve alınan tüm yanıtları önbelleğe alır. Aynı bağlantıda sonraki bir alma isteği bu önbelleğe alma yararlanacak ve gecikme süreleri daha düşük olacaktır. Ancak, birden çok bağlantınız varsa veya HTTP kullanıyorsanız, bu her istek için yeni bir bağlantı kurar. Bu nedenle, aynı düğüm üzerine ineceğinigaranti yoktur. Varolan tüm iletiler kilitlenir ve başka bir ön uçta önbelleğe alınırsa, alma işlemi **geçersiz olarak**döndürür. İletilerin süresi sonunda sona erer ve iletileri yeniden alabilirsiniz. HTTP canlı tutulması önerilir. Düşük hacimli senaryolarda bölümleme kullanırken, alma işlemleri beklenenden daha uzun sürebilir. Bu nedenle, bu senaryolarda bölümleme kullanmamanızı öneririz. Varolan bölümlenmiş varlıkları silin ve performansı artırmak için devre dışı bırakılmış bölümleme ile yeniden oluşturun.
* **Gözat/Gözetleme iletileri**: Yalnızca eski [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) kitaplığında kullanılabilir. [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) her zaman [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.messagecount) özelliğinde belirtilen ileti sayısını döndürmez. Bu davranışın iki yaygın nedeni vardır. Bunun nedenlerinden biri, ileti lerin toplanmasının toplam boyutunun en fazla 256 KB boyutundan fazla olmasıdır. Başka bir nedeni, sıra veya konu **true** [EnablePartitioning özelliği](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning) ayarlanmışsa, bir bölüm iletileri istenen sayısını tamamlamak için yeterli iletileri olmayabilir. Genel olarak, bir uygulama belirli sayıda ileti almak istiyorsa, bu sayıda ileti alana kadar [PeekBatch'ı](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) tekrar tekrar aramalıdır veya başka ileti bulunmaz. Kod örnekleri de dahil olmak üzere daha fazla bilgi için [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) veya [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.peekbatch) API belgelerine bakın.

## <a name="latest-added-features"></a>En son eklenen özellikler

* Ekle veya kaldır kuralı artık bölümlenmiş varlıklarla desteklenir. Bölümlenmemiş varlıklardan farklı olarak, bu işlemler işlemler altında desteklenmez. 
* AMQP artık bölümlenmiş bir varlığa ve bu varlıktan ileti göndermek ve almak için desteklenir.
* AMQP şimdi aşağıdaki işlemler için desteklenir: [Toplu Gönder](/dotnet/api/microsoft.servicebus.messaging.queueclient.sendbatch), [Toplu Alma](/dotnet/api/microsoft.servicebus.messaging.queueclient.receivebatch), Sıra Numarası ile [Al](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive), [Peek](/dotnet/api/microsoft.servicebus.messaging.queueclient.peek), [Kilit Yenile](/dotnet/api/microsoft.servicebus.messaging.queueclient.renewmessagelock), [Zamanlanmış İleti,](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync) [İptal Zamanlanan İleti](/dotnet/api/microsoft.azure.servicebus.queueclient.cancelscheduledmessageasync), Kural Ekle , [Kural](/dotnet/api/microsoft.azure.servicebus.ruledescription) [Kaldır](/dotnet/api/microsoft.azure.servicebus.ruledescription), [Oturum Yenile Kilidi](/dotnet/api/microsoft.servicebus.messaging.messagesession.renewlock), Oturum [Durumu Ayarla](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate), [Oturum Durumu Alın](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate)ve Oturumları [Numaralandırın](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions).

## <a name="partitioned-entities-limitations"></a>Bölümlenmiş varlıklar sınırlamaları

Şu anda Servis Veri Servisi, bölümlenmiş kuyruklar ve konular için aşağıdaki sınırlamaları uygular:

* Bölümlenmiş kuyruklar ve konular Premium ileti katmanında desteklenmez. Oturumlar, SessionId kullanılarak birinci kademede desteklenir. 
* Bölümlenmiş kuyruklar ve konular, tek bir harekette farklı oturumlara ait iletigöndermeyi desteklemez.
* Service Bus şu anda ad alanı başına en çok 100 bölümlenmiş kuyruğa veya konuya izin vermektedir. Bölümlenmiş her sıra veya konu, ad alanı başına 10.000 varlık kotasına doğru sayılır (Premium katman için geçerli değildir).

## <a name="next-steps"></a>Sonraki adımlar

[AMQP 1.0 protokol kılavuzunda](service-bus-amqp-protocol-guide.md)AMQP 1.0 mesajlaşma belirtiminin temel kavramları hakkında bilgi edinin.

[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablepartitioning
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
