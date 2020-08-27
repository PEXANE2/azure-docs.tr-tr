---
title: OData karşılaştırma işleci başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında OData karşılaştırma işleçleri (EQ, ne, gt, lt, GE ve Le) kullanılmasına yönelik sözdizimi ve başvuru belgeleri.
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
ms.openlocfilehash: fc5803f96c30ea1df362676aa8c4104bb0b69db3
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934880"
---
# <a name="odata-comparison-operators-in-azure-cognitive-search---eq-ne-gt-lt-ge-and-le"></a>Azure bilişsel arama- `eq` , `ne` ,, `gt` `lt` , `ge` ve içinde OData karşılaştırma işleçleri `le`

Azure Bilişsel Arama 'deki bir [OData filtre ifadesinde](query-odata-filter-orderby-syntax.md) en temel işlem, bir alanı verilen değerle karşılaştırmaktır. İki tür karşılaştırma yapılabilir--eşitlik karşılaştırması ve Aralık karşılaştırması. Bir alanı sabit değerle karşılaştırmak için aşağıdaki işleçleri kullanabilirsiniz:

Eşitlik işleçleri:

- `eq`: Bir alanın sabit değere **eşit** olup olmadığını test edin
- `ne`: Bir alanın sabit değere **eşit** olup olmadığını test edin

Aralık işleçleri:

- `gt`: Bir alanın sabit değerden **büyük** olup olmadığını test edin
- `lt`: Bir alanın sabit değerden **küçük** olup olmadığını test edin
- `ge`: Bir alanın sabit değere **eşit veya ondan büyük** olup olmadığını test edin
- `le`: Bir alanın sabit değere **eşit veya ondan küçük** olup olmadığını test edin

Bir alanın belirli bir değer aralığı içinde olup olmadığını test etmek için, Aralık işleçlerini [mantıksal işleçlerle](search-query-odata-logical-operators.md) birlikte kullanabilirsiniz. Bu makalenin devamındaki [örneklere](#examples) bakın.

> [!NOTE]
> İsterseniz, sabit değeri işlecin sol tarafına ve alan adını sağ tarafta koyabilirsiniz. Aralık işleçleri için karşılaştırma anlamı tersine çevrilir. Örneğin, sabit değer Solsa, `gt` sabit değerinin alandan büyük olup olmadığını test edecektir. Karşılaştırma işleçlerini, gibi bir işlevin sonucunu karşılaştırmak için de kullanabilirsiniz `geo.distance` . Gibi Boole işlevleri için `search.ismatch` , sonucu veya ile karşılaştırma `true` `false` isteğe bağlıdır.

## <a name="syntax"></a>Syntax

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)), karşılaştırma işleçlerini kullanan bir OData ifadesinin dilbilgisini tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
comparison_expression ::=
    variable_or_function comparison_operator constant |
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'
```

Etkileşimli bir sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#comparison_expression)

> [!NOTE]
> Tüm EBNF için bkz. [Azure bilişsel arama Için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md) .

İki karşılaştırma ifadesi biçimi vardır. Bunlar arasındaki tek fark, işlecin, işlecin sol veya sağ tarafında görünüp görüntülenmemesidir. İşlecin diğer tarafındaki ifade bir **değişken** veya işlev çağrısı olmalıdır. Bir değişken, bir alan adı ya da bir [lambda ifadesi](search-query-odata-collection-operators.md)söz konusu olduğunda bir Aralık değişkeni olabilir.

## <a name="data-types-for-comparisons"></a>Karşılaştırmalar için veri türleri

Karşılaştırma işlecinin her iki tarafındaki veri türleri uyumlu olmalıdır. Örneğin, sol kenar türünde bir alan ise `Edm.DateTimeOffset` , sağ kenar bir tarih-saat sabiti olmalıdır. Sayısal veri türleri daha esnektir. Aşağıdaki tabloda açıklandığı gibi, herhangi bir sayısal türdeki değişkenleri ve işlevleri diğer herhangi bir sayısal tür sabitleri ile karşılaştırabilirsiniz.

| Değişken veya işlev türü | Sabit değer türü | Sınırlamalar |
| --- | --- | --- |
| `Edm.Double` | `Edm.Double` | Karşılaştırma, [ `NaN` için özel kurallara](#special-case-nan) tabidir |
| `Edm.Double` | `Edm.Int64` | Sabit `Edm.Double` , büyük boyutlu değerler için duyarlık kaybına neden olarak dönüştürülür. |
| `Edm.Double` | `Edm.Int32` | yok |
| `Edm.Int64` | `Edm.Double` | `NaN`, `-INF` , Veya karşılaştırmalara `INF` izin verilmez |
| `Edm.Int64` | `Edm.Int64` | yok |
| `Edm.Int64` | `Edm.Int32` | Sabit, `Edm.Int64` karşılaştırmaya önce öğesine dönüştürüldü |
| `Edm.Int32` | `Edm.Double` | `NaN`, `-INF` , Veya karşılaştırmalara `INF` izin verilmez |
| `Edm.Int32` | `Edm.Int64` | yok |
| `Edm.Int32` | `Edm.Int32` | yok |

Türündeki bir alanı ile karşılaştırma gibi izin verilmeyen karşılaştırmalar için, `Edm.Int64` `NaN` Azure bilişsel arama REST API "http 400: Hatalı istek" hatası döndürür.

> [!IMPORTANT]
> Sayısal tür karşılaştırmaları esnek olsa da, sabit değerin karşılaştırıldığı değişken veya işlevle aynı veri türü olması için filtrelerdeki karşılaştırmalar yazmanız önemle önerilir. Bu, özellikle de duyarlık kaybı olan örtük dönüştürmeler mümkün olduğunda kayan nokta ve tamsayı değerlerini karıştırdığınızda önemlidir.

<a name="special-case-nan"></a>

### <a name="special-cases-for-null-and-nan"></a>Ve için özel durumlar `null``NaN`

Karşılaştırma işleçlerini kullanırken, Azure Bilişsel Arama tüm koleksiyon olmayan alanların potansiyel olarak olabileceğini unutmamak önemlidir `null` . Aşağıdaki tabloda her iki tarafın de bir karşılaştırma ifadesi için olası tüm sonuçlar gösterilmektedir `null` :

| Operatör | Yalnızca alan veya değişken olduğunda sonuç `null` | Yalnızca sabit değer olduğunda sonuç `null` | Hem alan hem de değişken ve sabit olduğunda sonuç `null` |
| --- | --- | --- | --- |
| `gt` | `false` | HTTP 400: Hatalı Istek hatası | HTTP 400: Hatalı Istek hatası |
| `lt` | `false` | HTTP 400: Hatalı Istek hatası | HTTP 400: Hatalı Istek hatası |
| `ge` | `false` | HTTP 400: Hatalı Istek hatası | HTTP 400: Hatalı Istek hatası |
| `le` | `false` | HTTP 400: Hatalı Istek hatası | HTTP 400: Hatalı Istek hatası |
| `eq` | `false` | `false` | `true` |
| `ne` | `true` | `true` | `false` |

Özetle, `null` yalnızca kendine eşittir ve diğer herhangi bir değerden daha az veya daha büyük değildir.

Dizininizdeki tür alanları varsa `Edm.Double` ve `NaN` Bu alanlara değerleri yüklerseniz, filtre yazarken bu hesaba hesap eklemeniz gerekir. Azure Bilişsel Arama, değerleri işlemek için IEEE 754 standardını uygular `NaN` ve bu tür değerlerle karşılaştırmalar, aşağıdaki tabloda gösterildiği gibi belirgin olmayan sonuçlar üretir.

| Operatör | En az bir işlenen olduğunda sonuç `NaN` |
| --- | --- |
| `gt` | `false` |
| `lt` | `false` |
| `ge` | `false` |
| `le` | `false` |
| `eq` | `false` |
| `ne` | `true` |

Özet ' de `NaN` dahil olmak üzere herhangi bir değere eşit değildir.

### <a name="comparing-geo-spatial-data"></a>Coğrafi uzamsal verileri karşılaştırma

Türü bir alanı bir sabit değerle doğrudan karşılaştıramazsınız `Edm.GeographyPoint` , ancak `geo.distance` işlevini kullanabilirsiniz. Bu işlev türünde bir değer döndürür; bu `Edm.Double` nedenle, sabit coğrafi uzamsal koordinatlardan uzaklıktan yola çıkarak filtrelemek için sayısal bir sabitle karşılaştırabilirsiniz. Aşağıdaki [örneklere](#examples) bakın.

### <a name="comparing-string-data"></a>Dize verilerini karşılaştırma

Dizeler, ve işleçleri kullanılarak tam eşleşmelerin filtrelerinde karşılaştırılabilir `eq` `ne` . Bu karşılaştırmalar büyük/küçük harfe duyarlıdır.

## <a name="examples"></a>Örnekler

`Rating`Alanın 3 ile 5 arasında (dahil) olduğu belgeleri eşleştirin:

```text
Rating ge 3 and Rating le 5
```

`Location`Alanın, verilen Enlem ve Boylam 'nin 2 kilometreden küçük olduğu belgeleri eşleştirin:

```text
geo.distance(Location, geography'POINT(-122.031577 47.578581)') lt 2.0
```

`LastRenovationDate`Alanın 1 ocak 2015, gece yarısı UTC 'den büyük veya buna eşit olduğu belgeleri eşleştirin:

```text
LastRenovationDate ge 2015-01-01T00:00:00.000Z
```

Alanın olmadığı belge Eşleştir `Details/Sku` `null` :

```text
Details/Sku ne null
```

En az bir odada "Deluxe Oda" türüne sahip olduğu oteller için, `Rooms/Type` alanın dizesinin filtreye tam olarak eşleştiği bir belge eşleştirin:

```text
Rooms/any(room: room/Type eq 'Deluxe Room')
```

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama REST API &#40;belgelerde arama yapın&#41;](/rest/api/searchservice/Search-Documents)