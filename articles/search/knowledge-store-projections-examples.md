---
title: Bir bilgi deposunda projeksiyonlarını tanımlama
titleSuffix: Azure Cognitive Search
description: Power BI veya Azure ML ile kullanmak üzere belgeleri bilgi deposuna nasıl bir şekilde zenginleştirirsiniz hakkında yaygın desenler örnekleri.
manager: eladz
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/15/2020
ms.openlocfilehash: 23c370289669c2dde4f8969a2921018cd0abc08c
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78943671"
---
# <a name="knowledge-store-projections-how-to-shape-and-export-enrichments"></a>Bilgi deposu projeksiyonları: zenginleştirme ve dışarı aktarma

> [!IMPORTANT] 
> Bilgi deposu Şu anda genel önizleme aşamasındadır. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) , Önizleme özellikleri sağlar. Şu anda sınırlı sayıda portal desteği var ve .NET SDK desteği yok.

Projeksiyonlar, bilgi deposundaki belgelerin fiziksel ifadesidir. Zenginleştirilmiş belgelerinizin etkili kullanımı yapı gerektirir. Bu makalede her iki yapıyı ve ilişkiyi keşfedeceğiz, projeksiyon özelliklerinin nasıl oluşturulduğunu öğrenerek, oluşturduğunuz yansıtma türleri arasında verilerin nasıl ilişkilendirileceğiyle karşılaşırsınız. 

Bir yansıtma oluşturmak için, özel bir nesne oluşturmak veya bir yansıtma tanımında satır içi şekillendirme sözdizimini kullanmak için, her bir [beceriye](cognitive-search-skill-shaper.md) kullanarak verileri şekillendirmelidir. 

Bir veri şekli, proje için istediğiniz tüm verileri içerir, bu da düğümlerin hiyerarşisi olarak oluşturulur. Bu makalede, verileri şekillendirmeye, çözümlemeye veya aşağı akış işlemeye yönelik fiziksel yapılara yansıtılabilmesi için çeşitli teknikler gösterilmektedir. 

Bu makalede sunulan örnekler, bir HTTP istemcisinde indirebileceğiniz ve çalıştırabileceğiniz bu [REST API örneğinde](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/projections/Projections%20Docs.postman_collection.json)bulunabilir.

## <a name="introduction-to-the-examples"></a>Örneklere giriş

[Projeksiyonlar](knowledge-store-projection-overview.md)hakkında bilginiz varsa, üç tür olduğunu hatırlayacaksınız:

+ Tablolar
+ Nesneler
+ Dosyalar

Tablo projeksiyonları Azure Tablo depolama alanında depolanır. Nesne ve dosya projeksiyonları BLOB depolama alanına yazılır; burada nesne projeksiyonları JSON dosyaları olarak kaydedilir ve kaynak belgeden ve tüm beceri çıkışları ya da zenginler ile içerik içerebilir. Zenginleştirme işlem hattı, görüntüler gibi ikilileri de ayıklayabilir, bu ikili dosyalar dosya projeksiyonları olarak yansıtılmış olabilir. Bir ikili nesne bir nesne projeksiyonu olarak yansıtıldığınızda, yalnızca onunla ilişkilendirilen meta veriler JSON blobu olarak kaydedilir. 

Veri şekillendirme ve projeksiyonlar arasındaki kesişmeyi anlamak için, çeşitli yapılandırmaların araştırmasına yönelik temel olarak aşağıdaki beceri kullanacağız. Bu beceri Ham görüntü ve metin içeriğini işler. Projeksiyonlar, desteklemek istediğimiz senaryolar için belge içerikleri ve yeteneklerin çıkışları için tanımlanır.

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

Bu beceri kullanarak, null `knowledgeStore` temel olarak, ilk örneğimiz, diğer senaryolarda kullandığımız tablo veri yapılarını oluşturan yansıtmalarda yapılandırılmış `knowledgeStore` nesnesini doldurduk. 

## <a name="projecting-to-tables"></a>Tablolara yansıtma

Azure Storage 'da tablolara yansıtma, Power BI gibi araçları kullanarak raporlama ve analizler için yararlıdır. Power BI tablolardan okuyabilir ve projeksiyon sırasında oluşturulan anahtarlara göre ilişkileri bulabilir. Bir pano oluşturmaya çalışıyorsanız ilgili verilerin bulunması bu görevi basitleştirir. 

Belgelerden bir sözcük bulutu olarak ayıklanan anahtar tümceleri görselleştirebilmemiz için bir pano oluşturmayı denediğinizi varsayalım. Doğru veri yapısını oluşturmak için, belgeye özgü ayrıntıları ve anahtar tümceciklerini içeren özel bir şekil oluşturmak üzere beceri 'e bir beceri için bir mil ekleyebiliriz. Özel şekil, `document` kök düğümünde `pbiShape` çağrılır.

> [!NOTE] 
> Tablo projeksiyonları, Azure depolama tarafından uygulanan depolama limitleriyle yönetilen Azure depolama tablolarıdır. Daha fazla bilgi için bkz. [Tablo depolama sınırları](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). Varlık boyutunun 1 MB 'ı aşmaması ve tek bir özelliğin 64 KB 'den büyük olmaması gerektiğini bilmek yararlı olur. Bu kısıtlamalar, tabloları çok sayıda küçük varlığı depolamak için iyi bir çözüm yapar.

### <a name="using-a-shaper-skill-to-create-a-custom-shape"></a>Özel bir şekil oluşturmak için bir beceri başına mil kullanma

Tablo depolamaya proje oluşturabileceğiniz özel bir şekil oluşturun. Özel bir şekil olmadan, projeksiyon yalnızca tek bir düğüme başvurabilir (çıktı başına bir projeksiyon). Özel bir şekil oluşturmak, çeşitli öğeleri tek bir tablo olarak yansıtılbilen veya dilimlenmiş ve bir tablo koleksiyonuna dağılmış yeni bir mantıksal bir tamamına toplamanızı sağlar. 

Bu örnekte, özel şekil meta verileri ve tanımlanan varlıkları ve anahtar tümceleri birleştirir. Nesne `pbiShape` olarak adlandırılır ve `/document`alt öğesi olarak kullanılır. 

> [!IMPORTANT] 
> Şekillendirinin bir amacı, tüm zenginleştirme düğümlerinin, bilgi deposu 'nda yansıtma için gereken, iyi biçimlendirilmiş JSON 'da ifade olmasını sağlamaktır. Bu, bir zenginleştirme ağacı doğru biçimlendirilmiş JSON (örneğin, bir enzenginleştirme bir dize gibi bir temel öğenin üst öğesi olduğunda) içerdiğinde özellikle doğrudur.
>
> Son iki düğüm, `KeyPhrases` ve `Entities`dikkat edin. Bunlar, `sourceContext`geçerli bir JSON nesnesine sarmalanır. `keyphrases` olarak bu gereklidir ve `entities` temel elemanlar üzerinde zenginleştirilir ve yansıtılmadan önce geçerli JSON 'a dönüştürülmesi gerekir.
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

Tablolara proje için gereken tüm verilere sahip olduğumuz için, knowledgeStore nesnesini tablo tanımlarına güncelleştirin. Bu örnekte, `tableName`, `source` ve `generatedKeyName` özellikleri ayarlanarak tanımlanmış üç tablonuz vardır.

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

1. ```storageConnectionString``` özelliğini geçerli bir v2 genel amaçlı depolama hesabı bağlantı dizesi olarak ayarlayın.  

1. PUT isteğini vererek beceri güncelleştirin.

1. Beceri güncelleştirildikten sonra, Dizin oluşturucuyu çalıştırın. 

Artık üç tablo içeren bir çalışma projeksiyonu vardır. Bu tabloların Power BI içe aktarılması, ilişkilerin otomatik olarak keşfedilmesine Power BI neden olmalıdır.

Sonraki örneğe geçmeden önce, tablo projeksiyonunun yeniden ziyaret etmenizi sağlar ve verilerin dilimlerinin ve ilgili olduğu unsurlarını anlayın.

### <a name="slicing"></a>Dilimleme 

Dilimleme, bir bütün birleştirilmiş şekli alt bölümlere ayıran bir tekniktir. Sonuç, tek tek ile kullanabileceğiniz ayrı ancak ilgili tablolardan oluşur.

Örnekte, `pbiShape` birleştirilmiş şekil (veya zenginleştirme düğümü). Projeksiyon tanımında `pbiShape`, şeklin parçalarını ```keyPhrases``` ve ```Entities```göndermenizi sağlayan ek tablolara dilimlenebilir. Power BI, bu, birden çok varlık olarak yararlı olur ve keyPhrases her belgeyle ilişkilendirilir ve sınıflandırılan veriler olarak varlıkları ve keyPhrases 'yi görebiliyorsanız daha fazla öngörü elde edersiniz.

Alt tabloda aynı ada sahip bir sütun oluşturmak için üst tablodaki ```generatedKeyName``` kullanarak, Dilimleme, üst ve alt tablolar arasında örtük olarak bir ilişki oluşturur. 

### <a name="naming-relationships"></a>Adlandırma ilişkileri

```generatedKeyName``` ve ```referenceKeyName``` özellikleri, verileri tablolar arasında veya yansıtma türleri arasında ilişkilendirmek için kullanılır. Alt tablodaki/projeksiyondaki her satır, üst öğeye işaret eden bir özelliğe sahiptir. Alt öğe içindeki sütunun veya özelliğin adı, üst öğeden ```referenceKeyName```. ```referenceKeyName``` sağlanmazsa, hizmet bunu üst öğeden ```generatedKeyName``` varsayılan olarak alır. 

Power BI tablolardaki ilişkileri saptamak için bu oluşturulan anahtarlara bağımlıdır. Alt tabloda farklı adlı bir sütuna ihtiyacınız varsa üst tablodaki ```referenceKeyName``` özelliğini ayarlayın. Bir örnek, pbiDocument tablosundaki KIMLIĞI as ```generatedKeyName``` ve ```referenceKeyName``` Belgetıd olarak ayarlamak olacaktır. Bu, Belgetıd olarak adlandırılan belge kimliğini içeren pbiEntities ve pbiKeyPhrases tablolarındaki sütuna neden olur.

## <a name="projecting-to-objects"></a>Nesnelere yansıtma

Nesne projeksiyonları tablo projeksiyonlar ile aynı sınırlamalara sahip değildir ve büyük belgeleri yansıtmaları için daha uygundur. Bu örnekte, tüm belgeyi bir nesne projeksiyonu ile projeceğiz. Nesne projeksiyonu, kapsayıcıda tek bir projeksiyon ile sınırlıdır ve dilimlenemez.

Bir nesne projeksiyonu tanımlamak için projeksiyonlar içinde ```objects``` diziyi kullanacağız. Her yeteneği kullanarak yeni bir şekil oluşturabilir veya nesne projeksiyonu için satır içi şekillendirme kullanabilirsiniz. Tablolar örneği bir şekil ve dilimleme oluşturma yaklaşımını gösterirken, bu örnek satır içi şekillendirme kullanımını gösterir. 

Satır içi şekillendirme, projeksiyondaki girişlerin tanımında yeni bir şekil oluşturma olanağıdır. Satır içi şekillendirme, beceriye göre (bizim örneğimizde `pbiShape`) aynı olan anonim bir nesne oluşturur. Yeniden kullanmayı planlamadığınız bir şekil tanımlıyorsanız satır içi şekillendirme yararlı olur.

Projeksiyonlar özelliği bir dizidir. Bu örnekte, diziye yeni bir projeksiyon örneği ekliyoruz, burada knowledgeStore tanımı satır içi projeksiyler içerir. Satır içi projeksiyonları kullanırken, her yetenek için mil 'yi atlayabilirsiniz.

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

Bir dosya projeksiyonu oluşturmak için, İzdüşüm nesnesinde `files` dizisini kullanıyoruz. Bu örnek, belgeden ayıklanan tüm görüntüleri, `samplefile`adlı bir kapsayıcıya projeler.

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

Bu örnekte, Beceri güncelleştirmeleri aşağıdaki değişiklikleri içerir:

1. Her belge için bir satır içeren bir tablo oluşturun.
1. Bu tabloda satır olarak tanımlanan her anahtar tümceciği içeren belge tablosuyla ilgili bir tablo oluşturun.
1. Bu tabloda satır olarak tanımlanan her varlıkla birlikte belge tablosuyla ilgili bir tablo oluşturun.
1. Her görüntü için Düzen metniyle bir nesne projeksiyonu oluşturun.
1. Ayıklanan her görüntüyü yansıtarak bir dosya projeksiyonu oluşturun.
1. Belge tablosuna başvuruları, düzen metni ve dosya projeksiyonu içeren nesne projeksiyonu içeren bir çapraz başvuru tablosu oluşturun.

Bu değişiklikler knowledgeStore tanımına daha sonra yansıtılır. 

### <a name="shape-data-for-cross-projection"></a>Çapraz projeksiyon için veri şekil

Bu projeksiyonlar için ihtiyacımız olan şekilleri almak için, `crossProjection`adlı şekillendirilmiş bir nesne oluşturan yetenek başına yeni bir mil ekleyerek başlayın. 

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

Birleştirilmiş çapraz projeksiyon nesnesinden nesneyi birden çok tabloya dilimleyip OCR çıkışını blob olarak yakalayabilir ve sonra görüntüyü dosya olarak (BLOB depolama alanında da) kaydedebilirsiniz.

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

Bu örnek ayrıca projeksiyonun başka bir özelliğini de vurgular. Aynı projeksiyon nesnesi içinde birden çok tür projeksiyonları tanımlayarak, farklı türlerde (tablolar, nesneler, dosyalar) ifade edilen bir ilişki vardır ve bu, bir belge için bir tablo satırıyla başlayabilmenizi ve görüntüler için tüm OCR metinlerini bulmanızı sağlar nesne projeksiyonundaki bu belgenin içinde. 

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

+ Dize enzenginleştirmelerinin geçerli bir JSON 'a şekillendirmiyor. Dizeler uyumlu olduğunda (örneğin, anahtar tümcecikleriyle `merged_content` zenginleştirildikten sonra, zenginleştirme özelliği, zenginleştirme ağacı içinde `merged_content` alt öğesi olarak temsil edilir. Varsayılan Gösterim doğru biçimlendirilmiş JSON değil. Yansıtma zamanında, bir ad ve değer ile geçerli bir JSON nesnesine zenginleştirme dönüştürdiğinizden emin olun.

+ Kaynak yolunun sonundaki ```/*``` dışarıda. Projeksiyonun kaynağı `/document/pbiShape/keyPhrases`ise, anahtar tümceleri dizisi tek bir nesne/satır olarak yansıtıldır. Bunun yerine, anahtar tümceciklerin her biri için tek bir satır veya nesne sağlamak üzere kaynak yolunu `/document/pbiShape/keyPhrases/*` olarak ayarlayın.

+ Yol sözdizimi hataları. Yol seçicileri büyük/küçük harfe duyarlıdır ve seçici için tam büyük/küçük harf kullanmıyorsanız eksik giriş uyarılarına yol açabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki örneklerde, projeksiyonları oluşturma hakkında genel desenler gösterilmektedir. Kavramların iyi şekilde anlayışınız olduğuna göre, belirli senaryonuz için daha iyi bir geliştirme oluşturmaya daha iyi bir şekilde donatılmış olursunuz.

Yeni özellikleri araştırırken, bir sonraki adımınız olarak artımlı zenginleştirme yapmayı düşünün. Artımlı zenginleştirme, daha önce bir beceri değişikliğinden etkilenmeyen tüm zenginler 'i yeniden kullanmanıza imkan tanıyan önbelleğe alma işlemini temel alır. Bu, özellikle OCR ve görüntü analizini içeren işlem hatları için yararlıdır.

> [!div class="nextstepaction"]
> [Artımlı zenginleştirme ve önbelleğe alma bilgilerine giriş](cognitive-search-incremental-indexing-conceptual.md)

Projeksiyonlar hakkında genel bakış için, gruplar ve dilimleme gibi yetenekler hakkında daha fazla bilgi edinin ve [bunları beceri içinde nasıl tanımlacağınızı](knowledge-store-projection-overview.md) öğrenin

> [!div class="nextstepaction"]
> [Bilgi deposundaki projeksiyonler](knowledge-store-projection-overview.md)

