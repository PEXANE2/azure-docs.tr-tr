---
title: Bir bilgi deposunda projeksiyonlarını tanımlama
titleSuffix: Azure Cognitive Search
description: Power BI veya Azure ML ile kullanmak üzere belgeleri bilgi deposuna nasıl bir şekilde zenginleştirirsiniz hakkında yaygın desenler örnekleri.
manager: eladz
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: f29f4b91b85c0027df4be2fd5f26ef8f9749fe33
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77165520"
---
# <a name="knowledge-store-projections-how-to-shape-and-export-enrichments-to-the-knowledge-store"></a>Bilgi deposu projeksiyonları: bilgi deposuna zenginleştirme ve dışarı aktarma

> [!IMPORTANT] 
> Bilgi deposu Şu anda genel önizleme aşamasındadır. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) , Önizleme özellikleri sağlar. Şu anda sınırlı sayıda portal desteği var ve .NET SDK desteği yok.

Projeksiyonlar, bilgi deposundaki belgelerin fiziksel ifadesidir. Zenginleştirilmiş belgelerinizin etkili kullanımı yapı gerektirir. Bu makalede her iki yapıyı ve ilişkiyi keşfedeceğiz, projeksiyon özelliklerinin nasıl oluşturulduğunu öğrenerek, oluşturduğunuz yansıtma türleri arasında verilerin nasıl ilişkilendirileceğiyle karşılaşırsınız. 

Bir yansıtma oluşturmak için, özel bir nesne oluşturmak veya satır içi şekillendirme sözdizimini kullanmak için, her bir beceriye kullanarak verileri şekillendirmelidir. Veri şekli, projeyi planladığınız tüm verileri içerir. Bu belge, her bir seçeneğe bir örnek verir, oluşturduğunuz projeksiyonlar için seçeneklerden birini kullanmayı tercih edebilirsiniz.


Üç tür projeksiyonlar vardır:
+ Tablolar
+ Nesneler
+ Dosyalar

Tablo projeksiyonları Azure Tablo depolama alanında depolanır. Nesne ve dosya projeksiyonları blob depolamaya yazılır, nesne projeksiyonları JSON dosyaları olarak kaydedilir ve belge ve tüm yetenek çıkışları ya da zenginler arasında içerik içerebilir. Zenginleştirme işlem hattı, görüntüler gibi ikilileri de ayıklayabilir, bu ikili dosyalar dosya projeksiyonları olarak yansıtılmış olabilir. Bir ikili nesne bir nesne projeksiyonu olarak yansıtıldığınızda, yalnızca onunla ilişkilendirilen meta veriler JSON blobu olarak kaydedilir. 

Veri şekillendirme ve projeksiyonlar arasındaki kesişmeyi anlamak için, çeşitli yapılandırmaların araştırmasına yönelik temel olarak aşağıdaki beceri kullanacağız. Bu beceri Ham görüntü ve metin içeriğini işler. Projeksiyonlar, desteklemek istediğimiz senaryolar için belge içerikleri ve yeteneklerin çıkışları için tanımlanır.

Alternatif olarak, bu yönergedeki tüm çağrılarla bir [REST API örneğini](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/projections/Projections%20Docs.postman_collection.json) indirebilir ve kullanabilirsiniz.

> [!IMPORTANT] 
> Projeksiyonlar ile denemeler yaparken, maliyet denetimini sağlamak için [Dizin Oluşturucu önbelleği özelliğini ayarlamak](search-howto-incremental-index.md) yararlı olur. Projeksiyonları düzenleme, Dizin Oluşturucu önbelleği ayarlanmamışsa tüm belgenin yeniden zenginleştirmesiyle sonuçlanır. Önbellek ayarlandığında ve yalnızca projeksiyonlar güncelleştirildikten sonra, daha önce zenginleştirilmiş belgeler için beceri yürütmeler, bilişsel hizmetler ücretlerine neden olmaz.


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

Artık `knowledgeStore` nesnesini ekleyebilir ve her bir senaryo için tahminleri gerektiği gibi yapılandırabiliriz. 

## <a name="projecting-to-tables-for-scenarios-like-power-bi"></a>Power BI gibi senaryolar için tablolara yansıtma

> [!NOTE] 
> Bilgi deposu bir Azure depolama hesabı olduğundan, tablo projeksiyonları Azure depolama tabloları ve tablolardaki depolama limitleriyle yönetilir. daha fazla bilgi için bkz. [Tablo depolama sınırları](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). Varlık boyutunun 1 MB 'ı aşmaması ve tek bir özelliğin 64 KB 'den büyük olmaması gerektiğini bilmek yararlı olur. Bu kısıtlamalar, tabloları çok sayıda küçük varlığı depolamak için iyi bir çözüm yapar.

Power BI tablolardan okuyabilir ve bilgi deposu projeksiyonun oluşturabileceği anahtarlara göre ilişkileri bulabilir, bu, zenginleştirilmiş verilerde bir pano oluşturmaya çalışırken tabloları proje verileri için iyi bir seçenek yapar. Belgelerden bir sözcük bulutu olarak ayıklanan anahtar tümceleri görselleştirdiğimiz bir pano oluşturmaya çalıştığımız varsayılarak, belgeye özgü ayrıntılar ve anahtar tümceciklerini içeren özel bir şekil oluşturmak için beceri 'e bir beceri eklemek için. ```document``````pbiShape``` adlı yeni bir zenginleştirme oluşturmak için, her bir beceriye beceri ekleyin.

### <a name="using-a-shaper-skill-to-create-a-custom-shape"></a>Özel bir şekil oluşturmak için bir beceri başına mil kullanma

Tablo depolamaya proje oluşturabileceğiniz özel bir şekil oluşturun. Özel bir şekil olmadan, projeksiyon yalnızca tek bir düğüme başvurabilir (çıktı başına bir projeksiyon). Özel bir şekil oluşturmak, çeşitli öğeleri tek bir tablo olarak yansıtılbilen veya dilimlenmiş ve bir tablo koleksiyonuna dağılmış yeni bir mantıksal bir tamamına toplamanızı sağlar. Bu örnekte, özel şekil meta verileri ve tanımlanan varlıkları ve anahtar tümceleri birleştirir. Nesnesi pbiShape olarak adlandırılır ve `/document`altında ana öğesi. 

> [!IMPORTANT] 
> Enzenginler için kaynak yollarının, yansıtılmadan önce düzgün biçimlendirilmiş JSON nesneleri olması gerekir. Zenginleştirme ağacı, doğru biçimlendirilmiş JSON olarak temsil edebilir, örneğin bir enzenginleştirme bir bir primitte ve bir dize gibi bir üst öğe olduğunda. `KeyPhrases` ve `Entities` `sourceContext`ile geçerli bir JSON nesnesine nasıl sarmalandığına, bu, `keyphrases` olarak gereklidir ve `entities`, temel elemanlar üzerinde zenginleştirilir ve yansıtılmadan önce geçerli JSON 'a dönüştürülmesi gerekir.

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
Beceri ' de beceri listesi ' ne tanımlamış olduğumuz her yeteneği ekleyin. 

Tablolara proje için gereken tüm verilere sahip olduğumuz için, knowledgeStore nesnesini tablo tanımlarına güncelleştirin. 

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

```storageConnectionString``` özelliğini geçerli bir v2 genel amaçlı depolama hesabı bağlantı dizesi olarak ayarlayın. Bu senaryoda, ```tableName```, ```source``` ve ```generatedKeyName``` özelliklerini ayarlayarak İzdüşüm nesnesinde üç tablo tanımlayacağız. Artık PUT isteğini yayımlayarak beceri 'i güncelleştirebilirsiniz.

### <a name="slicing"></a>Dilimleme 

Bir birleştirilmiş şekil ile başlayarak, yansıtılması gereken tüm içeriklerin tek bir şekil (veya zenginleştirme düğümü) olması halinde, Dilimleme tek bir düğümü birden çok tabloya veya nesneye dilimleyebilme olanağı sağlar. Burada ```pbiShape``` nesnesi birden çok tabloya dilimlenebilir. Dilimleme özelliği, bir şeklin parçalarını, ```keyPhrases``` ve ```Entities``` ayrı tablolara çekmenize olanak sağlar. Bu, birden çok varlık olarak yararlı olur ve keyPhrases her belge ile ilişkilendirilir. Dilimleme implicity, üst ve alt tablolar arasında bir ilişki oluşturur ve üst tablodaki ```generatedKeyName``` kullanarak alt tabloda aynı ada sahip bir sütun oluşturur. 

### <a name="naming-relationships"></a>Adlandırma ilişkileri
```generatedKeyName``` ve ```referenceKeyName``` özellikleri, verileri tablolar arasında veya yansıtma türleri arasında ilişkilendirmek için kullanılır. Alt tablodaki/projeksiyondaki her satır, üst öğeye işaret eden bir özelliğe sahiptir. Alt öğe içindeki sütunun veya özelliğin adı, üst öğeden ```referenceKeyName```. ```referenceKeyName``` sağlanmazsa, hizmet bunu üst öğeden ```generatedKeyName``` varsayılan olarak alır. PowerBI, tablolardaki ilişkileri saptamak için bu oluşturulan anahtarları kullanır. Alt tabloda farklı adlı bir sütuna ihtiyacınız varsa üst tablodaki ```referenceKeyName``` özelliğini ayarlayın. Bir örnek, pbiDocument tablosundaki KIMLIĞI as ```generatedKeyName``` ve ```referenceKeyName``` Belgetıd olarak ayarlamak olacaktır. Bu, Belgetıd olarak adlandırılan belge kimliğini içeren pbiEntities ve pbiKeyPhrases tablolarındaki sütuna neden olur.

Güncelleştirilmiş beceri kaydedin ve Dizin oluşturucuyu çalıştırın, artık üç tablo içeren bir çalışma projeksiyonunuz vardır. Bu tabloların Power BI içe aktarılması, ilişkilerin otomatik olarak keşfedilmesine Power BI neden olmalıdır.

## <a name="projecting-to-objects"></a>Nesnelere yansıtma

Nesne projeksiyonları tablo projeksiyonlarıyla aynı sınırlamalara sahip değildir, büyük belgeleri yansıtmalarında daha uygundur. Bu örnekte, tüm belgeyi bir nesne projeksiyonu ile projeceğiz. Nesne projeksiyonu, kapsayıcıda tek bir projeksiyon ile sınırlıdır.
Bir nesne projeksiyonu tanımlamak için projeksiyonlar içinde ```objects``` diziyi kullanacağız. Her yeteneği kullanarak yeni bir şekil oluşturabilir veya nesne projeksiyonu için satır içi şekillendirme kullanabilirsiniz. Tablolar örneği bir şekil ve dilimleme oluşturma yaklaşımını gösterirken, bu örnek satır içi şekillendirme kullanımını gösterir. Satır içi şekillendirme, bir projeksiyondaki girişlerin tanımında yeni bir şekil oluşturmanıza olanak sağlar. Satır içi şekillendirme, benzer bir biçimlendiriciyle aynı şekilde bir anonim nesne oluşturur. Yeniden kullanmayı planlamadığınız bir şekil tanımlıyorsanız satır içi şekillendirme yararlı olur.
Tahminler özelliği bir dizidir, bu örnek için diziye yeni bir yansıtma örneği ekliyoruz. KnowledgeStore tanımını satır içi tanımlanan projeksiyonlar ile güncelleştirin, satır içi projeksiyonları kullanırken beceri başına bir mil gerekmez.

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
## <a name="file-projections"></a>Dosya projeksiyonlarını

Dosya projeksiyonları, zenginleştirme işleminden yansıtılabilen kaynak belgeden veya kendi zenginlerinin çıktılarından çıkarılan görüntülerdir. Nesne projeksiyonlarına benzer dosya projeksiyonları blob olarak uygulanır ve görüntüyü içerir. Bir dosya projeksiyonu oluşturmak için, İzdüşüm nesnesinde ```files``` dizisini kullanıyoruz. Bu örnek, belgeden ayıklanan tüm görüntüleri, `samplefile`adlı bir kapsayıcıya projeler.

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

Daha karmaşık bir senaryo, içerik yansıtma türleri arasında proje gerektirebilir. Örneğin, tablo için anahtar tümceleri ve varlıklar gibi bazı verileri bir proje yapmanız gerekiyorsa metin ve düzen metninin OCR sonuçlarını nesneler olarak kaydedin ve görüntüleri dosya olarak yansımış olursunuz. Bu beceri güncelleştirmesi şu şekilde olur:

1. Her belge için bir satır içeren bir tablo oluşturun.
2. Bu tabloda satır olarak tanımlanan her anahtar tümceciği içeren belge tablosuyla ilgili bir tablo oluşturun.
3. Bu tabloda satır olarak tanımlanan her varlıkla birlikte belge tablosuyla ilgili bir tablo oluşturun.
4. Her görüntü için Düzen metniyle bir nesne projeksiyonu oluşturun.
5. Ayıklanan her görüntüyü yansıtarak bir dosya projeksiyonu oluşturun.
6. Belge tablosuna başvuruları, düzen metni ve dosya projeksiyonu içeren nesne projeksiyonu içeren bir çapraz başvuru tablosu oluşturun.

Şekillendirilmiş bir nesne oluşturan yetenek dizisine bir yetenek başına yeni bir mil ekleyerek başlayın. 

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

### <a name="relationships"></a>İlişkiler

Bu örnek ayrıca aynı projeksiyon nesnesi içinde birden çok tür projeksiyonu tanımlayarak farklı projeksiyonun bir özelliğini vurgular, projeksiyonun farklı türlerinde (tablolar, nesneler, dosyalar) ifade edilen bir ilişki vardır ve bu da buna izin verir bir belge için bir tablo satırıyla başlayıp nesne projeksiyonu içindeki bu belgedeki görüntülerin tüm OCR metinlerini bulabilirsiniz. Verilerin birbiriyle ilgili olmasını istemiyorsanız, farklı projeksiyon nesnelerinde projeksiyonları tanımlayın, örneğin, aşağıdaki kod parçacığı ilgili tabloların ilişkili olduğu, ancak tablolar ve OCR metin projeksiyonları arasında ilişki olmaması halinde olur. Yansıtma grupları, farklı şekillerde aynı verileri farklı şekillerde proje yapmak istediğinizde faydalıdır. Örneğin, Power BI panosu için bir yansıtma grubu ve bir yetenek için bir AI modelini eğitmek üzere verileri kullanmaya yönelik başka bir projeksiyon grubu.
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

Bu örneklerde, projeksiyonların nasıl kullanılacağına ilişkin yaygın desenler gösterilmektedir. Ayrıca, özel senaryonuz için bir yansıtma oluşturma kavramlarını iyi anlamak da gerekir.

## <a name="common-issues"></a>Genel Sorunlar

Projeksiyon tanımlarken, beklenmeyen sonuçlara neden olabilecek bazı yaygın sorunlar vardır.

1. Dize enzenginleştirmelerinin şekillendirmiyor. Dizeler uyumlu olduğunda (örneğin, anahtar tümcecikleriyle ```merged_content``` zenginleştirildikten sonra, zenginleştirme özelliği, zenginleştirme ağacı içinde merged_content alt öğesi olarak temsil edilir. Ancak, bu süre içinde, bu, bir adı ve değeri olan geçerli bir JSON nesnesine dönüştürülmesi gerekir.
2. Kaynak yolunun sonundaki ```/*``` dışarıda. Örneğin, bir projeksiyonun kaynağı ```/document/pbiShape/keyPhrases```, anahtar tümceleri dizisi tek bir nesne/satır olarak yansıtıldır. Kaynak yolunun ```/document/pbiShape/keyPhrases/*``` ayarlanması, her anahtar tümceciği için tek bir satır veya nesne verir.
3. Yol seçicileri büyük/küçük harfe duyarlıdır ve seçici için tam büyük/küçük harf kullanmıyorsanız eksik giriş uyarılarına yol açabilir.

