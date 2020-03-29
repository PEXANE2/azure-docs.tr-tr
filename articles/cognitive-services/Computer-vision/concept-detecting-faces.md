---
title: Yüz algılama - Bilgisayarlı Görme
titleSuffix: Azure Cognitive Services
description: Bilgisayarlı Görme API'sinin yüz algılama özelliğiyle ilgili kavramları öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: d10e4c98d3e2f00e7139063c8263e65ffd015d7c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80244710"
---
# <a name="face-detection-with-computer-vision"></a>BilgisayarLı Görme ile yüz algılama

Computer Vision görüntü deki insan yüzlerini algılayabilir ve algılanan her yüz için yaş, cinsiyet ve dikdörtgen oluşturabilir. 

> [!NOTE]
> Bu özellik Azure [Face](/azure/cognitive-services/face/) hizmeti tarafından da sunulur. Yüz tanımlama ve poz algılama dahil olmak üzere daha ayrıntılı yüz analizi için bu alternatife bakın. 

## <a name="face-detection-examples"></a>Yüz algılama örnekleri

Aşağıdaki örnek, Bilgisayar Lı Görüş tarafından tek bir insan yüzü içeren bir görüntü için döndürülen JSON yanıtını göstermektedir.

![Görüntü Analizi Damdaki Kadının Yüzü](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

Sonraki örnek, birden çok insan yüzü içeren bir görüntü için döndürülen JSON yanıtını gösterir.

![Vizyon Analiz Aile Fotoğraf Yüz](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="use-the-api"></a>API’yi kullanma

Yüz algılama [özelliği, Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API'sinin bir parçasıdır. Bu API'yi yerel bir SDK veya REST aramaları aracılığıyla arayabilirsiniz. `Faces` **visualFeatures** sorgu parametresini ekleyin. Daha sonra, tam JSON yanıtı aldığınızda, `"faces"` bölümün içeriği için dize ayrıştını.

* [Hızlı Başlangıç: Bilgisayarlı Vizyon .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Quickstart: Görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)
