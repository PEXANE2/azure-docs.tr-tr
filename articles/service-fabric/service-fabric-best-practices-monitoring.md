---
title: Azure Hizmet Kumaşı en iyi uygulamaları izleme
description: Azure Hizmet Kumaşı'nı kullanarak kümeleri ve uygulamaları izlemek için en iyi uygulamalar ve tasarım konuları.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: af03223e8b007cbd2a00d54c3076056cd110ecc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551825"
---
# <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

[İzleme ve tanılama,](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) iş yüklerinin herhangi bir bulut ortamında geliştirilmesi, test edilmesi ve dağıtılması açısından önemlidir. Örneğin, uygulamalarınızın nasıl kullanıldığını, Service Fabric platformu tarafından gerçekleştirilen eylemleri, performans sayaçlarıyla kaynak kullanımınızı ve kümenizin genel durumunu izleyebilirsiniz. Bu bilgileri sorunları tanılamak ve düzeltmek ve gelecekte oluşmasını önlemek için kullanabilirsiniz.

## <a name="application-monitoring"></a>Uygulama izleme

Uygulama izleme, uygulamanızın özelliklerinin ve bileşenlerinin nasıl kullanıldığını izler. Kullanıcılarınızı etkileyen sorunların yakalandığından emin olmak için uygulamalarınızı izleyin. Uygulama izleme, uygulamanızın iş mantığına özgü olduğu için uygulamayı ve hizmetlerini geliştirenlerin sorumluluğundadır. Azure'un uygulama izleme aracı [olan Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-monitoring-aspnet)ile uygulama izleme ayarlamanız önerilir.

## <a name="cluster-monitoring"></a>Küme izleme

Service Fabric'in hedeflerinden biri, uygulamaları donanım hatalarına karşı esnek hale getirmektir. Bu hedefe, platformun sistem hizmetlerinin altyapı sorunlarını algılama ve kümedeki diğer düğümlere iş yüklerinin hızla başarısız olması sayesinde ulaşılır. Peki ya sistem hizmetlerinin kendi sorunları varsa? Veya bir iş yükünü dağıtmaya veya taşımaya çalışırken, hizmetlerin yerleşimine yönelik kurallar ihlal edilir mi? Service Fabric, Service Fabric platformunun uygulamalarınız, hizmetleri, kapları ve düğümlerinizle nasıl etkileşimde olduğu hakkında bilgilendirilmenizi sağlamak için bu ve diğer sorunlar için tanılama sağlar.

Windows kümeleri için, [Tanılama Aracısı](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) ve Azure Monitor [günlükleriyle](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-setup)küme izleme ayarlamanız önerilir.

Linux kümeleri için Azure Monitor günlükleri, Azure platformu ve altyapı izleme için önerilen araçtır. Linux platformu tanılama [Syslog Hizmet Kumaş Linux küme olaylarda](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-syslog)belirtildiği gibi farklı yapılandırma gerektirir.

## <a name="infrastructure-monitoring"></a>Altyapı izleme

Küme düzeyindeki olayları izlemek için [Azure Monitor günlükleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-agent) önerilir. Log Analytics aracısını önceki bağlantıda açıklandığı gibi çalışma alanınızla yapılandırdıktan sonra, CPU Kullanımı, .NET performans sayaçları gibi performans ölçümlerini, işlem düzeyi CPU kullanımı, güvenilir bir hizmetten istisnaların #'ı gibi Hizmet Kumaşı performans sayaçları ve CPU Kullanımı gibi konteyner ölçümleri gibi performans ölçümlerini toplayabilirsiniz.  Azure Monitor günlüklerinde kullanılabilmesi için stdout veya stderr için kapsayıcı günlükleri yazmanız gerekir.

## <a name="watchdogs"></a>Watchdogs

Genellikle, bir izleme örgütü, hizmetler arasında sağlık ve yükleme izleyen, ping uç noktaları ve kümedeki beklenmeyen sağlık olaylarını bildiren ayrı bir hizmettir. Bu, yalnızca tek bir hizmetin performansına bağlı olarak algılanamayan hataları önlemeye yardımcı olabilir. Watchdogs da belirli zaman aralıklarında depolama günlük dosyaları temizleme gibi kullanıcı etkileşimi gerektirmeyen iyileştirici eylem gerçekleştiren kod barındırmak için iyi bir yerdir. [Syslog Service Fabric Linux küme olaylarında](https://github.com/Azure-Samples/service-fabric-watchdog-service)örnek bir izleme hizmeti uygulamasına bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamalarınızı enstrümantize etmeye başlayın: [Uygulama düzeyi etkinliği ve günlük oluşturma.](service-fabric-diagnostics-event-generation-app.md)
* Monitor ile uygulamanız için Uygulama Öngörüleri'ni ayarlamak [ve Service Fabric'te ASP.NET Çekirdek uygulamasını tanılamak](service-fabric-tutorial-monitoring-aspnet.md)için gereken adımları gözden geçirin.
* Hizmet Kumaşı'nın size sunduğu platformu ve etkinlikleri izleme hakkında daha fazla bilgi edinin: [Platform düzeyinde etkinlik ve günlük oluşturma.](service-fabric-diagnostics-event-generation-infra.md)
* Azure Monitör günlükleri tümleştirmeyi Hizmet Dokusu ile yapılandırma: [Bir küme için Azure Monitor günlüklerini ayarlama](service-fabric-diagnostics-oms-setup.md)
* Kapsayıcıları izlemek için Azure Monitor günlüklerini nasıl ayarlayizleyeceğinizi öğrenin: [Azure Hizmet Dokusunda Windows Kapsayıcıları için İzleme ve Tanılama.](service-fabric-tutorial-monitoring-wincontainers.md)
* Service Fabric ile örnek tanılama sorunlarına ve çözümlerine bakın: [Sık karşılaşılan senaryoları tanılama](service-fabric-diagnostics-common-scenarios.md)
* Azure kaynakları için genel izleme önerileri hakkında bilgi edinin: [En İyi Uygulamalar - İzleme ve tanılama.](https://docs.microsoft.com/azure/architecture/best-practices/monitoring)