---
title: Belge ayıklama Bilişsel Beceri (Önizleme)
titleSuffix: Azure Cognitive Search
description: Zenginleştirme işlem hattının içindeki bir dosyadan içerik ayıklar. Bu yetenek Şu anda genel önizlemededir.
manager: nitinme
author: careyjmac
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: chalton
ms.openlocfilehash: 0f67caad03c4ebd1cf8f3721f377d8362219016a
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76837740"
---
# <a name="document-extraction-cognitive-skill"></a>Belge ayıklama bilişsel yeteneği

> [!IMPORTANT] 
> Bu yetenek Şu anda genel önizlemededir. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Şu anda portal veya .NET SDK desteği yok.

**Belge ayıklama** becerisi, zenginleştirme işlem hattının içindeki bir dosyadan içerik ayıklar. Bu, normalde, diğer yetenekler tarafından oluşturulabilecek dosyalarla beceri yürütmeden önce gerçekleşen belge ayıklama adımının avantajlarından yararlanmanızı sağlar.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve dizin oluşturma aşamasında belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücret tahakkuk eder. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması [fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.
## <a name="odatatype"></a>@odata.type  
Microsoft. yetenekler. util. Belgetextractionbeceri

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Girişler            | İzin Verilen Değerler | Açıklama |
|-----------------|----------------|-------------|
| `parsingMode`   | `default` <br/> `text` <br/> `json`  | Saf metin veya JSON olmayan dosyalardan belge ayıklama için `default` olarak ayarlayın. Düz metin dosyalarında performansı artırmak için `text` olarak ayarlayın. JSON dosyalarından yapılandırılmış içeriği ayıklamak için `json` olarak ayarlayın. `parsingMode` açıkça tanımlanmamışsa, `default`olarak ayarlanır. |
| `dataToExtract` | `contentAndMetadata` <br/> `allMetadata` | Her dosyadan tüm meta verileri ve metin içeriğini ayıklamak için `contentAndMetadata` olarak ayarlayın. Yalnızca [içerik türü belirli meta verileri](search-howto-indexing-azure-blob-storage.md#ContentSpecificMetadata) (örneğin, yalnızca. png dosyalarına özgü meta veriler) ayıklamak için `allMetadata` olarak ayarlayın. `dataToExtract` açıkça tanımlanmamışsa, `contentAndMetadata`olarak ayarlanır. |
| `configuration` | Aşağıya bakın. | Belge ayıklamanın nasıl gerçekleştirileceğini belirten isteğe bağlı parametrelerin bir sözlüğü. Desteklenen yapılandırma özelliklerinin açıklamaları için aşağıdaki tabloya bakın. |

| Yapılandırma parametresi   | İzin Verilen Değerler | Açıklama |
|-------------------------|----------------|-------------|
| `imageAction`           | `none`<br/> `generateNormalizedImages`<br/> `generateNormalizedImagePerPage` | Veri kümesindeki gömülü görüntüleri veya resim dosyalarını yoksaymak için `none` olarak ayarlayın. Bu varsayılandır. <br/>Bilişsel [becerileri kullanan görüntü analizi](cognitive-search-concept-image-scenarios.md)için, niteliğin belge çözme kapsamında normalleştirilmiş görüntüler dizisi oluşturmasını sağlamak üzere `generateNormalizedImages` olarak ayarlayın. Bu eylem `parsingMode` `default` olarak ayarlanmasını ve `dataToExtract` `contentAndMetadata`olarak ayarlanmasını gerektirir. Normalleştirilmiş bir görüntü, görsel arama sonuçlarına görüntü eklediğinizde (örneğin, [JFK demo](https://github.com/Microsoft/AzureSearch_JFK_Files)bölümünde görüldüğü gibi bir grafik denetimindeki aynı boyutlu fotoğraflar gibi) tutarlı işleme sağlamak için boyutlandırılmış ve döndürülen Tekdüzen görüntü çıkışına neden olan diğer işleme başvurur. Bu bilgi, bu seçeneği kullandığınızda her bir görüntü için oluşturulur.  <br/>`generateNormalizedImagePerPage`ayarlarsanız, PDF dosyaları gömülü görüntüleri ayıklamak yerine farklı şekilde değerlendirilir, her sayfa bir görüntü olarak işlenir ve buna göre normalleştirilirler.  PDF olmayan dosya türleri `generateNormalizedImages` ayarlandıysa aynı kabul edilir.
| `normalizedImageMaxWidth` | 50-10000 arasında herhangi bir tamsayı | Oluşturulan normalleştirilmiş görüntülerin en büyük genişliği (piksel cinsinden). Varsayılan değer 2000 ' dir. | 
| `normalizedImageMaxHeight` | 50-10000 arasında herhangi bir tamsayı | Oluşturulan normalleştirilmiş görüntülerin en büyük yüksekliği (piksel cinsinden). Varsayılan değer 2000 ' dir. |

> [!NOTE]
> Normalleştirilmiş görüntüler için varsayılan değer olan 2000 piksel en fazla genişlik ve yükseklik, [OCR becerisi](cognitive-search-skill-ocr.md) ve [görüntü analizi yeteneği](cognitive-search-skill-image-analysis.md)tarafından desteklenen boyut üst sınırını temel alır. [OCR becerisi](cognitive-search-skill-ocr.md) , İngilizce dışındaki diller için maksimum genişlik ve yükseklik 4200 ve ingilizce için 10000 ' i destekler.  Maksimum sınırları artırırsanız, Beceri tanımınıza ve belgelerin diline bağlı olarak daha büyük görüntülerde işleme başarısız olabilir. 
## <a name="skill-inputs"></a>Beceri girişleri

| Giriş adı     | Açıklama |
|--------------------|-------------|
| file_data | İçeriğin ayıklanabilmesi gereken dosya. |

"File_data" girişi şöyle tanımlanmış bir nesne olmalıdır:

```json
{
  "$type": "file",
  "data": "BASE64 encoded string of the file"
}
```

Bu dosya başvuru nesnesi 3 farklı bir şekilde oluşturulabilir:

 - Dizin Oluşturucu tanımınızda `allowSkillsetToReadFileData` parametresi "true" olarak ayarlanıyor.  Bu işlem, blob veri kaynağınızdan indirilen özgün dosya verilerini temsil eden bir nesne olan `/document/file_data` bir yol oluşturur. Bu parametre yalnızca blob depolamada bulunan veriler için geçerlidir.

 - Dizin Oluşturucu tanımınızda `imageAction` parametresi `none`dışında bir değere ayarlanıyor.  Bu, tek tek geçirilirse (yani `/document/normalized_images/*`), bu beceriye giriş için gerekli kuralı izleyen bir görüntü dizisi oluşturur.

 - Özel bir beceriye sahip olmak, yukarıdaki gibi tam olarak tanımlanmış bir JSON nesnesi döndürür.  `$type` parametresi tam olarak `file` olarak ayarlanmalıdır ve `data` parametresi dosya içeriğinin temel 64 kodlu bayt dizisi verileri olmalıdır.

## <a name="skill-outputs"></a>Yetenek çıkışları

| Çıkış adı    | Açıklama |
|--------------|-------------|
| content | Belgenin metinsel içeriği. |
| normalized_images | `imageAction`, `none`başka bir değere ayarlandığında, yeni *normalized_images* alanı bir görüntü dizisi içerir. Her bir görüntünün çıkış biçimi hakkında daha fazla bilgi için bkz. [görüntü ayıklama belgeleri](cognitive-search-concept-image-scenarios.md) . |

##  <a name="sample-definition"></a>Örnek tanım

```json
 {
    "@odata.type": "#Microsoft.Skills.Util.DocumentExtractionSkill",
    "parsingMode": "default",
    "dataToExtract": "contentAndMetadata",
    "configuration": {
        "imageAction": "generateNormalizedImages",
        "normalizedImageMaxWidth": 2000,
        "normalizedImageMaxHeight": 2000
    },
    "context": "/document",
    "inputs": [
      {
        "name": "file_data",
        "source": "/document/file_data"
      }
    ],
    "outputs": [
      {
        "name": "content",
        "targetName": "content"
      },
      {
        "name": "normalized_images",
        "targetName": "normalized_images"
      }
    ]
  }
```

##  <a name="sample-input"></a>Örnek giriş

```json
{
  "values": [
    {
      "recordId": "1",
      "data":
      {
        "file_data": {
          "$type": "file",
          "data": "aGVsbG8="
        }
      }
    }
  ]
}
```


##  <a name="sample-output"></a>Örnek çıktı

```json
{
  "values": [
    {
      "recordId": "1",
      "data": {
        "content": "hello",
        "normalized_images": []
      }
    }
  ]
}
```

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Bilişsel arama senaryolarında görüntülerden bilgi işleme ve çıkarma](cognitive-search-concept-image-scenarios.md)