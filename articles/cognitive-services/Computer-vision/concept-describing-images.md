---
title: Resim açıklamaları - Bilgisayarlı Vizyon
titleSuffix: Azure Cognitive Services
description: Bilgisayarlı Görme API'sinin görüntü açıklaması özelliğiyle ilgili kavramlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 7bf95a2b49608ef1f031a3b443db92b42cdae624
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80244724"
---
# <a name="describe-images-with-human-readable-language"></a>Görüntüleri insan tarafından okunabilir bir dille anlatın

Computer Vision bir görüntüyü analiz edebilir ve içeriğini açıklayan insan tarafından okunabilir bir cümle oluşturabilir. Algoritma aslında farklı görsel özelliklere dayalı çeşitli açıklamalar döndürür ve her açıklamaya bir güven puanı verilir. Son çıktı, en yüksekten en düşük güvene doğru sıralanmış açıklamaların bir listesidir.

## <a name="image-description-example"></a>Resim açıklaması örneği

Aşağıdaki JSON yanıtı, örnek görüntüyü görsel özelliklerine göre açıklarken Computer Vision'ın ne döndürettiğini göstermektedir.

![Manhattan'daki binaların siyah beyaz resmi](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

## <a name="use-the-api"></a>API’yi kullanma

Görüntü açıklaması özelliği, Görüntü API'sini [Çözümle'nin](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) bir parçasıdır. Bu API'yi yerel bir SDK veya REST aramaları aracılığıyla arayabilirsiniz. `Description` **visualFeatures** sorgu parametresini ekleyin. Daha sonra, tam JSON yanıtı aldığınızda, `"description"` bölümün içeriği için dize ayrıştını.

* [Hızlı Başlangıç: Bilgisayarlı Vizyon .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Quickstart: Görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)

## <a name="next-steps"></a>Sonraki adımlar

[Görüntüleri etiketleme](concept-tagging-images.md) ve [görüntüleri kategorilere ayırma](concept-categorizing-images.md)ile ilgili kavramları öğrenin.
