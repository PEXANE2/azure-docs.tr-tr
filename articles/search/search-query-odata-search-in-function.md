---
title: OData search.in fonksiyon başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında search.in işlevini kullanmak için sözdizimi ve başvuru belgeleri.
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
ms.openlocfilehash: b43c46599cbacaf40bc9583e364d088fa27a3ac9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113110"
---
# <a name="odata-searchin-function-in-azure-cognitive-search"></a>Azure `search.in` Bilişsel Arama'da OData işlevi

[OData filtre ifadelerinde](query-odata-filter-orderby-syntax.md) yaygın bir senaryo, her belgedeki tek bir alanın olası değerlerden birine eşit olup olmadığını denetlemektir. Örneğin, bazı uygulamalar, bir veya daha fazla temel iD içeren bir alanı, sorguyu veren kullanıcıyı temsil eden temel iT'ler listesiyle karşılaştırarak [güvenlik kırpma](search-security-trimming-for-azure-search.md) uygulamasını bu şekilde uygular. Böyle bir sorgu yazmanın bir [`eq`](search-query-odata-comparison-operators.md) yolu, [`or`](search-query-odata-logical-operators.md) aşağıdakileri ve işleçleri kullanmaktır:

    group_ids/any(g: g eq '123' or g eq '456' or g eq '789')

Ancak, `search.in` işlevi kullanarak, bu yazmak için daha kısa bir yolu vardır:

    group_ids/any(g: search.in(g, '123, 456, 789'))

> [!IMPORTANT]
> Daha kısa ve kolay okunmasının `search.in` yanı sıra, filtreye dahil edilmesi gereken yüzlerce hatta binlerce değer olduğunda, kullanmak [performans avantajları](#bkmk_performance) sağlar ve filtrelerin belirli [boyut sınırlamalarını](search-query-odata-filter.md#bkmk_limits) önler. Bu nedenle, eşitlik ifadelerinin `search.in` daha karmaşık bir şekilde ayrıştırılmasının yerine kullanmanızı şiddetle öneririz.

> [!NOTE]
> OData standardının 4.01 sürümü, Azure Bilişsel Arama'daki `search.in` işlevle benzer davranışlara sahip [ `in` olan operatörü](https://docs.oasis-open.org/odata/odata/v4.01/cs01/part2-url-conventions/odata-v4.01-cs01-part2-url-conventions.html#_Toc505773230)yakın zamanda tanıttı. Ancak, Azure Bilişsel Arama bu işleç desteklemez, bu nedenle `search.in` işlevi kullanmanız gerekir.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form) `search.in` fonksiyonun dilbilgisi tanımlar:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#search_in_call)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

İşlev, `search.in` belirli bir dize alanı nın veya aralık değişkeninin belirli bir değer listesinden birine eşit olup olmadığını sınar. Listedeki değişken ve her değer arasındaki eşitlik, `eq` işleç için olduğu gibi, büyük/küçük harf duyarlı bir şekilde belirlenir. Bu nedenle `search.in(myfield, 'a, b, c')` gibi bir `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'`ifade `search.in` eşdeğerdir , çok daha iyi performans verecek dışında.

İşlevin iki aşırı `search.in` yüklemesi vardır:

- `search.in(variable, valueList)`
- `search.in(variable, valueList, delimiters)`

Parametreler aşağıdaki tabloda tanımlanır:

| Parametre adı | Tür | Açıklama |
| --- | --- | --- |
| `variable` | `Edm.String` | Bir dize alan başvurusu (veya bir `search.in` `any` veya `all` ifade içinde kullanıldığı durumda bir dize toplama alanı üzerinde bir aralık değişkeni). |
| `valueList` | `Edm.String` | `variable` Parametreyle eşleşecek sınırlı bir değer listesi içeren dize. `delimiters` Parametre belirtilmemişse, varsayılan sınır layıcılar boşluk ve virgüldür. |
| `delimiters` | `Edm.String` | `valueList` Parametreyi ayrıştırırken her karakterin ayırıcı olarak kabul edildiği bir dize. Bu parametrenin varsayılan `' ,'` değeri, aralarında boşluk ve/veya virgül olan değerlerin ayrılacağı anlamına gelir. Değerleriniz bu karakterleri içerdiğinden boşluk ve virgül dışındaki ayırıcılar kullanmanız gerekiyorsa, bu parametredeki gibi `'|'` alternatif sınırlayıcılar belirtebilirsiniz. |

<a name="bkmk_performance"></a>

### <a name="performance-of-searchin"></a>Performans`search.in`

Kullanırsanız, `search.in`ikinci parametre yüzlerce veya binlerce değerden oluşan bir liste içerdiğinde ikinci yanıt süresini bekleyebilirsiniz. Yine de maksimum istek boyutuyla sınırlı olmasına `search.in`rağmen, geçebileceğiniz öğe sayısında açık bir sınır yoktur. Ancak, değerlerin sayısı arttıkça gecikme seve sayılacaktır.

## <a name="examples"></a>Örnekler

'Sea View motel' veya 'Budget hotel' adlarına sahip tüm otelleri bulun. Tümcecikler, varsayılan bir sınır layıcı olan boşluklar içerir. Üçüncü dize parametresi olarak tek tırnak içinde alternatif bir sınırlayıcı belirtebilirsiniz:  

    search.in(HotelName, 'Sea View motel,Budget hotel', ',')

'Sea View motel' veya 'Budget hotel' ile ayrılmış ada sahip tüm otelleri bulun:

    search.in(HotelName, 'Sea View motel|Budget hotel', '|')

'wifi' veya 'küvet' etiketine sahip odaları olan tüm otelleri bulun:

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub')))

Etiketlerdeki 'ısıtmalı havlu rafları' veya 'saç kurutma makinesi dahil' gibi koleksiyondaki ifadelerde bir eşleşme bulun.

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))

'Motel' veya 'kabin' etiketi olmayan tüm otelleri bulun:

    Tags/all(tag: not search.in(tag, 'motel, cabin'))

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
