---
title: Azure Service Bus-mesajlaşma varlıklarını askıya alma
description: Bu makalede Azure Service Bus ileti varlıklarının (kuyruklar, konular ve abonelikler) geçici olarak askıya alınması ve yeniden etkinleştirilmesi açıklanmaktadır.
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
ms.openlocfilehash: 7386932f19eee064926184eb17f5e92e30add98e
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76760394"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Mesajlaşma varlıklarını askıya alma ve yeniden etkinleştirme (devre dışı bırakma)

Kuyruklar, konular ve abonelikler geçici olarak askıya alınabilir. Askıya alma, varlığı tüm iletilerin depolamada bulunduğu devre dışı durumuna geçirir. Ancak, iletiler kaldırılamaz veya eklenemez ve ilgili protokol işlemleri hatalara sebep olur.

Bir varlığın askıya alınması genellikle acil yönetim nedenleriyle yapılır. Tek bir senaryo, iletileri kuyruktan alan, işleme başarısız olan ve iletileri yanlış bir şekilde tamamlayan ve bunları kaldıran hatalı bir alıcı dağıtmış olur. Bu davranış tanılıyorsa, düzeltilen kod dağıtılana ve hatalı kodun neden olduğu daha fazla veri kaybı önyükleninceye kadar kuyruk alma için devre dışı bırakılabilir.

Askıya alma veya yeniden etkinleştirme, Kullanıcı ya da sistem tarafından gerçekleştirilebilir. Sistem yalnızca abonelik harcama sınırına vurmaya yönelik aksan nedeniyle yönetim nedenlerinden dolayı varlıkları askıya alır. Sistem devre dışı bırakılmış varlıklar Kullanıcı tarafından yeniden etkinleştirilemez, ancak askıya alınma nedeni sağlandığında geri yüklenir.

Portalda ilgili varlık için **Özellikler** bölümü, durumun değiştirilmesini mümkün; Aşağıdaki ekran görüntüsünde bir sıranın geçişi gösterilmektedir:

![][1]

Portal yalnızca sıraların tamamen devre dışı bırakılmasını sağlar. Ayrıca, .NET Framework SDK 'sında Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API 'lerini kullanarak veya Azure clı veya Azure PowerShell aracılığıyla Azure Resource Manager şablonuyla, gönderme ve alma işlemlerini ayrı olarak devre dışı bırakabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="suspension-states"></a>Askıya alma durumları

Bir kuyruk için ayarlanabilir durumlar şunlardır:

-   **Etkin**: kuyruk etkin.
-   **Devre dışı**: sıra askıya alındı.
-   **Senddisabled**: sıra, almaya izin verildiğinde kısmen askıya alınır.
-   **Receivedisabled**: sıra, gönderilmeye izin verilen kısmen askıya alındı.

Abonelikler ve konular için yalnızca **etkin** ve **devre dışı** ayarlanabilir.

[EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) numaralandırması Ayrıca yalnızca sistem tarafından ayarlanabileceğini belirten bir geçiş durumları kümesi tanımlar. Bir sırayı devre dışı bırakmak için PowerShell komutu aşağıdaki örnekte gösterilmiştir. Yeniden etkinleştirme komutu eşdeğerdir ve `Status` **etkin**olarak ayarlanıyor.

```powershell
$q = Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

