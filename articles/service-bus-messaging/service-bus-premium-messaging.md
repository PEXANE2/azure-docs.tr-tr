---
title: Azure Servis Veri Servisi premium ve standart katmanlar
description: Bu makalede, Azure Hizmet Veri Servisi'nin standart ve premium katmanları açıklanmaktadır. Bu katmanları karşılaştırır ve teknik farklılıklar sağlar.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: aschhab
ms.openlocfilehash: ef3cc8d4c7354b43389244e72c2dbc5899b8db25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76774558"
---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Service Bus Premium ve Standart Mesajlaşma katmanları

Kuyruklar ve konu başlıkları gibi varlıkları içeren Service Bus Mesajlaşma, kuruluşun mesajlaşma işlevlerini bulut ölçeğinde zengin yayımla-abone ol semantiği ile birleştirir. Service Bus Mesajlaşması birçok gelişmiş bulut çözümü için iletişimin temel öğesi olarak kullanılır.

Servis Veri Servisi Mesajlaşmasının *Premium* katmanı, görev açısından kritik uygulamalar için ölçek, performans ve kullanılabilirlik konusunda yaygın müşteri isteklerini giderir. Üretim senaryoları için Premium katman önerilir. Özellikler kümeleri neredeyse aynı olsa da, Service Bus Mesajlaşma hizmetinin bu iki katmanı farklı kullanım durumlarına göre tasarlanmıştır.

Aşağıdaki tabloda bazı üst düzey farklılıklar vurgulanmıştır.

| Premium | Standart |
| --- | --- |
| Yüksek verimlilik |Değişken işleme |
| Tahmin edilebilir performans |Değişken gecikme süresi |
| Sabit fiyatlandırma |Kullandıkça Öde değişken fiyatlandırması |
| İş yükünün ölçeğini artırma veya azaltma |Yok |
| İleti boyutu 1 MB’a kadar |İleti boyutu 256 KB’a kadar |

**Service Bus Premium Mesajlaşma Hizmeti**, CPU'da ve bellek düzeyinde kaynak yalıtımına olanak sağladığından her müşterinin iş yükü yalıtımlı şekilde çalışır. Bu kaynak kapsayıcısı *mesajlaşma birimi* olarak adlandırılır. Her premium ad alanı, en az bir mesajlaşma birimi için ayrılmıştır. Her Service Bus Premium ad alanı için 1, 2, 4 veya 8 mesajlaşma birimi satın alabilirsiniz. Tek bir iş yükü veya varlık birden çok ileti birimine yayılabilir ve ileti birimi sayısı değiştirilebilir. Sonuç olarak, Service Bus tabanlı çözümünüz için tahmin edilebilir ve tekrarlanabilir bir performans elde edersiniz.

Daha tahmin edilebilir ve kullanılabilir olmasının yanı sıra bu performans, daha hızlıdır. Service Bus Premium Messaging, Azure Etkinlik [Hub'larında](https://azure.microsoft.com/services/event-hubs/)tanıtılan depolama motoruna dayanmaktadır. Premium Mesajlaşma sayesinde, en yüksek performans Standart katmanda olduğundan daha hızlıdır.

## <a name="premium-messaging-technical-differences"></a>Premium Mesajlaşmanın teknik farklılıkları

Aşağıdaki bölümlerde Premium ve Standart mesajlaşma katmanları arasındaki bazı farklar ele alınmaktadır.

### <a name="partitioned-queues-and-topics"></a>Bölümlenmiş kuyruklar ve konular

Bölümlenmiş kuyruk ve konular Premium Mesajlaşma’da desteklenmez. Bölümleme hakkında daha fazla bilgi için bkz. [Bölümlenmiş kuyruklar ve konular](service-bus-partitioning.md).

### <a name="express-entities"></a>İfade varlıkları

Premium mesajlaşma tamamen yalıtılmış bir çalışma zamanı ortamında çalıştığından Premium ad alanlarında ifade varlıkları desteklenmez. İfade özellikleri hakkında daha fazla bilgi için [QueueDescription.EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) özelliğine bakın.

Standart mesajlaşma altında çalışan bir kodunuz varsa ve Premium katmanı ile bağlantı noktası oluşturmak istiyorsanız, [EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) özelliğinin **false** (varsayılan değer) olarak ayarlandığından emin olun.

## <a name="premium-messaging-resource-usage"></a>Premium Mesajlaşma kaynak kullanımı
Genel olarak, bir varlık üzerinde herhangi bir işlem CPU ve bellek kullanımına neden olabilir. Bu işlemlerden bazıları şunlardır: 

- Kuyruklar, konular ve abonelikler üzerindeki CRUD (Oluştur, Al, Güncelle ve Sil) işlemleri gibi yönetim işlemleri.
- Çalışma zamanı işlemleri (ileti gönderme ve alma)
- İzleme işlemleri ve uyarılar

Ek CPU Ve bellek kullanımı olsa da ek olarak fiyatlandırılır değildir. Premium İleti katmanı için ileti birimi için tek bir fiyat vardır.

CPU ve bellek kullanımı aşağıdaki nedenlerle izlenir ve size görüntülenir: 

- Sistem içlerinde şeffaflık sağlama
- Satın alınan kaynakların kapasitesini anlayın.
- Büyütmeye/küçültmeye karar vermenize yardımcı olan kapasite planlaması.

## <a name="messaging-unit---how-many-are-needed"></a>Mesajlaşma birimi - Kaç tane gereklidir?

Azure Hizmet Veri Servisi Premium ad alanı sağlarken, ayrılan ileti birimi sayısının belirtilmesi gerekir. Bu ileti birimleri ad alanına ayrılan ayrılmış kaynaklardır.

Service Bus Premium ad alanına ayrılan ileti birimi sayısı, iş yüklerindeki değişime (artış veya azalış) faktöre **dinamik olarak ayarlanabilir.**

Mimariniz için ileti birimi sayısına karar verirken göz önünde bulundurulması gereken birkaç faktör vardır:

- Ad alanınıza ayrılmış ***1 veya 2 ileti birimiyle*** başlayın.
- Ad alanınız için [Kaynak kullanım ölçümleri](service-bus-metrics-azure-monitor.md#resource-usage-metrics) içindeki CPU kullanım ölçümlerini incein.
    - CPU kullanımı ***%20'nin altındaysa,*** ad alanınıza ayrılan ileti birimi sayısını ***küçültebilirsiniz.***
    - CPU kullanımı ***%70'in üzerindeyse,*** uygulamanız ad alanınıza ayrılan ileti birimi sayısını ***ölçeklemeden*** yararlanır.

Hizmet Veri Birimi ad alanlarına ayrılan kaynakları ölçekleme işlemi Azure [Otomasyon Runbook'ları](../automation/automation-quickstart-create-runbook.md)kullanılarak otomatikleştirilebilir.

> [!NOTE]
> Ad alanına ayrılan kaynakların **ölçekleilmesi** önleyici veya reaktif olabilir.
>
>  * **Preemptive**: Ek iş yükü bekleniyorsa (mevsimsellik veya eğilimler nedeniyle), iş yükleri çarpmadan önce ad alanına daha fazla ileti birimi ayırmaya devam edebilirsiniz.
>
>  * **Reaktif**: Kaynak kullanım ölçümleri incelenerek ek iş yükleri tanımlanırsa, artan talebi birleştirmek için ad alanına ek kaynaklar ayrılabilir.
>
> Servis Otobüsü için faturalandırma sayaçları saatliktir. Ölçekleme durumunda, yalnızca bu saatler için ek kaynaklar için ödeme.
>

## <a name="get-started-with-premium-messaging"></a>Premium Mesajlaşmayı kullanmaya başlama

Premium Mesajlaşma ile çalışmaya başlamak kolaydır ve süreç Standart Mesajlaşma ile benzerlik gösterir. [Azure Portal](https://portal.azure.com)'da [ad alanı oluşturarak](service-bus-create-namespace-portal.md) başlayın. **Fiyatlandırma katmanı**altında **Premium'u** seçtiğinizden emin olun. Her bir katman hakkında daha fazla bilgi almak için **Fiyatlandırma ayrıntılarının tamamını görüntüle**'ye tıklayın.

![create-premium-namespace][create-premium-namespace]

Ayrıca [Azure Resource Manager şablonlarını kullanarak premium ad alanları](https://azure.microsoft.com/resources/templates/101-servicebus-pn-ar/) oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara bakın:

* [Azure Hizmet Veri Servisi Premium Mesajlaşma (blog gönderisi) tanıtımı](https://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure Hizmet Veri Servisi Premium Mesajlaşma (Channel9) Tanıtımı](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Servis Veri Servisi Mesajlaşma genel bakış](service-bus-messaging-overview.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png
