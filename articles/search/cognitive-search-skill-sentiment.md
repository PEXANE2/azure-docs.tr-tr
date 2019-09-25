---
title: Yaklaşım bilişsel arama yeteneği-Azure Search
description: Azure Search bir zenginleştirme ardışık düzeninde metinden pozitif negatif yaklaşım puanı ayıklayın.
services: search
manager: nitinme
author: luiscabrer
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.openlocfilehash: 8610506b7d62542e9eb801b06dd49a5e130fa7fa
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265749"
---
#   <a name="sentiment-cognitive-skill"></a>Yaklaşım Bilişsel Beceri

Yaklaşım **yeteneği,** yapılandırılmamış metinleri pozitif negatif bir Continuum boyunca değerlendirir ve her kayıt için 0 ile 1 arasında bir sayısal puan döndürür. Puanın 1’e yakın olması yaklaşımın olumlu olduğunu, 0’a yakın olması ise olumsuz olduğunu gösterir. Bu beceri bilişsel hizmetler 'de [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sunulan makine öğrenimi modellerini kullanır.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Search içinde belge çözme aşamasının bir parçası olarak görüntü ayıklama için tahakkuk ücretleri. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması [Azure Search fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.


## <a name="odatatype"></a>@odata.type  
Microsoft. yetenekler. Text. Sentimentbecerisi

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu, tarafından [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçülen 5000 karakter olmalıdır. Bu verileri yaklaşım Çözümleyicisi 'ne göndermeden önce bölmeniz gerekirse, [metin bölünmüş yeteneği](cognitive-search-skill-textsplit.md)kullanın.


## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre Adı |                      |
|----------------|----------------------|
| defaultLanguageCode | seçim Açıkça dil belirtmeyen belgelere uygulanacak dil kodu. <br/> [Desteklenen dillerin tam listesini](../cognitive-services/text-analytics/text-analytics-supported-languages.md) görün |

## <a name="skill-inputs"></a>Beceri girişleri 

| Giriş adı | Açıklama |
|--------------------|-------------|
| text | Çözümlenecek metin.|
| languageCode  |  Seçim Kayıtların dilini gösteren bir dize. Bu parametre belirtilmemişse, varsayılan değer "en" olur. <br/>[Desteklenen dillerin tam listesini](../cognitive-services/text-analytics/text-analytics-supported-languages.md)görüntüleyin.|

## <a name="skill-outputs"></a>Yetenek çıkışları

| Çıkış adı | Açıklama |
|--------------------|-------------|
| puan | Çözümlenen metnin yaklaşımını temsil eden 0 ile 1 arasında bir değer. 0 ' a yakın değerlerde negatif yaklaşım vardır, 0,5 ' a yakın nötr yaklaşım vardır ve 1 ' e yakın değerler pozitif yaklaşım vardır.|


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

+ [Önceden tanımlanmış yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
