---
title: Azure Service Bus ileti sıralaması ve zaman damgaları | Microsoft Docs
description: Bu makalede, Azure Service Bus iletilerinin sıralama ve sıralamayı (zaman damgalarına) nasıl koruyabileceğiniz açıklanır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: fdb18802e576ad114fd3f783d5efd7bb826a5f94
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341171"
---
# <a name="message-sequencing-and-timestamps"></a>İleti sıralama ve zaman damgaları

Sıralama ve zaman damgalama, tüm Service Bus varlıklarda her zaman etkin olan ve alınan veya taranan iletilerin [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber) ve [Enqueuedtimeutc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc) özellikleri aracılığıyla yüzey olan iki özellikten oluşur.

İletilerin mutlak sırasının önemli olduğu ve/veya bir tüketicinin iletiler için güvenilir benzersiz bir tanımlayıcıya ihtiyacı olan bu durumlar için, aracı, bir boşluk içermeyen iletileri damgalarına ve sıra veya konuya göre artan sıra numarasına sahip olur. Bölümlenmiş varlıklar için sıra numarası bölüme göre verilir.

**SequenceNumber** değeri, aracı ve işlevleri tarafından iç tanımlayıcı olarak kabul edildiği ve depolandığı için bir iletiye atanan benzersiz bir 64 bitlik tamsayıdır. Bölümlenmiş varlıklarda en üstteki 16 bit, Bölüm tanımlayıcısını yansıtır. 48/64 bit Aralık tükendiğinde sıra numaraları sıfıra geçer.

Sıra numarası, istemcilere göre değil, merkezi ve bağımsız bir yetkili tarafından atandığı için benzersiz bir tanımlayıcı olarak güvenilir olabilir. Ayrıca, aynı zamanda gerçek bir zaman damgasına da sahiptir ve zaman damgalarının, yoğun ileti fiyatları üzerinde yüksek düzeyde çözünürlüğü olmaması ve bu durum, aracı sahipliğinin düğümler arasında geçiş yaptığı durumlarda (ancak en düşük düzeyde) saat çarpıklığı tabi olabilmesinden kaynaklanır.

Mutlak varış siparişi, örneğin, en son tedarik edilirken sınırlı sayıda sunulan malları ilk kez sunulan bir kez sunulan iş senaryolarında önemli, konser bilet satışları bir örnektir.

Zaman damgalama özelliği, bir iletinin, **Enqueuedtimeutc** ÖZELLIĞINDE yansıtılan UTC saatini doğru bir şekilde yakalayan, nötr ve güvenilir bir yetkili olarak davranır. Bir iş senaryosu, gece yarısından önce belirli bir tarihte gönderilip gönderilmediğine karşın işlem sıranın biriktirme listesinin en altında olduğundan, bu değer yararlı olur.

## <a name="scheduled-messages"></a>Zamanlanan iletiler

Bir kuyruğa veya konu başlığına daha sonra işlenmek; örneğin belirli bir işi belirli bir zamanda sistem tarafından işlenmeye uygun hale gelmesi için zamanlamak üzere ileti gönderebilirsiniz. Bu yetenek, güvenilir bir dağıtılmış zaman tabanlı Scheduler 'ı daha gerçekçi hale getirir.

Zamanlanan iletiler, tanımlanan sıraya alma zamanına kadar sırada bir şekilde çalışmaz. Bu süreden önce zamanlanan iletiler iptal edilebilir. İptal etme iletiyi siler.

Sıradan gönderme yoluyla bir ileti gönderirken ya da [Schedulemessageasync](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) API ile açık olarak [Scheduledenqueuetimeutc](/dotnet/api/microsoft.azure.servicebus.message.scheduledenqueuetimeutc) özelliğini ayarlayarak iletileri zamanlayabilirsiniz. İkinci olarak, daha sonra gerekirse zamanlanmış iletiyi iptal etmek için kullanabileceğiniz zamanlanmış iletinin **SequenceNumber**döndürür. Zamanlanan iletiler ve sıra numaraları [ileti tarama](message-browsing.md)kullanılarak da bulunabilir.

Zamanlanmış bir ileti için **SequenceNumber** yalnızca ileti bu durumda olduğunda geçerlidir. İleti etkin duruma geçiş yaparken, ileti geçerli anında sıraya alınmış gibi sıraya eklenir, bu da yeni bir **SequenceNumber**atamayı içerir.

Özelliği tek tek iletilere sabitlenmiş olduğundan ve mesajlar yalnızca bir kez sıraya alınmışsa Service Bus iletiler için yinelenen zamanlamaları desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)