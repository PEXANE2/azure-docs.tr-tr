---
title: Dil algılama bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Yapılandırılmamış metni değerlendirir ve her kayıt için, Azure Bilişsel Arama'daki bir AI zenginleştirme ardışık hattında analizin gücünü gösteren bir puaniçeren bir dil tanımlayıcısı döndürür.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 8439788c63ec1b9feaea148ab52aba498791dc12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045026"
---
#   <a name="language-detection-cognitive-skill"></a>Dil algılama bilişsel beceri

**Dil Algılama** becerisi giriş metninin dilini algılar ve istek üzerine gönderilen her belge için tek bir dil kodu bildirir. Dil kodu, çözümlemenin gücünü gösteren bir skorla eşlenir. Bu beceri, Bilişsel Hizmetlerde [Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sağlanan makine öğrenimi modellerini kullanır.

Bu özellik, özellikle metnin dilini diğer becerilere (örneğin, Duygu Analizi becerisi veya [Metin Bölme becerisi)](cognitive-search-skill-textsplit.md)girdi olarak sağlamanız gerektiğinde kullanışlıdır. [Sentiment Analysis skill](cognitive-search-skill-sentiment.md)

Dil algılama, Bing'in Metin Analizi için listelenen [desteklenen dil ve bölge](https://docs.microsoft.com/azure/cognitive-services/text-analytics/language-support) sayısını aşan doğal dil işleme kitaplıklarından yararlanır. Dillerin tam listesi yayınlanmaz, ancak yaygın olarak konuşulan tüm dillerin yanı sıra varyantları, lehçeleri ve bazı bölgesel ve kültürel dilleri içerir. Daha az kullanılan bir dilde ifade edilen içeriğiniz varsa, bir kodu döndürür mü görmek için [Dil Algılama API'sini deneyebilirsiniz.](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) Algılanamayan dilleriçin yanıt `unknown`.

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın maksimum boyutu 50.000 karakter olarak [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçüldü. Verilerinizi dil algılama becerisine göndermeden önce ayırmanız gerekiyorsa, [Metin Bölme becerisini](cognitive-search-skill-textsplit.md)kullanabilirsiniz.

## <a name="skill-inputs"></a>Beceri girdileri

Parametreler büyük/küçük harfe duyarlıdır.

| Girişler     | Açıklama |
|--------------------|-------------|
| metin | İncelenecek metin.|

## <a name="skill-outputs"></a>Beceri çıktıları

| Çıktı Adı    | Açıklama |
|--------------------|-------------|
| languageCode | Tanımlanan dil için ISO 6391 dil kodu. Örneğin, "en". |
| languageName | Dilin adı. Örneğin "İngilizce". |
| puan | 0 ile 1 arasında bir değer. Dilin doğru bir şekilde tanımlanma olasılığı. Cümlekarışık dillere sahipse, puan 1'den düşük olabilir.  |

##  <a name="sample-definition"></a>Örnek tanımı

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "languageCode",
        "targetName": "myLanguageCode"
      },
      {
        "name": "languageName",
        "targetName": "myLanguageName"
      },
      {
        "name": "score",
        "targetName": "myLanguageScore"
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
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. "
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Estamos muy felices de estar con ustedes."
           }
      }
    ]
```


##  <a name="sample-output"></a>Örnek çıktı

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
            {
              "languageCode": "en",
              "languageName": "English",
              "score": 1,
            }
      },
      {
        "recordId": "2",
        "data":
            {
              "languageCode": "es",
              "languageName": "Spanish",
              "score": 1,
            }
      }
    ]
}
```


## <a name="error-cases"></a>Hata örnekleri
Metin desteklenmeyen bir dilde ifade edilirse, bir hata oluşturulur ve dil tanımlayıcısı döndürülür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
