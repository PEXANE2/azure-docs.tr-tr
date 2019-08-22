---
title: OData karşılaştırma işleci başvurusu-Azure Search
description: Azure Search sorgularda OData karşılaştırma işleçleri, EQ, ne, gt, lt, GE ve Le.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
manager: nitinme
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
ms.openlocfilehash: a8bd8b05fd874e05e5e59042d461f4a4286c81e4
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648070"
---
# <a name="odata-comparison-operators-in-azure-search---eq-ne-gt-lt-ge-and-le"></a>Azure Search- `eq`, `ne`, `gt`, ,veiçindeODatakarşılaştırmaişleçleri`lt` `ge``le`

Azure Search bir [OData filtre ifadesinde](query-odata-filter-orderby-syntax.md) en temel işlem, bir alanı verilen değerle karşılaştırmaktır. İki tür karşılaştırma yapılabilir--eşitlik karşılaştırması ve Aralık karşılaştırması. Bir alanı sabit değerle karşılaştırmak için aşağıdaki işleçleri kullanabilirsiniz:

Eşitlik işleçleri:

- `eq`: Bir alanın sabit değere **eşit** olup olmadığını test etme
- `ne`: Bir alanın sabit değere eşit olup olmadığını test **etme**

Aralık işleçleri:

- `gt`: Bir alanın sabit değerden **büyük** olup olmadığını test etme
- `lt`: Bir alanın sabit değerden **küçük** olup olmadığını test etme
- `ge`: Bir alanın sabit değere **eşit veya ondan büyük** olup olmadığını test edin
- `le`: Bir alanın sabit değere **eşit veya ondan küçük** olup olmadığını test etme

Bir alanın belirli bir değer aralığı içinde olup olmadığını test etmek için, Aralık işleçlerini [mantıksal işleçlerle](search-query-odata-logical-operators.md) birlikte kullanabilirsiniz. Bu makalenin devamındaki [örneklere](#examples) bakın.

> [!NOTE]
> İsterseniz, sabit değeri işlecin sol tarafına ve alan adını sağ tarafta koyabilirsiniz. Aralık işleçleri için karşılaştırma anlamı tersine çevrilir. Örneğin, sabit değer Solsa `gt` , sabit değerinin alandan büyük olup olmadığını test edecektir. Karşılaştırma işleçlerini, gibi bir işlevin `geo.distance`sonucunu karşılaştırmak için de kullanabilirsiniz. Gibi Boole işlevleri `search.ismatch`için, sonucu veya `false` ile `true` karşılaştırma isteğe bağlıdır.

## <a name="syntax"></a>Sözdizimi

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
> [Azure Search için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#comparison_expression)

> [!NOTE]
> Tüm EBNF için [Azure Search Için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

İki karşılaştırma ifadesi biçimi vardır. Bunlar arasındaki tek fark, işlecin, işlecin sol veya sağ tarafında görünüp görüntülenmemesidir. İşlecin diğer tarafındaki ifade bir **değişken** veya işlev çağrısı olmalıdır. Bir değişken, bir alan adı ya da bir [lambda ifadesi](search-query-odata-collection-operators.md)söz konusu olduğunda bir Aralık değişkeni olabilir.

## <a name="data-types-for-comparisons"></a>Karşılaştırmalar için veri türleri

Karşılaştırma işlecinin her iki tarafındaki veri türleri uyumlu olmalıdır. Örneğin, sol kenar türünde `Edm.DateTimeOffset`bir alan ise, sağ kenar bir tarih-saat sabiti olmalıdır. Sayısal veri türleri daha esnektir. Aşağıdaki tabloda açıklandığı gibi, herhangi bir sayısal türdeki değişkenleri ve işlevleri diğer herhangi bir sayısal tür sabitleri ile karşılaştırabilirsiniz.

| Değişken veya işlev türü | Sabit değer türü | Sınırlamalar |
| --- | --- | --- |
| `Edm.Double` | `Edm.Double` | Karşılaştırma, [ `NaN` için özel kurallara](#special-case-nan) tabidir |
| `Edm.Double` | `Edm.Int64` | Sabit, büyük boyutlu `Edm.Double`değerler için duyarlık kaybına neden olarak dönüştürülür. |
| `Edm.Double` | `Edm.Int32` | yok |
| `Edm.Int64` | `Edm.Double` | `NaN`, ,`-INF`Veya karşılaştırmalaraizin`INF` verilmez |
| `Edm.Int64` | `Edm.Int64` | yok |
| `Edm.Int64` | `Edm.Int32` | Sabit, karşılaştırmaya önce `Edm.Int64` öğesine dönüştürüldü |
| `Edm.Int32` | `Edm.Double` | `NaN`, ,`-INF`Veya karşılaştırmalaraizin`INF` verilmez |
| `Edm.Int32` | `Edm.Int64` | yok |
| `Edm.Int32` | `Edm.Int32` | yok |

Türündeki `Edm.Int64` bir alanı ile `NaN`karşılaştırma gibi izin verilmeyen karşılaştırmalar için, Azure Search REST API "http 400" döndürür: Hatalı Istek "hatası.

> [!IMPORTANT]
> Sayısal tür karşılaştırmaları esnek olsa da, sabit değerin karşılaştırıldığı değişken veya işlevle aynı veri türü olması için filtrelerdeki karşılaştırmalar yazmanız önemle önerilir. Bu, özellikle de duyarlık kaybı olan örtük dönüştürmeler mümkün olduğunda kayan nokta ve tamsayı değerlerini karıştırdığınızda önemlidir.

<a name="special-case-nan"></a>

### <a name="special-cases-for-null-and-nan"></a>Ve için `null` özel durumlar`NaN`

Karşılaştırma işleçleri kullanılırken, Azure Search tüm koleksiyon olmayan alanların potansiyel `null`olarak olabileceğini unutmamak önemlidir. Aşağıdaki tabloda her iki tarafın de `null`bir karşılaştırma ifadesi için olası tüm sonuçlar gösterilmektedir:

| Operator | Yalnızca alan veya değişken olduğunda sonuç`null` | Yalnızca sabit değer olduğunda sonuç`null` | Hem alan hem de değişken ve sabit olduğunda sonuç`null` |
| --- | --- | --- | --- |
| `gt` | `false` | HTTP 400: Hatalı Istek hatası | HTTP 400: Hatalı Istek hatası |
| `lt` | `false` | HTTP 400: Hatalı Istek hatası | HTTP 400: Hatalı Istek hatası |
| `ge` | `false` | HTTP 400: Hatalı Istek hatası | HTTP 400: Hatalı Istek hatası |
| `le` | `false` | HTTP 400: Hatalı Istek hatası | HTTP 400: Hatalı Istek hatası |
| `eq` | `false` | `false` | `true` |
| `ne` | `true` | `true` | `false` |

Özetle `null` , yalnızca kendine eşittir ve diğer herhangi bir değerden daha az veya daha büyük değildir.

Dizininizdeki tür `Edm.Double` alanları varsa ve `NaN` bu alanlara değerleri yüklerseniz, filtre yazarken bu hesaba hesap eklemeniz gerekir. Azure Search değerleri işlemek `NaN` için IEEE 754 standardını uygular ve bu tür değerlerle karşılaştırmalar, aşağıdaki tabloda gösterildiği gibi belirgin olmayan sonuçlar üretir.

| Operator | En az bir işlenen olduğunda sonuç`NaN` |
| --- | --- |
| `gt` | `false` |
| `lt` | `false` |
| `ge` | `false` |
| `le` | `false` |
| `eq` | `false` |
| `ne` | `true` |

Özet ' `NaN` de dahil olmak üzere herhangi bir değere eşit değildir.

### <a name="comparing-geo-spatial-data"></a>Coğrafi uzamsal verileri karşılaştırma

Türü `Edm.GeographyPoint` bir alanı bir sabit değerle doğrudan karşılaştıramazsınız, ancak `geo.distance` işlevini kullanabilirsiniz. Bu işlev türünde bir değer döndürür; `Edm.Double`bu nedenle, sabit coğrafi uzamsal koordinatlardan uzaklıktan yola çıkarak filtrelemek için sayısal bir sabitle karşılaştırabilirsiniz. Aşağıdaki [örneklere](#examples) bakın.

### <a name="comparing-string-data"></a>Dize verilerini karşılaştırma

Dizeler, `eq` ve `ne` işleçleri kullanılarak tam eşleşmelerin filtrelerinde karşılaştırılabilir. Bu karşılaştırmalar büyük/küçük harfe duyarlıdır.

## <a name="examples"></a>Örnekler

`Rating` Alanın 3 ile 5 arasında (dahil) olduğu belgeleri eşleştirin:

    Rating ge 3 and Rating le 5

`Location` Alanın, verilen Enlem ve Boylam 'nin 2 kilometreden küçük olduğu belgeleri eşleştirin:

    geo.distance(Location, geography'POINT(-122.031577 47.578581)') lt 2.0

`LastRenovationDate` Alanın 1 Ocak 2015, gece yarısı UTC 'den büyük veya buna eşit olduğu belgeleri eşleştirin:

    LastRenovationDate ge 2015-01-01T00:00:00.000Z

`Details/Sku` Alanın olmadığı`null`belge eşleştir:

    Details/Sku ne null

En az bir odada "Deluxe Oda" türüne sahip olduğu oteller için, `Rooms/Type` alanın dizesinin filtreye tam olarak eşleştiği bir belge eşleştirin:

    Rooms/any(room: room/Type eq 'Deluxe Room')

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search filtreler](search-filters.md)
- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
