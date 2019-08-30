---
title: Metin bölünmüş bilişsel arama yeteneği-Azure Search
description: Azure Search zenginleştirme işlem hattındaki uzunluğu temel alarak metin öbeklerine veya metin sayfalarına bölün.
services: search
manager: nitinme
author: luiscabrer
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.subservice: cognitive-search
ms.openlocfilehash: fcde448c664ad85b78d275304dd01a1eedaca006
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70183309"
---
#   <a name="text-split-cognitive-skill"></a>Metin bölünmüş Bilişsel Beceri

Metni **bölünmüş** metin, metni metin parçalara ayırır. Metni cümlelere veya belirli uzunluktaki sayfalara bölmek isteyip istemediğinizi belirtebilirsiniz. Bu beceri, özellikle diğer yeteneklerin akış yönündeki maksimum metin uzunluğu gereksinimi olduğunda yararlıdır. 

> [!NOTE]
> Bu yetenek bilişsel hizmetler API 'SI ile bağlantılı değildir ve bunu kullanmak için ücretlendirilirsiniz. Yine de bir bilişsel [Hizmetler kaynağı iliştirmelisiniz](cognitive-search-attach-cognitive-services.md), ancak her gün çok az sayıda günlük zenginleştirme için sizi sınırlayan **ücretsiz** Kaynak seçeneğini geçersiz kılabilirsiniz.

## <a name="odatatype"></a>@odata.type  
Microsoft. yetenekler. Text. Splitbeceri 

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| textSplitMode      | "Pages" veya "cümleler" | 
| maximumPageLength | TextSplitMode, "Pages" olarak ayarlandıysa, bu, ile `String.Length`ölçülen en fazla sayfa uzunluğuna başvurur. Minimum değer 100 ' dir.  TextSplitMode değeri "Pages" olarak ayarlandıysa, algoritma metni en çok "maximumPageLength" boyutunda olan parçalara bölmeye çalışır. Bu durumda, algoritma bir cümle sınırında tümceyi bölmek için en iyi şekilde yapılır, bu nedenle öbek boyutu "maximumPageLength" değerinden biraz daha az olabilir. | 
| defaultLanguageCode   | seçim Aşağıdaki dil kodlarından biri: `da, de, en, es, fi, fr, it, ko, pt`. Varsayılan değer Ingilizce 'dir (en). Göz önünde bulundurulması gereken birkaç nokta vardır:<ul><li>LanguageCode-CountryCode biçimi geçirirseniz, yalnızca biçimin languageCode kısmı kullanılır.</li><li>Dil önceki listede değilse, bölünmüş beceri metin karakter sınırlarındaki metni keser.</li><li>Bir dil kodu sağlamak, Çince, Japonca ve Korece gibi boşluk olmayan diller için bir sözcüğün yarısını sonlandırmamak için yararlıdır.</li></ul>  |


## <a name="skill-inputs"></a>Beceri girişleri

| Parametre adı       | Açıklama      |
|----------------------|------------------|
| text  | Alt dizeden bölünecek metin. |
| languageCode  | Seçim Belge için dil kodu.  |

## <a name="skill-outputs"></a>Yetenek çıkışları 

| Parametre adı     | Açıklama |
|--------------------|-------------|
| Textıtems | Ayıklanan alt dizeler dizisi. |


##  <a name="sample-definition"></a>Örnek tanım

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

##  <a name="sample-output"></a>Örnek çıkış

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

## <a name="error-cases"></a>Hata durumları
Bir dil desteklenmiyorsa, bir uyarı oluşturulur ve metin karakter sınırlarına bölünür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
