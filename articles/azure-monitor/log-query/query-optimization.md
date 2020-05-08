---
title: Azure Izleyici 'de günlük sorgularını iyileştirme
description: Azure Izleyici 'de günlük sorgularını iyileştirmek için en iyi uygulamalar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/30/2019
ms.openlocfilehash: 9ae0aec6b87a746ed1f141dcf98f599acd20ab3a
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82864258"
---
# <a name="optimize-log-queries-in-azure-monitor"></a>Azure Izleyici 'de günlük sorgularını iyileştirme
Azure Izleyici günlükleri günlük verilerini depolamak ve bu verileri çözümlemek için sorguları çalıştırmak üzere [azure Veri Gezgini (ADX)](/azure/data-explorer/) kullanır. Sizin için ADX kümelerini oluşturur, yönetir ve korur ve bunları günlük Analizi iş yükünüz için en iyi duruma getirir. Bir sorgu çalıştırdığınızda, en iyi duruma getirilir ve çalışma alanı verilerini depolayan uygun ADX kümesine yönlendirilir. Hem Azure Izleyici günlükleri hem de Azure Veri Gezgini birçok otomatik sorgu iyileştirme mekanizması kullanır. Otomatik iyileştirmeler önemli ölçüde artırma sağlarken, bu durumlar bazı durumlarda sorgu performansınızı ciddi ölçüde İyileştirebileceğiniz bir durumlardır. Bu makalede, performans konuları ve bunları gidermeye yönelik çeşitli teknikler açıklanmaktadır.

Çoğu teknikte doğrudan Azure Veri Gezgini ve Azure Izleyici günlüklerinde çalıştırılan sorgularda yaygın olarak, burada ele alınan birkaç benzersiz Azure Izleyici günlüğü konuları bulunur. Daha fazla Azure Veri Gezgini iyileştirme ipucu için bkz. [sorgu en iyi uygulamaları](/azure/kusto/query/best-practices).

İyileştirilmiş sorgular:

- Daha hızlı çalıştırın, sorgu yürütmenin genel süresini azaltın.
- Daha az kısıtlanacak veya reddedilme şansı vardır.

Panolar, uyarılar, Logic Apps ve Power BI gibi yinelenen kira ve bursty kullanımı için kullanılan sorgulara özellikle dikkat etmeniz gerekir. Bu durumlarda, verimsiz bir sorgunun etkisi önemli değildir.

## <a name="query-performance-pane"></a>Sorgu performansı bölmesi
Log Analytics bir sorgu çalıştırdıktan sonra sorgu sonuçlarının üzerindeki aşağı oka tıklayarak sorgu için çeşitli performans göstergelerinin sonuçlarını gösteren sorgu performansı bölmesini görüntüleyin. Bu performans göstergeleri, her biri aşağıdaki bölümde açıklanmıştır.

![Sorgu performansı bölmesi](media/query-optimization/query-performance-pane.png)


## <a name="query-performance-indicators"></a>Sorgu performans göstergeleri

Aşağıdaki sorgu performans göstergeleri, yürütülen her sorgu için kullanılabilir:

- [Toplam CPU](#total-cpu): tüm işlem düğümleri genelinde sorguyu işlemek için kullanılan genel işlem. Bilgi işlem, ayrıştırma ve veri getirme için kullanılan süreyi temsil eder. 

- [İşlenen sorgu için kullanılan veriler](#data-used-for-processed-query): sorguyu işlemek için erişilen genel veriler. Hedef tablonun boyutuyla, kullanılan zaman aralığı, uygulanan filtrelerin ve başvurulan sütun sayısının etkilenerek etkilenir.

- [İşlenen sorgunun zaman aralığı](#time-span-of-the-processed-query): sorguyu işlemek için erişilen en yeni ve en eski veriler arasındaki boşluk. Sorgu için belirtilen açık zaman aralığına göre etkilendi.

- [İşlenen verilerin yaşı](#age-of-processed-data): Şu anda ve sorguyu işlemek için erişilen en eski veriler arasındaki boşluk. Veri getirme verimliliğini oldukça etkiler.

- [Çalışma alanı sayısı](#number-of-workspaces): örtük veya açık seçim nedeniyle sorgu işlemi sırasında kaç çalışma alanına erişildiği.

- [Bölge sayısı](#number-of-regions): örtük veya açık çalışma alanı seçimine bağlı olarak sorgu işleme sırasında kaç bölgeye erişildiğini. Çok bölgeli sorgular çok daha az verimlidir ve performans göstergeleri kısmi kapsam sunar.

- [Paralellik](#parallelism): sistemin birden çok düğümde bu sorguyu ne kadar yürütebileceğini belirtir. Yalnızca yüksek CPU tüketimi olan sorgularla ilgilidir. Belirli işlevlerin ve işleçlerin kullanımından etkilendi.


## <a name="total-cpu"></a>Toplam CPU
Tüm sorgu işleme düğümlerinde bu sorguyu işlemek için yatırılan gerçek işlem CPU 'SU. Çoğu sorgu çok sayıda düğüm üzerinde yürütüldüğü için bu, genellikle sorgunun yürütülmesi için geçen süreden daha büyük olur. 

Sorgu işleme süresi şu saatte harcanan:
- Veri alma – eski verilerin alınması, son verilerin alınmından daha fazla zaman harcar.
- Veri işleme – verilerin mantığı ve değerlendirmesi. 

Sorgu işleme düğümlerinde harcanan süre dışında, Azure Izleyici günlükleri tarafından şu şekilde harcayacak ek zaman vardır: kullanıcının kimliğini doğrular ve bu verilere erişim izni verildiğini doğrulayın, veri deposunu bulun, sorguyu ayrıştırır ve sorgu işleme düğümlerini ayırır. Bu zaman sorguya toplam CPU süresi dahil değildir.

### <a name="early-filtering-of-records-prior-of-using-high-cpu-functions"></a>Yüksek CPU işlevleri kullanılmadan önce kayıtların erken filtrelenmesi

Sorgu komutlarının ve işlevlerinin bazıları CPU tüketimine ağır. Bu, özellikle JSON ve XML ayrıştırma veya karmaşık normal ifadeleri ayıklama komutları için geçerlidir. Bu tür ayrıştırma, açıkça [parse_json ()](/azure/kusto/query/parsejsonfunction) veya [parse_xml ()](/azure/kusto/query/parse-xmlfunction) işlevleri aracılığıyla veya dinamik sütunlara başvurulurken örtülü olarak gerçekleşebilir.

Bu işlevler, işlenen satır sayısıyla orantılı olarak CPU kullanır. En verimli iyileştirme, CPU yoğun işlevi yürütülmeden önce mümkün olduğunca çok kayıt filtreleyebileceği şekilde sorgu içinde nerede koşul eklemektir.

Örneğin, aşağıdaki sorgular tam olarak aynı sonucu üretir ancak ikinci tane, ayrıştırma işleminden [önce koşul çok](/azure/kusto/query/whereoperator) sayıda kaydı dışladığı için en verimli şekilde belirlenir:

```Kusto
//less efficient
SecurityEvent
| extend Details = parse_xml(EventData)
| extend FilePath = tostring(Details.UserData.RuleAndFileData.FilePath)
| extend FileHash = tostring(Details.UserData.RuleAndFileData.FileHash)
| summarize count() by FileHash, FilePath
| where FileHash != "" and FilePath !startswith "%SYSTEM32"  // Problem: irrelevant results are filtered after all processing and parsing is done
```
```Kusto
//more efficient
SecurityEvent
| where EventID == 8002 //Only this event have FileHash
| where EventData !has "%SYSTEM32" //Early removal of unwanted records
| extend Details = parse_xml(EventData)
| extend FilePath = tostring(Details.UserData.RuleAndFileData.FilePath)
| extend FileHash = tostring(Details.UserData.RuleAndFileData.FileHash)
| summarize count() by FileHash, FilePath
| where FileHash != "" // No need to filter out %SYSTEM32 here as it was removed before
```

### <a name="avoid-using-evaluated-where-clauses"></a>Değerlendirilen WHERE yan tümceleri kullanmaktan kaçının

Veri kümesinde fiziksel olarak bulunan sütunlarda değil, değerlendirilen bir sütunda [WHERE](/azure/kusto/query/whereoperator) yan tümceleri içeren sorgular, verimliliği kaybeder. Değerlendirilen sütunlarda filtreleme, büyük veri kümeleri işlendiği sırada bazı sistem iyileştirmelerinin yapılmasını önler.
Örneğin, aşağıdaki sorgular tam olarak aynı sonucu üretir ancak ikinci tane, [WHERE](/azure/kusto/query/whereoperator) koşulunun yerleşik sütuna başvurduğu şekilde daha verimlidir

```Kusto
//less efficient
Heartbeat 
| extend IPRegion = iif(RemoteIPLongitude  < -94,"WestCoast","EastCoast")
| where IPRegion == "WestCoast"
| summarize count() by Computer
```
```Kusto
//more efficient
Heartbeat 
| where RemoteIPLongitude  < -94
| extend IPRegion = iif(RemoteIPLongitude  < -94,"WestCoast","EastCoast")
| summarize count() by Computer
```

### <a name="use-effective-aggregation-commands-and-dimensions-in-summarize-and-join"></a>Özetleme ve birleştirme bölümünde etkili toplama komutlarını ve boyutlarını kullanma

[Max ()](/azure/kusto/query/max-aggfunction), [Sum ()](/azure/kusto/query/sum-aggfunction), [Count ()](/azure/kusto/query/count-aggfunction)ve [AVG ()](/azure/kusto/query/avg-aggfunction) gibi bazı toplama komutlarının mantığı nedeniyle düşük CPU etkisi olsa da, diğerleri daha karmaşıktır ve verimli bir şekilde yürütülmesine izin veren buluşsal yöntemler ve tahminler içerir. Örneğin, [DCount ()](/azure/kusto/query/dcount-aggfunction) , her bir değeri gerçekten saymadan, büyük veri kümelerinin ayrı sayısına kapanış tahmini sağlamak Için HyperLogLog algoritmasını kullanır; yüzdebirlik işlevleri, en yakın derecelendirme yüzdebirlik algoritmasını kullanarak benzer bir şekilde yapılır. Birçok komut, etkilerini azaltmak için isteğe bağlı parametreler içerir. Örneğin, [makeset ()](/azure/kusto/query/makeset-aggfunction) IŞLEVININ, CPU ve belleği önemli ölçüde etkileyen en büyük küme boyutunu tanımlamak için isteğe bağlı bir parametresi vardır.

[JOIN](/azure/kusto/query/joinoperator?pivots=azuremonitor) ve [özetleme](/azure/kusto/query/summarizeoperator) komutları, büyük bir VERI kümesini işlerken yüksek CPU kullanımına neden olabilir. Karmaşıklığı, özetleme olarak ya da JOIN özniteliği olarak kullanılan `by` sütunların *kardinalite*olarak adlandırılan olası değer sayısıyla doğrudan ilgilidir. Katılmayı ve özetlemeyi açıklama ve iyileştirme için bkz. belge makaleleri ve iyileştirme ipuçları.

Örneğin, **CounterPath** her zaman **CounterName** ve **ObjectName**'e eşlenmiş olduğundan aşağıdaki sorgular tam olarak aynı sonucu üretir. İkinci bir, toplama boyutu daha küçük olduğu için daha verimlidir:

```Kusto
//less efficient
Perf
| summarize avg(CounterValue) 
by CounterName, CounterPath, ObjectName
```
```Kusto
//make the group expression more compact improve the performance
Perf
| summarize avg(CounterValue), any(CounterName), any(ObjectName) 
by CounterPath
```

CPU tüketimi Ayrıca koşullarda veya yoğun bilgi işlem gerektiren genişletilmiş sütunlarda da etkilenebilir. [Eşittir = =](/azure/kusto/query/datatypes-string-operators) ve [StartsWith](/azure/kusto/query/datatypes-string-operators) gibi tüm önemsiz dize karşılaştırmaları, Gelişmiş metin eşleştirmelerinde daha fazla etkiye sahip olsa da kabaca aynı CPU etkisi vardır. Özellikle, [sahip](/azure/kusto/query/datatypes-string-operators) operatörü [Contains](/azure/kusto/query/datatypes-string-operators) işlecinin daha etkilidir. Dize işleme teknikleri nedeniyle, kısa dizelerdeki dört karakterden daha uzun dizeler aramak daha etkilidir.

Örneğin, aşağıdaki sorgular bilgisayar adlandırma ilkesine bağlı olarak benzer sonuçlar üretir ancak ikincisi daha etkilidir:

```Kusto
//less efficient – due to filter based on contains
Heartbeat
| where Computer contains "Production" 
| summarize count() by ComputerIP 
```
```Kusto
//less efficient – due to filter based on extend
Heartbeat
| extend MyComputer = Computer
| where MyComputer startswith "Production" 
| summarize count() by ComputerIP 
```
```Kusto
//more efficient
Heartbeat
| where Computer startswith "Production" 
| summarize count() by ComputerIP 
```

> [!NOTE]
> Bu gösterge yalnızca en hızlı kümeden CPU gösterir. Çok bölgeli sorguda bölge yalnızca birini temsil eder. Çoklu çalışma alanı sorgusunda, tüm çalışma alanlarını içermeyebilir.

### <a name="avoid-full-xml-and-json-parsing-when-string-parsing-works"></a>Dize ayrıştırması çalışırken tam XML ve JSON ayrıştırmayı önleyin
Bir XML veya JSON nesnesinin tam ayrıştırması, yüksek CPU ve bellek kaynakları tüketebilir. Çoğu durumda, yalnızca bir veya iki parametre gerektiğinde ve XML veya JSON nesneleri basit olduğunda, [ayrıştırma işlecini](/azure/kusto/query/parseoperator) veya diğer [metin ayrıştırma tekniklerini](/azure/azure-monitor/log-query/parse-text)kullanarak bunları dizeler olarak ayrıştırmanız daha kolay olur. XML veya JSON nesnesindeki kayıt sayısı arttıkça performans artışı daha önemli olacaktır. Kayıt sayısı on milyona ulaştığında, bu önemlidir.

Örneğin, aşağıdaki sorgu tam XML ayrıştırma gerçekleştirmeksizin yukarıdaki sorgularla tam olarak aynı sonuçları döndürür. XML dosya yapısında, FilePath öğesinin dosya karmasından sonra geldiği ve hiçbirinin özniteliklere sahip olmadığı gibi bazı varsayımlar yaptığı unutulmamalıdır. 

```Kusto
//even more efficient
SecurityEvent
| where EventID == 8002 //Only this event have FileHash
| where EventData !has "%SYSTEM32" //Early removal of unwanted records
| parse EventData with * "<FilePath>" FilePath "</FilePath>" * "<FileHash>" FileHash "</FileHash>" *
| summarize count() by FileHash, FilePath
| where FileHash != "" // No need to filter out %SYSTEM32 here as it was removed before
```


## <a name="data-used-for-processed-query"></a>İşlenen sorgu için kullanılan veriler

Sorgu işlenirken kritik bir faktör, taranan ve sorgu işleme için kullanılan veri birimidir. Azure Veri Gezgini, diğer veri platformları ile karşılaştırıldığında veri hacmini önemli ölçüde azaltan agresif iyileştirmeler kullanır. Hala, sorguda kullanılan veri birimini etkileyebilecek kritik etmenler vardır.

Azure Izleyici günlüklerinde **TimeGenerated** sütunu, verileri dizine almanın bir yolu olarak kullanılır. **TimeGenerated** değerlerinin mümkün olduğunca dar olarak sınırlanması, işlenmesi gereken veri miktarını önemli ölçüde sınırlandırarak performansı sorgulamak için önemli bir geliştirme sağlar.

### <a name="avoid-unnecessary-use-of-search-and-union-operators"></a>Arama ve birleşim işleçlerinin gereksiz kullanımını önleyin

İşlem olan verileri artıran bir faktör çok sayıda tablo kullanmaktır. Bu genellikle ve `union *` komutlarının `search *` kullanıldığı zaman gerçekleşir. Bu komutlar, sistem çalışma alanındaki tüm tablolardaki verileri değerlendirmeye ve taramaya zorlar. Bazı durumlarda, çalışma alanında yüzlerce tablo olabilir. "Arama *" veya herhangi bir aramayı belirli bir tabloya kapsama almadan mümkün olduğunca kaçınmaya çalışın.

Örneğin, aşağıdaki sorgular tam olarak aynı sonucu üretir, ancak sonuncusu en verimli bir şekilde yapılır:

```Kusto
// This version scans all tables though only Perf has this kind of data
search "Processor Time" 
| summarize count(), avg(CounterValue)  by Computer
```
```Kusto
// This version scans all strings in Perf tables – much more efficient
Perf
| search "Processor Time" 
| summarize count(), avg(CounterValue)  by Computer
```
```Kusto
// This is the most efficient version 
Perf 
| where CounterName == "% Processor Time"  
| summarize count(), avg(CounterValue)  by Computer
```

### <a name="add-early-filters-to-the-query"></a>Sorguya erken filtreler ekleme

Veri birimini azaltmaya yönelik başka bir yöntem ise, koşullarda sorgunun başlarında [yer](/azure/kusto/query/whereoperator) almak için kullanılır. Azure Veri Gezgini platformu, belirli bir koşul için ilgili verileri hangi bölümlerin içerdiğini öğrenmenizi sağlayan bir önbellek içerir. Örneğin, bir sorgu içeriyorsa `where EventID == 4624` , sorguyu yalnızca eşleşen olaylara sahip bölümleri işleyen düğümlere dağıtır.

Aşağıdaki örnek sorgular tam olarak aynı sonucu üretir ancak ikincisi daha etkilidir:

```Kusto
//less efficient
SecurityEvent
| summarize LoginSessions = dcount(LogonGuid) by Account
```
```Kusto
//more efficient
SecurityEvent
| where EventID == 4624 //Logon GUID is relevant only for logon event
| summarize LoginSessions = dcount(LogonGuid) by Account
```

### <a name="reduce-the-number-of-columns-that-is-retrieved"></a>Alınan sütun sayısını azaltın

Azure Veri Gezgini bir sütunlu veri deposu olduğundan, her sütunun alınması diğerlerinden bağımsızdır. Alınan sütun sayısı, genel veri hacmini doğrudan etkiler. Yalnızca sonuçları [özetleyerek](/azure/kusto/query/summarizeoperator) veya belirli sütunları [yansıtırken](/azure/kusto/query/projectoperator) gereken çıktıya sütunları dahil etmelisiniz. Azure Veri Gezgini alınan sütun sayısını azaltmak için çeşitli iyileştirmeler içerir. Bir sütunun gerekli olmadığını belirlerse, örneğin, [özetleme](/azure/kusto/query/summarizeoperator) komutunda başvurulmamışsa, bunu alamaz.

Örneğin, ikinci sorgu üç kat daha fazla veri işleyebilir, çünkü bir sütun değil, üç kez getirmek gerekir:

```Kusto
//Less columns --> Less data
SecurityEvent
| summarize count() by Computer  
```
```Kusto
//More columns --> More data
SecurityEvent
| summarize count(), dcount(EventID), avg(Level) by Computer  
```

## <a name="time-span-of-the-processed-query"></a>İşlenen sorgunun zaman aralığı

Azure Izleyici günlüklerindeki tüm Günlükler, **TimeGenerated** sütununa göre bölümlendirilir. Erişilen bölüm sayısı doğrudan zaman dilimi ile ilgilidir. Zaman aralığını azaltmak, istem sorgusu yürütmeyi en verimli yoludur.

Zaman aralığı, [Azure izleyici Log Analytics günlük sorgusu kapsamı ve zaman aralığı](scope.md#time-range)bölümünde açıklandığı gibi Log Analytics ekranındaki zaman aralığı Seçicisi kullanılarak ayarlanabilir. Bu, seçili zaman aralığı, sorgu meta verileri kullanılarak arka uca geçirildiği için önerilen yöntemdir. 

Alternatif bir yöntem, sorguda her **zaman** bir [WHERE](/azure/kusto/query/whereoperator) koşulunu açıkça içermelidir. Bu yöntemi, sorgu farklı bir arabirimden kullanıldığında bile zaman aralığının düzeltildiğinden emin olmak için kullanmanız gerekir.
Sorgunun tüm bölümlerinin **TimeGenerated** filtrelerine sahip olduğundan emin olmanız gerekir. Bir sorguda çeşitli tablolardan veya aynı tablodan veri getirilirken alt sorgular olduğunda, her birinin kendi [WHERE](/azure/kusto/query/whereoperator) koşulunu içermesi gerekir.

### <a name="make-sure-all-sub-queries-have-timegenerated-filter"></a>Tüm alt sorguların TimeGenerated filtresi olduğundan emin olun

Örneğin, aşağıdaki sorguda, **performans** tablosu yalnızca son gün için taranırken, **sinyal** tablosu, en fazla iki yıla kadar olan tüm geçmişi için taranacaktır:

```Kusto
Perf
| where TimeGenerated > ago(1d)
| summarize avg(CounterValue) by Computer, CounterName
| join kind=leftouter (
    Heartbeat
    //No time span filter in this part of the query
    | summarize IPs = makeset(ComputerIP, 10) by  Computer
) on Computer
```

Bir hata oluşması durumunda, en son oluşumu bulmak için [arg_max ()](/azure/kusto/query/arg-max-aggfunction) kullanıldığında oluşan yaygın bir durumdur. Örneğin:

```Kusto
Perf
| where TimeGenerated > ago(1d)
| summarize avg(CounterValue) by Computer, CounterName
| join kind=leftouter (
    Heartbeat
    //No time span filter in this part of the query
    | summarize arg_max(TimeGenerated, *), min(TimeGenerated)   
by Computer
) on Computer
```

Bu, iç sorguya bir zaman filtresi eklenerek kolayca düzeltilebilir:

```Kusto
Perf
| where TimeGenerated > ago(1d)
| summarize avg(CounterValue) by Computer, CounterName
| join kind=leftouter (
    Heartbeat
    | where TimeGenerated > ago(1d) //filter for this part
    | summarize arg_max(TimeGenerated, *), min(TimeGenerated)   
by Computer
) on Computer
```

Bu hata için başka bir örnek, birkaç tablo üzerinde bir [birleşimden](/azure/kusto/query/unionoperator?pivots=azuremonitor) hemen sonra zaman kapsamı filtrelemesinin gerçekleştirilemedir. Birleşim gerçekleştirirken, her alt sorgu kapsamlı olmalıdır. Kapsam tutarlılığı güvence altına almak için [Let](/azure/kusto/query/letstatement) deyiminizi kullanabilirsiniz.

Örneğin, aşağıdaki sorgu yalnızca son 1 güne değil, *sinyal* ve *performans* tablolarındaki tüm verileri tarayacaktır:

```Kusto
Heartbeat 
| summarize arg_min(TimeGenerated,*) by Computer
| union (
    Perf 
    | summarize arg_min(TimeGenerated,*) by Computer) 
| where TimeGenerated > ago(1d)
| summarize min(TimeGenerated) by Computer
```

Bu sorgu şu şekilde düzeltilmelidir:

```Kusto
let MinTime = ago(1d);
Heartbeat 
| where TimeGenerated > MinTime
| summarize arg_min(TimeGenerated,*) by Computer
| union (
    Perf 
    | where TimeGenerated > MinTime
    | summarize arg_min(TimeGenerated,*) by Computer) 
| summarize min(TimeGenerated) by Computer
```

### <a name="time-span-measurement-limitations"></a>Zaman aralığı ölçüm sınırlamaları

Ölçüm, belirtilen gerçek süreden her zaman daha büyüktür. Örneğin, sorgudaki filtre 7 gün ise, sistem 7,5 veya 8,1 gün tarama alabilir. Bunun nedeni, sistemin verileri değişken boyuttaki parçalara bölümlendirmesi nedeniyle oluşur. Tüm ilgili kayıtların tarandığından emin olmak için, birkaç saati ve hatta bir günden daha fazlasını kapsayan tüm bölümü tarar.

Sistemin zaman aralığının doğru bir ölçümünü sağlayamadığının birkaç durumu vardır. Bu durum, sorgunun bir günden veya çok çalışma alanı sorgularından daha az olduğu durumlarda oluşur.


> [!IMPORTANT]
> Bu gösterge yalnızca anında kümede işlenen verileri gösterir. Çok bölgeli sorguda bölge yalnızca birini temsil eder. Çoklu çalışma alanı sorgusunda, tüm çalışma alanlarını içermeyebilir.

## <a name="age-of-processed-data"></a>İşlenen verilerin yaşı
Azure Veri Gezgini, çeşitli depolama katmanlarını kullanır: bellek içi, yerel SSD diskleri ve çok daha yavaş Azure Blob 'Ları. Veriler arttıkça, daha yüksek gecikme süresine sahip daha fazla performans ve sorgu süresi ve CPU 'YU azaltan bir daha iyi hale gelir. Verilerin kendisi dışında, sistem meta veriler için de önbelleğe sahiptir. Veriler daha eski olduğunda meta verileri önbellekte olacaktır.

Bazı sorgular eski verilerin kullanımını gerektirirken, eski verilerin yanlışlıkla kullanıldığı durumlar vardır. Bu durum, sorguları meta verilerinde zaman aralığı sağlamaktan yürütülene ve tüm tablo başvuruları, **TimeGenerated** sütununda filtre içermez. Bu durumlarda sistem, bu tabloda depolanan tüm verileri tarar. Veri saklama süresi uzunsa, uzun zaman aralıklarını ve bu nedenle veri saklama süresi kadar eski olan verileri kapsayacaktır.

Bu gibi durumlarda, örneğin:

- Sınırlı olmayan bir alt sorgu ile Log Analytics zaman aralığı ayarlamamıyor. Yukarıdaki örneğe bakın.
- Zaman aralığı isteğe bağlı parametreleri olmadan API 'YI kullanma.
- Power BI Bağlayıcısı gibi zaman aralığına zormeyen bir istemciyi kullanma.

Bu durumda da alakalı olduklarından, bazı örnekler bölümündeki örneklere ve notlara göz atın.

## <a name="number-of-regions"></a>Bölge sayısı
Farklı bölgelerde tek bir sorgunun yürütülebileceği birkaç durum vardır:

- Birden çok çalışma alanı açık olarak listeleniyorsa ve bunlar farklı bölgelerde bulunuyorsa.
- Kaynak kapsamlı bir sorgu veri getirilirken ve veriler farklı bölgelerde bulunan birden çok çalışma alanında depolanıyorsa.

Çapraz bölge sorgusu yürütme, sistemin son kullanılan büyük öbeklere, genellikle sorgu nihai sonuçlarından çok daha büyük parçalar halinde serileştirmek ve aktarım yapılmasını gerektirir. Ayrıca sistemin iyileştirmeler, buluşsal yöntemler gerçekleştirme ve önbellekleri kullanma yeteneğini de kısıtlar.
Tüm bu bölgeleri taramak için gerçek bir neden yoksa, kapsamı daha az bölge içerecek şekilde ayarlamanız gerekir. Kaynak kapsamı simge durumuna küçültülmüş, ancak hala çok sayıda bölge kullanılıyorsa, yanlış yapılandırma nedeniyle bu durum oluşabilir. Örneğin, denetim günlükleri ve Tanılama ayarları farklı bölgelerdeki farklı çalışma alanlarına gönderilir veya birden çok Tanılama ayarları yapılandırması vardır. 

> [!IMPORTANT]
> Bir sorgu çeşitli bölgelerde çalıştırıldığında, CPU ve veri ölçümleri doğru olmayacaktır ve yalnızca bölgelerden birindeki ölçüyü temsil eder.

## <a name="number-of-workspaces"></a>Çalışma alanı sayısı
Çalışma alanları, günlük verilerini ayırmak ve yönetmek için kullanılan mantıksal kapsayıcılardır. Arka uç, Seçili bölge içindeki fiziksel kümelerdeki çalışma alanı yerleştirmelerinin en iyi duruma getirir.

Birden çok çalışma alanının kullanımı şu sonucu verebilir: 

- Burada birkaç çalışma alanı açık olarak listelenir.
- Kaynak kapsamlı bir sorgu veri getirilirken ve veriler birden fazla çalışma alanında depolandığında.
 
Sorguların bölgeler arası ve çapraz küme yürütmesi, sistemin son görüntülenen büyük öbeklere, genellikle sorgu nihai sonuçlarından çok daha büyük parçalara serileştirmesinin ve aktarılmasını gerektirir. Ayrıca sistem, iyileştirmeler, buluşsal yöntemler ve önbellekler gerçekleştirme becerisini de kısıtlar.

> [!IMPORTANT]
> Bazı çok çalışma alanı senaryolarında CPU ve veri ölçümleri doğru olmayacaktır ve yalnızca birkaç çalışma alanında ölçüyü temsil eder.

## <a name="parallelism"></a>Paralellik
Azure Izleyici günlükleri sorguları çalıştırmak için büyük Azure Veri Gezgini kümeleri kullanıyor ve bu kümeler ölçekli, büyük olasılıkla düzinelerce işlem düğümlerine göre farklılık gösterir. Sistem, çalışma alanı yerleştirme mantığı ve kapasitesine göre kümeleri otomatik olarak ölçeklendirir.

Bir sorguyu verimli bir şekilde yürütmek için bölümlenmiş ve işleme için gereken verilere göre işlem düğümlerine dağıtılır. Sistemin bunu verimli bir şekilde yapamadığı bazı durumlar vardır. Bu, sorgunun uzun süreli olmasına neden olabilir. 

Paralellik 'i azaltan sorgu davranışları şunlardır:

- Seri hale getirme [işleci](/azure/kusto/query/serializeoperator), [Next ()](/azure/kusto/query/nextfunction), [önceki ()](/azure/kusto/query/prevfunction)ve [Row](/azure/kusto/query/rowcumsumfunction) işlevleri gibi serileştirme ve pencere işlevlerinin kullanımı. Bu durumlarda zaman serisi ve Kullanıcı Analizi işlevleri kullanılabilir. Şu işleçler sorgu sonunda değilse, verimsiz serileştirme de oluşabilir: [Range](/azure/kusto/query/rangeoperator), [Sort](/azure/kusto/query/sortoperator), [Order](/azure/kusto/query/orderoperator), [top](/azure/kusto/query/topoperator), [top-hitters](/azure/kusto/query/tophittersoperator), [GetSchema](/azure/kusto/query/getschemaoperator).
-    [DCount ()](/azure/kusto/query/dcount-aggfunction) toplama işlevinin kullanımı, sistemi farklı değerlerin merkezi kopyasına sahip olacak şekilde zorlar. Verilerin ölçeği yüksek olduğunda, doğruluğu azaltmak için DCount işlevi isteğe bağlı parametrelerini kullanmayı deneyin.
-    Birçok durumda, [JOIN](/azure/kusto/query/joinoperator?pivots=azuremonitor) işleci genel paralellik düşürür. Performans sorunlu olduğunda bir alternatif olarak karışık katılmayı inceleyin.
-    Kaynak kapsamı sorgularında, ön yürütme RBAC denetimleri çok fazla sayıda RBAC atamasının olduğu durumlarda kalabilir. Bu, daha uzun bir denetim oluşmasına neden olabilir ve bu da daha fazla paralellik sağlar. Örneğin, bir sorgu binlerce kaynağın bulunduğu bir abonelikte yürütülür ve her bir kaynağın abonelik veya kaynak grubunda değil kaynak düzeyinde birçok rol ataması vardır.
-    Bir sorgu küçük veri öbeklerini işlerse, sistem çok sayıda işlem düğümüne yayılmadığından paralelliği düşük olur.



## <a name="next-steps"></a>Sonraki adımlar

- [Kusto sorgu dili Için başvuru belgeleri](/azure/kusto/query/).
