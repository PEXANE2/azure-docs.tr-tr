---
title: Belge Çıkarma bilişsel beceri (önizleme)
titleSuffix: Azure Cognitive Search
description: Zenginleştirme ardışık alanı içindeki bir dosyadan içerik ayıklar. Bu beceri şu anda genel önizlemede.
manager: nitinme
author: careyjmac
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: chalton
ms.openlocfilehash: 0f67caad03c4ebd1cf8f3721f377d8362219016a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76837740"
---
# <a name="document-extraction-cognitive-skill"></a>Belge Çıkarma bilişsel beceri

> [!IMPORTANT] 
> Bu beceri şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. Şu anda portal veya .NET SDK desteği yok.

**Belge Ayıklama** becerisi, zenginleştirme ardışık alanındaki bir dosyadan içerik ayıklar. Bu, normalde diğer beceriler tarafından oluşturulabilir dosyaları ile skillset yürütme önce olur belge ayıklama adımı yararlanmak için izin verir.

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve dizin oluşturmada belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Resim çıkarma [fiyatlandırması fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.
## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.DocumentExtractionSkill

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Girişler            | İzin Verilen Değerler | Açıklama |
|-----------------|----------------|-------------|
| `parsingMode`   | `default` <br/> `text` <br/> `json`  | Saf `default` metin veya json olmayan dosyalardan belge ayıklama için ayarlayın. Düz `text` metin dosyalarındaki performansı artırmak için ayarlayın. Json `json` dosyalarından yapılandırılmış içerik ayıklamak için ayarlayın. Açıkça `parsingMode` tanımlanmamışsa, '' olarak `default`ayarlanır. |
| `dataToExtract` | `contentAndMetadata` <br/> `allMetadata` | Her `contentAndMetadata` dosyadaki tüm meta verileri ve metin içeriğini ayıklamak için ayarlayın. Yalnızca `allMetadata` [içerik türüne özgü meta verileri](search-howto-indexing-azure-blob-storage.md#ContentSpecificMetadata) ayıklamak için ayarlayın (örneğin, yalnızca .png dosyalarına özgü meta veriler). Açıkça `dataToExtract` tanımlanmamışsa, '' olarak `contentAndMetadata`ayarlanır. |
| `configuration` | Aşağıya bakın. | Belge ayıklamanın nasıl gerçekleştirildiğini ayarlayan isteğe bağlı parametreler sözlüğü. Desteklenen yapılandırma özelliklerinin açıklamaları için aşağıdaki tabloya bakın. |

| Yapılandırma Parametresi   | İzin Verilen Değerler | Açıklama |
|-------------------------|----------------|-------------|
| `imageAction`           | `none`<br/> `generateNormalizedImages`<br/> `generateNormalizedImagePerPage` | Veri `none` kümesinde katışılmış görüntüleri veya görüntü dosyalarını yok sayacak şekilde ayarlayın. Bu varsayılandır. <br/>[Bilişsel becerileri kullanarak görüntü](cognitive-search-concept-image-scenarios.md)analizi `generateNormalizedImages` için, beceri belge çatlama bir parçası olarak normalleştirilmiş görüntülerin bir dizi oluşturmak için ayarlayın. Bu eylem `parsingMode` için `default` ayarlanmış `dataToExtract` ve ayarlanır `contentAndMetadata`gerektirir. Normalleştirilmiş bir görüntü, görsel arama sonuçlarına görüntüleri eklediğinizde tutarlı görüntülemeyi (örneğin, [JFK demosunda](https://github.com/Microsoft/AzureSearch_JFK_Files)görüldüğü gibi grafik denetimindeki aynı boyuttaki fotoğraflar) teşvik etmek için tek tip görüntü çıkışıyla sonuçlanan, boyutlandırılmış ve döndürülen ek işleme anlamına gelir. Bu seçeneği kullandığınızda, bu bilgiler her görüntü için oluşturulur.  <br/>Eğer `generateNormalizedImagePerPage`ayarlarsanız, PDF dosyaları gömülü görüntüleri ayıklamak yerine farklı şekilde ele alınacaktır, her sayfa bir görüntü olarak işlenir ve buna göre normalleştirilmiş.  PDF olmayan dosya türleri ayarlanmış gibi `generateNormalizedImages` aynı şekilde işlem göreceksiniz.
| `normalizedImageMaxWidth` | 50-10000 arasında herhangi bir tümsavar | Oluşturulan normalleştirilmiş görüntüler için maksimum genişlik (piksel olarak). Varsayılan değer 2000’dir. | 
| `normalizedImageMaxHeight` | 50-10000 arasında herhangi bir tümsavar | Oluşturulan normalleştirilmiş görüntüler için maksimum yükseklik (piksel olarak). Varsayılan değer 2000’dir. |

> [!NOTE]
> Normalleştirilmiş görüntüler için maksimum genişlik ve yükseklik için 2000 piksel varsayılan [OCR beceri](cognitive-search-skill-ocr.md) ve [görüntü analizi beceri](cognitive-search-skill-image-analysis.md)tarafından desteklenen maksimum boyutlara dayanmaktadır. [OCR becerisi,](cognitive-search-skill-ocr.md) İngilizce olmayan diller için maksimum 4200, İngilizce için 10000 maksimum genişlik ve yüksekliği destekler.  Maksimum sınırları artırırsanız, skillset tanımınıza ve belgelerin diline bağlı olarak daha büyük görüntülerde işleme başarısız olabilir. 
## <a name="skill-inputs"></a>Beceri girdileri

| Giriş adı     | Açıklama |
|--------------------|-------------|
| file_data | İçeriğin ayıklanması gereken dosya. |

"file_data" girişi aşağıdaki gibi tanımlanmış bir nesne olmalıdır:

```json
{
  "$type": "file",
  "data": "BASE64 encoded string of the file"
}
```

Bu dosya başvuru nesnesi 3 şekilde n içinden biri oluşturulabilir:

 - Dizinleyici tanımınızdaki parametreyi `allowSkillsetToReadFileData` "true" olarak ayarlama.  Bu, blob `/document/file_data` veri kaynağınızdan indirilen özgün dosya verilerini temsil eden bir nesne olan bir yol oluşturur. Bu parametre yalnızca Blob depolamadaki veriler için geçerlidir.

 - Dizinleyici tanımınızdaki parametreyi `imageAction` ' den `none`başka bir değere ayarlama  Bu, tek tek geçilmesi durumunda bu beceriye giriş için gerekli kuralı izleyen `/document/normalized_images/*`bir dizi görüntü oluşturur (örn. ).

 - Özel bir beceri dönüş bir json nesnesi tam olarak yukarıda olarak tanımlanan olması.  Parametre `$type` tam olarak `file` ayarlanmalıdır `data` ve parametre dosya içeriğinin kodlanmış 64 kodlu bayt dizi verileri olmalıdır.

## <a name="skill-outputs"></a>Beceri çıktıları

| Çıktı adı    | Açıklama |
|--------------|-------------|
| content | Belgenin metin içeriği. |
| normalized_images | `imageAction` Bir değer diğer sonra `none`ayarlandığında, yeni *normalized_images* alanı görüntülerin bir dizi içerecektir. Her görüntünün çıktı biçimi hakkında daha fazla bilgi için [görüntü ayıklama için belgelere](cognitive-search-concept-image-scenarios.md) bakın. |

##  <a name="sample-definition"></a>Örnek tanımı

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
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Bilişsel arama senaryolarında görüntülerden bilgi işleme ve ayıklama](cognitive-search-concept-image-scenarios.md)