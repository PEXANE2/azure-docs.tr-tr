---
title: OData koleksiyon işleci başvurusu-Azure Search
description: Azure Search sorgularında OData koleksiyon işleçleri, any ve All ve lambda ifadeleri.
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
ms.openlocfilehash: e057d0b57162d10aab13d8b1f77e0eaddca2ec2a
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647647"
---
# <a name="odata-collection-operators-in-azure-search---any-and-all"></a>Azure Search `any` ve içindeki OData koleksiyon işleçleri`all`

Azure Search ile kullanmak üzere bir [OData filtresi ifadesi](query-odata-filter-orderby-syntax.md) yazarken, genellikle koleksiyon alanlarını filtrelemek yararlı olur. `any` Ve`all` işleçlerini kullanarak bunu elde edebilirsiniz.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)), veya `any` `all`kullanan bir OData ifadesinin dilbilgisini tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression
```

Etkileşimli bir sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#collection_filter_expression)

> [!NOTE]
> Tüm EBNF için [Azure Search Için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

Koleksiyonları filtreleyerek üç ifade biçimi vardır.

- Koleksiyon alanı üzerinde ilk iki yineleme, bir lambda ifadesi biçiminde verilen bir koşulu koleksiyonun her öğesine uygulayarak.
  - ' İ kullanan `all` bir `true` ifade, koşulun her öğe için true olması halinde döndürür.
  - Deyimi, koleksiyonun `any` en `true` az bir öğesi için true ise, döndüren bir ifade döndürür.
- Koleksiyon filtresinin üçüncü biçimi, bir koleksiyon `any` alanının boş olup olmadığını test etmek için lambda ifadesi olmadan kullanılır. Koleksiyonda herhangi bir öğe varsa, döndürür `true`. Koleksiyon boşsa, döndürür `false`.

Bir koleksiyon filtresindeki **lambda ifadesi** , bir programlama dilindeki Döngünün gövdesi gibidir. Yineleme sırasında koleksiyonun geçerli öğesini tutan **Aralık değişkeni**olarak adlandırılan bir değişkeni tanımlar. Ayrıca, koleksiyondaki her öğe için Aralık değişkenine uygulanacak filtre ölçütü olan başka bir Boolean ifadesi tanımlar.

## <a name="examples"></a>Örnekler

`tags` Alanı tam olarak "WiFi" dizesini içeren belgeleri eşleştir:

    tags/any(t: t eq 'wifi')

`ratings` Alandaki her öğenin 3 ila 5 (dahil) arasında olduğu belgeleri eşleştirin:

    ratings/all(r: r ge 3 and r le 5)

`locations` Alandaki coğrafi koordinatların herhangi birinin verilen çokgen içinde olduğu belgeleri eşleştirin:

    locations/any(loc: geo.intersects(loc, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))

`rooms` Alanın boş olduğu belgeleri eşleştir:

    not rooms/any()

Tüm odaların bulunduğu belgelerde, `rooms/amenities` alan "TV" ve `rooms/baseRate` 100 ' den küçük olan belgeleri eşleştirin:

    rooms/all(room: room/amenities/any(a: a eq 'tv') and room/baseRate lt 100.0)

## <a name="limitations"></a>Sınırlamalar

Her filtre ifadesi özelliği bir lambda ifadesinin gövdesi içinde kullanılamaz. Sınırlamalar, filtrelemek istediğiniz koleksiyon alanının veri türüne bağlı olarak farklılık gösterir. Aşağıdaki tabloda sınırlamalar özetlenmektedir.

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

Bu sınırlamalar ve örnekler hakkında daha fazla bilgi için bkz. [Azure Search koleksiyon filtrelerinde sorun giderme](search-query-troubleshoot-collection-filters.md). Bu sınırlamaların neden olduğu hakkında daha ayrıntılı bilgi için bkz. [Azure Search koleksiyon filtrelerini anlama](search-query-understand-collection-filters.md).

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search filtreler](search-filters.md)
- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
