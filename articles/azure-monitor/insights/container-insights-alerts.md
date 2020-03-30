---
title: Kapsayıcılar için Azure Monitör için performans uyarıları oluşturma | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor bellek ve CPU kullanımı için günlük sorguları dayalı özel uyarılar oluşturmak için nasıl açıklanmaktadır.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 5d73f4399d10683597fb2a2e8a3a2ab4ba0d1165
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75730934"
---
# <a name="how-to-set-up-alerts-for-performance-problems-in-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici'de performans sorunları için uyarı oluşturma

Kapsayıcılar için Azure Monitörü, Azure Kapsayıcı Örnekleri'ne veya Azure Kubernetes Hizmetinde (AKS) barındırılan yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinin performansını izler.

Bu makalede, aşağıdaki durumlar için uyarıları etkinleştirmek için nasıl açıklanmıştır:

- Küme düğümlerinde CPU veya bellek kullanımı bir eşiği aştığında
- Denetleyici içindeki herhangi bir kapsayıcıdaki CPU veya bellek kullanımı, ilgili kaynak üzerinde ayarlanan bir sınırla karşılaştırıldığında bir eşiği aştığında
- *NotReady* durum düğümü sayar
- *Başarısız*, *Bekleyen*, *Bilinmeyen*, *Çalışan*, veya *Başarılı* pod-faz sayıları
- Küme düğümleri üzerindeki boş disk alanı bir eşiği aştığında 

Yüksek CPU veya bellek kullanımı veya küme düğümleri üzerinde düşük boş disk alanı için uyarı için, bir metrik uyarı veya metrik ölçüm uyarısı oluşturmak için sağlanan sorguları kullanın. Metrik uyarıların günlük uyarılarına göre daha düşük gecikme süreleri vardır. Ancak günlük uyarıları gelişmiş sorgulama ve daha fazla gelişmişlik sağlar. Günlük uyarıları *sorguları, şimdi* işleci kullanarak ve bir saat geriye giderek bir datetime ile şimdiki zamanile karşılaştırılır. (Kapsayıcılar için Azure Monitörü tüm tarihleri Eşgüdümlü Evrensel Saat (UTC) biçiminde saklar.)

Azure Monitor uyarılarını bilmiyorsanız, başlamadan önce [Microsoft Azure'daki uyarılara genel bakış](../platform/alerts-overview.md) bölümüne bakın. Günlük sorgularını kullanan uyarılar hakkında daha fazla bilgi edinmek için [Azure Monitor'da Günlük uyarılarına](../platform/alerts-unified-log.md)bakın. Metrik uyarılar hakkında daha fazla şey için [Azure Monitor'daki Metrik uyarılarına](../platform/alerts-metric-overview.md)bakın.

## <a name="resource-utilization-log-search-queries"></a>Kaynak kullanımı günlüğü arama sorguları

Bu bölümdeki sorgular her uyarı senaryosunu destekler. Bunlar, bu makalenin [uyarı oluşturma](#create-an-alert-rule) bölümünün 7.

Aşağıdaki sorgu, üye düğümlerinin her dakika CPU kullanımının ortalaması olarak ortalama CPU kullanımını hesaplar.  

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuCapacityNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```

Aşağıdaki sorgu, üye düğümlerin her dakika bellek kullanımının ortalaması olarak ortalama bellek kullanımını hesaplar.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryCapacityBytes';
let usageCounterName = 'memoryRssBytes';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```
>[!IMPORTANT]
>Aşağıdaki sorgular küme nizi \<ve denetleyicinizi temsil \<etmek için küme adınız> ve denetleyici adınız> değerlerini kullanır. Uyarıları ayarlarken bunları ortamınıza özgü değerlerle değiştirin.

Aşağıdaki sorgu, bir denetleyicideki tüm kapsayıcıların ortalama CPU kullanımını, her dakika bir denetleyicideki her kapsayıcı örneğinin CPU kullanımının ortalaması olarak hesaplar. Ölçüm, bir kapsayıcı için ayarlanan sınırın yüzdesidir.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuLimitNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

Aşağıdaki sorgu, bir denetleyicideki tüm kapsayıcıların ortalama bellek kullanımını, her dakika bir denetleyicideki her kapsayıcı örneğinin bellek kullanımının ortalaması olarak hesaplar. Ölçüm, bir kapsayıcı için ayarlanan sınırın yüzdesidir.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryLimitBytes';
let usageCounterName = 'memoryRssBytes';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

Aşağıdaki *sorgu, Hazır* ve *Hazır Olmayan*durumu olan tüm düğümleri ve sayıları döndürür.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let clusterName = '<your-cluster-name>';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| distinct ClusterName, Computer, TimeGenerated
| summarize ClusterSnapshotCount = count() by bin(TimeGenerated, trendBinSize), ClusterName, Computer
| join hint.strategy=broadcast kind=inner (
    KubeNodeInventory
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | summarize TotalCount = count(), ReadyCount = sumif(1, Status contains ('Ready'))
                by ClusterName, Computer,  bin(TimeGenerated, trendBinSize)
    | extend NotReadyCount = TotalCount - ReadyCount
) on ClusterName, Computer, TimeGenerated
| project   TimeGenerated,
            ClusterName,
            Computer,
            ReadyCount = todouble(ReadyCount) / ClusterSnapshotCount,
            NotReadyCount = todouble(NotReadyCount) / ClusterSnapshotCount
| order by ClusterName asc, Computer asc, TimeGenerated desc
```
Aşağıdaki sorgu tüm aşamaları temel alan bölme aşaması sayar döndürür: *Başarısız*, *Bekleyen*, *Bilinmeyen*, *Çalışan*, veya *Başarılı*.  

```kusto
let endDateTime = now();
    let startDateTime = ago(1h);
    let trendBinSize = 1m;
    let clusterName = '<your-cluster-name>';
    KubePodInventory
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ClusterName == clusterName
    | distinct ClusterName, TimeGenerated
    | summarize ClusterSnapshotCount = count() by bin(TimeGenerated, trendBinSize), ClusterName
    | join hint.strategy=broadcast (
        KubePodInventory
        | where TimeGenerated < endDateTime
        | where TimeGenerated >= startDateTime
        | distinct ClusterName, Computer, PodUid, TimeGenerated, PodStatus
        | summarize TotalCount = count(),
                    PendingCount = sumif(1, PodStatus =~ 'Pending'),
                    RunningCount = sumif(1, PodStatus =~ 'Running'),
                    SucceededCount = sumif(1, PodStatus =~ 'Succeeded'),
                    FailedCount = sumif(1, PodStatus =~ 'Failed')
                 by ClusterName, bin(TimeGenerated, trendBinSize)
    ) on ClusterName, TimeGenerated
    | extend UnknownCount = TotalCount - PendingCount - RunningCount - SucceededCount - FailedCount
    | project TimeGenerated,
              TotalCount = todouble(TotalCount) / ClusterSnapshotCount,
              PendingCount = todouble(PendingCount) / ClusterSnapshotCount,
              RunningCount = todouble(RunningCount) / ClusterSnapshotCount,
              SucceededCount = todouble(SucceededCount) / ClusterSnapshotCount,
              FailedCount = todouble(FailedCount) / ClusterSnapshotCount,
              UnknownCount = todouble(UnknownCount) / ClusterSnapshotCount
| summarize AggregatedValue = avg(PendingCount) by bin(TimeGenerated, trendBinSize)
```

>[!NOTE]
>*Beklemede*, *Başarısız Veya* *Bilinmeyen*gibi belirli bölme aşamalarında uyarmak için sorgunun son satırını değiştirin. Örneğin, *FailedCount* kullanımı nda uyarmak için: <br/>`| summarize AggregatedValue = avg(FailedCount) by bin(TimeGenerated, trendBinSize)`

Aşağıdaki sorgu, kullanılan %90 boş alanı aşan küme düğümleri disklerini döndürür. Küme kimliğini almak için önce aşağıdaki sorguyu çalıştırın `ClusterId` ve değeri özellikten kopyalayın:

```kusto
InsightsMetrics
| extend Tags = todynamic(Tags)            
| project ClusterId = Tags['container.azm.ms/clusterId']   
| distinct tostring(ClusterId)   
``` 

```kusto
let clusterId = '<cluster-id>';
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
InsightsMetrics
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where Origin == 'container.azm.ms/telegraf'            
| where Namespace == 'container.azm.ms/disk'            
| extend Tags = todynamic(Tags)            
| project TimeGenerated, ClusterId = Tags['container.azm.ms/clusterId'], Computer = tostring(Tags.hostName), Device = tostring(Tags.device), Path = tostring(Tags.path), DiskMetricName = Name, DiskMetricValue = Val   
| where ClusterId =~ clusterId       
| where DiskMetricName == 'used_percent'
| summarize AggregatedValue = max(DiskMetricValue) by bin(TimeGenerated, trendBinSize)
| where AggregatedValue >= 90
```

## <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma

Azure Monitörü'nde daha önce sağlanan günlük arama kurallarından birini kullanarak günlük uyarısı oluşturmak için aşağıdaki adımları izleyin. ARM şablonu kullanarak oluşturmak için [Azure Kaynak Şablonu'nunu kullanarak Örnek Günlük uyarısı oluşturma'ya](../platform/alerts-log.md#sample-log-alert-creation-using-azure-resource-template)bakın.

>[!NOTE]
>Kapsayıcı kaynak kullanımı için bir uyarı kuralı oluşturmak için aşağıdaki yordam, [günlük uyarıları için Switch API tercihinde](../platform/alerts-log-api-switch.md)açıklandığı gibi yeni bir günlük uyarıları API'ye geçmenizi gerektirir.
>

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure **portalında, Log Analytics çalışma alanlarını**arayın ve seçin.
3. Günlük Analizi çalışma alanları listenizde, kapsayıcılar için Azure Monitor'u destekleyen çalışma alanını seçin. 
4. Sol taraftaki bölmede, Azure Monitor günlükleri sayfasını açmak için **Günlükler'i** seçin. Azure Log Analytics sorguları yazmak ve yürütmek için bu sayfayı kullanırsınız.
5. **Günlükler** sayfasında, daha önce sağlanan [sorgulardan](#resource-utilization-log-search-queries) birini **Arama sorgusu** alanına yapıştırın ve sonuçları doğrulamak için **Çalıştır'ı** seçin. Bu adımı gerçekleştirmezseniz, **+Yeni uyarı** seçeneğini seçmek için kullanılamaz.
6. Günlük uyarısı oluşturmak için **+Yeni uyarısı'nı** seçin.
7. **Koşul** bölümünde, Özel günlük araması önceden tanımlanmış özel günlük koşulu **>\<tanımlanmamış mantık** olduğunda' yı seçin. Doğrudan Azure Monitor günlükleri sayfasından bir uyarı kuralı oluşturduğumuz için **özel günlük arama** sinyali türü otomatik olarak seçilir.  
8. Daha önce sağlanan [sorgulardan](#resource-utilization-log-search-queries) birini **Arama sorgusu** alanına yapıştırın.
9. Uyarıyı aşağıdaki gibi yapılandırın:

    1. Aşağı açılan **Tetikleyici** listesinden **Metrik ölçüm**'ü seçin. Metrik ölçüm, sorgudaki her nesne için, belirtilen eşiğin üzerinde bir değere sahip bir uyarı oluşturur.
    1. **Koşul**için, **Büyükten Büyük'u**seçin ve CPU ve bellek kullanım uyarıları için başlangıç temel **eşiği** olarak **75** girin. Düşük disk alanı uyarısı için **90**girin. Veya ölçütlerinizi karşılayan farklı bir değer girin.
    1. **Temeldeki Tetikleme Uyarısı** **bölümünde, Ardışık ihlalleri**seçin. Açılan listeden **Büyük'ten'i**seçin ve **2'yi**girin.
    1. Kapsayıcı CPU veya bellek kullanımı için bir uyarı yapılandırmak için, **Toplu altında**, **ContainerName**seçin. Küme düğümü düşük disk uyarısı için yapılandırma yapmak için **ClusterId'i**seçin.
    1. Değerlendirme **bölümüne göre,** **Dönem** değerini **60 dakika**olarak ayarlayın. Kural her 5 dakikada bir çalışır ve geçerli saatten itibaren son bir saat içinde oluşturulan kayıtları döndürecek. Olası veri gecikmesi için zaman dilimini geniş bir pencereye ayarlama. Ayrıca, uyarının hiçbir zaman çalışmayan yanlış negatiften kaçınmak için sorgunun verileri döndürmesini de sağlar.

10. Uyarı kuralını tamamlamak için **Bitti'yi** seçin.
11. **Uyarı kuralı adı** alanına bir ad girin. Uyarı yla ilgili ayrıntıları sağlayan bir **Açıklama** belirtin. Ve sağlanan seçeneklerden uygun önem düzeyi seçin.
12. Uyarı kuralını hemen etkinleştirmek için, oluşturma sırasında Kuralı Etkinleştir'in varsayılan değerini kabul **edin.**
13. Varolan bir **Eylem Grubu** seçin veya yeni bir grup oluşturun. Bu adım, bir uyarı tetiklenir her zaman aynı eylemlerin alınmasını sağlar. BT veya DevOps operasyon ekibinizin olayları nasıl yönettiğine göre yapılandırın.
14. Uyarı kuralını tamamlamak için **uyarı kuralını oluştur'u** seçin. Hemen çalıştırılmaya başlar.

## <a name="next-steps"></a>Sonraki adımlar

- Kümelerinizi uyarmak, görselleştirmek veya çözümlemek için değerlendirmek veya özelleştirmek için önceden tanımlanmış sorguları ve örnekleri görmek için [günlük sorgu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.
- Azure Monitörü ve Kubernetes kümenizin diğer yönlerini nasıl izleyeceğiniz hakkında daha fazla bilgi edinmek için [Kubernetes küme performansını görüntüle](container-insights-analyze.md) ve [Kubernetes küme sağlığını görüntüleyin.](container-insights-health.md)
