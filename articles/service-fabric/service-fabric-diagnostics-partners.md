---
title: Azure Service Fabric Izleme Iş ortakları | Microsoft Docs
description: Azure Service Fabric iş ortağı izleme çözümleriyle izlemeyi öğrenin
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/16/2018
ms.author: srrengar
ms.openlocfilehash: 8353c8846820aa996214e6b119a2f9027b8b6fe6
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74232442"
---
# <a name="azure-service-fabric-monitoring-partners"></a>Azure Service Fabric Izleme Iş ortakları

Bu makalede, bir arada iş ortağı çözümleriyle Service Fabric uygulamalarını, kümelerini ve altyapısını nasıl izleyebileceği gösterilmektedir. Service Fabric için tümleşik teklifler oluşturmak üzere aşağıdaki iş ortaklarının her biriyle çalıştık.

## <a name="dynatrace"></a>Dynatrace

DynaTrace ile tümleştirdiğimiz Service Fabric kümelerinizi izlemek için birçok Box özelliği sunar. VMSS örneklerinizde dynaTrace OneAgent yüklemesi, performans sayaçlarını ve uygulama düzeyine Service Fabric dağıtımınızın bir topolojisini sağlar. DynaTrace Ayrıca şirket içi izleme için harika bir seçimdir. Kümenizde dynaTrace 'i etkinleştirmek için [duyuru](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) ve [yönergeler](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) bölümünde listelenen özelliklerden daha fazlasını inceleyin. 

## <a name="datadog"></a>Datadog

Dataköpek, hem Windows hem de Linux örnekleri için bir VMSS uzantısına sahiptir. Datadog kullanarak Windows olay günlüklerini toplayabilir ve bu nedenle Windows 'da Service Fabric platform olayları toplayabilirsiniz. Tanılama verilerinizi [buradaki veri adaylarına](https://www.datadoghq.com/blog/azure-monitoring-enhancements/#integrate-with-azure-service-fabric)gönderme yönergelerine göz atın.

## <a name="appdynamics"></a>AppDynamics

AppDynamics ile Service Fabric tümleştirme uygulama düzeyindedir. Ortam değişkenlerini güncelleştirerek ve App Dynamics Nual kullanarak uygulama telemetrisini AppDynamics 'e gönderebilirsiniz. .NET Service Fabric uygulamalarınızı AppDynamics ile tümleştirme hakkında daha fazla bilgi için bu [yönergelere](https://docs.appdynamics.com/display/AZURE/Install+AppDynamics+for+Azure+Service+Fabric) bakın.

## <a name="new-relic"></a>New Relic

Yeni relik, Service Fabric uygulamalarla iyi tümleşen bir başka uygulama performansı yönetim aracıdır. Yeni relik NuGet paketlerini yükleyebilir ve uygulama telemetrinizi yeni bir relik 'e göndermek için bildirim dosyalarınıza özel ortam değişkenleri ekleyebilirsiniz. .NET Service Fabric uygulamalarınız için yeni relik telemetrisini etkinleştirmek üzere bu [yönergelere](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/install-net-agent-azure-service-fabric) göz atın.

## <a name="elk"></a>ELK 

ELK yığını, bir açık kaynak teknolojileri koleksiyonudur: Elaol Search, Logstash ve kibana. Bunların birleşimini kullanarak, Service Fabric izleme ve tanılama verilerini toplayabilir, saklayabilir ve analiz edebilirsiniz. Bunu Service Fabric yerel Java uygulamalarıyla nasıl yapacağız hakkında bir öğretici [sunuyoruz.](service-fabric-tutorial-java-elk.md) 

## <a name="humio"></a>Humio

Humio, buluttan veya Şirket içindeki Service Fabric gerçek zamanlı olarak uygulama ve olaylarınızdan günlük toplayabilen bir günlük toplama hizmetidir. Humio, Live Observability 'a ek olarak, tanılamalarınıza ilişkin öngörüleri görüntülemek ve toplamak için resim analizi ve görselleştirme özellikleri sağlar. Humio, uygun maliyetli fiyatlandırma planları içerir ve bu, hızlı bir hızla hızlanırken ölçeklendirilmesi için oluşturulmuştur. Service Fabric platform olayları ve uygulama telemetrisiyle doğrudan tümleştirilir. Humio ve [Service Fabric tümleştirme hakkında](https://github.com/humio/service-fabric-humio)daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Service Fabric ' de [izleme ve tanılamaya genel bakış](service-fabric-diagnostics-overview.md) alın
* İlk taraf araçlarımızla [yaygın senaryoları tanılamayı](service-fabric-diagnostics-common-scenarios.md) öğrenin
