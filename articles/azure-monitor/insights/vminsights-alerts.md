---
title: VM'ler için Azure Monitör'den uyarılar
description: Azure Monitor tarafından VM'ler için toplanan performans verilerinden uyarı kurallarının nasıl oluşturulutamamgerektiğini açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/23/2020
ms.openlocfilehash: 987537d8497b3d8f2728941334d8328320ec6997
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289608"
---
# <a name="how-to-create-alerts-from-azure-monitor-for-vms"></a>VM'ler için Azure Monitor'dan uyarı oluşturma
[Azure Monitor'daki uyarılar,](../platform/alerts-overview.md) izleme verilerinizdeki ilginç verileri ve desenleri proaktif olarak size bildirir. Sanal Taşıtlar için Azure Monitörü önceden yapılandırılmış uyarı kuralları içermez, ancak topladığı verilere dayanarak kendi uyarı kurallarınızı oluşturabilirsiniz. Bu makalede, örnek sorgular kümesi de dahil olmak üzere uyarı kuralları oluşturma konusunda rehberlik sağlar.


## <a name="alert-rule-types"></a>Uyarı kuralı türleri
Azure Monitor, uyarıyı oluşturmak için kullanılan verilere dayalı [olarak farklı türde uyarı kurallarına](../platform/alerts-overview.md#what-you-can-alert-on) sahiptir. Azure Monitor tarafından Sanal Taşıtlar için toplanan tüm veriler, [günlük uyarılarını](../platform/alerts-log.md)destekleyen Azure Monitor Günlükleri'nde depolanır. Veriler Azure Monitör Ölçümleri'nde toplanmadığından, VM'ler için Azure Monitor'dan toplanan performans verileriyle şu anda [metrik uyarıları](../platform/alerts-log.md) kullanamazsınız. Metrik uyarılar için veri toplamak için, Performans verilerini Ölçümlere toplamak [için](../platform/diagnostics-extension-overview.md) Windows VM'leri veya Linux VM'leri için [Telegraf aracısını](../platform/collect-custom-metrics-linux-telegraf.md) yükleyin.

Azure Monitor'da iki tür günlük uyarısı vardır:

- Bir sorgu en az belirli sayıda kayıt döndürdüğünde [sonuç uyarılarının sayısı](../platform/alerts-unified-log.md#number-of-results-alert-rules) tek bir uyarı oluşturur. Bunlar, [Log Analytics aracısı](../platform/log-analytics-agent.md) tarafından toplanan Windows ve Syslog etkinlikleri gibi sayısal olmayan veriler veya birden çok bilgisayardaki performans eğilimlerini analiz etmek için idealdir.
- [Metrik ölçüm uyarıları,](../platform/alerts-unified-log.md#metric-measurement-alert-rules) uyarı kuralında tanımlanan eşiği aşan bir değere sahip bir sorgudaki her kayıt için ayrı bir uyarı oluşturur. Bu uyarı kuralları, her bilgisayar için ayrı uyarılar oluşturabildiklerinden, Sanal Taşıtlar için Azure Monitor tarafından toplanan performans verileri için idealdir.


## <a name="alert-rule-walkthrough"></a>Uyarı kuralı gözden geçirme
Bu bölüm, VM'ler için Azure Monitor'dan alınan performans verilerini kullanarak bir metrik ölçüm uyarı kuralı oluşturulmasından geçer. Bu temel işlemi, farklı performans sayaçlarında uyarmak için çeşitli günlük sorgularıyla kullanabilirsiniz.

[Azure Monitor'u kullanarak günlük uyarılarını oluştur, görüntüle ve yönet](../platform/alerts-log.md)yordamını izleyerek yeni bir uyarı kuralı oluşturarak başlayın. **Kaynak**için, Azure Monitor VM'lerinin aboneliğinizde kullandığı Günlük Analizi çalışma alanını seçin. Günlük uyarı kuralları için hedef kaynak her zaman bir Log Analytics çalışma alanı olduğundan, günlük sorgusu belirli sanal makineler veya sanal makine ölçek kümeleri için herhangi bir filtre içermelidir. 

Uyarı kuralının **durumu** için, [aşağıdaki bölümdeki](#sample-alert-queries) sorgulardan birini **Arama sorgusu**olarak kullanın. Sorgu, *AggregatedValue*adlı sayısal bir özellik döndürmelidir. Eşiği aşan her sanal makine için ayrı bir uyarı oluşturabilmeniz için verileri bilgisayar tarafından özetlemelidir.

Uyarı **mantığında**Metrik **ölçümü** seçin ve ardından **bir Eşik değeri**sağlayın. **Tetikleyici Uyarı Tabanlı'da,** bir uyarı oluşturulmadan önce eşiğin kaç kez aşılması gerektiğini belirtin. Örneğin, işlemcinin bir eşiği bir kez aşıp sonra normale dönmesi büyük olasılıkla umurunda değildir, ancak birden çok ardışık ölçümde eşiği aşmaya devam ederse umursamış olursunuz.

**Bölüme göre değerlendirilen,** sorgunun ne sıklıkta çalıştırıldığını ve sorgunun zaman penceresini tanımlar. Aşağıda gösterilen örnekte, sorgu her 15 dakikada bir çalışır ve önceki 15 dakika boyunca toplanan performans değerlerini değerlendirir.


![Metrik ölçüm uyarı kuralı](media/vminsights-alerts/metric-measurement-alert.png)

## <a name="sample-alert-queries"></a>Örnek uyarı sorguları
Aşağıdaki sorgular, Azure Monitor tarafından VM'ler için toplanan performans verilerini kullanarak metrik ölçüm uyarı kuralıyla kullanılabilir. Her biri, eşiği aşan bir değere sahip her bilgisayar için bir uyarı oluşturulacak şekilde verileri bilgisayar tarafından özetler.

### <a name="cpu-utilization"></a>CPU kullanımı

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms" 
| where Namespace == "Processor" and Name == "UtilizationPercentage" 
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId
```

### <a name="available-memory-in-mb"></a>MB'de Kullanılabilir Bellek

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Memory" and Name == "AvailableMB"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId
```

### <a name="available-memory-in-percentage"></a>Kullanılabilir Bellek yüzde olarak

```kusto
InsightsMetrics 
| where Origin == "vm.azm.ms" 
| where Namespace == "Memory" and Name == "AvailableMB" 
| extend TotalMemory = toreal(todynamic(Tags)["vm.azm.ms/memorySizeMB"])
| extend AvailableMemoryPercentage = (toreal(Val) / TotalMemory) * 100.0 
| summarize AggregatedValue = avg(AvailableMemoryPercentage) by bin(TimeGenerated, 15m), Computer, _ResourceId 
```

### <a name="logical-disk-used---all-disks-on-each-computer"></a>Kullanılan mantıksal disk - her bilgisayardaki tüm diskler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "LogicalDisk" and Name == "FreeSpacePercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId 
```

### <a name="logical-disk-used---individual-disks"></a>Kullanılan mantıksal disk - tek tek diskler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "LogicalDisk" and Name == "FreeSpacePercentage"
| extend Disk=tostring(todynamic(Tags)["vm.azm.ms/mountId"])
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId, Disk
```

### <a name="logical-disk-iops"></a>Mantıksal disk IOPS

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

### <a name="network-interfaces-bytes-received---all-interfaces"></a>Alınan ağ arabirimleri bayt - tüm arayüzler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Network" and Name == "ReadBytesPerSecond"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId 
```

### <a name="network-interfaces-bytes-received---individual-interfaces"></a>Alınan ağ arabirimleri bayt - bireysel arabirimler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Network" and Name == "ReadBytesPerSecond"
| extend NetworkInterface=tostring(todynamic(Tags)["vm.azm.ms/networkDeviceId"])
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId, NetworkInterface
```

### <a name="network-interfaces-bytes-sent---all-interfaces"></a>Gönderilen ağ arabirimleri bayt - tüm arayüzler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Network" and Name == "WriteBytesPerSecond"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId
```

### <a name="network-interfaces-bytes-sent---individual-interfaces"></a>Gönderilen ağ arabirimleri bayt - tek tek arabirimler

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Network" and Name == "WriteBytesPerSecond"
| extend NetworkInterface=tostring(todynamic(Tags)["vm.azm.ms/networkDeviceId"])
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), Computer, _ResourceId, NetworkInterface
```

### <a name="virtual-machine-scale-set"></a>Sanal makine ölçek kümesi
Abonelik kimliğiniz, kaynak grubunuz ve sanal makine ölçeği kümesi adınız ile değiştirin.

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where _ResourceId startswith "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachineScaleSets/my-vm-scaleset" 
| where Namespace == "Processor" and Name == "UtilizationPercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), _ResourceId
```

### <a name="specific-virtual-machine"></a>Özel sanal makine
Abonelik kimliğiniz, kaynak grubunuz ve VM adınız ile değiştirin.

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where _ResourceId =~ "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-vm" 
| where Namespace == "Processor" and Name == "UtilizationPercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m)
```

### <a name="cpu-utilization-for-all-compute-resources-in-a-subscription"></a>Abonelikteki tüm işlem kaynakları için CPU kullanımı
Abonelik kimliğinizle değiştirin.

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where _ResourceId startswith "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" and (_ResourceId contains "/providers/Microsoft.Compute/virtualMachines/" or _ResourceId contains "/providers/Microsoft.Compute/virtualMachineScaleSets/")
| where Namespace == "Processor" and Name == "UtilizationPercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), _ResourceId
```

### <a name="cpu-utilization-for-all-compute-resources-in-a-resource-group"></a>Kaynak grubundaki tüm bilgi işlem kaynakları için CPU kullanımı
Abonelik kimliğiniz ve kaynak grubunuzla değiştirin.

```kusto
InsightsMetrics
| where Origin == "vm.azm.ms"
| where _ResourceId startswith "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/"
or _ResourceId startswith "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachineScaleSets/" 
| where Namespace == "Processor" and Name == "UtilizationPercentage"
| summarize AggregatedValue = avg(Val) by bin(TimeGenerated, 15m), _ResourceId

```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitor'daki uyarılar](../platform/alerts-overview.md)hakkında daha fazla bilgi edinin.
- [Sanal Veriler için Azure Monitor'dan](vminsights-log-search.md)gelen verileri kullanarak günlük sorguları hakkında daha fazla bilgi edinin.
