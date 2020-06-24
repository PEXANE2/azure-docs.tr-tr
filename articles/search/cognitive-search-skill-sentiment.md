---
title: Yaklaşım Bilişsel Beceri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama içindeki bir AI zenginleştirme ardışık düzeninde bulunan metinden pozitif negatif bir yaklaşım puanı ayıklayın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: 15118c9c3d2d1a528c62b539ab7bdd8f174c19de
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85080110"
---
# <a name="sentiment-cognitive-skill"></a>Yaklaşım Bilişsel Beceri

Yaklaşım **yeteneği,** yapılandırılmamış metinleri pozitif negatif bir Continuum boyunca değerlendirir ve her kayıt için 0 ile 1 arasında bir sayısal puan döndürür. 1 ' e yakın puan pozitif yaklaşımı gösterir ve 0 ' a yakın puan negatif yaklaşımı gösterir. Bu beceri bilişsel hizmetler 'de [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sunulan makine öğrenimi modellerini kullanır.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Bilişsel Arama belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk eder. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması, [Azure bilişsel arama fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)açıklanmaktadır.


## <a name="odatatype"></a>@odata.type  
Microsoft. yetenekler. Text. Sentimentbecerisi

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu, tarafından ölçülen 5000 karakter olmalıdır [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length) . Bu verileri yaklaşım Çözümleyicisi 'ne göndermeden önce bölmeniz gerekirse, [metin bölünmüş yeteneği](cognitive-search-skill-textsplit.md)kullanın.


## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre Adı |                      |
|----------------|----------------------|
| `defaultLanguageCode` | seçim Açıkça dil belirtmeyen belgelere uygulanacak dil kodu. <br/> [Desteklenen dillerin tam listesini](../cognitive-services/text-analytics/text-analytics-supported-languages.md) görün |

## <a name="skill-inputs"></a>Beceri girişleri 

| Giriş adı | Description |
|--------------------|-------------|
| `text` | Çözümlenecek metin.|
| `languageCode`    |  Seçim Kayıtların dilini gösteren bir dize. Bu parametre belirtilmemişse, varsayılan değer "en" olur. <br/>[Desteklenen dillerin tam listesini](../cognitive-services/text-analytics/text-analytics-supported-languages.md)görüntüleyin.|

## <a name="skill-outputs"></a>Yetenek çıkışları

| Çıkış adı | Description |
|--------------------|-------------|
| `score` | Çözümlenen metnin yaklaşımını temsil eden 0 ile 1 arasında bir değer. 0 ' a yakın değerlerde negatif yaklaşım vardır, 0,5 ' a yakın nötr yaklaşım vardır ve 1 ' e yakın değerler pozitif yaklaşım vardır.|


##  <a name="sample-definition"></a>Örnek tanım

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
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
            "name": "score",
            "targetName": "mySentiment"
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
            "data": {
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
                "languageCode": "en"
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
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Notlar
Boşsa, bu kayıtlar için bir yaklaşım puanı döndürülmez.

## <a name="error-cases"></a>Hata durumları
Bir dil desteklenmiyorsa bir hata oluşturulur ve bir yaklaşım puanı döndürülmez.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
