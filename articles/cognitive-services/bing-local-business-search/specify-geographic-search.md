---
title: Bing Yerel İşletme Arama API'sindeki sonuçları filtrelemek için coğrafi sınırları kullanın
titleSuffix: Azure Cognitive Services
description: Bing Yerel İşletme Arama API'sindeki arama sonuçlarını nasıl filtreleyeceğimi öğrenmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: 213457bc583494bbe039269b96b25990f7d0a961
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "69906240"
---
# <a name="use-geographic-boundaries-to-filter-results-from-the-bing-local-business-search-api"></a>Bing Yerel İşletme Arama API'sindeki sonuçları filtrelemek için coğrafi sınırları kullanın

Bing Yerel İş Arama API'si, arama yapmak istediğiniz belirli coğrafi bölge üzerinde `localCircularView` `localMapView` sınırları veya sorgu parametrelerini kullanarak belirlemenize olanak tanır. Sorgularınızda yalnızca bir parametre kullandığınızdan emin olun. 

Bir arama terimi açık bir coğrafi konum içeriyorsa, Bing Yerel İşletme API'si arama sonuçlarının sınırlarını belirlemek için otomatik olarak kullanır. Örneğin, arama terimi `sailing in San Diego`, `San Diego` konum olarak kullanılır ve sorgu parametrelerinde veya kullanıcı üstbilgilerinde belirtilen diğer konumlar yoksayılır. 

Arama teriminde bir coğrafi konum algılanmamışsa ve sorgu parametreleri kullanılarak coğrafi konum belirtilmemişse, Bing Yerel İşletme `X-Search-ClientIP` Arama `X-Search-Location` API'si isteğin veya üstbilginin konumunu belirlemeye çalışır. Üstbilgi belirtilmemişse, API isteği istemci IP'sinden veya mobil aygıtlar için GPS koordinatlarından konumu belirler.

## <a name="localcircularview"></a>localCircularView

Parametre, `localCircularView` bir yarıçap tarafından tanımlanan enlem/boylam koordinatları kümesi etrafında dairesel bir coğrafi bölge oluşturur. Bu parametreyi kullanırken, Bing Yerel İş Arama API'sından gelen `localMapView` yanıtlar, arama alanının biraz dışındaki konumları içerebilecek parametrenin aksine, yalnızca bu daireiçindeki konumları içerir.

Dairesel bir coğrafi arama alanı belirlemek için, dairenin merkezi olarak hizmet vermek için bir enlem ve boylam ve metre olarak bir yarıçap seçin. Bu parametre daha sonra bir sorgu dizesine `q=Restaurants&localCircularView=47.6421,-122.13715,5000`eklenebilir, örneğin: .

Tam sorgu:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localCircularView=47.6421,-122.13715,5000&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="localmapview"></a>localMapView

Parametre, `localMapView` güneydoğu ve kuzeybatı köşelerini belirtmek için iki koordinat kümesi kullanarak, aramak için dikdörtgen bir coğrafi alanı belirtir. Bu parametreyi kullanırken, Bing Yerel İş Arama API'sından gelen yanıtlar, `localCircularView` yalnızca arama alanı içindeki konumları içeren parametrenin aksine, belirtilen alanın içindeki ve hemen dışındaki konumları içerebilir.

Dikdörtgen bir arama alanı belirlemek için, sınırın güneydoğu ve kuzeybatı köşeleri olarak hizmet vermek için iki enlem/boylam koordinatları kümesi seçin. Aşağıdaki örnekte olduğu gibi, ilk güneydoğu koordinatları `localMapView=47.619987,-122.181671,47.6421,-122.13715`tanımlamak için emin olun: .

Tam sorgu:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localMapView=47.619987,-122.181671,47.6421,-122.13715&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="next-steps"></a>Sonraki adımlar
- [Yerel İş Arama Java Quickstart](quickstarts/local-search-java-quickstart.md)
- [Yerel İş Arama C# Quickstart](quickstarts/local-quickstart.md)
- [Yerel İş Arama Düğümü Quickstart](quickstarts/local-search-node-quickstart.md)
- [Yerel İş Arama Python quickstart](quickstarts/local-search-python-quickstart.md)
