---
title: OData mantıksal işleç başvurusu-Azure Search
description: Azure Search sorgularında OData mantıksal işleçleri, ve, veya, ve değildir.
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
ms.openlocfilehash: bf4939a40a2fdf1c8fc6cf97beca0184b1604c98
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647991"
---
# <a name="odata-logical-operators-in-azure-search---and-or-not"></a>Azure Search- `and`, `or`,,`not`

Azure Search içindeki [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) , `true` veya `false`sonucunu veren Boole ifadeleridir. Bir dizi [daha basit](search-query-odata-comparison-operators.md) filtre yazarak ve bunları [Boolean ALKU](https://en.wikipedia.org/wiki/Boolean_algebra)'dan mantıksal işleçler kullanarak oluşturarak karmaşık bir filtre yazabilirsiniz:

- `and`: Hem sol hem de sağ alt `true` ifadelerinin olarak `true`değerlendirilmesi için değerlendirilen bir ikili işleç.
- `or`: Sol veya sağ alt ifadelerden birinin `true` olarak `true`değerlendirilip değerlendirilmeyeceğini değerlendiren bir ikili işleç.
- `not`: Alt ifadesi olarak `false`değerlendirilen ve tam `true` tersi olarak değerlendirilen birli işleç.

Bunlar, [ `any` koleksiyon işleçleri ve `all` ](search-query-odata-collection-operators.md)ile birlikte çok karmaşık arama ölçütlerine hızlı bir şekilde ifade edebilen filtreler oluşturmanız için izin verir.

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
> [Azure Search için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#logical_expression)

> [!NOTE]
> Tüm EBNF için [Azure Search Için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

İki mantıksal ifade biçimi`and`vardır: ikili (/`or`), burada iki alt ifade ve birli (`not`) vardır; burada yalnızca bir tane vardır. Alt ifadeler herhangi bir türden Boolean ifadeler olabilir:

- Türündeki alanlar veya Aralık değişkenleri`Edm.Boolean`
- Türü `Edm.Boolean`değer döndüren işlevler, `geo.intersects` örneğin veya`search.ismatch`
- [Karşılaştırma ifadeleri](search-query-odata-comparison-operators.md), örneğin`rating gt 4`
- Gibi [koleksiyon ifadeleri](search-query-odata-collection-operators.md)`Rooms/any(room: room/Type eq 'Deluxe Room')`
- Boolean sabit değerleri `true` veya `false`.
- , `and` `or`Ve kullanılarakoluşturulandiğermantıksalifadeler.`not`

> [!IMPORTANT]
> Özellikle lambda ifadeleri içinde, tüm alt ifade türlerinin birlikte `and` / `or`kullanılabileceği bazı durumlar vardır. Ayrıntılar için [Azure Search ' de OData koleksiyon işleçleri '](search-query-odata-collection-operators.md#limitations) ne bakın.

### <a name="logical-operators-and-null"></a>Mantıksal işleçler ve`null`

İşlevler ve karşılaştırmalar gibi çoğu Boole ifadesi değer üretemiyor `null` ve mantıksal işleçler doğrudan `null` sabit değere uygulanamaz (örneğin, `x and null` buna izin verilmez). Ancak, Boolean `null`alanları olabilir, bu nedenle, ve `not` işleçlerinin, null durumunda `and`nasıl `or`davrandığına dikkat etmeniz gerekir. Bu, aşağıdaki tabloda özetlenmiştir, burada `b` , türünde `Edm.Boolean`bir alandır:

| İfade | Ne zaman `b` sonuç`null` |
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

Bir `b` Boole alanı bir filtre ifadesinde kendisiyle göründüğünde, yazılmış `b` `b eq true`gibi davranır, yani ise `null`ifadesi olarak `false`değerlendirilir. Benzer şekilde `not b` , gibi `not (b eq true)`davranır, olarak `true`değerlendirilir. Bu şekilde, `null` alanlar aynı şekilde `false`davranır. Bu, yukarıdaki tabloda gösterildiği gibi ve `and` `or`kullanan diğer ifadelerle birleştirildiğinde, nasıl davrandıklarından tutarlıdır. Buna rağmen, ( `false` `b eq false`) öğesine doğrudan karşılaştırma olmaya devam edecektir. `false` Diğer bir deyişle, `null` Boolean ifadelerde gibi davransa bile, öğesine `false`eşit değildir.

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

- [Azure Search filtreler](search-filters.md)
- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
