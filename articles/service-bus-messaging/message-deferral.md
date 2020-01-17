---
title: Azure Service Bus ileti erteleme | Microsoft Docs
description: Service Bus iletilerinin teslimini erteleyin
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
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: fc7e40661ae345412eb0336322599616dc89d6c4
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122194"
---
# <a name="message-deferral"></a>İleti erteleme

Bir kuyruk veya abonelik istemcisi işlemek isteyen bir ileti aldığında, ancak uygulamanın içindeki özel koşullar nedeniyle şu anda mümkün olmadığı için, iletinin daha sonra "erteleme" seçeneği vardır. İleti sıra veya abonelikte kalır ancak bir kenara ayrılır.

Deferral, iş akışı işleme senaryoları için özel olarak oluşturulan bir özelliktir. İş akışı çerçeveleri belirli işlemler için belirli bir sırada işlenmesini gerektirebilir ve diğer iletiler tarafından bilgilendirilmiş önceki çalışmalarla birlikte, bazı alınan iletilerin işlenmesini erteleyebilir.

Basit bir tanım örneği, eşleşen satın alma siparişi mağazadan öne çıkan sisteme yayılmadan önce bir sistemde, bir dış ödeme sağlayıcısından ödeme bildiriminin bir sistemde göründüğü bir sipariş işleme sırasıdır. Bu durumda, karşılama sistemi, ilişkilendirilecek bir sipariş olana kadar ödeme bildirimini işlemeye erteleyebilirsiniz. Farklı kaynaklardaki iletilerin bir iş akışını ileriye doğru bir şekilde iletmekte olduğu buluşma senaryolarında gerçek zamanlı yürütme sırası gerçekten doğru olabilir, ancak sonuçları yansıtan iletiler sıra dışına çıkabilir.

Sonuç olarak, alma sırasındaki iletileri, işleme için bir ileti deposunda güvenli durumda bırakarak, bu iletileri işlenmek üzere yeniden sıralama konusunda yardımcı olur.

## <a name="message-deferral-apis"></a>İleti erteleme API 'Leri

API, .NET Standard istemcisinde .NET Framework istemcisi, [MessageReceiver.](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) Defsilinsync ve Java istemcisinde [IMessageReceiver. ertele](/java/api/com.microsoft.azure.servicebus.imessagereceiver.defer?view=azure-java-stable) ya da [IMessageReceiver. Defsilinsync](/java/api/com.microsoft.azure.servicebus.imessagereceiver.deferasync?view=azure-java-stable) olan [brokeredmessage.](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) erteleme veya [brokeredmessage. defsilinebilir](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync) Sync. 

Ertelenmiş iletiler, diğer tüm etkin iletilerle birlikte ana sırada kalır (bir alt sırada bulunan atılacak ileti iletilerinin aksine), ancak artık normal alma/ReceiveAsync işlevleri kullanılarak alınamazlar. Ertelenmiş iletiler, bir uygulama tarafından izlenmeleri kaybolursa [ileti göz atma](message-browsing.md) aracılığıyla bulunabilir.

Ertelenmiş bir ileti almak için, [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) onu erteler. Ertelenmiş bir iletinin sıra numarasını bilen herhangi bir alıcı daha sonra `Receive(sequenceNumber)`ile iletiyi açıkça alabilir. Kuyruklar için [Queueclient](/dotnet/api/microsoft.servicebus.messaging.queueclient)' ı kullanabilirsiniz, konu abonelikleri [subscriptionclient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient)'ı kullanır.

Bu iletiyi işlemeye yönelik belirli bir kaynak geçici olarak kullanılamadığından bir ileti işlenemezse, ileti işleme summarily askıya alınmamalıdır, bu iletiyi birkaç dakika için tarafa koymak için bir yol, [Zamanlanmış bir Iletideki](message-sequencing.md) **SequenceNumber** 'i birkaç dakika içinde yeniden almak ve zamanlanan ileti geldiğinde ertelenmiş iletiyi yeniden almak olacaktır. Bir ileti işleyicisi tüm işlemler için bir veritabanına bağımlıysa ve bu veritabanı geçici olarak kullanılamıyorsa, ertelemeal kullanmamalıdır, ancak veritabanı yeniden kullanılabilir olana kadar iletileri almayı askıya almamalıdır.


## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
