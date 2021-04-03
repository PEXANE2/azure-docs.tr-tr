---
title: Azure Service Bus-iletiye göz atma
description: Göz atma ve göz atma Service Bus iletileri, bir Azure Service Bus istemcisinin bir kuyruktaki veya abonelikteki tüm iletileri numaralandırmasını sağlar.
ms.topic: article
ms.date: 11/11/2020
ms.openlocfilehash: c52c9c967d4eada1a931e188ed4d25f7691cfb91
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94553650"
---
# <a name="message-browsing"></a>İletilere göz atma

İletiye göz atma veya gözatma, bir Service Bus istemcisinin tanılama ve hata ayıklama amacıyla bir kuyruktaki veya abonelikteki tüm iletileri numaralandırmasını sağlar.

Bir kuyruktaki göz atma işlemi, yalnızca `Receive()` veya döngüsüyle anında alma için kullanılabilir olanları değil, kuyruktaki tüm iletileri döndürür `OnMessage()` . `State`Her iletinin özelliği, iletinin etkin (alınması için kullanılabilir), [ertelenmiş](message-deferral.md)veya [Zamanlanmış](message-sequencing.md)olduğunu bildirir. Bir abonelikteki göz atma işlemi, abonelik ileti günlüğündeki zamanlanmış iletiler dışındaki tüm iletileri döndürür. 

Tüketilen ve biten iletiler, zaman uyumsuz bir "çöp toplama" çalıştırması tarafından temizlenir. Bu adım, iletilerin süre dolduktan hemen sonra gerçekleşmeyebilir. Bu nedenle, `Peek` zaten kullanım süreniz geçen iletileri döndürebiliriz. Bu iletiler, bir sonraki sefer sırada veya abonelikte bir alma işlemi çağrıldığında kaldırılacak veya atılacak duruma getirilir. Ertelenmiş iletileri kuyruktan kurtarmaya çalışırken bu davranışı aklınızda bulundurun. Süresi biten bir ileti, göz atma tarafından döndürülse bile diğer yollarla düzenli alma için uygun değildir. Bu iletilerin döndürülmesi tasarıma göre yapılır çünkü Özet, günlüğün geçerli durumunu yansıtan bir tanılama aracıdır.

Göz atma ayrıca kilitli olan ve diğer alıcılar tarafından işlenmekte olan iletileri de döndürür. Ancak, Peek bağlantısı kesik bir anlık görüntü döndürdüğünden, iletinin kilit durumu atılamıyor iletilerinde gözlemlenemez. [Lockeduntilutc](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.lockeduntilutc) ve [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) özellikleri, uygulama bunları okumaya çalıştığında bir [InvalidOperationException](/dotnet/api/system.invalidoperationexception) oluşturur.

## <a name="peek-apis"></a>API 'Leri göz atma

[Peek/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) ve [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) yöntemleri .net ve Java istemci kitaplıklarında ve alıcı nesnelerinde bulunur: **MessageReceiver**, **messagesession**. Kuyruklar, abonelikler ve ilgili atılacak ileti kuyrukları üzerinde çalışmalar göz atın.

Tekrar tekrar çağrıldığında, **Peek** , sıradaki en düşük sıra numarasından en yükseğe doğru sırada veya abonelik günlüğündeki tüm iletileri numaralandırır. İletilerin son sırada alınamayacağını sıraya göre değil, mesajların sıraya alınma sırası vardır.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) birden çok ileti alır ve bunları bir sabit listesi olarak döndürür. Kullanılabilir bir ileti yoksa, numaralandırma nesnesi boş değildir, null değildir.

Ayrıca, başlatılacak bir [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) ile metodun aşırı yüklemesini kullanabilir ve daha sonra numaralandırmak için parametresiz yöntem aşırı yüklemesini çağırabilirsiniz. **PeekBatch** işlevleri equivalently, ancak her seferinde bir dizi ileti alır.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
