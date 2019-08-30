---
title: Dil algılama bilişsel arama yeteneği-Azure Search
description: Yapılandırılmamış metni değerlendirir ve her kayıt için bir Azure Search zenginleştirme ardışık düzeninde analizin şiddetini belirten puanla bir dil tanımlayıcısı döndürür.
services: search
manager: nitinme
author: luiscabrer
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.subservice: cognitive-search
ms.openlocfilehash: 8375d41e3ece3bdd97b218925a8b1b6d45fc327d
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186371"
---
#   <a name="language-detection-cognitive-skill"></a>Dil algılama Bilişsel Beceri

**Dil algılama** Beceri, giriş metninin dilini algılar ve istekte gönderilen her belge için tek bir dil kodu bildirir. Dil kodu, çözümlemenin gücünü gösteren bir puanla eşleştirilir. Bu beceri bilişsel hizmetler 'de [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sunulan makine öğrenimi modellerini kullanır.

Bu özellik özellikle, örneğin [yaklaşım Analizi beceri](cognitive-search-skill-sentiment.md) veya [metin bölünmüş beceri](cognitive-search-skill-textsplit.md)gibi, metnin dilini girmeniz gerektiğinde faydalıdır.

Dil algılama, Bing 'in doğal dil işleme kitaplıklarını kullanır ve bu, Metin Analizi için listelenen [desteklenen dil ve bölge](https://docs.microsoft.com/azure/cognitive-services/text-analytics/language-support) sayısını aşıyor. Dillerin tam listesi yayımlanmaz, ancak tüm yaygın olarak konuşulan dillerin yanı sıra çeşitleri, diapacts ve bazı bölgesel ve kültürel dillerini içerir. Daha az sıklıkta kullanılan bir dilde ifade ettiğiniz bir içeriğiniz varsa, bir kodu döndürüp döndürdüğünü görmek için [DIL ALGıLAMA API 'sini deneyebilirsiniz](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) . Tespit edilemez dillerin yanıtı `unknown`.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Search içinde belge çözme aşamasının bir parçası olarak görüntü ayıklama için tahakkuk ücretleri. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması [Azure Search fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.


## <a name="odatatype"></a>@odata.type  
Microsoft. yetenekler. Text. LanguageDetectionSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu, tarafından [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçülen 50.000 karakter olmalıdır. Bu verileri yaklaşım Çözümleyicisi 'ne göndermeden önce bölmeniz gerekirse, [metin bölme becerisine](cognitive-search-skill-textsplit.md)sahip olabilirsiniz.

## <a name="skill-inputs"></a>Beceri girişleri

Parametreler büyük/küçük harfe duyarlıdır.

| Girişler     | Açıklama |
|--------------------|-------------|
| text | Çözümlenecek metin.|

## <a name="skill-outputs"></a>Yetenek çıkışları

| Çıkış adı    | Açıklama |
|--------------------|-------------|
| languageCode | Tanımlanan dilin ISO 6391 dil kodu. Örneğin, "en". |
| languageName | Dilin adı. Örneğin, "Ingilizce". |
| puan | 0 ile 1 arasında bir değer. Dilin doğru şekilde tanımlanması olasılığı. Tümcede karışık diller varsa puan 1 ' den düşük olabilir.  |

##  <a name="sample-definition"></a>Örnek tanım

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


## <a name="error-cases"></a>Hata durumları
Metin desteklenmeyen bir dilde ifade edildiğinde bir hata oluşturulur ve hiçbir dil tanımlayıcısı döndürülmez.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
