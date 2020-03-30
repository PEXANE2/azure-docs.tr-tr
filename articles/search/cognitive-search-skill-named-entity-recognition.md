---
title: İsimli Varlık Tanıma bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'daki bir AI zenginleştirme ardışık noktasındaki metinden kişi, konum ve kuruluş için adlandırılmış varlıkları ayıklayın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 127155e492b556ce1ce02b67cf0b0846b99ebcd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72791945"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>İsimli Varlık Tanıma bilişsel beceri

**Adlandırılmış Varlık Tanıma** becerisi, metinden adlandırılmış varlıkları ayıklar. Kullanılabilir varlıklar türleri `person`içerir `location` `organization`ve .

> [!IMPORTANT]
> Adlandırılmış varlık tanıma becerisi artık [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md)ile değiştirilerek durdurulur. Destek 15 Şubat 2019'da durduruldu ve API 2 Mayıs 2019'da üründen kaldırıldı. Desteklenen bir beceriye geçiş yapmak için [Deprecated bilişsel arama becerileri](cognitive-search-skill-deprecated.md) önerileri izleyin.

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityTanıma Beceri

## <a name="data-limits"></a>Veri sınırları
Bir kaydın maksimum boyutu 50.000 karakter olarak [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçüldü. Verilerinizi anahtar tümcecik çıkarıcıya göndermeden önce ayırmanız gerekiyorsa, [Metin Bölme becerisini](cognitive-search-skill-textsplit.md)kullanmayı düşünün.

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| kategoriler    | Ayıklanması gereken kategoriler dizisi.  Olası kategori `"Person"`türleri: `"Organization"`, , `"Location"`. Kategori sağlanmadıysa, tüm türler döndürülür.|
|defaultLanguageCode |  Giriş metninin dil kodu. Aşağıdaki diller desteklenir:`de, en, es, fr, it`|
| minimum Hassasiyet  | 0 ile 1 arasında bir sayı. Kesinlik bu değerden daha düşükse, varlık döndürülmez. Varsayılan değer, 0'dur.|

## <a name="skill-inputs"></a>Beceri girdileri

| Giriş adı      | Açıklama                   |
|---------------|-------------------------------|
| languageCode  | İsteğe bağlı. `"en"` varsayılan değerdir.  |
| metin          | Çözümlemek için metin.          |

## <a name="skill-outputs"></a>Beceri çıktıları

| Çıktı adı     | Açıklama                   |
|---------------|-------------------------------|
| Kişi      | Her dize bir kişinin adını temsil dizeleri bir dizi. |
| Konum  | Her dize bir konumu temsil eder dizeleri bir dizi. |
| organizations  | Her dize bir organizasyonu temsil eden dizeleri dizisi. |
| Varlık | Karmaşık türleri bir dizi. Her karmaşık tür aşağıdaki alanları içerir: <ul><li>kategori`"person"`( `"organization"`, `"location"`, veya )</li> <li>değeri (gerçek varlık adı)</li><li>ofset (Metinde bulunduğu yer)</li><li>güven (0 ile 1 arasındaki değer, değerin gerçek bir varlık olduğu güvenini gösterir)</li></ul> |

##  <a name="sample-definition"></a>Örnek tanımı

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person", "Location", "Organization"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
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
             "text": "This is the loan application for Joe Romero, a Microsoft employee who was born in Chile and who then moved to Australia… Ana Smith is provided as a reference.",
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
      "data" : 
      {
        "persons": [ "Joe Romero", "Ana Smith"],
        "locations": ["Chile", "Australia"],
        "organizations":["Microsoft"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 33,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 124,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Chile",
            "offset": 88,
            "confidence": 0.99
          },
          {
            "category":"location",
            "value": "Australia",
            "offset": 112,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft",
            "offset": 54,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Hata örnekleri
Belgenin dil kodu desteklenmezse, bir hata döndürülür ve hiçbir varlık ayıklanır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Varlık Tanıma Becerisi](cognitive-search-skill-entity-recognition.md)
