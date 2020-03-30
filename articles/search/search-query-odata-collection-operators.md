---
title: OData toplama operatörü başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında filtre ifadeleri oluştururken, filtre bir koleksiyon veya karmaşık toplama alanındayken lambda ifadelerinde "any" ve "all" işleçlerini kullanın.
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
ms.openlocfilehash: 54ddc8222816831b5b436297bbb1b40d03230f0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113242"
---
# <a name="odata-collection-operators-in-azure-cognitive-search---any-and-all"></a>Azure Bilişsel Arama'da OData toplama operatörleri - `any` ve`all`

Azure Bilişsel Arama'da kullanmak üzere [bir OData filtre ifadesi](query-odata-filter-orderby-syntax.md) yazarken, toplama alanlarında filtre uygulama genellikle yararlıdır. Bunu operatörler ve `any` `all` operatörler kullanarak elde edebilirsiniz.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)kullanan `any` veya `all`kullanan bir OData ifadesinin dilbilgisini tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#collection_filter_expression)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

Koleksiyonları filtreleyen üç ifade biçimi vardır.

- İlk iki toplama alanı üzerinde, koleksiyonun her öğesi için bir lambda ifadesi şeklinde verilen bir yüklem uygulayarak.
  - Yüklem `all` koleksiyonun her öğesi için doğruysa, döndürür `true` kullanan bir ifade.
  - Yüklem `any` koleksiyonun en az bir öğesi için doğruysa, döndürür `true` kullanan bir ifade.
- Toplama filtresinin üçüncü `any` biçimi, bir toplama alanının boş olup olmadığını test etmek için lambda ifadesi olmadan kullanır. Koleksiyonda herhangi bir öğe `true`varsa, döndürür. Koleksiyon boşsa, döndürür. `false`

Toplama filtresindeki **lambda ifadesi,** programlama dilindeki bir döngünün gövdesi gibidir. Yineleme sırasında koleksiyonun geçerli öğesini tutan **aralık değişkeni**olarak adlandırılan bir değişken tanımlar. Ayrıca, koleksiyonun her öğesi için aralık değişkenine uygulanacak filtre ölçütleri olan başka bir boolean ifadesini tanımlar.

## <a name="examples"></a>Örnekler

`tags` Alanı tam olarak "wifi" dizesini içeren belgeleri eşleştirin:

    tags/any(t: t eq 'wifi')

Alanın her elemanının `ratings` 3 ile 5 arasında düştüğü, dahil olduğu belgeleri eşleştirin:

    ratings/all(r: r ge 3 and r le 5)

Alandaki herhangi bir jeo koordinatın `locations` verilen çokgen içinde olduğu belgeleri eşleştirin:

    locations/any(loc: geo.intersects(loc, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))

Alanın boş `rooms` olduğu belgeleri eşleştirin:

    not rooms/any()

Tüm odalar için `rooms/amenities` alanın "tv" içerdiği `rooms/baseRate` ve 100'den az olduğu belgeleri eşleştirin:

    rooms/all(room: room/amenities/any(a: a eq 'tv') and room/baseRate lt 100.0)

## <a name="limitations"></a>Sınırlamalar

Filtre ifadelerinin her özelliği lambda ifadesinin gövdesiiçinde bulunmaz. Sınırlamalar, filtrelemek istediğiniz toplama alanının veri türüne bağlı olarak farklılık gösterir. Aşağıdaki tablo sınırlamaları özetleyin.

[!INCLUDE [Limitations on OData lambda expressions in Azure Cognitive Search](../../includes/search-query-odata-lambda-limitations.md)]

Bu sınırlamalar ve örnekler hakkında daha fazla bilgi için [Azure Bilişsel Arama'da Sorun Giderme toplama filtrelerine](search-query-troubleshoot-collection-filters.md)bakın. Bu sınırlamaların neden var olduğu hakkında daha ayrıntılı bilgi için Azure [Bilişsel Arama'da koleksiyon filtrelerini anlama 'ya](search-query-understand-collection-filters.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
