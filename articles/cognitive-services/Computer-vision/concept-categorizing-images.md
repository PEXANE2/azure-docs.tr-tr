---
title: Görüntü kategorizasyonu - Bilgisayarlı Vizyon
titleSuffix: Azure Cognitive Services
description: BilgisayarLı Görme API'sinin görüntü kategorizasyonu özelliğiyle ilgili kavramları öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 477349f1addf71a30e8ecb179266d8eac5510887
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80244759"
---
# <a name="categorize-images-by-subject-matter"></a>Görüntüleri konuya göre kategorilere ayırın

Etiketlere ve açıklamaya ek olarak, Computer Vision görüntüde algılanan taksonomi tabanlı kategorileri döndürür. Etiketlerden farklı olarak, kategoriler bir üst/alt kalıtsal hiyerarşide düzenlenir ve bunlardan daha azdır (binlerce etiketin aksine 86). Tüm kategori adları İngilizcedir. Kategorilere ayırma kendisi veya yeni etiketleri modeli yanında yapılabilir.

## <a name="the-86-category-concept"></a>86 kategori kavramı

Bilgisayar görüşü, aşağıdaki diyagramdaki 86 kategorinin listesini kullanarak görüntüyü genel olarak veya özel olarak kategorilere ayırabilir. Metin biçiminde tam taksonomi için bkz. [Kategori Taksonomisi](category-taxonomy.md).

![Kategori taksonomi kategorisindeki tüm kategorilerin gruplanmış listeleri](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Resim kategorizasyonu örnekleri

Aşağıdaki JSON yanıtı, örnek görüntüyü görsel özelliklerine göre kategorilere ayırırken Computer Vision'ın ne döndürdediğini göstermektedir.

![Bir apartmanın çatısında bir kadın](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

Aşağıdaki tabloda tipik bir görüntü kümesi ve her görüntü için Computer Vision tarafından döndürülen kategori gösterilebilir.

| Görüntü | Kategori |
|-------|----------|
| ![Dört kişi bir aile gibi birlikte poz verdi](./Images/family_photo.png) | people_group |
| ![Çimenli bir tarlada oturan bir köpek yavrusu](./Images/cute_dog.png) | animal_dog |
| ![Gün batımında bir dağ kayası üzerinde duran bir kişi](./Images/mountain_vista.png) | outdoor_mountain |
| ![Masada bir yığın ekmek rolü](./Images/bread.png) | food_bread |

## <a name="use-the-api"></a>API’yi kullanma

Kategorizasyon özelliği, Analyze [Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API'sinin bir parçasıdır. Bu API'yi yerel bir SDK veya REST aramaları aracılığıyla arayabilirsiniz. `Categories` **visualFeatures** sorgu parametresini ekleyin. Daha sonra, tam JSON yanıtı aldığınızda, `"categories"` bölümün içeriği için dize ayrıştını.

* [Hızlı Başlangıç: Bilgisayarlı Vizyon .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Quickstart: Görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)

## <a name="next-steps"></a>Sonraki adımlar

[Görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri açıklama](concept-describing-images.md)ile ilgili kavramları öğrenin.
