---
title: OData karşılaştırma operatör referansı
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında OData karşılaştırma işleçlerini (eq, ne, gt, lt, ge ve le) kullanmak için sözdizimi ve başvuru belgeleri.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 62c8c93e07326e776cbe089042abc481544794bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113213"
---
# <a name="odata-comparison-operators-in-azure-cognitive-search---eq-ne-gt-lt-ge-and-le"></a>Azure Bilişsel Arama'daki OData `gt`karşılaştırma `lt` `ge`operatörleri - `eq`, `ne`, , , ve`le`

Azure Bilişsel Arama'daki [Bir OData filtre ifadesindeki](query-odata-filter-orderby-syntax.md) en temel işlem, bir alanı belirli bir değerle karşılaştırmaktır. Eşitlik karşılaştırması ve aralık karşılaştırması olmak üzere iki tür karşılaştırma mümkündür. Bir alanı sabit bir değerle karşılaştırmak için aşağıdaki işleçleri kullanabilirsiniz:

Eşitlik operatörleri:

- `eq`: Bir alanın sabit bir **değere eşit** olup olmadığını test edin
- `ne`: Bir alanın sabit bir **değere eşit olup olmadığını** test edin

Aralık operatörleri:

- `gt`: Bir alanın sabit bir **değerden büyük** olup olmadığını test edin
- `lt`: Bir alanın sabit bir **değerden küçük** olup olmadığını test edin
- `ge`: Bir alanın sabit bir **değerden büyük mü yoksa eşit** mi olduğunu test edin
- `le`: Bir alanın sabit bir **değerden küçük mü yoksa eşit** mi olduğunu test edin

Bir alanın belirli bir değer aralığında olup olmadığını sınamak için aralık işleçlerini [mantıksal işleçlerle](search-query-odata-logical-operators.md) birlikte kullanabilirsiniz. Bu makalenin sonraki [örneklerine](#examples) bakın.

> [!NOTE]
> İsterseniz, operatörün sol tarafına sabit değeri ve alan adını sağ tarafa koyabilirsiniz. Aralık işleçleri için karşılaştırmanın anlamı tersine çevrilir. Örneğin, sabit değer soldaysa, `gt` sabit değerin alandan büyük olup olmadığını sınar. Karşılaştırma `geo.distance`işleçlerini, bir işlevin sonucunu bir değerle karşılaştırmak için de kullanabilirsiniz. Boolean işlevleri için, `search.ismatch`sonucu karşılaştırmak `true` `false` isteğe bağlıdır.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur Formu),](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)karşılaştırma işleçlerini kullanan bir OData ifadesinin dilbilgisini tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
comparison_expression ::=
    variable_or_function comparison_operator constant |
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#comparison_expression)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

Karşılaştırma ifadelerinin iki biçimi vardır. Aralarındaki tek fark, sabitin operatörün sol veya sağ tarafında görünüp görünmediğidir. İşleticinin diğer tarafındaki ifade bir **değişken** veya işlev çağrısı olmalıdır. Bir değişken bir alan adı veya [bir lambda ifade](search-query-odata-collection-operators.md)durumunda bir aralık değişkeni olabilir.

## <a name="data-types-for-comparisons"></a>Karşılaştırmalar için veri türleri

Karşılaştırma işlecinin her iki tarafındaki veri türleri uyumlu olmalıdır. Örneğin, sol taraf bir tür `Edm.DateTimeOffset`alanı ise, sağ tarafın bir tarih-saat sabiti olmalıdır. Sayısal veri türleri daha esnektir. Aşağıdaki tabloda açıklandığı gibi, herhangi bir sayısal türün değişkenlerini ve işlevlerini başka bir sayısal türdeki sabitlerle birkaç sınırlamayla karşılaştırabilirsiniz.

| Değişken veya işlev türü | Sabit değer türü | Sınırlamalar |
| --- | --- | --- |
| `Edm.Double` | `Edm.Double` | Karşılaştırma [için `NaN` özel kurallara](#special-case-nan) tabidir |
| `Edm.Double` | `Edm.Int64` | Sabit dönüştürülür `Edm.Double`, büyük büyüklükteki değerler için hassasiyet kaybına neden |
| `Edm.Double` | `Edm.Int32` | yok |
| `Edm.Int64` | `Edm.Double` | `NaN`Karşılaştırmalar , `-INF`, `INF` veya izin verilmez |
| `Edm.Int64` | `Edm.Int64` | yok |
| `Edm.Int64` | `Edm.Int32` | Sabit `Edm.Int64` karşılaştırmadan önce dönüştürülür |
| `Edm.Int32` | `Edm.Double` | `NaN`Karşılaştırmalar , `-INF`, `INF` veya izin verilmez |
| `Edm.Int32` | `Edm.Int64` | yok |
| `Edm.Int32` | `Edm.Int32` | yok |

Azure Bilişsel Arama REST API'nin bir `Edm.Int64` "HTTP `NaN`400: Kötü İstek" hatasını döndürecek tür alanını karşılaştırmak gibi, izin verilmeyen karşılaştırmalar için.

> [!IMPORTANT]
> Sayısal tür karşılaştırmaları esnek olsa da, sabit değerin karşılaştırıldığı değişken veya işlevle aynı veri türünde olması için filtrelere karşılaştırma yazmanızı önemle öneririz. Bu, özellikle, kesinlik kaybeden örtük dönüşümlerin mümkün olduğu kayan nokta ve tamsayı değerlerini karıştırırken önemlidir.

<a name="special-case-nan"></a>

### <a name="special-cases-for-null-and-nan"></a>Özel durumlar `null` için ve`NaN`

Karşılaştırma işleçlerini kullanırken, Azure Bilişsel Arama'daki tüm koleksiyon dışı alanların `null`potansiyel olarak . Aşağıdaki tablo, her iki tarafın da olabileceği `null`bir karşılaştırma ifadesi için tüm olası sonuçları gösterir:

| İşleç | Sonuç, yalnızca alan veya değişken`null` | Sonuç, yalnızca sabit`null` | Sonuç, hem alan hem de değişken ve sabit`null` |
| --- | --- | --- | --- |
| `gt` | `false` | HTTP 400: Kötü İstek hatası | HTTP 400: Kötü İstek hatası |
| `lt` | `false` | HTTP 400: Kötü İstek hatası | HTTP 400: Kötü İstek hatası |
| `ge` | `false` | HTTP 400: Kötü İstek hatası | HTTP 400: Kötü İstek hatası |
| `le` | `false` | HTTP 400: Kötü İstek hatası | HTTP 400: Kötü İstek hatası |
| `eq` | `false` | `false` | `true` |
| `ne` | `true` | `true` | `false` |

Özetle, `null` yalnızca kendisine eşittir ve diğer değerlerden daha az veya büyük değildir.

Dizininizin tür `Edm.Double` alanları varsa ve `NaN` bu alanlara değerler yüklerseniz, filtre yazarken bunun hesabını vermeniz gerekir. Azure Bilişsel Arama değerleri işlemek `NaN` için IEEE 754 standardını uygular ve bu değerlerle karşılaştırmalar aşağıdaki tabloda gösterildiği gibi belirgin olmayan sonuçlar üretir.

| İşleç | Sonuç en az bir operand olduğunda`NaN` |
| --- | --- |
| `gt` | `false` |
| `lt` | `false` |
| `ge` | `false` |
| `le` | `false` |
| `eq` | `false` |
| `ne` | `true` |

Özetle, `NaN` kendisi de dahil olmak üzere herhangi bir değere eşit değildir.

### <a name="comparing-geo-spatial-data"></a>Coğrafi uzamsal verileri karşılaştırma

Bir tür `Edm.GeographyPoint` alanını sabit bir değerle doğrudan karşılaştıramazsınız, `geo.distance` ancak işlevi kullanabilirsiniz. Bu işlev, sabit `Edm.Double`jeo-uzamsal koordinatlardan gelen uzaklığı temel alan filtrelemek için sayısal bir sabitle karşılaştırabilmeniz için bir tür değeri döndürür. Aşağıdaki [örneklere](#examples) bakın.

### <a name="comparing-string-data"></a>Dize verilerini karşılaştırma

Dizeleri `eq` ve `ne` işleçleri kullanarak tam eşleşmeleri için filtreler de karşılaştırılabilir. Bu karşılaştırmalar büyük/küçük harf duyarlıdır.

## <a name="examples"></a>Örnekler

Alanın 3 `Rating` ile 5 arasında olduğu belgeleri, dahil:

    Rating ge 3 and Rating le 5

`Location` Alanın verilen enlem ve boylamdan 2 kilometreden daha az olduğu belgeleri eşleştirin:

    geo.distance(Location, geography'POINT(-122.031577 47.578581)') lt 2.0

`LastRenovationDate` Alanın 1 Ocak 2015, gece yarısı UTC'sinden daha büyük veya eşit olduğu belgeleri eşleştirin:

    LastRenovationDate ge 2015-01-01T00:00:00.000Z

Alanın olmadığı `null` `Details/Sku` belgeleri eşleştirin:

    Details/Sku ne null

En az bir odanın "Deluxe Oda" türüne sahip olduğu `Rooms/Type` ve saha dizesinin filtreyle tam olarak eşleştiği otelleriçin belgeleri eşleştirin:

    Rooms/any(room: room/Type eq 'Deluxe Room')

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
