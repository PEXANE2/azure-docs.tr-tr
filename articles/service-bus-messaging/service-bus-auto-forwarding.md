---
title: Azure Servis Veri Servisi mesajlaşma varlıklarıotomatik yönlendirme
description: Bu makalede, azure hizmet veri hizmeti sırasını veya aboneliği başka bir kuyruğa veya konuya nasıl zincirleneği açıklanmaktadır.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 8b8883b579233962de61e7247e6ac1cbcb2a6d80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76761058"
---
# <a name="chaining-service-bus-entities-with-autoforwarding"></a>Otomatik yönlendirme ile Zincirleme Servis Veri Günü varlıkları

Service Bus *otomatik yönlendirme* özelliği, aynı ad alanının parçası olan başka bir kuyruğa veya konuya bir kuyruk veya abonelik zincirlemenizi sağlar. Otomatik yönlendirme etkinleştirildiğinde, Servis Veri Servisi ilk sıraya veya aboneye (kaynak) yerleştirilen iletileri otomatik olarak kaldırır ve ikinci sıraya veya konuya (hedefe) koyar. Hedef varlığa doğrudan ileti göndermek yine de mümkündür.

## <a name="using-autoforwarding"></a>Otomatik yönlendirmeyi kullanma

Aşağıdaki örnekte olduğu [SubscriptionDescription][SubscriptionDescription] gibi, kaynak için [QueueDescription.ForwardTo][QueueDescription.ForwardTo] veya [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo] özelliklerini ayarlayarak otomatik yönlendirmeyi etkinleştirebilirsiniz: [QueueDescription][QueueDescription]

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Hedef varlığın kaynak varlığın oluşturulduğu anda var olması gerekir. Hedef varlık yoksa, Hizmet Veri Servisi kaynak varlık oluşturmak istendiğinde bir özel durum döndürür.

Tek bir konuyu ölçeklendirmek için otomatik yönlendirmeyi kullanabilirsiniz. Service Bus, [belirli bir konudaki abonelik sayısını](service-bus-quotas.md) 2.000 ile sınırlar. İkinci düzey konular oluşturarak ek abonelikleri barındırabilirsiniz. Abonelik sayısındaki Servis Veri Servisi sınırlaması ile bağlı olmasabile, ikinci bir konu düzeyi eklemek konunuzun genel iş düzeyini artırabilir.

![Otomatik yönlendirme senaryosu][0]

İleti gönderenleri alıcılardan ayırmak için otomatik yönlendirmeyi de kullanabilirsiniz. Örneğin, üç modülden oluşan bir ERP sistemi düşünün: sipariş işleme, stok yönetimi ve müşteri ilişkileri yönetimi. Bu modüllerin her biri, ilgili bir konuya sıralanmış iletiler oluşturur. Gamze ve Bob, müşterileri ile ilgili tüm iletilerle ilgilenen satış temsilcileridir. Bu iletileri almak için Gamze ve Gamze, tüm iletileri sıralarına otomatik olarak ileten ERP konularının her birinde kişisel bir kuyruk ve abonelik oluşturur.

![Otomatik yönlendirme senaryosu][1]

Alice tatile giderse, ERP konusu yerine kişisel sırası doluyor. Bu senaryoda, bir satış temsilcisi herhangi bir ileti almadığı için, ERP konularının hiçbiri kotaya ulaşmaz.

> [!NOTE]
> Otomatik yönlendirme kurulumu yapılırken, **Hem Kaynak hem de Hedef'teki** AutoDeleteOnIdle değeri otomatik olarak veri türünün maksimum değerine ayarlanır.
> 
>   - Kaynak tarafında, otomatik yönlendirme bir alma işlemi olarak hareket eder. Yani otomatik yönlendirme kurulumu olan kaynak gerçekten "boşta" asla.
>   - Hedef tarafta, bu her zaman iletiyi iletecek bir hedef olduğundan emin olmak için yapılır.

## <a name="autoforwarding-considerations"></a>Otomatik yönlendirme hususları

Hedef varlık çok fazla ileti biriktirir ve kotayı aşarsa veya hedef varlık devre dışı bırakılırsa, kaynak varlık hedefte boşluk olana (veya varlık yeniden etkinleştirilene kadar) iletileri [ölü harf kuyruğuna](service-bus-dead-letter-queues.md) ekler. Bu iletiler ölü harf kuyruğunda yaşamaya devam eder, bu nedenle bunları ölü harf kuyruğundan açıkça almalı ve işlemeniz gerekir.

Birçok abonelikle bileşik bir konu elde etmek için tek tek konuları birbirine zincirlerken, birinci düzey konu yla ilgili orta sayıda aboneliğiniz ve ikinci düzey konularda ki birçok aboneliğiniz olması önerilir. Örneğin, her biri 200 abonelikten oluşan ikinci düzey bir konuya zincirlenmiş 20 abonelikli birinci düzey bir konu, her biri 20 abonelikli ikinci düzey bir konuya zincirlenmiş, 200 abonelikli birinci düzey bir konuya göre daha yüksek iş elde edilmesine olanak tanır.

Servis Veri Servisi, iletilen her ileti için bir işlem fatura eder. Örneğin, her biri iletileri başka bir kuyruğa veya konuya otomatik olarak iletmek üzere yapılandırılan 20 aboneliği olan bir konuya ileti göndermek, tüm birinci düzey abonelikler iletinin bir kopyasını alırsa 21 işlem olarak faturalandırılır.

Başka bir kuyruğa veya konuya zincirlenmiş bir abonelik oluşturmak için, aboneliğin oluşturucusu hem kaynak hem de hedef varlık üzerinde **Denetim** İzinlerine sahip olmalıdır. Kaynak konuya ileti göndermek yalnızca kaynak konuda izin **gönder'i** gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

Otomatik yönlendirme hakkında ayrıntılı bilgi için aşağıdaki başvuru konularına bakın:

* [ForwardTo][QueueDescription.ForwardTo]
* [Sıra Açıklaması][QueueDescription]
* [Abonelik Açıklaması][SubscriptionDescription]

Service Bus performans iyileştirmeleri hakkında daha fazla bilgi edinmek için bkz. 

* [Service Bus Mesajlaşması kullanarak performans geliştirmek için en iyi yöntemler](service-bus-performance-improvements.md)
* [Bölümlenmiş ileti varlıkları.][Partitioned messaging entities]

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
