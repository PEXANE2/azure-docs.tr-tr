---
title: Azure Servis Veri Servisi ön alma iletileri | Microsoft Dokümanlar
description: Azure Hizmet Veri Hizmeti Veri Hizmeti iletilerini önceden getirerek performansı artırın. İletiler, başvuru isteklerinden önce yerel geri alma için hazır dır.
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
ms.openlocfilehash: 80717ab940d27e9bf108b3740309bcd7d71668fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760666"
---
# <a name="prefetch-azure-service-bus-messages"></a>Azure Hizmet Veri Gönderi iletilerini önceden getirme

*Prefetch,* resmi Hizmet Veri Gönderi istemcilerinden herhangi birinde etkinleştirildiğinde, alıcı, uygulamanın başlangıçta istediği iletinin ötesinde, [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) sınırına kadar sessizce daha fazla ileti edinir.

Bu nedenle, tek bir ilk [Al](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive) veya [ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) çağrısı, kullanılabilir olduğu anda döndürülen anında tüketim için bir ileti alır. İstemci daha sonra ön alma arabelleği doldurmak için arka planda başka iletiler kazanır.

## <a name="enable-prefetch"></a>Ön getirmeyi etkinleştirme

.NET ile, **MessageReceiver,** **QueueClient**veya **SubscriptionClient'ın** [PrefetchCount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) özelliğini sıfırdan büyük bir sayıya ayarlayarak Prefetch özelliğini etkinleştirin. Değeri sıfıra ayarlamak ön getirmeyi kapatır.

Bu bağlamlarda etkisini görmek için bu ayarı Kolayca [QueuesGettingStarted](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/QueuesGettingStarted) veya [ReceiveLoop](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ReceiveLoop) örneklerinin ayarlarının alma tarafına ekleyebilirsiniz.

İletiler ön alma arabelleğinde kullanılabilir ken, sonraki/**ReceiveAsync** çağrıları arabellekten hemen yerine getirilir ve alan kullanılabilir hale geldikçe arabellek arka planda doldurulur. **Receive** Teslim için kullanılabilir ileti yoksa, alma işlemi arabelleği boşaltır ve beklendiği gibi bekler veya engeller.

Prefetch, [OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) ve [OnMessageAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessageasync) API'lerinde de aynı şekilde çalışır.

## <a name="if-it-is-faster-why-is-prefetch-not-the-default-option"></a>Daha hızlıysa, Prefetch neden varsayılan seçenek değil?

Prefetch, uygulama nın istediği zaman ve öncesinde yerel geri alma için hazır bir ileti oluşturarak ileti akışını hızlandırAr. Bu iş elde edilen kazanç, uygulama yazarının açıkça yapması gereken bir takasın sonucudur:

[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) alma modu ile, önalma arabelleğinde edinilen tüm iletiler artık kuyrukta kullanılamaz ve yalnızca **ReceiveAsync**/veya **OnMessage**/**OnMessageAsync** API'leri aracılığıyla uygulamaya alınana kadar bellek önalma arabelleğinde kalır.**ReceiveAsync** İletiler uygulamaya alınmadan önce uygulama sona ererse, bu iletiler geri alınamaz şekilde kaybolur.

[PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) alma modunda, Prefetch arabellesine getirilen iletiler kilitli durumda arabelleğe alınır ve kilit işlemesi için zaman aşım saatine sahip olur. Ön alma arabelleği büyükse ve işlem o kadar uzun sürüyorsa, ileti kilitleri ön alma arabelleğinde ikamet ederken veya uygulama iletiyi işlerken bile sona eriyorsa, uygulamanın işlemesi için bazı kafa karıştırıcı olaylar olabilir.

Uygulama, süresi dolmuş veya süresi dolan bir kilit içeren bir ileti edinebilir. Bu nedenle, uygulama iletiyi işleyebilir, ancak daha sonra kilit süresi dolması nedeniyle bu iletiyi tamamlayamayacağını görebilirsiniz. Uygulama [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.lockeduntilutc) özelliğini kontrol edebilir (broker ve yerel makine saati arasında saat çarpıklık tabidir). İleti kilidi süresi dolmuşsa, uygulama iletiyi yok saymalıdır; ileti üzerinde veya iletiyle birlikte API çağrısı yapılmaması gerekir. İletinin süresi dolmamışsa ancak son kullanma tarihi yakınsa, kilit, iletiyi arayarak başka bir varsayılan kilit süresi tarafından yenilenebilir ve [uzatılabilir. Yenileme Kilidi()](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_)

Kilit önalma arabelleğinde sessizce sona ererse, ileti terk edilmiş olarak kabul edilir ve yeniden kuyruktan alınmak üzere kullanılabilir hale getirilir. Bu prefetch arabelleği içine getirilen neden olabilir; sonuna yerleştirilir. Ön alma arabelleği genellikle ileti nin sona ermesi sırasında çalışılamıyorsa, bu iletilerin tekrar tekrar önceden alınmasına, ancak kullanılabilir (geçerli olarak kilitlenmiş) bir durumda etkili bir şekilde teslim edilmelerine ve maksimum teslimat sayısı aşılır.

İleti işleme için yüksek derecede güvenilirliğe ihtiyacınız varsa ve işleme önemli iş ve zaman alıyorsa, prefetch özelliğini konservatif olarak veya hiç kullanmamanız önerilir.

Eğer yüksek iş verimi gerekiyorsa ve ileti işleme genellikle ucuzsa, prefetch önemli verim avantajları sağlar.

En yüksek ön alma sayısı ve kuyruk veya abonelik üzerinde yapılandırılan kilit süresi, kilit zaman aşımıen en azından ön getirme arabelleği, artı bir iletinin maksimum boyutu için kümülatif beklenen ileti işleme süresini aşacak şekilde dengelenmelidir. Aynı zamanda, kilitleme zaman aşımı, iletilerin yanlışlıkla bırakıldığında maksimum [TimeToLive'larını](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) aşabileceği kadar uzun olmamalıdır, böylece kilitlerinin yeniden teslim edilmeden önce süresinin dolmasını gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
