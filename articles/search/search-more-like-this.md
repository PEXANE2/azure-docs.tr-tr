---
title: moreLikeThis (Önizleme) sorgu özelliği
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama REST API önizleme sürümlerinde kullanılabilen moreLikeThis (Önizleme) özelliğini açıklar.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: cd6b64f118460a115963ed0bf105641d80334348
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934999"
---
# <a name="morelikethis-preview-in-azure-cognitive-search"></a>Azure Bilişsel Arama moreLikeThis (Önizleme)

> [!IMPORTANT] 
> Bu özellik şu anda genel önizleme aşamasındadır. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2020-06-30-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

`moreLikeThis=[key]` , belge anahtarı tarafından belirtilen belgeye benzer belgeleri bulan [arama belgeleri API 'sindeki](/rest/api/searchservice/search-documents) bir sorgu parametresidir. İle bir arama isteği yapıldığında `moreLikeThis` , bu belgenin en iyi şekilde betimleyen, belirtilen belgeden ayıklanan arama terimleriyle bir sorgu oluşturulur. Oluşturulan sorgu daha sonra arama isteğini yapmak için kullanılır. Varsayılan olarak, tüm aranabilir alanların içeriği, parametresini kullanarak belirttiğiniz tüm kısıtlı alanlar olarak değerlendirilir `searchFields` . `moreLikeThis`Parametresi, arama parametresiyle birlikte kullanılamaz `search=[string]` .

Varsayılan olarak, en üst düzey aranabilir alanların içeriği göz önünde bulundurululur. Bunun yerine belirli alanları belirtmek istiyorsanız `searchFields` parametresini kullanabilirsiniz. 

`MoreLikeThis` [Karmaşık bir türdeki](search-howto-complex-data-types.md)aranabilir alt alanlar üzerinde kullanamazsınız.

## <a name="examples"></a>Örnekler

Aşağıdaki örneklerde, [hızlı başlangıç: Azure Portal bir arama dizini oluşturma ' daki](search-get-started-portal.md)oteller örneği kullanılmaktadır.

### <a name="simple-query"></a>Basit sorgu

Aşağıdaki sorgu, açıklama alanları, parametre tarafından belirtildiği gibi, kaynak belgenin alanına benzer olan belgeleri bulur `moreLikeThis` :

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=29&searchFields=Description&api-version=2020-06-30-Preview
```

Bu örnekte, istek, 29 ile benzer bir oteller için arama yapar `HotelId` .
HTTP GET 'in kullanılması yerine, `MoreLikeThis` http post kullanarak da çağırabilirsiniz:

```
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview
    {
      "moreLikeThis": "29",
      "searchFields": "Description"
    }
```

### <a name="apply-filters"></a>Filtreleri uygulama

`MoreLikeThis` , gibi diğer ortak sorgu parametreleriyle birleştirilebilir `$filter` . Örneğin, sorgu yalnızca kategorisi ' bütçe ' olan ve derecelendirmeyi 3,5 ' den yüksek olan oteller ile kısıtlanabilir:

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=20&searchFields=Description&$filter=(Category eq 'Budget' and Rating gt 3.5)&api-version=2020-06-30-Preview
```

### <a name="select-fields-and-limit-results"></a>Alanları seçin ve sonuçları sınırlayın

`$top`Seçici, bir sorguda kaç sonucun döndürüleceğini sınırlamak için kullanılabilir `MoreLikeThis` . Ayrıca, ile alanları seçilebilir `$select` . Burada, ilk üç otel KIMLIĞI, adı ve derecelendirmesi ile birlikte seçilir: 

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=20&searchFields=Description&$filter=(Category eq 'Budget' and Rating gt 3.5)&$top=3&$select=HotelId,HotelName,Rating&api-version=2020-06-30-Preview
```

## <a name="next-steps"></a>Sonraki adımlar

Bu özellikle denemek için herhangi bir Web testi aracı kullanabilirsiniz.  Bu alıştırma için Postman kullanmanızı öneririz.

> [!div class="nextstepaction"]
> [Postman kullanarak Azure Bilişsel Arama REST API 'Lerini keşfet](search-get-started-postman.md)