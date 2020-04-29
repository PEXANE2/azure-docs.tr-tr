---
title: Service Bus atılacak ileti sıraları | Microsoft Docs
description: Azure Service Bus içindeki atılacak ileti kuyruklarını açıklar. Service Bus kuyrukları ve konu abonelikleri, atılacak ileti sırası adlı ikincil bir alt sıra sağlar.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 68b2aa38-dba7-491a-9c26-0289bc15d397
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2020
ms.author: aschhab
ms.openlocfilehash: 9c1a0cb92fbaf98d25799ffb5a85e666e7c05f8c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80158919"
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Service Bus atılacak ileti sıralarına genel bakış

Azure Service Bus kuyrukları ve konu abonelikleri, *atılacak ileti sırası* (DLQ) adlı ikincil bir alt sıra sağlar. Atılacak mektup sırasının açık bir şekilde oluşturulması ve silinemediği ya da ana varlıktan bağımsız olarak yönetilebilmesi gerekmez.

Bu makalede Service Bus içindeki atılacak ileti sıraları açıklanmaktadır. Tartışmanın büyük bölümü GitHub 'daki [atılacak ileti sıraları örneğine](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) göre gösterilmiştir.
 
## <a name="the-dead-letter-queue"></a>Atılacak ileti sırası

Atılacak ileti sırasının amacı, hiçbir alıcıya teslim edilemeyen iletileri veya işlenemeyen iletileri tutmaktır. İletiler daha sonra DLQ 'dan kaldırılabilir ve incelenebilir. Bir uygulama, bir operatör yardımıyla, sorunları gidermenize ve iletiyi yeniden gönderebilecek, hatayı bir hata olduğunu günlüğe günlüğünden ve düzeltici bir işlem yapmanıza yardımcı olabilir. 

Bir API ve protokol perspektifinden, DLCı 'ler, yalnızca üst varlığın atılacak ileti işlemi yoluyla gönderilebileceği sürece, genellikle diğer bir sıraya benzer. Ayrıca, yaşam süresi gözlemlenmez ve bir DLQ 'dan bir iletiyi atılacak şekilde görüntüleyemezsiniz. Atılacak ileti kuyruğu, Peek-kilit dağıtımını ve işlem işlemlerini tamamen destekler.

DLCı 'nin otomatik olarak temizlenmesi yoktur. İletiler, DLCı 'lerden açıkça geri alınana ve atılacak ileti iletisinde [tam ()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) çağrısı yapana kadar DLQ 'da kalır.

## <a name="dlq-message-count"></a>DLQ ileti sayısı
Konu düzeyinde teslim edilemeyen ileti sırasındaki ileti sayısını almak mümkün değildir. Bunun nedeni, Service Bus bir iç hata oluşturulmadığı takdirde iletiler konu düzeyinde yer duymayın. Bunun yerine, gönderen bir konuya bir ileti gönderdiğinde, ileti milisaniye içinde konuya iletilir ve bu nedenle artık konu düzeyinde yer alır. Bu nedenle, konu başlığı için abonelikle ilişkili DLQ içindeki iletileri görebilirsiniz. Aşağıdaki örnekte, **Service Bus Explorer** "test1" ABONELIĞINE ait DLCI 'de şu anda 62 ileti olduğunu gösterir. 

![DLQ ileti sayısı](./media/service-bus-dead-letter-queues/dead-letter-queue-message-count.png)

Ayrıca, Azure CLı komutunu kullanarak DLQ iletilerinin sayısını alabilirsiniz: [`az servicebus topic subscription show`](/cli/azure/servicebus/topic/subscription?view=azure-cli-latest#az-servicebus-topic-subscription-show). 

## <a name="moving-messages-to-the-dlq"></a>İletileri DLQ 'a taşıma

Service Bus, iletilerin ileti altyapısı içinden DLCı 'ye itilmesine neden olan birkaç etkinlik vardır. Bir uygulama Ayrıca iletileri doğrudan DLCı 'ye taşıyabilir. 

İleti, aracı tarafından taşındığında, aracı ileti üzerinde [sahipsiz](/dotnet/api/microsoft.azure.servicebus.queueclient.deadletterasync) yöntemin iç sürümünü çağırdığı için iletiye iki özellik eklenir: `DeadLetterReason` ve. `DeadLetterErrorDescription`

Uygulamalar, `DeadLetterReason` özelliği için kendi kodlarını tanımlayabilir, ancak sistem aşağıdaki değerleri ayarlar.

| Koşul | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Her zaman |Headersizeaşıldı |Bu akış için boyut kotası aşıldı. |
| ! TopicDescription.<br />Enablefilteringiletibefoyeniden yayımlama ve SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |duruma. GetType (). Ada |duruma. İleti |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |İletinin süresi doldu ve teslim edilmeyenler sırasına eklendi. |
| SubscriptionDescription. RequiresSession |Oturum KIMLIĞI null. |Oturumun etkin olduğu varlık, oturum tanımlayıcısı null olan bir iletiye izin vermiyor. |
| ! atılacak ileti sırası | Maxtransferhopcountexceden | Kuyruklar arasında iletme sırasında izin verilen en fazla atlama sayısı. Değer 4 ' e ayarlanır. |
| Uygulama açık atılacak |Uygulama tarafından belirtilen |Uygulama tarafından belirtilen |

## <a name="exceeding-maxdeliverycount"></a>MaxDeliveryCount aşılıyor

Kuyruklar ve abonelikler sırasıyla bir [queuedescription. maxdeliverycount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) ve [Subscriptiondescription. maxdeliverycount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.maxdeliverycount) özelliğine sahiptir; Varsayılan değer 10 ' dur. Bir kilit ([ReceiveMode. PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode)) altına bir ileti teslim edildiğinde, ancak açıkça terk edildiğinde ya da kilidin süresi dolmuşsa, [Brokeredmessage. deliverycount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) değeri artırılır. [Deliverycount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) , [maxdeliverycount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount)değerini aştığında, ileti, `MaxDeliveryCountExceeded` neden kodunu belirterek DLQ öğesine taşınır.

Bu davranış devre dışı bırakılamaz, ancak [Maxdeliverycount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) değerini büyük bir sayıya ayarlayabilirsiniz.

## <a name="exceeding-timetolive"></a>TimeToLive aşma

[Queuedescription. EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription) veya [Subscriptiondescription. EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) özelliği **true** olarak ayarlandığında (varsayılan değer **false**ise), süresi dolan tüm iletiler DLQ 'a taşınır ve `TTLExpiredException` neden kodu belirlenir.

Süre dolabilecek iletiler yalnızca, ana kuyruktan veya abonelikten çeken en az bir etkin alıcı olduğunda DLCı 'ler için temizlenir ve taşınır. Bu davranış tasarıma göre yapılır.

## <a name="errors-while-processing-subscription-rules"></a>Abonelik kuralları işlenirken hatalar oluştu

Abonelik için [Subscriptiondescription. EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) özelliği etkinleştirildiğinde, BIR aboneliğin SQL filtre kuralı yürütülürken oluşan tüm hatalar, sorunlu iletiyle birlikte DLQ içinde yakalanır.

## <a name="application-level-dead-lettering"></a>Uygulama düzeyi atılacak

Sistem tarafından sunulan kullanım dışı kullanım özelliklerine ek olarak, uygulamalar kabul edilmeyen iletileri açıkça reddetmek için DLQ ' ı kullanabilir. Her türlü sistem sorunu, hatalı biçimlendirilmiş yükleri tutan mesajlar veya ileti düzeyindeki bazı güvenlik şemaları kullanılırken kimlik doğrulaması başarısız olan iletiler nedeniyle düzgün işlenemeyecek iletiler içerebilir.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>ForwardTo veya SendVia senaryolarında atılacak

İletiler aşağıdaki koşullarda Aktarım teslim edilemeyen ileti sırasına gönderilir:

- Bir ileti, [birbirine](service-bus-auto-forwarding.md)çok dört kuyruk veya konu başlığı üzerinden geçirilir.
- Hedef kuyruk veya konu devre dışı veya silinmiş.
- Hedef kuyruğu veya konu başlığı, en büyük varlık boyutunu aşıyor.

Bu kullanılmayan iletileri almak için [FormatTransferDeadletterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formattransferdeadletterpath) yardımcı programı yöntemini kullanarak bir alıcı oluşturabilirsiniz.

## <a name="example"></a>Örnek

Aşağıdaki kod parçacığı bir ileti alıcısı oluşturur. Ana sıranın Alım döngüsünde, kod [Receive (TimeSpan. Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver)ile iletiyi alır. Bu, aracıda, aracı açık bir şekilde kullanılabilir olan herhangi bir iletiyi anında döndürmesini veya sonuç olmadan dönmesini ister. Kod bir ileti alırsa, onu artıran `DeliveryCount`. Sistem iletiyi DLCı 'ye taşıdıktan sonra, [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) **null**döndürdüğünden, ana sıra boştur ve döngü çıkar.

```csharp
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="path-to-the-dead-letter-queue"></a>Atılacak ileti sırasının yolu
Aşağıdaki sözdizimini kullanarak atılacak ileti kuyruğuna erişebilirsiniz:

```
<queue path>/$deadletterqueue
<topic path>/Subscriptions/<subscription path>/$deadletterqueue
```

.NET SDK kullanıyorsanız, SubscriptionClient. FormatDeadLetterPath () yöntemini kullanarak atılacak ileti sırasının yolunu alabilirsiniz. Bu yöntem, **/$DeadLetterQueue**konu adını/abonelik adını ve soneklerini alır.


## <a name="next-steps"></a>Sonraki adımlar

Service Bus kuyrukları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Azure kuyrukları ve Service Bus kuyrukları karşılaştırması](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

