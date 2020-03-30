---
title: Varlık Tanıma bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da zenginleştirme ardışık bir ardışık alandaki metinden farklı türde varlıklar ayıklayın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 6ef5952b6413563b2c2e16ff2218f709b414fb84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297807"
---
#    <a name="entity-recognition-cognitive-skill"></a>Varlık Tanıma bilişsel beceri

**Varlık Tanıma** becerisi metinden farklı türde varlıkları ayıklar. Bu beceri, Bilişsel Hizmetlerde [Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sağlanan makine öğrenimi modellerini kullanır.

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.EntityTanıma Beceri

## <a name="data-limits"></a>Veri sınırları
Bir kaydın maksimum boyutu 50.000 karakter olarak [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçüldü. Verilerinizi anahtar tümcecik çıkarıcıya göndermeden önce ayırmanız gerekiyorsa, [Metin Bölme becerisini](cognitive-search-skill-textsplit.md)kullanmayı düşünün.

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harf duyarlıdır ve tüm isteğe bağlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| kategoriler    | Ayıklanması gereken kategoriler dizisi.  Olası kategori `"Person"`türleri: `"Quantity"`, `"Datetime"` `"URL"`, `"Email"` `"Location"` `"Organization"`, , , . Kategori sağlanmadıysa, tüm türler döndürülür.|
|defaultLanguageCode |    Giriş metninin dil kodu. Aşağıdaki diller desteklenir: `ar, cs, da, de, en, es, fi, fr, hu, it, ja, ko, nl, no, pl, pt-BR, pt-PT, ru, sv, tr, zh-hans`. Tüm varlık kategorileri tüm diller için desteklenmez; aşağıdaki nota bakın.|
|minimum Hassasiyet | 0 ile 1 arasında bir değer. Güven puanı `namedEntities` (çıktıdaki) bu değerden daha düşükse, varlık döndürülmez. Varsayılan değer, 0'dur. |
|includeTypelessEntities | Geçerli `true` kategorilere uymayan tanınmış varlıkları tanımak istiyorsanız ayarlayın. Tanınan varlıklar `entities` karmaşık çıktı alanında döndürülür. Örneğin, "Windows 10" iyi bilinen bir varlıktır (bir ürün), ancak "Ürünler" desteklenen bir kategori olmadığından, bu varlık varlıklar çıktı alanına dahil edilir. Varsayılan değer,`false` |


## <a name="skill-inputs"></a>Beceri girdileri

| Giriş adı      | Açıklama                   |
|---------------|-------------------------------|
| languageCode    | İsteğe bağlı. `"en"` varsayılan değerdir.  |
| metin          | Çözümlemek için metin.          |

## <a name="skill-outputs"></a>Beceri çıktıları

> [!NOTE]
> Tüm varlık kategorileri tüm diller için desteklenmez. , `"Person"` `"Location"`ve `"Organization"` varlık kategori türleri yukarıdaki dillerin tam listesi için desteklenir. Sadece _de_, _en_, _es_, _fr_, `"Datetime"`ve `"URL"` _zh-hans_ destek çıkarma `"Quantity"`, , , ve `"Email"` türleri. Daha fazla bilgi için [Metin Analizi API'si için Dil ve bölge desteğine](https://docs.microsoft.com/azure/cognitive-services/text-analytics/language-support)bakın.  

| Çıktı adı      | Açıklama                   |
|---------------|-------------------------------|
| Kişi       | Her dize bir kişinin adını temsil dizeleri bir dizi. |
| Konum  | Her dize bir konumu temsil eder dizeleri bir dizi. |
| organizations  | Her dize bir organizasyonu temsil eden dizeleri dizisi. |
| Miktar  | Her dize bir miktar temsil dizeleri bir dizi. |
| dateTimes  | Her dize bir DateTime (metinde göründüğü gibi) değerini temsil eden dizeleri dizisi. |
| Url 'leri | Her dize bir URL temsil dizeleri bir dizi |
| Emails | Her dize bir e-postayı temsil eden dizeleri bir dizi |
| adlandırılmış Varlıklar | Aşağıdaki alanları içeren karmaşık türleri dizisi: <ul><li>category</li> <li>değeri (Gerçek varlık adı)</li><li>ofset (Metinde bulunduğu yer)</li><li>güven (Yüksek değer gerçek bir varlık olmak daha fazla olduğu anlamına gelir)</li></ul> |
| Varlık | Aşağıdaki alanları içeren metinden çıkarılan varlıklar hakkında zengin bilgiler içeren karmaşık türler dizisi <ul><li> adı (gerçek varlık adı. Bu bir "normalleştirilmiş" formu temsil eder)</li><li> wikipediaId</li><li>vikipediDil</li><li>wikipediaUrl (varlık için Vikipedi sayfasına bir bağlantı)</li><li>bingId</li><li>türü (tanınan varlığın kategorisi)</li><li>subType (yalnızca belirli kategoriler için kullanılabilir, bu varlık türünün daha ayrıntılı bir görünüm verir)</li><li> eşleşmeleri (içeren karmaşık bir koleksiyon)<ul><li>metin (varlık için ham metin)</li><li>ofset (bulunduğu yer)</li><li>uzunluk (ham varlık metninin uzunluğu)</li></ul></li></ul> |

##    <a name="sample-definition"></a>Örnek tanımı

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en",
    "includeTypelessEntities": true,
    "minimumPrecision": 0.5,
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
      },
      {
        "name": "emails",
        "targetName": "contact"
      },
      {
        "name": "entities"
      }
    ]
  }
```
##    <a name="sample-input"></a>Örnek giriş

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Contoso corporation was founded by John Smith. They can be reached at contact@contoso.com",
             "languageCode": "en"
           }
      }
    ]
}
```

##    <a name="sample-output"></a>Örnek çıktı

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "persons": [ "John Smith"],
        "emails":["contact@contoso.com"],
        "namedEntities": 
        [
          {
            "category":"Person",
            "value": "John Smith",
            "offset": 35,
            "confidence": 0.98
          }
        ],
        "entities":  
        [
          {
            "name":"John Smith",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Person",
            "subType": null,
            "matches": [{
                "text": "John Smith",
                "offset": 35,
                "length": 10
            }]
          },
          {
            "name": "contact@contoso.com",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Email",
            "subType": null,
            "matches": [
            {
                "text": "contact@contoso.com",
                "offset": 70,
                "length": 19
            }]
          },
          {
            "name": "Contoso",
            "wikipediaId": "Contoso",
            "wikipediaLanguage": "en",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/Contoso",
            "bingId": "349f014e-7a37-e619-0374-787ebb288113",
            "type": null,
            "subType": null,
            "matches": [
            {
                "text": "Contoso",
                "offset": 0,
                "length": 7
            }]
          }
        ]
      }
    }
  ]
}
```

Bu becerinin çıktısındaki varlıklar için döndürülen uzaklıkların doğrudan [Text Analytics API'sinden](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)döndürüldİğİni unutmayın , yani bunları özgün dizeiçin kullanıyorsanız, doğru içeriği ayıklamak için [StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) sınıfını .NET'te kullanmanız gerekir.  [Daha fazla bilgi burada bulabilirsiniz.](https://docs.microsoft.com/azure/cognitive-services/text-analytics/concepts/text-offsets)

## <a name="error-cases"></a>Hata örnekleri
Belgenin dil kodu desteklenmezse, bir hata döndürülür ve hiçbir varlık ayıklanır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
