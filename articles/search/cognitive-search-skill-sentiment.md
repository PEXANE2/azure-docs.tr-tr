---
title: Duygusal bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'daki bir AI zenginleştirme boru hattındaki metinden olumlu-negatif duyarlılık puanı çıkarın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: cc3aab703b9c5ffcb5f3280060417ce32fcec2fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72791906"
---
# <a name="sentiment-cognitive-skill"></a>Duygusal bilişsel beceri

**Sentiment** becerisi, yapılandırılmamış metni pozitif-negatif süreklilik boyunca değerlendirir ve her kayıt için 0 ile 1 arasında sayısal bir puan verir. 1'e yakın puanlar olumlu, 0'a yakın puanlar ise olumsuz duyguları gösterir. Bu beceri, Bilişsel Hizmetlerde [Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sağlanan makine öğrenimi modellerini kullanır.

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Veri sınırları
Bir kaydın maksimum boyutu 5000 karakter olarak [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçülmelidir. Verilerinizi duygu çözümleyicisine göndermeden önce ayırmanız gerekiyorsa, [Metin Bölme becerisini](cognitive-search-skill-textsplit.md)kullanın.


## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre Adı |                      |
|----------------|----------------------|
| defaultLanguageCode | (isteğe bağlı) Dili açıkça belirtmeyin belgelere uygulanacak dil kodu. <br/> [Desteklenen dillerin tam listesine](../cognitive-services/text-analytics/text-analytics-supported-languages.md) bakın |

## <a name="skill-inputs"></a>Beceri girdileri 

| Giriş Adı | Açıklama |
|--------------------|-------------|
| metin | İncelenecek metin.|
| languageCode  |  (İsteğe bağlı) Kayıtların dilini gösteren bir dize. Bu parametre belirtilmemişse, varsayılan değer "en"dir. <br/>[Desteklenen dillerin tam listesine](../cognitive-services/text-analytics/text-analytics-supported-languages.md)bakın.|

## <a name="skill-outputs"></a>Beceri çıktıları

| Çıktı Adı | Açıklama |
|--------------------|-------------|
| puan | Analiz edilen metnin duyarlılığını temsil eden 0 ile 1 arasındaki değer. 0'a yakın değerler olumsuz, 0,5'e yakın nötr duyguya ve 1'e yakın değerler olumlu duygulara sahiptir.|


##  <a name="sample-definition"></a>Örnek tanımı

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
Boşsa, bu kayıtlar için bir duyarlılık puanı döndürülmez.

## <a name="error-cases"></a>Hata örnekleri
Bir dil desteklenmezse, bir hata oluşturulur ve hiçbir duyarlılık puanı döndürülür.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
