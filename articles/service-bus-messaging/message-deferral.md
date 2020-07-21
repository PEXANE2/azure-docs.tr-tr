---
title: Azure Service Bus-ileti erteleme
description: Bu makalede Azure Service Bus iletilerinin teslimini erteleme açıklanır. İleti sıra veya abonelikte kalır ancak bir kenara ayrılır.
ms.topic: article
ms.date: 06/23/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 7c9ec55de24c97df3530d80deef55ed87be84077
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86511288"
---
# <a name="message-deferral"></a>İleti erteleme

Bir kuyruk veya abonelik istemcisi işlemek isteyen bir ileti aldığında, ancak uygulamanın içindeki özel koşullar nedeniyle şu anda mümkün olmadığı için, iletinin daha sonra "erteleme" seçeneği vardır. İleti sıra veya abonelikte kalır ancak bir kenara ayrılır.

Deferral, iş akışı işleme senaryoları için özel olarak oluşturulan bir özelliktir. İş akışı çerçeveleri belirli işlemler için belirli bir sırada işlenmesini gerektirebilir ve diğer iletiler tarafından bilgilendirilmiş önceki çalışmalarla birlikte, bazı alınan iletilerin işlenmesini erteleyebilir.

Basit bir tanım örneği, eşleşen satın alma siparişi mağazadan öne çıkan sisteme yayılmadan önce bir sistemde, bir dış ödeme sağlayıcısından ödeme bildiriminin bir sistemde göründüğü bir sipariş işleme sırasıdır. Bu durumda, karşılama sistemi, ilişkilendirilecek bir sipariş olana kadar ödeme bildirimini işlemeye erteleyebilirsiniz. Farklı kaynaklardaki iletilerin bir iş akışını ileriye doğru bir şekilde iletmekte olduğu buluşma senaryolarında gerçek zamanlı yürütme sırası gerçekten doğru olabilir, ancak sonuçları yansıtan iletiler sıra dışına çıkabilir.

Sonuç olarak, alma sırasındaki iletileri, işleme için bir ileti deposunda güvenli durumda bırakarak, bu iletileri işlenmek üzere yeniden sıralama konusunda yardımcı olur.

> [!NOTE]
> Ertelenmiş iletiler, [süreleri dolduktan sonra](./service-bus-dead-letter-queues.md#exceeding-timetolive)otomatik olarak atılacak ileti kuyruğuna taşınmaz. Bu davranış tasarıma göre yapılır.

## <a name="message-deferral-apis"></a>İleti erteleme API 'Leri

API, .NET Standard istemcisinde .NET Framework istemcisi, [MessageReceiver.](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) Defsilinsync ve Java istemcisinde [IMessageReceiver. ertele](/java/api/com.microsoft.azure.servicebus.imessagereceiver.defer?view=azure-java-stable) ya da [IMessageReceiver. Defsilinsync](/java/api/com.microsoft.azure.servicebus.imessagereceiver.deferasync?view=azure-java-stable) olan [brokeredmessage.](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.defer?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_Defer) erteleme veya [brokeredmessage. defsilinebilir](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deferasync?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeferAsync) Sync. 

Ertelenmiş iletiler, diğer tüm etkin iletilerle birlikte ana sırada kalır (bir alt sırada bulunan atılacak ileti iletilerinin aksine), ancak artık normal alma/ReceiveAsync işlevleri kullanılarak alınamazlar. Ertelenmiş iletiler, bir uygulama tarafından izlenmeleri kaybolursa [ileti göz atma](message-browsing.md) aracılığıyla bulunabilir.

Ertelenmiş bir ileti almak için, [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) onu erteler. Ertelenmiş bir iletinin sıra numarasını bilen herhangi bir alıcı, daha sonra iletiyi ile açıkça alabilir `Receive(sequenceNumber)` .

Bu iletiyi işlemeye yönelik belirli bir kaynak geçici olarak kullanılamadığından bir ileti işlenemezse, ileti işleme summarily askıya alınmamalıdır, bu iletiyi birkaç dakika için tarafa koymak için bir yol, [Zamanlanmış bir Iletideki](message-sequencing.md) **SequenceNumber** 'i birkaç dakika içinde yeniden almak ve zamanlanan ileti geldiğinde ertelenmiş iletiyi yeniden almak olacaktır. Bir ileti işleyicisi tüm işlemler için bir veritabanına bağımlıysa ve bu veritabanı geçici olarak kullanılamıyorsa, ertelemeal kullanmamalıdır, ancak veritabanı yeniden kullanılabilir olana kadar iletileri almayı askıya almamalıdır.


## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
