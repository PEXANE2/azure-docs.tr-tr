---
title: Azure Izleyici 'de günlük uyarı sorguları | Microsoft Docs
description: Azure Izleyici güncelleştirmelerinde günlük uyarılarına yönelik etkili sorgular yazma ve mevcut sorguları dönüştürme işlemi için öneriler sağlar.
author: yossi-y
ms.author: yossiy
ms.topic: conceptual
ms.date: 02/19/2019
ms.subservice: alerts
ms.openlocfilehash: be2d49a824066b8926ae455978facb34c0b44310
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86505474"
---
# <a name="log-alert-queries-in-azure-monitor"></a>Azure Izleyici 'de günlük uyarısı sorguları
[Azure izleyici günlüklerine dayanan uyarı kuralları](alerts-unified-log.md) düzenli aralıklarla çalışır, bu nedenle ek yükü ve gecikmeyi en aza indirmek için yazıldıklarından emin olun. Bu makalede, günlük uyarılarına yönelik etkili sorgular ve mevcut sorguları dönüştürme işlemi için yazma önerileri sağlanır. 

## <a name="types-of-log-queries"></a>Günlük sorgularının türleri
[Azure izleyici 'de günlük sorguları](../log-query/log-query-overview.md) bir tablo veya [arama](/azure/kusto/query/searchoperator) ya da [birleşim](/azure/kusto/query/unionoperator) işleciyle başlar.

Örneğin, aşağıdaki sorgu _securityevent_ tablosunun kapsamına alınır ve belırlı olay kimliğini arar. Bu, sorgunun işlemesi gereken tek tablodur.

``` Kusto
SecurityEvent | where EventID == 4624 
```

`search` `union` Bir tabloda veya hatta birden çok tabloda birden çok sütunda arama yapmanıza olanak sağlayan veya ile başlayan sorgular. Aşağıdaki örneklerde, _bellek_terimini aramak için birden çok yöntem gösterilmektedir:

```Kusto
search "Memory"
search * | where == "Memory"
search ObjectName: "Memory"
search ObjectName == "Memory"
union * | where ObjectName == "Memory"
```

`search`Veri keşfi `union` sırasında, tüm veri modeli üzerinde terimler arama sırasında yararlı olsa da, birden çok tablo arasında taramaları gerektiğinden tablo kullanmaktan daha az verimlidir. Uyarı kurallarındaki sorgular düzenli aralıklarla çalıştırıldıklarından, bu, uyarıya gecikme süresinin aşırı yüküyle sonuçlanmasından kaynaklanabilir. Bu ek yük nedeniyle, Azure 'daki günlük uyarısı kuralları sorguları her zaman bir açık kapsam tanımlamak için her zaman bir tablo ile başlamalı ve bu da hem sorgu performansını hem de sonuçların uygunluğunu geliştirir.

## <a name="unsupported-queries"></a>Desteklenmeyen sorgular
11 Ocak 2019 ' den itibaren, veya işleçlerini kullanan günlük uyarı kuralları oluşturma veya değiştirme, `search` `union` Azure Portal sürümünde desteklenmeyecektir. Bu işleçler bir uyarı kuralında kullanıldığında bir hata mesajı döndürülür. Log Analytics API 'siyle oluşturulan ve düzenlenen mevcut uyarı kuralları ve uyarı kuralları bu değişiklikten etkilenmez. Yine de bu tür sorgular kullanan tüm uyarı kurallarını değiştirmeyi göz önünde bulundurmanız gerekir.  

Çapraz kaynak [sorgularını](../log-query/cross-workspace-query.md) kullanan günlük uyarı kuralları `union` , sorgu kapsamını belirli kaynaklarla sınırlayan, çapraz kaynak sorgularının kullanıldığı tarihten itibaren bu değişiklikten etkilenmez. Bu eşdeğer değildir ve `union *` kullanılamaz.  Aşağıdaki örnek, bir günlük uyarı kuralında geçerli olacaktır:

```Kusto
union 
app('Contoso-app1').requests, 
app('Contoso-app2').requests, 
workspace('Contoso-workspace1').Perf 
```

>[!NOTE]
>Log uyarılarındaki [çapraz kaynak sorgusu](../log-query/cross-workspace-query.md) , yeni [SCHEDULEDQUERYRULES API](/rest/api/monitor/scheduledqueryrules)'sinde desteklenir. Azure Izleyici, [eski günlük uyarıları API](alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api)'sinden geçiş yapmadığınız takdirde, varsayılan olarak, Azure Portal ' dan yeni günlük uyarı kuralları oluşturmak için [eskı Log Analytics uyarı API](api-alerts.md) 'sini kullanır. Anahtar sonrasında, yeni API Azure portal yeni uyarı kuralları için varsayılan olur ve çapraz kaynak sorgu günlüğü uyarı kuralları oluşturmanıza olanak sağlar. [Scheduledqueryrules API 'si Için ARM şablonunu](alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) kullanarak anahtarı yapmadan [çapraz kaynak sorgu](../log-query/cross-workspace-query.md) günlüğü uyarı kuralları oluşturabilirsiniz, ancak bu uyarı kuralı Azure Portal değil, [scheduledqueryrules API 'si](/rest/api/monitor/scheduledqueryrules) ile yönetilebilir.

## <a name="examples"></a>Örnekler
Aşağıdaki örnekler, ve kullanan günlük sorgularını içerir `search` `union` ve bu sorguları uyarı kurallarıyla kullanılmak üzere değiştirmek için kullanabileceğiniz adımları sağlar.

### <a name="example-1"></a>Örnek 1
Kullanarak performans bilgilerini alan aşağıdaki sorguyu kullanarak bir günlük uyarı kuralı oluşturmak istersiniz `search` : 

``` Kusto
search * | where Type == 'Perf' and CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```
  

Bu sorguyu değiştirmek için, özelliklerin ait olduğu tabloyu tanımlamak üzere aşağıdaki sorguyu kullanarak başlayın:

``` Kusto
search * | where CounterName == '% Free Space'
| summarize by $table
```
 

Bu sorgunun sonucu, _CounterName_ özelliğinin _perf_ tablosundan geldiğini gösterir. 

Bu sonucu, uyarı kuralı için kullanacağınız aşağıdaki sorguyu oluşturmak için kullanabilirsiniz:

``` Kusto
Perf 
| where CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```


### <a name="example-2"></a>Örnek 2
Kullanarak performans bilgilerini alan aşağıdaki sorguyu kullanarak bir günlük uyarı kuralı oluşturmak istersiniz `search` : 

``` Kusto
search ObjectName =="Memory" and CounterName=="% Committed Bytes In Use"  
| summarize Avg_Memory_Usage =avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
  

Bu sorguyu değiştirmek için, özelliklerin ait olduğu tabloyu tanımlamak üzere aşağıdaki sorguyu kullanarak başlayın:

``` Kusto
search ObjectName=="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize by $table 
```
 

Bu sorgunun sonucu, _ObjectName_ ve _CounterName_ özelliğinin _perf_ tablosundan geldiğini gösterir. 

Bu sonucu, uyarı kuralı için kullanacağınız aşağıdaki sorguyu oluşturmak için kullanabilirsiniz:

``` Kusto
Perf 
| where ObjectName =="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize Avg_Memory_Usage=avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
 

### <a name="example-3"></a>Örnek 3

`search`Performans bilgilerini almak için hem hem de kullanan aşağıdaki sorguyu kullanarak bir günlük uyarı kuralı oluşturmak istiyorsunuz `union` : 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| where Computer !in ((union * | where CounterName == "% Processor Utility" | summarize by Computer))
| summarize Avg_Idle_Time = avg(CounterValue) by Computer|  count  
```
 

Bu sorguyu değiştirmek için, sorgunun ilk bölümündeki özelliklerin ait olduğu tabloyu tanımlamak üzere aşağıdaki sorguyu kullanarak başlayın: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| summarize by $table 
```

Bu sorgunun sonucu, tüm bu özelliklerin _perf_ tablosundan geldiğini gösterir. 

Artık `union` `withsource` her satıra katkıda bulunan kaynak tabloyu belirlemek için komutuyla komutunu kullanın.

``` Kusto
union withsource=table * | where CounterName == "% Processor Utility" 
| summarize by table 
```
 

Bu sorgunun sonucu, bu özelliklerin _performans_ tablosundan da geldiğini gösterir. 

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
İki sorgunun sonucunu birleştiren aşağıdaki sorguyu kullanarak bir günlük uyarı kuralı oluşturmak istersiniz `search` :

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
 

Sorguyu değiştirmek için, birleştirmenin sol tarafındaki özellikleri içeren tabloyu tanımlamak üzere aşağıdaki sorguyu kullanarak başlayın: 

``` Kusto
search Type == 'SecurityEvent' and EventID == '4625' 
| summarize by $table 
```
 

Sonuç, birleştirmenin sol tarafındaki özelliklerin _Securityevent_ tablosuna ait olduğunu gösterir. 

Şimdi, birleştirmenin sağ tarafındaki özellikleri içeren tabloyu belirlemek için aşağıdaki sorguyu kullanın: 

 
``` Kusto
search in (Heartbeat) OSType == 'Windows' 
| summarize by $table 
```

 
Sonuç, birleştirmenin sağ tarafındaki özelliklerin sinyal tablosuna ait olduğunu gösterir. 

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
- Azure Izleyici 'de [günlük uyarıları](alerts-log.md) hakkında bilgi edinin.
- [Günlük sorguları](../log-query/log-query-overview.md)hakkında bilgi edinin.
