---
title: Azure Monitor günlük kayıtlarındaki standart özellikler | Microsoft Dokümanlar
description: Azure Monitor günlüklerinde birden çok veri türünde ortak olan özellikleri açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/18/2019
ms.openlocfilehash: 252ddeb372744986df0b8ba9b742d0462a4e8202
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274482"
---
# <a name="standard-properties-in-azure-monitor-logs"></a>Azure Monitör Günlükleri'ndeki standart özellikler
Azure Monitör Günlükleri'ndeki veriler, her biri benzersiz bir özellik kümesine sahip belirli bir veri türüne sahip [bir Log Analytics çalışma alanında veya Application Insights uygulamasında bir kayıt kümesi olarak depolanır.](../log-query/logs-structure.md) Birçok veri türü, birden çok türarasında yaygın olan standart özelliklere sahip olacaktır. Bu makalede, bu özellikleri açıklar ve sorgularda bunları nasıl kullanabileceğinize örnekler sağlar.

> [!NOTE]
> Standart özelliklerden bazıları Log Analytics'te şema görünümünde veya intellisense'de gösterilmez ve çıktıdaki özelliği açıkça belirtmediğiniz sürece sorgu sonuçlarında gösterilmez.

## <a name="timegenerated-and-timestamp"></a>Zaman Ve zaman damgası
**TimeGenerated** (Log Analytics çalışma alanı) ve **zaman damgası** (Application Insights uygulaması) özellikleri, kaydın veri kaynağı tarafından oluşturulduğu tarih ve saati içerir. Daha fazla bilgi için [Azure Monitor'da veri alım süresini](data-ingestion-time.md) günlüğüne bakın.

**Zaman Oluşturuldu ve** **zaman damgası,** zamana göre filtreleme veya özetleme için kullanılacak ortak bir özellik sağlar. Azure portalındaki bir görünüm veya pano için bir zaman aralığı seçtiğinizde, sonuçları filtrelemek için TimeGenerated veya zaman damgası kullanır. 

### <a name="examples"></a>Örnekler

Aşağıdaki sorgu, önceki haftadaki her gün için oluşturulan hata olaylarının sayısını döndürür.

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by TimeGenerated asc 
```

Aşağıdaki sorgu, önceki haftadaki her gün için oluşturulan özel durum sayısını döndürür.

```Kusto
exceptions
| where timestamp between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by timestamp asc 
```

## <a name="_timereceived"></a>\_Zaman Alınan
** \_TimeReceived** özelliği, kaydın Azure bulutundaki Azure İzleme noktası tarafından alındığı tarihi ve saati içerir. Bu, veri kaynağı ve bulut arasındaki gecikme sorunlarını tanımlamak için yararlı olabilir. Bir örnek, bir aracıdan gönderilen verilerle gecikmeye neden olan bir ağ sorunu olabilir. Daha fazla bilgi için [Azure Monitor'da veri alım süresini](data-ingestion-time.md) günlüğüne bakın.

Aşağıdaki sorgu, bir aracıdan olay kayıtları için saat başına ortalama gecikme verir. Bu, aracıdan buluta geçen süreyi ve kaydın günlük sorguları için kullanılabilir olması için gereken toplam süreyi içerir.

```Kusto
Event
| where TimeGenerated > ago(1d) 
| project TimeGenerated, TimeReceived = _TimeReceived, IngestionTime = ingestion_time() 
| extend AgentLatency = toreal(datetime_diff('Millisecond',TimeReceived,TimeGenerated)) / 1000
| extend TotalLatency = toreal(datetime_diff('Millisecond',IngestionTime,TimeGenerated)) / 1000
| summarize avg(AgentLatency), avg(TotalLatency) by bin(TimeGenerated,1hr)
``` 

## <a name="type-and-itemtype"></a>Türü ve itemType
**Türü** (Log Analytics çalışma alanı) ve **itemType** (Application Insights uygulaması) özellikleri, kaydın alındığı tablonun adını tutar ve bu da kayıt türü olarak düşünülebilir. Bu özellik, farklı türdeki kayıtları ayırt etmek için `search` işleç kullananlar gibi birden çok tablodaki kayıtları birleştiren sorgularda yararlıdır. **$table** bazı yerlerde **Type** yerine kullanılabilir.

### <a name="examples"></a>Örnekler
Aşağıdaki sorgu, son bir saat içinde toplanan türe göre kayıtların sayısını döndürür.

```Kusto
search * 
| where TimeGenerated > ago(1h)
| summarize count() by Type

```
## <a name="_itemid"></a>\_ıtemıd
** \_ItemId** özelliği kayıt için benzersiz bir tanımlayıcı tutar.


## <a name="_resourceid"></a>\_ResourceId
** \_ResourceId** özelliği, kaydın ilişkili olduğu kaynak için benzersiz bir tanımlayıcı tutar. Bu, sorgunuzu yalnızca belirli bir kaynaktan gelen kayıtlara genişletmek veya ilgili verileri birden çok tabloda birleştirmek için kullanmak için kullanılacak standart bir özellik sağlar.

Azure kaynakları için **_ResourceId** değeri [Azure kaynak kimliği](../../azure-resource-manager/templates/template-functions-resource.md)URL'sidir. Özellik şu anda Azure kaynaklarıyla sınırlıdır, ancak şirket içi bilgisayarlar gibi Azure dışındaki kaynaklara genişletilir.

> [!NOTE]
> Bazı veri türlerinin zaten Azure kaynak kimliği veya abonelik kimliği gibi en azından bir kısmını içeren alanları vardır. Bu alanlar geriye dönük uyumluluk için tutulurken, daha tutarlı olacağı için çapraz korelasyon gerçekleştirmek için _ResourceId kullanılması önerilir.

### <a name="examples"></a>Örnekler
Aşağıdaki sorgu, her bilgisayar için performans ve olay verilerini birleştirir. _101_ kimliği ve %50'nin üzerinde işlemci kullanımı ile tüm olayları gösterir.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

Aşağıdaki sorgu, _AzureEtkinlik_ kayıtlarını _SecurityEvent_ kayıtlarıyla birleştirir. Bu makinelerde oturum açmış kullanıcılarile tüm etkinlik işlemleri gösterir.

```Kusto
AzureActivity 
| where  
    OperationName in ("Restart Virtual Machine", "Create or Update Virtual Machine", "Delete Virtual Machine")  
    and ActivityStatus == "Succeeded"  
| join kind= leftouter (    
   SecurityEvent 
   | where EventID == 4624  
   | summarize LoggedOnAccounts = makeset(Account) by _ResourceId 
) on _ResourceId  
```

Aşağıdaki **sorgu, _ResourceId** ayrıştırır ve Azure aboneliği başına faturalanan veri birimlerini toplar.

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last 
```

Veri `union withsource = tt *` türleri arasında taramalar yürütmek için pahalı olduğundan bu sorguları tutumlu kullanın.

## <a name="_isbillable"></a>\_IsBillable
IsBillable özelliği, yutulan verilerin faturalandırılabilir olup olmadığını belirtir. ** \_** ** \_IsBillable** false'a _false_ eşit olan veriler ücretsiz olarak toplanır ve Azure hesabınıza faturalandırılmaz.

### <a name="examples"></a>Örnekler
Faturalı veri türleri gönderen bilgisayarların listesini almak için aşağıdaki sorguyu kullanın:

> [!NOTE]
> Veri türleri `union withsource = tt *` arasında taramalar yürütmek için pahalı olduğundan tutumlu sorguları kullanın. 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Bu, faturalı veri türleri gönderen saat başına bilgisayar sayısını döndürmek için uzatılabilir:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="_billedsize"></a>\_Faturalı Boyut
** \_Faturalı Boyut** özelliği, ** \_IsBillable** doğruysa Azure hesabınıza fatura edilecek baytveri boyutu belirtir.


### <a name="examples"></a>Örnekler
Bilgisayar başına faturalandırılabilir olayların boyutunu görmek için, bayt boyutunu sağlayan `_BilledSize` özelliği kullanın:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last 
```

Abonelik başına yutulan faturalandırılabilir olayların boyutunu görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource=table * 
| where _IsBillable == true 
| parse _ResourceId with "/subscriptions/" SubscriptionId "/" *
| summarize Bytes=sum(_BilledSize) by  SubscriptionId | sort by Bytes nulls last 
```

Kaynak grubu başına kaptıktan sonra faturalandırılabilir olayların boyutunu görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource=table * 
| where _IsBillable == true 
| parse _ResourceId with "/subscriptions/" SubscriptionId "/resourcegroups/" ResourceGroupName "/" *
| summarize Bytes=sum(_BilledSize) by  SubscriptionId, ResourceGroupName | sort by Bytes nulls last 

```


Bilgisayar başına yutulan olayların sayısını görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last
```

Bilgisayar başına faturalandırılabilir olayların sayısını görmek için aşağıdaki sorguyu kullanın: 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last
```

Belirli bir bilgisayardan faturalandırılabilir veri türlerinin sayısını görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last 
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitor [günlük verilerinin](../log-query/log-query-overview.md)nasıl depolanması hakkında daha fazla bilgi edinin.
- [Günlük sorguları yazma](../../azure-monitor/log-query/get-started-queries.md)hakkında bir ders alın.
- [Günlük sorgularında tabloları birleştirme](../../azure-monitor/log-query/joins.md)konusunda bir ders alın.
