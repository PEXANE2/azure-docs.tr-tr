---
title: OData Select başvurusu-Azure Search
description: Azure Search sorgularında sözdizimi seç için OData dil başvurusu.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
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
ms.openlocfilehash: 64e9ad75d88f595ab5def6fe8b63fee9407ae0fe
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647883"
---
# <a name="odata-select-syntax-in-azure-search"></a>Azure Search içinde OData $select söz dizimi

 Azure Search arama sonuçlarına dahil edilecek alanları seçmek için [OData **$Select** parametresini](query-odata-filter-orderby-syntax.md) kullanabilirsiniz. Bu makalede **$Select** sözdizimi ayrıntılı olarak açıklanmaktadır. Arama sonuçlarını sunarken **$Select** kullanma hakkında daha fazla genel bilgi için bkz. [Azure Search arama sonuçlarıyla çalışma](search-pagination-page-layout.md).

## <a name="syntax"></a>Sözdizimi

**$Select** parametresi, her belge için hangi alanların sorgu sonuç kümesinde döndürüleceğini belirler. Aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) **$Select** parametresi için dilbilgisini tanımlar:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*
```

Etkileşimli bir sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#select_expression)

> [!NOTE]
> Tüm EBNF için [Azure Search Için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

**$Select** parametresi iki şekilde gelir:

1. Tüm alınabilir alanların döndürülmesi`*`gerektiğini belirten tek bir yıldız () veya
1. Alan yollarının virgülle ayrılmış bir listesi, hangi alanların döndürüleceğini tanımlar.

İkinci formu kullanırken, yalnızca listede alınabilir alanları belirtebilirsiniz.

Kendi alt alanlarını açıkça belirtmeden bir karmaşık alanı listelemiyorsanız, tüm alınabilir alt alanlar sorgu sonuç kümesine dahil edilir. `Address` Örneğin, dizininizin, `Street` `City`, ve`Country` tüm alınabilir alt alanlar içeren bir alanı olduğunu varsayalım. `Address` **$Select**belirtirseniz, sorgu sonuçları üç alt alanı da içerir.

## <a name="examples"></a>Örnekler

`HotelName` `Rating` Sonuçlara ve alt alanına `HotelId` ,veenüst`Address`düzeyalanlarıekleyin: `City`

    $select=HotelId, HotelName, Rating, Address/City

Örnek bir sonuç şöyle görünebilir:

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "City": "New York"
  }
}
```

`Rooms` `BaseRate` `Type` `Address`Sonuçlarda en üst düzey alanı, tüm alt alanlarını ve koleksiyondaki her bir nesnenin ve alt alanlarını dahil edin: `HotelName`

    $select=HotelName, Address, Rooms/Type, Rooms/BaseRate

Örnek bir sonuç şöyle görünebilir:

```json
{
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY",
    "Country": "USA",
    "PostalCode": "10022"
  },
  "Rooms": [
    {
      "Type": "Budget Room",
      "BaseRate": 9.69
    },
    {
      "Type": "Budget Room",
      "BaseRate": 8.09
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search arama sonuçlarıyla çalışma](search-pagination-page-layout.md)
- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
