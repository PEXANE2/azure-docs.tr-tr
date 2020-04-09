---
title: Azure Servis Veri Servisi - ileti sayısı
description: Azure Kaynak Yöneticisi ve Azure Hizmet Veri Mesulini Ad Alanı Yöneticisi API'lerini kullanarak kuyruklarda ve aboneliklerde tutulan ileti sayısını alın.
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
ms.date: 04/08/2020
ms.author: aschhab
ms.openlocfilehash: 8020b12ca892fbf7dec6fed6259526d958fb110f
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80891773"
---
# <a name="message-counters"></a>İleti sayaçları

.NET Framework SDK'daki Azure Kaynak Yöneticisi ve Hizmet Veri [GönderisI Ad Alanı Yöneticisi](/dotnet/api/microsoft.servicebus.namespacemanager) API'lerini kullanarak kuyruklarda ve aboneliklerde tutulan ileti sayısını alabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell ile aşağıdaki gibi sayabilirsiniz:

```powershell
(Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>İleti sayısı ayrıntıları

Etkin ileti sayısını bilmek, bir kuyruğun şu anda dağıtılandan daha fazla kaynak gerektiren bir biriktirme listesi oluşturup oluşturmadığını belirlemede yararlıdır. Aşağıdaki sayaç ayrıntıları [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) sınıfında mevcuttur:

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount): Etkin durumda ve teslime hazır olan sırada veya abonelikteki iletiler.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): Ölü harf kuyruğundaki iletiler.
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): Zamanlanan durumdaki iletiler.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): Başka bir kuyruğa veya konuya aktarılamayan ve aktarım ölü harf sırasına taşınan iletiler.
-   [TransferİletiSayısı](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): Başka bir kuyruğa veya konuya aktarılanınbekleyen iletiler.

Bir uygulama kaynakları sıranın uzunluğuna göre ölçeklendirmek istiyorsa, bunu ölçülen bir hızda yapmalıdır. İleti sayaçlarının edinimi, ileti aracısı içinde pahalı bir işlemdir ve bunu sık sık doğrudan ve olumsuz bir şekilde yürütmek varlık performansını etkiler.

> [!NOTE]
> Hizmet Veri Servisi konusuna gönderilen iletiler, bu konuyla ilgili aboneliklere iletilir. Bu nedenle, bu iletiler aboneye başarıyla iletildik gibi, konunun kendisi üzerinde etkin ileti sayısı 0'dır. Abonelikteki ileti sayısını alın ve 0'dan büyük olduğunu doğrulayın. İletileri abonelikte görmenize rağmen, bunlar aslında konuya ait bir depolama alanında depolanır. 

Aboneliklere bakarsanız, sıfır olmayan ileti sayısına sahip olurlar (bu tüm varlık için 323 MB'a kadar alan ekler).

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
