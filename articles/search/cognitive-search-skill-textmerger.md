---
title: Metin birleştirme bilişsel arama yeteneği-Azure Search
description: Bir alan koleksiyonundan metin birleştirme birleştirilmiş bir alan. Bu bilişsel yeteneği bir Azure Search zenginleştirme ardışık düzeninde kullanın.
services: search
manager: nitinme
author: luiscabrer
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.openlocfilehash: 1e88fcc13d97d92cf9b35616ecb7d71c2d24db1f
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265270"
---
#    <a name="text-merge-cognitive-skill"></a>Metin birleştirme Bilişsel Beceri

**Metin birleştirme** yeteneği, bir alan koleksiyonundan metni tek bir alanda birleştirir. 

> [!NOTE]
> Bu yetenek bilişsel hizmetler API 'SI ile bağlantılı değildir ve bunu kullanmak için ücretlendirilirsiniz. Yine de bir bilişsel [Hizmetler kaynağı iliştirmelisiniz](cognitive-search-attach-cognitive-services.md), ancak her gün çok az sayıda günlük zenginleştirme için sizi sınırlayan **ücretsiz** Kaynak seçeneğini geçersiz kılabilirsiniz.

## <a name="odatatype"></a>@odata.type  
Microsoft. yetenekler. Text. Mergeskıll

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| ınsertpretag  | Her ekleme işleminden önce eklenecek dize. Varsayılan değer `" "` şeklindedir. Alanı atlamak için değerini olarak `""`ayarlayın.  |
| ınsertposttag | Her ekleme işleminden sonra eklenecek dize. Varsayılan değer `" "` şeklindedir. Alanı atlamak için değerini olarak `""`ayarlayın.  |


##  <a name="sample-input"></a>Örnek giriş
Bu yetenek için kullanılabilir giriş sağlayan bir JSON belgesi şu olabilir:

```json
{
  "values": [
    {
      "recordId": "1",
      "data":
      {
        "text": "The brown fox jumps over the dog",
        "itemsToInsert": ["quick", "lazy"],
        "offsets": [3, 28],
      }
    }
  ]
}
```

##  <a name="sample-output"></a>Örnek çıktı
Bu örnek, *ınsertpretag* öğesinin olarak ayarlandığı `" "`ve *ınsertposttag* 'in olarak `""`ayarlandığı varsayıldığında, önceki girdinin çıktısını gösterir. 

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

Metin birleştirme kullanmanın yaygın bir senaryosu, görüntülerin metinsel gösterimini (bir OCR becerinden gelen metin veya bir görüntünün resim yazısı) belgenin içerik alanına birleştirmektir. 

Aşağıdaki örnek Beceri, belgeye katıştırılmış görüntülerden metin ayıklamak için OCR becerisi kullanır. Sonra, her görüntüden hem orijinal hem de OCRed metin içeren bir *merged_text* alanı oluşturur. [Burada](https://docs.microsoft.com/azure/search/cognitive-search-skill-ocr)OCR yeteneği hakkında daha fazla bilgi edinebilirsiniz.

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
Yukarıdaki örnekte, normalleştirilmiş görüntüler alanının var olduğu varsayılır. Normalleştirilmiş görüntüler alanını almak için, Indexer tanımınızdaki *ımageaction* yapılandırmasını aşağıda gösterildiği gibi *Generatenormalizediges* olarak ayarlayın:

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

+ [Önceden tanımlanmış yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Dizin Oluşturucu oluştur (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
