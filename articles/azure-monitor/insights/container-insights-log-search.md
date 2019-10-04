---
title: Kapsayıcılar için Azure Izleyici günlüklerini sorgulama | Microsoft Docs
description: Kapsayıcılar için Azure Izleyici ölçümleri ve günlük verilerini toplar ve bu makalede kayıtları açıklanmakta ve örnek sorgular yer almaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: ae8dd4cccb6795faa02e6705404644f6ccc24864
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71948059"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici günlüklerini sorgulama

Kapsayıcılar için Azure Izleyici, kapsayıcı konakları ve kapsayıcılarından performans ölçümleri, envanter verileri ve sistem durumu bilgilerini toplar ve Azure Izleyici 'de Log Analytics çalışma alanına iletir. Veriler her üç dakikada bir toplanır. Bu veriler Azure Izleyici 'de [sorgu](../../azure-monitor/log-query/log-query-overview.md) için kullanılabilir. Bu verileri, geçiş planlama, Kapasite Analizi, bulma ve isteğe bağlı performans sorunlarını gidermeyle ilgili senaryolara uygulayabilirsiniz.

## <a name="container-records"></a>Kapsayıcı kayıtları

Kapsayıcılar için Azure Izleyici tarafından toplanan kayıt örnekleri ve günlük araması sonuçlarında görünen veri türleri aşağıdaki tabloda görüntülenir:

| Veri türü | Günlük aramasında veri türü | Alanlar |
| --- | --- | --- |
| Konaklar ve kapsayıcılar için performans | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% işlemci zamanı, DISK okuma MB, DISK yazma MB, bellek kullanımı MB, ağ alma baytları, ağ gönderme baytları, işlemci kullanım sn, ağ&#41;, CounterValue, TimeGenerated, CounterPath, dir |
| Kapsayıcı envanteri | `ContainerInventory` | TimeGenerated, bilgisayar, kapsayıcı adı, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, komut, CreatedTime, StartedTime, Sonlandırhedtime, dir, Containerıd, ImageID |
| Kapsayıcı günlüğü | `ContainerLog` | TimeGenerated, bilgisayar, görüntü KIMLIĞI, kapsayıcı adı, LogEntrySource, LogEntry, dir, Containerıd |
| Kapsayıcı düğümü envanteri | `ContainerNodeInventory`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, dir|
| Bir Kubernetes kümesinde Pod sayımı | `KubePodInventory` | TimeGenerated, Computer, Clusterıd, ContainerCreationTimeStamp, Poduıd, Pod Creationtimestamp, ContainerRestartCount, PodRestartCount, Pod StartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus,  ContainerStatusReason, Containerıd, ContainerName, Name, Pod etiketi, Namespace, Pod durum, ClusterName, Podıp, dir |
| Bir Kubernetes kümesinin bir parçası olan düğümlerin envanteri | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, Clusterıd, Lastgeçişli Tiontimeready, Etiketler, durum, KubeletVersion, KubeProxyVersion, CreationTimeStamp, dir | 
| Kubernetes olayları | `KubeEvents` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Ileti, dir | 
| Kubernetes kümesindeki hizmetler | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, dir | 
| Kubernetes kümesinin düğümlerin bir parçası için performans ölçümleri | Perf &#124; WHERE ObjectName = = "K8SNode" | Bilgisayar, ObjectName, CounterName &#40;Cpuallocatablebytes, memoryAllocatableBytes, CpuCapacityNanoCores, Memorycapacitybyte, memoryRssBytes, Cpuusme Anoçekirdekler, memoryWorkingsetBytes,&#41;restartTimeEpoch, CounterValue, TimeGenerated, CounterPath, dir | 
| Kubernetes kümesinin kapsayıcılar bölümü için performans ölçümleri | Perf &#124; WHERE ObjectName = = "K8SContainer" | CounterName &#40; CpuRequestNanoCores, memoryRequestBytes, CpuLimitNanoCores, memoryWorkingSetBytes, RestartTimeEpoch, Cpuuslationanoçekirdekler, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath, dir | 
| Özel ölçümler |`InsightsMetrics` | Bilgisayar, ad, ad alanı, Origin, dir, Etiketler<sup>1</sup>, TimeGenerated, Type, VA, _resourceıd | 

<sup>1</sup> *Etiketler* özelliği, karşılık gelen ölçüm için [birden çok boyutu](../platform/data-platform-metrics.md#multi-dimensional-metrics) temsil eder. @No__t-0 tablosunda toplanan ve depolanan ölçümler ve kayıt özelliklerinin açıklaması hakkında daha fazla bilgi için bkz. [ınsightsölçümlerini genel bakış](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md).

>[!NOTE]
>Prometheus desteği şu anda genel önizlemede bir özelliktir.
>

## <a name="search-logs-to-analyze-data"></a>Verileri çözümlemek için günlüklere arama yapın

Azure Izleyici günlükleri, geçerli küme yapılandırmasının en iyi şekilde performans yapıp gerçekleştirmediğini belirlemenize yardımcı olabilecek eğilimleri bulmanıza, performans sorunlarını tanılamanıza, tahmin etmenize veya aralarındaki verileri ilişkilendirmenize yardımcı olabilir. Önceden tanımlanmış günlük aramaları, bilgileri istediğiniz şekilde döndürmek için veya ' i hemen kullanmaya başlayabilmeniz için sağlanır.

Önizleme bölmesindeki **Kubernetes olay günlüklerini görüntüle** veya **kapsayıcı günlüklerini görüntüle** seçeneğini belirleyerek çalışma alanındaki etkileşimli veri analizini gerçekleştirebilirsiniz. **Günlük araması** sayfası, üzerinde olduğunuz Azure Portal sayfanın sağında görüntülenir.

![Log Analytics verileri analiz etme](./media/container-insights-analyze/container-health-log-search-example.png)   

Çalışma alanınıza iletilen kapsayıcı günlüğü çıkışları STDOUT ve STDERR ' dir. Azure Izleyici, Azure tarafından yönetilen Kubernetes (AKS) ' i izlerken, oluşturulan verilerin büyük hacmi nedeniyle Kuof-System bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları

Genellikle bir örnekle başlayan sorgular oluşturmak ve sonra gereksinimlerinize uyacak şekilde değiştirmek yararlıdır. Daha gelişmiş sorgular oluşturmaya yardımcı olmak için aşağıdaki örnek sorgularla denemeler yapabilirsiniz:

| Sorgu | Açıklama | 
|-------|-------------|
| Containerınventory<br> &#124;Proje bilgisayar, ad, resim, ImageTag, ContainerState, CreatedTime, StartedTime, Sonlandırhedtime<br> &#124;oluşturma tablosu | Kapsayıcının yaşam döngüsü bilgilerinin tümünü listeleme| 
| KubeEvents_CL<br> &#124;Where Not (IsEmpty (Namespace_s))<br> &#124;TimeGenerated DESC 'e göre sırala<br> &#124;oluşturma tablosu | Kubernetes olayları|
| Containerımageınventory<br> &#124;Aggregcount değerini özetleme = Count () by Image, ImageTag, çalışıyor | Görüntü envanteri | 
| **Çizgi grafik görüntüleme seçeneğini belirleyin**:<br> Perf<br> &#124;Burada ObjectName = = "K8SContainer" ve CounterName = = "Cpuusstananoçekirdekler &#124; ", bin (TimeGenerated, 30gb) tarafından avgcpuusstananoçekirdekleri = Ort (CounterValue) öğesini özetler. | Kapsayıcı CPU 'SU | 
| **Çizgi grafik görüntüleme seçeneğini belirleyin**:<br> Perf<br> &#124;Burada ObjectName = = "K8SContainer" ve CounterName = = "memoryRssBytes" &#124; , bin (TimeGenerated, 30D), InstanceName | Kapsayıcı belleği |
| Insightsölçümlerini<br> &#124;Burada Name = = "requests_count"<br> &#124;Değer = Any (Val) ile TimeGenerated = bin (TimeGenerated, 1m)<br> &#124;TimeGenerated ASC 'e göre sırala<br> &#124;Proje RequestsPerMinute = Val-önceki (Val), TimeGenerated <br> &#124;oluşturma bargrafiği  | Özel ölçümler ile dakika başına istek |

Aşağıdaki örnek bir Prometheus ölçümleri sorgusudur. Toplanan ölçümler sayılır ve belirli bir dönemde kaç hatanın oluştuğunu öğrenmek için, sayımız sayısından çıkarıdık. Veri kümesi *Partitionkey*tarafından bölümlenir, her bir benzersiz ad, *ana bilgisayar* *adı*ve *OperationType*kümesi için, bu küme üzerinde günlükleri *TimeGenerated*tarafından sipariş eden bir işlem, bunu mümkün kılan bir işlem olan bir alt sorgu çalıştırdık. bir oranı öğrenmek için, önceki *TimeGenerated* ve bu süre için kaydedilen sayıyı bulun.

```
let data = InsightsMetrics 
| where Namespace contains 'prometheus' 
| where Name == 'kubelet_docker_operations' or Name == 'kubelet_docker_operations_errors'    
| extend Tags = todynamic(Tags) 
| extend OperationType = tostring(Tags['operation_type']), HostName = tostring(Tags.hostName) 
| extend partitionKey = strcat(HostName, '/' , Name, '/', OperationType) 
| partition by partitionKey ( 
    order by TimeGenerated asc 
    | extend PrevVal = prev(Val, 1), PrevTimeGenerated = prev(TimeGenerated, 1) 
    | extend Rate = iif(TimeGenerated == PrevTimeGenerated, 0.0, Val - PrevVal) 
    | where isnull(Rate) == false 
) 
| project TimeGenerated, Name, HostName, OperationType, Rate; 
let operationData = data 
| where Name == 'kubelet_docker_operations' 
| project-rename OperationCount = Rate; 
let errorData = data 
| where Name == 'kubelet_docker_operations_errors' 
| project-rename ErrorCount = Rate; 
operationData 
| join kind = inner ( errorData ) on TimeGenerated, HostName, OperationType 
| project-away TimeGenerated1, Name1, HostName1, OperationType1 
| extend SuccessPercentage = iif(OperationCount == 0, 1.0, 1 - (ErrorCount / OperationCount))
```

Çıktı aşağıdakine benzer sonuçları gösterir:

![Veri alma biriminin günlük sorgu sonuçları](./media/container-insights-log-search/log-query-example-prometheus-metrics.png)

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcılar için Azure Izleyici, önceden tanımlanmış bir uyarı kümesi içermez. DevOps veya işletimsel işlemlerinizi ve yordamlarınızı desteklemek üzere yüksek CPU ve bellek kullanımı için önerilen uyarılar oluşturmayı öğrenmek üzere [kapsayıcılar Için Azure izleyici ile performans uyarılarını oluşturma](container-insights-alerts.md) ' yı gözden geçirin. 
