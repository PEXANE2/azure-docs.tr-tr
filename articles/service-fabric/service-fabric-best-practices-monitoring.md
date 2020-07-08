---
title: Azure Service Fabric izleme en iyi uygulamaları
description: Azure Service Fabric kullanarak kümeleri ve uygulamaları izlemeye yönelik en iyi yöntemler ve tasarım konuları.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: af03223e8b007cbd2a00d54c3076056cd110ecc9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75551825"
---
# <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

[İzleme ve tanılama](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) , iş yüklerini herhangi bir bulut ortamında geliştirmek, test etmek ve dağıtmak için önemlidir. Örneğin, uygulamalarınızın nasıl kullanıldığını, Service Fabric platformu tarafından gerçekleştirilen eylemleri, performans sayaçlarıyla kaynak kullanımınızı ve kümenizin genel durumunu izleyebilirsiniz. Sorunları tanılamak ve düzeltmek ve gelecekte oluşmasını önlemek için bu bilgileri kullanabilirsiniz.

## <a name="application-monitoring"></a>Uygulama izleme

Uygulama izleme, uygulamanızın özelliklerinin ve bileşenlerinin nasıl kullanıldığını izler. Kullanıcılarınızı etkileyen sorunların yakalanıp yakalanmeyeceğinden emin olmak için uygulamalarınızı izleyin. Uygulama izleme, uygulamanızın iş mantığı için benzersiz olduğundan, uygulamayı ve hizmetlerini geliştirmekte olan sorumluluğun sorumluluğundadır. Azure 'un uygulama izleme aracı [Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-monitoring-aspnet), uygulama izlemeyi ayarlamanız önerilir.

## <a name="cluster-monitoring"></a>Küme izleme

Service Fabric amaçlarından biri, uygulamaların donanım hatalarıyla dayanıklı olmasını sağlar. Bu hedef, platformun sistem hizmetlerinin altyapı sorunlarını algılama ve kümedeki diğer düğümlere hızlı yük devretme iş yüklerini algılama yeteneği aracılığıyla gerçekleştirilir. Ancak sistem hizmetlerinde sorun varsa ne olacak? Ya da bir iş yükünü dağıtmaya veya taşımaya çalışıyorsanız, hizmetlerin yerleştirilme kuralları ihlal edilir mi? Service Fabric, Service Fabric platformunun uygulamalar, hizmetler, kapsayıcılar ve düğümleriniz ile nasıl etkileşime gireceğini öğrenmek için bu ve diğer sorunlar için tanılama sağlar.

Windows kümeleri için, [Tanılama Aracısı](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) ve [Azure izleyici günlükleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-setup)ile küme izlemeyi ayarlamanız önerilir.

Linux kümelerinde Azure Izleyici günlükleri de Azure platformu ve altyapı izleme için önerilen araçtır. Linux platform tanılama, [Syslog 'daki Service Fabric Linux küme olayları](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-syslog)' nda belirtildiği gibi farklı yapılandırmalar gerektirir.

## <a name="infrastructure-monitoring"></a>Altyapı izleme

[Azure izleyici günlükleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-agent) , küme düzeyindeki olayları izlemek için önerilir. Önceki bağlantıda açıklandığı gibi, çalışma alanınız ile Log Analytics aracısını yapılandırdıktan sonra CPU kullanımı, işlem düzeyi CPU kullanımı gibi .NET performans sayaçları, güvenilir bir hizmetten gelen özel durum sayısı gibi performans sayaçlarını Service Fabric ve CPU kullanımı gibi kapsayıcı ölçümlerini toplayacaksınız.  Azure Izleyici günlüklerinde kullanılabilir olmaları için, stdout veya stderr 'e kapsayıcı günlükleri yazmanız gerekir.

## <a name="watchdogs"></a>Watchdogs

Genellikle, izleme, hizmetler genelinde sistem durumunu ve yükünü izleyen, uç noktalara ping yapan ve kümedeki beklenmeyen sistem durumu olaylarını raporlayan ayrı bir hizmettir. Bu, yalnızca tek bir hizmetin performansına göre algılanamayan hataları önlemeye yardımcı olabilir. Watchdogs, belirli zaman aralıklarında depolama alanındaki günlük dosyalarını temizleme gibi kullanıcı etkileşimi gerektirmeyen düzeltici eylem gerçekleştiren kodu barındırmak için de iyi bir yerdir. [Syslog 'daki Service Fabric Linux küme olaylarında](https://github.com/Azure-Samples/service-fabric-watchdog-service)örnek bir izleme hizmeti uygulamasına bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamalarınızı oluşturmaya başlayın: [uygulama düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-app.md).
* [Service Fabric bir ASP.NET Core uygulamasını izleyerek](service-fabric-tutorial-monitoring-aspnet.md)uygulamanız için Application Insights ayarlama adımlarını izleyin.
* Platformu izleme hakkında daha fazla bilgi edinin ve Service Fabric şunları sağlar: [Platform düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md).
* Azure Izleyici günlüklerini yapılandırma Service Fabric: [bir küme Için Azure izleyici günlüklerini ayarlama](service-fabric-diagnostics-oms-setup.md)
* İzleme kapsayıcıları için Azure Izleyici günlüklerini ayarlamayı öğrenin: [azure Service Fabric Windows kapsayıcıları Için izleme ve tanılama](service-fabric-tutorial-monitoring-wincontainers.md).
* Bkz. Service Fabric ile örnek Tanılama sorunları ve çözümleri: [yaygın senaryoları tanılama](service-fabric-diagnostics-common-scenarios.md)
* Azure kaynakları için genel izleme önerileri hakkında bilgi edinin: [En Iyi Yöntemler-izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).