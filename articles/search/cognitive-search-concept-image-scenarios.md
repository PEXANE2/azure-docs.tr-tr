---
title: Görüntülerdeki metinleri ayıklama
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama ardışık hatlarındaki resimlerden metin ve diğer bilgileri işleme ve ayıkla.
manager: nitinme
author: LuisCabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 98054060210f55803d6e2811e1f494fd3ff00e48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76838267"
---
# <a name="how-to-process-and-extract-information-from-images-in-ai-enrichment-scenarios"></a>AI zenginleştirme senaryolarında görüntülerden bilgi işleme ve ayıklama

Azure Bilişsel Arama,resimler ve görüntü dosyalarıyla çalışmak için çeşitli özelliklere sahiptir. Belge kırma sırasında, dur işaretindeki "STOP" sözcüğü gibi alfanümerik metin içeren fotoğraflardan veya resimlerden metin ayıklamak için *imageAction* parametresini kullanabilirsiniz. Diğer senaryolar arasında, karahindiba fotoğrafı için "karahindiba" veya "sarı" rengi gibi bir görüntünün metin gösterimi oluşturmak yer almaktadır. Ayrıca, görüntünün boyutu gibi meta verileri de ayıklayabilirsiniz.

Bu makalede, görüntü işleme daha ayrıntılı olarak kapsar ve bir AI zenginleştirme ardışık boru hattı görüntüleri ile çalışmak için rehberlik sağlar.

<a name="get-normalized-images"></a>

## <a name="get-normalized-images"></a>Normalleştirilmiş görüntüler elde edin

Belge kırmanın bir parçası olarak, görüntü dosyalarını veya dosyalara katıştırılmış görüntüleri işlemek için yeni bir dizinleyici yapılandırma parametreleri kümesi vardır. Bu parametreler, daha fazla aşağı işlem için görüntüleri normalleştirmek için kullanılır. Görüntüleri normalleştirmek onları daha düzgün yapar. Büyük görüntüler, tüketilebilir hale getirmek için maksimum yüksekliğe ve genişliğe kadar yeniden boyutlandırılır. Yönlendirme üzerine meta veri sağlayan görüntüler için dikey yükleme için görüntü döndürme ayarlanır. Meta veri ayarlamaları, her görüntü için oluşturulan karmaşık bir türde yakalanır. 

Görüntü normalleştirmeyi kapatamazsınız. Görüntüler üzerinde tekrarlayan beceriler normalleştirilmiş görüntüler bekler. Bir dizinleyicide görüntü normalleştirmeyi etkinleştirmek için bir skillset'in bu dizinleyiciye eklenmesi gerekir.

| Yapılandırma Parametresi | Açıklama |
|--------------------|-------------|
| imageAction   | Katıştırılmış görüntüler veya görüntü dosyalarıyla karşılaşıldığında herhangi bir işlem yapılmazsa "yok" olarak ayarlayın. <br/>Belge kırmanın bir parçası olarak normalleştirilmiş görüntüler dizisi oluşturmak için "Normalleştirilmiş Görüntüler" oluşturmaya ayarlayın.<br/>Veri kaynağınızdaki PDF'ler için her sayfanın bir çıktı görüntüsüne işlendiği normalleştirilmiş görüntüler dizisi oluşturmak için "Normalize ImagePerPage" oluşturacak şekilde ayarlayın.  İşlevsellik, PDF olmayan dosya türleri için "GenerateNormalizedImages" ile aynıdır.<br/>"Hiçbiri" olmayan herhangi bir seçenek için, görüntüler *normalized_images* alanında açıklanır. <br/>Varsayılan değer "yok" olur. Bu yapılandırma yalnızca "dataToExtract" "contentAndMetadata" olarak ayarlandığında veri kaynaklarını şişirme ile ilgilidir. <br/>Belirli bir belgeden en fazla 1000 görüntü çıkarılır. Bir belgede 1000'den fazla görüntü varsa, ilk 1000 ayıklanır ve bir uyarı oluşturulur. |
|  normalleştirilmişImageMaxWidth | Oluşturulan normalleştirilmiş görüntüler için maksimum genişlik (piksel olarak). Varsayılan değer 2000’dir. İzin verilen maksimum değer 10000'dir. | 
|  normalleştirilmişImageMaxHeight | Oluşturulan normalleştirilmiş görüntüler için maksimum yükseklik (piksel olarak). Varsayılan değer 2000’dir. İzin verilen maksimum değer 10000'dir.|

> [!NOTE]
> *imageAction* özelliğini "yok" dışında bir şeye ayarlarsanız, *ayrıştırmaMode* özelliğini "varsayılan" dışında bir şeye ayarlayamazsınız.  Bu iki özelliklerden yalnızca birini dizin oluşturma yapılandırmanızda varsayılan olmayan bir değere ayarlayabilirsiniz.

**AyrıştırmaModu** parametresini `json` (her blob'u tek bir `jsonArray` belge olarak dizilemek için) veya (bloblarınız JSON dizileri içeriyorsa ve bir dizinin her öğesinin ayrı bir belge olarak ele alınması gerekiyorsa) ayarlayın.

Normalleştirilmiş görüntüler için maksimum genişlik ve yükseklik için 2000 piksel varsayılan [OCR beceri](cognitive-search-skill-ocr.md) ve [görüntü analizi beceri](cognitive-search-skill-image-analysis.md)tarafından desteklenen maksimum boyutlara dayanmaktadır. [OCR becerisi,](cognitive-search-skill-ocr.md) İngilizce olmayan diller için maksimum 4200, İngilizce için 10000 maksimum genişlik ve yüksekliği destekler.  Maksimum sınırları artırırsanız, skillset tanımınıza ve belgelerin diline bağlı olarak daha büyük görüntülerde işleme başarısız olabilir. 

[Dizinleyici tanımınızda](https://docs.microsoft.com/rest/api/searchservice/create-indexer) imageAction'u aşağıdaki gibi belirtirsiniz:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters":
  {
    "configuration": 
    {
        "dataToExtract": "contentAndMetadata",
        "imageAction": "generateNormalizedImages"
    }
  }
}
```

*imageAction* sonra "hiçbiri" bir değer olarak ayarlandığında, yeni *normalized_images* alanı bir dizi görüntü içerir. Her görüntü, aşağıdaki üyelere sahip karmaşık bir türdür:

| Görüntü üyesi       | Açıklama                             |
|--------------------|-----------------------------------------|
| veri               | NORMALleştirilmiş görüntünün Base64 kodlanmış dizesi JPEG formatında.   |
| genişlik              | Normalleştirilmiş görüntünün piksellerde genişliği. |
| yükseklik             | Normalleştirilmiş görüntünün piksellerde yüksekliği. |
| originalGenişlik      | Normalleştirmeden önce görüntünün orijinal genişliği. |
| originalYükseklik      | Normalleştirmeden önce görüntünün orijinal yüksekliği. |
| rotasyonFromOriginal |  Normalleştirilmiş görüntü oluşturmak için meydana gelen derecelerde saat yönünün tersine dönüş. 0 derece ile 360 derece arasında bir değer. Bu adım, bir kamera veya tarayıcı tarafından oluşturulan görüntüden meta verileri okur. Genellikle 90 derecenin bir katı. |
| içerikOffset | Görüntünün çıkarıldığı içerik alanında karakter ofset. Bu alan yalnızca katıştırılmış görüntülere sahip dosyalar için geçerlidir. |
| Pagenumber | Görüntü PDF'den çıkarılmış veya işlenmişse, bu alan 1'den başlayarak çıkarılan veya işlenen PDF'deki sayfa numarasını içerir.  Görüntü PDF'den değilse, bu alan 0 olacaktır.  |

 *normalized_images*örnek değeri :
```json
[
  {
    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
    "width": 500,
    "height": 300,
    "originalWidth": 5000,  
    "originalHeight": 3000,
    "rotationFromOriginal": 90,
    "contentOffset": 500,
    "pageNumber": 2
  }
]
```

## <a name="image-related-skills"></a>İmajla ilgili beceriler

Bir giriş olarak görüntü almak iki yerleşik bilişsel becerileri vardır: [OCR](cognitive-search-skill-ocr.md) ve [Görüntü Analizi.](cognitive-search-skill-image-analysis.md) 

Şu anda, bu beceriler yalnızca belge kırma adımından oluşturulan görüntülerle çalışır. Bu nedenle, desteklenen tek `"/document/normalized_images"`giriş.

### <a name="image-analysis-skill"></a>Görüntü Analizi becerisi

[Görüntü Analizi becerisi,](cognitive-search-skill-image-analysis.md) görüntü içeriğine dayalı zengin bir görsel özellik kümesini ayıklar. Örneğin, bir resimden resim yazısı oluşturabilir, etiketler oluşturabilir veya ünlüleri ve simgeleri tanımlayabilirsiniz.

### <a name="ocr-skill"></a>OCR becerisi

[OCR becerisi,](cognitive-search-skill-ocr.md) JpG'ler, PNG'ler ve bit eşlemler gibi görüntü dosyalarından metin ayıklar. Bu metin yanı sıra düzen bilgileri ayıklayabilirsiniz. Düzen bilgileri, tanımlanan dizeleri her biri için sınırlayıcı kutuları sağlar.

## <a name="embedded-image-scenario"></a>Gömülü görüntü senaryosu

Yaygın bir senaryo, aşağıdaki adımları gerçekleştirerek, hem metin hem de görüntü kökenli metin olmak üzere tüm dosya içeriğini içeren tek bir dize oluşturmayı içerir:  

1. [Ekstre normalized_images](#get-normalized-images)
1. Giriş olarak kullanarak `"/document/normalized_images"` OCR becerisini çalıştırın
1. Bu görüntülerin metin gösterimini dosyadan çıkarılan ham metinle birleştirin. [Metin Birleştirme](cognitive-search-skill-textmerger.md) becerisini, her iki metin parçasını da tek bir büyük dizede birleştirmek için kullanabilirsiniz.

Aşağıdaki örnek skillset, belgenizin metin içeriğini içeren *merged_text* bir alan oluşturur. Ayrıca, katıştırılmış görüntülerin her birinden OKRed metni ni içerir. 

#### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi
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

Artık merged_text bir alanınız olduğuna göre, dizin oluşturucu tanımınızda onu aranabilir bir alan olarak eşleyebilirsiniz. Resimlerin metni de dahil olmak üzere dosyalarınızın tüm içeriği aranabilir.

## <a name="visualize-bounding-boxes-of-extracted-text"></a>Çıkarılan metnin sınırlayıcı kutularını görselleştirin

Başka bir yaygın senaryo arama sonuçları düzen bilgilerini görselleştirmektir. Örneğin, arama sonuçlarınızın bir parçası olarak bir resimde bir metin parçasının nerede bulunduğunu vurgulamak isteyebilirsiniz.

OCR adımı normalleştirilmiş görüntülerde gerçekleştirildiğinden, düzen koordinatları normalleştirilmiş görüntü alanındadır. Normalleştirilmiş görüntüyü görüntülerken, koordinatların varlığı genellikle bir sorun değildir, ancak bazı durumlarda orijinal görüntüyü görüntülemek isteyebilirsiniz. Bu durumda, düzendeki koordinat noktalarının her birini özgün görüntü koordinat sistemine dönüştürün. 

Yardımcı olarak, normalleştirilmiş koordinatları özgün koordinat alanına dönüştürmeniz gerekiyorsa, aşağıdaki algoritmayı kullanabilirsiniz:

```csharp
        /// <summary>
        ///  Converts a point in the normalized coordinate space to the original coordinate space.
        ///  This method assumes the rotation angles are multiples of 90 degrees.
        /// </summary>
        public static Point GetOriginalCoordinates(Point normalized,
                                    int originalWidth,
                                    int originalHeight,
                                    int width,
                                    int height,
                                    double rotationFromOriginal)
        {
            Point original = new Point();
            double angle = rotationFromOriginal % 360;

            if (angle == 0 )
            {
                original.X = normalized.X;
                original.Y = normalized.Y;
            } else if (angle == 90)
            {
                original.X = normalized.Y;
                original.Y = (width - normalized.X);
            } else if (angle == 180)
            {
                original.X = (width -  normalized.X);
                original.Y = (height - normalized.Y);
            } else if (angle == 270)
            {
                original.X = height - normalized.Y;
                original.Y = normalized.X;
            }

            double scalingFactor = (angle % 180 == 0) ? originalHeight / height : originalHeight / width;
            original.X = (int) (original.X * scalingFactor);
            original.Y = (int)(original.Y * scalingFactor);

            return original;
        }
```

## <a name="see-also"></a>Ayrıca bkz.
+ [Dizin oluştur (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
+ [Görüntü Analizi becerisi](cognitive-search-skill-image-analysis.md)
+ [OCR becerisi](cognitive-search-skill-ocr.md)
+ [Metin birleştirme becerisi](cognitive-search-skill-textmerger.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Zenginleştirilmiş alanların haritası nasıl eşlenir?](cognitive-search-output-field-mapping.md)
