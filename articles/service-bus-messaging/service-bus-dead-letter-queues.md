---
title: Servis Otobüsü ölü harf kuyrukları | Microsoft Dokümanlar
description: Azure Hizmet Veri Servisi'ndeki ölü harf kuyruklarını açıklar. Servis Veri Hizmetleri kuyrukları ve konu abonelikleri, ölü harf sırası olarak adlandırılan ikincil bir alt sıra sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158919"
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Servis Veri Servisi ölü harf kuyruklarına genel bakış

Azure Hizmet Veri Hizmetleri sıraları ve konu abonelikleri, *ölü harf sırası* (DLQ) adı verilen ikincil bir alt sıra sağlar. Ölü harf sırasının açıkça oluşturulması gerekmez ve ana varlıktan bağımsız olarak silinemez veya başka bir şekilde yönetilemez.

Bu makalede, Servis Veri Servisi'ndeki ölü harf kuyrukları açıklanmaktadır. Tartışmanın çoğu GitHub'daki [Dead-Letter kuyrukları örneği](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) ile gösterilmiştir.
 
## <a name="the-dead-letter-queue"></a>Ölü harf sırası

Ölü harf sırasının amacı, herhangi bir alıcıya teslim edilemeyecek iletileri veya işlenemeyen iletileri tutmaktır. İletiler daha sonra DLQ'dan kaldırılabilir ve denetlenebilir. Bir uygulama, bir işleç yardımıyla sorunları düzeltebilir ve iletiyi yeniden gönderebilir, hata olduğu gerçeğini günlüğe kaydedebilir ve düzeltici eylemde bulunabilir. 

API ve protokol açısından bakıldığında, Iletilerin yalnızca ana varlığın ölü harf çalışması yoluyla gönderilebildiği dışında, DLQ çoğunlukla diğer kuyruğa benzer. Buna ek olarak, zaman-to-live gözlenmez ve ölü bir DLQ bir mesaj mektup olamaz. Ölü harf sırası, peek-lock teslimini ve işlem işlemlerini tam olarak destekler.

DLQ'nun otomatik temizliği yok. İletiler, dlq'dan açıkça alıp ölü harfli iletide [Tam()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) diye arayana kadar DLQ'da kalır.

## <a name="dlq-message-count"></a>DLQ ileti sayısı
Konu düzeyinde ölü harf kuyruğunda ileti sayısı elde etmek mümkün değildir. Bunun nedeni, Servis Veri Servisi iç hata atmadığı sürece iletilerin konu düzeyinde yer almamasıdır. Bunun yerine, bir gönderen bir konuya ileti gönderdiğinde, ileti milisaniyeler içinde konu aboneliklerine iletilir ve böylece artık konu düzeyinde bulunmaz. Bu nedenle, dlq konu için abonelik ile ilişkili iletileri görebilirsiniz. Aşağıdaki örnekte, **Service Bus Explorer** şu anda DLQ'da "test1" aboneliği için 62 ileti olduğunu gösterir. 

![DLQ ileti sayısı](./media/service-bus-dead-letter-queues/dead-letter-queue-message-count.png)

Ayrıca Azure CLI komutunu kullanarak DLQ iletilerinin [`az servicebus topic subscription show`](/cli/azure/servicebus/topic/subscription?view=azure-cli-latest#az-servicebus-topic-subscription-show)sayısını da alabilirsiniz: . 

## <a name="moving-messages-to-the-dlq"></a>İletileri DLQ'ya taşıma

Servis Veri Servisi'nde iletilerin ileti motorunun kendi içinden DLQ'ya itilmesine neden olan çeşitli etkinlikler vardır. Bir uygulama da açıkça DLQ iletileri taşıyabilirsiniz. 

İleti aracı tarafından taşındıkça, broker [mesajda DeadLetter](/dotnet/api/microsoft.azure.servicebus.queueclient.deadletterasync) yönteminin dahili sürümünü çağırdığı için iletiye iki özellik eklenir: `DeadLetterReason` ve `DeadLetterErrorDescription`.

Uygulamalar `DeadLetterReason` özellik için kendi kodlarını tanımlayabilir, ancak sistem aşağıdaki değerleri ayarlar.

| Koşul | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Her zaman |ÜstbilgiAşıAşıldı |Bu akış için boyut kotası aşıldı. |
| ! Konu Açıklaması.<br />EtkinleştirFiltrelemeİletileriÖncekiYayıncılık ve AbonelikAçıklaması.<br />EnableDeadLetteringOnFilterEvaluationExceptions |Özel durum. GetType(). Adı |Özel durum. İleti |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |İletinin süresi doldu ve teslim edilmeyenler sırasına eklendi. |
| SubscriptionDescription.RequiresSession |Oturum kimliği geçersizdir. |Oturumun etkin olduğu varlık, oturum tanımlayıcısı null olan bir iletiye izin vermiyor. |
| !ölü harf sırası | MaxTransferHopSayısı Aşıldı | Kuyruklar arasında iletme yaparken izin verilen maksimum atlama sayısı. Değer 4 olarak ayarlanır. |
| Uygulama açık ölü harf |Uygulama tarafından belirtilen |Uygulama tarafından belirtilen |

## <a name="exceeding-maxdeliverycount"></a>MaxDeliveryCount aşan

Kuyrukların ve aboneliklerin her birinde sırasıyla [queuedescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) ve [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.maxdeliverycount) özelliği vardır; varsayılan değer 10'dur. Bir ileti bir kilit altında teslim edildiğinde[(ReceiveMode.PeekLock),](/dotnet/api/microsoft.azure.servicebus.receivemode)ancak açıkça terk edilmiş veya kilidin süresi dolmuşsa, [Aracılı Mesaj.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) iletisi artmıştır. [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) [MaxDeliveryCount'u](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount)aştığında, ileti `MaxDeliveryCountExceeded` neden kodunu belirterek DLQ'ya taşınır.

Bu davranış devre dışı bırakılamaz, ancak [MaxDeliveryCount'ı](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) çok sayıda ayarlayabilirsiniz.

## <a name="exceeding-timetolive"></a>Aşan Zaman

[QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription) veya [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) özelliği **doğru** ayarlandığında (varsayılan **yanlıştır),** tüm süresi dolan iletiler `TTLExpiredException` DLQ'ya taşınır ve neden kodunu belirtir.

Süresi dolmuş iletiler yalnızca ana kuyruktan veya abonelikten en az bir etkin alıcı çekildiğinde temizlenir ve DLQ'ya taşınır; bu davranış tasarım gereğidir.

## <a name="errors-while-processing-subscription-rules"></a>Abonelik kurallarını işlerken hatalar

[SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) özelliği bir abonelik için etkinleştirildiğinde, bir aboneliğin SQL filtresi kuralı yürütürken oluşan hatalar rahatsız edici iletiyle birlikte DLQ'da yakalanır.

## <a name="application-level-dead-lettering"></a>Uygulama düzeyinde dead-lettering

Sistem tarafından sağlanan ölü harfler özelliklerine ek olarak, uygulamalar kabul edilemez iletileri açıkça reddetmek için DLQ'yu kullanabilir. Bunlar, herhangi bir tür sistem sorunu nedeniyle düzgün işlenemeyen iletiler, bozuk yük leri tutan iletiler veya ileti düzeyinde bir güvenlik düzeni kullanıldığında kimlik doğrulamada başarısız olan iletileri içerebilir.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>ForwardTo veya SendVia senaryolarında ölü harfler

İletiler aşağıdaki koşullar altında transfer ölü harf sıraya gönderilecektir:

- İleti, dörtten fazla sıradan veya [birbirine zincirlenmiş](service-bus-auto-forwarding.md)konular dan geçer.
- Hedef sıra veya konu devre dışı bırakılır veya silinir.
- Hedef sıra veya konu en büyük varlık boyutunu aşıyor.

Bu ölü harflerle yazılmış iletileri almak için [FormatTransferDeadletterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formattransferdeadletterpath) yardımcı programı yöntemini kullanarak bir alıcı oluşturabilirsiniz.

## <a name="example"></a>Örnek

Aşağıdaki kod snippet bir ileti alıcısı oluşturur. Ana sıra için alma döngüsünde kod, aracıdan hazır herhangi bir iletiyi anında döndürmesini veya sonuç alınamadan dönmesini isteyen [Iletiyi Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver)ile alır. Kod bir ileti alırsa, hemen terk eder ve bu `DeliveryCount`da 'nın .'ı Sistem iletiyi DLQ'ya taşırsa, ana sıra boşolur ve [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) **null**döndürürken döngü çıkar.

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

## <a name="path-to-the-dead-letter-queue"></a>Ölü harf kuyruğuna giden yol
Aşağıdaki sözdizimini kullanarak ölü harf sırana erişebilirsiniz:

```
<queue path>/$deadletterqueue
<topic path>/Subscriptions/<subscription path>/$deadletterqueue
```

.NET SDK kullanıyorsanız, SubscriptionClient.FormatDeadLetterPath() yöntemini kullanarak ölü harf kuyruğuna giden yolu alabilirsiniz. Bu yöntem, **/$DeadLetterQueue**ile konu adı/abonelik adı ve sonekleri alır.


## <a name="next-steps"></a>Sonraki adımlar

Servis Veri Servisi kuyrukları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Azure Kuyrukları ve Servis Veri Servisi kuyrukları karşılaştırıldığında](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

