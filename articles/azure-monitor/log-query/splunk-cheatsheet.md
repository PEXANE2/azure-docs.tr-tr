---
title: Azure Izleyici günlük sorgusuna splunk Microsoft Docs
description: Azure Izleme günlük sorgularını öğrenirken splunk hakkında bilgili olan kullanıcılar için yardım.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/21/2018
ms.openlocfilehash: 00fdaf93553c97112c67caa66cb2246756b63c33
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207491"
---
# <a name="splunk-to-azure-monitor-log-query"></a>Azure Izleyici günlük sorgusuna splunk

Bu makale, Azure Izleyici 'de günlük sorgularını yazmak üzere kusto sorgu dilini öğrenmek üzere splunk 'yi bilen kullanıcılara yardımcı olmaya yöneliktir. Doğrudan karşılaştırmalar, temel farklılıkları anlamak için iki arasında yapılır ve ayrıca var olan bilginizi kullanabileceğiniz benzerlikler arasında yapılır.

## <a name="structure-and-concepts"></a>Yapı ve kavramlar

Aşağıdaki tabloda, splunk ile Azure Izleyici günlükleri arasındaki kavramlar ve veri yapıları karşılaştırılmaktadır.

 | Konsept  | Splunk | Azure İzleyici |  Yorum
 | --- | --- | --- | ---
 | Dağıtım birimi  | cluster |  cluster |  Azure Izleyici, rastgele çapraz küme sorgularına izin verir. Splunk değildir. |
 | Veri önbellekleri |  demet  |  Önbelleğe alma ve bekletme ilkeleri |  Verilerin süresini ve önbelleğe alma düzeyini denetler. Bu ayar, sorguların ve dağıtım maliyetinin performansını doğrudan etkiler. |
 | Verilerin mantıksal bölümü  |  dizin  |  database  |  Verilerin mantıksal olarak ayrılmasını sağlar. Her iki uygulama da birleşimlere ve bu bölümlerde birleştirme yapmasına izin verir. |
 | Yapılandırılmış olay meta verileri | Yok | table |  Splunk, olay meta verisinin arama diline açık bir kavram içermez. Azure Izleyici günlüklerinde sütunları olan bir tablo kavramı vardır. Her olay örneği bir satıra eşlenir. |
 | Veri kaydı | event | sırada |  Yalnızca terminoloji değişikliği. |
 | Veri kaydı özniteliği | alan |  sütun |  Azure Izleyici 'de, bu tablo yapısının bir parçası olarak önceden tanımlanmıştır. Splunk 'de, her olayın kendi alan kümesi vardır. |
 | Türler | x |  x |  Azure Izleyici veri türleri sütunlarda ayarlandığı şekilde daha açıktır. Her ikisinin de, JSON desteği dahil olmak üzere veri türleri ve kabaca eşdeğer veri türleri kümesiyle dinamik olarak çalışma yeteneği vardır. |
 | Sorgu ve arama  | search | sorgu |  Kavramlar temelde Azure Izleyici ve splunk arasında aynıdır. |
 | Olay alımı süresi | Sistem saati | ingestion_time() |  Splunk 'de, her olay, olayın dizine alındığı zamanın sistem zaman damgasını alır. Azure Izleyici 'de, ingestion_time () işlevi aracılığıyla başvurulabilen bir sistem sütununu açığa çıkaran ingestion_time adlı bir ilke tanımlayabilirsiniz. |

## <a name="functions"></a>İşlevler

Aşağıdaki tabloda, Azure Izleyici 'deki splunk işlevlerine denk gelen işlevler belirtilir.

|Splunk | Azure İzleyici |Yorum
|---|---|---
|strcat | strcat()| (1) |
|split  | split() | (1) |
|if     | FF ()   | (1) |
|ToNumber | todouble()<br>tolong()<br>toint() | (1) |
|üst<br>düşürül |toupper()<br>tolower()|(1) |
| değiştirin | replace() | (1)<br> Ayrıca `replace()` , her iki üründe de üç parametre alacağından, parametrelerin farklı olduğunu unutmayın. |
| substr | substring() | (1)<br>Ayrıca, splunk 'nin tek tabanlı dizinler kullandığını unutmayın. Azure Izleyici, sıfır tabanlı dizinleri not edin. |
| ToLower |  tolower() | (1) |
| ToUpper | toupper() | (1) |
| match | Regex ile eşleşir |  (2)  |
| Regex | Regex ile eşleşir | Splunk 'de `regex` bir işleçtir. Azure Izleyici 'de ilişkisel bir işleçtir. |
| searchmatch | == | Splunk 'de `searchmatch` Tam dize aramasına izin verir.
| rastgele | rand()<br>S_SAYI_ÜRET (n) | Splunk işlevi, sıfırdan 2<sup>31</sup>-1 arasında bir sayı döndürür. Azure Izleyici ', 0,0 ile 1,0 arasında bir sayı veya bir parametre sağlanmışsa 0 ile n-1 arasında bir sayı döndürür.
| şimdi | now() | (1)
| relative_time | totimespan() | (1)<br>Azure Izleyici 'de, splunk 'ın relative_time (datetimeVal, offsetVal) ' nin eşdeğeri datetimeVal + ToTimeSpan (offsetVal) şeklindedir.<br>Örneğin, <code>search &#124; eval n=relative_time(now(), "-1d@d")</code> olur <code>...  &#124; extend myTime = now() - totimespan("1d")</code> .

(1) splunk 'de işlev `eval` işleçle çağrılır. Azure Izleyici 'de, veya parçası olarak kullanılır `extend` `project` .<br>(2) splunk içinde, işlev `eval` işleçle çağrılır. Azure Izleyici 'de `where` işleçle birlikte kullanılabilir.


## <a name="operators"></a>İşleçler

Aşağıdaki bölümlerde, splunk ve Azure Izleyici arasında farklı işleçler kullanma örnekleri verilmektedir.

> [!NOTE]
> Aşağıdaki örnekte, splunk alan _kuralı_ , Azure izleyici 'deki bir tabloyla eşlenir ve splunk 'ın varsayılan zaman damgası, günlük Analizi _ingestion_time ()_ sütunuyla eşlenir.

### <a name="search"></a>Arama
Splunk 'de, `search` anahtar sözcüğünü atlayabilir ve tırnak işareti olmayan bir dize belirtebilirsiniz. Azure Izleyici 'de, her sorguyu ile başlatmanız gerekir `find` ; tırnak içine alınmış olmayan bir dize bir sütun adıdır ve arama değeri tırnak içine alınmış bir dize olmalıdır. 

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **aramanız** | <code>search Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" earliest=-24h</code> |
| **Azure İzleyici** | **find** | <code>find Session.Id=="c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time()> ago(24h)</code> |


### <a name="filter"></a>Filtre
Azure Izleyici günlük sorguları, filtrenin bulunduğu tablolu bir sonuç kümesinden başlar. Splunk 'de, filtreleme geçerli dizindeki varsayılan işlemdir. `where`İşleci de splunk içinde de kullanabilirsiniz, ancak önerilmez.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **aramanız** | <code>Event.Rule="330009.2" Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" _indextime>-24h</code> |
| **Azure İzleyici** | **olmadığı** | <code>Office_Hub_OHubBGTaskError<br>&#124; where Session_Id == "c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time() > ago(24h)</code> |

### <a name="getting-n-eventsrows-for-inspection"></a>İnceleme için n olay/satır alma 
Azure Izleyici günlük sorguları, `take` için diğer adı da destekler `limit` . Splunk 'de, sonuçlar Sıralansa `head` ilk n sonucunu döndürür. Azure Izleyici 'de sınır sıralı değildir ancak bulunan ilk n satırı döndürür.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **başlı** | <code>Event.Rule=330009.2<br>&#124; head 100</code> |
| **Azure İzleyici** | **sınırlı** | <code>Office_Hub_OHubBGTaskError<br>&#124; limit 100</code> |

### <a name="getting-the-first-n-eventsrows-ordered-by-a-fieldcolumn"></a>Alan/sütun tarafından sıralanan ilk n olay/satırı alma
En alttaki sonuçlar için, kullandığınız splunk `tail` . Azure Izleyici 'de, ile sıralama yönünü belirtebilirsiniz `asc` .

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **başlı** |  <code>Event.Rule="330009.2"<br>&#124; sort Event.Sequence<br>&#124; head 20</code> |
| **Azure İzleyici** | **top** | <code>Office_Hub_OHubBGTaskError<br>&#124; top 20 by Event_Sequence</code> |

### <a name="extending-the-result-set-with-new-fieldscolumns"></a>Sonuç kümesini yeni alanlarla/sütunlarla genişletme
Splunk Ayrıca `eval` işleçle karşılaştırılabilir olmayan bir işleve sahiptir `eval` . Hem `eval` splunk hem de `extend` Azure izleyici 'deki işleci yalnızca skaler işlevleri ve aritmetik işleçleri destekler.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **Eval** |  <code>Event.Rule=330009.2<br>&#124; eval state= if(Data.Exception = "0", "success", "error")</code> |
| **Azure İzleyici** | **genişletmeyi** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend state = iif(Data_Exception == 0,"success" ,"error")</code> |

### <a name="rename"></a>Rename 
Azure Izleyici, `project-rename` bir alanı yeniden adlandırmak için işlecini kullanır. `project-rename`sorgunun bir alan için önceden oluşturulmuş dizinlerden faydalanmasını sağlar. Splunk 'ın `rename` aynı şekilde yapması için bir işleci vardır.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **Yeniden Adlandır** |  <code>Event.Rule=330009.2<br>&#124; rename Date.Exception as execption</code> |
| **Azure İzleyici** | **proje-yeniden adlandır** | <code>Office_Hub_OHubBGTaskError<br>&#124; project-rename exception = Date_Exception</code> |

### <a name="format-resultsprojection"></a>Biçimlendirme sonuçları/projeksiyonu
Splunk şuna benzer bir işlece sahip görünmüyor `project-away` . Alan alanlarını filtrelemek için Kullanıcı arabirimini kullanabilirsiniz.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **tablosundan** |  <code>Event.Rule=330009.2<br>&#124; table rule, state</code> |
| **Azure İzleyici** | **Proje**<br>**Proje-dışarıda** | <code>Office_Hub_OHubBGTaskError<br>&#124; project exception, state</code> |

### <a name="aggregation"></a>Toplama
Farklı toplama işlevleri için [Azure izleyici günlük sorgularındaki toplamalara](aggregations.md) bakın.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **stats** |  <code>search (Rule=120502.*)<br>&#124; stats count by OSEnv, Audience</code> |
| **Azure İzleyici** | **ölçütü** | <code>Office_Hub_OHubBGTaskError<br>&#124; summarize count() by App_Platform, Release_Audience</code> |


### <a name="join"></a>Birleştir
Splunk 'da JOIN önemli sınırlamalara sahiptir. Alt sorgu 10000 sonuçlý bir sınıra sahiptir (dağıtım yapılandırma dosyasında ayarlanır) ve sınırlı sayıda JOIN türüler vardır.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **ayrılma** |  <code>Event.Rule=120103* &#124; stats by Client.Id, Data.Alias \| join Client.Id max=0 [search earliest=-24h Event.Rule="150310.0" Data.Hresult=-2147221040]</code> |
| **Azure İzleyici** | **ayrılma** | <code>cluster("OAriaPPT").database("Office PowerPoint").Office_PowerPoint_PPT_Exceptions<br>&#124; where  Data_Hresult== -2147221040<br>&#124; join kind = inner (Office_System_SystemHealthMetadata<br>&#124; summarize by Client_Id, Data_Alias)on Client_Id</code>   |

### <a name="sort"></a>Sırala
Splunk 'da artan düzende sıralamak için işlecini kullanmanız gerekir `reverse` . Azure Izleyici Ayrıca, ne zaman null, sonunda veya sonda yerleştirileceğini tanımlamayı destekler.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **düzenine** |  <code>Event.Rule=120103<br>&#124; sort Data.Hresult<br>&#124; reverse</code> |
| **Azure İzleyici** | **sıralama ölçütü** | <code>Office_Hub_OHubBGTaskError<br>&#124; order by Data_Hresult,  desc</code> |

### <a name="multivalue-expand"></a>Çoklu değer genişletme
Bu, hem splunk hem de Azure Izleyici 'de benzer bir işleçtir.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **mvexpand** |  `mvexpand foo` |
| **Azure İzleyici** | **mvexpand** | `mvexpand foo` |

### <a name="results-facets-interesting-fields"></a>Sonuç modelleri, ilginç alanlar
Azure portal Log Analytics içinde yalnızca ilk sütun gösterilir. Tüm sütunlar API aracılığıyla kullanılabilir.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **alanını** |  <code>Event.Rule=330009.2<br>&#124; fields App.Version, App.Platform</code> |
| **Azure İzleyici** | **lerle** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; facet by App_Branch, App_Version</code> |

### <a name="de-duplicate"></a>Yineleneni Kaldır
`summarize arg_min()`Bunun yerine, kaydın seçildiği sırayı tersine çevirmek için kullanabilirsiniz.

| | İşleç | Örnek |
|:---|:---|:---|
| **Splunk** | **Yinelenenleri** |  <code>Event.Rule=330009.2<br>&#124; dedup device_id sortby -batterylife</code> |
| **Azure İzleyici** | **özetleme arg_max ()** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; summarize arg_max(batterylife, *) by device_id</code> |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici 'de yazma günlüğü sorgularıyla](get-started-queries.md)bir derste ilerleyin.
