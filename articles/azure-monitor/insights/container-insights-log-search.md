---
title: Kapsayıcılar için Azure Izleyici günlüklerini sorgulama | Microsoft Docs
description: Kapsayıcılar için Azure Izleyici ölçümleri ve günlük verilerini toplar ve bu makalede kayıtları açıklanmakta ve örnek sorgular yer almaktadır.
ms.topic: conceptual
ms.date: 06/01/2020
ms.openlocfilehash: 392aac8f81ac3894fca8b6f70570834a5af16ade
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84298312"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici günlüklerini sorgulama

Kapsayıcılar için Azure Izleyici, kapsayıcı konakları ve kapsayıcılarından performans ölçümleri, envanter verileri ve sistem durumu bilgilerini toplar. Veriler her üç dakikada bir toplanır ve Azure Izleyici 'de Log Analytics çalışma alanına iletilir. Bu veriler Azure Izleyici 'de [sorgu](../../azure-monitor/log-query/log-query-overview.md) için kullanılabilir. Bu verileri, geçiş planlama, Kapasite Analizi, bulma ve isteğe bağlı performans sorunlarını gidermeyle ilgili senaryolara uygulayabilirsiniz.

## <a name="container-records"></a>Kapsayıcı kayıtları

Aşağıdaki tabloda, kapsayıcılar için Azure Izleyici tarafından toplanan kayıtların ayrıntıları verilmiştir. 

| Veriler | Veri kaynağı | Veri türü | Alanlar |
|------|-------------|-----------|--------|
| Konaklar ve kapsayıcılar için performans | Kullanım ölçümleri, Cadvizörü 'den ve Kuto API 'si sınırlarından alınır | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% Işlemci zamanı, disk okuma MB, disk yazma MB, bellek kullanımı MB, ağ alma baytları, ağ gönderme baytları, Işlemci kullanım sn, ağ&#41;, CounterValue, TimeGenerated, CounterPath, dir |
| Kapsayıcı envanteri | Docker | `ContainerInventory` | TimeGenerated, bilgisayar, kapsayıcı adı, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, komut, CreatedTime, StartedTime, Sonlandırhedtime, dir, Containerıd, ImageID |
| Kapsayıcı günlüğü | Docker | `ContainerLog` | TimeGenerated, bilgisayar, görüntü KIMLIĞI, kapsayıcı adı, LogEntrySource, LogEntry, dir, Containerıd |
| Kapsayıcı düğümü envanteri | Kuin API 'SI | `ContainerNodeInventory`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, dir|
| Bir Kubernetes kümesinde Pod sayımı | Kuin API 'SI | `KubePodInventory` | TimeGenerated, Computer, Clusterıd, ContainerCreationTimeStamp, Poduıd, Pod Creationtimestamp, ContainerRestartCount, PodRestartCount, Pod StartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus, ContainerStatusReason, Containerıd, ContainerName, Name, Pod etiketi, Namespace, Pod durum, ClusterName, Podıp, dir |
| Bir Kubernetes kümesinin bir parçası olan düğümlerin envanteri | Kuin API 'SI | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, Clusterıd, Lastgeçişli Tiontimeready, Etiketler, durum, KubeletVersion, KubeProxyVersion, CreationTimeStamp, dir | 
| Kubernetes Olayları | Kuin API 'SI | `KubeEvents` | TimeGenerated, bilgisayar, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Ileti, dir | 
| Kubernetes kümesindeki hizmetler | Kuin API 'SI | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, dir | 
| Kubernetes kümesinin düğümlerin bir parçası için performans ölçümleri || Perf &#124; WHERE ObjectName = = "K8SNode" | Bilgisayar, ObjectName, CounterName &#40;cpuAllocatableBytes, memoryAllocatableBytes, cpuCapacityNanoCores, Memorycapacitybyte, memoryRssBytes, Cpuusme Anoçekirdekler, memoryWorkingsetBytes, restartTimeEpoch&#41;, CounterValue, TimeGenerated, CounterPath, dir | 
| Kubernetes kümesinin kapsayıcılar bölümü için performans ölçümleri || Perf &#124; WHERE ObjectName = = "K8SContainer" | CounterName &#40; cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryWorkingSetBytes, restartTimeEpoch, Cpuuslationanoçekirdekler, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath, dir | 
| Özel ölçümler ||`InsightsMetrics` | Bilgisayar, ad, ad alanı, Origin, dir, Etiketler<sup>1</sup>, TimeGenerated, Type, Va, _ResourceId | 

<sup>1</sup> *Etiketler* özelliği, karşılık gelen ölçüm için [birden çok boyutu](../platform/data-platform-metrics.md#multi-dimensional-metrics) temsil eder. Tabloda toplanan ve depolanan ölçümler ve kayıt özelliklerinin açıklaması hakkında daha fazla bilgi için `InsightsMetrics` bkz. [ınsightsölçümlerini genel bakış](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md).

## <a name="search-logs-to-analyze-data"></a>Verileri çözümlemek için günlüklere arama yapın

Azure Izleyici günlükleri, geçerli küme yapılandırmasının en iyi şekilde performans yapıp gerçekleştirmediğini belirlemenize yardımcı olabilecek eğilimleri bulmanıza, performans sorunlarını tanılamanıza, tahmin etmenize veya aralarındaki verileri ilişkilendirmenize yardımcı olabilir. Önceden tanımlanmış günlük aramaları, bilgileri istediğiniz şekilde döndürmek için veya ' i hemen kullanmaya başlayabilmeniz için sağlanır.

**Analiz bölmesindeki Görünüm** açılır listesinden **Kubernetes olay günlüklerini görüntüle** veya **kapsayıcı günlüklerini görüntüle** seçeneğini belirleyerek çalışma alanındaki verilerin etkileşimli analizini yapabilirsiniz. **Günlük araması** sayfası, üzerinde olduğunuz Azure Portal sayfanın sağında görüntülenir.

![Log Analytics’te verileri analiz etme](./media/container-insights-analyze/container-health-log-search-example.png)

Çalışma alanınıza iletilen kapsayıcı günlüğü çıkışları STDOUT ve STDERR ' dir. Azure Izleyici, Azure tarafından yönetilen Kubernetes (AKS) ' i izlerken, oluşturulan verilerin büyük hacmi nedeniyle Kuof-System bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları

Genellikle bir örnekle başlayan sorgular oluşturmak ve sonra gereksinimlerinize uyacak şekilde değiştirmek yararlıdır. Daha gelişmiş sorgular oluşturmaya yardımcı olmak için aşağıdaki örnek sorgularla denemeler yapabilirsiniz:

| Sorgu | Açıklama | 
|-------|-------------|
| Containerınventory<br> &#124; proje bilgisayar, ad, resim, ImageTag, ContainerState, CreatedTime, StartedTime, Sonlandırhedtime<br> &#124; oluşturma tablosu | Kapsayıcının yaşam döngüsü bilgilerinin tümünü listeleme| 
| KubeEvents_CL<br> Not &#124; (IsEmpty (Namespace_s))<br> TimeGenerated DESC 'e göre sıralama &#124;<br> &#124; oluşturma tablosu | Kubernetes olayları|
| Containerımageınventory<br> &#124; özetleme değeri = Count () by Image, ImageTag, çalışıyor | Görüntü envanteri | 
| **Çizgi grafik görüntüleme seçeneğini belirleyin**:<br> Perf<br> &#124;, ObjectName = = "K8SContainer" ve CounterName = = "Cpuusstananoçekirdekleri" &#124;, bin (TimeGenerated, 30D), InstanceName | Kapsayıcı CPU 'SU | 
| **Çizgi grafik görüntüleme seçeneğini belirleyin**:<br> Perf<br> &#124;, ObjectName = = "K8SContainer" ve CounterName = = "memoryRssBytes" &#124; bin (TimeGenerated, 30D), InstanceName | Kapsayıcı belleği |
| Insightsölçümlerini<br> &#124; Name = = "requests_count"<br> &#124; özetleme aralığı = herhangi bir (Val) TimeGenerated = bin (TimeGenerated, 1m)<br> TimeGenerated ASC 'e göre sıralama &#124;<br> &#124; proje RequestsPerMinute = Val-önceki (Val), TimeGenerated <br> &#124; işleme bargrafiği  | Özel ölçümler ile dakika başına istek |

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

Azure Izleyici tarafından ad alanına göre filtrelenmiş Prometheus ölçümlerini görüntülemek için "Prometheus" seçeneğini belirtin. Aşağıda, Kubernetes ad alanından Prometheus ölçümlerini görüntülemek için örnek bir sorgu verilmiştir `default` .

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

Kapsayıcılar için Azure Izleyici, önceden tanımlanmış bir uyarı kümesi içermez. DevOps veya işletimsel işlemlerinizi ve yordamlarınızı desteklemek üzere yüksek CPU ve bellek kullanımı için önerilen uyarılar oluşturmayı öğrenmek üzere [kapsayıcılar Için Azure izleyici ile performans uyarılarını oluşturma](container-insights-alerts.md) ' yı gözden geçirin. 
