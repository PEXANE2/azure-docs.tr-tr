---
title: Azure Servis Veri Servisi yinelenen ileti algılama | Microsoft Dokümanlar
description: Bu makalede, Azure Hizmet Veri Servisi iletilerinde yinelenenleri nasıl algılayabildiğiniz açıklanmaktadır. Yinelenen ileti yoksayılabilir ve bırakılabilir.
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
ms.openlocfilehash: c109b9fd310a09e5eb4c6d18cc3536e4d8069c0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760377"
---
# <a name="duplicate-detection"></a>Yineleme algılama

Bir uygulama, ileti gönderdikten hemen sonra ölümcül bir hata nedeniyle başarısız olursa ve yeniden başlatılan uygulama örneği hatalı bir şekilde önceki ileti tesliminin gerçekleşmediğini düşünüyorsa, sonraki bir gönderme aynı iletinin sistemde iki kez görünmesine neden olur.

İstemci veya ağ düzeyinde bir hatanın bir an önce oluşması ve gönderilen bir iletinin sıraya işlenmesi ve bildirimin istemciye başarıyla döndürülmesi de mümkündür. Bu senaryo, istemciyi gönderme işleminin sonucu hakkında şüpheye soyar.

Yinelenen algılama, gönderenin aynı iletiyi yeniden göndermesini sağlayarak bu durumların şüphesini ortadan çıkarır ve sıra veya konu yinelenen kopyaları atar.

Yinelenen algılamayı etkinleştirmek, belirli bir zaman penceresinde kuyruğa veya konuya gönderilen tüm iletilerin uygulama kontrollü *MessageId'ini* izlemeye yardımcı olur. Zaman penceresinde günlüğe kaydedilmiş *MessageId* ile yeni bir ileti gönderilirse, ileti kabul edilmiş olarak bildirilir (gönderme işlemi başarılı olur), ancak yeni gönderilen ileti anında yoksayılır ve bırakılır. *İletinin MessageId* dışında başka hiçbir bölümü dikkate alınmaz.

Tanımlayıcının uygulama denetimi esastır, çünkü yalnızca bu uygulamanın *MessageId'i* bir hata oluştuğunda tahmin edilebilir şekilde yeniden oluşturulabileceği bir iş süreci bağlamına bağlamasına olanak tanır.

Bazı uygulama bağlamını işleme sürecinde birden çok iletinin gönderildiği bir iş süreci için *MessageId,* satınalma sipariş numarası gibi uygulama düzeyi bağlam tanımlayıcısının ve örneğin **12345.2017/payment'ın**konusunun bir lösyonu olabilir.

*MessageId* her zaman bazı GUID olabilir, ancak tanımlayıcıyı iş sürecine bağlamak, yinelenen algılama özelliğinden etkin bir şekilde yararlanmak için istenen öngörülebilir tekrarlanabilirlik sağlar.

> [!NOTE]
> Yinelenen algılama etkinleştirilmişse ve oturum kimliği veya bölüm anahtarı ayarlanmazsa, ileti kimliği bölüm anahtarı olarak kullanılır. İleti kimliği de ayarlı değilse, .NET ve AMQP kitaplıkları ileti için otomatik olarak bir ileti kimliği oluşturur. Daha fazla bilgi için [bkz.](service-bus-partitioning.md#use-of-partition-keys)

## <a name="enable-duplicate-detection"></a>Yinelenen algılamayı etkinleştirme

Portalda, özellik varsayılan olarak kapalı olan **yinelenen algılama** onay kutusunu etkinleştir ile varlık oluşturma sırasında açılır. Yeni konular oluşturmak için ayar eşdeğerdir.

![][1]

> [!IMPORTANT]
> Sıra oluşturulduktan sonra yinelenen algılamayı etkinleştiremez/devre dışı kalamazsınız. Bunu yalnızca sırayı oluştururken yapabilirsiniz. 

Programlı olarak, tam çerçeve .NET API üzerinde [QueueDescription.requiresDuplicateDetection](/dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection) özelliği ile bayrağı ayarlarsınız. Azure Kaynak Yöneticisi API ile [değer, queueProperties.requiresDuplicateDetection](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliğiyle ayarlanır.

Yinelenen algılama süresi geçmişi, en fazla yedi günlük bir değerle kuyruklar ve konular için 30 saniyeye kadar varsayılandır. Bu ayarı Azure portalındaki sıra ve konu özellikleri penceresinde değiştirebilirsiniz.

![][2]

Programlı olarak, tam .NET Framework API ile [QueueDescription.DuplicateDetectionHistoryTimeWindow](/dotnet/api/microsoft.servicebus.messaging.queuedescription.duplicatedetectionhistorytimewindow#Microsoft_ServiceBus_Messaging_QueueDescription_DuplicateDetectionHistoryTimeWindow) özelliğini kullanarak ileti kimliklerinin tutulduğu yinelenen algılama penceresinin boyutunu yapılandırabilirsiniz. Azure Kaynak Yöneticisi API ile değer [queueProperties.duplicateDetectionHistoryTimeWindow](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) özelliğiyle ayarlanır.

Yinelenen algılamayı etkinleştirmek ve pencerenin boyutu, kaydedilen tüm ileti kimliklerinin yeni gönderilen ileti tanımlayıcısı ile eşleşmesi gerektiğinden, sıra (ve konu) çıktısını doğrudan etkiler.

Pencereyi küçük tutmak, daha az ileti kimliği nin saklanıp eşleşmesi ve iş çıktısının daha az etkilemesi gerektiği anlamına gelir. Yinelenen algılama gerektiren yüksek iş elde varlıklar için pencereyi olabildiğince küçük tutmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

İstemci kodunun eskisi gibi aynı *MessageId'e* sahip bir iletiyi yeniden gönderemediği senaryolarda, güvenli bir şekilde yeniden işlenebilen iletiler tasarlamak önemlidir. [Idemiktidarhakkında](https://particular.net/blog/what-does-idempotent-mean) bu blog yazısı bunu yapmak için nasıl çeşitli teknikler açıklar.

[1]: ./media/duplicate-detection/create-queue.png
[2]: ./media/duplicate-detection/queue-prop.png
