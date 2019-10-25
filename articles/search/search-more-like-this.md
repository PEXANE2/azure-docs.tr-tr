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
ms.openlocfilehash: b7959beca8a7787a331388b77ebe4060c3675e6d
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793469"
---
# <a name="morelikethis-preview-in-azure-cognitive-search"></a>Azure Bilişsel Arama moreLikeThis (Önizleme)

> [!Note]
> Morelikebu önizleme aşamasındadır ve üretim kullanımı için tasarlanmamıştır. [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.

`moreLikeThis=[key]`, belge anahtarı tarafından belirtilen belgeye benzer belgeleri bulan [arama belgeleri API 'sindeki](https://docs.microsoft.com/rest/api/searchservice/search-documents) bir sorgu parametresidir. `moreLikeThis`ile bir arama isteği yapıldığında, bu belgeyi en iyi şekilde tanımlayan belirtilen belgeden ayıklanan arama terimleriyle bir sorgu oluşturulur. Oluşturulan sorgu daha sonra arama isteğini yapmak için kullanılır. Varsayılan olarak, tüm aranabilir alanların içeriği, `searchFields` parametresini kullanarak belirttiğiniz tüm kısıtlı alanlar olarak değerlendirilir. `moreLikeThis` parametresi, `search=[string]`arama parametresiyle birlikte kullanılamaz.

Varsayılan olarak, en üst düzey aranabilir alanların içeriği göz önünde bulundurululur. Bunun yerine belirli alanları belirtmek istiyorsanız `searchFields` parametresini kullanabilirsiniz. 

[Karmaşık bir türdeki](search-howto-complex-data-types.md)aranabilir alt alanlarda moreLikeThis kullanamazsınız.

## <a name="examples"></a>Örnekler 

Aşağıda bir moreLikeThis sorgusuna örnek verilmiştir. Sorgu, açıklama alanları `moreLikeThis` parametresi tarafından belirtildiği gibi, kaynak belgenin alanına benzer olan belgeleri bulur.

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2019-05-06-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```


## <a name="next-steps"></a>Sonraki adımlar

Bu özellikle denemek için herhangi bir Web testi aracı kullanabilirsiniz.  Bu alıştırma için Postman kullanmanızı öneririz.

> [!div class="nextstepaction"]
> [Postman kullanarak Azure Bilişsel Arama REST API 'Lerini keşfet](search-get-started-postman.md)