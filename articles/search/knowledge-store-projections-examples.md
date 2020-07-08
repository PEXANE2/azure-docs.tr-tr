---
title: Bir bilgi deposunda projeksiyonlarını tanımlama
titleSuffix: Azure Cognitive Search
description: Power BI veya Azure ML ile kullanmak üzere belgeleri bilgi deposuna nasıl bir şekilde zenginleştirirsiniz hakkında yaygın desenler örnekleri.
manager: nitinme
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: f030e382a5378c84df347c545e9426adee6eacb1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85566009"
---
# <a name="how-to-shape-and-export-enrichments"></a>Zenginleştirme ve dışarı aktarma

Projeksiyonlar, bilgi deposundaki belgelerin fiziksel ifadesidir. Zenginleştirilmiş belgelerin etkili kullanımı yapı gerektirir. Bu makalede, her iki yapıyı ve ilişkiyi keşfedeceğiz, projeksiyon özelliklerinin nasıl oluşturulduğunu öğrenerek, oluşturulan projeksiyon türlerinde verilerin nasıl ilişkilendirileceğiyle birlikte. 

Bir yansıtma oluşturmak için, veriler özel bir nesne oluşturmak veya bir yansıtma tanımı içindeki satır içi şekillendirme sözdizimini kullanmak için bir [beceri olarak biçimlendirilmiştir](cognitive-search-skill-shaper.md) . 

Veri şekli, proje için tasarlanan tüm verileri içerir, bu da düğümlerin hiyerarşisi olarak oluşturulur. Bu makalede, verileri şekillendirmeye yönelik çeşitli teknikler gösterilmektedir ve bu sayede raporlama, analiz veya aşağı akış işleme için fiziksel yapılar yüzeylerine yansıtılmalıdır. 

Bu makalede sunulan örnekler, bir HTTP istemcisinde indirebileceğiniz ve çalıştırabileceğiniz bu [REST API örneğinde](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/projections/Projections%20Docs.postman_collection.json)bulunabilir.

## <a name="introduction-to-projection-examples"></a>Projeksiyon örneklerine giriş

Üç tür [projeksiyonlar](knowledge-store-projection-overview.md)vardır:

+ Tablolar
+ Nesneler
+ Dosyalar

Tablo projeksiyonları Azure Tablo depolama alanında depolanır. Nesne ve dosya projeksiyonları BLOB depolama alanına yazılır; burada nesne projeksiyonları JSON dosyaları olarak kaydedilir ve kaynak belgeden ve tüm beceri çıkışları ya da zenginler ile içerik içerebilir. Zenginleştirme işlem hattı, görüntüler gibi ikilileri de ayıklayabilir, bu ikili dosyalar dosya projeksiyonları olarak yansıtılmış olabilir. Bir ikili nesne bir nesne projeksiyonu olarak yansıtıldığınızda, yalnızca onunla ilişkilendirilen meta veriler JSON blobu olarak kaydedilir. 

Veri şekillendirme ve projeksiyonlar arasındaki kesişmeyi anlamak için, çeşitli yapılandırmaların araştırmasına yönelik temel olarak aşağıdaki beceri kullanacağız. Bu beceri Ham görüntü ve metin içeriğini işler. Projeksiyonlar, istenen senaryolar için belge içerikleri ve yeteneklerin çıkışları için tanımlanır.

> [!IMPORTANT] 
> Projeksiyonlar ile denemeler yaparken, maliyet denetimini sağlamak için [Dizin Oluşturucu önbelleği özelliğini ayarlamak](search-howto-incremental-index.md) yararlı olur. Projeksiyonları düzenleme, Dizin Oluşturucu önbelleği ayarlanmamışsa tüm belgenin yeniden zenginleştirmesiyle sonuçlanır. Önbellek ayarlandığında ve yalnızca projeksiyonlar güncelleştirildikten sonra, daha önce zenginleştirilmiş belgeler için beceri yürütmeler yeni bilişsel hizmetler ücretlerine neden olmaz.

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

Bu beceri 'ı temel olarak null ile kullanarak, `knowledgeStore` ilk örneğimiz `knowledgeStore` nesneyi doldurduk ve diğer senaryolarda kullandığımız tablo veri yapılarını oluşturan projeksiyonlar ile yapılandırılır. 

## <a name="projecting-to-tables"></a>Tablolara yansıtma

Azure Storage 'da tablolara yansıtma, Power BI gibi araçları kullanarak raporlama ve analizler için yararlıdır. Power BI tablolardan okuyabilir ve projeksiyon sırasında oluşturulan anahtarlara göre ilişkileri bulabilir. Bir pano oluşturmaya çalışıyorsanız ilgili verilerin bulunması bu görevi basitleştirir. 

Belgelerden bir sözcük bulutu olarak ayıklanan anahtar tümceciklerini görselleştirmek için bir pano oluşturalım. Doğru veri yapısını oluşturmak için, belgeye özgü ayrıntıları ve anahtar tümceciklerini içeren özel bir şekil oluşturmak üzere beceri 'e bir beceri başına ekleyin. Özel şekil kök düğümünde çağrılacaktır `pbiShape` `document` .

> [!NOTE] 
> Tablo projeksiyonları, Azure depolama tarafından uygulanan depolama limitleriyle yönetilen Azure depolama tablolarıdır. Daha fazla bilgi için bkz. [Tablo depolama sınırları](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). Varlık boyutunun 1 MB 'ı aşmaması ve tek bir özelliğin 64 KB 'den büyük olmaması gerektiğini bilmek yararlı olur. Bu kısıtlamalar, tabloları çok sayıda küçük varlığı depolamak için iyi bir çözüm yapar.

### <a name="using-a-shaper-skill-to-create-a-custom-shape"></a>Özel bir şekil oluşturmak için bir beceri başına mil kullanma

Tablo depolamaya proje oluşturabileceğiniz özel bir şekil oluşturun. Özel bir şekil olmadan, projeksiyon yalnızca tek bir düğüme başvurabilir (çıktı başına bir projeksiyon). Özel bir şekil oluşturmak, çeşitli öğeleri tek bir tablo olarak yansıtılmış veya dilimlenmiş ve tablo koleksiyonu genelinde dağıtılmış yeni bir mantıksal tam olarak toplar. 

Bu örnekte, özel şekil meta verileri ve tanımlanan varlıkları ve anahtar tümceleri birleştirir. Nesne çağrılır `pbiShape` ve altında üst öğe olur `/document` . 

> [!IMPORTANT] 
> Şekillendirinin bir amacı, tüm zenginleştirme düğümlerinin, bilgi deposu 'nda yansıtma için gereken, iyi biçimlendirilmiş JSON 'da ifade olmasını sağlamaktır. Bu, bir zenginleştirme ağacı doğru biçimlendirilmiş JSON (örneğin, bir enzenginleştirme bir dize gibi bir temel öğenin üst öğesi olduğunda) içerdiğinde özellikle doğrudur.
>
> Son iki düğüme `KeyPhrases` ve `Entities` . Bunlar ile geçerli bir JSON nesnesine sarmalanır `sourceContext` . Bu gereklidir `keyphrases` ve `entities` temel elemanlar üzerinde zenginleştirilir ve yansıtılmadan önce GEÇERLI bir JSON 'a dönüştürülmesi gerekir.
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

Beceri başına yukarıdaki mil sayısını ekleyin. 

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

Tablolara proje için gereken tüm verilere sahip olduğumuz için, knowledgeStore nesnesini tablo tanımlarına güncelleştirin. Bu örnekte, ve özellikleri ayarlanarak tanımlanmış üç tablonuz vardır `tableName` `source` `generatedKeyName` .

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

1. ```storageConnectionString```Özelliği geçerli bir v2 genel amaçlı depolama hesabı bağlantı dizesi olarak ayarlayın.  

1. PUT isteğini vererek beceri güncelleştirin.

1. Beceri güncelleştirildikten sonra, Dizin oluşturucuyu çalıştırın. 

Artık üç tablo içeren bir çalışma projeksiyonu vardır. Bu tabloların Power BI içe aktarılması, ilişkilerin otomatik olarak keşfedilmesine Power BI neden olmalıdır.

Bir sonraki örneğe geçmeden önce, verileri dilimlerinin ve ilgili verilerin bulunduğu mekanizması anlamak için tablo projeksiyonunun yeniden ziyaret edelim.

### <a name="slicing"></a>Dilimleme 

Dilimleme, bir bütün birleştirilmiş şekli alt bölümlere ayıran bir tekniktir. Sonuç, tek tek ile kullanabileceğiniz ayrı ancak ilgili tablolardan oluşur.

Örnekte, `pbiShape` birleştirilmiş şekil (veya zenginleştirme düğümü). Projeksiyon tanımında, `pbiShape` şeklin parçalarını göndermenizi sağlayan ek tablolara dilimlenebilir ```keyPhrases``` ```Entities``` . Power BI, bu, birden çok varlık olarak yararlı olur ve keyPhrases her belgeyle ilişkilendirilir ve sınıflandırılan veriler olarak varlıkları ve keyPhrases 'yi görebiliyorsanız daha fazla öngörü elde edersiniz.

```generatedKeyName```Alt tabloda aynı ada sahip bir sütun oluşturmak için üst tablodaki öğesini kullanarak, Dilimleme, üst ve alt tablolar arasında örtülü olarak bir ilişki oluşturur. 

### <a name="naming-relationships"></a>Adlandırma ilişkileri

```generatedKeyName```Ve ```referenceKeyName``` Özellikleri, verileri tablolar arasında veya yansıtma türleri arasında ilişkilendirmek için kullanılır. Alt tablodaki/projeksiyondaki her satır, üst öğeye işaret eden bir özelliğe sahiptir. Alt öğe içindeki sütunun veya özelliğin adı, üst öğeden oluşur ```referenceKeyName``` . Sağlanmazsa, ```referenceKeyName``` hizmet bunu üst öğeden varsayılan olarak yapar ```generatedKeyName``` . 

Power BI tablolardaki ilişkileri saptamak için bu oluşturulan anahtarlara bağımlıdır. Alt tablodaki sütunun farklı olarak adlandırılmış olması gerekiyorsa, ```referenceKeyName``` üst tablodaki özelliğini ayarlayın. Bir örnek, ```generatedKeyName``` pbiDocument tablosundaki as ID değerini ve ```referenceKeyName``` belgeikimliği olarak olarak ayarlamak için bir örnektir. Bu, Belgetıd olarak adlandırılan belge kimliğini içeren pbiEntities ve pbiKeyPhrases tablolarındaki sütuna neden olur.

## <a name="projecting-to-objects"></a>Nesnelere yansıtma

Nesne projeksiyonları tablo projeksiyonlar ile aynı sınırlamalara sahip değildir ve büyük belgeleri yansıtmaları için daha uygundur. Bu örnekte, tüm belge bir nesne projeksiyonu olarak gönderilir. Nesne projeksiyonu, kapsayıcıda tek bir projeksiyon ile sınırlıdır ve dilimlenemez.

Bir nesne projeksiyonu tanımlamak için ```objects``` projeksiyondaki diziyi kullanın. Her yeteneği kullanarak yeni bir şekil oluşturabilir veya nesne projeksiyonu için satır içi şekillendirme kullanabilirsiniz. Tablolar örneği bir şekil ve dilimleme oluşturma yaklaşımını gösterirken, bu örnek satır içi şekillendirme kullanımını gösterir. 

Satır içi şekillendirme, projeksiyondaki girişlerin tanımında yeni bir şekil oluşturma olanağıdır. Satır içi şekillendirme, beceriye göre (bizim örneğimizde) aynı olan anonim bir nesne oluşturur `pbiShape` . Yeniden kullanmayı planlamadığınız bir şekil tanımlıyorsanız satır içi şekillendirme yararlı olur.

Projeksiyonlar özelliği bir dizidir. Bu örnek diziye yeni bir projeksiyon örneği ekler ve burada knowledgeStore tanımı satır içi projeksiyler içerir. Satır içi projeksiyonları kullanırken, her yetenek için mil 'yi atlayabilirsiniz.

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

Dosya projeksiyonları, zenginleştirme işleminden elde edilebilir kaynak belgeden veya zenginleştirme çıktılarından çıkarılan görüntülerdir. Nesne projeksiyonlarına benzer dosya projeksiyonları Azure Storage 'da blob olarak uygulanır ve görüntüyü içerir. 

Bir dosya projeksiyonu oluşturmak için, `files` İzdüşüm nesnesindeki diziyi kullanın. Bu örnek, belgeden ayıklanan tüm görüntüleri, belgesinden adlı bir kapsayıcıya projeler `samplefile` .

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

Daha karmaşık bir senaryo, içerik yansıtma türleri arasında proje gerektirebilir. Örneğin, tablo için anahtar tümceleri ve varlıklar gibi bazı verileri, metin ve düzen metninin OCR sonuçlarını nesneler olarak kaydedin ve ardından görüntüleri dosya olarak yansıdıysanız. 

Bu örnek, aşağıdaki değişikliklerle beceri öğesini güncelleştirir:

1. Her belge için bir satır içeren bir tablo oluşturun.
1. Bu tabloda satır olarak tanımlanan her anahtar tümceciği içeren belge tablosuyla ilgili bir tablo oluşturun.
1. Bu tabloda satır olarak tanımlanan her varlıkla birlikte belge tablosuyla ilgili bir tablo oluşturun.
1. Her görüntü için Düzen metniyle bir nesne projeksiyonu oluşturun.
1. Ayıklanan her görüntüyü yansıtarak bir dosya projeksiyonu oluşturun.
1. Belge tablosuna başvuruları, düzen metni ve dosya projeksiyonu içeren nesne projeksiyonu içeren bir çapraz başvuru tablosu oluşturun.

Bu değişiklikler knowledgeStore tanımına daha sonra yansıtılır. 

### <a name="shape-data-for-cross-projection"></a>Çapraz projeksiyon için veri şekil

Bu projeksiyonlar için gereken şekilleri almak için, adlı şekillendirilmiş bir nesne oluşturan yetenek başına yeni bir mil ekleyerek başlayın `crossProjection` . 

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

Birleştirilmiş çapraz projeksiyon nesnesinden, nesneyi birden çok tabloya dilimleme, OCR çıkışını Bloblar olarak yakalama ve sonra görüntüyü dosyalar (blob depolamada de) olarak kaydedin.

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

Nesne projeksiyonu her projeksiyon için bir kapsayıcı adı gerektirir, nesne projeksiyonu veya dosya projeksiyonu bir kapsayıcıyı paylaşamaz. 

### <a name="relationships-among-table-object-and-file-projections"></a>Tablo, nesne ve dosya projeksiyonları arasındaki ilişkiler

Bu örnek ayrıca projeksiyonun başka bir özelliğini de vurgular. Aynı projeksiyon nesnesi içinde birden çok tür projeksiyonu tanımlayarak, farklı türlerde ve içinde ifade edilen bir ilişki vardır (tablolar, nesneler, dosyalar). Bu, bir belge için tablo satırıyla başlayabilmeniz ve nesne projeksiyonu içindeki bu belgedeki görüntülerin tüm OCR metinlerini bulmanızı sağlar. 

Verilerin birbiriyle ilgili olmasını istemiyorsanız, projeksiyonu farklı projeksiyon nesnelerinde tanımlayın. Örneğin, aşağıdaki kod parçacığı, tablolar ile nesne (OCR metni) projeksiyonları arasında ilişki olmadan, ilişkili olan tabloların oluşmasına neden olur. 

Yansıtma grupları, farklı şekillerde aynı verileri farklı şekillerde proje yapmak istediğinizde faydalıdır. Örneğin, Power BI panosu için bir yansıtma grubu ve özel bir yeteneğe Sarmalanan makine öğrenimi modelini eğitmek için kullanılan verileri yakalamaya yönelik başka bir projeksiyon grubu.

Farklı türlerin projeksiyonlarını oluştururken, önce dosya ve nesne projeksiyonları oluşturulur ve yollar tablolara eklenir.

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

Projeksiyon tanımlarken, beklenmeyen sonuçlara neden olabilecek bazı yaygın sorunlar vardır. Bilgi deposunda çıkış beklediğiniz gibi değilse bu sorunları kontrol edin.

+ Dize enzenginleştirmelerinin geçerli bir JSON 'a şekillendirmiyor. Dizeler, `merged_content` anahtar tümcecikleriyle zenginleştirilmiş olduğunda, zenginleştirme özelliği, `merged_content` zenginleştirme ağacı içinde bir alt öğe olarak temsil edilir. Varsayılan Gösterim doğru biçimlendirilmiş JSON değil. Yansıtma zamanında, bir ad ve değer ile geçerli bir JSON nesnesine zenginleştirme dönüştürdiğinizden emin olun.

+ ```/*```Kaynak yolun sonunda yok. Projeksiyonun kaynağı ise `/document/pbiShape/keyPhrases` , anahtar tümceleri dizisi tek bir nesne/satır olarak yansıtıldır. Bunun yerine, kaynak yolunu `/document/pbiShape/keyPhrases/*` her anahtar tümceciği için tek bir satır veya nesne verecek şekilde ayarlayın.

+ Yol sözdizimi hataları. Yol seçicileri büyük/küçük harfe duyarlıdır ve seçici için tam büyük/küçük harf kullanmıyorsanız eksik giriş uyarılarına yol açabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki örneklerde, projeksiyonları oluşturma hakkında genel desenler gösterilmektedir. Kavramların iyi şekilde anlayışınız olduğuna göre, belirli senaryonuz için daha iyi bir geliştirme oluşturmaya daha iyi bir şekilde donatılmış olursunuz.

Yeni özellikleri araştırırken, bir sonraki adımınız olarak artımlı zenginleştirme yapmayı düşünün. Artımlı zenginleştirme, daha önce bir beceri değişikliğinden etkilenmeyen tüm zenginler 'i yeniden kullanmanıza imkan tanıyan önbelleğe alma işlemini temel alır. Bu, özellikle OCR ve görüntü analizini içeren işlem hatları için yararlıdır.

> [!div class="nextstepaction"]
> [Artımlı zenginleştirme ve önbelleğe alma bilgilerine giriş](cognitive-search-incremental-indexing-conceptual.md)

Projeksiyonlar hakkında genel bakış için, gruplar ve dilimleme gibi yetenekler hakkında daha fazla bilgi edinin ve [bunları beceri içinde nasıl tanımlacağınızı](knowledge-store-projection-overview.md) öğrenin

> [!div class="nextstepaction"]
> [Bilgi deposundaki projeksiyonler](knowledge-store-projection-overview.md)

