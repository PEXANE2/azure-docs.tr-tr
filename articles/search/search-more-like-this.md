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
ms.openlocfilehash: 95b9c76a2ff962cb2fa4bacbb1b1e9a953b7014f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74873820"
---
# <a name="morelikethis-preview-in-azure-cognitive-search"></a>Azure Bilişsel Arama moreLikeThis (Önizleme)

> [!IMPORTANT] 
> Bu özellik şu anda genel önizleme aşamasındadır. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

`moreLikeThis=[key]`, belge anahtarı tarafından belirtilen belgeye benzer belgeleri bulan [arama belgeleri API 'sindeki](https://docs.microsoft.com/rest/api/searchservice/search-documents) bir sorgu parametresidir. İle `moreLikeThis`bir arama isteği yapıldığında, bu belgenin en iyi şekilde betimleyen, belirtilen belgeden ayıklanan arama terimleriyle bir sorgu oluşturulur. Oluşturulan sorgu daha sonra arama isteğini yapmak için kullanılır. Varsayılan olarak, tüm aranabilir alanların içeriği, `searchFields` parametresini kullanarak belirttiğiniz tüm kısıtlı alanlar olarak değerlendirilir. `moreLikeThis` Parametresi, `search=[string]`arama parametresiyle birlikte kullanılamaz.

Varsayılan olarak, en üst düzey aranabilir alanların içeriği göz önünde bulundurululur. Bunun yerine belirli alanları belirtmek istiyorsanız `searchFields` parametresini kullanabilirsiniz. 

[Karmaşık](search-howto-complex-data-types.md)bir türdeki `MoreLikeThis` aranabilir alt alanlar üzerinde kullanamazsınız.

## <a name="examples"></a>Örnekler

Aşağıdaki örneklerde, [hızlı başlangıç: Azure Portal bir arama dizini oluşturma ' daki](search-get-started-portal.md)oteller örneği kullanılmaktadır.

### <a name="simple-query"></a>Basit sorgu

Aşağıdaki sorgu, açıklama alanları, `moreLikeThis` parametre tarafından belirtildiği gibi, kaynak belgenin alanına benzer olan belgeleri bulur:

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=29&searchFields=Description&api-version=2019-05-06-Preview
```

Bu örnekte, istek, 29 ile `HotelId` benzer bir oteller için arama yapar.
HTTP GET 'in kullanılması yerine, HTTP POST kullanarak da `MoreLikeThis` çağırabilirsiniz:

```
POST /indexes/hotels-sample-index/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "29",
      "searchFields": "Description"
    }
```

### <a name="apply-filters"></a>Filtreleri uygulama

`MoreLikeThis`, gibi `$filter`diğer ortak sorgu parametreleriyle birleştirilebilir. Örneğin, sorgu yalnızca kategorisi ' bütçe ' olan ve derecelendirmeyi 3,5 ' den yüksek olan oteller ile kısıtlanabilir:

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=20&searchFields=Description&$filter=(Category eq 'Budget' and Rating gt 3.5)&api-version=2019-05-06-Preview
```

### <a name="select-fields-and-limit-results"></a>Alanları seçin ve sonuçları sınırlayın

Seçici `$top` , bir `MoreLikeThis` sorguda kaç sonucun döndürüleceğini sınırlamak için kullanılabilir. Ayrıca, ile `$select`alanları seçilebilir. Burada, ilk üç otel KIMLIĞI, adı ve derecelendirmesi ile birlikte seçilir: 

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=20&searchFields=Description&$filter=(Category eq 'Budget' and Rating gt 3.5)&$top=3&$select=HotelId,HotelName,Rating&api-version=2019-05-06-Preview
```

## <a name="next-steps"></a>Sonraki adımlar

Bu özellikle denemek için herhangi bir Web testi aracı kullanabilirsiniz.  Bu alıştırma için Postman kullanmanızı öneririz.

> [!div class="nextstepaction"]
> [Postman kullanarak Azure Bilişsel Arama REST API 'Lerini keşfet](search-get-started-postman.md)
