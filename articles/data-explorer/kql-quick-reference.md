---
title: KQL hızlı başvurusu
description: Yararlı KQL işlevlerinin ve sözdizimi örnekleri içeren tanımlarının listesi.
author: orspod
ms.author: orspodek
ms.reviewer: ''
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/19/2020
ms.openlocfilehash: e7196ccccb1203ce56b2e53fa358d091374cd3f8
ms.sourcegitcommit: d322d0a9d9479dbd473eae239c43707ac2c77a77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79139088"
---
# <a name="kql-quick-reference"></a>KQL hızlı başvurusu

Bu makalede, kusto sorgu dilini kullanmaya başlamanıza yardımcı olacak işlevlerin ve açıklamalarının bir listesi gösterilir.

| İşleç/Işlev                               | Açıklama                           | Sözdizimi                                           |
| :---------------------------------------------- | :------------------------------------ |:-------------------------------------------------|
|**Filtre/arama/koşul**                      |**_Filtreleyerek veya arayarak ilgili verileri bulma_** |                      |
| [olmadığı](/azure/kusto/query/whereoperator)                      | Belirli bir koşuldaki filtreler           | `T | where Predicate`                         |
| [Burada içerir/sahip](/azure/kusto/query/whereoperator)        | `Contains`: herhangi bir alt dize eşleşmesi arar <br> `Has`: belirli bir sözcüğe bakar (daha iyi performans)  | `T | where col1 contains/has "[search term]"`|
| [aramanız](/azure/kusto/query/searchoperator)                    | Tablodaki tüm sütunları bir değere göre arar | `[TabularSource |] search [kind=CaseSensitivity] [in (TableSources)] SearchPredicate` |
| [almanız](/azure/kusto/query/takeoperator)                        | Belirtilen sayıda kayıt döndürür. Sorguyu test etmek için kullanın<br>**_Note_** : `_take`_ ve `_limit`_ eş anlamlılardır. | `T | take NumberOfRows` |
| [harflerini](/azure/kusto/query/casefunction)                        | Diğer sistemlerde if/then/elseşuna benzer bir koşul ekstresi ekler. | `case(predicate_1, then_1, predicate_2, then_2, predicate_3, then_3, else)` |
| [ayrı](/azure/kusto/query/distinctoperator)                | Giriş tablosunun belirtilen sütunlarının ayrı bir birleşimini içeren bir tablo oluşturur | `distinct [ColumnName], [ColumnName]` |
| **Tarih/saat**                                   |**_Tarih ve saat işlevleri kullanan işlemler_**               |                          |
|[önce](/azure/kusto/query/agofunction)                           | Sorgunun yürütüldüğü zamana göre zaman farkını döndürür. Örneğin, geçerli saatin okunmasından bir saat `ago(1h)`. | `ago(a_timespan)` |
| [format_datetime](/azure/kusto/query/format-datetimefunction)  | [Çeşitli tarih biçimlerindeki](/azure/kusto/query/format-datetimefunction#supported-formats)verileri döndürür. | `format_datetime(datetime , format)` |
| [bölme](/azure/kusto/query/binfunction)                          | Bir zaman çerçevesi içindeki tüm değerleri yuvarlar ve gruplar | `bin(value,roundTo)` |
| **Sütunları oluştur/kaldır**                   |**_Tablodaki sütunları ekleme veya kaldırma_** |                                                    |
| [yazdırdığımda](/azure/kusto/query/printoperator)                      | Bir veya daha fazla skaler ifadeye sahip tek bir satır çıkışı verir | `print [ColumnName =] ScalarExpression [',' ...]` |
| [Proje](/azure/kusto/query/projectoperator)                  | Belirtilen sıraya dahil edilecek sütunları seçer | `T | project ColumnName [= Expression] [, ...]` <br> Veya <br> `T | project [ColumnName | (ColumnName[,]) =] Expression [, ...]` |
| [Proje-dışarıda](/azure/kusto/query/projectawayoperator)         | Çıktıdan çıkarılacak sütunları seçer | `T | project-away ColumnNameOrPattern [, ...]` |
| [genişletmeyi](/azure/kusto/query/extendoperator)                    | Hesaplanmış bir sütun oluşturur ve sonuç kümesine ekler | `T | extend [ColumnName | (ColumnName[, ...]) =] Expression [, ...]` |
| **Veri kümesini sıralama ve toplama**                 |**_Verileri anlamlı yollarla sıralayarak veya gruplandırarak verileri yeniden yapılandır_**|                  |
| [düzenine](/azure/kusto/query/sortoperator)                        | Giriş tablosunun satırlarını artan veya azalan sırada bir veya daha fazla sütuna göre sıralar | `T | sort by expression1 [asc|desc], expression2 [asc|desc], …` |
| [Sayfanın Üstü](/azure/kusto/query/topoperator)                          | Veri kümesi `by` kullanılarak sıralandığında veri kümesinin ilk N satırını döndürür | `T | top numberOfRows by expression [asc|desc] [nulls first|last]` |
| [ölçütü](/azure/kusto/query/summarizeoperator)              | Satırları `by` grubu sütunlarına göre gruplandırır ve her grup için toplamaları hesaplar | `T | summarize [[Column =] Aggregation [, ...]] [by [Column =] GroupExpression [, ...]]` |
| [count](/azure/kusto/query/countoperator)                       | Giriş tablosundaki kayıtları sayar (örneğin, T)<br>Bu işleç `summarize count() ` için toplu özelliktir| `T | count` |
| [join](/azure/kusto/query/joinoperator)                        | Her tablodan belirtilen sütunların değerlerini eşleştirerek yeni bir tablo oluşturmak için iki tablonun satırlarını birleştirir. Tam bir tür birleştirmeyi destekler: `flouter`, `inner`, `innerunique`, `leftanti`, `leftantisemi`, `leftouter`, `leftsemi`, `rightanti`, `rightantisemi`, `rightouter`, `rightsemi` | `LeftTable | join [JoinParameters] ( RightTable ) on Attributes` |
| [birleşim](/azure/kusto/query/unionoperator)                      | İki veya daha fazla tablo alır ve tüm satırlarını döndürür | `[T1] | union [T2], [T3], …` |
| [aralığı](/azure/kusto/query/rangeoperator)                      | Aritmetik bir değer serisi içeren bir tablo oluşturur | `range columnName from start to stop step step` |
| **Verileri Biçimlendir**                                 | **_Verileri yararlı bir şekilde çıkışa yeniden yapılandır_** | |
| [Ma](/azure/kusto/query/lookupoperator)                    | Bir olgu tablosunun sütunlarını, bir Boyut tablosunda aranan değerlerle genişletir | `T1 | lookup [kind = (leftouter|inner)] ( T2 ) on Attributes` |
| [MV-Genişlet](/azure/kusto/query/mvexpandoperator)               | Dinamik dizileri satırlara dönüştürür (çoklu değer genişletmesi) | `T | mv-expand Column` |
| [MAZ](/azure/kusto/query/parseoperator)                      | Bir dize ifadesini değerlendirir ve değerini bir veya daha fazla hesaplanmış sütunda ayrıştırır. Yapılandırılmamış verileri yapılandırmak için kullanın. | `T | parse [kind=regex  [flags=regex_flags] |simple|relaxed] Expression with * (StringConstant ColumnName [: ColumnType]) *...` |
| [diziyi oluştur](/azure/kusto/query/make-seriesoperator)          | Belirtilen bir eksen üzerinde belirtilen toplanmış değerler serisini oluşturur | `T | make-series [MakeSeriesParamters] [Column =] Aggregation [default = DefaultValue] [, ...] on AxisColumn from start to end step step [by [Column =] GroupExpression [, ...]]` |
| [atalım](/azure/kusto/query/letstatement)                         | Bir adı, bağlı değerine başvurabilen ifadelere bağlar. Değerler, sorgunun bir parçası olarak geçici işlevler oluşturmak için lambda ifadeleri olabilir. Sonuçları yeni bir tabloya benzer olan tablolar üzerinde ifadeler oluşturmak için `let` kullanın. | `let Name = ScalarExpression | TabularExpression | FunctionDefinitionExpression` |
| **Genel**                                     | **_Çeşitli işlemler ve işlev_** | |
| [Resync](/azure/kusto/query/invokeoperator)                    | İşlevi giriş olarak aldığı tabloda çalıştırır. | `T | invoke function([param1, param2])` |
| [pluginName 'i değerlendir](/azure/kusto/query/evaluateoperator)     | Sorgu dili uzantılarını değerlendirir (eklentiler) | `[T |] evaluate [ evaluateParameters ] PluginName ( [PluginArg1 [, PluginArg2]... )` |
| **Ğiyle**                               | **_Verileri grafik biçimde görüntüleyen işlemler_** | |
| [işlenecek](/azure/kusto/query/renderoperator) | Sonuçları grafik çıkış olarak işler | `T | render Visualization [with (PropertyName = PropertyValue [, ...] )]` |
