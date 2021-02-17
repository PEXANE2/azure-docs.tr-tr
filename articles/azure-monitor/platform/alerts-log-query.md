---
title: Günlük uyarısı sorgularını iyileştirme | Microsoft Docs
description: Verimli uyarı sorguları yazma önerileri
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 09/22/2020
ms.subservice: alerts
ms.openlocfilehash: e4b0789a02e7bb9a0802c54475056a7dae6bff5b
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100545080"
---
# <a name="optimizing-log-alert-queries"></a>Günlük uyarısı sorgularını iyileştirme
Bu makalede, en iyi performansı elde etmek için [günlük uyarısı](alerts-unified-log.md) sorgularının nasıl yazılacağı ve dönüştürüleceği açıklanır. İyileştirilmiş sorgular, sıklıkla çalışan uyarıları ve uyarı yükünü azaltır.

## <a name="how-to-start-writing-an-alert-log-query"></a>Uyarı günlüğü sorgusu yazmaya başlama

Uyarı sorguları, sorunu belirten [Log Analytics günlük verilerini sorgulamadan](alerts-log.md#create-a-log-alert-rule-with-the-azure-portal) başlar. Neleri keşfedebileceğinizi anlamak için [Uyarı sorgu örnekleri konusunu](../log-query/example-queries.md) kullanabilirsiniz. [Kendi sorgunuzu yazmaya de başlamanızı](../log-query/log-analytics-tutorial.md)sağlayabilirsiniz. 

### <a name="queries-that-indicate-the-issue-and-not-the-alert"></a>Uyarı değil, sorunu belirten sorgular

Uyarı akışı, sorunu belirten sonuçları bir uyarıya dönüştürmek için oluşturulmuştur. Örneğin, şöyle bir sorgu söz konusu olduğunda:

``` Kusto
SecurityEvent
| where EventID == 4624
```

Kullanıcının amacı uyarılardaysa, bu olay türü gerçekleştiğinde, uyarı mantığı `count` sorguya ekler. Çalıştırılacak sorgu şu şekilde olacaktır:

``` Kusto
SecurityEvent
| where EventID == 4624
| count
```

Sorguya uyarı mantığı eklemeniz ve bu durum bile sorunlara neden olabilir. Yukarıdaki örnekte, sorgunuza eklerseniz, `count` Uyarı hizmeti ' nin yapamasından bu yana her zaman değeri 1 ' de olur `count` `count` .

### <a name="avoid-limit-and-take-operators"></a>Kaçınma `limit` ve `take` işleçler

`limit` `take` Sorgular zaman içinde tutarlı olmadığından, ve sorguları içinde kullanmak, gecikme süresini ve uyarıları artırabilir. Bunu yalnızca gerektiğinde kullanmanız tercih edilir.

## <a name="log-query-constraints"></a>Günlük sorgusu kısıtlamaları
[Azure izleyici 'de günlük sorguları](../log-query/log-query-overview.md) bir tablo, [`search`](/azure/kusto/query/searchoperator) ya da [`union`](/azure/kusto/query/unionoperator) işleçle başlar.

Günlük uyarı kuralları sorguları, hem sorgu performansını hem de sonuçların uygunluğunu artıran bir açık kapsam tanımlamak için her zaman bir tabloyla başlamalıdır. Uyarı kurallarındaki sorgular sıklıkla çalışır; bu nedenle, ve ' ı kullanarak `search` `union` birden çok tablo üzerinde tarama gerektirdiği için, uyarıya gecikme süresini fazla yüke neden olabilir. Bu işleçler ayrıca uyarı hizmeti 'nin sorguyu en uygun hale getirme yeteneğini de azaltır.

Veya işleçlerini kullanan günlük uyarı kuralları oluşturma veya değiştirme desteklememiz `search` `union` , çapraz kaynak sorgular için bekleniyor.

Örneğin, aşağıdaki uyarı sorgusu _securityevent_ tablosunun kapsamına alınır ve belırlı olay kimliğini arar. Sorgunun işlemesi gereken tek tablo.

``` Kusto
SecurityEvent
| where EventID == 4624
```

Çapraz kaynak sorguları, [](../log-query/cross-workspace-query.md) `union` sorgu kapsamını belirli kaynaklarla sınırlayan bir türü kullandığından, çapraz kaynak sorgularını kullanan günlük uyarı kuralları bu değişiklikten etkilenmez. Aşağıdaki örnek geçerli günlük uyarı sorgusu olacaktır:

```Kusto
union
app('Contoso-app1').requests,
app('Contoso-app2').requests,
workspace('Contoso-workspace1').Perf 
```

>[!NOTE]
> Yeni [Scheduledqueryrules API](/rest/api/monitor/scheduledqueryrules)'sinde [çapraz kaynak sorguları](../log-query/cross-workspace-query.md) desteklenir. Günlük uyarıları oluşturmak için [eski Log Analytics uyarı API](api-alerts.md) 'sini kullanmaya devam ediyorsanız, [buradan](alerts-log-api-switch.md)geçiş yapmayı öğrenebilirsiniz.

## <a name="examples"></a>Örnekler
Aşağıdaki örnekler, ve kullanan günlük sorgularını içerir `search` `union` ve bu sorguları uyarı kurallarında kullanılmak üzere değiştirmek için kullanabileceğiniz adımları sağlar.

### <a name="example-1"></a>Örnek 1
Kullanarak performans bilgilerini alan aşağıdaki sorguyu kullanarak bir günlük uyarı kuralı oluşturmak istersiniz `search` : 

``` Kusto
search *
| where Type == 'Perf' and CounterName == '% Free Space'
| where CounterValue < 30
```

Bu sorguyu değiştirmek için, özelliklerin ait olduğu tabloyu tanımlamak üzere aşağıdaki sorguyu kullanarak başlayın:

``` Kusto
search *
| where CounterName == '% Free Space'
| summarize by $table
```

Bu sorgunun sonucu, _CounterName_ özelliğinin _perf_ tablosundan geldiğini gösterir.

Bu sonucu, uyarı kuralı için kullanacağınız aşağıdaki sorguyu oluşturmak için kullanabilirsiniz:

``` Kusto
Perf
| where CounterName == '% Free Space'
| where CounterValue < 30
```

### <a name="example-2"></a>Örnek 2
Kullanarak performans bilgilerini alan aşağıdaki sorguyu kullanarak bir günlük uyarı kuralı oluşturmak istersiniz `search` : 

``` Kusto
search ObjectName =="Memory" and CounterName=="% Committed Bytes In Use"
| summarize Avg_Memory_Usage =avg(CounterValue) by Computer
| where Avg_Memory_Usage between(90 .. 95)  
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
```

### <a name="example-3"></a>Örnek 3

`search`Performans bilgilerini almak için hem hem de kullanan aşağıdaki sorguyu kullanarak bir günlük uyarı kuralı oluşturmak istiyorsunuz `union` : 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")
| where Computer !in (
    union *
    | where CounterName == "% Processor Utility"
    | summarize by Computer)
| summarize Avg_Idle_Time = avg(CounterValue) by Computer
```

Bu sorguyu değiştirmek için, sorgunun ilk bölümündeki özelliklerin ait olduğu tabloyu tanımlamak üzere aşağıdaki sorguyu kullanarak başlayın: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")
| summarize by $table
```

Bu sorgunun sonucu, tüm bu özelliklerin _perf_ tablosundan geldiğini gösterir.

Artık `union` `withsource` her satıra katkıda bulunan kaynak tabloyu belirlemek için komutuyla komutunu kullanın.

``` Kusto
union withsource=table *
| where CounterName == "% Processor Utility"
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
) on Hour
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
 
Sonuç, birleştirmenin sağ tarafındaki özelliklerin _sinyal_ tablosuna ait olduğunu gösterir.

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
) on Hour
```

## <a name="next-steps"></a>Sonraki adımlar
- Azure Izleyici 'de [günlük uyarıları](alerts-log.md) hakkında bilgi edinin.
- [Günlük sorguları](../log-query/log-query-overview.md)hakkında bilgi edinin.