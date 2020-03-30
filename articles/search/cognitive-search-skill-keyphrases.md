---
title: Anahtar Cümle çıkarma bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Yapılandırılmamış metni değerlendirir ve her kayıt için Azure Bilişsel Arama'da bir AI zenginleştirme ardışık hattındaki anahtar ifadelerin listesini döndürür.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: ccdd25d82af2b4893260af18dac818816d9e4579
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72791971"
---
#   <a name="key-phrase-extraction-cognitive-skill"></a>Anahtar Cümle çıkarma bilişsel beceri

**Anahtar Tümcecik Ayıklama** becerisi yapılandırılmamış metni değerlendirir ve her kayıt için anahtar ifadelerin listesini döndürür. Bu beceri, Bilişsel Hizmetlerde [Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sağlanan makine öğrenimi modellerini kullanır.

Bu özellik, kayıttaki ana konuşma noktalarını hızlı bir şekilde tanımlamanız gerekiyorsa yararlıdır. Örneğin, giriş metni verilen "Gıda lezzetli ve orada harika personel vardı", hizmet döner "gıda" ve "harika personel".

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.KeyPhraseExtractionSkill 

## <a name="data-limits"></a>Veri sınırları
Bir kaydın maksimum boyutu 50.000 karakter olarak [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçüldü. Verilerinizi anahtar tümcecik çıkarıcıya göndermeden önce ayırmanız gerekiyorsa, [Metin Bölme becerisini](cognitive-search-skill-textsplit.md)kullanmayı düşünün.

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Girişler                | Açıklama |
|---------------------|-------------|
| defaultLanguageCode | (İsteğe bağlı) Dili açıkça belirtmeyin belgelere uygulanacak dil kodu.  Varsayılan dil kodu belirtilmemişse, Varsayılan dil kodu olarak İngilizce (en) kullanılır. <br/> [Desteklenen dillerin tam listesine](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)bakın. |
| maxKeyPhraseCount   | (İsteğe bağlı) Üretilen en fazla anahtar tümcecik sayısı. |

## <a name="skill-inputs"></a>Beceri girdileri

| Girişler     | Açıklama |
|--------------------|-------------|
| metin | İncelenecek metin.|
| languageCode  |  Kayıtların dilini gösteren bir dize. Bu parametre belirtilmemişse, kayıtları çözümlemek için varsayılan dil kodu kullanılır. <br/>[Desteklenen dillerin tam listesine](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) bakın|

##  <a name="sample-definition"></a>Örnek tanımı

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      },
      {
        "name": "languageCode",
        "source": "/document/languagecode" 
      }
    ],
    "outputs": [
      {
        "name": "keyPhrases",
        "targetName": "myKeyPhrases"
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
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. They accumulate ice from snowfall and lose it through melting. As global temperatures have risen, many of the world’s glaciers have already started to shrink and retreat. Continued warming could see many iconic landscapes – from the Canadian Rockies to the Mount Everest region of the Himalayas – lose almost all their glaciers by the end of the century.",
             "language": "en"
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
            "keyPhrases": 
            [
              "world’s glaciers", 
              "huge rivers of ice", 
              "Canadian Rockies", 
              "iconic landscapes",
              "Mount Everest region",
              "Continued warming"
            ]
           }
      }
    ]
}
```


## <a name="errors-and-warnings"></a>Hatalar ve uyarılar
Desteklenmeyen bir dil kodu sağlarsanız, bir hata oluşturulur ve anahtar tümcecikler ayıklanmaz.
Metniniz boşsa, bir uyarı üretilir.
Metniniz 50.000 karakterden büyükse, yalnızca ilk 50.000 karakter analiz edilir ve bir uyarı verilir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
