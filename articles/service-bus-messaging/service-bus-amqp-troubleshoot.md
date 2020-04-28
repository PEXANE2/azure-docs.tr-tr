---
title: Azure Service Bus AMQP hatalarıyla ilgili sorunları giderin | Microsoft Docs
description: Azure Service Bus kullanırken alabileceğiniz AMQP hatalarının bir listesini ve bu hataların nedenlerini sağlar.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2019
ms.author: aschhab
ms.openlocfilehash: 85d24a9e7c753ec5dba80c4f259dd3fb51d9c14b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60402794"
---
# <a name="amqp-errors-in-azure-service-bus"></a>Azure Service Bus AMQP hataları
Bu makalede, AMQP 'yi Azure Service Bus kullanırken aldığınız bazı hatalar sunulmaktadır. Bu hizmetler hizmetin tüm standart davranışlarından oluşur. Bağlantıyı/bağlantıyı otomatik olarak yeniden oluşturan bağlantı/bağlantı üzerinde gönderme/alma çağrıları yaparak bu onlardan kaçınabilirsiniz.

## <a name="link-is-closed"></a>Bağlantı kapalı 
AMQP bağlantısı ve bağlantısı etkin olduğunda, ancak 10 dakikalık bağlantı kullanılarak hiçbir çağrı (örneğin, gönderme veya alma) yapıldığında aşağıdaki hatayı görürsünüz. Bu nedenle bağlantı kapatılır. Bağlantı hala açık.

```
amqp:link:detach-forced:The link 'G2:7223832:user.tenant0.cud_00000000000-0000-0000-0000-00000000000000' is force detached by the broker due to errors occurred in publisher(link164614). Detach origin: AmqpMessagePublisher.IdleTimerExpired: Idle timeout: 00:10:00. TrackingId:00000000000000000000000000000000000000_G2_B3, SystemTracker:mynamespace:Topic:MyTopic, Timestamp:2/16/2018 11:10:40 PM
```

## <a name="connection-is-closed"></a>Bağlantı kapalı
Herhangi bir etkinlik (boşta) olmadığından ve 5 dakika içinde yeni bir bağlantı oluşturulmadığı için, bağlantı içindeki tüm bağlantılar kapatıldığında AMQP bağlantısında aşağıdaki hatayı görürsünüz.

```
Error{condition=amqp:connection:forced, description='The connection was inactive for more than the allowed 300000 milliseconds and is closed by container 'LinkTracker'. TrackingId:00000000000000000000000000000000000_G21, SystemTracker:gateway5, Timestamp:2019-03-06T17:32:00', info=null}
```

## <a name="link-is-not-created"></a>Bağlantı oluşturulmadı 
Yeni bir AMQP bağlantısı oluşturulduğunda bu hatayı görürsünüz ancak AMQP bağlantısı oluşturulurken 1 dakika içinde bir bağlantı oluşturulmaz.

```
Error{condition=amqp:connection:forced, description='The connection was inactive for more than the allowed 60000 milliseconds and is closed by container 'LinkTracker'. TrackingId:0000000000000000000000000000000000000_G21, SystemTracker:gateway5, Timestamp:2019-03-06T18:41:51', info=null}
```

## <a name="next-steps"></a>Sonraki adımlar

AMQP ve Service Bus hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları ziyaret edin:

* [Service Bus AMQP 'ye Genel Bakış]
* [AMQP 1.0 protokol kılavuzu]
* [Windows Server için Service Bus AMQP]

[Service Bus AMQP 'ye Genel Bakış]: service-bus-amqp-overview.md
[AMQP 1.0 protokol kılavuzu]: service-bus-amqp-protocol-guide.md
[Windows Server için Service Bus AMQP]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)
