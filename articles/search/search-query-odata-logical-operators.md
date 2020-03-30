---
title: OData mantıksal operatör başvurusu
titleSuffix: Azure Cognitive Search
description: OData mantıksal işleçlerini kullanmak için sözdizimi ve başvuru belgeleri ve Azure Bilişsel Arama sorgularında veya veya değil.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113192"
---
# <a name="odata-logical-operators-in-azure-cognitive-search---and-or-not"></a>Azure Bilişsel Arama'da OData mantıksal operatörleri - `and`, , `or``not`

Azure Bilişsel Arama'daki [OData filtre ifadeleri,](query-odata-filter-orderby-syntax.md) boolean ifadelerini değerlendiren `true` veya `false`. Daha [basit filtreler](search-query-odata-comparison-operators.md) bir dizi yazarak ve [Boolean cebir](https://en.wikipedia.org/wiki/Boolean_algebra)mantıksal operatörler kullanarak bunları oluşturarak karmaşık bir filtre yazabilirsiniz:

- `and`: Hem `true` sol hem de sağ alt `true`ifadelerinin .
- `or`: Sol `true` veya sağ alt ifadelerinden birinin `true`.
- `not`: Alt ifadesinin `true` `false`,veya tam tersi olarak değerlendirilip değerlendirildiğini değerlendiren bir unary işleci.

Bunlar, toplama [ `any` işleçleri `all` ](search-query-odata-collection-operators.md)ile birlikte ve , çok karmaşık arama ölçütleri ifade edebilir filtreler oluşturmanıza olanak sağlar.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur Formu),](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)mantıksal işleçleri kullanan bir OData ifadesinin dilbilgisini tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#logical_expression)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

Mantıksal ifadelerin iki biçimi vardır:`and`/`or`ikili ( ), iki alt ifadenin bulunduğu`not`yer, tek bir ifadenin olduğu yerde ise unary ( ). Alt ifadeler her türlü Boolean ifadeleri olabilir:

- Tür deki alanlar veya aralık değişkenleri`Edm.Boolean`
- Tür `Edm.Boolean`değerlerini döndüren işlevler ( örneğin, `geo.intersects``search.ismatch`
- [Karşılaştırma ifadeleri](search-query-odata-comparison-operators.md), örneğin`rating gt 4`
- [Koleksiyon ifadeleri](search-query-odata-collection-operators.md), örneğin`Rooms/any(room: room/Type eq 'Deluxe Room')`
- Boolean edebi `true` ya `false`da .
- Diğer mantıksal ifadeler `and`kullanılarak `or`oluşturulmuş `not`, , ve .

> [!IMPORTANT]
> Her türlü alt ifadenin özellikle lambda ifadeleri `and` / `or`ile kullanılamayacağı bazı durumlar vardır. Ayrıntılar için [Azure Bilişsel Arama'daki OData toplama operatörlerine](search-query-odata-collection-operators.md#limitations) bakın.

### <a name="logical-operators-and-null"></a>Mantıksal işleçler ve`null`

Işlevler ve karşılaştırmalar gibi boolean `null` ifadelerinin çoğu değer üretemez ve `null` mantıksal işleçler `x and null` doğrudan edebi bölüme uygulanamaz (örneğin, izin verilmez). Ancak, Boolean alanları `null`olabilir , bu yüzden nasıl `and` `or`farkında `not` olmak gerekir , , ve operatörler null varlığında nasıl. Bu, bir tür `b` `Edm.Boolean`alanının bulunduğu aşağıdaki tabloda özetlenmiştir:

| İfadeler | Sonuç `b` ne zaman`null` |
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

Bir Boolean `b` alanı bir filtre ifadesinde tek başına göründüğünde, `b eq true`yazılmış gibi görünür , `b` `null`yani eğer ise `false`, ifade değerlendirir . Benzer şekilde, `not b` gibi `not (b eq true)`bir şekilde , `true`bu yüzden değerlendirir . Bu şekilde, `null` alanlar aynı `false`şekilde. Bu, yukarıdaki tabloda gösterildiği gibi, diğer `and` ifadeler `or`kullanarak ve , birleştiğinde nasıl davrandıkları ile tutarlıdır. Buna rağmen, `false` doğrudan bir`b eq false`karşılaştırma ( `false`) hala değerlendirecektir . Başka bir `null` deyişle, boolean ifadelerinde olduğu gibi görünse de `false`eşit değildir.

## <a name="examples"></a>Örnekler

Alanın 3 `rating` ile 5 arasında olduğu belgeleri, dahil:

    rating ge 3 and rating le 5

Alanın tüm öğelerinin `ratings` 3'ten küçük veya 5'ten büyük olduğu belgeleri eşleştirin:

    ratings/all(r: r lt 3 or r gt 5)

Alanın verilen `location` çokgen içinde olduğu belgeleri eşleştirin ve belge "genel" terimini içermez.

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))') and not search.ismatch('public')

Vancouver, Kanada'da taban fiyatı 160'tan az olan lüks bir odanın bulunduğu otelleriçin maç belgeleri:

    Address/City eq 'Vancouver' and Address/Country eq 'Canada' and Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 160)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
