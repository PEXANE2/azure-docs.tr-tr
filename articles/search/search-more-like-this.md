---
title: moreLikeThis Azure Search (Önizleme)-Azure Search
description: Azure Search REST API kullanıma sunulan moreLikeThis (Önizleme) özelliğinin ön belgeleri.
author: brjohnstmsft
manager: nitinme
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: d7c816c545c6647907aa9d700a4eb6ed91277465
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70182314"
---
# <a name="morelikethis-in-azure-search"></a>Azure Search içinde moreLikeThis

> [!Note]
> Morelikebu önizleme aşamasındadır ve üretim kullanımı için tasarlanmamıştır. [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.

`moreLikeThis=[key]`, belge anahtarı tarafından belirtilen belgeye benzer belgeleri bulan [arama belgeleri API 'sindeki](https://docs.microsoft.com/rest/api/searchservice/search-documents) bir sorgu parametresidir. İle `moreLikeThis`bir arama isteği yapıldığında, bu belgenin en iyi şekilde betimleyen, belirtilen belgeden ayıklanan arama terimleriyle bir sorgu oluşturulur. Oluşturulan sorgu daha sonra arama isteğini yapmak için kullanılır. Varsayılan olarak, tüm aranabilir alanların içeriği, `searchFields` parametresini kullanarak belirttiğiniz tüm kısıtlı alanlar olarak değerlendirilir. `moreLikeThis` Parametresi ,`search=[string]`arama parametresiyle birlikte kullanılamaz.

Varsayılan olarak, en üst düzey aranabilir alanların içeriği göz önünde bulundurululur. Bunun yerine belirli alanları belirtmek istiyorsanız `searchFields` parametresini kullanabilirsiniz. 

[Karmaşık bir türdeki](search-howto-complex-data-types.md)aranabilir alt alanlarda moreLikeThis kullanamazsınız.

## <a name="examples"></a>Örnekler 

Aşağıda bir moreLikeThis sorgusuna örnek verilmiştir. Sorgu, açıklama alanları, `moreLikeThis` parametre tarafından belirtildiği gibi, kaynak belgenin alanına benzer olan belgeleri bulur.

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
> [Postman kullanarak Azure Search REST API 'Leri keşfet](search-get-started-postman.md)