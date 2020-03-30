---
title: Metin Çevirisi bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Metni değerlendirir ve her kayıt için Azure Bilişsel Arama'da bir AI zenginleştirme ardışık alanında belirtilen hedef dile çevrilmiş metni döndürür.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 5089174fcfd5a97128c1f789b818243243a5282f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75460763"
---
#   <a name="text-translation-cognitive-skill"></a>Metin Çevirisi bilişsel beceri

**Metin Çevirisi** becerisi metni değerlendirir ve her kayıt için, çevrilen metni belirtilen hedef dile döndürür. Bu beceri, Bilişsel Hizmetler'de bulunan [Translator Text API v3.0'ı](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate) kullanır.

Bu özellik, belgelerinizin hepsinin tek bir dilde olmamasını bekliyorsanız, bu durumda çevirerek arama için dizin oluşturmadan önce metni tek bir dile normalleştirebilirsiniz.  Aynı metnin kopyalarının birden çok dilde kullanılabilmesi isteyebileceğiniz yerelleştirme kullanım örnekleri için de yararlıdır.

[Translator Text API v3.0](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) bölgesel olmayan bir Bilişsel Hizmettir, yani verilerinizin Azure Bilişsel Aramanızla veya bağlı Bilişsel Hizmetler kaynağınızla aynı bölgede kalacağı garanti edilmez.

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.TranslationSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın maksimum boyutu 50.000 karakter olarak [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçüldü. Metin çevirisi becerisine göndermeden önce verilerinizi ayırmanız gerekiyorsa, [Metin Bölme becerisini](cognitive-search-skill-textsplit.md)kullanmayı düşünün.

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Girişler                | Açıklama |
|---------------------|-------------|
| defaultToLanguageCode | (Gerekli) Dili açıkça belirtmeyen belgeler için belgeleri tercüme etmek için dil kodu. <br/> [Desteklenen dillerin tam listesine](https://docs.microsoft.com/azure/cognitive-services/translator/language-support)bakın. |
| defaultFromLanguageCode | (İsteğe bağlı) Dilden açıkça belirtmeyen belgeler için belgeleri tercüme etmek için dil kodu.  VarsayılanFromLanguageCode belirtilmemişse, dilden gelen dili belirlemek için Çevirmen Metin API'sı tarafından sağlanan otomatik dil algılama sı kullanılır. <br/> [Desteklenen dillerin tam listesine](https://docs.microsoft.com/azure/cognitive-services/translator/language-support)bakın. |
| önerilenFrom | (İsteğe bağlı) Ne fromLanguageCode girişi ne de varsayılan FromLanguageCode parametresi sağlandığında ve otomatik dil algılama başarısız olduğunda belgeleri çevirmek için dil kodu.  ÖnerilenFrom dili belirtilmemişse, İngilizce (en) önerilen From language olarak kullanılır. <br/> [Desteklenen dillerin tam listesine](https://docs.microsoft.com/azure/cognitive-services/translator/language-support)bakın. |

## <a name="skill-inputs"></a>Beceri girdileri

| Giriş adı     | Açıklama |
|--------------------|-------------|
| metin | Çevrilecek metin.|
| toLanguageCode    | Metnin çevrilmesi gereken dili gösteren bir dize. Bu giriş belirtilmemişse, metni çevirmek için varsayılan ToLanguageCode kullanılır. <br/>[Desteklenen dillerin tam listesine](https://docs.microsoft.com/azure/cognitive-services/translator/language-support) bakın|
| fromLanguageCode  | Metnin geçerli dilini gösteren bir dize. Bu parametre belirtilmemişse, metni çevirmek için varsayılan FromLanguageCode (veya varsayılanFromLanguageCode sağlanmamışsa otomatik dil algılama) kullanılır. <br/>[Desteklenen dillerin tam listesine](https://docs.microsoft.com/azure/cognitive-services/translator/language-support) bakın|

## <a name="skill-outputs"></a>Beceri çıktıları

| Çıktı adı    | Açıklama |
|--------------------|-------------|
| çevrilmişMetin | ÇevrilmişFromLanguageCode'dan çevrilmişToLanguageCode'a metin çevirisinin string sonucu.|
| translatedToLanguageCode  | Metnin çevrildiği dil kodunu gösteren dize. Birden çok dile çeviri yapmak ta fayda sağlamak ve hangi metnin hangi dil olduğunu takip edebilmek istiyorsanız kullanışlıdır.|
| çevrilmişFromLanguageCode    | Metnin çevrildiği dil kodunu gösteren bir dize. Bu çıktı size bu algılamanın sonucunu verecektir gibi otomatik dil algılama seçeneğini seçtiyseniz yararlıdır.|

##  <a name="sample-definition"></a>Örnek tanımı

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.TranslationSkill",
    "defaultToLanguageCode": "fr",
    "suggestedFrom": "en",
    "context": "/document",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "translatedText",
        "targetName": "translatedText"
      },
      {
        "name": "translatedFromLanguageCode",
        "targetName": "translatedFromLanguageCode"
      },
      {
        "name": "translatedToLanguageCode",
        "targetName": "translatedToLanguageCode"
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
          "text": "We hold these truths to be self-evident, that all men are created equal."
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
}
```


##  <a name="sample-output"></a>Örnek çıktı

```json
{
  "values": [
    {
      "recordId": "1",
      "data":
        {
          "translatedText": "Nous tenons ces vérités pour évidentes, que tous les hommes sont créés égaux.",
          "translatedFromLanguageCode": "en",
          "translatedToLanguageCode": "fr"
        }
    },
    {
      "recordId": "2",
      "data":
        {
          "translatedText": "Nous sommes très heureux d'être avec vous.",
          "translatedFromLanguageCode": "es",
          "translatedToLanguageCode": "fr"
        }
    }
  ]
}
```


## <a name="errors-and-warnings"></a>Hatalar ve uyarılar
Dilden veya gönderen için desteklenmeyen bir dil kodu sağlarsanız, bir hata oluşturulur ve metin çevrilmez.
Metniniz boşsa, bir uyarı üretilir.
Metniniz 50.000 karakterden büyükse, yalnızca ilk 50.000 karakter çevrilecek ve bir uyarı verilir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
