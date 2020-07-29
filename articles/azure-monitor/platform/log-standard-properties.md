---
title: Azure Izleme günlük kayıtlarında standart özellikler | Microsoft Docs
description: Azure Izleyici günlüklerinde birden çok veri türü için ortak olan özellikleri açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/01/2020
ms.openlocfilehash: 9c7e6733a9149008f6ed98ae7a7e0103f92f79f5
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87325279"
---
# <a name="standard-properties-in-azure-monitor-logs"></a>Azure Izleyici günlüklerinde standart özellikler
Azure Izleyici günlüklerindeki veriler, her biri benzersiz bir özellik kümesine sahip olan belirli bir veri türüne sahip bir [Log Analytics çalışma alanında veya Application Insights uygulamasında bir kayıt kümesi olarak depolanır](../log-query/logs-structure.md). Birçok veri türü, birden çok tür genelinde ortak olan standart özelliklere sahip olacaktır. Bu makalede bu özellikler açıklanmakta ve bunları sorgularda nasıl kullanabileceğiniz hakkında örnekler verilmektedir.

> [!IMPORTANT]
> APM 2,1 kullanıyorsanız, Application Insights uygulamalar diğer tüm günlük verileriyle birlikte bir Log Analytics çalışma alanında depolanır. Tablolar yeniden adlandırıldı ve yeniden yapılandırıldı, ancak Application Insights uygulamasındaki tablolarla aynı bilgilere sahip. Bu yeni tablolar, Log Analytics çalışma alanındaki diğer tablolarla aynı standart özelliklere sahiptir.

> [!NOTE]
> Standart özelliklerden bazıları Log Analytics şema görünümünde veya IntelliSense 'de gösterilmez ve bu özellik çıkışta özelliği açıkça belirtmediğiniz takdirde sorgu sonuçlarında gösterilmez.

## <a name="timegenerated-and-timestamp"></a>TimeGenerated ve zaman damgası
**TimeGenerated** (Log Analytics çalışma alanı) ve **zaman damgası** (Application Insights uygulaması) özellikleri, kaydın veri kaynağı tarafından oluşturulduğu tarih ve saati içerir. Daha fazla ayrıntı için bkz. [Azure izleyici 'de günlük verisi alma süresi](data-ingestion-time.md) .

**TimeGenerated** ve **timestamp** zamana göre filtreleme veya özetleme için kullanılacak ortak bir özellik sağlar. Azure portal bir görünüm veya Pano için zaman aralığı seçtiğinizde, sonuçları filtrelemek için TimeGenerated veya timestamp kullanılır. 

### <a name="examples"></a>Örnekler

Aşağıdaki sorgu, önceki haftanın her günü için oluşturulan hata olaylarının sayısını döndürür.

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by TimeGenerated asc 
```

Aşağıdaki sorgu, önceki hafta içinde her gün için oluşturulan özel durumların sayısını döndürür.

```Kusto
exceptions
| where timestamp between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by timestamp asc 
```

## <a name="_timereceived"></a>\_Timerecelmiş
** \_ Timereceiıntıtıon** özelliği, Azure bulutundaki Azure izleyici alma noktası tarafından kaydın alındığı tarih ve saati içerir. Bu, veri kaynağı ve bulut arasındaki gecikme sorunlarını belirlemek için yararlı olabilir. Bir örneğin, bir aracıdan alınan verilerle gecikmeye neden olan bir ağ sorunu olabilir. Daha fazla ayrıntı için bkz. [Azure izleyici 'de günlük verisi alma süresi](data-ingestion-time.md) .

Aşağıdaki sorgu, bir aracıdan gelen olay kayıtları için saate göre ortalama gecikme süresini verir. Bu, aracıdan buluta kadar olan süreyi ve kayıt sorgularının günlük sorguları için kullanılabilir olması için toplam süreyi içerir.

```Kusto
Event
| where TimeGenerated > ago(1d) 
| project TimeGenerated, TimeReceived = _TimeReceived, IngestionTime = ingestion_time() 
| extend AgentLatency = toreal(datetime_diff('Millisecond',TimeReceived,TimeGenerated)) / 1000
| extend TotalLatency = toreal(datetime_diff('Millisecond',IngestionTime,TimeGenerated)) / 1000
| summarize avg(AgentLatency), avg(TotalLatency) by bin(TimeGenerated,1hr)
``` 

## <a name="type-and-itemtype"></a>Tür ve ItemType
**Tür** (Log Analytics çalışma alanı) ve **ıtemtype** (Application Insights uygulama) özellikleri, kaydın alındığı tablonun adını, kayıt türü olarak da düşünülebilir. Bu özellik, `search` farklı türlerin kayıtlarını ayırt etmek için işleç kullananlar gibi birden çok tablodan kayıtları birleştiren sorgularda yararlıdır. **$Table** , bazı yerlerde **tür** yerine kullanılabilir.

### <a name="examples"></a>Örnekler
Aşağıdaki sorgu, son saate göre toplanan kayıt sayısını döndürür.

```Kusto
search * 
| where TimeGenerated > ago(1h)
| summarize count() by Type

```
## <a name="_itemid"></a>\_ID
** \_ ItemId** özelliği, kayıt için benzersiz bir tanımlayıcı tutar.


## <a name="_resourceid"></a>\_ResourceId
** \_ RESOURCEID** özelliği, kaydın ilişkilendirildiği kaynak için benzersiz bir tanımlayıcı tutar. Bu, sorgunuzu yalnızca belirli bir kaynaktaki kayıtlarla birleştirmek veya ilgili verileri birden çok tablo genelinde birleştirmek için kullanabileceğiniz standart bir özellik sunar.

Azure kaynakları için **_ResourceId** değeri [Azure kaynak kimliği URL 'sidir](../../azure-resource-manager/templates/template-functions-resource.md). Özelliği şu anda Azure kaynaklarıyla sınırlıdır, ancak şirket içi bilgisayarlar gibi Azure dışındaki kaynaklara genişletilir.

> [!NOTE]
> Bazı veri türlerinde zaten Azure Kaynak KIMLIĞI veya abonelik KIMLIĞI gibi en az parçalar içeren alanlar var. Bu alanlar geriye dönük uyumluluk için tutulurken, daha tutarlı olacağı için _ResourceId çapraz bağıntı gerçekleştirmek üzere kullanılması önerilir.

### <a name="examples"></a>Örnekler
Aşağıdaki sorgu, her bilgisayar için performans ve olay verilerini birleştirir. KIMLIĞI _101_ olan tüm olayları ve %50 üzerinde işlemci kullanımını gösterir.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

Aşağıdaki sorgu, _AzureActivity_ kayıtlarını _securityevent_ kayıtlarıyla birleştirir. Bu makinede oturum açmış kullanıcılarla tüm etkinlik işlemleri gösterilir.

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

Aşağıdaki sorgu **_ResourceId** ayrıştırır ve Azure aboneliği başına faturalanan veri birimlerini toplar.

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last 
```

`union withsource = tt *`Veri türlerindeki taramaların yürütülmesi pahalı olduğundan bu sorguları dikkatli bir şekilde kullanın.

## <a name="_isbillable"></a>\_Miktarbirimi
** \_ Ifaturalanabilir** özelliği, alınan verilerin faturalandırılabilir olup olmadığını belirtir. ** \_ Ifaturalanabilir** değerine eşit olan veriler `false` ücretsiz olarak toplanır ve Azure hesabınıza faturalandırılmaz.

### <a name="examples"></a>Örnekler
Faturalanan veri türlerini gönderen bilgisayarların listesini almak için aşağıdaki sorguyu kullanın:

> [!NOTE]
> `union withsource = tt *`Veri türlerinde taramaların yürütülmesi pahalı olan sorguları gelişigüzel şekilde kullanın. 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Bu, faturalanan veri türlerini Gönderen saat başına bilgisayar sayısını döndürmek için Genişletilebilir:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="_billedsize"></a>\_BilledSize
** \_ Billedsize** özelliği, ** \_ ısfaturalandırılabilir** değeri true ise Azure hesabınıza faturalandırılacak verilerin bayt cinsinden boyutunu belirtir.


### <a name="examples"></a>Örnekler
Bilgisayar başına alınan faturalandırılabilir olayların boyutunu görmek için, `_BilledSize` boyutu bayt cinsinden sağlayan özelliği kullanın:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last 
```

Abonelik başına alınan faturalandırılabilir olayların boyutunu görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource=table * 
| where _IsBillable == true 
| parse _ResourceId with "/subscriptions/" SubscriptionId "/" *
| summarize Bytes=sum(_BilledSize) by  SubscriptionId | sort by Bytes nulls last 
```

Kaynak grubu başına alınan faturalandırılabilir olayların boyutunu görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource=table * 
| where _IsBillable == true 
| parse _ResourceId with "/subscriptions/" SubscriptionId "/resourcegroups/" ResourceGroupName "/" *
| summarize Bytes=sum(_BilledSize) by  SubscriptionId, ResourceGroupName | sort by Bytes nulls last 

```


Bilgisayar başına alınan olayların sayısını görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last
```

Bilgisayar başına alınan faturalandırılabilir olayların sayısını görmek için aşağıdaki sorguyu kullanın: 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last
```

Belirli bir bilgisayardan faturalanabilir veri türleri sayısını görmek için aşağıdaki sorguyu kullanın:

```Kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last 
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici günlük verilerinin nasıl depolandığı](../log-query/log-query-overview.md)hakkında daha fazla bilgi edinin.
- [Günlük sorgularını yazarken](../log-query/get-started-queries.md)bir ders alın.
- [Günlük sorgularında tabloları birleştirme](../log-query/joins.md)konusunda bir ders alın.

