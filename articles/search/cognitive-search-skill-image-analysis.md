---
title: Görüntü Analizi bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'daki bir AI zenginleştirme boru hattında Görüntü Analizi bilişsel becerisini kullanarak görüntü analizi yoluyla anlamsal metin ayıklayın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 4ff6972e2f7ea219a1c8c8dbabbf9fe12a8fa59e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369468"
---
# <a name="image-analysis-cognitive-skill"></a>Görüntü Analizi bilişsel beceri

**Görüntü Analizi** becerisi, görüntü içeriğine dayalı zengin bir görsel özellik kümesini ayıklar. Örneğin, bir resimden resim yazısı oluşturabilir, etiketler oluşturabilir veya ünlüleri ve simgeleri tanımlayabilirsiniz. Bu beceri, [Bilişsel Hizmetler'de Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) tarafından sağlanan makine öğrenme modellerini kullanır. 

> [!NOTE]
> Azure Bilişsel Arama'da küçük birimler (20'nin altında işlem) ücretsiz olarak yürütülebilir, ancak daha büyük iş yükleri [faturalandırılabilir Bilişsel Hizmetler kaynağının eklenmesini](cognitive-search-attach-cognitive-services.md)gerektirir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| defaultLanguageCode   |  Geri dönecek dili gösteren bir dize. Hizmet, tanıma sonuçlarını belirli bir dilde döndürür. Bu parametre belirtilmemişse, varsayılan değer "en"dir. <br/><br/>Desteklenen diller şunlardır: <br/>*tr* - İngilizce (varsayılan) <br/> *es* - İspanyolca <br/> *ja* - Japonca <br/> *pt* - Portekizce <br/> *zh* - Basitleştirilmiş Çince|
| visualÖzellikler |  Döndürülecek görsel özellik türlerini gösteren dizeler dizisi. Geçerli görsel özellik türleri şunlardır:  <ul><li>*yetişkin* - görüntü doğada pornografik olup olmadığını algılar (çıplaklık veya bir seks eylemi tasvir), ya da kanlı (aşırı şiddet veya kan tasvir). Cinsel müstehcen içerik (aka müstehcen içerik) de algılanır.</li><li>*markalar* - yaklaşık konumu da dahil olmak üzere bir görüntü içinde çeşitli markalar algılar. *Markaların* görsel özelliği sadece İngilizce olarak mevcuttur.</li><li> *kategoriler* - Bilişsel Hizmetler [Bilgisayar Vizyonu belgelerinde](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy)tanımlanan bir taksonomi göre görüntü içeriği kategorize eder. </li><li>*açıklama* - desteklenen dillerde tam bir cümle ile görüntü içeriğini açıklar.</li><li>*yüzleri* - yüzleri varsa algılar. Varsa, koordinatlar, cinsiyet ve yaş oluşturur.</li><li> *nesneler* - yaklaşık konum da dahil olmak üzere bir görüntü içinde çeşitli nesneleri algılar. *Nesnelerin* görsel özelliği yalnızca İngilizce olarak kullanılabilir.</li><li> *etiketler* - resmi görüntü içeriğiyle ilgili kelimelerin ayrıntılı bir listesiyle etiketler.</li></ul> Görsel özelliklerin adları büyük/küçük harf duyarlıdır. *Renk* ve *görüntüYazı* görsel özelliklerinin küçümsendiğini, ancak bu işlevselliğe yine de özel bir [beceri](https://go.microsoft.com/fwlink/?linkid=2121117)yle erişilebildiğini unutmayın.|
| Şey   | Hangi etki alanına özgü ayrıntıların döndürülecek lerini belirten dizeler dizisi. Geçerli görsel özellik türleri şunlardır: <ul><li>*ünlüler* - resimde algılanırsa ünlüleri tanımlar.</li><li>*yer işaretleri* - resimde algılanırsa yer işaretlerini tanımlar. </li></ul> |

## <a name="skill-inputs"></a>Beceri girdileri

| Giriş adı      | Açıklama                                          |
|---------------|------------------------------------------------------|
| image         | Karmaşık Tip. Şu anda yalnızca Azure Blob dizinleyicisi ```imageAction``` tarafından üretilen "/document/normalized_images" ```none```alanı yla çalışır. Daha fazla bilgi için [örneğe](#sample-output) bakın.|



##  <a name="sample-skill-definition"></a>Örnek beceri tanımı

```json
        {
            "description": "Extract image analysis.",
            "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
            "context": "/document/normalized_images/*",
            "defaultLanguageCode": "en",
            "visualFeatures": [
                "tags",
                "categories",
                "description",
                "faces",
                "brands"
            ],
            "inputs": [
                {
                    "name": "image",
                    "source": "/document/normalized_images/*"
                }
            ],
            "outputs": [
                {
                    "name": "categories"
                },
                {
                    "name": "tags"
                },
                {
                    "name": "description"
                },
                {
                    "name": "faces"
                },
                {
                    "name": "brands"
                }
            ]
        }
```
### <a name="sample-index-for-only-the-categories-description-faces-and-tags-fields"></a>Örnek dizini (yalnızca kategoriler, açıklamalar, yüzler ve etiketler alanları için)
```json
{
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": true,
            "searchable": true,
            "filterable": false,
            "facetable": false,
            "sortable": true
        },
        {
            "name": "blob_uri",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "facetable": false,
            "sortable": true
        },
        {
            "name": "content",
            "type": "Edm.String",
            "sortable": false,
            "searchable": true,
            "filterable": false,
            "facetable": false
        },
        {
            "name": "categories",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "name",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "score",
                    "type": "Edm.Double",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "detail",
                    "type": "Edm.ComplexType",
                    "fields": [
                        {
                            "name": "celebrities",
                            "type": "Collection(Edm.ComplexType)",
                            "fields": [
                                {
                                    "name": "name",
                                    "type": "Edm.String",
                                    "searchable": true,
                                    "filterable": false,
                                    "facetable": false
                                },
                                {
                                    "name": "faceBoundingBox",
                                    "type": "Collection(Edm.ComplexType)",
                                    "fields": [
                                        {
                                            "name": "x",
                                            "type": "Edm.Int32",
                                            "searchable": false,
                                            "filterable": false,
                                            "facetable": false
                                        },
                                        {
                                            "name": "y",
                                            "type": "Edm.Int32",
                                            "searchable": false,
                                            "filterable": false,
                                            "facetable": false
                                        }
                                    ]
                                },
                                {
                                    "name": "confidence",
                                    "type": "Edm.Double",
                                    "searchable": false,
                                    "filterable": false,
                                    "facetable": false
                                }
                            ]
                        },
                        {
                            "name": "landmarks",
                            "type": "Collection(Edm.ComplexType)",
                            "fields": [
                                {
                                    "name": "name",
                                    "type": "Edm.String",
                                    "searchable": true,
                                    "filterable": false,
                                    "facetable": false
                                },
                                {
                                    "name": "confidence",
                                    "type": "Edm.Double",
                                    "searchable": false,
                                    "filterable": false,
                                    "facetable": false
                                }
                            ]
                        }
                    ]
                }
            ]
        },
        {
            "name": "description",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "tags",
                    "type": "Collection(Edm.String)",
                    "searchable": true,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "captions",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "text",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": false,
                            "facetable": false
                        },
                        {
                            "name": "confidence",
                            "type": "Edm.Double",
                            "searchable": false,
                            "filterable": false,
                            "facetable": false
                        }
                    ]
                }
            ]
        },
        {
            "name": "faces",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "age",
                    "type": "Edm.Int32",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "gender",
                    "type": "Edm.String",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "faceBoundingBox",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "x",
                            "type": "Edm.Int32",
                            "searchable": false,
                            "filterable": false,
                            "facetable": false
                        },
                        {
                            "name": "y",
                            "type": "Edm.Int32",
                            "searchable": false,
                            "filterable": false,
                            "facetable": false
                        }
                    ]
                }
            ]
        },
        {
            "name": "tags",
            "type": "Collection(Edm.ComplexType)",
            "fields": [
                {
                    "name": "name",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "facetable": false
                },
                {
                    "name": "confidence",
                    "type": "Edm.Double",
                    "searchable": false,
                    "filterable": false,
                    "facetable": false
                }
            ]
        }
    ]
}

```
### <a name="sample-output-field-mapping-for-the-above-index"></a>Örnek çıktı alanı eşleme (yukarıdaki dizin için)
```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/normalized_images/*/categories/*",
            "targetFieldName": "categories"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/tags/*",
            "targetFieldName": "tags"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/description",
            "targetFieldName": "description"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/faces/*",
            "targetFieldName": "faces"
        },
        {
            "sourceFieldName": "/document/normalized_images/*/brands/*/name",
            "targetFieldName": "brands"
        }
```
### <a name="variation-on-output-field-mappings-nested-properties"></a>Çıktı alanı eşlemelerinde varyasyon (iç içe geçen özellikler)

Çıkış alanı eşlemelerini, sadece yer işaretleri veya ünlüler gibi alt düzey özelliklere tanımlayabilirsiniz. Bu durumda, dizin şemanızın özellikle simgesel yapılar içerecek bir alana sahip olduğundan emin olun.

```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/normalized_images/*/categories/detail/celebrities/*",
            "targetFieldName": "celebrities"
        }
```
##  <a name="sample-input"></a>Örnek giriş

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "image": {
                    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
                    "width": 500,
                    "height": 300,
                    "originalWidth": 5000,
                    "originalHeight": 3000,
                    "rotationFromOriginal": 90,
                    "contentOffset": 500,
                    "pageNumber": 2
                }
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
        "categories": [
          {
            "name": "abstract_",
            "score": 0.00390625
          },
          {
            "name": "people_",
            "score": 0.83984375,
            "detail": {
              "celebrities": [
                {
                  "name": "Satya Nadella",
                  "faceBoundingBox": [
                        {
                            "x": 273,
                            "y": 309
                        },
                        {
                            "x": 395,
                            "y": 309
                        },
                        {
                            "x": 395,
                            "y": 431
                        },
                        {
                            "x": 273,
                            "y": 431
                        }
                    ],
                  "confidence": 0.999028444
                }
              ],
              "landmarks": [
                {
                  "name": "Forbidden City",
                  "confidence": 0.9978346
                }
              ]
            }
          }
        ],
        "adult": {
          "isAdultContent": false,
          "isRacyContent": false,
          "isGoryContent": false,
          "adultScore": 0.0934349000453949,
          "racyScore": 0.068613491952419281,
          "goreScore": 0.08928389008070282
        },
        "tags": [
          {
            "name": "person",
            "confidence": 0.98979085683822632
          },
          {
            "name": "man",
            "confidence": 0.94493889808654785
          },
          {
            "name": "outdoor",
            "confidence": 0.938492476940155
          },
          {
            "name": "window",
            "confidence": 0.89513939619064331
          }
        ],
        "description": {
          "tags": [
            "person",
            "man",
            "outdoor",
            "window",
            "glasses"
          ],
          "captions": [
            {
              "text": "Satya Nadella sitting on a bench",
              "confidence": 0.48293603002174407
            }
          ]
        },
        "requestId": "0dbec5ad-a3d3-4f7e-96b4-dfd57efe967d",
        "metadata": {
          "width": 1500,
          "height": 1000,
          "format": "Jpeg"
        },
        "faces": [
          {
            "age": 44,
            "gender": "Male",
            "faceBoundingBox": [
                {
                    "x": 1601,
                    "y": 395
                },
                {
                    "x": 1653,
                    "y": 395
                },
                {
                    "x": 1653,
                    "y": 447
                },
                {
                    "x": 1601,
                    "y": 447
                }
            ]
          }
        ],
        "objects": [
          {
            "rectangle": {
              "x": 25,
              "y": 43,
              "w": 172,
              "h": 140
            },
            "object": "person",
            "confidence": 0.931
          }
        ],
        "brands":[  
           {  
              "name":"Microsoft",
              "confidence": 0.903,
              "rectangle":{  
                 "x":20,
                 "y":97,
                 "w":62,
                 "h":52
              }
           }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Hata örnekleri
Aşağıdaki hata durumlarında, hiçbir öğe ayıklanır.

| Hata Kodu | Açıklama |
|------------|-------------|
| NotSupportedLanguage | Sağlanan dil desteklenmez. |
| GeçersizImageUrl | Resim URL'si kötü biçimlendirilmiş veya erişilemiyor.|
| GeçersizImageFormat | Giriş verileri geçerli bir görüntü değildir. |
| GeçersizImageSize | Giriş görüntüsü çok büyük. |
| NotSupportedVisualFeature  | Belirtilen özellik türü geçerli değildir. |
| NotSupportedImage | Desteklenmeyen görüntü, örneğin, çocuk pornografisi. |
| Geçersiz Ayrıntılar | Desteklenmeyen etki alanına özgü model. |

Benzer bir hata `"One or more skills are invalid. Details: Error in skill #<num>: Outputs are not supported by skill: Landmarks"`alırsanız, yolu denetleyin. Hem ünlüler hem de önemli `detail`yerler.

```json
"categories":[  
      {  
         "name":"building_",
         "score":0.97265625,
         "detail":{  
            "landmarks":[  
               {  
                  "name":"Forbidden City",
                  "confidence":0.92013400793075562
               }
            ]
```

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Dizin Oluşturucu Oluşturma (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
