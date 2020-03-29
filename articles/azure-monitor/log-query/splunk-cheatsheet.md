---
title: Azure Monitor günlük sorgusuna splunk | Microsoft Dokümanlar
description: Azure Monitor günlük sorgularını öğrenmede Splunk'u tanıyan kullanıcılar için yardım.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/21/2018
ms.openlocfilehash: 6346055f1169bfa533d5dbfe441ecf27fb0d78a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75397755"
---
# <a name="splunk-to-azure-monitor-log-query"></a>Azure Monitör günlük sorgusuna splunk

Bu makale, Splunk'u bilen kullanıcıların Azure Monitor'da günlük sorguları yazmak için Kusto sorgu dilini öğrenmelerine yardımcı olmak için tasarlanmıştır. Mevcut bilginizden yararlanabileceğiniz temel farklılıkları ve benzerlikleri anlamak için ikisi arasında doğrudan karşılaştırmalar yapılır.

## <a name="structure-and-concepts"></a>Yapı ve kavramlar

Aşağıdaki tablo, Splunk ve Azure Monitor günlükleri arasındaki kavramları ve veri yapılarını karşılaştırıyor.

 | Kavram  | Splunk | Azure İzleyici |  Açıklama
 | --- | --- | --- | ---
 | Dağıtım birimi  | cluster |  cluster |  Azure Monitor rasgele çapraz küme sorgularına izin verir. Splunk bilmiyor. |
 | Veri önbellekleri |  Kova  |  Önbelleğe alma ve bekletme ilkeleri |  Verilerin dönem ve önbelleğe alma düzeyini denetler. Bu ayar, sorguların performansını ve dağıtım maliyetini doğrudan etkiler. |
 | Verilerin mantıksal bölümü  |  dizin  |  database  |  Verilerin mantıksal olarak ayrılmasını sağlar. Her iki uygulama da bu bölümler arasında birleştirme ve birleştirme sağlar. |
 | Yapılandırılmış olay meta verileri | Yok | tablo |  Splunk olay meta verilerinin arama diline maruz kavramına sahip değildir. Azure Monitor günlüklerinde sütunları olan bir tablo kavramı vardır. Her olay örneği bir satıra eşlenir. |
 | Veri kaydı | event | Satır |  Terminoloji sadece değişir. |
 | Veri kayıt özniteliği | alan |  sütun |  Azure Monitor'da bu, tablo yapısının bir parçası olarak önceden tanımlanır. Splunk'ta her olayın kendi alanları vardır. |
 | Türler | Datatype |  Datatype |  Azure Monitor veri türleri sütunlarda ayarlanırken daha açık hale gelir. Her ikisi de veri türleri ve JSON desteği de dahil olmak üzere veri türleri kabaca eşdeğer kümesi ile dinamik çalışma yeteneğine sahiptir. |
 | Sorgu ve arama  | search | sorgu |  Kavramlar temelde hem Azure Monitor hem de Splunk arasında aynıdır. |
 | Olay yutma süresi | Sistem Saati | ingestion_time() |  Splunk'ta, her olay, olayın dizine eklenmiş olduğu saatin sistem zaman damgasını alır. Azure Monitor'da, ingestion_time() işlevi aracılığıyla başvurulabilen bir sistem sütununu ortaya çıkaran ingestion_time adlı bir ilke tanımlayabilirsiniz. |

## <a name="functions"></a>İşlevler

Aşağıdaki tablo, Azure Monitor'da Splunk işlevlerine eşdeğer işlevleri belirtir.

|Splunk | Azure İzleyici |Açıklama
|---|---|---
|strcat | strcat()| (1) |
|split  | split() | (1) |
|if     | iff()   | (1) |
|tonumber | todouble()<br>tolong()<br>toint() | (1) |
|Üst<br>Alt |toupper()<br>tolower()|(1) |
| Değiştirmek | değiştir() | (1)<br> Ayrıca, her `replace()` iki üründe de üç parametre alırken, parametrelerin farklı olduğunu unutmayın. |
| Substr | substring() | (1)<br>Ayrıca Splunk'un tek tabanlı endeksler kullandığını da unutmayın. Azure Monitor sıfır tabanlı endeksleri not eder. |
| Tolower |  tolower() | (1) |
| Toupper | toupper() | (1) |
| match | regex eşleşir |  (2)  |
| Regex | regex eşleşir | Splunk yılında, `regex` bir operatördür. Azure Monitor'da ilişkisel bir işleç. |
| searchmatch | == | Splunk olarak, `searchmatch` tam dize arıyor sağlar.
| rastgele | rand()<br>rand(n) | Splunk'un işlevi sıfırdan 2<sup>31</sup>-1'e bir sayı döndürür. Azure Monitor, 0,0 ile 1,0 arasında veya bir parametre sağlanmışsa, 0 ile n-1 arasında bir sayı döndürür.
| şimdi | now() | (1)
| relative_time | totimespan() | (1)<br>Azure Monitor'da, Splunk'un relative_time eşdeğeri (datetimeVal, offsetVal) datetimeVal + totimespan(offsetVal) 'dir.<br>Örneğin, <code>search &#124; eval n=relative_time(now(), "-1d@d")</code> olur. <code>...  &#124; extend myTime = now() - totimespan("1d")</code>

(1) Splunk'ta işlev `eval` işleçle birlikte çağrılır. Azure Monitor'da, bir parçası `extend` `project`olarak kullanılır veya.<br>(2) Splunk'ta fonksiyon `eval` işleçle birlikte çağrılır. Azure Monitor'da `where` işleçle birlikte kullanılabilir.


## <a name="operators"></a>İşleçler

Aşağıdaki bölümlerde Splunk ve Azure Monitor arasında farklı işleçler kullanma örnekleri verilmektedir.

> [!NOTE]
> Aşağıdaki örnek için, Splunk alanı _kuralı_ Azure Monitor'daki bir tabloyla ve Splunk'un varsayılan zaman damgası eşlerini Logs Analytics _ingestion_time()_ sütununa eşler.

### <a name="search"></a>Search
Splunk'ta, anahtar kelimeyi `search` atlayabilir ve tırnak içinde teklif edilmemiş bir dize belirtebilirsiniz. Azure Monitor'da her sorguyu `find`, tırnak içinde olmayan bir dize sütun adıdır ve arama değeri teklif edilmiş bir dize olmalıdır. 

| |  | |
|:---|:---|:---|
| Splunk | **Arama** | <code>search Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" earliest=-24h</code> |
| Azure İzleyici | **find** | <code>find Session.Id=="c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time()> ago(24h)</code> |
| | |

### <a name="filter"></a>Filtre
Azure Monitor günlük sorguları, filtrenin bulunduğu noktadan başlar. Splunk'ta filtreleme, geçerli dizindeki varsayılan işlemdir. Ayrıca Splunk operatör kullanabilirsiniz, `where` ancak tavsiye edilmez.

| |  | |
|:---|:---|:---|
| Splunk | **Arama** | <code>Event.Rule="330009.2" Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" _indextime>-24h</code> |
| Azure İzleyici | **where** | <code>Office_Hub_OHubBGTaskError<br>&#124; where Session_Id == "c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time() > ago(24h)</code> |
| | |


### <a name="getting-n-eventsrows-for-inspection"></a>Denetim için n olayları/satırları alma 
Azure Monitor günlük sorguları da diğer `limit`ad olarak destekler. `take` Splunk'ta, sonuçlar sıralanırsa, `head` ilk n sonuçları döndürülecektir. Azure Monitor'da sınır sıralanmaz, ancak bulunan ilk n satırlarını döndürür.

| |  | |
|:---|:---|:---|
| Splunk | **Kafa** | <code>Event.Rule=330009.2<br>&#124; head 100</code> |
| Azure İzleyici | **Sınırı** | <code>Office_Hub_OHubBGTaskError<br>&#124; limit 100</code> |
| | |



### <a name="getting-the-first-n-eventsrows-ordered-by-a-fieldcolumn"></a>Alan/sütun tarafından sıralanan ilk n olaylarını/satırlarını alma
Alt sonuçlar için, Splunk'ta kullandığınız `tail`. Azure Monitor'da sipariş yönünü `asc`.

| |  | |
|:---|:---|:---|
| Splunk | **Kafa** |  <code>Event.Rule="330009.2"<br>&#124; sort Event.Sequence<br>&#124; head 20</code> |
| Azure İzleyici | **Sayfanın Üstü** | <code>Office_Hub_OHubBGTaskError<br>&#124; top 20 by Event_Sequence</code> |
| | |




### <a name="extending-the-result-set-with-new-fieldscolumns"></a>Yeni alanlar/sütunlarla belirlenen sonucu genişletme
Splunk da `eval` `eval` işleç ile karşılaştırılabilir değildir bir işlevi vardır. Hem `eval` Splunk'taki operatör `extend` hem de Azure Monitor'daki operatör yalnızca skaler işlevleri ve aritmetik işleçleri destekler.

| |  | |
|:---|:---|:---|
| Splunk | **eval** |  <code>Event.Rule=330009.2<br>&#124; eval state= if(Data.Exception = "0", "success", "error")</code> |
| Azure İzleyici | **Genişlet -mek** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend state = iif(Data_Exception == 0,"success" ,"error")</code> |
| | |


### <a name="rename"></a>Yeniden Adlandır 
Azure Monitor, `project-rename` bir alanı yeniden adlandırmak için işleci kullanır. `project-rename`sorgunun bir alan için önceden oluşturulmuş dizinlerden yararlanmasını sağlar. Splunk'un `rename` da aynısını yapacak bir operatörü var.

| |  | |
|:---|:---|:---|
| Splunk | **Yeni -den adlandırmak** |  <code>Event.Rule=330009.2<br>&#124; rename Date.Exception as execption</code> |
| Azure İzleyici | **proje yeniden adlandırma** | <code>Office_Hub_OHubBGTaskError<br>&#124; project-rename exception = Date_Exception</code> |
| | |




### <a name="format-resultsprojection"></a>Sonuçları biçimlendirme/Projeksiyon
Splunk benzer bir operatör var `project-away`gibi görünmüyor . Alanları filtrelemek için UI'yi kullanabilirsiniz.

| |  | |
|:---|:---|:---|
| Splunk | **Tablo** |  <code>Event.Rule=330009.2<br>&#124; table rule, state</code> |
| Azure İzleyici | **Proje**<br>**proje-away** | <code>Office_Hub_OHubBGTaskError<br>&#124; project exception, state</code> |
| | |



### <a name="aggregation"></a>Toplama
Farklı toplama işlevleri için [Azure Monitor günlük sorgularında](aggregations.md) Toplamalar'a bakın.

| |  | |
|:---|:---|:---|
| Splunk | **Istatistik** |  <code>search (Rule=120502.*)<br>&#124; stats count by OSEnv, Audience</code> |
| Azure İzleyici | **Özetle** | <code>Office_Hub_OHubBGTaskError<br>&#124; summarize count() by App_Platform, Release_Audience</code> |
| | |



### <a name="join"></a>Birleştir
Splunk'a katılmak için önemli sınırlamalar vardır. Alt sorgunun 10000 sonuç sınırı vardır (dağıtım yapılandırma dosyasında ayarlanır) ve sınırlı sayıda birleştirme tadı vardır.

| |  | |
|:---|:---|:---|
| Splunk | **Katılın** |  <code>Event.Rule=120103* &#124; stats by Client.Id, Data.Alias \| join Client.Id max=0 [search earliest=-24h Event.Rule="150310.0" Data.Hresult=-2147221040]</code> |
| Azure İzleyici | **Katılın** | <code>cluster("OAriaPPT").database("Office PowerPoint").Office_PowerPoint_PPT_Exceptions<br>&#124; where  Data_Hresult== -2147221040<br>&#124; join kind = inner (Office_System_SystemHealthMetadata<br>&#124; summarize by Client_Id, Data_Alias)on Client_Id</code>   |
| | |



### <a name="sort"></a>Sırala
Splunk'ta, artan sırada sıralamak için `reverse` işleci kullanmanız gerekir. Azure Monitor, null'ların nerede konulacağını, başlangıçta veya sonunda tanımlamayı da destekler.

| |  | |
|:---|:---|:---|
| Splunk | **Sıralama** |  <code>Event.Rule=120103<br>&#124; sort Data.Hresult<br>&#124; reverse</code> |
| Azure İzleyici | **tarafından sipariş** | <code>Office_Hub_OHubBGTaskError<br>&#124; order by Data_Hresult,  desc</code> |
| | |



### <a name="multivalue-expand"></a>Çok değer genişletme
Bu, hem Splunk hem de Azure Monitor'da benzer bir işleçtir.

| |  | |
|:---|:---|:---|
| Splunk | **mvexpand** |  `mvexpand foo` |
| Azure İzleyici | **mvexpand** | `mvexpand foo` |
| | |




### <a name="results-facets-interesting-fields"></a>Sonuçlar yönleri, ilginç alanlar
Azure portalındaki Log Analytics'te yalnızca ilk sütun açıkta kalır. Tüm sütunlar API aracılığıyla kullanılabilir.

| |  | |
|:---|:---|:---|
| Splunk | **Alanları** |  <code>Event.Rule=330009.2<br>&#124; fields App.Version, App.Platform</code> |
| Azure İzleyici | **Yön -leriyle** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; facet by App_Branch, App_Version</code> |
| | |




### <a name="de-duplicate"></a>Yinelemeyi çözme
Bunun yerine, hangi kaydın seçilme sırasını tersine çevirmek için kullanabilirsiniz. `summarize arg_min()`

| |  | |
|:---|:---|:---|
| Splunk | **dedup** |  <code>Event.Rule=330009.2<br>&#124; dedup device_id sortby -batterylife</code> |
| Azure İzleyici | **arg_max özetle()** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; summarize arg_max(batterylife, *) by device_id</code> |
| | |




## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitor'da yazma günlüğü sorguları](get-started-queries.md)hakkında bir ders ten geç.
