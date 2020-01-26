---
title: Önceden getirme iletilerini Azure Service Bus | Microsoft Docs
description: Azure Service Bus iletilerini önceden getirerek performansı artırabilirsiniz. İletiler, uygulama için istek yapılmadan önce yerel alma için hazır.
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
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76760666"
---
# <a name="prefetch-azure-service-bus-messages"></a>Önceden getirme Azure Service Bus iletileri

Herhangi bir resmi Service Bus istemcisinin herhangi birinde *önceden getirme* etkinleştirildiğinde, alıcı, uygulamanın ilk olarak sorulduğu kadar, [prefetchcount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) sınırına kadar daha fazla ileti alır.

Tek bir başlangıç [alma](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive) veya [ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) çağrısı bu nedenle, hemen kullanılabilir olarak döndürülen anında tüketim için bir ileti alır. Daha sonra istemci, önceden getirme arabelleğini dolduracak şekilde arka planda daha fazla ileti alır.

## <a name="enable-prefetch"></a>Önceden getirmeyi etkinleştir

.NET ile, bir **MessageReceiver**, **Queueclient**veya **Subscriptionclient** öğesinin [prefetchcount](/dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount#Microsoft_Azure_ServiceBus_QueueClient_PrefetchCount) özelliğini sıfırdan büyük bir sayıya ayarlayarak önceden getirme özelliğini etkinleştirirsiniz. Değerin sıfır olarak ayarlanması, önceden getirme özelliğini devre dışı bırakır.

Bu bağlamlarda etkisini görmek için, bu ayarı [Queuesgettingstarted](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/QueuesGettingStarted) veya [ReceiveLoop](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ReceiveLoop) Samples ' ayarlarının alma tarafına kolayca ekleyebilirsiniz.

İletiler önceden getirme arabelleğinde kullanılabilir olsa da, sonraki tüm **alma**/**ReceiveAsync** çağrıları arabelleğin hemen yerine getirilir ve alan kullanılabilir hale geldiğinde arabelleğin arka planda replenished olur. Teslim için kullanılabilecek bir ileti yoksa, alma işlemi arabelleği boşaltır ve beklendiği gibi bekler veya engeller.

Önceden getirme, [OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) ve [onmessageasync](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessageasync) API 'leriyle aynı şekilde de çalışacaktır.

## <a name="if-it-is-faster-why-is-prefetch-not-the-default-option"></a>Daha hızlıdır, neden varsayılan seçenek değildir.

Önceden getirme, uygulamanın bir kullanıcıdan önce ve öncesinde yerel olarak alımı için hazır bir ileti sunarak ileti akışını hızlandırır. Bu aktarım hızı, uygulama yazarının açıkça yapması gereken bir denge sonucudur:

[Receiveanddelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) alma moduyla birlikte, önceden getirme arabelleğine alınan tüm iletiler kuyrukta kullanılamaz ve yalnızca, **Receive**/**receiveasync** veya **OnMessage**/**onmessageasync** API 'leri aracılığıyla uygulamaya alınana kadar bellek içi önceden getirme arabelleğinde bulunur. Uygulama, iletiler uygulamaya alınmadan önce sonlanmışsa, bu iletiler kurtarılamayacak şekilde kaybedilir.

[PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) alma modunda, önceden getirme arabelleğine getirilen iletiler, ön belleğe kilitli durumda alınır ve kilit için zaman aşımı saatine sahiptir. Önceden getirme arabelleği büyükse ve işleme, önceden getirme arabelleğinde veya uygulama iletiyi işlerken bile ileti kilitlerinin süresinin dolacağı zaman alıyorsa, uygulamanın işlemesi için bazı kafa karıştırıcı olaylar olabilir.

Uygulama, süresi dolan veya ımınıda süresi dolan bir kilit olan bir ileti alabilir. Bu durumda, uygulama iletiyi işleyebilir, ancak kilit süre sonu nedeniyle tamamlanmayabileceğini bulabilir. Uygulama [Lockeduntilutc](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.lockeduntilutc) özelliğini denetleyebilir (aracı ve yerel makine saati arasındaki saat eğmaya tabidir). İleti kilidinin süresi dolmuşsa, uygulamanın iletiyi yoksayması gerekir; ileti üzerinde veya ile API çağrısı yapılmamalıdır. İletinin süresi dolmamışsa ancak süre sonu imminent ise, kilit, ileti çağırarak başka bir varsayılan kilit dönemi tarafından yenilenebilir ve Genişletilebilir [. RenewLock ()](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_RenewLockAsync_System_String_)

Kilit, önceden getirme arabelleğinde sessizce dolarsa, ileti terk edildi olarak değerlendirilir ve kuyruktan alma için kullanılabilir hale getirilir. Bu, önceden getirme arabelleğine getirilmemesine neden olabilir; sonuna yerleştirildi. Önceden getirme arabelleği genellikle ileti süre sonu sırasında çalışılamadıysa, iletilerin sürekli olarak önceden alınmasına, ancak artık kullanılabilir (geçerli kilitli) bir durumda teslim edilmesine ve bir süre sonra en fazla teslimat sayısı aşıldı.

İleti işleme için yüksek derecede güvenilirlik almanız gerekiyorsa ve işleme önemli iş ve zaman alırsa, önceden getirme özelliğini veya hiç bir şekilde kullanmanız önerilir.

Yüksek aktarım hızına ihtiyacınız varsa ve ileti işleme genellikle tek EAP ise, önceden getirme önemli verimlilik avantajları verir.

En fazla önceden getirme sayısı ve kuyruk veya abonelik üzerinde yapılandırılan kilit süresi, en az ön hazırlık arabelleğinin en büyük boyutu için bir ileti olan toplam beklenen ileti işleme süresini aşarak dengelenmesi gerekir. Aynı zamanda, kilit zaman aşımı, yanlışlıkla [ayrıldıklarında](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) Iletilerin maksimum zaman aşımına uğramasını aşmasına izin vermez, böylece yeniden teslim edilmeden önce kilidinin süresinin dolabilmesini gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
