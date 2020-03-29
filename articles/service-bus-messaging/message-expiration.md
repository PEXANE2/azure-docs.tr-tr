---
title: Azure Servis Veri Servisi - iletinin son kullanma süresi
description: Bu makalede, Azure Hizmet Veri Servisi iletilerinin süresi ve yaşam süresi hakkında açıklanmaktadır. Böyle bir son tarihten sonra, ileti artık teslim edilir.
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
ms.openlocfilehash: e86c92fa1cfb13929d5617502224f479709efdd3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76756343"
---
# <a name="message-expiration-time-to-live"></a>İleti süre sonu (Yaşam Süresi)

İletideki yük veya bir iletinin alıcıya ilettiği bir komut veya sorgu, hemen hemen her zaman bir tür uygulama düzeyi son kullanma tarihine tabidir. Böyle bir son tarihten sonra, içerik artık teslim edilmez veya istenen işlem artık yürütülmez.

Kuyrukların ve konuların genellikle uygulamaların veya uygulama parçalarının kısmi çalıştırmaları bağlamında kullanıldığı geliştirme ve test ortamları için, bir sonraki test çalışmasının otomatik olarak toplandığı mahsur test iletilerinin otomatik olarak toplanması da istenir temiz başlayın.

Herhangi bir tek iletiiçin son kullanma süresi, göreceli bir süre yi belirten [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) sistem özelliği ayarlayarak denetlenebilir. İleti varlığa sıralandığında son kullanma süresi mutlak bir anda gerçekleşir. O zaman, [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc) özelliği değeri alır [(**EnqueuedTimeUtc**](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc#Microsoft_ServiceBus_Messaging_BrokeredMessage_EnqueuedTimeUtc) + [**TimeToLive**)](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive). Aracılı iletide canlı olarak zaman (TTL) ayarı, etkin olarak dinleyen istemciler olmadığında zorlanmaz.

Sona **Erdirilen AtUtc** anından sonra, iletiler geri alınabilecek hale gelir. Son kullanma tarihi, şu anda teslim için kilitlenmiş iletileri etkilemez; bu iletiler hala normal olarak işlenir. Kilidin süresi dolduğunda veya ileti terk edilirse, son kullanma tarihi hemen etkin olur.

İleti kilit altındayken, uygulama süresi dolmuş bir iletiye sahip olabilir. Uygulamanın işleme devam etmek için istekli olup olmadığını veya iletiyi terk etmeyi seçer uygulayıcıya kalmış.

## <a name="entity-level-expiration"></a>Varlık düzeyinde sona erme

Bir kuyruğa veya konuya gönderilen tüm iletiler, [varsayılanMessageTimeToLive](/azure/templates/microsoft.servicebus/namespaces/queues) özelliğiyle varlık düzeyinde ayarlanan ve oluşturma sırasında portalda da ayarlanabilen ve daha sonra ayarlanabilen varsayılan bir son kullanma süresine tabidir. Varsayılan son kullanma süresi, [TimeToLive'ın](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) açıkça ayarlanmadığı varlığa gönderilen tüm iletiler için kullanılır. Varsayılan son kullanma süresi, **TimeToLive** değeri için bir tavan olarak da işlev görür. Varsayılan değerden daha uzun **TimeToLive** sona erme süresine sahip iletiler, sıraya girmeden önce **varsayılanMessageTimeToLive** değerine sessizce ayarlanır.

> [!NOTE]
> Aracılı ileti için varsayılan [TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) değeri, aksi belirtilmemişse [TimeSpan.Max'tir.](https://docs.microsoft.com/dotnet/api/system.timespan.maxvalue)
>
> İleti kuruluşları (kuyruklar ve konular) için varsayılan son kullanma süresi, Hizmet Veri Hizmetleri standardı ve premium katmanlar için [TimeSpan.Max'tir.](https://docs.microsoft.com/dotnet/api/system.timespan.maxvalue)  Temel katman için varsayılan son kullanma süresi 14 gündür.

Süresi dolan iletiler, [EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enabledeadletteringonmessageexpiration#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) özelliğini ayarlayarak veya portaldaki ilgili kutuyu işaretleyerek isteğe bağlı olarak [ölü harf kuyruğuna](service-bus-dead-letter-queues.md) taşınabilir. Seçenek devre dışı bırakılırsa, süresi dolmuş iletiler bırakılır. Ölü harf kuyruğuna taşınan süresi dolan iletiler, broker'ın kullanıcı özellikleri bölümünde [depoladığının DeadletterReason](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq) özelliğini değerlendirerek diğer ölü harflerle yazılmış iletilerden ayırt edilebilir; bu durumda değer [TTLExpiredException'dır.](service-bus-dead-letter-queues.md#moving-messages-to-the-dlq)

İletinin kilit altındayken son kullanma tarihine karşı korunduğu ve bayrak varlık üzerinde ayarlanmışsa, kilit terk edildikçe veya süresi dolduğunda ileti ölü harf kuyruğuna taşınır. Ancak, ileti başarıyla kapatılırsa, bu da uygulamanın nominal son kullanma süresine rağmen uygulamanın başarıyla işlediği varsayar.

[TimeToLive](/dotnet/api/microsoft.azure.servicebus.message.timetolive#Microsoft_Azure_ServiceBus_Message_TimeToLive) ve otomatik (ve işlemsel) son kullanma tarihinde dead-lettering kombinasyonu bir işleyici veya son tarih boyunca işleyicibir grup için verilen bir işleyici güven kurmak için değerli bir araçtır son tarihe kadar işleme için alınır.

Örneğin, ölçekle sınırlandırılmış arka uçtaki işleri güvenilir bir şekilde yürütmesi gereken ve zaman zaman trafik artışlarını deneyimleyen veya bu arka uç kullanılabilirliği bölümlerine karşı izole olmak isteyen bir web sitesini düşünün. Normal durumda, gönderilen kullanıcı verilerinin sunucu tarafındaki işleyici bilgileri kuyruğa iter ve daha sonra hareketin yanıt kuyruğuna başarılı bir şekilde işlenmesini onaylayan bir yanıt alır. Bir trafik sıçraması varsa ve arka uç işleyicibiriktirme listesi öğelerini zamanında işleyemezse, süresi dolan işler ölü harf kuyruğunda döndürülür. Etkileşimli kullanıcı, istenen işlemin normalden biraz daha uzun süreceği ve istek daha sonra nihai işlem sonucunun kullanıcıya e-posta ile gönderildiği bir işleme yolu için farklı bir sıraya konulabilir. 


## <a name="temporary-entities"></a>Geçici varlıklar

Servis Veri Hizmetleri kuyrukları, konular ve abonelikler, belirli bir süre kullanılmadığında otomatik olarak kaldırılan geçici varlıklar olarak oluşturulabilir.
 
Otomatik temizleme, varlıkların dinamik olarak oluşturulduğu ve test veya hata ayıklama çalışmasının kesintiye uğraması nedeniyle kullanımdan sonra temizlenmediği geliştirme ve test senaryolarında yararlıdır. Bir uygulama, yanıt kuyruğu gibi dinamik varlıklar oluşturduğunda, yanıtları bir web sunucusu işlemine geri almak için veya nesne bu varlıkları güvenilir bir şekilde temizlemenin zor olduğu başka bir görece kısa ömürlü nesneye de yararlıdır örnek kaybolur.

Özellik [autoDeleteOnIdle](/azure/templates/microsoft.servicebus/namespaces/queues) özelliği kullanılarak etkinleştirilir. Bu özellik, bir varlığın otomatik olarak silinmeden önce boşta (kullanılmayan) olması gereken süreye ayarlanır. Bu özellik için minimum değer 5'tir.
 
**autoDeleteOnIdle** özelliği bir Azure Kaynak Yöneticisi işlemi veya .NET Framework [istemcisi NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API'leri aracılığıyla ayarlanmalıdır. Portala ayarlayamazsın.

## <a name="idleness"></a>Tembellik

Varlıkların (kuyruklar, konular ve abonelikler) boştalığı olarak kabul edilen ler şunlardır:

- Kuyruklar
    - Gönderme yok  
    - Alıcı yok  
    - Sırada güncelleştirme yok  
    - Zamanlanmış ileti yok  
    - Gözat/gözetleme yok 
- Konular  
    - Gönderme yok  
    - Konuyla ilgili güncelleştirme yok  
    - Zamanlanmış ileti yok 
- Abonelikler
    - Alıcı yok  
    - Abonelikte güncelleştirme yok  
    - Abonelmeye yeni kurallar eklenmez  
    - Gözat/gözetleme yok  
 


## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
