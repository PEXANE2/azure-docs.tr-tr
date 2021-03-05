---
title: Günlükleri kapsayıcı öngörülerine sorgulama | Microsoft Docs
description: Kapsayıcı öngörüleri, ölçümleri ve günlük verilerini toplar ve bu makalede kayıtları açıklanmakta ve örnek sorgular yer almaktadır.
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: c2b7331255e1109f27f89a84d66e25eb07a20569
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102201388"
---
# <a name="how-to-query-logs-from-container-insights"></a>Kapsayıcı öngörülerine ait günlükleri sorgulama

Kapsayıcı öngörüleri, kapsayıcı konakları ve kapsayıcılarından performans ölçümlerini, envanter verilerini ve sistem durumu bilgilerini toplar. Veriler her üç dakikada bir toplanır ve Azure Izleyici 'de Log Analytics çalışma alanına iletilir. Bu veriler Azure Izleyici 'de [sorgu](../logs/log-query-overview.md) için kullanılabilir. Bu verileri, geçiş planlama, Kapasite Analizi, bulma ve isteğe bağlı performans sorunlarını gidermeyle ilgili senaryolara uygulayabilirsiniz.

## <a name="container-records"></a>Kapsayıcı kayıtları

Aşağıdaki tabloda, kapsayıcı öngörüleri tarafından toplanan kayıtların ayrıntıları verilmiştir. Sütun açıklamalarının bir listesi için bkz. [Containerınventory](/azure/azure-monitor/reference/tables/containerinventory) ve [ContainerLog](/azure/azure-monitor/reference/tables/containerlog) tabloları için başvuru.

| Veriler | Veri kaynağı | Veri türü | Alanlar |
|------|-------------|-----------|--------|
| Kapsayıcı envanteri | Kubelet | `ContainerInventory` | TimeGenerated, bilgisayar, ad, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, komut, CreatedTime, StartedTime, Sonlandırhedtime, dir, Containerıd, ImageID |
| Kapsayıcı günlüğü | Docker | `ContainerLog` | TimeGenerated, Computer, Image ID, Name, LogEntrySource, LogEntry, dir, Containerıd |
| Kapsayıcı düğümü envanteri | Kuin API 'SI | `ContainerNodeInventory`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, dir|
| Bir Kubernetes kümesinde Pod sayımı | Kuin API 'SI | `KubePodInventory` | TimeGenerated, Computer, Clusterıd, ContainerCreationTimeStamp, Poduıd, Pod Creationtimestamp, ContainerRestartCount, PodRestartCount, Pod StartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus, ContainerStatusReason, Containerıd, ContainerName, Name, Pod etiketi, Namespace, Pod durum, ClusterName, Podıp, dir |
| Bir Kubernetes kümesinin bir parçası olan düğümlerin envanteri | Kuin API 'SI | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, Clusterıd, Lastgeçişli Tiontimeready, Etiketler, durum, KubeletVersion, KubeProxyVersion, CreationTimeStamp, dir | 
|Bir Kubernetes kümesindeki kalıcı birimlerin envanterini oluşturma |Kuin API 'SI |`KubePVInventory` | TimeGenerated, PVName, Pvcapacitybyte, PVCName, PVCNamespace, PVStatus, Pvaccessmode, PVType, Pvtypeınfo, PVStorageClassName, PVCreationTimestamp, Clusterıd, ClusterName, _ResourceId, dir |
| Kubernetes Olayları | Kuin API 'SI | `KubeEvents` | TimeGenerated, bilgisayar, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Ileti, dir | 
| Kubernetes kümesindeki hizmetler | Kuin API 'SI | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, dir | 
| Kubernetes kümesinin düğümlerin bir parçası için performans ölçümleri | Kullanım ölçümleri, Cadvizörü 'den ve Kuto API 'si sınırlarından alınır | `Perf \| where ObjectName == "K8SNode"` | Bilgisayar, ObjectName, CounterName &#40;Cpuallocatablenanoçekirdekler, memoryAllocatableBytes, cpuCapacityNanoCores, Memorycapacitybyte, memoryRssBytes, Cpuusme Anoçekirdekleri, memoryWorkingsetBytes, restartTimeEpoch&#41;, CounterValue, TimeGenerated, CounterPath, dir | 
| Kubernetes kümesinin kapsayıcılar bölümü için performans ölçümleri | Kullanım ölçümleri, Cadvizörü 'den ve Kuto API 'si sınırlarından alınır | `Perf \| where ObjectName == "K8SContainer"` | CounterName &#40;cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryWorkingSetBytes, restartTimeEpoch, Cpuuslationanoçekirdekler, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath, dir | 
| Özel ölçümler ||`InsightsMetrics` | Bilgisayar, ad, ad alanı, Origin, dir, Etiketler<sup>1</sup>, TimeGenerated, Type, Va, _ResourceId | 

<sup>1</sup> *Etiketler* özelliği, karşılık gelen ölçüm için [birden çok boyutu](../essentials/data-platform-metrics.md#multi-dimensional-metrics) temsil eder. Tabloda toplanan ve depolanan ölçümler ve kayıt özelliklerinin açıklaması hakkında daha fazla bilgi için `InsightsMetrics` bkz. [ınsightsölçümlerini genel bakış](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md).

## <a name="search-logs-to-analyze-data"></a>Verileri çözümlemek için günlüklere arama yapın

Azure Izleyici günlükleri, geçerli küme yapılandırmasının en iyi şekilde performans yapıp gerçekleştirmediğini belirlemenize yardımcı olabilecek eğilimleri bulmanıza, performans sorunlarını tanılamanıza, tahmin etmenize veya aralarındaki verileri ilişkilendirmenize yardımcı olabilir. Önceden tanımlanmış günlük aramaları, bilgileri istediğiniz şekilde döndürmek için veya ' i hemen kullanmaya başlayabilmeniz için sağlanır.

**Analiz bölmesindeki Görünüm** açılır listesinden **Kubernetes olay günlüklerini görüntüle** veya **kapsayıcı günlüklerini görüntüle** seçeneğini belirleyerek çalışma alanındaki verileri etkileşimli bir şekilde çözümleyebilirsiniz. **Günlük araması** sayfası, üzerinde olduğunuz Azure Portal sayfanın sağında görüntülenir.

![Log Analytics’te verileri analiz etme](./media/container-insights-analyze/container-health-log-search-example.png)

Çalışma alanınıza iletilen kapsayıcı günlüğü çıkışları STDOUT ve STDERR ' dir. Azure Izleyici, Azure tarafından yönetilen Kubernetes (AKS) ' i izlerken, üretilen verilerin büyük hacmi nedeniyle Kuto-System bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları

Genellikle bir örnekle başlayan sorgular oluşturmak ve sonra gereksinimlerinize uyacak şekilde değiştirmek yararlıdır. Daha gelişmiş sorgular oluşturmaya yardımcı olmak için aşağıdaki örnek sorgularla denemeler yapabilirsiniz:

### <a name="list-all-of-a-containers-lifecycle-information"></a>Kapsayıcının yaşam döngüsü bilgilerinin tümünü listeleme

```kusto
ContainerInventory
| project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime
| render table
```

### <a name="kubernetes-events"></a>Kubernetes olayları

``` kusto
KubeEvents_CL
| where not(isempty(Namespace_s))
| sort by TimeGenerated desc
| render table
```
### <a name="image-inventory"></a>Görüntü envanteri

``` kusto
ContainerImageInventory
| summarize AggregatedValue = count() by Image, ImageTag, Running
```

### <a name="container-cpu"></a>Kapsayıcı CPU 'SU

**Çizgi grafik görüntüleme seçeneğini belirleyin**

``` kusto
Perf
| where ObjectName == "K8SContainer" and CounterName == "cpuUsageNanoCores" 
| summarize AvgCPUUsageNanoCores = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName 
```

### <a name="container-memory"></a>Kapsayıcı belleği

**Çizgi grafik görüntüleme seçeneğini belirleyin**

```kusto
Perf
| where ObjectName == "K8SContainer" and CounterName == "memoryRssBytes"
| summarize AvgUsedRssMemoryBytes = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName
```

### <a name="requests-per-minute-with-custom-metrics"></a>Özel ölçümler ile dakika başına istek

```kusto
InsightsMetrics
| where Name == "requests_count"
| summarize Val=any(Val) by TimeGenerated=bin(TimeGenerated, 1m)
| sort by TimeGenerated asc<br> &#124; project RequestsPerMinute = Val - prev(Val), TimeGenerated
| render barchart 
```

## <a name="query-prometheus-metrics-data"></a>Sorgu Prometheus ölçüm verileri

Aşağıdaki örnek, düğüm başına disk başına saniye başına disk okuma gösteren bir Prometheus ölçüm sorgusudur.

```
InsightsMetrics
| where Namespace == 'container.azm.ms/diskio'
| where TimeGenerated > ago(1h)
| where Name == 'reads'
| extend Tags = todynamic(Tags)
| extend HostName = tostring(Tags.hostName), Device = Tags.name
| extend NodeDisk = strcat(Device, "/", HostName)
| order by NodeDisk asc, TimeGenerated asc
| serialize
| extend PrevVal = iif(prev(NodeDisk) != NodeDisk, 0.0, prev(Val)), PrevTimeGenerated = iif(prev(NodeDisk) != NodeDisk, datetime(null), prev(TimeGenerated))
| where isnotnull(PrevTimeGenerated) and PrevTimeGenerated != TimeGenerated
| extend Rate = iif(PrevVal > Val, Val / (datetime_diff('Second', TimeGenerated, PrevTimeGenerated) * 1), iif(PrevVal == Val, 0.0, (Val - PrevVal) / (datetime_diff('Second', TimeGenerated, PrevTimeGenerated) * 1)))
| where isnotnull(Rate)
| project TimeGenerated, NodeDisk, Rate
| render timechart

```

Azure Izleyici tarafından ad alanına göre filtrelenmiş Prometheus ölçümlerini görüntülemek için "Prometheus" seçeneğini belirtin. Kubernetes ad alanından Prometheus ölçümlerini görüntülemek için örnek bir sorgu aşağıda verilmiştir `default` .

```
InsightsMetrics 
| where Namespace == "prometheus"
| extend tags=parse_json(Tags)
| summarize count() by Name
```

Prometheus verileri, ad ile doğrudan sorgulanabilir.

```
InsightsMetrics 
| where Namespace == "prometheus"
| where Name contains "some_prometheus_metric"
```

### <a name="query-config-or-scraping-errors"></a>Sorgu yapılandırması veya scraping hataları

Herhangi bir yapılandırmayı veya bir hatayı araştırmak için aşağıdaki örnek sorgu, tablodan bilgi olayları döndürür `KubeMonAgentEvents` .

```
KubeMonAgentEvents | where Level != "Info" 
```

Çıktı aşağıdaki örneğe benzer sonuçları gösterir:

![Bilgi olaylarının aracıdan günlük sorgusu sonuçları](./media/container-insights-log-search/log-query-example-kubeagent-events.png)

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcı öngörüleri, önceden tanımlanmış bir uyarı kümesi içermez. DevOps veya işletimsel işlemlerinizi ve yordamlarını desteklemek üzere yüksek CPU ve bellek kullanımı için önerilen uyarılar oluşturmayı öğrenmek için [kapsayıcı öngörüleriyle performans uyarılarını oluşturma](./container-insights-log-alerts.md) ' yı gözden geçirin.