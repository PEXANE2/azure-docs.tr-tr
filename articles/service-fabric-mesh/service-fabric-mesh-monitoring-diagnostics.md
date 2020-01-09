---
title: Azure Service Fabric kafes uygulamalarında izleme ve tanılama
description: Azure 'da Service Fabric ağı 'nda uygulama izleme ve tanılama hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: srrengar
ms.custom: mvc, devcenter
ms.openlocfilehash: 247a1de4d00668371337295616d31caf101f0cc5
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75498154"
---
# <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama
Azure Service Fabric Mesh, geliştiricilerin sanal makineleri, depolama alanını veya ağ bileşenlerini yönetmeden mikro hizmet uygulamaları dağıtmasını sağlayan tam olarak yönetilen bir hizmettir. Service Fabric ağ için izleme ve tanılama, üç temel tanılama verisi türüne kategorize edilir:

- Uygulama günlükleri-Bunlar, uygulamanızı nasıl kapatdığınıza (ör. Docker günlükleri) göre Kapsayıcılı uygulamalardaki Günlükler olarak tanımlanmıştır
- Platform olayları-şu anda kapsayıcı etkinleştirme, devre dışı bırakma ve sonlandırma dahil olmak üzere, ağ platformunun kapsayıcı işleminizi ilgilendiren olaylar.
- Kapsayıcı ölçümleri-kapsayıcılarınız için kaynak kullanımı ve performans ölçümleri (Docker istatistikleri)

Bu makalede, kullanılabilir en son önizleme sürümü için izleme ve tanılama seçenekleri açıklanmaktadır.

## <a name="application-logs"></a>Uygulama günlükleri

Her kapsayıcı için dağıtılan kapsayıcılarınızdaki Docker günlüklerinizi görüntüleyebilirsiniz. Service Fabric kafes uygulama modelinde, her kapsayıcı uygulamanızdaki bir kod paketidir. Bir kod paketiyle ilişkili günlükleri görmek için aşağıdaki komutu kullanın:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --app-name <nameOfApp> --service-name <nameOfService> --replica-name <nameOfReplica> --code-package-name <nameOfCodePackage>
```

> [!NOTE]
> Çoğaltma adını almak için "az kafes Service-Replica" komutunu kullanabilirsiniz. Çoğaltma adları 0 ' dan tamsayıları artırmadır.

Bu, oylama uygulamasındaki VotingWeb. Code kapsayıcısından günlükleri görmek için şöyle görünür:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --application-name SbzVoting --service-name VotingWeb --replica-name 0 --code-package-name VotingWeb.Code
```

## <a name="container-metrics"></a>Kapsayıcı ölçümleri 

Ağ ortamı, kapsayıcılarınızın nasıl gerçekleştiğini gösteren bir dizi ölçüm sunar. Aşağıdaki ölçümler Azure portal ve Azure izleyici CLı aracılığıyla sunulmaktadır:

| Ölçüm | Açıklama | Birimler|
|----|----|----|
| Cpukullanımı | Yüzde olarak ActualCpu/Ayrıtedcpu | % |
| Memoryutilileştirme | Yüzde olarak ActualMem/Ayrıtedbellek | % |
| Ayrılatedcpu | Azure Resource Manager şablona göre ayrılan CPU | Milicore |
| Ayrılan bellek | Azure Resource Manager şablonuna göre ayrılan bellek | MB |
| ActualCpu | CPU kullanımı | Milicore |
| ActualMemory | Bellek kullanımı | MB |
| ContainerStatus | 0-geçersiz: kapsayıcı durumu bilinmiyor <br> 1-bekliyor: kapsayıcı başlatılmaya zamanlandı <br> 2-başlatılıyor: kapsayıcı başlangıç sürecinde <br> 3-başlatıldı: kapsayıcı başarıyla başlatıldı <br> 4-durduruluyor: kapsayıcı durduruluyor <br> 5-durduruldu: kapsayıcı başarıyla durduruldu | Yok |
| ApplicationStatus | 0-bilinmiyor: durum alınabilir değil <br> 1-Ready: uygulama başarıyla çalışıyor <br> 2-yükseltiliyor: sürmekte olan bir yükseltme var <br> 3-oluşturma: uygulama oluşturuluyor <br> 4-silme: uygulama siliniyor <br> 5-başarısız: uygulama dağıtılamadı | Yok |
| ServiceStatus | 0-geçersiz: hizmetin Şu anda bir sistem durumu yok <br> 1-Tamam: hizmet sağlıklı  <br> 2-Uyarı: araştırma gerektiren yanlış bir sorun olabilir <br> 3-hata: araştırma gerektiren bir sorun oluştu <br> 4-bilinmiyor: durum alınabilir değil | Yok |
| Servicereperepstatus | 0-geçersiz: çoğaltmanın Şu anda bir sistem durumu yok <br> 1-Tamam: hizmet sağlıklı  <br> 2-Uyarı: araştırma gerektiren yanlış bir sorun olabilir <br> 3-hata: araştırma gerektiren bir sorun oluştu <br> 4-bilinmiyor: durum alınabilir değil | Yok | 
| RestartCount | Kapsayıcı yeniden başlatmaları sayısı | Yok |

> [!NOTE]
> ServiceStatus ve Servicereperepstatus değerleri, Service Fabric [HealthState](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet) ile aynıdır. 

Her ölçüm farklı boyutlarda kullanılabilir ve bu sayede toplamaları farklı düzeylerde görebilirsiniz. Geçerli boyut listesi aşağıdaki gibidir:

* ApplicationName
* serviceName
* ServiceReplicaName
* CodePackageName

> [!NOTE]
> CodePackageName Dimension, Linux uygulamaları için kullanılamaz. 

Her boyut [Service Fabric uygulama modelinin](service-fabric-mesh-service-fabric-resources.md#applications-and-services) farklı bileşenlerine karşılık gelir

### <a name="azure-monitor-cli"></a>Azure Izleyici CLı

Komutların tam listesi [Azure IZLEYICI CLI belgelerinden](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list) edinilebilir ancak aşağıda birkaç faydalı örnek verilmiştir 

Her örnekte, kaynak KIMLIĞI bu düzene uyar

`"/subscriptions/<your sub ID>/resourcegroups/<your RG>/providers/Microsoft.ServiceFabricMesh/applications/<your App name>"`


* Bir uygulamadaki kapsayıcıların CPU kullanımı

```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization"
```
* Her hizmet çoğaltması için bellek kullanımı
```cli
    az monitor metrics list --resource <resourceId> --metric "MemoryUtilization" --dimension "ServiceReplicaName"
``` 

* 1 saatlik penceredeki her bir kapsayıcı için yeniden başlatmalar 
```cli
    az monitor metrics list --resource <resourceId> --metric "RestartCount" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z
``` 

* 1 saatlik bir pencerede "VotingWeb" adlı hizmetler genelinde ortalama CPU kullanımı
```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z --aggregation "Average" --filter "ServiceName eq 'VotingWeb'"
``` 

### <a name="metrics-explorer"></a>Ölçüm Gezgini

Ölçüm Gezgini, portaldaki, kafes uygulamanız için tüm ölçümleri görselleştirebileceğiniz bir dikey pencere. Bu dikey pencere, portalın ve Azure izleyici dikey penceresindeki uygulamanın sayfasında, Azure Izleyicisini destekleyen tüm Azure kaynaklarınızın ölçümlerini görüntülemek için kullanabileceğiniz Azure izleyici dikey penceresinde erişilebilir. 

![Ölçüm Gezgini](./media/service-fabric-mesh-monitoring-diagnostics/metricsexplorer.png)


<!--
### Container Insights

In addition to the metrics explorer, we also have a dashboard available out of the box that shows sample metrics over time under the Insights blade in the application's page in the portal. 

![Container Insights](./media/service-fabric-mesh-monitoring-diagnostics/containerinsights.png)
-->

## <a name="next-steps"></a>Sonraki adımlar
* Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.
* Azure Izleyici ölçümleri komutları hakkında daha fazla bilgi edinmek için [Azure IZLEYICI CLI belgelerine](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list)göz atın.
