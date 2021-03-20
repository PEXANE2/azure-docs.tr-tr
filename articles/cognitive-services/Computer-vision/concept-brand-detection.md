---
title: Marka Algılama-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Bu makalede, nesne algılamada özelleştirilmiş bir mod ele alınmaktadır; Görüntü İşleme API'si kullanarak marka ve/veya amblem algılama.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: pafarley
ms.openlocfilehash: 40792585fbc52aaeec8a535b6a82decfce7618f2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96533698"
---
# <a name="detect-popular-brands-in-images"></a>Görüntülerde popüler markaların algılanması

Marka algılama, resimlerde veya videoda ticari markaların tanımlanması için binlerce küresel logoın bir veritabanını kullanan [nesne algılamasında](concept-object-detection.md) özelleştirilmiş bir moddur. Bu özelliği örneğin, sosyal medya veya en popüler medya ürün yerleştirmesi üzerinde en popüler olan markalar öğrenmek için kullanabilirsiniz.

Görüntü İşleme Hizmeti belirli bir görüntüde marka logoları olup olmadığını algılar; Bu durumda, bir marka adı, Güvenirlik puanı ve logo etrafındaki bir sınırlayıcı kutunun koordinatlarını döndürür.

Yerleşik logo veritabanı, tüketici elektroniği, giyme ve daha birçok konuda popüler markaların yer aldığı bir şeydir. Aradığınız marka Görüntü İşleme Hizmeti tarafından algılanmadığını fark ederseniz, [özel görüntü işleme](../custom-vision-service/index.yml) hizmetini kullanarak kendi amblem algıanızı oluşturma ve eğitim konusunda daha iyi bir işlem yapabilirsiniz.

## <a name="brand-detection-example"></a>Marka algılama örneği

Aşağıdaki JSON yanıtları, örnek görüntülerde markalar algılayarak ne Görüntü İşleme döndüğünü gösterir.

![Microsoft etiketi ve logosu içeren kırmızı bir gömlek](./Images/red-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":20,
         "y":97,
         "w":62,
         "h":52
      }
   }
]
```

Bazı durumlarda, marka algılayıcısı hem logo görüntüsünü hem de stilize marka adını iki ayrı logo olarak seçer.

![Üzerinde bir Microsoft etiketi ve logosu olan gri bir elter](./Images/gray-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":106,
         "w":55,
         "h":46
      }
   },
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":86,
         "w":202,
         "h":63
      }
   }
]
```

## <a name="use-the-api"></a>API’yi kullanma

Marka algılama özelliği, [görüntüyü çözümle](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b) API 'sinin bir parçasıdır. Bu API 'YI yerel bir SDK aracılığıyla veya REST çağrıları aracılığıyla çağırabilirsiniz. `Brands` **Visualfeatures** sorgu parametresine dahil edin. Ardından, tam JSON yanıtını aldığınızda, bu dizeyi yalnızca bölüm içeriği için ayrıştırın `"brands"` .

* [Hızlı başlangıç: Görüntü İşleme REST API veya istemci kitaplıkları](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)