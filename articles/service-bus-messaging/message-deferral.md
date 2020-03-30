---
title: Azure Hizmet Veri Servisi - ileti erteleme
description: Bu makalede, Azure Hizmet Veri Servisi iletilerinin tesliminin nasıl erteleniği açıklanmaktadır. İleti sıra veya abonelikte kalır ancak bir kenara ayrılır.
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
ms.openlocfilehash: 5e32c461902c1e340c6cece22669a59847e660cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77538405"
---
# <a name="message-deferral"></a>İleti erteleme

Bir kuyruğa veya abonelik istemcisi işlemeye istekli olduğu, ancak uygulamanın içindeki özel koşullar nedeniyle şu anda işlemenin mümkün olmadığı bir ileti aldığında, iletinin daha sonraki bir noktaya "ertelenmesi" seçeneği vardır. İleti sıra veya abonelikte kalır ancak bir kenara ayrılır.

Erteleme, iş akışı işleme senaryoları için özel olarak oluşturulan bir özelliktir. İş akışı çerçeveleri belirli bir sırada işlenmesi ni gerektirebilir ve alınan bazı iletilerin, diğer iletiler tarafından bildirilmiş olan önceden öngörülen çalışma tamamlanana kadar işlenmesini ertelemek zorunda kalabilir.

Basit bir açıklayıcı örnek, eşleşen satınalma siparişi mağaza önünden yerine getirme sistemine yayılmadan önce harici bir ödeme sağlayıcısından gelen ödeme bildiriminin sistemde göründüğü bir sipariş işleme dizisidir. Bu durumda, yerine getirme sistemi ödeme bildiriminin işlenmesini, ilişkilendirilecek bir sipariş gelene kadar erteleyebilir. Farklı kaynaklardan gelen iletilerin iş akışını ileriye yönlendirdiği randevu senaryolarında, gerçek zamanlı yürütme emri gerçekten doğru olabilir, ancak sonuçları yansıtan iletiler bozuk olabilir.

Sonuç olarak, gelen iletileri varış emrinden işlenebilir bir sıraya yeniden sıralamaya yönelik erteleme yardımcıları, bu iletileri işlemenin ertelenmesi gereken ileti deposunda güvenli bir şekilde bırakır.

## <a name="message-deferral-apis"></a>İleti erteleme API'leri

API [BrokeredMessage.Defer](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) veya [BrokeredMessage.DeferAsync](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync) .NET Framework istemcisinde, [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) .NET Standart istemcisinde ve [IMessageReceiver.defer](/java/api/com.microsoft.azure.servicebus.imessagereceiver.defer?view=azure-java-stable) veya [IMessageReceiver.deferAsync](/java/api/com.microsoft.azure.servicebus.imessagereceiver.deferasync?view=azure-java-stable) Java istemcisinde. 

Ertelenmiş iletiler diğer tüm etkin iletilerle birlikte ana sırada kalır (alt sırada yaşayan ölü harfli iletilerin aksine), ancak artık normal Receive/ReceiveAsync işlevleri kullanılarak alınamaz. Ertelenmiş iletiler, bir uygulama nın izini kaybederse [ileti tarama](message-browsing.md) yoluyla bulunabilir.

Ertelenmiş bir iletiyi almak için, sahibi [SequenceNumber'ı](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) erteler gibi hatırlamaktan sorumludur. Ertelenmiş iletinin sıra numarasını bilen herhangi bir alıcı daha sonra `Receive(sequenceNumber)`ileaçık olarak ' ile ileilgili olarak alabilir.

İletiyi işlemek için belirli bir kaynak geçici olarak kullanılamadığından ancak ileti işleme nin kısa bir süre askıya alınmaması gerektiğinden bir ileti işlenemezse, bu iletiyi birkaç dakika lığına kenara koymanın bir yolu, birkaç dakika içinde yayınlanacak [zamanlanmış](message-sequencing.md) iletide **SequenceNumber'ı** hatırlamak ve zamanlanan ileti geldiğinde ertelenen iletiyi yeniden almaktır. İleti işleyicisi tüm işlemler için bir veritabanına bağlıysa ve bu veritabanı geçici olarak kullanılamıyorsa, erteleme kullanmamalı, veritabanı yeniden kullanılabilir duruma gelene kadar iletileri tamamen askıya almalıdır.


## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
