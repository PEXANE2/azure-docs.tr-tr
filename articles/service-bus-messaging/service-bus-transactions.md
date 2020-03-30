---
title: Azure Hizmet Veri Tos'unda işlem işlemeye genel bakış
description: Bu makale, Azure Hizmet Veri Tos'undaki işlem işleme ve gönderme özelliğine genel bir bakış sağlar.
services: service-bus-messaging
documentationcenter: .net
author: axisc
editor: spelluru
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2020
ms.author: aschhab
ms.openlocfilehash: 22744ecbced40b3195f4d047227b1e2a37228102
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260910"
---
# <a name="overview-of-service-bus-transaction-processing"></a>Servis Veri Servisi işlem işlemlerine genel bakış

Bu makalede, Microsoft Azure Hizmet Veri Servisi'nin işlem yetenekleri anlatılmaktadır. Tartışmanın çoğu [Hizmet Veri Servisi örneği ile AMQP İşlemleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TransactionsAndSendVia/TransactionsAndSendVia/AMQPTransactionsSendVia)ile gösterilmiştir. Bu makale, Atomik İşlemler örneğinin kapsamı daha geniş ve daha karmaşık ken, işlem işleme ve Hizmet Veri Servisi özelliği *üzerinden gönderme* özelliğine genel bir bakışla sınırlıdır.

## <a name="transactions-in-service-bus"></a>Servis Otobüsünde İşlemler

Bir *hareket,* iki veya daha fazla işlemi yürütme *kapsamına*gruplatır. Doğası gereği, bu tür bir işlem, belirli bir operasyon grubuna ait tüm işlemlerin birlikte başarılı veya başarısız olmasını sağlamalıdır. Bu bağlamda işlemler genellikle *atomiklik*olarak adlandırılır bir birim olarak hareket.

Service Bus bir işlem iletisi aracısýdýr ve ileti depolarýna karýdaki tüm iúlemler için işlem bütünlüğü sağlýklýksýný sa İletileri [ölü harf kuyruğuna](service-bus-dead-letter-queues.md) taşıma veya varlıklar arasında iletilerin otomatik [olarak iletilmesi](service-bus-auto-forwarding.md) gibi Hizmet Veri Kurumu içindeki iletilerin tüm aktarımları işlemseldir. Bu nedenle, Servis Veri Servisi bir iletiyi kabul ederse, ileti zaten depolanmış ve bir sıra numarasıyla etiketlenmiştir. Bundan sonra, Hizmet Veri Yolu içindeki tüm ileti aktarımları varlıklar arasında eşgüdümlü işlemlerdir ve ne kayıplara (kaynak başarılı ve hedef başarısız olur) ne de iletinin çoğaltılmasına (kaynak başarısız olur ve hedef başarılı olur) yol açmaz.

Service Bus, bir hareketin kapsamı içindeki işlemlerin (kuyruk, konu başlığı, abonelik gibi) tek bir mesajlaşma varlığına göre gruplanmasını destekler. Örneğin, bir işlem kapsamı içinden bir kuyruğa birkaç ileti gönderebilirsiniz ve iletiler yalnızca işlem başarıyla tamamlandığında sıranın günlüğüne kaydedilir.

## <a name="operations-within-a-transaction-scope"></a>İşlem kapsamındaki işlemler

İşlem kapsamında gerçekleştirilebilecek işlemler aşağıdaki gibidir:

* ** [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient), [MessageSender](/dotnet/api/microsoft.azure.servicebus.core.messagesender), [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient)**: Gönder, SendAsync, SendBatch, SendBatchAsync 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: Komple, CompleteAsync, Abandon, AbandonAsync, Deadletter, DeadletterAsync, Erteleme, DeferAsync, RenewLock, RenewLockAsync 

Uygulamanın [ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modunu kullanarak ileti ler aldığı varsayıldığından, bazı alma döngüsü nde veya [OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) geri aramasıyla ileti leri aldığı ve ancak daha sonra iletiyi işlemek için bir işlem kapsamı açtığı varsayıldığı için alma işlemleri dahil edilmez.

İletinin (tam, terk, ölü harf, erteleme) devri, işlemin genel sonucunun kapsamı içinde ve bağlı olarak gerçekleşir.

## <a name="transfers-and-send-via"></a>Transferler ve "üzerinden göndermek"

Bir kuyruktan işlemciye ve ardından başka bir kuyruğa veri işlemini etkinleştirmek için Servis Veri Kurumu *aktarımları*destekler. Aktarım işleminde, gönderen önce bir *aktarım kuyruğuna*ileti gönderir ve aktarım sırası otomatik iletme özelliğinin dayandığı aynı sağlam aktarım uygulamasını kullanarak iletiyi hemen hedeflenen hedef sıraya taşır. İleti, aktarım sırasının tüketicileri için görünür hale gelir şekilde aktarım sırasının günlüğüne asla bağlanmaz.

Bu işlem özelliğinin gücü, aktarım sırasının kendisi gönderenin giriş iletilerinin kaynağı olduğunda görünür hale gelir. Başka bir deyişle, Servis Veri Servisi, giriş iletisi üzerinde tüm atomik işlemde tam (veya erteleme veya ölü harf) işlemi gerçekleştirirken, iletiyi "geçiş kuyruğu üzerinden" hedef sıraya aktarabilir. 

### <a name="see-it-in-code"></a>Kodda görün

Bu tür aktarımları ayarlamak için, aktarım kuyruğu üzerinden hedef sırasını hedefleyen bir ileti gönderen oluşturursunuz. Ayrıca, aynı sıradan iletiçeken bir alıcınız da vardır. Örnek:

```csharp
var connection = new ServiceBusConnection(connectionString);

var sender = new MessageSender(connection, QueueName);
var receiver = new MessageReceiver(connection, QueueName);
```

Basit bir işlem daha sonra aşağıdaki örnekte olduğu gibi bu öğeleri kullanır. Tam örneği görmek için [GitHub'daki kaynak koduna](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TransactionsAndSendVia/TransactionsAndSendVia/AMQPTransactionsSendVia)bakın:

```csharp
var receivedMessage = await receiver.ReceiveAsync();

using (var ts = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
{
    try
    {
        // do some processing
        if (receivedMessage != null)
            await receiver.CompleteAsync(receivedMessage.SystemProperties.LockToken);

        var myMsgBody = new MyMessage
        {
            Name = "Some name",
            Address = "Some street address",
            ZipCode = "Some zip code"
        };

        // send message
        var message = myMsgBody.AsMessage();
        await sender.SendAsync(message).ConfigureAwait(false);
        Console.WriteLine("Message has been sent");

        // complete the transaction
        ts.Complete();
    }
    catch (Exception ex)
    {
        // This rolls back send and complete in case an exception happens
        ts.Dispose();
        Console.WriteLine(ex.ToString());
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Servis Veri Servisi kuyrukları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus kuyruklarını kullanma](service-bus-dotnet-get-started-with-queues.md)
* [Otomatik yönlendirme ile Zincirleme Servis Veri Günü varlıkları](service-bus-auto-forwarding.md)
* [Otomatik yönlendirme örneği](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Servis Veri Otobüsü örneği ile Atomik İşlemler](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Azure Kuyrukları ve Servis Veri Servisi kuyrukları karşılaştırıldığında](service-bus-azure-and-service-bus-queues-compared-contrasted.md)


