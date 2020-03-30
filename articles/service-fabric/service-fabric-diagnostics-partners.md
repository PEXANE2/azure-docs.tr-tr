---
title: Azure Hizmet Kumaş İzleme İş Ortakları
description: İş ortağı izleme çözümleriyle Azure Hizmet Dokusu uygulamalarını, kümelerini ve altyapıyı nasıl izleyeceğinizi öğrenin.
author: srrengar
ms.topic: article
ms.date: 10/16/2018
ms.author: srrengar
ms.openlocfilehash: fd24d65ebdf4e458870819286024d1ea2e13d83e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645727"
---
# <a name="azure-service-fabric-monitoring-partners"></a>Azure Hizmet Kumaş İzleme İş Ortakları

Bu makalede, bir avuç iş ortağı çözümleri ile hizmet kumaş uygulamaları, kümeleri ve altyapı izleyebilirsiniz nasıl gösterilebilir. Hizmet Kumaşı için entegre teklifler oluşturmak için aşağıdaki ortakların her biriyle birlikte çalıştık.

## <a name="dynatrace"></a>Dynatrace

Dynatrace ile entegrasyonumuz, Service Fabric kümelerinizi izlemek için kutudan birçok özellik sağlar. Dynatrace OneAgent'ı VMSS örneklerinize yüklemek, performans sayaçları ve Hizmet Kumaşı dağıtımınızın topolojisini Uygulama düzeyine kadar indirebilirsiniz. Dynatrace aynı zamanda şirket içi izleme için de mükemmel bir seçimdir. Kümenizde Dynatrace'i etkinleştirmek için [duyuruda](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) ve [talimatlarda](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) listelenen özelliklerden daha fazlasına göz atın. 

## <a name="datadog"></a>Datadog

Datadog hem Windows hem de Linux örnekleri için VMSS için bir uzantısı vardır. Datadog'u kullanarak Windows etkinlik günlüklerini toplayabilir ve böylelikle Windows'da Service Fabric platform olaylarını toplayabilirsiniz. Tanılama verilerinizi [Datadog'a](https://www.datadoghq.com/blog/azure-monitoring-enhancements/#integrate-with-azure-service-fabric)nasıl göndereceğinize ilişkin talimatlara buradan göz atın.

## <a name="appdynamics"></a>AppDynamics

AppDynamics ile Servis Kumaşı entegrasyonu uygulama düzeyindedir. Ortam değişkenlerini güncelleyerek ve App Dynamics NuGets'i kullanarak AppDynamics'e uygulama telemetrisi gönderebilirsiniz. .NET Service Fabric uygulamalarınızın AppDynamics ile nasıl entegre edilebildiğiniz le ilgili [bu talimatlara](https://docs.appdynamics.com/display/AZURE/Install+AppDynamics+for+Azure+Service+Fabric) bakın.

## <a name="new-relic"></a>New Relic

Yeni Emanet, Service Fabric uygulamalarıyla iyi bir şekilde bütünleşen başka bir Uygulama Performans Yönetimi aracıdır. Yeni Emanet NuGet paketlerini yükleyebilir ve uygulama telemetrinizi Yeni Emanet'e göndermek için manifesto dosyalarınıza belirli ortam değişkenleri ekleyebilirsiniz. .NET Service Fabric uygulamalarınız için Yeni Emanet telemetrisini etkinleştirmek için bu [talimatlara](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/install-net-agent-azure-service-fabric) göz atın.

## <a name="elk"></a>Elk 

ELK yığını açık kaynak teknolojileri topluluğudur: Elasticsearch, Logstash ve Kibana. Bu teknolojileri birlikte kullanarak Service Fabric izleme ve tanılama verilerini toplayabilir, depolayabilir ve analiz edebilirsiniz. Burada Service Fabric yerli Java [uygulamaları](service-fabric-tutorial-java-elk.md)ile bunu yapmak için nasıl bir öğretici var. 

## <a name="humio"></a>Humio

Humio, uygulamalarınızın ve etkinliklerinizden bulutta veya şirket içinde Hizmet Kumaşı'ndan gerçek zamanlı olarak günlükleri toplayabilen bir günlük toplama hizmetidir. Humio, canlı gözlemlenebilirliğe ek olarak, tanılamanızdan bilgi almak ve görüntülemek için en son teknolojiye sahip analiz ve görselleştirme özellikleri sunar. Humio maliyet etkin fiyatlandırma planları vardır ve hızlı aydınlatma's koruyarak ölçeklendirmek için inşa edilmiştir. Doğrudan Service Fabric platform olayları ve Uygulama telemetri ile entegre. [Burada](https://github.com/humio/service-fabric-humio)Humio ve Hizmet Kumaş entegrasyonu hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Service Fabric'te [izleme ve tanılama hakkında genel bilgi](service-fabric-diagnostics-overview.md) alın
* İlk taraf araçlarımızla [sık kullanılan senaryoları](service-fabric-diagnostics-common-scenarios.md) nasıl tanılayarak tanılamayı öğrenin
