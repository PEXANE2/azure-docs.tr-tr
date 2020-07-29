---
title: Azure Service Bus-iletiye göz atma
description: Göz atma ve göz atma Service Bus iletilerinde Azure Service Bus istemcinin bir kuyrukta veya abonelikte bulunan tüm iletileri listeleyebilmesine olanak sağlar.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 0f2d4ed1225aef4c28a5f3d841669c2e3122ba10
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341240"
---
# <a name="message-browsing"></a>İletilere göz atma

İletiye göz atma veya gözatma, bir Service Bus istemcisinin, genellikle tanılama ve hata ayıklama amacıyla bir kuyrukta veya abonelikte bulunan tüm iletileri listeleyebilmesine olanak sağlar.

Göz atma işlemleri, yalnızca `Receive()` veya döngüsüyle anında alma için kullanılabilir olan, kuyrukta veya abonelik ileti günlüğünde bulunan tüm iletileri döndürür `OnMessage()` . `State`Her iletinin özelliği, iletinin etkin (alınması için kullanılabilir), [ertelenmiş](message-deferral.md)veya [Zamanlanmış](message-sequencing.md)olduğunu bildirir.

Tüketilen ve sona ermeyen iletiler zaman uyumsuz bir "çöp toplama" çalıştırması tarafından temizlenir ve iletilerin süreleri dolduğunda tam olarak zaman aşımına uğradı ve bu nedenle `Peek` kuyrukta veya abonelikte bir alma işlemi başlatıldığında kaldırılacak veya atılacak duruma getirilir.

Bu özellikle ertelenmiş iletileri kuyruktan kurtarmaya çalışırken göz önünde bulundurmanız önemlidir. [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc#Microsoft_Azure_ServiceBus_Message_ExpiresAtUtc) Instant 'ın geçirildiği bir Ileti, göz atma tarafından döndürülse bile diğer yollarla düzenli alma için artık uygun değildir. Peek, günlüğün geçerli durumunu yansıtan bir tanılama aracı olduğundan, bu iletilerin döndürülmesi bilinçli.

Göz atma ayrıca kilitli olan ve şu anda diğer alıcılar tarafından işlenmekte olan ancak henüz tamamlanmamış iletileri döndürür. Ancak, Peek bağlantısı kesik bir anlık görüntü döndürdüğünden, iletinin kilit durumu atılamıyor iletilerinde gözlemlenemez ve [Lockeduntilutc](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.lockeduntilutc) ve [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) özellikleri uygulama bunları okumaya çalıştığında bir [InvalidOperationException](/dotnet/api/system.invalidoperationexception) oluşturur.

## <a name="peek-apis"></a>API 'Leri göz atma

[Peek/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) ve [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) yöntemleri tüm .net ve Java istemci kitaplıklarında ve tüm alıcı nesnelerinde bulunur: **MessageReceiver**, **messagesession**. Tüm kuyruklar ve aboneliklerde ve ilgili atılacak iletiler sıralarında göz atın.

Tekrar tekrar çağrıldığında, Peek yöntemi kuyruk veya abonelik günlüğünde bulunan tüm iletileri sıra numarası sırasıyla, en düşük kullanılabilir sıra numarasından en yükseğe sıralar. Bu, iletilerin sıraya alınma sırası ve iletilerin sonunda ne sırada alınamayacağını gösteren bir sıradır.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) birden çok ileti alır ve bunları bir sabit listesi olarak döndürür. Kullanılabilir bir ileti yoksa, numaralandırma nesnesi boş değildir, null değildir.

Ayrıca, başlatılacak bir [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) ile metodun aşırı yüklemesini temel alabilir ve daha sonra numaralandırmak için parametresiz yöntem aşırı yüklemesini çağırabilirsiniz. **PeekBatch** işlevleri equivalently, ancak her seferinde bir dizi ileti alır.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
