---
title: Azure Data Factory 'de wrangling veri akışı dönüştürme işlevleri
description: Azure Data Factory içindeki kullanılabilir denetimi veri akışı işlevlerine genel bakış
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: 3ee7761d43710e0833eb8002851e286ce5449983
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92636128"
---
# <a name="transformation-functions-in-wrangling-data-flow"></a>Denetimi veri akışındaki dönüştürme işlevleri

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Data Factory 'de denetimi veri akışı, bulut ölçeğinde kod ücretsiz çevik veri hazırlama ve denetimi kullanmanıza olanak sağlar. Denetimi veri akışı, [çevrimiçi Power Query](/powerquery-m/power-query-m-reference) ile tümleşir ve Spark yürütmesi aracılığıyla veriler için Power Query M işlevlerini kullanılabilir hale getirir. 

> [!NOTE]
> Wrangling veri akışı şu anda genel önizlemede bir avılamaz

Şu anda tüm Power Query M işlevleri, yazma sırasında kullanılabilir olmasına rağmen veri denetimi için desteklenmez. Denetimi veri akışlarınızı oluştururken, bir işlev desteklenmiyorsa aşağıdaki hata iletisiyle karşılaşırsınız:

`The wrangling data flow is invalid. Expression.Error: The transformation logic isn't supported. Please try a simpler expression`

Desteklenen Power Query d işlevlerinin listesi aşağıda verilmiştir.

## <a name="column-management"></a>Sütun yönetimi

* Seçim: [Table. SelectColumns](/powerquery-m/table-selectcolumns)
* Kaldırma: [Table. RemoveColumns](/powerquery-m/table-removecolumns)
* Yeniden adlandırma: [Table. RenameColumns](/powerquery-m/table-renamecolumns), [Table. PrefixColumns](/powerquery-m/table-prefixcolumns), [Table. transformcolumnnames](/powerquery-m/table-transformcolumnnames)
* Yeniden sıralama: [Table. ReorderColumns](/powerquery-m/table-reordercolumns)

## <a name="row-filtering"></a>Satır filtreleme

Aşağıdaki koşullara göre filtrelemek için, [. SelectRows.](/powerquery-m/table-selectrows) ini kullanın:

* Eşitlik ve eşitsizlik
* Sayısal, metin ve Tarih karşılaştırmaları (DateTime değil)
* Sayı gibi sayısal bilgiler [. ıyedi](/powerquery-m/number-iseven) / [tek](/powerquery-m/number-iseven)
* [Metin. Contains](/powerquery-m/text-contains), [Text. StartsWith](/powerquery-m/text-startswith)veya [Text. EndsWith](/powerquery-m/text-endswith) kullanılarak metin kapsama
* Tüm ' ısin' [Tarih işlevlerini](/powerquery-m/date-functions)içeren tarih aralıkları) 
* Bu koşulların ve, veya değil, koşullarını kullanma

## <a name="adding-and-transforming-columns"></a>Sütun ekleme ve dönüştürme

Şu n işlevleri sütun ekler veya dönüştürür: [Table. AddColumn](/powerquery-m/table-addcolumn), [Table. transformcolumns](/powerquery-m/table-transformcolumns), [Table. Replacevalue](/powerquery-m/table-replacevalue), [Table. duplicatecolumn](/powerquery-m/table-duplicatecolumn). Desteklenen dönüştürme işlevleri aşağıda verilmiştir.

* Sayısal aritmetik
* Metin birleştirme
* Tarih ve saat aritmetiği (aritmetik işleçler, [Tarih. AddDays](/powerquery-m/date-adddays), [date. addaylar](/powerquery-m/date-addmonths), [date. Addçeyrekler](/powerquery-m/date-addquarters), [date. addhaftalar](/powerquery-m/date-addweeks), [date. addyıllar](/powerquery-m/date-addyears))
* Süreler tarih ve saat aritmetiği için kullanılabilir, ancak bir havuza yazılmadan önce başka bir türe dönüştürülmelidir (aritmetik işleçler, [#duration](/powerquery-m/sharpduration), [Duration. Days](/powerquery-m/duration-days), [Duration. hours](/powerquery-m/duration-hours), [Duration. dakika](/powerquery-m/duration-minutes), [Duration. saniye](/powerquery-m/duration-seconds), [Duration. TotalDays](/powerquery-m/duration-totaldays), Duration. [TotalHours](/powerquery-m/duration-totalhours), [Duration. TotalMinutes](/powerquery-m/duration-totalminutes), [Duration. TotalSeconds](/powerquery-m/duration-totalseconds))    
* En standart, bilimsel ve trigonometrik sayısal işlevler (sayı *hariç* [işlemler](/powerquery-m/number-functions#operations), [yuvarlama](/powerquery-m/number-functions#rounding)ve [trigonometrik metri](/powerquery-m/number-functions#trigonometry) , sayı. çarpınmalar ve sayı. birleşimler)
* Değiştirme (tekrar[cer. ReplaceText](/powerquery-m/replacer-replacetext), tekrar [cer. replacevalue](/powerquery-m/replacer-replacevalue), [Text. Replace](/powerquery-m/text-replace), [Text. Remove](/powerquery-m/text-remove))
* Konumsal metin ayıklama ([Text. PositionOf](/powerquery-m/text-positionof), [Text. length](/powerquery-m/text-length), [Text. Start](/powerquery-m/text-start), [Text. End](/powerquery-m/text-end), [Text. ortadaki](/powerquery-m/text-middle), [Text. değiştirici serange](/powerquery-m/text-replacerange), [Text. RemoveRange](/powerquery-m/text-removerange))
* Temel metin biçimlendirme ([Text. Lower](/powerquery-m/text-lower), [Text. Upper](/powerquery-m/text-upper), [Text. Trim](/powerquery-m/text-trim) / [Start](/powerquery-m/text-trimstart) / [End](/powerquery-m/text-trimend), [Text. asma start](/powerquery-m/text-padstart) / [End](/powerquery-m/text-padend), [Text. Reverse](/powerquery-m/text-reverse))
* Tarih/saat Işlevleri ([Tarih. gün](/powerquery-m/date-day), [Tarih. ay](/powerquery-m/date-month), [Tarih. yıl](/powerquery-m/date-year) [saat. saat](/powerquery-m/time-hour), [saat. dakika](/powerquery-m/time-minute), [saat. saniye](/powerquery-m/time-second), [Tarih. DayOfWeek](/powerquery-m/date-dayofweek), [Tarih. DayOfYear](/powerquery-m/date-dayofyear), [date. DaysInMonth](/powerquery-m/date-daysinmonth))
* If ifadeleri (ancak dallar eşleşen türlere sahip olmalıdır)
* Mantıksal sütun olarak satır filtreleri
* Sayı, metin, mantıksal, tarih ve tarih/saat sabitleri

<a name="mergingjoining-tables"></a>Tabloları birleştirme/birleştirme
----------------------
* Power Query iç içe bir JOIN (Table. Nestedjoın) oluşturacak ve kullanıcılar el ile [Table. AddJoinColumn](/powerquery-m/table-addjoincolumn)yazabilir.
    Kullanıcılar daha sonra iç içe JOIN sütununu iç içe olmayan bir birleşime genişletmelidir (başka bir bağlamda desteklenmez).
* M işlevi   [Table. JOIN](/powerquery-m/table-join) , ek bir genişletme adımının gereksinimini ortadan kaldırmak için doğrudan yazılabilir, ancak kullanıcının birleştirilmiş tablolar arasında yinelenen sütun adları olmadığından emin olması gerekir
* Desteklenen birleşim türleri:   [iç](/powerquery-m/joinkind-inner),   [soltouter](/powerquery-m/joinkind-leftouter),   [sağtouter](/powerquery-m/joinkind-rightouter),   [FullOuter](/powerquery-m/joinkind-fullouter)
* [Değer. Equals](/powerquery-m/value-equals) ve [Value. nullableequals](/powerquery-m/value-nullableequals) , anahtar eşitlik Karşılaştırıcılar olarak desteklenir

## <a name="group-by"></a>Gruplandırma ölçütü:

Değerleri toplamak için [Table. Group](/powerquery-m/table-group) kullanın.
* Bir toplama işleviyle birlikte kullanılması gerekir
* Desteklenen toplama işlevleri:   [Table. RowCount](/powerquery-m/table-rowcount),   [List. Sum](/powerquery-m/list-sum),   [List. Count](/powerquery-m/list-count),   [List. Average](/powerquery-m/list-average),   [List. min](/powerquery-m/list-min),   [List. Max](/powerquery-m/list-max),   [List. standardsapması](/powerquery-m/list-standarddeviation),   [List. First](/powerquery-m/list-first),   [List. Last](/powerquery-m/list-last)

## <a name="sorting"></a>Sıralama

Değerleri sıralamak için [Table. Sort](/powerquery-m/table-sort) kullanın.

## <a name="reducing-rows"></a>Satırları azaltma

Bağımsız tut ve çıkar, aralığı tut (karşılık gelen k işlevleri, yalnızca destekleme sayısı, koşullar değil: [Table. FirstN](/powerquery-m/table-firstn), [Table. Skip](/powerquery-m/table-skip), [Table. Removefirstn](/powerquery-m/table-removefirstn), [Table. Range](/powerquery-m/table-range), [Table. Minn](/powerquery-m/table-minn), [Table. MAXn](/powerquery-m/table-maxn))

## <a name="known-unsupported-functions"></a>Bilinen desteklenmeyen işlevler

| İşlev | Durum |
| -- | -- |
| Table.PromoteHeaders | Desteklenmez. Aynı sonuç, veri kümesindeki "Ilk satır üstbilgi olarak" ayarlanarak elde edilebilir. |
| Table.CombineColumns | Bu, doğrudan desteklenmeyen ancak verilen iki sütunu birleştiren yeni bir sütun eklenerek elde edilen yaygın bir senaryodur.  Örneğin, Table. AddColumn (RemoveEmailColumn, "ad", her [FirstName] & "" & [LastName]) |
| Table.TransformColumnTypes | Çoğu durumda bu desteklenir. Aşağıdaki senaryolar desteklenmez: dizeyi para birimi türüne dönüştürme, dizeyi zaman türüne dönüştürme, dizeyi yüzde türüne dönüştürme. |
| Table.NestedJoin | Yalnızca bir birleşimi yapmanız doğrulama hatasına neden olur. Çalışması için sütunların genişletilmesi gerekir. |
| Table.Distinct | Yinelenen satırları kaldırma desteklenmiyor. |
| Table.RemoveLastN | Alt satırları kaldır desteklenmiyor. |
| Table.RowCount | Desteklenmez, ancak tüm hücreler boş olan bir Ekle sütunuyla elde edilebilir (koşul sütunu kullanılabilir) ve ardından bu sütunda Group By kullanılıyor. Table. Group desteklenir. | 
| Satır düzeyi hata işleme | Satır düzeyi hata işleme Şu anda desteklenmiyor. Örneğin, bir sütundan sayısal olmayan değerleri filtrelemek için, bir yaklaşım metin sütununu bir sayıya dönüştürmelidir. Dönüştürülemezse her hücre bir hata durumunda olur ve filtrelenebilir. Bu senaryo, denetimi veri akışında mümkün değildir. |
| Table.Transpose | Desteklenmez |
| Table.Pivot | Desteklenmez |

## <a name="next-steps"></a>Sonraki adımlar

[Bir denetimi veri akışı oluşturmayı](wrangling-data-flow-tutorial.md)öğrenin.