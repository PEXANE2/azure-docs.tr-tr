---
title: Azure Servis Veri Servisi - ileti tarama
description: Servis Veri Yolunda iletilere göz atın ve göz atın, bir Azure Servis Veri Yolundan istemcinin kuyrukta veya abonelikte bulunan tüm iletileri saymasını sağlar.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 6156557d10210535b287aa516070c0b5da416512
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77539374"
---
# <a name="message-browsing"></a>İletilere göz atma

İleti taraması veya gözetleme, bir Servis Veri Servisi istemcisi genellikle tanılama ve hata ayıklama amacıyla bir kuyruk veya abonelikte bulunan tüm iletileri sıralama olanağı sağlar.

Gözetleme işlemleri, yalnızca döngüyle veya `Receive()` `OnMessage()` döngüyle anında edinimi için kullanılabilir olan larla değil, sıra veya abonelik ileti günlüğünde bulunan tüm iletileri döndürür. Her `State` iletinin özelliği, iletinin etkin (alınabilecek kullanılabilir), [ertelenmiş](message-deferral.md)mi yoksa [zamanlanmış](message-sequencing.md)mı olduğunu söyler.

Tüketilen ve süresi dolan iletiler eşzamanlı bir "çöp toplama" çalışması yla temizlenir ve iletilerin süresi tam olarak ne zaman dolacak olması gerekmez ve bu nedenle, `Peek` bir alma işlemi bir sonraki sıraya veya abonelikte çağrıldığınızda kaldırılacak veya ölü harflerle yazılmış iletileri gerçekten döndürebilir.

Bu, kuyruktan ertelenmiş iletileri kurtarmaya çalışırken akılda tutulması gereken özellikle önemlidir. [Süresi Dolan AtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc#Microsoft_Azure_ServiceBus_Message_ExpiresAtUtc) anlık geçtiği bir ileti artık peek tarafından iade edilirken bile başka yollarla düzenli olarak geri alınamaz. Peek günlüğün geçerli durumunu yansıtan bir tanılama aracı olduğundan, bu iletileri döndürmek kasıtlıdır.

Peek ayrıca kilitlenen ve şu anda diğer alıcılar tarafından işlenen, ancak henüz tamamlanmamış iletileri döndürür. Ancak, Peek bağlantısı kesilmiş bir anlık görüntü döndürür, bir iletinin kilit durumu gözetlenen iletilerde gözlemlenemez ve Uygulama bunları okumaya çalıştığında [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.lockeduntilutc) ve [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) özellikleri Geçersiz [İşlem Özel Durum](/dotnet/api/system.invalidoperationexception) atar.

## <a name="peek-apis"></a>Peek API'ler

[Peek/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) ve [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) yöntemleri tüm .NET ve Java istemci kitaplıklarında ve tüm alıcı nesnelerinde bulunur: **MessageReceiver**, **MessageSession**. Peek tüm kuyruklarda, aboneliklerde ve bunların ölü harf kuyruklarında çalışır.

Peek yöntemi art arda çağrıldığında, sıra veya abonelik günlüğünde bulunan tüm iletileri sıra numarası sırasına göre, kullanılabilir en düşük sıra numarasından en yüksek sıraya numaralandırır. İletilerin sıralandığı sıradır ve iletilerin sonunda alınabileceği sıra değildir.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) birden çok ileti alır ve numaralandırma olarak döndürür. İleti yoksa, numaralandırma nesnesi boştur, null değil.

Ayrıca, başlatılan bir [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) ile yöntemin aşırı yüklenmesini sağlayabilir ve daha fazla numaralandırmak için parametresiz yöntemi aşırı yükleyebilirsiniz. **PeekBatch** eşit olarak çalışır, ancak bir dizi iletiyi aynı anda alır.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
