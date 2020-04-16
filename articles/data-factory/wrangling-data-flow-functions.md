---
title: Azure Veri Fabrikası'nda veri akışı dönüştürme işlevlerini çarpıtmak
description: Azure Veri Fabrikası'ndaki kullanılabilir çekişme veri akışı işlevlerine genel bakış
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: 7235e95e5b33fb931411a51796a8dbec96c46355
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417666"
---
# <a name="transformation-functions-in-wrangling-data-flow"></a>Çekişmeli veri akışında dönüştürme işlevleri

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Veri Fabrikası'nda veri akışı oluşturmak, bulut ölçeğinde kodsuz çevik veri hazırlama ve çekişme yapmanızı sağlar. Wrangling veri akışı [Power Query Online](https://docs.microsoft.com/powerquery-m/power-query-m-reference) ile tümleşir ve Power Query M işlevlerini kıvılcım yürütme yoluyla veri çekişmesi için kullanılabilir hale getirir. 

Şu anda tüm Power Query M işlevleri, yazarlık sırasında kullanılabilir olmasına rağmen veri çekişmesi için desteklenmemektedir. Çekişmeli veri akışlarınızı yaparken, bir işlev desteklenmezse aşağıdaki hata iletisi istenir:

`The wrangling data flow is invalid. Expression.Error: The transformation logic isn't supported. Please try a simpler expression`

Aşağıda desteklenen Güç Sorgusu M işlevlerinin bir listesi verilmiştir.

## <a name="column-management"></a>Sütun Yönetimi

* Seçim: [Tablo.SelectColumns](https://docs.microsoft.com/powerquery-m/table-selectcolumns)
* Kaldırma: [Tablo.RemoveColumns](https://docs.microsoft.com/powerquery-m/table-removecolumns)
* Yeniden Adlandırma: [Tablo.RenameColumns](https://docs.microsoft.com/powerquery-m/table-renamecolumns), [Tablo.PrefixColumns](https://docs.microsoft.com/powerquery-m/table-prefixcolumns), [Tablo.TransformColumnNames](https://docs.microsoft.com/powerquery-m/table-transformcolumnnames)
* Yeniden Sıralama: [Tablo.ReorderColumns](https://docs.microsoft.com/powerquery-m/table-reordercolumns)

## <a name="row-filtering"></a>Satır Filtreleme

Aşağıdaki koşullara filtre sağlamak için M fonksiyonu [Tablosu.SelectRows'u](https://docs.microsoft.com/powerquery-m/table-selectrows) kullanın:

* Eşitlik ve eşitsizlik
* Sayısal, metin ve tarih karşılaştırmaları (ancak DateTime değil)
* [Number.IsEven](https://docs.microsoft.com/powerquery-m/number-iseven)/[Odd](https://docs.microsoft.com/powerquery-m/number-iseven) gibi sayısal bilgiler
* [Text.Contains](https://docs.microsoft.com/powerquery-m/text-contains), [Text.StartsWith](https://docs.microsoft.com/powerquery-m/text-startswith)veya [Text.EndsWith](https://docs.microsoft.com/powerquery-m/text-endswith) kullanarak metin içeren metin
* Tüm 'IsIn' [Date işlevlerini](https://docs.microsoft.com/powerquery-m/date-functions)içeren tarih aralıkları ) 
* Bu kullanım ve veya veya koşullar ya da değil kombinasyonları

## <a name="adding-and-transforming-columns"></a>Sütun Ekleme ve Dönüştürme

Aşağıdaki M işlevleri sütun eklemek veya dönüştürmek: [Tablo.AddColumn](https://docs.microsoft.com/powerquery-m/table-addcolumn), [Tablo.TransformColumns](https://docs.microsoft.com/powerquery-m/table-transformcolumns), [Tablo.ReplaceValue](https://docs.microsoft.com/powerquery-m/table-replacevalue), [Tablo.DuplicateColumn](https://docs.microsoft.com/powerquery-m/table-duplicatecolumn). Aşağıda desteklenen dönüşüm işlevleri verilmiştir.

* Sayısal aritmetik
* Metin birleşmesi
* Tarih ve Saat Aritmetik (Aritmetik işleçler, [Date.AddDays](https://docs.microsoft.com/powerquery-m/date-adddays), [Date.AddMonths](https://docs.microsoft.com/powerquery-m/date-addmonths), [Date.AddQuarters](https://docs.microsoft.com/powerquery-m/date-addquarters), [Date.AddWeeks](https://docs.microsoft.com/powerquery-m/date-addweeks), [Date.AddYears](https://docs.microsoft.com/powerquery-m/date-addyears))
* Süreler tarih ve saat aritmetiği için kullanılabilir, ancak lavaboya yazılmadan önce başka bir türe dönüştürülmelidir (Aritmetik işleçler, [#duration](https://docs.microsoft.com/powerquery-m/sharpduration), [Süre.Gün,](https://docs.microsoft.com/powerquery-m/duration-days) [Süre.Saat](https://docs.microsoft.com/powerquery-m/duration-hours), [Süre.Dakika](https://docs.microsoft.com/powerquery-m/duration-minutes), [Süre.Saniye,](https://docs.microsoft.com/powerquery-m/duration-seconds) [Süre.TotalGün](https://docs.microsoft.com/powerquery-m/duration-totaldays), [Süre.TotalHours](https://docs.microsoft.com/powerquery-m/duration-totalhours), [Süre.Toplam Dakika](https://docs.microsoft.com/powerquery-m/duration-totalminutes), [Süre.Toplam Saniyeler](https://docs.microsoft.com/powerquery-m/duration-totalseconds))    
* Çoğu standart, bilimsel ve trigonometrik sayısal fonksiyonlar [(Operasyonlar](https://docs.microsoft.com/powerquery-m/number-functions#operations)altındaki tüm fonksiyonlar , [Yuvarlama](https://docs.microsoft.com/powerquery-m/number-functions#rounding)ve [Trigonometri](https://docs.microsoft.com/powerquery-m/number-functions#trigonometry) *hariç* Number.Factorial, Number.Permütasyonlar ve Number.Kombinasyonları)
* Değiştirme ([Replacer.ReplaceText](https://docs.microsoft.com/powerquery-m/replacer-replacetext), [Replacer.ReplaceValue](https://docs.microsoft.com/powerquery-m/replacer-replacevalue), [Text.Replace](https://docs.microsoft.com/powerquery-m/text-replace), [Text.Remove](https://docs.microsoft.com/powerquery-m/text-remove))
* Konumsal metin çıkarma ([Text.PositionOf](https://docs.microsoft.com/powerquery-m/text-positionof), [Text.Length](https://docs.microsoft.com/powerquery-m/text-length), [Text.Start](https://docs.microsoft.com/powerquery-m/text-start), [Text.End](https://docs.microsoft.com/powerquery-m/text-end), [Text.Middle](https://docs.microsoft.com/powerquery-m/text-middle), [Text.ReplaceRange](https://docs.microsoft.com/powerquery-m/text-replacerange), [Text.RemoveRange](https://docs.microsoft.com/powerquery-m/text-removerange))
* Temel metin biçimlendirme ([Text.Lower](https://docs.microsoft.com/powerquery-m/text-lower), [Text.Upper](https://docs.microsoft.com/powerquery-m/text-upper), [Text.Trim](https://docs.microsoft.com/powerquery-m/text-trim)/[Başlat](https://docs.microsoft.com/powerquery-m/text-trimstart)/[Sonu](https://docs.microsoft.com/powerquery-m/text-trimend), [Text.PadStart](https://docs.microsoft.com/powerquery-m/text-padstart)/[End](https://docs.microsoft.com/powerquery-m/text-padend), [Text.Reverse](https://docs.microsoft.com/powerquery-m/text-reverse))
* Tarih/Saat Fonksiyonları ([Date.Day](https://docs.microsoft.com/powerquery-m/date-day), [Date.Month](https://docs.microsoft.com/powerquery-m/date-month), [Date.Year](https://docs.microsoft.com/powerquery-m/date-year) [Time.Hour](https://docs.microsoft.com/powerquery-m/time-hour), [Time.Minute](https://docs.microsoft.com/powerquery-m/time-minute), [Time.Second](https://docs.microsoft.com/powerquery-m/time-second), [Date.DayOfWeek](https://docs.microsoft.com/powerquery-m/date-dayofweek), [Date.DayOfYear](https://docs.microsoft.com/powerquery-m/date-dayofyear), [Date.DaysinMonth](https://docs.microsoft.com/powerquery-m/date-daysinmonth))
* If ifadeleri (ancak dalları eşleşen türleri olmalıdır)
* Mantıksal sütun olarak satır filtreleri
* Sayı, metin, mantıksal, tarih ve tarih sabitleri

<a name="mergingjoining-tables"></a>Birleştirme/Birleştirme tabloları
----------------------
* Power Query iç içe bir birleştirme oluşturur (Table.NestedJoin; kullanıcılar da el ile [Tablo.AddJoinColumn](https://docs.microsoft.com/powerquery-m/table-addjoincolumn)yazabilirsiniz).
    Kullanıcılar daha sonra iç içe geçen birleştirme sütununa iç içe geçmemiş bir birleştirme (Table.ExpandTableColumn, başka bir bağlamda desteklenmeyen) olarak genişletmelidir.
* M fonksiyonu [Table.Join](https://docs.microsoft.com/powerquery-m/table-join) ek bir genişletme adımı gereksinimini önlemek için doğrudan yazılabilir, ancak kullanıcı birleştirilmiş tablolar arasında yinelenen sütun adları olmadığından emin olmalıdır
* Desteklenen Join Kinds: [İç](https://docs.microsoft.com/powerquery-m/joinkind-inner), [LeftOuter](https://docs.microsoft.com/powerquery-m/joinkind-leftouter), [RightOuter](https://docs.microsoft.com/powerquery-m/joinkind-rightouter), [FullOuter](https://docs.microsoft.com/powerquery-m/joinkind-fullouter)
* Hem [Value.Equals](https://docs.microsoft.com/powerquery-m/value-equals) hem de [Value.NullableEquals](https://docs.microsoft.com/powerquery-m/value-nullableequals) anahtar eşitlik karşılaştırıcıları olarak desteklenir

## <a name="group-by"></a>Gruplandırma ölçütü:

Değerleri toplamak için [Tablo.Grup'u](https://docs.microsoft.com/powerquery-m/table-group) kullanın.
* Toplama işlevi ile kullanılmalıdır
* Desteklenen toplama işlevleri: [Tablo.RowCount](https://docs.microsoft.com/powerquery-m/table-rowcount), [List.Sum](https://docs.microsoft.com/powerquery-m/list-sum), [List.Count](https://docs.microsoft.com/powerquery-m/list-count), [List.Average](https://docs.microsoft.com/powerquery-m/list-average), [List.Min](https://docs.microsoft.com/powerquery-m/list-min), [List.Max](https://docs.microsoft.com/powerquery-m/list-max), [List.StandardDeviation](https://docs.microsoft.com/powerquery-m/list-standarddeviation), [List.First](https://docs.microsoft.com/powerquery-m/list-first), [List.Last](https://docs.microsoft.com/powerquery-m/list-last)

## <a name="sorting"></a>Sıralama

Değerleri sıralamak için [Tablo.Sıralama'yı](https://docs.microsoft.com/powerquery-m/table-sort) kullanın.

## <a name="reducing-rows"></a>Satırları Azaltma

Üst tut ve kaldır (Aralığı Tut (karşılık gelen M işlevleri, yalnızca destekleyici sayıları değil, koşullar: [Table.FirstN](https://docs.microsoft.com/powerquery-m/table-firstn), [Table.Skip](https://docs.microsoft.com/powerquery-m/table-skip), [Table.RemoveFirstN](https://docs.microsoft.com/powerquery-m/table-removefirstn), [Table.Range](https://docs.microsoft.com/powerquery-m/table-range), [Table.MinN](https://docs.microsoft.com/powerquery-m/table-minn), [Table.MaxN](https://docs.microsoft.com/powerquery-m/table-maxn))

## <a name="known-unsupported-functions"></a>Bilinen desteklenmeyen işlevler

| İşlev | Durum |
| -- | -- |
| Table.PromoteHeaders | Desteklenmiyor. Aynı sonuç, veri kümesinde "İlk satır üstbilgi olarak" ayarlayarak elde edilebilir. |
| Table.CombineColumns | Bu, doğrudan desteklenmeyen, ancak verilen iki sütunu biraraya getiren yeni bir sütun ekleyerek elde edilebilen yaygın bir senaryodur.  Örneğin, Table.AddColumn(RemoveEmailColumn, "Name", her [FirstName] & " & [Soyad]) |
| Table.TransformColumnTypes | Bu çoğu durumda desteklenir. Aşağıdaki senaryolar desteklenmez: dizeyi para birimi türüne dönüştürme, dizeyi zaman türüne dönüştürme, dizeyi Yüzde türüne dönüştürme. |
| Table.NestedJoin | Yalnızca birleştirme yapmak bir doğrulama hatasına neden olur. Çalışabilmesi için sütunların genişletilmesi gerekir. |
| Table.Distinct | Yinelenen satırları kaldır desteklenmez. |
| Table.RemoveLastN | Alt satırları kaldır desteklenmez. |
| Table.RowCount | Desteklenmez, ancak tüm hücrelerin boş olduğu bir ekle sütunuyla (durum sütunu kullanılabilir) ve daha sonra bu sütunda grup kullanılarak elde edilebilir. Tablo.Grup desteklenir. | 
| Satır düzeyi hata işleme | Satır düzeyi hata işleme şu anda desteklenmez. Örneğin, bir sütundan sayısal olmayan değerleri filtrelemek için metin sütununa dönüştürme yaklaşımından biri olacaktır. Dönüşüm sağlayamayan her hücre bir hata durumunda olacak ve filtre edilmesi gerekir. Bu senaryo, veri akışı nda mümkün değildir. |
| Table.Transpose | Desteklenmiyor |
| Table.Pivot | Desteklenmiyor |

## <a name="next-steps"></a>Sonraki adımlar

[Nasıl çekişmeli bir veri akışı oluşturabilirsiniz](wrangling-data-flow-tutorial.md)öğrenin.