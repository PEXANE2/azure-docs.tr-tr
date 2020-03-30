---
title: Metin Birleştirme bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Alanlar koleksiyonundan gelen metni tek birleştirilmiş alanda birleştirin. Bu bilişsel beceriyi Azure Bilişsel Arama'daki bir AI zenginleştirme boru hattında kullanın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 98ea416305f080850d85498f74693eb2d45b0944
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162353"
---
#   <a name="text-merge-cognitive-skill"></a>Metin Birleştirme bilişsel beceri

**Metin Birleştirme** becerisi, alan koleksiyonundaki metni tek bir alanda birleştirir. 

> [!NOTE]
> Bu beceri Bilişsel Hizmetler API'sine bağlı değildir ve bunu kullanmak için ücret alınmaz. Yine de, günlük günlük zenginleştirmeler az sayıda sizi sınırlayan **Ücretsiz** kaynak seçeneği geçersiz kılmak için, [bilişsel hizmetler kaynak eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.MergeSkill

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| eklemePreTag  | Her eklemeden önce eklenecek dize. Varsayılan değer: `" "`. Alanı atlamak için `""`değeri .  |
| insertPostTag | Her eklemeden sonra eklenecek dize. Varsayılan değer: `" "`. Alanı atlamak için `""`değeri .  |


##  <a name="sample-input"></a>Örnek giriş
Bu beceri için kullanılabilir girdi sağlayan bir JSON belge olabilir:

```json
{
  "values": [
    {
      "recordId": "1",
      "data":
      {
        "text": "The brown fox jumps over the dog",
        "itemsToInsert": ["quick", "lazy"],
        "offsets": [3, 28]
      }
    }
  ]
}
```

##  <a name="sample-output"></a>Örnek çıktı
Bu örnek, *insertPreTag'ın ,insertPostTag* olarak ayarlı `" "` *insertPostTag* olduğunu varsayarak önceki `""`girişin çıktısını gösterir. 

```json
{
  "values": [
    {
      "recordId": "1",
      "data":
      {
        "mergedText": "The quick brown fox jumps over the lazy dog"
      }
    }
  ]
}
```

## <a name="extended-sample-skillset-definition"></a>Genişletilmiş örnek beceri tanımı

Metin Birleştirme'yi kullanmak için yaygın bir senaryo, görüntülerin metingösterimini (OCR becerisinden metin veya görüntünün alt yazısı) belgenin içerik alanına birleştirmektir. 

Aşağıdaki örnek skillset, belgeye katıştak resimlerden metin ayıklamak için OCR becerisini kullanır. Ardından, her görüntüden hem özgün hem de OKK'lu metin içerecek bir *merged_text* alanı oluşturur. [Burada](https://docs.microsoft.com/azure/search/cognitive-search-skill-ocr)OCR beceri hakkında daha fazla bilgi edinebilirsiniz.

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
      "description": "Extract text (plain and structured) from image.",
      "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
      "context": "/document/normalized_images/*",
      "defaultLanguageCode": "en",
      "detectOrientation": true,
      "inputs": [
        {
          "name": "image",
          "source": "/document/normalized_images/*"
        }
      ],
      "outputs": [
        {
          "name": "text"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```
Yukarıdaki örnekte, normalleştirilmiş görüntüler alanı nın var olduğu varsayar. Normalleştirilmiş görüntüler alanını elde etmek için, aşağıda gösterildiği gibi *Normalleştirilmiş Görüntüler oluşturmak* için dizinleyici tanımınızda *imageAction* yapılandırmasını ayarlayın:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters":{
    "configuration":{
        "dataToExtract":"contentAndMetadata",
        "imageAction":"generateNormalizedImages"
    }
  }
}
```

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Dizin Oluşturucu Oluşturma (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
