---
title: İçerik etiketleri-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme API'si resimler etiketleme özelliğiyle ilgili kavramları öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 887f6d9163f800ecbe8a5ab9a0715fd171f1d3d9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80244520"
---
# <a name="applying-content-tags-to-images"></a>Resimlere içerik etiketleri uygulama

Görüntü İşleme, nesnelerin binlerce tanınabilir nesne, canlı, manzara ve eylemlere göre, etiketleri döndürür. Belirsiz veya herkesçe bilinmeyen etiketler söz konusu olduğunda, API yanıtı, etiketin anlamının bilinen bir ortama ilişkin bağlamda açıklığa kavuşturulması için "ipuçları" sağlar. Etiketler taksonomi olarak tanınmaz ve hiçbir devralma hiyerarşisi yoktur. Bir içerik etiketi koleksiyonu, tam tümceler halinde biçimlendirilmiş insan tarafından okunabilir dilde görüntülenen bir görüntü 'açıklamasının' temelini oluşturur. Şu noktada görüntü açıklaması için desteklenen tek dilin İngilizce olduğunu unutmayın.

Resmi karşıya yükledikten veya resim URL'sini belirttikten sonra, Görüntü İşleme algoritmaları resimde tanımlanan nesnelere, canlılara ve eylemlere dayanarak etiketlerin çıkışını yapar. Etiketleme yalnızca temel konu ile sınırlı kalmayıp ortam (iç mekân veya dış mekân), mobilyalar, aletler, bitkiler, hayvanlar, aksesuarlar, araçlar ve benzeri öğeleri de kapsar.

## <a name="image-tagging-example"></a>Resim etiketleme örneği

Aşağıdaki JSON yanıtında, örnek görüntüde algılanan görsel özellikleri etiketlerken ne Görüntü İşleme döndürdüğü gösterilmektedir.

![Mavi Ev ve ön bahçe](./Images/house_yard.png).

```json
{
    "tags": [
        {
            "name": "grass",
            "confidence": 0.9999995231628418
        },
        {
            "name": "outdoor",
            "confidence": 0.99992108345031738
        },
        {
            "name": "house",
            "confidence": 0.99685388803482056
        },
        {
            "name": "sky",
            "confidence": 0.99532157182693481
        },
        {
            "name": "building",
            "confidence": 0.99436837434768677
        },
        {
            "name": "tree",
            "confidence": 0.98880356550216675
        },
        {
            "name": "lawn",
            "confidence": 0.788884699344635
        },
        {
            "name": "green",
            "confidence": 0.71250593662261963
        },
        {
            "name": "residential",
            "confidence": 0.70859086513519287
        },
        {
            "name": "grassy",
            "confidence": 0.46624681353569031
        }
    ],
    "requestId": "06f39352-e445-42dc-96fb-0a1288ad9cf1",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="use-the-api"></a>API’yi kullanma

Etiketleme özelliği, [görüntüyü çözümle](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API 'sinin bir parçasıdır. Bu API 'YI yerel bir SDK aracılığıyla veya REST çağrıları aracılığıyla çağırabilirsiniz. `Tags` **Visualfeatures** sorgu parametresine dahil edin. Ardından, tam JSON yanıtını aldığınızda, bu dizeyi yalnızca `"tags"` bölüm içeriği için ayrıştırın.

* [Hızlı başlangıç: Görüntü İşleme .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Hızlı başlangıç: bir görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)

## <a name="next-steps"></a>Sonraki adımlar

[Görüntüleri kategorilere ayırma](concept-categorizing-images.md) ve [görüntüleri açıklama](concept-describing-images.md)ile ilgili kavramları öğrenin.
