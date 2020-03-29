---
title: Servis Otobüsü asynchronous mesajlaşma | Microsoft Dokümanlar
description: Azure Hizmet Veri Yolu'nun kuyruklar, konular ve aboneliklerle birlikte bir mağaza ve iletme mekanizması aracılığıyla eşzamanlılığı nasıl desteklediğini öğrenin.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 554260f403104d815b9b63c576c7ba0a2f3cf1e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76761041"
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Eşzamanlı mesajlaşma desenleri ve yüksek kullanılabilirlik

Asynchronous mesajlaşma farklı şekillerde çeşitli şekillerde uygulanabilir. Kuyruklar, konular ve aboneliklerle Azure Servis Veri Servisi, mağaza ve iletme mekanizması aracılığıyla eşzamanlılığı destekler. Normal (senkron) işlemde, kuyruklara ve konulara ileti gönderir ve kuyruklardan ve aboneliklerden iletiler alırsınız. Yazdığınız uygulamalar, bu varlıkların her zaman kullanılabilir olmasına bağlıdır. Varlık sağlığı değiştiğinde, çeşitli koşullar nedeniyle, çoğu gereksinimi karşılayabilecek azaltılmış bir yetenek varlığı sağlamanın bir yolunu bulabilmeniz gerekir.

Uygulamalar genellikle bir dizi iletişim senaryosu etkinleştirmek için eşzamanlı ileti desenleri kullanır. Hizmet çalışmasa bile istemcilerin hizmetlere ileti gönderebileceği uygulamalar oluşturabilirsiniz. İletişim patlamaları yaşayan uygulamalar için, bir kuyruk iletişimara için bir yer sağlayarak yükü seviyedüzeyi yardımcı olabilir. Son olarak, iletileri birden çok makineye dağıtmak için basit ama etkili bir yük dengeleyicisi alabilirsiniz.

Bu varlıklardan herhangi birinin kullanılabilirliğini korumak için, bu varlıkların dayanıklı bir ileti sistemi için kullanılamamasıiçin çeşitli yollar düşünün. Genel olarak konuşursak, varlığın aşağıdaki farklı şekillerde yazdığımız uygulamalariçin kullanılamaz hale geldiğini görürsünüz:

* İleti gönderilemiş.
* İleti alınamıyor.
* Varlıkları yönetemiyor (varlıkları oluştur, al, güncelleştir veya sil).
* Servise başvuramıyor.

Bu hataların her biri için, bir uygulamanın azaltılmış yetenek düzeyinde çalışmayı gerçekleştirmeye devam etmesini sağlayan farklı hata modları vardır. Örneğin, ileti gönderebilen ancak almayan bir sistem müşterilerden sipariş almaya devam edebilir, ancak bu siparişleri işleyemez. Bu konu, oluşabilecek olası sorunları ve bu sorunların nasıl azaltılıcı olduğunu tartışır. Service Bus, tercih etmeniz gereken bir dizi azaltıcı etken ilerlemiştir ve bu konu, bu tercih edilen azaltmaların kullanımına ilişkin kuralları da tartışır.

## <a name="reliability-in-service-bus"></a>Servis Otobüsünde Güvenilirlik
İleti ve varlık sorunlarını işlemenin çeşitli yolları vardır ve bu azaltıcı etkenlerin uygun kullanımını düzenleyen yönergeler vardır. Yönergeleri anlamak için öncelikle Servis Veri Servisi'nde nelerin başarısız olabileceğini anlamanız gerekir. Azure sistemlerinin tasarımı nedeniyle, bu sorunların tümü kısa ömürlü olma eğilimindedir. Yüksek düzeyde, farklı kullanılabilirlik nedenleri aşağıdaki gibi görünür:

* Servis Veri Servisi'nin bağlı olduğu harici bir sistemden azaltma. Azaltma depolama ve bilgi işlem kaynakları ile etkileşimler oluşur.
* Servis Veri Servisi'nin bağlı olduğu bir sistem için sorun. Örneğin, depolama alanının belirli bir bölümü sorunlarla karşılaşabilir.
* Servis Veri Servisi'nin tek bir alt sistemde arızası. Bu durumda, bir bilgi işlem düğümü tutarsız bir duruma gelebilir ve kendisini yeniden başlatarak hizmet sunduğu tüm varlıkların diğer düğümlere bakiye yüklemesine neden olabilir. Bu da kısa bir süre yavaş ileti işlemeye neden olabilir.
* Bir Azure veri merkezi içinde Hizmet Veri Servisi'nin başarısızlığı. Bu, sistemin birkaç dakika veya birkaç saat boyunca erişilebildiği bir "felaket hatası"dır.

> [!NOTE]
> Depolama **terimi** hem Azure Depolama hem de SQL Azure anlamına gelebilir.
> 
> 

Servis Veri Servisi, bu sorunlar için bir dizi azaltıcı etken içerir. Aşağıdaki bölümlerde her sorun ve ilgili azaltıcı etkenler tartışılır.

### <a name="throttling"></a>Azaltma
Service Bus ile azaltma, işbirliğine dayalı ileti oranı yönetimi sağlar. Her bir Servis Otobüsü düğümü birçok tüzel kişilikbarındırmaktadır. Bu varlıkların her biri, CPU, bellek, depolama ve diğer yönleri açısından sistem üzerinde talepte bulunuyor. Bu uçlardan herhangi biri tanımlanan eşikleri aşan kullanımı algıladığında, Servis Veri Servisi belirli bir isteği reddedebilir. Arayan bir [ServerBusyException][ServerBusyException] alır ve 10 saniye sonra yeniden çalışır.

Azaltma olarak, kod hatayı okumalı ve iletinin yeniden denemelerini en az 10 saniye süreyle durdurmalıdır. Hata müşteri uygulamasının parçaları arasında gerçekleşebileceğinden, her parçanın yeniden deneme mantığını bağımsız olarak yürütmesi beklenir. Kod, bir kuyruk veya konu üzerinde bölümleme etkinleştirerek daraltılma olasılığını azaltabilir.

### <a name="issue-for-an-azure-dependency"></a>Azure bağımlılığı sorunu
Azure'daki diğer bileşenlerde zaman zaman hizmet sorunları olabilir. Örneğin, Servis Veri Kurumu'nun kullandığı bir sistem yükseltilirken, bu sistem geçici olarak azaltılmış özelliklerle karşılaşabilir. Bu tür sorunları aşmak için Servis Veri Servisi, azaltıcı etkenleri düzenli olarak araştırır ve uygular. Bu hafifletmelerin yan etkileri görünür. Örneğin, depolamayla ilgili geçici sorunları işlemek için, Hizmet Veri Birimi ileti gönderme işlemlerinin tutarlı bir şekilde çalışmasını sağlayan bir sistem uygular. Azaltmanın doğası gereği, gönderilen iletinin etkilenen sırada veya abonelikte görünmesi ve bir alma işlemi için hazır olması 15 dakika kadar sürebilir. Genel olarak konuşursak, çoğu varlık bu sorunla karşılaşmayacak. Ancak, Azure'daki Hizmet Veri Veri Mes'deki varlıkların sayısı göz önüne alındığında, bu azaltma bazen Hizmet Veri Ç'leri müşterilerinin küçük bir alt kümesi için gereklidir.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Tek bir alt sistemde Servis Veri Servisi hatası
Herhangi bir uygulamada, koşullar Servis Veri Servisi'nin dahili bir bileşeninin tutarsız olmasına neden olabilir. Servis Veri Servisi bunu algıladığında, ne olduğunu tanılamada yardımcı olmak için uygulamadan veri toplar. Veriler toplandıktan sonra, uygulama tutarlı bir duruma döndürmek amacıyla yeniden başlatılır. Bu işlem oldukça hızlı bir şekilde gerçekleşir ve tipik çalışma süreleri çok daha kısa olsa da, bir varlığın birkaç dakikaya kadar kullanılamıyor gibi görünmesine neden olur.

Bu gibi durumlarda, istemci uygulaması bir [System.TimeoutException][System.TimeoutException] veya [MessagingException exception][MessagingException] oluşturur. Service Bus, otomatik istemci yeniden deneme mantığı biçiminde bu sorun için bir azaltma içerir. Yeniden deneme süresi bittikten ve ileti teslim edilmedikten sonra, [kesintileri ve felaketleri işleme][handling outages and disasters]ile ilgili makalede belirtilen diğer kullanarak keşfedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi Servis Veri Servisi'nde eşzamanlı mesajlaşmanın temellerini öğrendiğinize göre, [kesintileri ve felaketleri ele alma][handling outages and disasters]hakkında daha fazla ayrıntı okuyun.

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN
[handling outages and disasters]: service-bus-outages-disasters.md
