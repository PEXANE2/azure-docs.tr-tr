---
title: KQL hızlı başvuru
description: Sözdizimi örnekleri ile yararlı KQL işlevleri ve tanımları listesi.
author: orspod
ms.author: orspodek
ms.reviewer: ''
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/19/2020
ms.openlocfilehash: e7196ccccb1203ce56b2e53fa358d091374cd3f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79139088"
---
# <a name="kql-quick-reference"></a>KQL hızlı başvuru

Bu makalede, Kusto Query Language'i kullanmaya başlamanıza yardımcı olacak işlevlerin ve açıklamalarının bir listesi gösterilmektedir.

| Operatör/Fonksiyon                               | Açıklama                           | Sözdizimi                                           |
| :---------------------------------------------- | :------------------------------------ |:-------------------------------------------------|
|**Filtre/Arama/Koşul**                      |**_Filtreuygulayarak veya arayarak ilgili verileri bulma_** |                      |
| [where](/azure/kusto/query/whereoperator)                      | Belirli bir yüklemdeki filtreler           | `T | where Predicate`                         |
| [içerdiği/bulunduğu](/azure/kusto/query/whereoperator)        | `Contains`: Herhangi bir alt dize eşleşmesi arar <br> `Has`: Belirli bir kelime (daha iyi performans) arar  | `T | where col1 contains/has "[search term]"`|
| [Arama](/azure/kusto/query/searchoperator)                    | Değer için tablodaki tüm sütunları arar | `[TabularSource |] search [kind=CaseSensitivity] [in (TableSources)] SearchPredicate` |
| [almak](/azure/kusto/query/takeoperator)                        | Belirtilen kayıt sayısını verir. Sorguyı test etmek için kullanma<br>**_Not_** `_take`: `_limit`_ ve _ eşanlamlıdır. | `T | take NumberOfRows` |
| [Durumda](/azure/kusto/query/casefunction)                        | Diğer sistemlerdeki if/then/elseif'e benzer bir koşul deyimi ekler. | `case(predicate_1, then_1, predicate_2, then_2, predicate_3, then_3, else)` |
| [distinct](/azure/kusto/query/distinctoperator)                | Giriş tablosunun sağlanan sütunlarının farklı birleşimini içeren bir tablo oluşturur | `distinct [ColumnName], [ColumnName]` |
| **Tarih/Saat**                                   |**_Tarih ve saat işlevlerini kullanan işlemler_**               |                          |
|[Önce](/azure/kusto/query/agofunction)                           | Sorgunun yürütülderken göreli zaman mahsup verir. Örneğin, `ago(1h)` geçerli saatin okunmasından bir saat öncedir. | `ago(a_timespan)` |
| [format_datetime](/azure/kusto/query/format-datetimefunction)  | [Verileri çeşitli tarih biçimlerinde](/azure/kusto/query/format-datetimefunction#supported-formats)döndürür. | `format_datetime(datetime , format)` |
| [bin](/azure/kusto/query/binfunction)                          | Tüm değerleri bir zaman diliminde yuvarlar ve gruplatır | `bin(value,roundTo)` |
| **Sütun Oluşturma/Kaldırma**                   |**_Tabloya sütun ekleme veya kaldırma_** |                                                    |
| [Yazdırma](/azure/kusto/query/printoperator)                      | Bir veya daha fazla skaler ifadeiçeren tek bir satırı çıktıalır | `print [ColumnName =] ScalarExpression [',' ...]` |
| [Proje](/azure/kusto/query/projectoperator)                  | Belirtilen sıraya dahil olacak sütunları seçer | `T | project ColumnName [= Expression] [, ...]` <br> Veya <br> `T | project [ColumnName | (ColumnName[,]) =] Expression [, ...]` |
| [proje-away](/azure/kusto/query/projectawayoperator)         | Çıktıdan hariç tutmak için sütunları seçer | `T | project-away ColumnNameOrPattern [, ...]` |
| [Genişlet -mek](/azure/kusto/query/extendoperator)                    | Hesaplanan bir sütun oluşturur ve sonuç kümesine ekler | `T | extend [ColumnName | (ColumnName[, ...]) =] Expression [, ...]` |
| **Veri Kümesini Sıralama ve Toplama**                 |**_Verileri anlamlı şekillerde sıralayarak veya gruplayarak yeniden yapılandırma_**|                  |
| [Sıralama](/azure/kusto/query/sortoperator)                        | Giriş tablosunun satırlarını artan veya azalan sırada bir veya daha fazla sütuna göre sıralar | `T | sort by expression1 [asc|desc], expression2 [asc|desc], …` |
| [Sayfanın Üstü](/azure/kusto/query/topoperator)                          | Veri kümesi kullanılarak sıralandığında veri kümesinin ilk N satırlarını döndürür`by` | `T | top numberOfRows by expression [asc|desc] [nulls first|last]` |
| [Özetle](/azure/kusto/query/summarizeoperator)              | Satırları `by` grup sütunlarına göre gruplave her grup üzerindeki toplamaları hesaplar | `T | summarize [[Column =] Aggregation [, ...]] [by [Column =] GroupExpression [, ...]]` |
| [count](/azure/kusto/query/countoperator)                       | Giriş tablosundaki kayıtları sayar (örneğin, T)<br>Bu işleç için steno`summarize count() `| `T | count` |
| [Katılın](/azure/kusto/query/joinoperator)                        | Her tablodan belirtilen sütun(lar) değerlerini eşleştirerek yeni bir tablo oluşturmak için iki tablonun satırlarını birleştirir. Tam bir birleştirme türünü `flouter`destekler: , `leftantisemi` `leftouter`, `leftsemi` `inner` `innerunique` `leftanti` `rightanti`, `rightantisemi` `rightouter`, , , , , ,`rightsemi` | `LeftTable | join [JoinParameters] ( RightTable ) on Attributes` |
| [Birliği](/azure/kusto/query/unionoperator)                      | İki veya daha fazla tablo alır ve tüm satırlarını döndürür | `[T1] | union [T2], [T3], …` |
| [Aralığı](/azure/kusto/query/rangeoperator)                      | Bir dizi değere sahip bir tablo oluşturur | `range columnName from start to stop step step` |
| **Verileri Biçimlendir**                                 | **_Verileri yararlı bir şekilde çıktıya yeniden yapılandırma_** | |
| [Arama](/azure/kusto/query/lookupoperator)                    | Boyut tablosundaki değerlerle olgu tablosunun sütunlarını genişletir | `T1 | lookup [kind = (leftouter|inner)] ( T2 ) on Attributes` |
| [mv-genişletmek](/azure/kusto/query/mvexpandoperator)               | Dinamik dizileri satıra dönüştürür (çok değerli genişletme) | `T | mv-expand Column` |
| [Ayrıştı -rma](/azure/kusto/query/parseoperator)                      | Dize ifadesini değerlendirir ve değerini bir veya daha fazla hesaplanan sütuna ayrışturır. Yapılandırılmamış verileri yapılandırmak için kullanın. | `T | parse [kind=regex  [flags=regex_flags] |simple|relaxed] Expression with * (StringConstant ColumnName [: ColumnType]) *...` |
| [make serisi](/azure/kusto/query/make-seriesoperator)          | Belirtilen eksen boyunca belirtilen toplu değerler serisi oluşturur | `T | make-series [MakeSeriesParamters] [Column =] Aggregation [default = DefaultValue] [, ...] on AxisColumn from start to end step step [by [Column =] GroupExpression [, ...]]` |
| [Izin](/azure/kusto/query/letstatement)                         | Bir adı, bağlı değerine başvurabilen ifadelere bağlar. Değerler, sorgunun bir parçası olarak geçici işlevler oluşturmak için lambda ifadeleri olabilir. Sonuçları `let` yeni bir tablo gibi görünen tablolar üzerinde ifadeler oluşturmak için kullanın. | `let Name = ScalarExpression | TabularExpression | FunctionDefinitionExpression` |
| **Genel**                                     | **_Çeşitli operasyonlar ve fonksiyon_** | |
| [Çağırmak](/azure/kusto/query/invokeoperator)                    | Giriş olarak aldığı tablodaki işlevi çalıştırın. | `T | invoke function([param1, param2])` |
| [pluginName'yi değerlendirin](/azure/kusto/query/evaluateoperator)     | Sorgu dili uzantılarını (eklentileri) değerlendirir | `[T |] evaluate [ evaluateParameters ] PluginName ( [PluginArg1 [, PluginArg2]... )` |
| **Görsel -leştirme**                               | **_Verileri grafik biçiminde görüntüleyen işlemler_** | |
| [Render](/azure/kusto/query/renderoperator) | Sonuçları grafik çıktı olarak işler | `T | render Visualization [with (PropertyName = PropertyValue [, ...] )]` |
