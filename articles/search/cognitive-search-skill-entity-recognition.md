---
title: Varlık tanıma bilişsel arama yeteneği-Azure Search
description: Azure Search bilişsel arama ardışık düzeninde metinden farklı varlık türlerini ayıklayın.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 5ca3b953f84677c13908028af968d5a2bf28b57c
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68347754"
---
#    <a name="entity-recognition-cognitive-skill"></a>Varlık tanıma Bilişsel Beceri

**Varlık tanıma** yeteneği, metinden farklı türlerdeki varlıkları ayıklar. Bu beceri bilişsel hizmetler 'de [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) tarafından sunulan makine öğrenimi modellerini kullanır.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Search içinde belge çözme aşamasının bir parçası olarak görüntü ayıklama için tahakkuk ücretleri. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması [Azure Search fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.


## <a name="odatatype"></a>@odata.type  
Microsoft. yetenekler. Text. Entityrecognitionbeceri

## <a name="data-limits"></a>Veri sınırları
Bir kaydın en büyük boyutu, tarafından [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)ölçülen 50.000 karakter olmalıdır. Anahtar ifade ayıklayıcıya göndermeden önce verilerinizi kesmeniz gerekiyorsa, [metin bölme becerinizi](cognitive-search-skill-textsplit.md)kullanmayı göz önünde bulundurun.

## <a name="skill-parameters"></a>Yetenek parametreleri

Parametreler büyük/küçük harfe duyarlıdır ve tümü isteğe bağlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| categories    | Ayıklanmak zorunda olan kategorilerin dizisi.  Olası kategori türleri: `"Person"`, `"Location"`, `"Organization"`, `"Quantity"`, `"Datetime"` ,`"URL"`, .`"Email"` Hiçbir kategori sağlanmazsa, tüm türler döndürülür.|
|defaultLanguageCode |  Giriş metninin dil kodu. Aşağıdaki diller desteklenir:`de, en, es, fr, it`|
|minimumPrecision | Kullanılmayan. Gelecekte kullanılmak üzere ayrılmıştır. |
|ıncludetypelessentities | Metin, tanınmış bir varlık içeriyorsa, ancak desteklenen kategorilerden birine kategorilere ayrılmamış olduğunda, bu, `"entities"` karmaşık çıkış alanının bir parçası olarak döndürülür. 
Bunlar iyi bilinen, ancak geçerli olarak desteklenen "kategorilerin" bir parçası olarak sınıflandırılmayan varlıklardır. "Windows 10" örneği, iyi bilinen bir varlıktır (bir ürün), ancak "Ürünler" bugün desteklenen kategorilerde değildir. Varsayılan değer`false` |


## <a name="skill-inputs"></a>Beceri girişleri

| Giriş adı      | Açıklama                   |
|---------------|-------------------------------|
| languageCode  | İsteğe bağlı. `"en"` varsayılan değerdir.  |
| text          | Analiz edilecek metin.          |

## <a name="skill-outputs"></a>Yetenek çıkışları

> [!NOTE]
> Tüm diller için varlık kategorilerinin hepsi desteklenmez. Yalnızca _en_, _es_ ,, `"Quantity"` `"Email"` türlerinin `"Datetime"`ayıklanmasını `"URL"`destekler.

| Çıkış adı     | Açıklama                   |
|---------------|-------------------------------|
| elemanları      | Her bir dizenin bir kişinin adını temsil ettiği dizeler dizisi. |
| locations  | Her bir dizenin bir konumu temsil ettiği dizeler dizisi. |
| organizations  | Her bir dizenin bir kuruluşu temsil ettiği dizeler dizisi. |
| miktarlara  | Her bir dizenin bir miktarı temsil ettiği dizeler dizisi. |
| Tarih saat  | Her bir dizenin bir tarih/saat değerini temsil ettiği dizeler dizisi (metin içinde göründüğü gibi). |
| adresleri | Her bir dizenin bir URL 'YI temsil ettiği dizelerin dizisi |
| e-postalar | Her bir dizenin bir e-postayı temsil ettiği dizelerin dizisi |
| namedEntities | Aşağıdaki alanları içeren karmaşık türlerin dizisi: <ul><li>category</li> <li>değer (gerçek varlık adı)</li><li>fark (metinde bulunduğu konum)</li><li>güven (şimdilik kullanılmıyor. -1 değerine ayarlanır)</li></ul> |
| varlıklar | Aşağıdaki alanlarla metinden ayıklanan varlıklar hakkında zengin bilgiler içeren karmaşık türlerin dizisi <ul><li> ad (gerçek varlık adı. Bu bir "normalleştirilmiş" formu temsil eder)</li><li> wikipediaId</li><li>wikipediaLanguage</li><li>wikipediaUrl (varlık için Vikipedi sayfasına bağlantı)</li><li>Bingıd</li><li>tür (tanınan varlığın kategorisi)</li><li>Alt tür (yalnızca belirli kategoriler için kullanılabilir, bu varlık türünün daha ayrıntılı bir görünümünü sağlar)</li><li> Eşleşmeler (içeren karmaşık bir koleksiyon)<ul><li>metin (varlık için ham metin)</li><li>fark (konumun bulunduğu konum)</li><li>Uzunluk (ham varlık metninin uzunluğu)</li></ul></li></ul> |

##  <a name="sample-definition"></a>Örnek tanım

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en",
    "includeTypelessEntities": true,
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
##  <a name="sample-input"></a>Örnek giriş

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

##  <a name="sample-output"></a>Örnek çıktı

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
            "confidence": -1
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


## <a name="error-cases"></a>Hata durumları
Belge için dil kodu desteklenmiyorsa bir hata döndürülür ve hiçbir varlık ayıklanmaz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Önceden tanımlanmış yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
