---
title: Azure Event Hubs nedir? - Büyük Veri alma hizmeti | Microsoft Dokümanlar
description: Saniyede milyonlarca olayın eklenmesini sağlayan bir Büyük Veri akış hizmeti olan Azure Event Hubs hakkında daha fazla bilgi edinin.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: overview
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 45276ab59f1a3dabea42b904ff54bd37326fdeca
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398118"
---
# <a name="azure-event-hubs--a-big-data-streaming-platform-and-event-ingestion-service"></a>Azure Etkinlik Hub'ları — Büyük veri akış platformu ve olay alma hizmeti
Azure Etkinlik Hub'ları büyük bir veri akışı platformu ve etkinlik hizmetidir. Saniyede milyonlarca olayı alabilir ve işleyebilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir.

Aşağıdaki senaryolar, Olay Hub'larını kullanabileceğiniz senaryolardan bazılarıdır:

- Anomali algılama (sahte/aykırı değer)
- Uygulama günlüğüne kaydetme
- Tıklama dizileri gibi analiz işlem hatları
- Canlı pano oluşturma
- Veri arşivleme
- İşlem gerçekleştirme
- Kullanıcı telemetrisi işleme
- Cihaz telemetrisi akışı

<iframe width="560" height="315" src="https://www.youtube.com/embed/45wgY-VSk9I" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## <a name="why-use-event-hubs"></a>Event Hubs’ı neden kullanmalıyım?

Veriler ancak kolay bir şekilde işlenebildiğinde ve veri kaynaklarından tam zamanında içgörüler alındığında değerlidir. Olay Hub'ları, azure içinde ve dışında veri ve analitik hizmetleriyle birlikte büyük veri ardınızı oluşturmak için düşük gecikme gecikmesi ve sorunsuz entegrasyona sahip dağıtılmış bir akış işleme platformu sağlar.

Event Hubs bir olay işlem hattı için "ön kapı" görevi yapar ve çözüm mimarilerinde genelde *olay alıcı* olarak adlandırılır. Olay yutucu, bir olay akışının üretimini ilgili olayların kullanılmasına ayıran, olay yayımcıları ile olay tüketicileri arasında duran bir bileşen veya hizmettir. Olay Hub'ları, olay üreticilerini olay tüketicilerinden ayıran zaman saklama arabelleği içeren birleşik bir akış platformu sağlar.

Aşağıdaki bölümlerde Azure Event Hubs hizmetinin temel özellikleri anlatılmaktadır:

## <a name="fully-managed-paas"></a>Tam olarak yönetilen PaaS

Olay Hub'ları, çok az yapılandırma veya yönetim yüküyle tam olarak yönetilen bir Hizmet Olarak Platform (PaaS) olduğundan, iş çözümlerinize odaklanırsınız. [Apache Kafka ekosistemleri için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md), kümelerinizi yönetmeye, yapılandırmaya veya çalıştırmaya gerek kalmadan PaaS Kafka deneyimi sunar.

## <a name="support-for-real-time-and-batch-processing"></a>Gerçek zamanlı ve toplu işlem desteği

Eyleme dönüştürülebilir içgörüler elde etmek için akışınızı gerçek zamanlı olarak ekleyin, arabelleğe alın, depolayın ve işleyin. Olay Hub'ları, akışı aynı anda işlemek için birden çok uygulama sağlayan ve işleme hızını denetlemenize olanak tanıyan [bölümlenmiş](event-hubs-scalability.md#partitions)bir tüketici modeli kullanır.

Uzun süreli bekletme veya mikro toplu işlem için verilerinizi bir [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/) veya [Azure Veri Gölü Depolama'da](https://azure.microsoft.com/services/data-lake-store/) neredeyse gerçek zamanlı olarak [yakalayın.](event-hubs-capture-overview.md) Bu davranışı, gerçek zamanlı analiz ler elde etmek için kullandığınız akışta elde edebilirsiniz. Olay verilerinin yakalanması nı ayarlamak hızlıdır. Çalıştırmak için hiçbir yönetim maliyeti vardır ve olay [hub'ları iş letme birimleri](event-hubs-scalability.md#throughput-units)ile otomatik olarak ölçeklenir. Olay Hub'ları, veri yakalama yerine veri işlemeye odaklanmanızı sağlar.

Azure Event Hubs, sunucusuz mimari için [Azure İşlevleri](/azure/azure-functions/) ile de tümleştirilebilir.

## <a name="scalable"></a>Ölçeklenebilir

Event Hubs ile megabayt boyutunda veri akışlarıyla başlayıp gigabayt veya terabayt boyutlarına ulaşabilirsiniz. [Otomatik şişme](event-hubs-auto-inflate.md) özelliği, aktarım hızı birimlerini kullanım ihtiyaçlarınıza göre ölçeklendirmek için kullanabileceğiniz birçok özellikten bir tanesidir.

## <a name="rich-ecosystem"></a>Zengin ekosistem

[Apache Kafka ekosistemleri için Etkinlik Hub'ları,](event-hubs-for-kafka-ecosystem-overview.md) [Apache Kafka (1.0 ve sonrası)](https://kafka.apache.org/) istemcilerinin ve uygulamalarının Event Hub'ları ile konuşmasını sağlar. Kendi Kafka kümelerinizi kurmanız, yapılandırmanız ve yönetmeniz gerekmez.

Çeşitli dillerde mevcut olan geniş bir ekosistem [ile .NET](https://github.com/Azure/azure-sdk-for-net/), [Java](https://github.com/Azure/azure-sdk-for-java/), [Python](https://github.com/Azure/azure-sdk-for-python/), [JavaScript](https://github.com/Azure/azure-sdk-for-js/), Kolayca Olay Hub'larından akışları işleme başlayabilirsiniz. Desteklenen tüm istemci dilleri, düşük düzeyde tümleştirme sağlar. Ekosistem ayrıca Azure Akış Analizi ve Azure Fonksiyonları gibi Azure hizmetleriyle sorunsuz entegrasyon sağlar ve böylece sunucusuz mimariler oluşturmanıza olanak tanır.

## <a name="key-architecture-components"></a>Temel mimari bileşenler
Event Hubs şu [temel bileşenleri](event-hubs-features.md) içerir:

- **Olay oluşturucuları**: Olay hub'ına veri gönderen tüm varlıklardır. Olay yayımcıları HTTPS, AMQP 1.0 veya Apache Kafka (1.0 ve üzeri) kullanarak olayları yayımlayabilir
- **Bölümler**: Her tüketici ileti akışının yalnızca belirli bir alt kümesini veya bölümünü okur.
- **Tüketici grupları**: Tüm olay hub'ının bir görünümüdür (durum, konum veya uzaklık). Tüketici grupları, her biri için olay akışı ayrı bir görünüme sahip tüketim uygulamaları sağlar. Akışı kendi hızlarında ve kendi ofsetleriyle bağımsız olarak okurlar.
- **Aktarım hızı birimleri**:Event Hubs'ın aktarım hızı kapasitesini denetleyen önceden satın alınan kapasite birimleridir.
- **Olay alıcıları**: Bir olay hub'ından olay verilerini okuyan tüm varlıklardır. Tüm Event Hub'ları tüketicileri AMQP 1.0 oturumu aracılığıyla bağlanır. Etkinlik Hub'ları hizmeti, etkinlikleri kullanılabilir olduklarında bir oturum aracılığıyla sunar. Tüm Kafka tüketicileri Kafka protokol 1.0 ve üzeri ile bağlanır.

Aşağıdaki şekilde Event Hubs akış işleme mimarisi gösterilmektedir:

![Event Hubs](./media/event-hubs-about/event_hubs_architecture.png)

## <a name="event-hubs-on-azure-stack-hub"></a>Azure Yığını Hub'ındaki Etkinlik Hub'ları
Azure Yığını Hub'ındaki Etkinlik Hub'ları karma bulut senaryolarını gerçekleştirmenize olanak tanır. Akış ve olay tabanlı çözümler, hem şirket içi hem de Azure bulut işleme için desteklenir. Senaryonuz karma (bağlı) veya bağlantısı kesilmiş olsun, çözümünüz olayların/akışların büyük ölçekte işlenmesini destekleyebilir. Senaryonuz yalnızca gereksinimlerinize göre sağlayabileceğiniz Olay Hub'ları küme boyutuna bağlıdır. 

Etkinlik Hub'ları sürümleri (Azure Stack Hub'da ve Azure'da) yüksek derecede özellik paritesi sunar. Bu eşlik, SDK'lar, numuneler, PowerShell, CLI ve portalların birkaç farklılıkla benzer bir deneyim sunabileceği anlamına gelir. 

Yığındaki Olay Hub'ları genel önizleme sırasında ücretsizdir. Daha fazla bilgi için [Azure Yığını Hub'ına genel bakışta Olay Hub'larına](https://docs.microsoft.com/azure-stack/user/event-hubs-overview)bakın.


## <a name="next-steps"></a>Sonraki adımlar

Olay Hub'larını kullanmaya başlamak için Gönder ve etkinlik öğreticilerini **alın:**

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](get-started-java-send-v2.md)
- [Python](get-started-python-send-v2.md)
- [JavaScript](get-started-node-send-v2.md)
- [Git](event-hubs-go-get-started-send.md)
- [C (yalnızca gönderme)](event-hubs-c-getstarted-send.md)
- [Apache Storm (yalnızca alma)](event-hubs-storm-getstarted-receive.md)


Event Hubs hakkında daha fazla bilgi edinmek için şu makalelere göz atın:

- [Event Hubs özelliklerine genel bakış](event-hubs-features.md)
- [Sık sorulan sorular](event-hubs-faq.md).


