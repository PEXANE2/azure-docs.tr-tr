---
title: Azure Service Bus-iletiye göz atma
description: Göz atma ve göz atma Service Bus iletileri, bir Azure Service Bus istemcisinin bir kuyruktaki veya abonelikteki tüm iletileri numaralandırmasını sağlar.
ms.topic: article
ms.date: 03/29/2021
ms.openlocfilehash: f4943685f03eccb1c3b8da079973cf083bdcc416
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106090316"
---
# <a name="message-browsing"></a>İletilere göz atma

İletiye göz atma veya gözatma, bir Service Bus istemcisinin tanılama ve hata ayıklama amacıyla bir kuyruktaki veya abonelikteki tüm iletileri numaralandırmasını sağlar.

**Kuyruktaki** göz atma işlemi, yalnızca anında alım için mevcut olanları değil, kuyruktaki tüm iletileri döndürür. Bir **abonelikteki** göz atma işlemi, abonelik ileti günlüğündeki zamanlanmış iletiler dışındaki tüm iletileri döndürür. 

Tüketilen ve biten iletiler, zaman uyumsuz bir "çöp toplama" çalıştırması tarafından temizlenir. Bu adım, iletilerin süre dolduktan hemen sonra gerçekleşmeyebilir. Bu nedenle, bir göz atma işleminin zaten kullanım süreniz geçen iletileri döndürebiliriz. Bu iletiler, bir sonraki sefer sırada veya abonelikte bir alma işlemi çağrıldığında kaldırılacak veya atılacak duruma getirilir. Ertelenmiş iletileri kuyruktan kurtarmaya çalışırken bu davranışı aklınızda bulundurun. Süresi biten bir ileti, göz atma tarafından döndürülse bile diğer yollarla düzenli alma için uygun değildir. Bu iletilerin döndürülmesi tasarıma göre yapılır çünkü Özet, günlüğün geçerli durumunu yansıtan bir tanılama aracıdır.

Göz atma ayrıca kilitli olan ve diğer alıcılar tarafından işlenmekte olan iletileri de döndürür. Ancak, Peek bağlantısı kesik bir anlık görüntü döndürdüğünden, iletinin kilit durumu atılamıyor iletilerinde gözlemlenemez.

## <a name="peek-apis"></a>API 'Leri göz atma
## <a name="azuremessagingservicebus"></a>[Azure. Messaging. ServiceBus](#tab/dotnet)
[PeekMessageAsync](/dotnet/api/azure.messaging.servicebus.servicebusreceiver.peekmessageasync) ve [PeekMessagesAsync](/dotnet/api/azure.messaging.servicebus.servicebusreceiver.peekmessagesasync) yöntemleri alıcı nesnelerinde mevcuttur: `ServiceBusReceiver` , `ServiceBusSessionReceiver` . Kuyruklar, abonelikler ve ilgili atılacak ileti kuyrukları üzerinde çalışmalar göz atın.

Tekrar tekrar çağrıldığında, `PeekMessageAsync` kuyruktaki veya abonelik günlüğündeki tüm iletileri sırasıyla en düşük kullanılabilir sıra numarasından en yükseğe sıralar. İletilerin son sırada alınamayacağını sıraya göre değil, mesajların sıraya alınma sırası vardır.
PeekMessagesAsync birden çok ileti alır ve bunları bir sabit listesi olarak döndürür. Kullanılabilir bir ileti yoksa, numaralandırma nesnesi boş değildir, null değildir.

Ayrıca [fromSequenceNumber](/dotnet/api/microsoft.servicebus.messaging.eventposition.fromsequencenumber) parametresini başlatılacak bir SequenceNumber ile doldurabilir ve daha sonra Numaralandırılacak parametreyi belirtmeden yöntemi yeniden çağırabilirsiniz. `PeekMessagesAsync` işlevler equivalently, ancak tek seferde bir dizi ileti alır.


## <a name="microsoftazureservicebus"></a>[Microsoft. Azure. ServiceBus](#tab/dotnetold)
[Peek/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) ve [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) yöntemleri alıcı nesnelerinde mevcuttur: `MessageReceiver` , `MessageSession` . Kuyruklar, abonelikler ve ilgili atılacak ileti kuyrukları üzerinde çalışmalar göz atın.

Tekrar tekrar çağrıldığında, `Peek` kuyruktaki veya abonelik günlüğündeki tüm iletileri sırasıyla en düşük kullanılabilir sıra numarasından en yükseğe sıralar. İletilerin son sırada alınamayacağını sıraya göre değil, mesajların sıraya alınma sırası vardır.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) birden çok ileti alır ve bunları bir sabit listesi olarak döndürür. Kullanılabilir bir ileti yoksa, numaralandırma nesnesi boş değildir, null değildir.

Ayrıca, başlatılacak bir [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) ile metodun aşırı yüklemesini kullanabilir ve daha sonra numaralandırmak için parametresiz yöntem aşırı yüklemesini çağırabilirsiniz. **PeekBatch** işlevleri equivalently, ancak her seferinde bir dizi ileti alır.


---

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
