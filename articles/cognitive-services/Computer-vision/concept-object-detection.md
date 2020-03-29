---
title: Nesne algılama - BilgisayarLı Görme
titleSuffix: Azure Cognitive Services
description: BilgisayarLı Görme API'sinin nesne algılama özelliğiyle ilgili kavramları öğrenin - kullanımı ve sınırları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 3957e15a09bd7e7ecd814d169451af3241108b64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131666"
---
# <a name="detect-common-objects-in-images"></a>Görüntülerdeki ortak nesneleri algılama

Nesne algılama [etiketlemeye](concept-tagging-images.md)benzer, ancak API bulunan her nesne için sınırlayıcı kutu koordinatlarını (piksel olarak) döndürür. Örneğin, bir görüntü bir köpek, kedi ve kişi içeriyorsa, Algıla işlemi bu nesneleri görüntüdeki koordinatlarıyla birlikte listeler. Bu işlevselliği, görüntüdeki nesneler arasındaki ilişkileri işlemek için kullanabilirsiniz. Ayrıca, görüntüde aynı etiketin birden çok örneği olup olmadığını belirlemenize olanak tanır.

AlgıAPI, görüntüde tanımlanan nesneleri veya canlıları temel alan etiketler uygular. Şu anda etiketleme taksonomi ve nesne algılama taksonomi arasında resmi bir ilişki yoktur. Kavramsal düzeyde, Algı API'si yalnızca nesneleri ve canlıları bulurken, Tag API'si sınırlayıcı kutularla yerelleştirilemeyecek "kapalı" gibi bağlamsal terimleri de içerebilir.

## <a name="object-detection-example"></a>Nesne algılama örneği

Aşağıdaki JSON yanıtı, örnek görüntüdeki nesneleri algılarken Computer Vision'ın ne döndürettiğini gösterir.

![Mutfakta Microsoft Surface cihazı kullanan bir kadın](./Images/windows-kitchen.jpg)

```json
{
   "objects":[
      {
         "rectangle":{
            "x":730,
            "y":66,
            "w":135,
            "h":85
         },
         "object":"kitchen appliance",
         "confidence":0.501
      },
      {
         "rectangle":{
            "x":523,
            "y":377,
            "w":185,
            "h":46
         },
         "object":"computer keyboard",
         "confidence":0.51
      },
      {
         "rectangle":{
            "x":471,
            "y":218,
            "w":289,
            "h":226
         },
         "object":"Laptop",
         "confidence":0.85,
         "parent":{
            "object":"computer",
            "confidence":0.851
         }
      },
      {
         "rectangle":{
            "x":654,
            "y":0,
            "w":584,
            "h":473
         },
         "object":"person",
         "confidence":0.855
      }
   ],
   "requestId":"a7fde8fd-cc18-4f5f-99d3-897dcd07b308",
   "metadata":{
      "width":1260,
      "height":473,
      "format":"Jpeg"
   }
}
```

## <a name="limitations"></a>Sınırlamalar

Yanlış negatiflerin (cevapsız nesneler) ve sınırlı ayrıntıların etkilerini önlemek veya azaltmak için nesne algılama sınırlamalarına dikkat etmek önemlidir.

* Nesneler genellikle küçükse algılanmaz (görüntünün %5'inden az).
* Nesneler genellikle birbirine yakın olarak düzenlenirse algılanmaz (örneğin, bir tabaka yığını).
* Nesneler marka veya ürün adlarına göre ayırt edilmez (örneğin, mağaza rafındaki farklı soda türleri). Ancak, [Marka algılama](concept-brand-detection.md) özelliğini kullanarak bir görüntüden marka bilgilerini alabilirsiniz.

## <a name="use-the-api"></a>API’yi kullanma

Nesne algılama özelliği, [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API'sinin bir parçasıdır. Bu API'yi yerel bir SDK veya REST aramaları aracılığıyla arayabilirsiniz. `Objects` **visualFeatures** sorgu parametresini ekleyin. Daha sonra, tam JSON yanıtı aldığınızda, `"objects"` bölümün içeriği için dize ayrıştını.

* [Hızlı Başlangıç: Bilgisayarlı Vizyon .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Quickstart: Görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)