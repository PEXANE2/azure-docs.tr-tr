---
title: VM'ler için Azure İzleyici uyarılar
description: VM'ler için Azure İzleyici tarafından toplanan performans verilerinden uyarı kuralları oluşturmayı açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/23/2020
ms.openlocfilehash: 987537d8497b3d8f2728941334d8328320ec6997
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80289608"
---
# <a name="how-to-create-alerts-from-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici uyarılar oluşturma
[Azure izleyici 'Deki uyarılar](../platform/alerts-overview.md) , izleme verilerinizde ilgi çekici veriler ve desenler konusunda size bir bildirim gönderir. VM'ler için Azure İzleyici önceden yapılandırılmış uyarı kuralları içermez, ancak topladığı verilere göre kendi kendinize de oluşturabilirsiniz. Bu makale, örnek sorgu kümesi de dahil olmak üzere uyarı kuralları oluşturma hakkında rehberlik sağlar.


## <a name="alert-rule-types"></a>Uyarı kuralı türleri
Azure Izleyici, uyarı oluşturmak için kullanılan verileri temel alan [farklı tür uyarı kuralları](../platform/alerts-overview.md#what-you-can-alert-on) içerir. VM'ler için Azure İzleyici tarafından toplanan tüm veriler, [günlük uyarılarını](../platform/alerts-log.md)destekleyen Azure izleyici günlüklerinde depolanır. Veriler Azure Izleyici ölçümlerine toplanmadığından, şu anda [ölçüm uyarılarını](../platform/alerts-log.md) VM'ler için Azure izleyici tarafından toplanan performans verileriyle kullanamazsınız. Ölçüm uyarıları için veri toplamak üzere Windows VM 'Ler için [Tanılama uzantısını](../platform/diagnostics-extension-overview.md) veya Linux sanal makineleri için [telegraf aracısını](../platform/collect-custom-metrics-linux-telegraf.md) , performans verilerini ölçümlere toplamak üzere yükler.

Azure Izleyici 'de iki tür günlük uyarısı vardır:

- Sorgu en az belirtilen sayıda kayıt döndürdüğünde, [sonuç uyarıları sayısı](../platform/alerts-unified-log.md#number-of-results-alert-rules) tek bir uyarı oluşturur. Bunlar, [Log Analytics Aracısı](../platform/log-analytics-agent.md) tarafından toplanan sayısal olmayan veriler ve Windows ve Syslog olayları için veya birden çok bilgisayardaki performans eğilimlerini analiz etmek için idealdir.
- [Ölçüm ölçüm uyarıları](../platform/alerts-unified-log.md#metric-measurement-alert-rules) , uyarı kuralında tanımlanan bir eşiği aşan bir değer içeren bir sorgudaki her kayıt için ayrı bir uyarı oluşturur. Bu uyarı kuralları, her bilgisayar için ayrı uyarılar oluşturduklarında VM'ler için Azure İzleyici tarafından toplanan performans verileri için idealdir.


## <a name="alert-rule-walkthrough"></a>Uyarı kuralı Kılavuzu
Bu bölüm, VM'ler için Azure İzleyici performans verilerini kullanarak bir ölçüm ölçümü uyarı kuralının oluşturulmasını açıklar. Bu temel işlemi, farklı performans sayaçlarıyla uyarı vermek için çeşitli günlük sorgularıyla kullanabilirsiniz.

[Azure izleyici 'yi kullanarak günlük uyarılarını oluşturma, görüntüleme ve yönetme](../platform/alerts-log.md)bölümündeki yordamı izleyerek yeni bir uyarı kuralı oluşturarak başlayın. **Kaynak**Için, Azure izleyici VM 'lerinin aboneliğinizde kullandığı Log Analytics çalışma alanını seçin. Günlük uyarısı kuralları için hedef kaynak her zaman bir Log Analytics çalışma alanı olduğundan, günlük sorgusunun belirli sanal makineler veya sanal makine ölçek kümeleri için herhangi bir filtre içermesi gerekir. 

Uyarı kuralının **koşulu** Için, **arama sorgusu**olarak [aşağıdaki bölümdeki](#sample-alert-queries) sorgulardan birini kullanın. Sorgu, *aggregdbulunan değeri*adlı bir sayısal Özellik döndürmelidir. Eşiği aşan her bir sanal makine için ayrı bir uyarı oluşturabilmeniz için verileri bilgisayara göre özetlemelidir.

**Uyarı mantığındaki** **ölçüm ölçümü** ' ni seçin ve ardından bir **eşik değeri**belirtin. **Tetikleyici uyarısı temelinde**, bir uyarı oluşturulmadan önce eşiğin kaç kez aşılacağı belirtin. Örneğin, işlemcinin bir eşiği bir kez aşıp daha sonra normal 'e geri döndürdüğünü de dikkate almamanız gerekir, ancak birden çok ardışık ölçüm üzerinden eşiği aşmaya devam ederse dikkatli olursunuz.

Bölüm **temelinde değerlendirilen** , sorgunun ne sıklıkta çalıştırılacağını ve sorgu için zaman penceresini tanımlar. Aşağıda gösterilen örnekte, sorgu 15 dakikada bir çalışacaktır ve önceki 15 dakika içinde toplanan performans değerlerini değerlendirmeyecektir.


![Ölçüm ölçümü uyarı kuralı](media/vminsights-alerts/metric-measurement-alert.png)

## <a name="sample-alert-queries"></a>Örnek uyarı sorguları
Aşağıdaki sorgular, VM'ler için Azure İzleyici tarafından toplanan performans verilerini kullanan bir ölçüm ölçümü uyarı kuralıyla birlikte kullanılabilir. Her biri, eşiği aşan bir değere sahip her bilgisayar için bir uyarının oluşturulması için verileri bilgisayara göre özetler.

### <a name="cpu-utilization"></a>CPU kullanımı

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms" 
| where Namespace == "Processor" and Name == "UtilizationPercentage" 
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId
```

### <a name="available-memory-in-mb"></a>MB cinsinden kullanılabilir bellek

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Memory" and Name == "AvailableMB"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId
```

### <a name="available-memory-in-percentage"></a>Kullanılabilir bellek yüzdesi

```kusto
InsightsMetrics 
| where Origin == "vm.azm.ms" 
| where Namespace == "Memory" and Name == "AvailableMB" 
| extend TotalMemory = toreal(todynamic(Tags)["vm.azm.ms/memorySizeMB"])
| extend AvailableMemoryPercentage = (toreal(Val) / TotalMemory) * 100.0 
| summarize AggregatedValue = avg(AvailableMemoryPercentage) by bin(TimeGenerated, 15m), Computer, _ResourceId 
```

### <a name="logical-disk-used---all-disks-on-each-computer"></a>Kullanılan mantıksal disk-her bilgisayardaki tüm diskler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "LogicalDisk" and Name == "FreeSpacePercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId 
```

### <a name="logical-disk-used---individual-disks"></a>Kullanılan mantıksal disk-ayrı diskler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "LogicalDisk" and Name == "FreeSpacePercentage"
| extend Disk=tostring(todynamic(Tags)["vm.azm.ms/mountId"])
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId, Disk
```

### <a name="logical-disk-iops"></a>Mantıksal disk ıOPS 'si

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms" 
| where Namespace == "LogicalDisk" and Name == "TransfersPerSecond"
| extend Disk=tostring(todynamic(Tags)["vm.azm.ms/mountId"])
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m) ), Computer, _ResourceId, Disk
```

### <a name="logical-disk-data-rate"></a>Mantıksal disk veri hızı

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms" 
| where Namespace == "LogicalDisk" and Name == "BytesPerSecond"
| extend Disk=tostring(todynamic(Tags)["vm.azm.ms/mountId"])
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m) , Computer, _ResourceId, Disk
```

### <a name="network-interfaces-bytes-received---all-interfaces"></a>Ağ arabirimleri alınan bayt-tüm arabirimler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Network" and Name == "ReadBytesPerSecond"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId 
```

### <a name="network-interfaces-bytes-received---individual-interfaces"></a>Ağ arabirimleri alınan bayt-tek arabirimler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Network" and Name == "ReadBytesPerSecond"
| extend NetworkInterface=tostring(todynamic(Tags)["vm.azm.ms/networkDeviceId"])
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId, NetworkInterface
```

### <a name="network-interfaces-bytes-sent---all-interfaces"></a>Ağ arabirimleri gönderilen bayt-tüm arabirimler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Network" and Name == "WriteBytesPerSecond"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId
```

### <a name="network-interfaces-bytes-sent---individual-interfaces"></a>Ağ arabirimleri gönderilen bayt-tek arabirimler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Network" and Name == "WriteBytesPerSecond"
| extend NetworkInterface=tostring(todynamic(Tags)["vm.azm.ms/networkDeviceId"])
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId, NetworkInterface
```

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi
Abonelik KIMLIĞINIZ, kaynak grubunuz ve sanal makine ölçek kümesi adınızla değiştirin.

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where _ResourceId startswith "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachineScaleSets/my-vm-scaleset" 
| where Namespace == "Processor" and Name == "UtilizationPercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), _ResourceId
```

### <a name="specific-virtual-machine"></a>Belirli bir sanal makine
Abonelik KIMLIĞINIZ, kaynak grubunuz ve VM adınızla değiştirin.

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where _ResourceId =~ "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-vm" 
| where Namespace == "Processor" and Name == "UtilizationPercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m)
```

### <a name="cpu-utilization-for-all-compute-resources-in-a-subscription"></a>Bir abonelikteki tüm işlem kaynakları için CPU kullanımı
Abonelik KIMLIĞINIZLE değiştirin.

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where _ResourceId startswith "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" and (_ResourceId contains "/providers/Microsoft.Compute/virtualMachines/" or _ResourceId contains "/providers/Microsoft.Compute/virtualMachineScaleSets/")
| where Namespace == "Processor" and Name == "UtilizationPercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), _ResourceId
```

### <a name="cpu-utilization-for-all-compute-resources-in-a-resource-group"></a>Bir kaynak grubundaki tüm işlem kaynakları için CPU kullanımı
Abonelik KIMLIĞINIZ ve kaynak grubuyla değiştirin.

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where _ResourceId startswith "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/"
or _ResourceId startswith "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachineScaleSets/" 
| where Namespace == "Processor" and Name == "UtilizationPercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), _ResourceId

```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici 'de uyarılar](../platform/alerts-overview.md)hakkında daha fazla bilgi edinin.
- [VM'ler için Azure izleyici verileri kullanarak günlük sorguları](vminsights-log-search.md)hakkında daha fazla bilgi edinin.
