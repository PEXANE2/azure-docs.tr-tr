---
title: Etki alanına özel içerik - Computer Vision
titleSuffix: Azure Cognitive Services
description: Bir resim hakkında daha ayrıntılı bilgi vermek için görüntü kategorizasyonu etki alanını nasıl belirtin öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 8d6dc91ae7bb0f6d7a24064749d9295558a7d39c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68946333"
---
# <a name="detect-domain-specific-content"></a>Etki alanına özgü içeriği algılama

Etiketleme ve üst düzey kategorilere ayırmaya ek olarak, Computer Vision özel veriler üzerinde eğitilmiş modelleri kullanarak daha fazla etki alanına özgü analizi de destekler.

Etki alanına özgü modelleri kullanmanın iki yolu vardır: kendileri tarafından (kapsamlı çözümleme) veya kategorizasyon özelliğini geliştirme olarak.

### <a name="scoped-analysis"></a>Kapsamlı analiz

[Modelleri/\<modeli\>/Analyze](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) API'yi arayarak yalnızca seçilen etki alanına özgü modeli kullanarak görüntüyü çözümleyebilirsiniz.

Aşağıda, verilen görüntü için **modeller/ünlüler/analiz** API'si tarafından döndürülen örnek bir JSON yanıtı verilmiştir:

![Satya Nadella ayakta, gülümseyerek](./images/satya.jpeg)

```json
{
  "result": {
    "celebrities": [{
      "faceRectangle": {
        "top": 391,
        "left": 318,
        "width": 184,
        "height": 184
      },
      "name": "Satya Nadella",
      "confidence": 0.99999856948852539
    }]
  },
  "requestId": "8217262a-1a90-4498-a242-68376a4b956b",
  "metadata": {
    "width": 800,
    "height": 1200,
    "format": "Jpeg"
  }
}
```

### <a name="enhanced-categorization-analysis"></a>Gelişmiş kategorizasyon analizi

Genel görüntü çözümlemesi için etki alanına özel modelleri de kullanabilirsiniz. Bunu, [Çözümle](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API çağrısının *ayrıntı* parametresinde etki alanına özgü modelleri belirterek [üst düzey kategorilere ayırmanın](concept-categorizing-images.md) bir parçası olarak yaparsınız.

Bu durumda, 86 kategorili taksonomi sınıflandırıcı ilk denir. Algılanan kategorilerden herhangi biri eşleşen etki alanına özgü bir modele sahipse, görüntü bu modelden de geçirilir ve sonuçlar eklenir.

Aşağıdaki JSON yanıtı, etki alanına özgü çözümlemenin `detail` daha geniş bir kategorizasyon analizine düğüm olarak nasıl dahil edilebildiğini gösterir.

```json
"categories":[
  {
    "name":"abstract_",
    "score":0.00390625
  },
  {
    "name":"people_",
    "score":0.83984375,
    "detail":{
      "celebrities":[
        {
          "name":"Satya Nadella",
          "faceRectangle":{
            "left":597,
            "top":162,
            "width":248,
            "height":248
          },
          "confidence":0.999028444
        }
      ],
      "landmarks":[
        {
          "name":"Forbidden City",
          "confidence":0.9978346
        }
      ]
    }
  }
]
```

## <a name="list-the-domain-specific-models"></a>Etki alanına özgü modelleri listele

Şu anda, Computer Vision aşağıdaki etki alanına özgü modelleri destekler:

| Adı | Açıklama |
|------|-------------|
| Ünlü | Ünlü tanıma, `people_` kategorisinde sınıflandırılan görüntüler için desteklenen |
| Simge | Yer işareti tanıma, `outdoor_` veya `building_` kategorilerde sınıflandırılan görüntüler için desteklenen |

[Modeller](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fd) API'yi çağırmak, bu bilgileri her modelin uygulayabileceği kategorilerle birlikte döndürecektir:

```json
{
  "models":[
    {
      "name":"celebrities",
      "categories":[
        "people_",
        "人_",
        "pessoas_",
        "gente_"
      ]
    },
    {
      "name":"landmarks",
      "categories":[
        "outdoor_",
        "户外_",
        "屋外_",
        "aoarlivre_",
        "alairelibre_",
        "building_",
        "建筑_",
        "建物_",
        "edifício_"
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Görüntüleri kategorilere ayırma](concept-categorizing-images.md)yla ilgili kavramları öğrenin.
