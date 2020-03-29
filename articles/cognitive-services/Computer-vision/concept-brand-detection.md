---
title: Marka algılama - BilgisayarLı Vizyon
titleSuffix: Azure Cognitive Services
description: Bu makalede, nesne algılama özel bir modu tartışır; BilgisayarLı Vizyon API'sini kullanarak marka ve/veya logo algılama.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: pafarley
ms.openlocfilehash: 50e4fe1e2573c8566bbdf5697bb81b025a00935c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131729"
---
# <a name="detect-popular-brands-in-images"></a>Resimlerdeki popüler markaları algılama

Marka algılama, resim veya videodaki ticari markaları tanımlamak için binlerce küresel logodan oluşan bir veritabanı kullanan özel bir [nesne algılama](concept-object-detection.md) modudur. Bu özelliği, örneğin, hangi markaların sosyal medyada en popüler olduğunu veya medya ürün yerleştirmesinde en yaygın olduğunu keşfetmek için kullanabilirsiniz.

Computer Vision hizmeti, belirli bir resimde marka logoları olup olmadığını algılar; eğer öyleyse, marka adını, güven puanını ve logo etrafındaki sınırlayıcı kutunun koordinatlarını döndürür.

Yerleşik logo veritabanı tüketici elektroniği, giyim ve daha popüler markaları kapsar. Aradığınız markanın Computer Vision hizmeti tarafından algılanmadığını fark ederseniz, [Custom Vision](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) hizmetini kullanarak kendi logo dedektörünüzü oluşturmave eğitme hizmeti daha iyi sunulabilir.

## <a name="brand-detection-example"></a>Marka algılama örneği

Aşağıdaki JSON yanıtları, örnek görüntülerde markaları algılarken Computer Vision'ın neleri döndürettiğini göstermektedir.

![Üzerinde Microsoft etiketi ve logosu olan kırmızı bir gömlek](./Images/red-shirt-logo.jpg)

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

Bazı durumlarda, marka dedektörü iki ayrı logo olarak hem logo görüntü ve stilize marka adı alır.

![Üzerinde Microsoft etiketi ve logosu olan gri bir sweatshirt](./Images/gray-shirt-logo.jpg)

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

Marka algılama [özelliği, Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API'sinin bir parçasıdır. Bu API'yi yerel bir SDK veya REST aramaları aracılığıyla arayabilirsiniz. `Brands` **visualFeatures** sorgu parametresini ekleyin. Daha sonra, tam JSON yanıtı aldığınızda, `"brands"` bölümün içeriği için dize ayrıştını.

* [Hızlı Başlangıç: Bilgisayarlı Vizyon .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Quickstart: Görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)
