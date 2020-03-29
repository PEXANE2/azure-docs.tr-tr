---
title: Azure Servis Veri Servisi - ileti varlıklarını askıya alma
description: Bu makalede, Azure Hizmet Veri Hizmeti veri aracı varlıkları (kuyruklar, konular ve abonelikler) geçici olarak askıya alma ve yeniden etkinleştirme nasıl açıklanmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760394"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>İleti varlıklarını askıya alma ve yeniden etkinleştirme (devre dışı bırakma)

Kuyruklar, konular ve abonelikler geçici olarak askıya alınabilir. Askıya alma, varlığı tüm iletilerin depoda muhafaza edildiği devre dışı bırakılmış bir duruma sokar. Ancak, iletiler kaldırılamaz veya eklenemez ve ilgili protokol işlemleri hata lar verir.

Bir varlığın askıya alınması genellikle acil idari nedenlerle yapılır. Senaryolardan biri, iletileri sıradan çıkaran, işleme de başarısız olan ve yine de iletileri yanlış tamamlayan ve kaldıran hatalı bir alıcı dağıtmaktır. Bu davranış tanılanırsa, düzeltilmiş kod dağıtılana ve hatalı kodun neden olduğu diğer veri kaybı önlenene kadar sıra alır için devre dışı bırakılabilir.

Askıya alma veya yeniden etkinleştirme kullanıcı veya sistem tarafından gerçekleştirilebilir. Sistem, yalnızca abonelik harcama limitine vurmak gibi ciddi idari nedenlerden dolayı varlıkları askıya alıyor. Sistem engelli varlıklar kullanıcı tarafından yeniden etkinleştirilemez, ancak askıya alma nedeni ele alınınca geri yüklenir.

Portalda, ilgili varlığın **Özellikleri** bölümü durumu değiştirmeyi sağlar; aşağıdaki ekran görüntüsü bir sıra için geçiş gösterir:

![][1]

Portal yalnızca kuyrukları tamamen devre dışı bırakmaya izin verir. Gönder ve alma işlemlerini .NET Framework SDK'daki Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API'lerini kullanarak veya Azure CLI veya Azure PowerShell üzerinden Azure Kaynak Yöneticisi şablonu yla da devre dışı kullanabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="suspension-states"></a>Askıya alma durumları

Bir sıra için ayarlanabilecek durumlar şunlardır:

-   **Active**: Kuyruk etkin.
-   **Devre Dışı :** Sıra askıya alındı.
-   **SendDisabled**: Sıra kısmen askıya alınır ve almaya izin verilir.
-   **ReceiveDisabled**: Sıra kısmen askıya alınır ve gönderiye izin verilir.

Abonelikler ve konular için yalnızca **Etkin** ve **Devre Dışı olarak** ayarlanabilir.

[EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) numaralandırması, yalnızca sistem tarafından ayarlanabilen bir geçiş durumları kümesini de tanımlar. Bir kuyruğa devre dışı koymak için PowerShell komutu aşağıdaki örnekte gösterilmiştir. Yeniden etkinleştirme komutu `Status` eşdeğerdir ve **Active'e**ayarlanır.

```powershell
$q = Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

