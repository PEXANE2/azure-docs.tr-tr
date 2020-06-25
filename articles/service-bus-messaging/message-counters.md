---
title: Azure Service Bus-ileti sayısı
description: Azure Resource Manager ve Azure Service Bus NamespaceManager API 'Leri kullanarak kuyruklarda ve aboneliklerde tutulan ileti sayısını alın.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: d0e1a7a5c6eb0b281b4e6ac08135f41f28ecbec8
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85341277"
---
# <a name="message-counters"></a>İleti sayaçları

.NET Framework SDK 'sında Azure Resource Manager ve Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API 'lerini kullanarak kuyruklarda ve aboneliklerde tutulan ileti sayısını alabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell ile, sayımı aşağıdaki gibi edinebilirsiniz:

```powershell
(Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>İleti sayısı ayrıntıları

Etkin ileti sayısını bilmek, bir kuyruğun daha önce dağıtılmış olandan daha fazla kaynağın işlemesini gerektiren bir biriktirme listesi oluşturup oluşturmadığını belirlemek için faydalıdır. [Messagecountdetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) sınıfında aşağıdaki sayaç ayrıntıları mevcuttur:

-   [Activemessagecount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount): kuyruktaki veya abonelikteki iletiler etkin durumda ve teslim için hazırlanıyor.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): teslim edilemeyen ileti sırasındaki iletiler.
-   [Scheduledmessagecount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): zamanlanan durumdaki iletiler.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): başka bir kuyruğa veya konuya aktarılamayan ve aktarım atılacak ileti sırasına taşınmış iletiler.
-   [Transfermessagecount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): başka bir kuyruğa veya konuya Aktarılmayı bekleyen iletiler.

Bir uygulama, sıranın uzunluğuna bağlı olarak kaynakları ölçeklendirmek isterse, bunu ölçülen bir hızda yapmanız gerekir. İleti sayaçlarının alımı, ileti aracısının içindeki pahalı bir işlemdir ve genellikle doğrudan yürütülerek varlık performansını olumsuz yönde etkiler.

> [!NOTE]
> Bir Service Bus konusuna gönderilen iletiler, bu konunun aboneliklerine iletilir. Bu nedenle, söz konusu iletiler aboneliğe başarıyla iletileceği için konunun etkin ileti sayısı 0 ' dır. Abonelikte ileti sayısını alın ve 0 ' dan büyük olduğunu doğrulayın. Abonelikte iletileri görseniz bile, bunlar aslında konunun sahip olduğu bir depolama alanında depolanır. 

Aboneliklerle karşılaşırsanız, sıfır olmayan ileti sayımına sahip olur (Bu, tüm varlık için en fazla 323MB alan ekler).

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
