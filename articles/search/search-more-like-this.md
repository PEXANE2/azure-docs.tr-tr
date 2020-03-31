---
title: moreLikeThis (önizleme) sorgu özelliği
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama REST API'sinin önizleme sürümlerinde kullanılabilen Daha Fazla LikeThis (önizleme) özelliğini açıklar.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 95b9c76a2ff962cb2fa4bacbb1b1e9a953b7014f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873820"
---
# <a name="morelikethis-preview-in-azure-cognitive-search"></a>moreAzure Bilişsel Arama'da LikeThis (önizleme)

> [!IMPORTANT] 
> Bu özellik şu anda genel önizlemede dir. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

`moreLikeThis=[key]`[Arama Belgeleri API'sinde](https://docs.microsoft.com/rest/api/searchservice/search-documents) belge anahtarıtarafından belirtilen belgeye benzer belgeleri bulan bir sorgu parametresitir. Bir arama isteği ile `moreLikeThis`yapıldığında, bu belgeyi en iyi açıklayan belgeden çıkarılan arama terimleriyle bir sorgu oluşturulur. Oluşturulan sorgu daha sonra arama isteği yapmak için kullanılır. Varsayılan olarak, tüm aranabilir alanların içeriği, `searchFields` parametreyi kullanarak belirttiğiniz kısıtlı alanlar hariç olarak kabul edilir. Parametre `moreLikeThis` arama parametresi ile kullanılamaz. `search=[string]`

Varsayılan olarak, tüm üst düzey aranabilir alanların içeriği kabul edilir. Bunun yerine belirli alanları belirtmek istiyorsanız, `searchFields` parametreyi kullanabilirsiniz. 

Karmaşık bir `MoreLikeThis` [türde](search-howto-complex-data-types.md)aranabilir alt alanlarda kullanamazsınız.

## <a name="examples"></a>Örnekler

Aşağıdaki tüm örnekler Quickstart'taki otel örneğini [kullanır: Azure portalında bir arama dizini oluşturun.](search-get-started-portal.md)

### <a name="simple-query"></a>Basit sorgu

Aşağıdaki sorgu, açıklama alanları `moreLikeThis` parametrede belirtildiği şekilde kaynak belgenin alanına en çok benzeyen belgeleri bulur:

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=29&searchFields=Description&api-version=2019-05-06-Preview
```

Bu örnekte, istek 29 ile `HotelId` benzer oteller arar.
HTTP GET'i kullanmak yerine, `MoreLikeThis` HTTP POST'u kullanarak da çağrıda da bulabilirsiniz:

```
POST /indexes/hotels-sample-index/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "29",
      "searchFields": "Description"
    }
```

### <a name="apply-filters"></a>Filtreleri uygulama

`MoreLikeThis`gibi `$filter`diğer ortak sorgu parametreleri ile kombine edilebilir. Örneğin, sorgu yalnızca kategorisi 'Bütçe' olan ve derecelendirmesi 3,5'ten yüksek olan otellerle sınırlandırılabilir:

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=20&searchFields=Description&$filter=(Category eq 'Budget' and Rating gt 3.5)&api-version=2019-05-06-Preview
```

### <a name="select-fields-and-limit-results"></a>Alanları seçin ve sonuçları sınırlandırın

Seçici, `$top` bir `MoreLikeThis` sorguda kaç sonuç döndürülmesi gerektiğini sınırlamak için kullanılabilir. Ayrıca, alanlar ile `$select`seçilebilir. Burada ilk üç otel, kimlikleri, adları ve derecelendirmeleri ile birlikte seçilir: 

```
GET /indexes/hotels-sample-index/docs?moreLikeThis=20&searchFields=Description&$filter=(Category eq 'Budget' and Rating gt 3.5)&$top=3&$select=HotelId,HotelName,Rating&api-version=2019-05-06-Preview
```

## <a name="next-steps"></a>Sonraki adımlar

Bu özelliği denemek için herhangi bir web test aracık kullanabilirsiniz.  Bu egzersiz için Postacı kullanmanızı öneririz.

> [!div class="nextstepaction"]
> [Postacı kullanarak Azure Bilişsel Arama REST API'lerini keşfedin](search-get-started-postman.md)
