---
title: Azure Servis Veri İdaresi ileti sıralaması ve zaman damgaları | Microsoft Dokümanlar
description: Bu makalede, Azure Hizmet Veri Yolu iletilerinin sıralanması ve sıralanmasının (zaman damgalarıyla) nasıl korunup korunulduğu açıklanmaktadır.
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
ms.openlocfilehash: 54d774c00fa650cb9608f46cc07b9d899709eaa5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261664"
---
# <a name="message-sequencing-and-timestamps"></a>İleti sıralama ve zaman damgaları

Sıralama ve zaman damgalama, alınan veya göz lenen iletilerin [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber) ve [EnqueuedTimeUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc) özellikleri aracılığıyla tüm Hizmet Veri Hizmeti varlıkları ve yüzeyinde her zaman etkinleştirilen iki özelliktir.

İletilerin mutlak sırasının önemli olduğu ve/veya tüketicinin iletiler için güvenilir bir benzersiz tanımlayıcıya ihtiyaç duyduğu durumlarda, aracı, sıra veya konuya göre boşluksuz, artan sıra numarasıyla iletileri damgalar. Bölümlenmiş varlıklar için sıra numarası bölüme göre verilir.

**SequenceNumber** değeri, broker tarafından kabul edilip depolanır ve dahili tanımlayıcısı olarak işlev görür. Bölümlenmiş varlıklar için en üstteki 16 bit bölüm tanımlayıcısını yansıtır. 48/64-bit aralığı tükendiğinde sıra numaraları sıfıra yuvarlanır.

İstemciler tarafından değil, merkezi ve tarafsız bir otorite tarafından atandığından, sıra numarası benzersiz bir tanımlayıcı olarak güvenilebilir. Aynı zamanda varış gerçek sırasını temsil eder ve bir sipariş kriteri olarak bir zaman damgası daha kesindir, çünkü zaman damgaları aşırı mesaj oranlarında yeterince yüksek bir çözünürlüğe sahip olmayabilir ve broker'ın (ancak minimal) saat çarpıklıklarına maruz kalabilir düğümler arasındaki mülkiyet geçişleri.

Mutlak varış sırası önemlidir, örneğin, sınırlı sayıda sunulan malların ilk gelen-ilk hizmet esasına göre sunulduğu iş senaryolarında; konser bileti satışları bir örnektir.

Zaman damgalama özelliği, **ENqueuedTimeUtc** özelliğine yansıyan utc mesajının varış saatini doğru bir şekilde yakalayan tarafsız ve güvenilir bir otorite görevi görür. Bir iş senaryosu, bir iş öğesinin gece yarısından önce belirli bir tarihte gönderilip gönderilmediği gibi son tarihlere bağlıysa, ancak işlem sıra biriktirme listesinin çok gerisinde yse yararlıdır.

## <a name="scheduled-messages"></a>Zamanlanmış iletiler

Bir kuyruğa veya konu başlığına daha sonra işlenmek; örneğin belirli bir işi belirli bir zamanda sistem tarafından işlenmeye uygun hale gelmesi için zamanlamak üzere ileti gönderebilirsiniz. Bu özellik, güvenilir dağıtılmış zaman tabanlı zaman çizelgesi sağlar.

Zamanlanan iletiler tanımlanan enqueue saatine kadar kuyrukta gerçekleşmez. Bu tarihten önce, zamanlanan iletiler iptal edilebilir. İptal iletisini siler.

İletileri, normal gönderme yolu üzerinden ileti gönderirken [ZamanlamaZamanUtc](/dotnet/api/microsoft.azure.servicebus.message.scheduledenqueuetimeutc) özelliğini ayarlayarak veya [açıkça ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync#Microsoft_Azure_ServiceBus_QueueClient_ScheduleMessageAsync_Microsoft_Azure_ServiceBus_Message_System_DateTimeOffset_) API ile zamanlayabilirsiniz. İkincisi, daha sonra gerekirse zamanlanan iletiyi iptal etmek için kullanabileceğiniz zamanlanan iletinin **SequenceNumber'ını**hemen döndürür. Zamanlanmış iletiler ve sıra numaraları da [ileti tarama](message-browsing.md)kullanılarak keşfedilebilir.

Zamanlanan iletinin **SequenceNumber'ı** yalnızca ileti bu durumdayken geçerlidir. İleti etkin duruma geçerken, ileti geçerli anda sıraya alınmış gibi kuyruğa eklenir ve bu da yeni bir **SequenceNumber**atamayı içerir.

Özellik tek tek iletilere bağlı olduğundan ve iletiler yalnızca bir kez sıraya girebildiği için, Servis Veri Servisi iletiler için yinelenen zamanlamaları desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)