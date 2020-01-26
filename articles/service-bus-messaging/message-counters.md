---
title: Azure Service Bus-ileti sayısı
description: Azure Resource Manager ve Azure Service Bus NamespaceManager API 'Leri kullanarak kuyruklarda ve aboneliklerde tutulan ileti sayısını alın.
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
ms.openlocfilehash: 3a4fca0b3b60fcb76bcdc4f5f2d53df816c5053b
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76756411"
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

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
