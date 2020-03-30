---
title: Kapsayıcılar için Azure Monitör'den Günlükler Nasıl Sorgulanır | Microsoft Dokümanlar
description: Kapsayıcılar için Azure Monitor ölçümleri ve günlük verilerini toplar ve bu makalede kayıtlar açıklanır ve örnek sorgular içerir.
ms.topic: conceptual
ms.date: 03/26/2020
ms.openlocfilehash: ff7cbff708b794847d8be69ca8f829e622d7c7ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80333470"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitor'dan günlükleri sorgulama

Kapsayıcılar için Azure Monitor, kapsayıcı ana bilgisayarlarından ve kapsayıcılardan performans ölçümleri, envanter verileri ve sistem durumu bilgilerini toplar ve Azure Monitor'daki Günlük Analizi çalışma alanına iletin. Veriler her üç dakikada bir toplanır. Bu [veriler, Azure](../../azure-monitor/log-query/log-query-overview.md) Monitor'da sorgu için kullanılabilir. Bu verileri geçiş planlaması, kapasite çözümlemesi, bulma ve isteğe bağlı performans sorun giderme gibi senaryolara uygulayabilirsiniz.

## <a name="container-records"></a>Konteyner kayıtları

Kapsayıcılar için Azure Monitor tarafından toplanan kayıt örnekleri ve günlük arama sonuçlarında görünen veri türleri aşağıdaki tabloda görüntülenir:

| Veri türü | Günlük Arama'da veri türü | Alanlar |
| --- | --- | --- |
| Ana bilgisayarlar ve kapsayıcılar için performans | `Perf` | Bilgisayar, ObjectName, CounterName %&#40; İşlemci Süresi, Disk OKUR MB, Disk Yazar MB, Bellek Kullanımı MB, Ağ Bayt ları Al, Ağ Gönder Bayt, İşlemci Kullanımı sn, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Konteyner envanteri | `ContainerInventory` | TimeGenerated, Bilgisayar, konteyner adı, ContainerHostname, Resim, ImageTag, ContainerState, ExitCode, EnvironmentVar, Komut, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Konteyner günlüğü | `ContainerLog` | TimeGenerated, Bilgisayar, resim kimliği, konteyner adı, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Konteyner düğümü envanteri | `ContainerNodeInventory`| Zamanlı, Bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, Kaynak Sistemi|
| Bir Kubernetes kümesindeki bakla ların envanteri | `KubePodInventory` | TimeGenerated, Bilgisayar, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus,  ContainerStatusReason, ContainerID, ContainerName, Name, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Bir Kubernetes kümesinin düğümlerinin envanteri | `KubeNodeInventory` | TimeGenerated, Bilgisayar, ClusterName, ClusterId, LastTransitionTimeReady, Etiketler, Durum, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes Etkinlikleri | `KubeEvents` | Zaman, Bilgisayar, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Mesaj, Kaynak Sistemi | 
| Kubernetes kümesindeki hizmetler | `KubeServices` | Zaman, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, Kaynak Sistemi | 
| Kubernetes kümesinin düğümleri için performans ölçümleri | Perf &#124; where ObjectName == "K8SNode" | Bilgisayar, ObjectName, CounterName &#40;cpuAllocatableBytes, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes, memoryRssBytes, cpuUsageNanoCores, memoryWorkingsetBytes, restartTimeEpoch&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Kubernetes kümesinin bir parçası olan kapsayıcılar için performans ölçümleri | Perf &#124; where ObjectName == "K8SContainer" | CounterName &#40; cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryWorkingSetBytes, restartTimeEpoch, cpuUsageNanoCores, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Özel Ölçümler |`InsightsMetrics` | Bilgisayar, Ad, Ad Alanı, Origin, SourceSystem, Etiketler<sup>1</sup>, TimeGenerated, Tür, Va, _ResourceId | 

<sup>1</sup> *Etiketler* özelliği, ilgili metrik için [birden çok boyutu](../platform/data-platform-metrics.md#multi-dimensional-metrics) temsil eder. `InsightsMetrics` Tabloda toplanan ve depolanan ölçümler ve kayıt özelliklerinin açıklaması hakkında ek bilgi için [bkz.](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md)

## <a name="search-logs-to-analyze-data"></a>Verileri analiz etmek için günlükleri arama

Azure Monitör Günlükleri eğilimleri aramanıza, darboğazları tanılamanıza, tahmin demenize veya geçerli küme yapılandırmasının en iyi şekilde çalışıp çalışmadığını belirlemenize yardımcı olabilecek verileri ilişkilendirmenize yardımcı olabilir. Önceden tanımlanmış günlük aramaları, bilgileri istediğiniz gibi döndürmek için hemen kullanmaya başlamanız veya özelleştirmeniz için sağlanır.

**Analitik** açılan listede Görünüm listesinden önizleme bölmesinde **Görünüm Kubernetes olay günlüklerini** veya **görünüm kapsayıcı günlüklerini** seçerek çalışma alanındaki verilerin etkileşimli analizini gerçekleştirebilirsiniz. **Günlük Arama** sayfası, içinde olduğunuz Azure portalı sayfasının sağında görünür.

![Log Analytics’te verileri analiz etme](./media/container-insights-analyze/container-health-log-search-example.png)

Çalışma alanınıza iletilen kapsayıcı günlükleri çıktısı STDOUT ve STDERR'dir. Azure Monitor Azure tarafından yönetilen Kubernetes'i (AKS) izlediği için, oluşturulan verilerin büyük hacmi nedeniyle Kube sistemi bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları

Genellikle bir veya iki örnekle başlayan sorgular oluşturmak ve gereksinimlerinize uyacak şekilde değiştirmek genellikle yararlıdır. Daha gelişmiş sorgular oluşturmaya yardımcı olmak için aşağıdaki örnek sorguları deneyebilirsiniz:

| Sorgu | Açıklama | 
|-------|-------------|
| Konteyner Envanteri<br> &#124; proje Bilgisayar, Ad, Resim, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; oluşturma tablosu | Bir konteynerin tüm yaşam döngüsü bilgilerini listele| 
| KubeEvents_CL<br> &#124; nerede değil(boş(Namespace_s))<br> TimeGenerated desc göre &#124; sıralama<br> &#124; oluşturma tablosu | Kubernetes etkinlikleri|
| KonteynerImageInventory<br> &#124; AggregatedValue = count() tarafından Image, ImageTag, Running özetlemek | Görüntü envanteri | 
| **Çizgi grafik görüntüleme seçeneğini seçin:**<br> Perf<br> &#124; nerede ObjectName == "K8SContainer" ve CounterName == "cpuUsageNanoCores" &#124; AvgCPUUsageNanoCores = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName özetlemek | Konteyner CPU | 
| **Çizgi grafik görüntüleme seçeneğini seçin:**<br> Perf<br> objectname == "K8SContainer" ve CounterName == "memoryRssBytes" &#124; AvgUsedRssMemoryBytes = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName özetlemek &#124; | Kapsayıcı bellek |
| ÖngörülerÖlçümler<br> &#124; nerede Adı == "requests_count"<br> &#124; TimeGenerated=bin(TimeGenerated, 1m) tarafından Val=any(Val) özetlemek<br> TimeGenerated asc göre &#124; sıralama<br> &#124; proje RequestsPerMinute = Val - prev(Val), TimeGenerated <br> &#124; render barchart  | Özel Ölçümlerle Dakika Başına İstekler |

## <a name="query-prometheus-metrics-data"></a>Prometheus ölçümlerini sorgula veri

Aşağıdaki örnek, disk okumalarını her düğüm başına saniyede gösteren bir Prometheus ölçümleri sorgusudur.

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

Namespace tarafından filtreuygulanmış Azure Monitor tarafından kazınmış Prometheus ölçümlerini görüntülemek için "prometheus" belirtin. Burada `default` kubernetes ad alanından Prometheus ölçümleri görüntülemek için örnek bir sorgu.

```
InsightsMetrics 
| where Namespace == "prometheus"
| extend tags=parse_json(Tags)
| summarize count() by Name
```

Prometheus verileri de doğrudan adıyla sorgulanabilir.

```
InsightsMetrics 
| where Namespace == "prometheus"
| where Name contains "some_prometheus_metric"
```

### <a name="query-config-or-scraping-errors"></a>Config veya kazıma hatalarını sorgula

Herhangi bir yapılandırma veya kazıma hatalarını araştırmak için, `KubeMonAgentEvents` aşağıdaki örnek sorgu tablodan bilgi olaylarını döndürür.

```
KubeMonAgentEvents | where Level != "Info" 
```

Çıktı aşağıdakilere benzer sonuçlar gösterecektir:

![Aracıdan bilgilendirme olaylarının günlük sorgu sonuçları](./media/container-insights-log-search/log-query-example-kubeagent-events.png)

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcılar için Azure Monitörü önceden tanımlanmış bir uyarı kümesi içermez. DevOps veya operasyonel süreç ve yordamlarınızı desteklemek için yüksek CPU ve bellek kullanımı için önerilen uyarıları nasıl oluşturup nasıl oluşturup nasıl oluşturup oluşturabilirsiniz öğrenmek [için kapsayıcılar için Azure Monitor ile performans uyarıları oluştur'u](container-insights-alerts.md) gözden geçirin. 
