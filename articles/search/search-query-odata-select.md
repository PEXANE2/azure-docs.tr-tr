---
title: OData select reference
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularının arama sonuçlarında döndürülecek alanların açık seçimi için sözdizimi ve dil başvurusu.
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
ms.openlocfilehash: 64f15bf3d262249cdda2760c7ddf768be2590419
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113109"
---
# <a name="odata-select-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da OData $select sözdizimi

 Azure Bilişsel Arama'nın arama sonuçlarına hangi alanların ekeceğini seçmek için [OData **$select** parametresini](query-odata-filter-orderby-syntax.md) kullanabilirsiniz. Bu **makalede, $select** sözdizimini ayrıntılı olarak açıklanmaktadır. Arama sonuçlarını sunarken **$select** nasıl kullanılacağı hakkında daha genel bilgi için Azure [Bilişsel Arama'da arama sonuçlarıyla nasıl çalışacağınız](search-pagination-page-layout.md)hakkında bilgi edin.

## <a name="syntax"></a>Sözdizimi

**$select** parametresi, sorgu sonuç kümesinde her belge için hangi alanların döndürüldiyi belirler. Aşağıdaki EBNF ([Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form) **$select** parametresi için dilbilgisi tanımlar:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#select_expression)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

**$select** parametresi iki şekilde gelir:

1. Tüm geri`*`alınabilir alanların döndürülmesi gerektiğini belirten tek bir yıldız (
1. Hangi alanların döndürülmesi gerektiğini tanımlayan, virgülle ayrılmış alan yollarının listesi.

İkinci formu kullanırken, yalnızca listede ki alınabilecek alanları belirtebilirsiniz.

Karmaşık bir alanı alt alanlarını açıkça belirtmeden listelerseniz, tüm alınabilen alt alanlar sorgu sonuç kümesine eklenir. Örneğin, dizininizin , `Address` ve `Street` `City`tüm alınabilen `Country` alt alanları olan bir alanı olduğunu varsayalım. $select olarak `Address` **$select**belirtirseniz, sorgu sonuçları üç alt alanı da içerir.

## <a name="examples"></a>Örnekler

Sonuçlara `HotelId` `HotelName`, `Rating` ve üst düzey alanları ve `City` alt alanını `Address`ekleyin:

    $select=HotelId, HotelName, Rating, Address/City

Örnek bir sonuç şu şekilde görünebilir:

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

Sonuçlara `HotelName` üst düzey alanı `Address`ve `Type` `BaseRate` `Rooms` koleksiyondaki her nesnenin tüm alt alanlarını ve alt alanlarını ekleyin:

    $select=HotelName, Address, Rooms/Type, Rooms/BaseRate

Örnek bir sonuç şu şekilde görünebilir:

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

- [Azure Bilişsel Arama'da arama sonuçlarıyla çalışma](search-pagination-page-layout.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
