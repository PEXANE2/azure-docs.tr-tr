---
title: Renk düzeni algılama - Bilgisayar Lı
titleSuffix: Azure Cognitive Services
description: BilgisayarLı Görme API'sini kullanarak görüntülerdeki renk düzenini algılamayla ilgili kavramlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: af0c39ed8211ac2041d143112437ad5d6b384259
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80244741"
---
# <a name="detect-color-schemes-in-images"></a>Görüntülerdeki renk düzenlerini algılama

Computer Vision, üç farklı özellik sağlamak için görüntüdeki renkleri analiz eder: baskın ön plan rengi, baskın arka plan rengi ve bir bütün olarak görüntü için baskın renk kümesi. Geri dönen renkler sete aittir: siyah, mavi, kahverengi, gri, yeşil, turuncu, pembe, mor, kırmızı, kahverengi, beyaz ve sarı. 

Computer Vision ayrıca, baskın renkler ve doygunluk kombinasyonuna dayanan görüntüdeki en canlı rengi temsil eden bir vurgu rengi de ayıklar. Vurgu rengi hexadecimal HTML renk kodu olarak döndürülür. 

Computer Vision ayrıca görüntünün siyah beyaz olup olmadığını belirten bir boolean değeri döndürür.

## <a name="color-scheme-detection-examples"></a>Renk düzeni algılama örnekleri

Aşağıdaki örnek, örnek görüntünün renk düzenini algılarken Computer Vision tarafından döndürülen JSON yanıtını göstermektedir. Bu durumda, örnek görüntü siyah beyaz bir görüntü değildir, ancak baskın ön plan ve arka plan renkleri siyahtır ve bir bütün olarak görüntünün baskın renkleri siyah ve beyazdır.

![Gün batımında Açık Dağ, bir kişinin silueti ile](./Images/mountain_vista.png)

```json
{
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": ["Black", "White"],
        "accentColor": "BB6D10",
        "isBwImg": false
    },
    "requestId": "0dc394bf-db50-4871-bdcc-13707d9405ea",
    "metadata": {
        "height": 202,
        "width": 300,
        "format": "Jpeg"
    }
}
```

### <a name="dominant-color-examples"></a>Baskın renk örnekleri

Aşağıdaki tablo, her örnek görüntü için döndürülen ön planı, arka planı ve görüntü renklerini gösterir.

| Görüntü | Baskın renkler |
|-------|-----------------|
|![Yeşil arka planlı beyaz bir çiçek](./Images/flower.png)| Ön plan: Siyah<br/>Arka Plan: Beyaz<br/>Renkler: Siyah, Beyaz, Yeşil|
![İstasyonda çalışan bir tren](./Images/train_station.png) | Ön plan: Siyah<br/>Arka Plan: Siyah<br/>Renkler: Siyah |

### <a name="accent-color-examples"></a>Vurgu renk örnekleri

 Aşağıdaki tablo, her örnek görüntü için hexadecimal HTML renk değeri olarak döndürülen vurgu rengini gösterir.

| Görüntü | Vurgu rengi |
|-------|--------------|
|![Gün batımında bir dağ kayası üzerinde duran bir kişi](./Images/mountain_vista.png) | #BB6D10 |
|![Yeşil arka planlı beyaz bir çiçek](./Images/flower.png) | #C6A205 |
|![İstasyonda çalışan bir tren](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Siyah & beyaz algılama örnekleri

Aşağıdaki tabloda örnek görüntülerde Computer Vision'ın siyah beyaz değerlendirmesini gösterilmektedir.

| Görüntü | Siyah & beyaz mı? |
|-------|----------------|
|![Manhattan'daki binaların siyah beyaz resmi](./Images/bw_buildings.png) | true |
|![Mavi bir ev ve ön bahçe.](./Images/house_yard.png) | yanlış |

## <a name="use-the-api"></a>API’yi kullanma

Renk düzeni algılama [özelliği, Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API'sinin bir parçasıdır. Bu API'yi yerel bir SDK veya REST aramaları aracılığıyla arayabilirsiniz. `Color` **visualFeatures** sorgu parametresini ekleyin. Daha sonra, tam JSON yanıtı aldığınızda, `"color"` bölümün içeriği için dize ayrıştını.

* [Hızlı Başlangıç: Bilgisayarlı Vizyon .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Quickstart: Görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)
