---
title: Görüntü türü algılama - Bilgisayar Lı
titleSuffix: Azure Cognitive Services
description: Bilgisayarlı Görme API'sinin görüntü türü algılama özelliğiile ilgili kavramlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 4e6c2db5333962d7ae43534998ffc1c48b0dba45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80244571"
---
# <a name="detecting-image-types-with-computer-vision"></a>Bilgisayar Görüşü ile görüntü türlerini algılama

Görüntü [API'sini Çözümle](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) ile Computer Vision, görüntünün küçük resim mi yoksa çizgi çizimi mi olduğunu gösteren görüntünün içerik türünü analiz edebilir.

## <a name="detecting-clip-art"></a>Küçük resmi algılama

Computer Vision bir görüntüyü analiz eder ve aşağıdaki tabloda açıklandığı gibi görüntünün 0 ile 3 arasında bir ölçekte küçük resim olma olasılığını tanımlar.

| Değer | Anlamı |
|-------|---------|
| 0 | Küçük resim değil |
| 1 | Belirsiz |
| 2 | Normal klip-resim |
| 3 | İyi klip-resim |

### <a name="clip-art-detection-examples"></a>Küçük resim algılama örnekleri

Aşağıdaki JSON yanıtları, örnek görüntülerin küçük resim olma olasılığını derecelendirirken Computer Vision'ın ne döndürdediğini gösterir.

![Bir dilim peynirin küçük resim görüntüsü](./Images/cheese_clipart.png)

```json
{
    "imageType": {
        "clipArtType": 3,
        "lineDrawingType": 0
    },
    "requestId": "88c48d8c-80f3-449f-878f-6947f3b35a27",
    "metadata": {
        "height": 225,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Mavi bir ev ve ön bahçe.](./Images/house_yard.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "a9c8490a-2740-4e04-923b-e8f4830d0e47",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="detecting-line-drawings"></a>Çizgi çizimlerini algılama

Computer Vision görüntüyü analiz eder ve görüntünün bir çizgi çizimi olup olmadığını belirten bir boolean değeri döndürür.

### <a name="line-drawing-detection-examples"></a>Çizgi çizim algılama örnekleri

Aşağıdaki JSON yanıtları, örnek görüntülerin çizgi çizimleri olup olmadığını gösterirken Computer Vision'ın ne döndürdüğünü gösterir.

![Aslan resmi çizen bir çizgi](./Images/lion_drawing.png)

```json
{
    "imageType": {
        "clipArtType": 2,
        "lineDrawingType": 1
    },
    "requestId": "6442dc22-476a-41c4-aa3d-9ceb15172f01",
    "metadata": {
        "height": 268,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Yeşil arka planlı beyaz bir çiçek](./Images/flower.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "98437d65-1b05-4ab7-b439-7098b5dfdcbf",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="use-the-api"></a>API’yi kullanma

Görüntü türü algılama özelliği, [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API'sinin bir parçasıdır. Bu API'yi yerel bir SDK veya REST aramaları aracılığıyla arayabilirsiniz. `ImageType` **visualFeatures** sorgu parametresini ekleyin. Daha sonra, tam JSON yanıtı aldığınızda, `"imageType"` bölümün içeriği için dize ayrıştını.

* [Hızlı Başlangıç: Bilgisayarlı Vizyon .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Quickstart: Görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)
