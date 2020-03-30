---
title: Azure Service Fabric Mesh uygulamalarında izleme ve tanılama
description: Azure'daki Service Fabric Mesh'te uygulamayı izleme ve tanılama hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: srrengar
ms.custom: mvc, devcenter
ms.openlocfilehash: 247a1de4d00668371337295616d31caf101f0cc5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75498154"
---
# <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama
Azure Service Fabric Mesh, geliştiricilerin sanal makineleri, depolama alanını veya ağ bileşenlerini yönetmeden mikro hizmet uygulamaları dağıtmasını sağlayan tam olarak yönetilen bir hizmettir. Service Fabric Mesh için izleme ve tanılama üç ana tanılama veritürüne ayrılır:

- Uygulama günlükleri - bunlar, uygulamanızı nasıl alet ettiğinize (örn. docker günlükleri) bağlı olarak, konteyner uygulamalarınızdaki günlükler olarak tanımlanır.
- Platform etkinlikleri - Konteyner etkinleştirme, devre dışı bırakma ve sonlandırma dahil olmak üzere şu anda konteyner işleminizle ilgili Mesh platformundan olaylar.
- Konteyner ölçümleri - konteynerleriniz için kaynak kullanımı ve performans ölçümleri (docker istatistikleri)

Bu makalede, mevcut en son önizleme sürümü için izleme ve tanılama seçenekleri açıklanır.

## <a name="application-logs"></a>Uygulama günlükleri

Konteyner başına dağıtılan konteynerlerinizden docker günlüklerinizi görüntüleyebilirsiniz. Service Fabric Mesh uygulama modelinde, her konteyner uygulamanızda bir kod paketidir. Kod paketiyle ilişkili günlükleri görmek için aşağıdaki komutu kullanın:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --app-name <nameOfApp> --service-name <nameOfService> --replica-name <nameOfReplica> --code-package-name <nameOfCodePackage>
```

> [!NOTE]
> Yineleme adını almak için "az mesh hizmet çoğaltma" komutunu kullanabilirsiniz. Yineleme adları 0'dan itibaren tümseleri artmaktadır.

Oylama uygulamasından VotingWeb.Code konteynerindeki günlükleri görmek için bu durum şu şekilde dir:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --application-name SbzVoting --service-name VotingWeb --replica-name 0 --code-package-name VotingWeb.Code
```

## <a name="container-metrics"></a>Konteyner ölçümleri 

Kafes ortamı, kapsayıcılarınızın nasıl performans gösterdiğini gösteren bir avuç ölçüm ortaya çıkarır. Aşağıdaki ölçümler, Azure portalı ve Azure monitörü CLI üzerinden kullanılabilir:

| Ölçüm | Açıklama | Birimler|
|----|----|----|
| CpuKullanımı | Yüzde olarak ActualCpu/AllocatedCpu | % |
| MemoryKullanımation | Yüzde olarak ActualMem/AllocatedMem | % |
| TahsisCpu | Azure Kaynak Yöneticisi şablonuna göre ayrılan cpu | Millicores |
| Ayrılmış Bellek | Azure Kaynak Yöneticisi şablonuna göre ayrılmış bellek | MB |
| ActualCpu | CPU kullanımı | Millicores |
| Fiili Bellek | Bellek kullanımı | MB |
| Konteyner Durumu | 0 - Geçersiz: Konteyner durumu bilinmiyor <br> 1 - Beklemede: Konteyner başlatmak için zamanlanmış <br> 2 - Başlangıç: Konteyner başlangıç sürecinde <br> 3 - Başladı: Konteyner başarıyla başladı <br> 4 - Durdurma: Konteyner durduruluyor <br> 5 - Durduruldu: Konteyner başarıyla durduruldu | Yok |
| Başvuru Durumu | 0 - Bilinmiyor: Durum alınamaz <br> 1 - Hazır: Uygulama başarıyla çalışıyor <br> 2 - Yükseltme: Devam eden bir yükseltme var <br> 3 - Oluşturma: Uygulama oluşturuluyor <br> 4 - Silme: Uygulama siliniyor <br> 5 - Başarısız: Uygulama dağıtmak için başarısız oldu | Yok |
| Hizmet Durumu | 0 - Geçersiz: Hizmet şu anda bir sağlık durumu yok <br> 1 - Ok: Hizmet sağlıklı  <br> 2 - Uyarı: Soruşturma gerektiren yanlış bir şey olabilir <br> 3 - Hata: Soruşturma gerektiren yanlış bir şey var <br> 4 - Bilinmiyor: Durum alınamaz | Yok |
| ServiceReplicaStatus | 0 - Geçersiz: Yineleme şu anda bir sağlık durumu yok <br> 1 - Ok: Hizmet sağlıklı  <br> 2 - Uyarı: Soruşturma gerektiren yanlış bir şey olabilir <br> 3 - Hata: Soruşturma gerektiren yanlış bir şey var <br> 4 - Bilinmiyor: Durum alınamaz | Yok | 
| Yeniden BaşlatmaSayısı | Kapsayıcı yeniden başlatma sayısı | Yok |

> [!NOTE]
> ServiceStatus ve ServiceReplicaStatus değerleri, Hizmet Dokusundaki [HealthState](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet) ile aynıdır. 

Her metrik farklı boyutlarda kullanılabilir, böylece farklı düzeylerde agregalar görebilirsiniz. Boyutların geçerli listesi aşağıdaki gibidir:

* ApplicationName
* ServiceName
* ServiceReplicaName
* CodePackageName

> [!NOTE]
> CodePackageName boyutu Linux uygulamaları için kullanılamaz. 

Her [boyut, Service Fabric Application modelinin](service-fabric-mesh-service-fabric-resources.md#applications-and-services) farklı bileşenlerine karşılık gelir

### <a name="azure-monitor-cli"></a>Azure Monitör CLI

Komutların tam listesi [Azure Monitor CLI dokümanlarında](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list) mevcuttur, ancak aşağıda birkaç yararlı örnek ekledik 

Her örnekte, Kaynak Kimliği bu deseni izler

`"/subscriptions/<your sub ID>/resourcegroups/<your RG>/providers/Microsoft.ServiceFabricMesh/applications/<your App name>"`


* Bir uygulamada kapsayıcıların CPU kullanımı

```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization"
```
* Her Hizmet Çoğaltma için Bellek Kullanımı
```cli
    az monitor metrics list --resource <resourceId> --metric "MemoryUtilization" --dimension "ServiceReplicaName"
``` 

* 1 saatlik bir süre içinde her kapsayıcı için yeniden başlatılır 
```cli
    az monitor metrics list --resource <resourceId> --metric "RestartCount" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z
``` 

* 1 saatlik bir pencerede "VotingWeb" adlı hizmetler arasında ortalama CPU Kullanımı
```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z --aggregation "Average" --filter "ServiceName eq 'VotingWeb'"
``` 

### <a name="metrics-explorer"></a>Ölçümler gezgini

Metrics explorer, Ağ uygulamanızın tüm ölçümlerini görselleştirebileceğiniz portaldaki bir bıçaktır. Bu bıçak, uygulamanın portaldaki sayfasından ve Azure Monitörü'ndeki Azure monitör bıçaklarından erişilebilir ve bunlardan ikincisi Azure Monitor'u destekleyen tüm Azure kaynaklarınız için ölçümleri görüntülemek için kullanabilirsiniz. 

![Ölçüm Gezgini](./media/service-fabric-mesh-monitoring-diagnostics/metricsexplorer.png)


<!--
### Container Insights

In addition to the metrics explorer, we also have a dashboard available out of the box that shows sample metrics over time under the Insights blade in the application's page in the portal. 

![Container Insights](./media/service-fabric-mesh-monitoring-diagnostics/containerinsights.png)
-->

## <a name="next-steps"></a>Sonraki adımlar
* Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.
* Azure Monitör ölçümleri komutları hakkında daha fazla bilgi edinmek için [Azure Monitor CLI dokümanlarına](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list)göz atın.
