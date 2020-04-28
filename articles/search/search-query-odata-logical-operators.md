---
title: OData mantıksal işleç başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında OData mantıksal işleçleri ve, veya, ve değil, için sözdizimi ve başvuru belgeleri.
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
ms.openlocfilehash: 2d3952f7d2adc26892cbebcd962f2ea25b86de7d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74113192"
---
# <a name="odata-logical-operators-in-azure-cognitive-search---and-or-not"></a>Azure Bilişsel Arama OData mantıksal işleçleri- `and`,, `or``not`

Azure Bilişsel Arama 'de [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) , `true` veya `false`sonucunu veren Boole ifadeleridir. Bir dizi [daha basit](search-query-odata-comparison-operators.md) filtre yazarak ve bunları [Boolean ALKU](https://en.wikipedia.org/wiki/Boolean_algebra)'dan mantıksal işleçler kullanarak oluşturarak karmaşık bir filtre yazabilirsiniz:

- `and`: Hem sol hem de sağ alt `true` ifadelerinin olarak `true`değerlendirilmesi için değerlendirilen bir ikili işleç.
- `or`: Sol veya sağ alt ifadelerden birinin `true` olarak `true`değerlendirilip değerlendirilmeyeceğini değerlendiren bir ikili işleç.
- `not`: Alt ifadesi olarak `true` `false`değerlendirilen ve tam tersi olarak değerlendirilen birli bir işleç.

Bunlar, [koleksiyon işleçleri `any` ve `all` ](search-query-odata-collection-operators.md)ile birlikte çok karmaşık arama ölçütlerine hızlı bir şekilde ifade edebilen filtreler oluşturmanız için izin verir.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)), mantıksal işleçleri kullanan bir OData ifadesinin dilbilgisini tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression
```

Etkileşimli bir sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#logical_expression)

> [!NOTE]
> Tüm EBNF için bkz. [Azure bilişsel arama Için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md) .

İki mantıksal ifade biçimi vardır`and`/`or`: ikili (), burada iki alt ifade ve birli (`not`) vardır; burada yalnızca bir tane vardır. Alt ifadeler herhangi bir türden Boolean ifadeler olabilir:

- Türündeki alanlar veya Aralık değişkenleri`Edm.Boolean`
- Türü `Edm.Boolean`değer döndüren işlevler, örneğin `geo.intersects` veya`search.ismatch`
- [Karşılaştırma ifadeleri](search-query-odata-comparison-operators.md), örneğin`rating gt 4`
- Gibi [koleksiyon ifadeleri](search-query-odata-collection-operators.md)`Rooms/any(room: room/Type eq 'Deluxe Room')`
- Boolean sabit değerleri `true` veya `false`.
- , `and` `or`Ve `not`kullanılarak oluşturulan diğer mantıksal ifadeler.

> [!IMPORTANT]
> Özellikle lambda ifadeleri içinde, tüm alt ifade türlerinin birlikte `and` / `or`kullanılabileceği bazı durumlar vardır. Ayrıntılar için bkz. [Azure 'Da OData koleksiyon işleçleri bilişsel arama](search-query-odata-collection-operators.md#limitations) .

### <a name="logical-operators-and-null"></a>Mantıksal işleçler ve`null`

İşlevler ve karşılaştırmalar gibi çoğu Boole ifadesi değer üretemiyor `null` ve mantıksal işleçler doğrudan `null` sabit değere uygulanamaz (örneğin, `x and null` buna izin verilmez). Ancak, `null`Boolean alanları olabilir, bu nedenle, ve `and` `or` `not` işleçlerinin, null durumunda nasıl davrandığına dikkat etmeniz gerekir. Bu, aşağıdaki tabloda özetlenmiştir, burada `b` , türünde `Edm.Boolean`bir alandır:

| İfadeler | Ne zaman `b` sonuç`null` |
| --- | --- |
| `b` | `false` |
| `not b` | `true` |
| `b eq true` | `false` |
| `b eq false` | `false` |
| `b eq null` | `true` |
| `b ne true` | `true` |
| `b ne false` | `true` |
| `b ne null` | `false` |
| `b and true` | `false` |
| `b and false` | `false` |
| `b or true` | `true` |
| `b or false` | `false` |

`b` Bir Boole alanı bir filtre ifadesinde kendisiyle göründüğünde, yazılmış `b eq true`gibi davranır, `b` yani ise `null`ifadesi olarak `false`değerlendirilir. Benzer şekilde `not b` , gibi `not (b eq true)`davranır, olarak `true`değerlendirilir. Bu şekilde, `null` alanlar aynı şekilde `false`davranır. Bu, yukarıdaki tabloda gösterildiği gibi ve `and` `or`kullanan diğer ifadelerle birleştirildiğinde, nasıl davrandıklarından tutarlıdır. Buna rağmen, ( `false` `b eq false`) öğesine doğrudan karşılaştırma olmaya devam edecektir. `false` Diğer bir deyişle, `null` Boolean ifadelerde gibi davransa bile, öğesine `false`eşit değildir.

## <a name="examples"></a>Örnekler

`rating` Alanın 3 ile 5 arasında (dahil) olduğu belgeleri eşleştirin:

    rating ge 3 and rating le 5

`ratings` Alanın tüm öğelerinin 3 ' ten küçük veya 5 ' ten büyük olduğu belgeleri eşleştirin:

    ratings/all(r: r lt 3 or r gt 5)

`location` Alanın verilen çokgen içinde bulunduğu belgeleri eşleştirin ve belge "genel" terimini içermez.

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))') and not search.ismatch('public')

160 'den düşük bir temel fiyata sahip olan bir lüks, Kanada 'daki otel belgelerini eşleştirin:

    Address/City eq 'Vancouver' and Address/Country eq 'Canada' and Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 160)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama REST API &#40;belgelerde arama yapın&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
