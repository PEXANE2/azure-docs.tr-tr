---
title: Metin bölünmüş bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'daki bir AI zenginleştirme ardışık hattındaki uzunluğa göre metni parçalara veya metin sayfalarına ayırın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 3f80169808b1e6420f04b786d2bb06bde9c96231
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73479662"
---
# <a name="text-split-cognitive-skill"></a>Metin bölünmüş bilişsel beceri

**Metin Bölme** becerisi metni metin yığınlarına böler. Metni cümlelere mi yoksa belirli bir uzunluktaki sayfalara mı ayırmak istediğinizi belirtebilirsiniz. Bu beceri, özellikle akış aşağı diğer becerilerde maksimum metin uzunluğu gereksinimleri varsa yararlıdır. 

> [!NOTE]
> Bu beceri Bilişsel Hizmetler API'sine bağlı değildir ve bunu kullanmak için ücret alınmaz. Yine de, günlük günlük zenginleştirmeler az sayıda sizi sınırlayan **Ücretsiz** kaynak seçeneği geçersiz kılmak için, [bilişsel hizmetler kaynak eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Beceri Parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| textSplitMode      | Ya "sayfalar" ya da "cümleler" | 
| maksimumPageLength | textSplitMode "sayfalar" olarak ayarlanmışsa, bu, `String.Length`'ile ölçülen maksimum sayfa uzunluğuna başvurur. Minimum değer 100'dür.  TextSplitMode "sayfalar" olarak ayarlanmışsa, algoritma metni en fazla "maximumPageLength" boyutunda olan parçalara bölmeye çalışır. Bu durumda, algoritma cümleyi bir cümle sınırında kırmak için elinden geleni yapar, bu nedenle östen boyutu "maksimum PageLength"tan biraz daha az olabilir. | 
| defaultLanguageCode   | (isteğe bağlı) Aşağıdaki dil kodlarından `da, de, en, es, fi, fr, it, ko, pt`biri: . Varsayılan, İngilizce (tr) olur. Göz önünde bulundurulması gereken birkaç şey:<ul><li>Bir languagecode-countrycode biçimini geçerseniz, biçimin yalnızca languagecode bölümü kullanılır.</li><li>Dil önceki listede değilse, bölme becerisi karakter sınırlarında metni kırar.</li><li>Dil kodu sağlamak, Çince, Japonca ve Korece gibi beyaz olmayan diller için bir sözcüğü yarıya kesmekten kaçınmak için yararlıdır.</li><li>Dili bilmiyorsanız (yani [LanguageDetectionSkill'e](cognitive-search-skill-language-detection.md)giriş için metni bölmeniz gerekir), İngilizce (tr) varsayılanı yeterli olmalıdır. </li></ul>  |


## <a name="skill-inputs"></a>Beceri Girişleri

| Parametre adı       | Açıklama      |
|----------------------|------------------|
| metin  | Alt dize ye bölünecek metin. |
| languageCode  | (İsteğe bağlı) Belgenin dil kodu. Dili bilmiyorsanız (yani [languageDetectionSkill'e](cognitive-search-skill-language-detection.md)giriş için metni bölmeniz gerekir), bu girişi kaldırmak güvenlidir.  |

## <a name="skill-outputs"></a>Beceri Çıktıları 

| Parametre adı     | Açıklama |
|--------------------|-------------|
| textItems | Çıkarılan bir dizi alt dize. |


##  <a name="sample-definition"></a>Örnek tanımı

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
    "textSplitMode" : "pages", 
    "maximumPageLength": 1000,
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "textItems",
            "targetName": "mypages"
        }
    ]
}
```

##  <a name="sample-input"></a>Örnek Giriş

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "This is a the loan application for Joe Romero, a Microsoft employee who was born in Chile and who then moved to Australia…",
                "languageCode": "en"
            }
        },
        {
            "recordId": "2",
            "data": {
                "text": "This is the second document, which will be broken into several pages...",
                "languageCode": "en"
            }
        }
    ]
}
```

##  <a name="sample-output"></a>Örnek Çıktı

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "textItems": [
                    "This is the loan…",
                    "On the second page we…"
                ]
            }
        },
        {
            "recordId": "2",
            "data": {
                "textItems": [
                    "This is the second document...",
                    "On the second page of the second doc…"
                ]
            }
        }
    ]
}
```

## <a name="error-cases"></a>Hata örnekleri
Bir dil desteklenmezse, bir uyarı oluşturulur ve metin karakter sınırlarında bölünür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
