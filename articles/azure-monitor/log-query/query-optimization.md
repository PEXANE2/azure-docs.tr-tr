---
title: Azure Monitor'da günlük sorgularını optimize edin
description: Azure Monitor'da günlük sorgularını en iyi duruma duruma duruma en iyi şekilde uygulamak için en iyi uygulamalar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/28/2019
ms.openlocfilehash: c32731ce2de2b0f886a1e21ee8ccad3996e395eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480275"
---
# <a name="optimize-log-queries-in-azure-monitor"></a>Azure Monitor'da günlük sorgularını optimize edin
Azure Monitor Günlükleri, günlük verilerini depolamak ve bu verileri çözümlemek için sorguları çalıştırmak için [Azure Veri Gezgini 'ni (ADX)](/azure/data-explorer/) kullanır. ADX kümelerini sizin için oluşturur, yönetir ve korur ve bunları günlük çözümlemesi iş yükünüz için optimize eder. Bir sorgu çalıştırdığınızda, en iyi duruma getirilmiş ve çalışma alanı verilerini depolayan uygun ADX kümesine yönlendirilir. Hem Azure Monitor Günlükleri hem de Azure Veri Gezgini birçok otomatik sorgu optimizasyonu mekanizması kullanır. Otomatik optimizasyonlar önemli bir artış sağlarken, sorgu performansınızı önemli ölçüde artırabileceğiniz bazı durumlarda bunlardır. Bu makalede, performans konuları ve bunları düzeltmek için çeşitli teknikler açıklanmaktadır.

Tekniklerin çoğu, doğrudan Azure Veri Gezgini ve Azure Monitör Günlükleri'nde çalıştırılan sorgularda yaygındır, ancak burada tartışılan birkaç benzersiz Azure Monitör Günlükleri göz önünde bulundurulur. Daha fazla Azure Veri Gezgini optimizasyonu ipucu için [sorgula en iyi uygulamaları](/azure/kusto/query/best-practices)görün.

En iyi duruma getirilmiş sorgular:

- Daha hızlı çalıştırın, sorgu yürütmesinin toplam süresini azaltın.
- Daraltılmış veya reddedilme şansınız daha azdır.

Panolar, uyarılar, Mantık Uygulamaları ve Power BI gibi tekrarlayan ve patlamalı kullanımiçin kullanılan sorgulara özellikle dikkat etmelisiniz. Bu gibi durumlarda etkisiz bir sorgunun etkisi önemli.

## <a name="query-performance-pane"></a>Performans bölmesine sorgula
Günlük Analizi'nde bir sorgu çalıştırdıktan sonra, sorgu için çeşitli performans göstergelerinin sonuçlarını gösteren sorgu performans bölmesini görüntülemek için sorgu sonuçlarının üzerindeki aşağı ok'u tıklatın. Bu performans göstergelerinin her biri aşağıdaki bölümde açıklanmıştır.

![Performans bölmesine sorgula](media/query-optimization/query-performance-pane.png)


## <a name="query-performance-indicators"></a>Performans göstergelerini sorgula

Yürütülen her sorgu için aşağıdaki sorgu performans göstergeleri kullanılabilir:

- [Toplam Cpu](#total-cpu): Tüm işlem düğümleri arasında sorgu işlemek için kullanılan genel işlem. Bilgi işlem, ayrıştma ve veri alma için kullanılan zamanı temsil eder. 

- [İşlenmiş sorgu için kullanılan veriler](#data-used-for-processed-query): Sorguyu işlemek için erişilen genel veriler. Hedef tablonun boyutundan, kullanılan zaman aralığından, uygulanan filtrelerden ve başvurulan sütun sayısından etkilenir.

- [İşlenen sorgunun zaman aralığı](#time-span-of-the-processed-query): Sorguyu işlemek için erişilen en yeni ve en eski veriler arasındaki boşluk. Sorgu için belirtilen açık zaman aralığından etkilenir.

- [İşlenen verilerin yaşı](#age-of-processed-data): Sorguyu işlemek için erişilen en eski verilerle şimdiki arasındaki boşluk. Veri alma verimliliğini son derece etkiler.

- [Çalışma alanı sayısı](#number-of-workspaces): Örtülü veya açık seçim nedeniyle sorgu işleme sırasında kaç çalışma alanı erişildi.

- Bölge Sayısı : Çalışma [alanlarının](#number-of-regions)örtülü veya açık seçimi nedeniyle sorgu işleme sırasında kaç bölgeye erişildi. Çok bölgeli sorgular çok daha az verimlidir ve performans göstergeleri kısmi kapsama alanı sunar.

- [Paralellik](#parallelism): Sistemin bu sorguyu birden çok düğümde ne kadar yürütebildiğini gösterir. Yalnızca yüksek CPU tüketimi olan sorgularla alakalıdır. Belirli işlevlerin ve işleçlerin kullanımından etkilenir.


## <a name="total-cpu"></a>Toplam CPU
Bu sorguyu tüm sorgu işleme düğümleri arasında işlemek için yatırılan gerçek işlem cpu'su. Sorguların çoğu çok sayıda düğümüzerinde yürütüldolduğundan, bu genellikle sorgunun gerçekten yürütmek için aldığı süreden çok daha büyük olacaktır. 

Sorgu işleme süresi harcanmış:
- Veri alma - eski verilerin alınması, son verilerin alınmasından daha fazla zaman tüketir.
- Veri işleme – verilerin mantığı ve değerlendirilmesi. 

Sorgu işleme düğümlerinde harcanan zaman dışında, Azure Monitor Günlükleri tarafından harcanan ek süre vardır: kullanıcının kimliğini doğrulamak ve bu verilere erişmelerine izin verildiğinden doğrulama, veri deposunu bulma, sorguyu ayrıştırmak ve sorgu işlemini ayırmak Düğüm. Bu süre sorgu toplam CPU süresine dahil edilmez.

### <a name="early-filtering-of-records-prior-of-using-high-cpu-functions"></a>Yüksek CPU işlevleri kullanmadan önce kayıtların erken filtrelemi

Bazı sorgu komutları ve işlevleri CPU tüketiminde ağırdır. Bu, özellikle JSON ve XML'i ayrıştıran veya karmaşık düzenli ifadeleri ayıklayan komutlar için geçerlidir. Bu tür ayrıştırma, [parse_json()](/azure/kusto/query/parsejsonfunction) veya [parse_xml()](/azure/kusto/query/parse-xmlfunction) işlevleri aracılığıyla veya dinamik sütunlara atıfta bulunurken dolaylı olarak gerçekleşebilir.

Bu işlevler, işlemyaptıkları satır sayısıyla orantılı olarak CPU tüketir. En verimli en iyi duruma getirilmesi, CPU yoğun işlevi yürütülmeden önce mümkün olduğunca çok kaydı filtreleyebilen sorgunun erken koşullarını eklemektir.

Örneğin, aşağıdaki sorgular tam olarak aynı sonucu üretir, ancak ikincisi, ayrıştırmadan önceki koşulun birçok kaydı dışladığı [yer](/azure/kusto/query/whereoperator) olarak açık ara en verimli olandır:

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

### <a name="avoid-using-evaluated-where-clauses"></a>Değerlendirilmiş maddeleri kullanmaktan kaçının

Veri kümesinde fiziksel olarak bulunan sütunlar yerine değerlendirilmiş sütundaki [yan](/azure/kusto/query/whereoperator) tümcelerin bulunduğu sorgular verimlilik kaybeder. Değerlendirilen sütunlara filtre uygulama, büyük veri kümeleri işlendiğinde bazı sistem optimizasyonlarını önler.
Örneğin, aşağıdaki sorgular tam olarak aynı sonucu üretir, ancak ikincisi, koşulun yerleşik sütuna atıfta bulunduğu [yer](/azure/kusto/query/whereoperator) olarak daha verimlidir

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

### <a name="use-effective-aggregation-commands-and-dimmentions-in-summarize-and-join"></a>Özetlemek ve katılmak etkili toplama komutları ve dimmentions kullanın

[Max()](/azure/kusto/query/max-aggfunction), [sum()](/azure/kusto/query/sum-aggfunction), [count()](/azure/kusto/query/count-aggfunction)ve [avg()](/azure/kusto/query/avg-aggfunction) gibi bazı toplama komutları, mantıkları nedeniyle düşük CPU etkisine sahip olsa da, diğer leri daha karmaşıktır ve buluşsal ve verimli bir şekilde yürütülmesini sağlayan tahminler içerir. Örneğin, [dcount()](/azure/kusto/query/dcount-aggfunction) hyperloglog algoritmasını kullanarak her değeri gerçekten saymadan büyük veri kümelerinin farklı sayılarına yakın bir tahmin sağlar; yüzdelik fonksiyonlar en yakın sıralama yüzdelik algoritması kullanarak benzer yaklaşımlar yapıyoruz. Komutların bazıları, etkilerini azaltmak için isteğe bağlı parametreler içerir. Örneğin, [makeset()](/azure/kusto/query/makeset-aggfunction) işlevi, CPU ve belleği önemli ölçüde etkileyen maksimum ayar boyutunu tanımlamak için isteğe bağlı bir parametreye sahiptir.

[Birleştirme](/azure/kusto/query/joinoperator?pivots=azuremonitor) ve [özet komutları,](/azure/kusto/query/summarizeoperator) büyük bir veri kümesini işlerken yüksek CPU kullanımına neden olabilir. Karmaşıklıkları, özetlemek veya birleştirme öznitelikleri olarak `by` kullanan sütunların *kardinallik*olarak adlandırılan olası değerlerin sayısıyla doğrudan ilişkilidir. Birleştirme ve özetleme nin açıklaması ve optimizasyonu için, dokümantasyon makalelerine ve optimizasyon ipuçlarına bakın.

Örneğin, **CounterPath** her zaman **CounterName** ve **ObjectName**eşlenir, çünkü aşağıdaki sorgular tam olarak aynı sonucu üretir. Toplama boyutu daha küçük olduğundan ikincisi daha verimlidir:

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

CPU tüketimi, yoğun bilgi işlem gerektiren koşulların veya genişletilmiş sütunların bulunduğu yerlerden de etkilenebilir. Gelişmiş metin eşleşmeleri daha fazla etkiye [sahipken, eşit ==](/azure/kusto/query/datatypes-string-operators) ve [başlangıçlar](/azure/kusto/query/datatypes-string-operators) gibi tüm önemsiz dize karşılaştırmaları kabaca aynı CPU etkisine sahiptir. Özellikle, [operatör](/azure/kusto/query/datatypes-string-operators) daha verimli olduğunu [operatör içerir.](/azure/kusto/query/datatypes-string-operators) Dize işleme teknikleri nedeniyle, kısa dizeleri daha dört karakterdaha uzun dizeleri aramak için daha verimlidir.

Örneğin, aşağıdaki sorgular Bilgisayar adlandırma ilkesine bağlı olarak benzer sonuçlar üretir, ancak ikincisi daha verimlidir:

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
> Bu gösterge, hemen kümeden yalnızca CPU'yu sunar. Çok günlük sorgusunda, yalnızca bir bölgeyi temsil eder. Çok çalışma alanı sorgusunda, tüm çalışma alanlarını içermeyebilir.


## <a name="data-used-for-processed-query"></a>İşlenmiş sorgu için kullanılan veriler

Sorgunun işlenmesinde önemli bir faktör, taranan ve sorgu işleme için kullanılan veri hacmidir. Azure Veri Gezgini, diğer veri platformlarına kıyasla veri hacmini önemli ölçüde azaltan agresif optimizasyonlar kullanır. Yine de, sorguda kullanılan veri hacmini etkilenebilen kritik etkenler vardır.

Azure Monitor Günlükleri'nde, **Zaman Oluşturulan** sütun verileri dizine ekolarak kullanılır. **TimeGenerated** değerlerinin mümkün olduğunca dar bir aralıkla sınırlandırılması, işlenmesi gereken veri miktarını önemli ölçüde sınırlayarak sorgu performansında önemli bir iyileşme sağlar.

### <a name="avoid-unnecessary-use-of-search-and-union-operators"></a>Arama ve sendika operatörlerinin gereksiz kullanımından kaçının

İşlem olan verileri artıran bir diğer faktör de çok sayıda tablonun kullanılmasıdır. Bu genellikle `search *` ve `union *` komutları kullanıldığında olur. Bu komutlar, sistemi çalışma alanındaki tüm tablolardaki verileri değerlendirmeye ve tarayabilir. Bazı durumlarda, çalışma alanında yüzlerce tablo olabilir. Belirli bir tabloya kapsam olmadan "arama *" veya herhangi bir arama kullanarak mümkün olduğunca uzak durmaya çalışın.

Örneğin, aşağıdaki sorgular tam olarak aynı sonucu üretir, ancak sonuncusu açık ara en verimli dir:

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

Veri hacmini azaltmak için başka bir [yöntem,](/azure/kusto/query/whereoperator) sorgunun erken koşulları olmasıdır. Azure Veri Gezgini platformu, hangi bölümlerin belirli bir durum için uygun veriler içerdiğini bilmesini sağlayan bir önbellek içerir. Örneğin, bir sorgu `where EventID == 4624` içeriyorsa, sorguyu yalnızca eşleşen olaylarla bölümleri işleyen düğümlere dağıtır.

Aşağıdaki örnek sorgular tam olarak aynı sonucu üretir, ancak ikincisi daha verimlidir:

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

Azure Veri Gezgini bir sütun veri deposu olduğundan, her sütunun alınması diğerlerinden bağımsızdır. Alınan sütun sayısı genel veri hacmini doğrudan etkiler. Yalnızca sonuçları [özetleyerek](/azure/kusto/query/summarizeoperator) veya belirli sütunları [yansıtarak](/azure/kusto/query/projectoperator) gereken çıktısütunlarını eklemeniz gerekir. Azure Veri Gezgini, alınan sütun sayısını azaltmak için çeşitli optimizasyonlara sahiptir. Bir sütunun gerekli olmadığını belirlerse, örneğin [özet](/azure/kusto/query/summarizeoperator) komutunda başvurulmuyorsa, sütunu geri almaz.

Örneğin, ikinci sorgu, bir sütun değil üç sütun getirmesi gerektiğinden üç kat daha fazla veri işleyebilir:

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

Azure Monitör Günlükleri'ndeki tüm günlükler **TimeGenerated** sütununa göre bölümlere ayrılmıştır. Erişilen bölüm sayısı, zaman aralığıyla doğrudan ilişkilidir. Zaman aralığını azaltmak, bir istem sorgusu yürütmesini güvence altına almanın en etkili yoludur.

Zaman aralığı, [Azure Monitor Log Analytics'te Log sorgu kapsamı ve zaman aralığında](scope.md#time-range)açıklandığı gibi Log Analytics ekranındaki zaman aralığı seçici kullanılarak ayarlanabilir. Seçili zaman aralığı sorgu meta verileri kullanılarak arka uca geçirildiği için önerilen yöntem budur. 

Alternatif bir yöntem açıkça sorguda **TimeGenerated** bir [nerede](/azure/kusto/query/whereoperator) koşulu eklemektir. Sorgu farklı bir arabirimden kullanıldığında bile, zaman aralığının sabit olmasını garanti ettiği için bu yöntemi kullanmalısınız.
Sorgunun tüm bölümlerinin **TimeGenerated** filtreleri olduğundan emin olmalısınız. Bir sorgu, çeşitli tablolardan veya aynı tablodan veri getiren alt sorgulara sahipse, her birinin kendi [koşulunu](/azure/kusto/query/whereoperator) eklemesi vardır.

### <a name="make-sure-all-sub-queries-have-timegenerated-filter"></a>Tüm alt sorguların TimeGenerated filtresi olduğundan emin olun

Örneğin, aşağıdaki sorguda, **Perf** tablosu yalnızca son gün için taranırken, **Kalp Atışı** tablosu tüm geçmişi için taranacaktır ve bu tablo iki yıla kadar olabilir:

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

Böyle bir hatanın meydana geldiği yaygın bir durum, en son oluşumu bulmak için [arg_max()](/azure/kusto/query/arg-max-aggfunction) kullanıldığında gerçekleşir. Örnek:

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

Bu hata için başka bir örnek, zaman kapsamı filtreleme nin birkaç tablo üzerinde [birbirleğe](/azure/kusto/query/unionoperator?pivots=azuremonitor) hemen sonra gerçekleştirilmesidir. Birliği gerçekleştirirken, her alt sorgu kapsamlı olmalıdır. Kapsam tutarlılığını sağlamak için [let](/azure/kusto/query/letstatement) deyimini kullanabilirsiniz.

Örneğin, aşağıdaki sorgu yalnızca son 1 gün değil, *Kalp Atışı* ve *Perf* tablolarında bulunan tüm verileri tarar:

```Kusto
Heartbeat 
| summarize arg_min(TimeGenerated,*) by Computer
| union (
    Perf 
    | summarize arg_min(TimeGenerated,*) by Computer) 
| where TimeGenerated > ago(1d)
| summarize min(TimeGenerated) by Computer
```

Bu sorgu aşağıdaki gibi düzeltilmelidir:

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

Ölçüm her zaman belirtilen gerçek süreden daha büyüktür. Örneğin, sorgudaki filtre 7 gün ise, sistem 7,5 veya 8,1 gün tarayabilir. Bunun nedeni, sistemin verileri değişken boyutta parçalara bölmesidir. İlgili tüm kayıtların tarandığından emin olmak için, birkaç saat ve hatta bir günden daha uzun bir süreyi kapsayabilecek tüm bölümü tarar.

Sistemin zaman aralığının doğru bir ölçümünün sağlayamadığı birkaç durum vardır. Bu, sorgunun bir günden kısa bir süre içinde veya çok çalışma alanı sorgularında olduğu durumların çoğunda gerçekleşir.


> [!IMPORTANT]
> Bu gösterge yalnızca hemen kümede işlenen verileri sunar. Çok günlük sorgusunda, yalnızca bir bölgeyi temsil eder. Çok çalışma alanı sorgusunda, tüm çalışma alanlarını içermeyebilir.

## <a name="age-of-processed-data"></a>İşlenen verilerin yaşı
Azure Veri Gezgini çeşitli depolama katmanları kullanır: bellek içi, yerel SSD diskler ve çok daha yavaş Azure Blob'ları. Veriler ne kadar yeniyse, sorgu süresini ve CPU'yu azaltarak daha küçük gecikme süresine sahip daha performant bir katmanda depolanma olasılığı o kadar yüksek olur. Verilerin kendisi dışında, sistemin meta veriler için bir önbelleği de vardır. Veriler ne kadar eskiolursa, meta verilerinin önbellekte olma olasılığı da o kadar az olur.

Bazı sorgular eski verilerin kullanımını gerektirirken, eski verilerin yanlışlıkla kullanıldığı durumlar vardır. Bu, sorgular meta verilerinde zaman aralığı sağlamadan yürütüldüğünde ve tüm tablo başvuruları **TimeGenerated** sütununa filtre içermediğinde olur. Bu gibi durumlarda, sistem bu tabloda depolanan tüm verileri tayacaktır. Veri saklama uzun olduğunda, uzun zaman aralıkları ve böylece veri saklama süresi kadar eski verileri kapsayabilir.

Bu gibi durumlar örneğin olabilir:

- Log Analytics'te zaman aralığını sınırlı olmayan bir alt sorguyla ayarlamamak. Yukarıdaki örneğe bakın.
- Zaman aralığı isteğe bağlı parametreler olmadan API kullanma.
- Power BI konektörü gibi bir zaman aralığı zorlamayan bir istemci kullanma.

Bu durumda da ilgili olduğu gibi bölümünde örnekler ve notlar bakın.

## <a name="number-of-regions"></a>Bölge sayısı
Tek bir sorgunun farklı bölgelerde yürütülebileceği birkaç durum vardır:

- Birkaç çalışma alanı açıkça listelendiğinde ve bunlar farklı bölgelerde bulunduğunda.
- Kaynak kapsamı yla yapılan bir sorgu veri alıyorsa ve veriler farklı bölgelerde bulunan birden çok çalışma alanında depolandığında.

Bölgeler arası sorgu yürütme, sistemin genellikle sorgu nihai sonuçlarından çok daha büyük olan büyük ara veri yığınlarını serihale ve aktarmasını gerektirir. Ayrıca, sistemin optimizasyonları, buluşsal işlemleri gerçekleştirme ve önbellekkullanma yeteneğini de sınırlar.
Tüm bu bölgeleri taramak için gerçek bir neden yoksa, kapsamı daha az bölgeyi kapsaya göre ayarlamanız gerekir. Kaynak kapsamı en aza indirilmiş, ancak yine de birçok bölge kullanılıyorsa, yanlış yapılandırma nedeniyle oluşabilir. Örneğin, denetim günlükleri ve tanılama ayarları farklı bölgelerdeki farklı çalışma alanlarına gönderilir veya birden çok tanılama ayarları yapılandırmaları vardır. 

> [!IMPORTANT]
> Bir sorgu çeşitli bölgeler arasında çalıştırıldığında, CPU ve veri ölçümleri doğru olmaz ve ölçümü yalnızca bölgelerden birinde temsil eder.

## <a name="number-of-workspaces"></a>Çalışma alanı sayısı
Çalışma alanları, günlük verilerini ayırmak ve yönetmek için kullanılan mantıksal kapsayıcılardır. Arka uç, seçili bölge içindeki fiziksel kümeler üzerindeki çalışma alanı yerleşimlerini en iyi duruma getirin.

Birden çok çalışma alanının kullanımı şu nedenlerden kaynaklanabilir: 

- Birden çok çalışma alanının açıkça listelendiği yer.
- Kaynak kapsamı yla yapılan bir sorgu veri alıyorsa ve veriler birden çok çalışma alanında depolandığında.
 
Sorguların çapraz bölge ve çapraz küme yürütmesi, sistemin genellikle sorgu nihai sonuçlarından çok daha büyük olan arka uçtaki büyük ara veri yığınlarında serihale getirilmesini ve aktarılmasını gerektirir. Ayrıca optimizasyonlar, buluşsal ve önbellekleri kullanarak gerçekleştirmek için sistem yeteneğini sınırlar.

> [!IMPORTANT]
> Bazı çok çalışma alanı senaryolarında, CPU ve veri ölçümleri doğru olmaz ve ölçümü yalnızca çalışma alanlarının birkaçına gösterir.

## <a name="parallelism"></a>Parallelism
Azure Monitor Günlükleri sorguları çalıştırmak için büyük Azure Veri Gezgini kümeleri kullanıyor ve bu kümeler ölçek olarak farklılık gösterarak düzinelerce işlem düğümüne kadar çıkma potansiyeline varıyor. Sistem kümeleri çalışma alanı yerleştirme mantığına ve kapasitesine göre otomatik olarak ölçeklendirilir.

Bir sorguyu verimli bir şekilde yürütmek için, işlem için gerekli olan verilere göre bölümlere bölünür ve düğümleri hesaplamak için dağıtılır. Sistemin bunu verimli bir şekilde yapamayacağı bazı durumlar vardır. Bu, sorgunun uzun bir süreye yol açabilir. 

Paralelliği azaltabilecek sorgu davranışları şunlardır:

- [Serialize işleci](/azure/kusto/query/serializeoperator), [sonraki()](/azure/kusto/query/nextfunction), [prev()](/azure/kusto/query/prevfunction)ve [satır](/azure/kusto/query/rowcumsumfunction) işlevleri gibi serileştirme ve pencere işlevlerinin kullanımı. Bu durumlardan bazılarında zaman serisi ve kullanıcı analizi işlevleri kullanılabilir. Aşağıdaki işleçler sorgunun sonunda kullanılmazsa verimsiz serileştirme de oluşabilir: [aralık](/azure/kusto/query/rangeoperator), [sıralama,](/azure/kusto/query/sortoperator) [sıra](/azure/kusto/query/orderoperator), [üst](/azure/kusto/query/topoperator), [üst hitters](/azure/kusto/query/tophittersoperator), [getschema](/azure/kusto/query/getschemaoperator).
-    [Dcount()](/azure/kusto/query/dcount-aggfunction) toplama işlevinin kullanımı sistemi farklı değerlerin merkezi kopyasına sahip olmaya zorlar. Veri ölçeği yüksek olduğunda, doğruluğu azaltmak için dcount işlevini isteğe bağlı parametreleri kullanmayı düşünün.
-    Çoğu durumda, [birleştirme](/azure/kusto/query/joinoperator?pivots=azuremonitor) işleci genel paralelliği düşürür. Performans sorunlu olduğunda shuffle birleştirme'yi alternatif olarak inceleyin.
-    Kaynak kapsamı sorgularında, yürütme öncesi RBAC denetimleri çok sayıda RBAC ataması olduğu durumlarda oyalanabilir. Bu, daha düşük paralelliğe neden olacak daha uzun denetimlere yol açabilir. Örneğin, bir sorgu, binlerce kaynağın bulunduğu ve her kaynağın kaynak düzeyinde abonelik veya kaynak grubunda değil, çok sayıda rol ataması olduğu bir abonelikte yürütülür.
-    Bir sorgu küçük veri yığınlarını işliyorsa, sistem onu birçok işlem düğümüne yaymayacağı için paralelliği düşük olacaktır.



## <a name="next-steps"></a>Sonraki adımlar

- [Kusto sorgu dili için başvuru belgeleri](/azure/kusto/query/).
