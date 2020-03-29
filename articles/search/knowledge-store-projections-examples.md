---
title: Projeksiyonları bilgi deposunda tanımlama
titleSuffix: Azure Cognitive Search
description: Power BI veya Azure ML ile kullanılmak üzere zenginleştirilmiş belgelerin bilgi deposuna nasıl yansıtılanabildiği yle ilgili ortak desenlere örnekler.
manager: eladz
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/15/2020
ms.openlocfilehash: 23c370289669c2dde4f8969a2921018cd0abc08c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78943671"
---
# <a name="knowledge-store-projections-how-to-shape-and-export-enrichments"></a>Bilgi deposu projeksiyonları: Zenginleştirmelerin nasıl şekillendirilir ve dışa aktarılabilen

> [!IMPORTANT] 
> Bilgi deposu şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) önizleme özellikleri sağlar. Şu anda sınırlı portal desteği ve .NET SDK desteği yoktur.

Projeksiyonlar, bir bilgi deposunda zenginleştirilmiş belgelerin fiziksel ifadesidir. Zenginleştirilmiş belgelerinetkin kullanımı yapı gerektirir. Bu makalede, hem yapıyı hem de ilişkileri inceleyeceğiz, projeksiyon özelliklerini nasıl oluşturabileceğinizi ve oluşturduğunuz projeksiyon türleri arasında verileri nasıl ilişkilendireceğiniöğreneceksiniz. 

Bir projeksiyon oluşturmak için, özel bir nesne oluşturmak için [shaper becerisini](cognitive-search-skill-shaper.md) kullanarak verileri şekillendirmeniz veya projeksiyon tanımı içinde satır içi şekillendirme sözdizimini kullanmanız gerekir. 

Veri şekli, düğümler hiyerarşisi olarak oluşturulan projelandırmak istediğiniz tüm verileri içerir. Bu makalede, raporlama, analiz veya aşağı akış işleme için elverişli fiziksel yapılara yansıtılabilmesi için verileri şekillendirmek için çeşitli teknikler gösterilmektedir. 

Bu makalede sunulan örnekler, http istemcisinde indirip çalıştırabileceğiniz bu [REST API örneğinde](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/projections/Projections%20Docs.postman_collection.json)bulunabilir.

## <a name="introduction-to-the-examples"></a>Örneklere giriş

[Projeksiyonlara](knowledge-store-projection-overview.md)aşinaysanız, üç tür olduğunu hatırlarsınız:

+ Tablolar
+ Nesneler
+ Dosyalar

Tablo projeksiyonları Azure Tablo depolama alanında depolanır. Nesne ve dosya projeksiyonları, nesne projeksiyonlarının JSON dosyaları olarak kaydedildiği blob depolamasına yazılır ve kaynak belgeden gelen içeriğin yanı sıra herhangi bir beceri çıktısı veya zenginleştirme içerebilir. Zenginleştirme boru hattı da görüntüler gibi ikili ayıklayabilirsiniz, bu ikili dosya projeksiyonları olarak yansıtılır. İkili nesne nesne projeksiyonu olarak yansıtıldığında, yalnızca onunla ilişkili meta veriler JSON blob olarak kaydedilir. 

Veri şekillendirme ve projeksiyonlar arasındaki kesişimi anlamak için, çeşitli yapılandırmaları keşfetmek için aşağıdaki becerileri temel olarak kullanacağız. Bu beceri ham görüntü ve metin içeriğini işler. Projeksiyonlar, desteklemek istediğimiz senaryolar için belgenin içeriğinden ve becerilerin çıktılarından tanımlanır.

> [!IMPORTANT] 
> Projeksiyonlarla denemeler yaparken, maliyet denetimini sağlamak için [dizinleyici önbellek özelliğini ayarlamak](search-howto-incremental-index.md) yararlıdır. Dizin oluşturma zonu ayarlanmazsa, projeksiyonların düzenlenmesi tüm belgenin yeniden zenginleşmesine neden olur. Önbellek ayarlandığında ve yalnızca projeksiyonlar güncellendiğinde, önceden zenginleştirilmiş belgeler için becerikli yürütmeler yeni Bilişsel Hizmetler ücretlerine neden olmaz.

```json
{
    "name": "azureblob-skillset",
    "description": "Skillset created from the portal. skillsetName: azureblob-skillset; contentField: merged_content; enrichmentGranularity: document; knowledgeStoreStorageAccount: confdemo;",
    "skills": [
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "name": "#1",
            "description": null,
            "context": "/document/merged_content",
            "categories": [
                "Person",
                "Quantity",
                "Organization",
                "URL",
                "Email",
                "Location",
                "DateTime"
            ],
            "defaultLanguageCode": "en",
            "minimumPrecision": null,
            "includeTypelessEntities": null,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                },
                {
                    "name": "languageCode",
                    "source": "/document/language"
                }
            ],
            "outputs": [
                {
                    "name": "persons",
                    "targetName": "people"
                },
                {
                    "name": "organizations",
                    "targetName": "organizations"
                },
                {
                    "name": "locations",
                    "targetName": "locations"
                },
                {
                    "name": "entities",
                    "targetName": "entities"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
            "name": "#2",
            "description": null,
            "context": "/document/merged_content",
            "defaultLanguageCode": "en",
            "maxKeyPhraseCount": null,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                },
                {
                    "name": "languageCode",
                    "source": "/document/language"
                }
            ],
            "outputs": [
                {
                    "name": "keyPhrases",
                    "targetName": "keyphrases"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
            "name": "#3",
            "description": null,
            "context": "/document",
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                }
            ],
            "outputs": [
                {
                    "name": "languageCode",
                    "targetName": "language"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
            "name": "#4",
            "description": null,
            "context": "/document",
            "insertPreTag": " ",
            "insertPostTag": " ",
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/content"
                },
                {
                    "name": "itemsToInsert",
                    "source": "/document/normalized_images/*/text"
                },
                {
                    "name": "offsets",
                    "source": "/document/normalized_images/*/contentOffset"
                }
            ],
            "outputs": [
                {
                    "name": "mergedText",
                    "targetName": "merged_content"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
            "name": "#5",
            "description": null,
            "context": "/document/normalized_images/*",
            "textExtractionAlgorithm": "printed",
            "lineEnding": "Space",
            "defaultLanguageCode": "en",
            "detectOrientation": true,
            "inputs": [
                {
                    "name": "image",
                    "source": "/document/normalized_images/*"
                }
            ],
            "outputs": [
                {
                    "name": "text",
                    "targetName": "text"
                },
                {
                    "name": "layoutText",
                    "targetName": "layoutText"
                }
            ]
        }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "DemosCS",
        "key": "<COGNITIVE SERVICES KEY>"
    },
    "knowledgeStore": null
}
```

Bu skillset kullanarak, `knowledgeStore` temel olarak null ile, ilk `knowledgeStore` örnek nesne doldurur, diğer senaryolarda kullanabileceğiniz tabular veri yapıları oluşturmak projeksiyonlar ile yapılandırılmış. 

## <a name="projecting-to-tables"></a>Tablolara yansıtma

Azure Depolama'daki tablolara yansıtma, Power BI gibi araçları kullanarak raporlama ve analiz için yararlıdır. Power BI tablolardan okuyabilir ve projeksiyon sırasında oluşturulan anahtarlara dayalı ilişkileri keşfedebilir. Bir pano oluşturmaya çalışıyorsanız, ilgili verilere sahip olmak bu görevi basitleştirir. 

Belgelerden çıkarılan anahtar ifadeleri bir kelime bulutu olarak görselleştirebileceğimiz bir pano oluşturmaya çalıştığımızı varsayalım. Doğru veri yapısını oluşturmak için, belgeye özgü ayrıntılara ve anahtar tümceciklere sahip özel bir şekil oluşturmak için skillset'e bir Şekillendirici becerisi ekleyebiliriz. Özel şekil kök `pbiShape` düğümünde `document` çağrılır.

> [!NOTE] 
> Tablo projeksiyonları, Azure Depolama tarafından uygulanan depolama limitlerine tabi olan Azure Depolama tablolarıdır. Daha fazla bilgi için [tablo depolama sınırlarına](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model)bakın. Varlık boyutunun 1 MB'ı geçemeyeceğini ve tek bir özelliğin 64 KB'den büyük olamayacağını bilmek yararlıdır. Bu kısıtlamalar tabloları çok sayıda küçük varlığı depolamak için iyi bir çözüm haline getirin.

### <a name="using-a-shaper-skill-to-create-a-custom-shape"></a>Özel bir şekil oluşturmak için Şekillendirici becerisi kullanma

Tablo depolama alanına yansıtabileceğiniz özel bir şekil oluşturun. Özel bir şekil olmadan, projeksiyon yalnızca tek bir düğüme (çıktı başına bir projeksiyon) başvuruyapabilir. Özel bir şekil oluşturmak, çeşitli öğeleri tek bir tablo olarak yansıtılabilen veya tablo koleksiyonunda dilimlenerek dağıtılabilen yeni bir mantıksal bütüne toplamanızı sağlar. 

Bu örnekte, özel şekil meta verileri ve tanımlanmış varlıkları ve anahtar tümcecikleri birleştirir. Nesne çağrılır `pbiShape` ve altında `/document`ebeveynli . 

> [!IMPORTANT] 
> Şekillendirmenin bir amacı, tüm zenginleştirme düğümlerinin bilgi deposuna yansıtmak için gerekli olan iyi biçimlendirilmiş JSON ile ifade edilmesini sağlamaktır. Bu, özellikle bir zenginleştirme ağacı iyi biçimlendirilmiş JSON olmayan düğümler içerdiğinde (örneğin, bir zenginleştirme bir dize gibi ilkel bir paranteze geçtiğinde) geçerlidir.
>
> Son iki düğüme dikkat `KeyPhrases` `Entities`edin ve. Bunlar geçerli bir JSON nesnesine `sourceContext`sarılır. Bu, ilkel `keyphrases` `entities` ler üzerindeki zenginleştirmeler olarak gereklidir ve yansıtılmadan önce geçerli JSON'a dönüştürülmesi gerekir.
>


```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "name": "ShaperForTables",
    "description": null,
    "context": "/document",
    "inputs": [
        {
            "name": "metadata_storage_content_type",
            "source": "/document/metadata_storage_content_type",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "metadata_storage_name",
            "source": "/document/metadata_storage_name",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "metadata_storage_path",
            "source": "/document/metadata_storage_path",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "metadata_content_type",
            "source": "/document/metadata_content_type",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "keyPhrases",
            "source": null,
            "sourceContext": "/document/merged_content/keyphrases/*",
            "inputs": [
                {
                    "name": "KeyPhrases",
                    "source": "/document/merged_content/keyphrases/*"
                }

            ]
        },
        {
            "name": "Entities",
            "source": null,
            "sourceContext": "/document/merged_content/entities/*",
            "inputs": [
                {
                    "name": "Entities",
                    "source": "/document/merged_content/entities/*/name"
                }

            ]
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "pbiShape"
        }
    ]
}
```

Skillset için yukarıdaki Shaper beceri ekleyin. 

```json
    "name": "azureblob-skillset",
    "description": "A friendly description of the skillset goes here.",
    "skills": [
        {
            Shaper skill goes here
            }
        ],
    "cognitiveServices":  "A key goes here",
    "knowledgeStore": []
}  
```

Artık tablolara yansıtmak için gereken tüm verilere sahip olduğumuza göre, knowledgeStore nesnesini tablo tanımlarıyla güncelleyin. Bu örnekte, `tableName`"ve `source` `generatedKeyName` özellikleri" ayarlayarak tanımlanan üç tablomuz vardır.

```json
"knowledgeStore" : {
    "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
    "projections": [
        {
            "tables": [
                {
                    "tableName": "pbiDocument",
                    "generatedKeyName": "Documentid",
                    "source": "/document/pbiShape"
                },
                {
                    "tableName": "pbiKeyPhrases",
                    "generatedKeyName": "KeyPhraseid",
                    "source": "/document/pbiShape/keyPhrases/*"
                },
                {
                    "tableName": "pbiEntities",
                    "generatedKeyName": "Entityid",
                    "source": "/document/pbiShape/Entities/*"
                }
            ],
            "objects": [],
            "files": []
        }
    ]
}
```

Aşağıdaki adımları izleyerek çalışmanızı işleyebilirsiniz:

1. ```storageConnectionString``` Özelliği geçerli bir V2 genel amaçlı depolama hesabı bağlantı dizesine ayarlayın.  

1. PUT isteğini vererek skillset'i güncelleştirin.

1. Skillset'i güncelledikten sonra dizinleyiciyi çalıştırın. 

Şimdi üç tablo ile çalışan bir projeksiyon var. Bu tablolarıN Power BI'ye aktarılması, Power BI'nin ilişkileri otomatik olarak keşfetmesine neden olmalıdır.

Bir sonraki örnekte geçmeden önce, dilimleme ve ilgili verilerin mekaniğini anlamak için tablo projeksiyonunun yönlerini yeniden gözden geçirin.

### <a name="slicing"></a>Dilimleme 

Dilimleme, bütün birleştirilmiş şekli kurucu parçalara ayıran bir tekniktir. Sonuç, tek tek çalışabileceğiniz ayrı ama ilgili tablolardan oluşur.

Örnekte, `pbiShape` birleştirilmiş şekil (veya zenginleştirme düğümü) bulunur. Projeksiyon tanımında, `pbiShape` şeklin parçalarını çekmenizi sağlayan ek tablolara dilimlenir ```keyPhrases``` ve. ```Entities``` Power BI'de, birden çok varlık ve anahtar Ifadeleri her belgeyle ilişkilendirilmelerinden bu kullanışlıdır ve varlıkları ve anahtar Ifadeleri kategorilere ayrılılmış veriler olarak görebiliyorsanız daha fazla bilgi alırsınız.

Dilimleme, alt tabloda aynı ada sahip bir ```generatedKeyName``` sütun oluşturmak için üst tablodaki leri kullanarak üst ve alt tablolar arasında bir ilişki oluşturur. 

### <a name="naming-relationships"></a>İlişkileri adlandırma

Ve ```generatedKeyName``` ```referenceKeyName``` özellikleri tablolar arasında ve hatta projeksiyon türleri arasında veri ilişkilendirmek için kullanılır. Alt tablo/projeksiyondaki her satırın ebeveyni gösteren bir özelliği vardır. Alttaki sütunun veya özelliğin adı ```referenceKeyName``` üst öğedendir. ```referenceKeyName``` Sağlanmadığında, hizmet varsayılan olarak üst ```generatedKeyName``` öğeden gelene olur. 

Power BI, tablolar içindeki ilişkileri keşfetmek için oluşturulan bu anahtarlara güvenir. Alt tablodaki sütuna farklı şekilde adlandırılırsa, ```referenceKeyName``` özelliği üst tabloya ayarlayın. Bir örnek, pbiDocument tablosunda kimlik ```generatedKeyName``` olarak ve ```referenceKeyName``` DocumentID olarak ayarlamak olacaktır. Bu, pbiEntities ve pbiKeyPhrases tablolarıdocumentID adlı belge id içeren sütun neden olur.

## <a name="projecting-to-objects"></a>Nesnelere yansıtma

Nesne projeksiyonları tablo projeksiyonları ile aynı sınırlamalara sahip değildir ve büyük belgeleri yansıtmak için daha uygundur. Bu örnekte, belgenin tamamını bir nesne projeksiyonuna yansıtıyoruz. Nesne projeksiyonları bir kaptaki tek bir projeksiyonla sınırlıdır ve dilimlenemez.

Bir nesne projeksiyonu tanımlamak için, projeksiyonlar daki ```objects``` diziyi kullanacağız. Şekillendirici becerisini kullanarak yeni bir şekil oluşturabilir veya nesne projeksiyonunun satır satır lı biçimlerini kullanabilirsiniz. Tablolar örneği bir şekil oluşturma ve dilimleme yaklaşımını gösterirken, bu örnek satır içinde şekillendirme kullanımını gösterir. 

Satır altı şekillendirme, bir projeksiyona girişlerin tanımında yeni bir şekil oluşturma yeteneğidir. Satır altı şekillendirme, Shaper becerisinin üreteceğiyle aynı anonim bir nesne `pbiShape`oluşturur (bizim durumumuzda). Satır altı biçim, yeniden kullanmayı planlamadığınız bir şekil tanımlıyorsanız yararlıdır.

Projeksiyonözelliği bir dizidir. Bu örnekiçin, array'e knowledgeStore tanımının satır içinde projeksiyonlar içerdiği yeni bir projeksiyon örneği ekliyoruz. Satır satırlı projeksiyonları kullanırken, Şekillendirici becerisini atlayabilirsiniz.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
             {
                "tables": [ ],
                "objects": [
                    {
                        "storageContainer": "sampleobject",
                        "source": null,
                        "generatedKeyName": "myobject",
                        "sourceContext": "/document",
                        "inputs": [
                            {
                                "name": "metadata_storage_name",
                                "source": "/document/metadata_storage_name"
                            },
                            {
                                "name": "metadata_storage_path",
                                "source": "/document/metadata_storage_path"
                            },
                            {
                                "name": "content",
                                "source": "/document/content"
                            },
                            {
                                "name": "keyPhrases",
                                "source": "/document/merged_content/keyphrases/*"
                            },
                            {
                                "name": "entities",
                                "source": "/document/merged_content/entities/*/name"
                            },
                            {
                                "name": "ocrText",
                                "source": "/document/normalized_images/*/text"
                            },
                            {
                                "name": "ocrLayoutText",
                                "source": "/document/normalized_images/*/layoutText"
                            }
                        ]

                    }
                ],
                "files": []
            }
        ]
    }
```

## <a name="projecting-to-file"></a>Dosyaya yansıtma

Dosya projeksiyonları, kaynak belgeden çıkarılan görüntüler veya zenginleştirme işleminden yansıtılabilen zenginleştirme çıktılarıdır. Nesne projeksiyonlarına benzer dosya projeksiyonları Azure Depolama'da blob olarak uygulanır ve görüntüyü içerir. 

Bir dosya projeksiyonu oluşturmak `files` için, projeksiyon nesnesindeki diziyi kullanırız. Bu örnekte, belgeden çıkarılan tüm görüntüleri `samplefile`.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
            {
                "tables": [ ],
                "objects": [ ],
                "files": [
                    {
                        "storageContainer": "samplefile",
                        "source": "/document/normalized_images/*"
                    }
                ]
            }
        ]
    }
```

## <a name="projecting-to-multiple-types"></a>Birden çok türe yansıtma

Daha karmaşık bir senaryo, içeriği projeksiyon türlerine yansıtmanızı gerektirebilir. Örneğin, anahtar tümcecikler ve varlıklar gibi bazı verileri tablolara yansıtmanız gerekiyorsa, metin ve düzen metninin OCR sonuçlarını nesne olarak kaydedin ve ardından görüntüleri dosya olarak yansıtın. 

Bu örnekte, skillset güncelleştirmeleri aşağıdaki değişiklikleri içerir:

1. Her belge için satır içeren bir tablo oluşturun.
1. Bu tabloda satır olarak tanımlanan her anahtar tümcecik ile belge tablosu ile ilgili bir tablo oluşturun.
1. Bu tabloda bir satır olarak tanımlanan her varlık ile belge tablosu ile ilgili bir tablo oluşturun.
1. Her görüntü için düzen metniyle birlikte bir nesne projeksiyonu oluşturun.
1. Çıkarılan her görüntüyü yansıtarak bir dosya projeksiyonu oluşturun.
1. Belge tablosuna başvurular, düzen metni ve dosya projeksiyonu ile nesne projeksiyonu içeren çapraz başvuru tablosu oluşturun.

Bu değişiklikler knowledgeStore tanımına daha aşağı yansıtılır. 

### <a name="shape-data-for-cross-projection"></a>Çapraz projeksiyon için şekil verileri

Bu projeksiyonlar için ihtiyacımız olan şekilleri elde etmek için, adlı `crossProjection`şekilli bir nesne oluşturan yeni bir Şekillendirici becerisi ekleyerek başlayın. 

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "name": "ShaperForCross",
    "description": null,
    "context": "/document",
    "inputs": [
        {
            "name": "metadata_storage_name",
            "source": "/document/metadata_storage_name",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "keyPhrases",
            "source": null,
            "sourceContext": "/document/merged_content/keyphrases/*",
            "inputs": [
                {
                    "name": "KeyPhrases",
                    "source": "/document/merged_content/keyphrases/*"
                }

            ]
        },
        {
            "name": "entities",
            "source": null,
            "sourceContext": "/document/merged_content/entities/*",
            "inputs": [
                {
                    "name": "Entities",
                    "source": "/document/merged_content/entities/*/name"
                }

            ]
        },
        {
            "name": "images",
            "source": null,
            "sourceContext": "/document/normalized_images/*",
            "inputs": [
                {
                    "name": "image",
                    "source": "/document/normalized_images/*"
                },
                {
                    "name": "layoutText",
                    "source": "/document/normalized_images/*/layoutText"
                },
                {
                    "name": "ocrText",
                    "source": "/document/normalized_images/*/text"
                }
                ]
        }
 
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "crossProjection"
        }
    ]
}
```

### <a name="define-table-object-and-file-projections"></a>Tablo, nesne ve dosya projeksiyonlarını tanımlama

Birleştirilmiş çapraz Projeksiyon nesnesinden nesneyi birden çok tabloya dilimleyebilir, OCR çıktısını blob olarak yakalayabilir ve görüntüyü dosya olarak kaydedebiliriz (ayrıca Blob depolama alanında).

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
             {
                "tables": [
                    {
                        "tableName": "crossDocument",
                        "generatedKeyName": "Id",
                        "source": "/document/crossProjection"
                    },
                    {
                        "tableName": "crossEntities",
                        "generatedKeyName": "EntityId",
                        "source": "/document/crossProjection/entities/*"
                    },
                    {
                        "tableName": "crossKeyPhrases",
                        "generatedKeyName": "KeyPhraseId",
                        "source": "/document/crossProjection/keyPhrases/*"
                    },
                    {
                        "tableName": "crossReference",
                        "generatedKeyName": "CrossId",
                        "source": "/document/crossProjection/images/*"
                    }
                     
                ],
                "objects": [
                    {
                        "storageContainer": "crossobject",
                        "generatedKeyName": "crosslayout",
                        "source": null,
                        "sourceContext": "/document/crossProjection/images/*/layoutText",
                        "inputs": [
                            {
                                "name": "OcrLayoutText",
                                "source": "/document/crossProjection/images/*/layoutText"
                            }
                        ]
                    }
                ],
                "files": [
                     {
                        "storageContainer": "crossimages",
                        "generatedKeyName": "crossimages",
                        "source": "/document/crossProjection/images/*/image"
                    }
                    ]
                
            }
        ]
    }
```

Nesne projeksiyonları her projeksiyon için bir kapsayıcı adı gerektirir, nesne projeksiyonları veya dosya projeksiyonları bir kapsayıcıyı paylaşamaz. 

### <a name="relationships-among-table-object-and-file-projections"></a>Tablo, nesne ve dosya projeksiyonları arasındaki ilişkiler

Bu örnek, projeksiyonların başka bir özelliğini de vurgulamaktadır. Aynı projeksiyon nesnesi içinde birden çok projeksiyon türü tanımlayarak, farklı türlerde (tablolar, nesneler, dosyalar) ifade edilen bir ilişki vardır ve belge için bir tablo satırıyla başlayıp resimleriçin tüm OCR metnini bulmanızı sağlar nesne projeksiyon bu belge içinde. 

Verilerin birbiriyle ilişkili olmasını istemiyorsanız, farklı projeksiyon nesnelerindeki projeksiyonları tanımlayın. Örneğin, aşağıdaki parçacık tablolar ilişkili, ancak tablolar ve nesne (OCR metin) projeksiyonları arasında ilişkiler olmadan neden olur. 

Projeksiyon grupları, aynı verileri farklı ihtiyaçlar için farklı şekillerde yansıtmak istediğinizde yararlıdır. Örneğin, Power BI panosu için bir projeksiyon grubu ve özel bir beceriyle sarılmış bir makine öğrenme modelini eğitmek için kullanılan verileri yakalamak için başka bir projeksiyon grubu.

Farklı türde projeksiyonlar oluşturulurken, önce dosya ve nesne projeksiyonları oluşturulur ve yollar tablolara eklenir.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
            {
                "tables": [
                    {
                        "tableName": "unrelatedDocument",
                        "generatedKeyName": "Documentid",
                        "source": "/document/pbiShape"
                    },
                    {
                        "tableName": "unrelatedKeyPhrases",
                        "generatedKeyName": "KeyPhraseid",
                        "source": "/document/pbiShape/keyPhrases"
                    }
                ],
                "objects": [
                    
                ],
                "files": []
            }, 
            {
                "tables": [],
                "objects": [
                    {
                        "storageContainer": "unrelatedocrtext",
                        "source": null,
                        "sourceContext": "/document/normalized_images/*/text",
                        "inputs": [
                            {
                                "name": "ocrText",
                                "source": "/document/normalized_images/*/text"
                            }
                        ]
                    },
                    {
                        "storageContainer": "unrelatedocrlayout",
                        "source": null,
                        "sourceContext": "/document/normalized_images/*/layoutText",
                        "inputs": [
                            {
                                "name": "ocrLayoutText",
                                "source": "/document/normalized_images/*/layoutText"
                            }
                        ]
                    }
                ],
                "files": []
            }
        ]
    }
```

## <a name="common-issues"></a>Genel Sorunlar

Bir projeksiyon tanımlanırken, beklenmeyen sonuçlara neden olabilecek birkaç yaygın sorun vardır. Bilgi deposundaki çıktı beklediğiniz gibi değilse, bu sorunları denetleyin.

+ Dize zenginleştirmelerini geçerli JSON'a dönüştürmemek. Dizeleri zenginleştirildiğinde, örneğin `merged_content` anahtar tümceciklerle zenginleştirildiğinde, zenginleştirilmiş özellik zenginleştirme `merged_content` ağacının bir çocuğu olarak temsil edilir. Varsayılan gösterim iyi biçimlendirilmiş JSON değildir. Yani projeksiyon zamanda, bir ad ve bir değer ile geçerli bir JSON nesneiçine zenginleştirme dönüştürmek için emin olun.

+ Kaynak yolun ```/*``` sonundaki atlayış. Bir projeksiyonun kaynağı `/document/pbiShape/keyPhrases`ise, anahtar tümcecikler dizisi tek bir nesne/satır olarak yansıtılır. Bunun yerine, anahtar `/document/pbiShape/keyPhrases/*` tümceciklerin her biri için tek bir satır veya nesne vermek için kaynak yolu ayarlayın.

+ Yol sözdizimi hataları. Yol seçiciler büyük/küçük harf duyarlıdır ve seçici için tam kılıfı kullanmazsanız eksik giriş uyarılarına yol açabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki örnekler, projeksiyonların nasıl oluşturulaca ilişkin ortak desenleri göstermektedir. Artık kavramları iyi anladığınıza göre, özel senaryonuz için projeksiyonlar oluşturmak için daha iyi donanıma sahipsiniz.

Yeni özellikleri keşfederken, artımlı zenginleştirmeyi bir sonraki adım olarak düşünün. Artımlı zenginleştirme önbelleğe dayalıdır, aksi takdirde bir beceri değişikliği etkilenmez herhangi bir zenginleştirmeyeniden sağlar. Bu, özellikle OCR ve görüntü çözümlemesi içeren boru hatları için yararlıdır.

> [!div class="nextstepaction"]
> [Artımlı zenginleştirme ve önbelleğe giriş](cognitive-search-incremental-indexing-conceptual.md)

Projeksiyonlara genel bir bakış için, gruplar ve dilimleme gibi yetenekler ve [bunları bir beceride](knowledge-store-projection-overview.md) nasıl tanımladığınız hakkında daha fazla bilgi edinin

> [!div class="nextstepaction"]
> [Bilgi deposundaki projeksiyonlar](knowledge-store-projection-overview.md)

