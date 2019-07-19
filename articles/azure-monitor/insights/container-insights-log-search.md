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
ms.openlocfilehash: d6e65331db53be5ba13a75e6b03b271f1071716d
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67989825"
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
| Kubernetes küme düğümleri parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SNode" == | Bilgisayar, ObjectName, CounterName &#40;Cpuallocatablebytes, memoryAllocatableBytes, CpuCapacityNanoCores, Memorycapacitybyte, memoryRssBytes, Cpuusme Anoçekirdekler, memoryWorkingsetBytes,&#41;restartTimeEpoch, CounterValue, TimeGenerated, CounterPath, dir | 
| Kubernetes kümesine kapsayıcıları parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SContainer" == | CounterName &#40; CpuRequestNanoCores, memoryRequestBytes, CpuLimitNanoCores, memoryWorkingSetBytes, RestartTimeEpoch, Cpuuslationanoçekirdekler, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath, dir | 
| Özel Ölçümler |`InsightsMetrics` | Bilgisayar, ad, ad alanı, Origin, dir, Etiketler<sup>1</sup>, TimeGenerated, Type, VA, _resourceıd | 

<sup>1</sup> *Etiketler* özelliği, karşılık gelen ölçüm için [birden çok boyutu](../platform/data-platform-metrics.md#multi-dimensional-metrics) temsil eder. `InsightsMetrics` Tabloda toplanan ve depolanan ölçümler ve kayıt özelliklerinin açıklaması hakkında daha fazla bilgi için bkz. [ınsightsölçümlerini genel bakış](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md).

>[!NOTE]
>Prometheus desteği şu anda genel önizlemede bir özelliktir.
>

## <a name="search-logs-to-analyze-data"></a>Verileri çözümlemek için günlüklerinde arama yapma

Azure Izleyici günlükleri, geçerli küme yapılandırmasının en iyi şekilde performans yapıp gerçekleştirmediğini belirlemenize yardımcı olabilecek eğilimleri bulmanıza, performans sorunlarını tanılamanıza, tahmin etmenize veya aralarındaki verileri ilişkilendirmenize yardımcı olabilir. Önceden tanımlanmış günlük aramaları, hemen kullanmaya başlayın ya da istediğiniz gibi bilgileri döndürmek için özelleştirmek için sağlanır.

Etkileşimli veri analizi seçerek çalışma alanında gerçekleştirebilirsiniz **görünümü Kubernetes olay günlüklerini** veya **kapsayıcı günlüklerini görüntüleme** önizleme bölmesinde seçeneği. **Günlük araması** bulunduğunuz Azure portal sayfasının sağındaki sayfası görüntülenir.

![Log Analytics’te verileri analiz etme](./media/container-insights-analyze/container-health-log-search-example.png)   

Çalışma alanınıza iletilen kapsayıcı günlüğü çıkışları STDOUT ve STDERR ' dir. Azure İzleyici, Azure tarafından yönetilen Kubernetes (AKS) izleme için Kube-system büyük oluşturulan veri hacmi nedeniyle bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları

Genellikle, bir örnek veya iki ile başlayıp ardından bunları gereksinimlerinize uyacak şekilde değiştirin sorguları oluşturmak kullanışlıdır. Daha gelişmiş sorgular oluşturmanıza yardımcı olmak için aşağıdaki örnek sorgularda ile denemeler yapabilirsiniz:

| Sorgu | Açıklama | 
|-------|-------------|
| ContainerInventory<br> &#124;Proje bilgisayar, ad, resim, ImageTag, ContainerState, oluşturulma zamanı, StartedTime, FinishedTime<br> &#124;Tablo oluşturma | Tüm bir kapsayıcının yaşam döngüsü bilgilerini Listele| 
| KubeEvents_CL<br> &#124;Burada not(isempty(Namespace_s))<br> &#124;TimeGenerated desc göre sırala<br> &#124;Tablo oluşturma | Kubernetes olayları|
| ContainerImageInventory<br> &#124;Summarize aggregatedvalue = count() ImageTag, görüntü tarafından çalıştırma | Görüntü envanteri | 
| **Çizgi grafik görüntüleme seçeneğini**:<br> Perf<br> &#124;Burada ObjectName "K8SContainer" ve CounterName == "cpuUsageNanoCores" == &#124; AvgCPUUsageNanoCores özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı CPU | 
| **Çizgi grafik görüntüleme seçeneğini**:<br> Perf<br> &#124;Burada ObjectName "K8SContainer" ve CounterName == "memoryRssBytes" == &#124; AvgUsedRssMemoryBytes özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı belleği |

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