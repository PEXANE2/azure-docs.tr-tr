---
title: OCR bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da bir zenginleştirme ardışık alanında optik karakter tanıma (OCR) kullanarak görüntü dosyalarından metin ayıklayın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: bdb510113a8d65ac04b54e77158f46d03cccd9de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72791917"
---
# <a name="ocr-cognitive-skill"></a>OCR bilişsel beceri

**Optik karakter tanıma (OCR)** becerisi, görüntü dosyalarında yazdırılan ve el yazısıyla yazılmış metni tanır. Bu beceri, [Bilişsel Hizmetler'de Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) tarafından sağlanan makine öğrenme modellerini kullanır. **OCR** beceri aşağıdaki işlevsellik eşler:

+ ["OCR"](../cognitive-services/computer-vision/concept-recognizing-text.md#ocr-optical-character-recognition-api) API'si İngilizce dışındaki diller için kullanılır. 
+ İngilizce için yeni ["Read"](../cognitive-services/computer-vision/concept-recognizing-text.md#read-api) API kullanılır.

**OCR** becerisi görüntü dosyalarından metin ayıklar. Desteklenen dosya biçimleri şunlardır:

+ . Jpeg
+ . Jpg
+ . Png
+ . Bmp
+ . Gıf
+ . Tıff

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.


## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| algılama Oryantasyon | Görüntü yönünün otomatik olarak algılanmasını sağlar. <br/> Geçerli değerler: true / false.|
|defaultLanguageCode | <p>  Giriş metninin dil kodu. Desteklenen diller: <br/> zh-Hans (Çince Basitleştirilmiş) <br/> zh-Hant (ÇinceGeleneksel) <br/>cs (Çekçe) <br/>da (Danca) <br/>nl (Felemenkçe) <br/>tr (İngilizce) <br/>fi (Fince)  <br/>fr (Fransızca) <br/>  de (Almanca) <br/>el (Yunanca) <br/> hu (Macarca) <br/> o (İtalyanca) <br/>  ja (Japonca) <br/> ko (Korece) <br/> nb (Norveççe) <br/>   pl (Lehçe) <br/> pt (Portekizce) <br/>  ru (Rusça) <br/>  es (İspanyolca) <br/>  sv (İsveççe) <br/>  tr (Türkçe) <br/> ar (Arapça) <br/> ro (Romence) <br/> sr-Cyrl (SırpKiril) <br/> sr-Latn (Sırplatincesi) <br/>  sk (Slovakça). <br/>  unk (Bilinmiyor) <br/><br/> Dil kodu belirtilmemiş veya null ise, dil İngilizce olarak ayarlanır. Dil açıkça "unk" olarak ayarlanırsa, dil otomatik olarak algılanır. </p> |
|lineEnding | Algılanan her satır arasında kullanılacak değer. Olası değerler: 'Space','CarriageReturn','LineFeed'.  Varsayılan 'Boşluk' |

Daha önce, beceri "yazdırılmış" veya "el yazısı" metin ayıklamak gerektiğini belirtmek için "textExtractionAlgorithm" adlı bir parametre vardı.  En son Okuma API algoritması her iki metin türünü aynı anda ayıklama yeteneğine sahip olduğundan, bu parametre amortismana alınır ve artık gerekli değildir.  Beceri tanımınız zaten bu parametreyi içeriyorsa, kaldırmanız gerekmez, ancak artık kullanılmaz ve her iki metin türü de ne olarak ayarlandığına bakılmaksızın ileriye doğru ayıklanır.

## <a name="skill-inputs"></a>Beceri girdileri

| Giriş adı      | Açıklama                                          |
|---------------|------------------------------------------------------|
| image         | Karmaşık Tip. Şu anda yalnızca Azure Blob dizinleyicisi ```imageAction``` tarafından üretilen "/document/normalized_images" ```none```alanı yla çalışır. Daha fazla bilgi için [örneğe](#sample-output) bakın.|


## <a name="skill-outputs"></a>Beceri çıktıları
| Çıktı adı     | Açıklama                   |
|---------------|-------------------------------|
| metin          | Görüntüden çıkarılan düz metin.   |
| düzenMetin    | Çıkarılan metni ve metnin bulunduğu konumu açıklayan karmaşık yazı.|


## <a name="sample-definition"></a>Örnek tanımı

```json
{
  "skills": [
    {
      "description": "Extracts text (plain and structured) from image.",
      "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
      "context": "/document/normalized_images/*",
      "defaultLanguageCode": null,
      "detectOrientation": true,
      "inputs": [
        {
          "name": "image",
          "source": "/document/normalized_images/*"
        }
      ],
      "outputs": [
        {
          "name": "text",
          "targetName": "myText"
        },
        {
          "name": "layoutText",
          "targetName": "myLayoutText"
        }
      ]
    }
  ]
}
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Örnek metin ve düzenMetin çıktısı

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Örnek: Katıştılı resimlerden çıkarılan metni belgenin içeriğiyle birleştirme.

Metin Birleştirme için ortak kullanım örneği, görüntülerin metinsel temsilini (OCR becerisinden metin veya görüntünün alt yazısı) belgenin içerik alanına birleştirme yeteneğidir.

Aşağıdaki örnek skillset *merged_text* bir alan oluşturur. Bu alan, belgenizin metin içeriğini ve bu belgeye katıştırılmış görüntülerin her birinden OKK'lu metni içerir.

#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
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
Yukarıdaki skillset örneği, normalleştirilmiş görüntüler alanının var olduğunu varsayar. Bu alanı oluşturmak için, aşağıda gösterildiği gibi *Normalize Görüntüler oluşturmak* için indexer tanımınızda *imageAction* yapılandırmasını ayarlayın:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters": {
    "configuration": {
      "dataToExtract":"contentAndMetadata",
      "imageAction":"generateNormalizedImages"
    }
  }
}
```

## <a name="see-also"></a>Ayrıca bkz.
+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [TextMerger beceri](cognitive-search-skill-textmerger.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Dizin Oluşturucu Oluşturma (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
