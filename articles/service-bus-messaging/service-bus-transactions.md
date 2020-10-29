---
title: Azure Service Bus içinde işlem işlemeye genel bakış
description: Bu makale, işlem işleme ve Azure Service Bus aracılığıyla gönderme özelliği için bir genel bakış sunar.
ms.topic: article
ms.date: 10/28/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 9162b8578fe4f48cc3740b38d9d84ffaa2f260de
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92927796"
---
# <a name="overview-of-service-bus-transaction-processing"></a>Service Bus işlem işlemeye genel bakış

Bu makale Microsoft Azure Service Bus işlem yeteneklerini açıklamaktadır. Tartışmanın çoğu [Service Bus örnekle AMQP işlemlerine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TransactionsAndSendVia/TransactionsAndSendVia/AMQPTransactionsSendVia)göre gösterilmiştir. Bu makale, işlem işleme ve Service Bus *aracılığıyla gönderme* özelliği hakkında genel bir bakış ile sınırlıdır, ancak Atomik işlemler örneği kapsamda daha geniş ve daha karmaşıktır.

## <a name="transactions-in-service-bus"></a>Service Bus işlemler

*İşlem* iki veya daha fazla işlemi bir *yürütme kapsamında* gruplandırır. Doğası gereği, bu tür bir işlem, belirli bir işlem grubuna ait tüm işlemlerin başarılı veya başarısız bir şekilde ortaklaşa bulunduğundan emin olmalıdır. Bu işlemler, genellikle *kararlılık* olarak anılan tek bir birim görevi görür.

Service Bus, işlem temelli bir ileti aracısıdır ve tüm iç işlemler için ileti depolarına karşı işlem bütünlüğü sağlar. İletilerin teslim [edilemeyen ileti sırasına](service-bus-dead-letter-queues.md) taşınması veya iletilerin varlıklar arasında [otomatik olarak iletilmesi](service-bus-auto-forwarding.md) gibi Service Bus içindeki iletilerin tüm aktarımları işlem yapar. Bu nedenle, Service Bus bir iletiyi kabul ediyorsa, zaten depolanmış ve bir sıra numarasıyla etiketlendi. Bundan sonra, Service Bus içindeki herhangi bir ileti aktarımı, varlıklar genelinde koordine edilen işlemlerdir ve bu, kayıp (kaynak başarılı ve hedef başarısız olur) veya ileti çoğaltma (kaynak başarısız ve hedef başarılı).

Service Bus, bir hareketin kapsamı içindeki işlemlerin (kuyruk, konu başlığı, abonelik gibi) tek bir mesajlaşma varlığına göre gruplanmasını destekler. Örneğin, bir işlem kapsamı içinden bir kuyruğa birkaç ileti gönderebilirsiniz ve iletiler yalnızca işlem başarıyla tamamlandığında kuyruğun günlüğüne kaydedilir.

## <a name="operations-within-a-transaction-scope"></a>İşlem kapsamı içindeki işlemler

Bir işlem kapsamı içinde gerçekleştirilebilecek işlemler aşağıdaki gibidir:

* **[Queueclient](/dotnet/api/microsoft.azure.servicebus.queueclient), [iletileyici](/dotnet/api/microsoft.azure.servicebus.core.messagesender), [topicclient](/dotnet/api/microsoft.azure.servicebus.topicclient)** : `Send` , `SendAsync` , `SendBatch` ,`SendBatchAsync`
* **[Brokeredmessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)** : `Complete` , `CompleteAsync` , `Abandon` , `AbandonAsync` , `Deadletter` , `DeadletterAsync` , `Defer` , `DeferAsync` , `RenewLock` , `RenewLockAsync` 

Uygulamanın [ReceiveMode. PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modunu kullanarak, bazı alma döngülerine veya bir [OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) geri çağırması ile veya yalnızca iletiyi işlemeye yönelik bir işlem kapsamını açtığından emin olduğu varsayıldığından alma işlemleri dahil değildir.

İletinin eğilimi (tam, Abandon, atılacak harf, erteleme), işlemin genel sonucunu ve bağımlı olduğunu bir şekilde ortaya çıkar.

## <a name="transfers-and-send-via"></a>Aktarımlar ve "gönderme aracılığıyla"

Bir kuyruktan veya konudan bir işlemciye işlem devreden Multipath ve daha sonra başka bir kuyruğa veya konuya yönelik işlemsel bir veri sağlamak için, Service Bus *aktarımları* destekler. Bir aktarım işleminde, bir gönderen öncelikle bir *Aktarım kuyruğuna veya konuya* bir ileti gönderir ve Aktarım kuyruğu veya konu başlığı, yeniden yönlendirme yeteneğinin bağımlı olduğu aynı güçlü aktarım uygulamasını kullanarak iletiyi amaçlanan hedef kuyruğuna veya konuya doğrudan taşımalıdır. İleti, Aktarım kuyruğu veya konunun tüketicileri için görünür hale gelmesi için aktarım kuyruğuna veya konunun günlüğüne hiçbir şekilde yürütülmedi.

Aktarım sırasının veya konusunun kendisi gönderenin giriş iletilerinin kaynağı olduğunda bu işlem yeteneğinin gücü görünür hale gelir. Diğer bir deyişle, Service Bus ileti, Aktarım kuyruğu veya konu başlığı altında ", bir atomik bir işlemde, giriş iletisinde tam (veya ertelenme ya da atılacak) işlemi gerçekleştirirken" iletiyi hedef sıraya veya konuya aktarabilir. 

### <a name="see-it-in-code"></a>Kodda gör

Bu tür aktarımları ayarlamak için, Aktarım kuyruğu aracılığıyla hedef kuyruğu hedefleyen bir ileti gönderici oluşturursunuz. Aynı sıradan iletiler çeken bir alıcıya da sahipsiniz. Örneğin:

```csharp
var connection = new ServiceBusConnection(connectionString);

var sender = new MessageSender(connection, QueueName);
var receiver = new MessageReceiver(connection, QueueName);
```

Daha sonra basit bir işlem, aşağıdaki örnekte olduğu gibi bu öğeleri kullanır. Tam örneği belirtmek için [GitHub 'da kaynak kodunu](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TransactionsAndSendVia/TransactionsAndSendVia/AMQPTransactionsSendVia)inceleyin:

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

## <a name="timeout"></a>Zaman aşımı
2 dakikadan sonra bir işlem zaman aşımına uğrar. İşlem süreölçeri, işlemdeki ilk işlem başladığında başlar. 

## <a name="next-steps"></a>Sonraki adımlar

Service Bus kuyrukları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus kuyruklarını kullanma](service-bus-dotnet-get-started-with-queues.md)
* [Oto Service Bus varlıkları tekrar yönlendirme ile zincirle](service-bus-auto-forwarding.md)
* [Oto ilet örneği](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Service Bus örnekle atomik Işlemler](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Azure kuyrukları ve Service Bus kuyrukları karşılaştırması](service-bus-azure-and-service-bus-queues-compared-contrasted.md)


