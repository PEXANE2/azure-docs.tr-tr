---
title: Nesne Algılama-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme API'si kullanımı ve limitlerin nesne algılama özelliği ile ilgili kavramları öğrenin.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80131666"
---
# <a name="detect-common-objects-in-images"></a>Görüntülerde ortak nesneleri Algıla

Nesne algılama [etiketlemeyle](concept-tagging-images.md)benzerdir, ancak API, bulunan her nesne için sınırlayıcı kutu koordinatlarını (piksel cinsinden) döndürür. Örneğin, bir görüntü köpek, kedi ve kişi içeriyorsa, Algıla işlemi bu nesneleri görüntüdeki koordinatlarıyla birlikte listeler. Bir görüntüdeki nesneler arasındaki ilişkileri işlemek için bu işlevi kullanabilirsiniz. Ayrıca, görüntüde aynı etiketin birden fazla örneğinin olup olmadığını belirlemenizi sağlar.

Algılama API 'SI nesneleri veya görüntüde tanımlanan işleri temel alarak etiketleri uygular. Şu anda etiketleme sınıflandırması ve nesne algılama sınıflandırması arasında hiç resmi ilişki yok. Kavramsal düzeyde, algılama API 'si yalnızca nesneleri ve oturma şeyleri bulur, ancak Tag API 'SI, sınırlayıcı kutulara yerelleştirilebilecek "ınkapaklı" gibi bağlamsal terimleri de içerebilir.

## <a name="object-detection-example"></a>Nesne algılama örneği

Aşağıdaki JSON yanıtında, örnek görüntüde nesneleri algılayarak ne Görüntü İşleme döndürdüğü gösterilmektedir.

![Mutfak 'de Microsoft Surface cihazı kullanan kadın](./Images/windows-kitchen.jpg)

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

Hatalı negatifler 'in (kaçırılan nesneler) ve sınırlı ayrıntıların etkilerini önleyebileceğiniz veya azaltmanıza olanak sağlamak için nesne algılamalarının sınırlamalarını göz önünde bulundurulmak önemlidir.

* Nesneler genellikle küçük (görüntünün %5 ' inden az) olduğunda saptanmaz.
* Nesneler genellikle daha yakından düzenlenmişse (örneğin, bir levha yığını) saptanmaz.
* Nesneler marka veya ürün adlarına göre değil (örneğin, bir mağaza rafındaki farklı sodalar türleri). Ancak, [marka algılama](concept-brand-detection.md) özelliğini kullanarak bir görüntüden marka bilgileri alabilirsiniz.

## <a name="use-the-api"></a>API’yi kullanma

Nesne algılama özelliği, [görüntüyü çözümle](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API 'sinin bir parçasıdır. Bu API 'YI yerel bir SDK aracılığıyla veya REST çağrıları aracılığıyla çağırabilirsiniz. `Objects` **Visualfeatures** sorgu parametresine dahil edin. Ardından, tam JSON yanıtını aldığınızda, bu dizeyi yalnızca `"objects"` bölüm içeriği için ayrıştırın.

* [Hızlı başlangıç: Görüntü İşleme .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Hızlı başlangıç: bir görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)