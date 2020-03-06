---
title: Kapsayıcılar için Azure Izleyici günlüklerini sorgulama | Microsoft Docs
description: Kapsayıcılar için Azure Izleyici ölçümleri ve günlük verilerini toplar ve bu makalede kayıtları açıklanmakta ve örnek sorgular yer almaktadır.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: dcd1656673e549b583de26bca897d0055f389d0a
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78362252"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici günlüklerini sorgulama

Kapsayıcılar için Azure Izleyici, kapsayıcı konakları ve kapsayıcılarından performans ölçümleri, envanter verileri ve sistem durumu bilgilerini toplar ve Azure Izleyici 'de Log Analytics çalışma alanına iletir. Üç dakikada bir toplanan veriler. Bu veriler Azure Izleyici 'de [sorgu](../../azure-monitor/log-query/log-query-overview.md) için kullanılabilir. Bu verileri, geçiş planlama, Kapasite Analizi, bulma ve isteğe bağlı performans sorunlarını gidermeyle ilgili senaryolara uygulayabilirsiniz.

## <a name="container-records"></a>Kapsayıcı kayıt

Kapsayıcılar ve günlük arama sonuçlarında görünmesini veri türleri için Azure İzleyici tarafından toplanan kayıtları örnekleri aşağıdaki tabloda görüntülenir:

| Veri türü | Günlük araması'nda veri türü | Alanlar |
| --- | --- | --- |
| Konaklar ve kapsayıcılar için performans | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% işlemci zamanı, Disk okuma MB, MB, bellek kullanımı MB Disk Yazar ağ bayt alma, ağ gönderme bayt, işlemci kullanımı sn, ağ&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki |
| Kapsayıcı envanteri | `ContainerInventory` | TimeGenerated, bilgisayar, kapsayıcı adı, ContainerHostname, görüntü, ImageTag, ContainerState, ExitCode, EnvironmentVar, komut, oluşturulma zamanı, StartedTime, FinishedTime, Analytics'teki, Containerıd, ImageID |
| Kapsayıcı günlüğü | `ContainerLog` | TimeGenerated, bilgisayar, görüntü kimliği, LogEntrySource LogEntry, Analytics'teki, Containerıd kapsayıcı adı |
| Kapsayıcı düğümü envanteri | `ContainerNodeInventory`| TimeGenerated, bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, Analytics'teki|
| Bir Kubernetes kümesinin pod envanterini | `KubePodInventory` | TimeGenerated, Computer, Clusterıd, ContainerCreationTimeStamp, Poduıd, Pod Creationtimestamp, ContainerRestartCount, PodRestartCount, Pod StartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus,  ContainerStatusReason, Containerıd, ContainerName, Name, Pod etiketi, Namespace, Pod durum, ClusterName, Podıp, dir |
| Bir Kubernetes kümesinin düğümleri bölümünün envanteri | `KubeNodeInventory` | TimeGenerated, bilgisayar, ClusterName, Lclusterıd, LastTransitionTimeReady, etiketler, durum, KubeletVersion, KubeProxyVersion, CreationTimeStamp, Analytics'teki | 
| Kubernetes olayları | `KubeEvents` | TimeGenerated, bilgisayar, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message, Analytics'teki | 
| Kubernetes kümesindeki Hizmetleri | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, Analytics'teki | 
| Kubernetes küme düğümleri parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SNode" == | Bilgisayar, ObjectName, CounterName &#40;Cpuallocatablebytes, memoryAllocatableBytes, CpuCapacityNanoCores, Memorycapacitybyte, memoryRssBytes, Cpuuslationanoçekirdekler, memoryWorkingsetBytes,&#41;restartTimeEpoch, CounterValue, TimeGenerated, CounterPath, dir | 
| Kubernetes kümesine kapsayıcıları parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SContainer" == | CounterName &#40; CpuRequestNanoCores, memoryRequestBytes, CpuLimitNanoCores, memoryWorkingSetBytes, RestartTimeEpoch, Cpuuslationanoçekirdekler, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath, dir | 
| Özel ölçümler |`InsightsMetrics` | Bilgisayar, ad, ad alanı, Origin, dir, Etiketler<sup>1</sup>, TimeGenerated, Type, Va, _ResourceId | 

<sup>1</sup> *Etiketler* özelliği, karşılık gelen ölçüm için [birden çok boyutu](../platform/data-platform-metrics.md#multi-dimensional-metrics) temsil eder. `InsightsMetrics` tablosunda toplanan ve depolanan ölçümler ve kayıt özelliklerinin açıklaması hakkında daha fazla bilgi için bkz. [ınsightsölçümlerini genel bakış](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md).

>[!NOTE]
>Prometheus desteği şu anda genel önizlemede bir özelliktir.
>

## <a name="search-logs-to-analyze-data"></a>Verileri çözümlemek için günlüklerinde arama yapma

Azure Izleyici günlükleri, geçerli küme yapılandırmasının en iyi şekilde performans yapıp gerçekleştirmediğini belirlemenize yardımcı olabilecek eğilimleri bulmanıza, performans sorunlarını tanılamanıza, tahmin etmenize veya aralarındaki verileri ilişkilendirmenize yardımcı olabilir. Önceden tanımlanmış günlük aramaları, hemen kullanmaya başlayın ya da istediğiniz gibi bilgileri döndürmek için özelleştirmek için sağlanır.

**Analiz bölmesindeki Görünüm** açılır listesinden **Kubernetes olay günlüklerini görüntüle** veya **kapsayıcı günlüklerini görüntüle** seçeneğini belirleyerek çalışma alanındaki verilerin etkileşimli analizini yapabilirsiniz. **Günlük araması** sayfası, üzerinde olduğunuz Azure Portal sayfanın sağında görüntülenir.

![Log Analytics’te verileri analiz etme](./media/container-insights-analyze/container-health-log-search-example.png)   

Çalışma alanınıza iletilen kapsayıcı günlüğü çıkışları STDOUT ve STDERR ' dir. Azure İzleyici, Azure tarafından yönetilen Kubernetes (AKS) izleme için Kube-system büyük oluşturulan veri hacmi nedeniyle bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları

Genellikle, bir örnek veya iki ile başlayıp ardından bunları gereksinimlerinize uyacak şekilde değiştirin sorguları oluşturmak kullanışlıdır. Daha gelişmiş sorgular oluşturmanıza yardımcı olmak için aşağıdaki örnek sorgularda ile denemeler yapabilirsiniz:

| Sorgu | Açıklama | 
|-------|-------------|
| ContainerInventory<br> &#124;Proje bilgisayar, ad, resim, ImageTag, ContainerState, oluşturulma zamanı, StartedTime, FinishedTime<br> &#124;Tablo oluşturma | Tüm bir kapsayıcının yaşam döngüsü bilgilerini Listele| 
| KubeEvents_CL<br> &#124;Burada not(isempty(Namespace_s))<br> &#124;TimeGenerated desc göre sırala<br> &#124;Tablo oluşturma | Kubernetes olayları|
| ContainerImageInventory<br> &#124;Summarize aggregatedvalue = count() ImageTag, görüntü tarafından çalıştırma | Görüntü envanteri | 
| **Çizgi grafik görüntüleme seçeneğini belirleyin**:<br> Perf<br> &#124;Burada ObjectName "K8SContainer" ve CounterName == "cpuUsageNanoCores" == &#124; AvgCPUUsageNanoCores özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı CPU | 
| **Çizgi grafik görüntüleme seçeneğini belirleyin**:<br> Perf<br> &#124;Burada ObjectName "K8SContainer" ve CounterName == "memoryRssBytes" == &#124; AvgUsedRssMemoryBytes özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı belleği |
| Insightsölçümlerini<br> &#124;Burada Name = = "requests_count"<br> &#124;Değer = Any (Val) ile TimeGenerated = bin (TimeGenerated, 1m)<br> &#124;TimeGenerated ASC 'e göre sırala<br> &#124;Proje RequestsPerMinute = Val-önceki (Val), TimeGenerated <br> &#124;oluşturma bargrafiği  | Özel ölçümler ile dakika başına istek |

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

Azure Izleyici tarafından ad alanına göre filtrelenmiş Prometheus ölçümlerini görüntülemek için "Prometheus" seçeneğini belirtin. İşte `default` Kubernetes ad alanından Prometheus ölçümlerini görüntülemek için örnek bir sorgu.

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

Herhangi bir yapılandırmayı veya bir hatayı araştırmak için aşağıdaki örnek sorgu `KubeMonAgentEvents` tablosundan bilgilendirici olaylar döndürür.

```
KubeMonAgentEvents | where Level != "Info" 
```

Çıktı aşağıdakine benzer sonuçları gösterir:

![Bilgi olaylarının aracıdan günlük sorgusu sonuçları](./media/container-insights-log-search/log-query-example-kubeagent-events.png)

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcılar için Azure Izleyici, önceden tanımlanmış bir uyarı kümesi içermez. DevOps veya işletimsel işlemlerinizi ve yordamlarınızı desteklemek üzere yüksek CPU ve bellek kullanımı için önerilen uyarılar oluşturmayı öğrenmek üzere [kapsayıcılar Için Azure izleyici ile performans uyarılarını oluşturma](container-insights-alerts.md) ' yı gözden geçirin. 
