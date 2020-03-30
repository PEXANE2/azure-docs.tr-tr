---
title: Azure Monitör'de uyarı sorgularını günlüğe kaydetme | Microsoft Dokümanlar
description: Azure Monitor güncelleştirmelerinde günlük uyarıları için verimli sorgular yazma ve varolan sorguları dönüştürme işlemi hakkında öneriler sağlar.
author: yossi-y
ms.author: yossiy
ms.topic: conceptual
ms.date: 02/19/2019
ms.subservice: alerts
ms.openlocfilehash: fdf492b8f103e725046b9b1cbbd079c4d249664a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77667797"
---
# <a name="log-alert-queries-in-azure-monitor"></a>Azure Monitor'da uyarı sorgularını günlüğe kaydetme
[Azure Monitor günlüklerine dayalı uyarı kuralları](alerts-unified-log.md) düzenli aralıklarla çalışır, bu nedenle genel ek yükü ve gecikme süresini en aza indirmek için yazıldığından emin olmalısınız. Bu makalede, günlük uyarıları için verimli sorgular yazma ve varolan sorguları dönüştürmek için bir işlem önerileri sağlar. 

## <a name="types-of-log-queries"></a>Günlük sorgu türleri
[Azure Monitor'da günlük sorguları](../log-query/log-query-overview.md) bir tablo veya [arama](/azure/kusto/query/searchoperator) veya [birleşim](/azure/kusto/query/unionoperator) işleciyle başlar.

Örneğin, aşağıdaki sorgu _SecurityEvent_ tablosuna ve belirli olay kimliği için aramalar kapsamındadır. Bu, sorgunun işlemesi gereken tek tablodur.

``` Kusto
SecurityEvent | where EventID == 4624 
```

Bir tablodaki `search` birden `union` çok sütunda ve hatta birden çok tabloda arama yapmak için başlayan veya buna izin veren sorgular. Aşağıdaki örnekler, _Bellek_terimini aramak için birden çok yöntem gösterir:

```Kusto
search "Memory"
search * | where == "Memory"
search ObjectName: "Memory"
search ObjectName == "Memory"
union * | where ObjectName == "Memory"
```

Veri `search` `union` arama sırasında yararlı olsa da ve tüm veri modeli üzerinde arama terimleri, onlar birden çok tablo arasında taramak gerekir, çünkü bir tablo kullanarak daha az verimlidir. Uyarı kurallarındaki sorgular düzenli aralıklarla çalıştırıladığından, bu durum uyarıya aşırı ek yükü eklemeyle sonuçlanabilir. Bu ek yükü nedeniyle, Azure'da günlük uyarı kuralları için sorgular her zaman net bir kapsam tanımlamak için bir tabloyla başlamalıdır ve bu da hem sorgu performansını hem de sonuçların alaka düzeyini artırır.

## <a name="unsupported-queries"></a>Desteklenmeyen sorgular
11 Ocak 2019'dan itibaren Azure portalında `search`kullanan `union` veya kullanan günlük uyarı kuralları nın oluşturulması veya değiştirilmesi desteklenmez. Bu işleçleri bir uyarı kuralında kullanmak bir hata iletisi döndürecektir. Log Analytics API ile oluşturulan ve düzenlenen varolan uyarı kuralları ve uyarı kuralları bu değişiklikten etkilenmez. Yine de verimliliklerini artırmak için olsa bu tür sorguları kullanan herhangi bir uyarı kuralları değiştirmeyi düşünmelisiniz.  

Çapraz kaynak [sorguları](../log-query/cross-workspace-query.md) kullanan günlük uyarı kuralları, sorgu kapsamını belirli `union`kaynaklarla sınırlayan çapraz kaynak sorguları kullandığından bu değişiklikten etkilenmez. Bu, kullanılamayacak `union *` eşdeğer değildir.  Aşağıdaki örnek bir günlük uyarı kuralı geçerli olacaktır:

```Kusto
union 
app('Contoso-app1').requests, 
app('Contoso-app2').requests, 
workspace('Contoso-workspace1').Perf 
```

>[!NOTE]
>Günlük uyarıları çapraz [kaynak sorgusu](../log-query/cross-workspace-query.md) yeni [scheduledQueryRules API'de](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)desteklenir. Varsayılan olarak, Azure Monitor, eski Log Uyarıları API'sinden geçiş yaptığınız sürece Azure portalından yeni günlük uyarı kuralları oluşturmak için [eski Log Analytics Alert API'sini](api-alerts.md) kullanır. [legacy Log Alerts API](alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api) Geçişten sonra, yeni API Azure portalında yeni uyarı kuralları için varsayılan olur ve kaynaklar arası sorgu günlüğü uyarıları kuralları oluşturmanıza olanak tanır. [ScheduledQueryRules API için ARM şablonu](alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) kullanarak geçiş yapmadan [kaynaklar arası sorgu](../log-query/cross-workspace-query.md) günlüğü uyarı kuralları oluşturabilirsiniz – ancak bu uyarı kuralı, Azure portalından değil, [planlanmışQueryRules API'si](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) olsa da yönetilebilir.

## <a name="examples"></a>Örnekler
Aşağıdaki örnekler, bu `search` sorguları `union` uyarı kurallarıyla kullanmak üzere değiştirmek için kullanabileceğiniz günlük sorgularını içerir ve bunları sağlar.

### <a name="example-1"></a>Örnek 1
Aşağıdaki sorguyu kullanarak `search`performans bilgilerini alan bir günlük uyarı kuralı oluşturmak istiyorsunuz: 

``` Kusto
search * | where Type == 'Perf' and CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```
  

Bu sorguyu değiştirmek için, özelliklerin ait olduğu tabloyu tanımlamak için aşağıdaki sorguyu kullanarak başlayın:

``` Kusto
search * | where CounterName == '% Free Space'
| summarize by $table
```
 

Bu sorgunun sonucu, _CounterName_ özelliğinin _Perf_ tablosundan geldiğini gösterir. 

Bu sonucu, uyarı kuralı için kullanacağınız aşağıdaki sorguyu oluşturmak için kullanabilirsiniz:

``` Kusto
Perf 
| where CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```


### <a name="example-2"></a>Örnek 2
Aşağıdaki sorguyu kullanarak `search`performans bilgilerini alan bir günlük uyarı kuralı oluşturmak istiyorsunuz: 

``` Kusto
search ObjectName =="Memory" and CounterName=="% Committed Bytes In Use"  
| summarize Avg_Memory_Usage =avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
  

Bu sorguyu değiştirmek için, özelliklerin ait olduğu tabloyu tanımlamak için aşağıdaki sorguyu kullanarak başlayın:

``` Kusto
search ObjectName=="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize by $table 
```
 

Bu sorgunun sonucu, _ObjectName_ ve _CounterName_ özelliğinin _Perf_ tablosundan geldiğini gösterir. 

Bu sonucu, uyarı kuralı için kullanacağınız aşağıdaki sorguyu oluşturmak için kullanabilirsiniz:

``` Kusto
Perf 
| where ObjectName =="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize Avg_Memory_Usage=avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
 

### <a name="example-3"></a>Örnek 3

Her ikisini de `search` kullanan aşağıdaki sorguyu kullanarak ve `union` performans bilgilerini almak için bir günlük uyarı kuralı oluşturmak istiyorsunuz: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| where Computer !in ((union * | where CounterName == "% Processor Utility" | summarize by Computer))
| summarize Avg_Idle_Time = avg(CounterValue) by Computer|  count  
```
 

Bu sorguyu değiştirmek için, sorgunun ilk bölümündeki özelliklerin ait olduğu tabloyu tanımlamak için aşağıdaki sorguyu kullanarak başlayın: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| summarize by $table 
```

Bu sorgunun sonucu, tüm bu _özelliklerin Perf_ tablosundan geldiğini gösterir. 

Şimdi `union` her `withsource` satıra hangi kaynak tablonun katkıda bulunduğunu belirlemek için komutla kullanın.

``` Kusto
union withsource=table * | where CounterName == "% Processor Utility" 
| summarize by table 
```
 

Bu sorgunun sonucu, bu özelliklerin de _Perf_ tablosundan geldiğini gösterir. 

Bu sonuçları, uyarı kuralı için kullanacağınız aşağıdaki sorguyu oluşturmak için kullanabilirsiniz: 

``` Kusto
Perf 
| where ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total" 
| where Computer !in ( 
    (Perf 
    | where CounterName == "% Processor Utility" 
    | summarize by Computer)) 
| summarize Avg_Idle_Time = avg(CounterValue) by Computer 
| count 
``` 

### <a name="example-4"></a>Örnek 4
İki `search` sorgunun sonuçlarını birleştiren aşağıdaki sorguyu kullanarak bir günlük uyarı kuralı oluşturmak istiyorsunuz:

```Kusto
search Type == 'SecurityEvent' and EventID == '4625' 
| summarize by Computer, Hour = bin(TimeGenerated, 1h) 
| join kind = leftouter ( 
    search in (Heartbeat) OSType == 'Windows' 
    | summarize arg_max(TimeGenerated, Computer) by Computer , Hour = bin(TimeGenerated, 1h) 
    | project Hour , Computer  
)  
on Hour 
| count 
```
 

Sorguyu değiştirmek için, birleştirmenin sol tarafındaki özellikleri içeren tabloyu tanımlamak için aşağıdaki sorguyu kullanarak başlayın: 

``` Kusto
search Type == 'SecurityEvent' and EventID == '4625' 
| summarize by $table 
```
 

Sonuç, birleşimin sol tarafındaki özelliklerin _SecurityEvent_ tablosuna ait olduğunu gösterir. 

Şimdi birleştirme sağ tarafında özellikleri içeren tablo tanımlamak için aşağıdaki sorguyu kullanın: 

 
``` Kusto
search in (Heartbeat) OSType == 'Windows' 
| summarize by $table 
```

 
Sonuç, birleşimin sağ tarafındaki özelliklerin Sinyal Tablosuna ait olduğunu gösterir. 

Bu sonuçları, uyarı kuralı için kullanacağınız aşağıdaki sorguyu oluşturmak için kullanabilirsiniz: 


``` Kusto
SecurityEvent
| where EventID == '4625'
| summarize by Computer, Hour = bin(TimeGenerated, 1h)
| join kind = leftouter (
    Heartbeat  
    | where OSType == 'Windows' 
    | summarize arg_max(TimeGenerated, Computer) by Computer , Hour = bin(TimeGenerated, 1h) 
    | project Hour , Computer  
)  
on Hour 
| count 
```

## <a name="next-steps"></a>Sonraki adımlar
- Azure Monitor'da [günlük uyarıları](alerts-log.md) hakkında bilgi edinin.
- Günlük [sorguları](../log-query/log-query-overview.md)hakkında bilgi edinin.

