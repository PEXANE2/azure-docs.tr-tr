---
title: MongoDB için Azure Cosmos DB API kullanırken sorgu sorunlarını giderme
description: MongoDB sorgu sorunları için Azure Cosmos DB API 'sini belirlemeyi, tanılamayı ve sorun gidermeyi öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 10/12/2020
ms.author: tisande
ms.reviewer: sngun
ms.openlocfilehash: 615bd423296fb9ed2ee28cab9e362873a30ee7b9
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92284681"
---
# <a name="troubleshoot-query-issues-when-using-the-azure-cosmos-db-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API kullanırken sorgu sorunlarını giderme

Bu makalede, Azure Cosmos DB sorgularda sorun giderme için önerilen genel bir yaklaşım gösterilmektedir. Bu makalede özetlenen adımları olası sorgu sorunlarına karşı kapsamlı bir savunma olarak düşünmemeniz durumunda, en sık karşılaşılan performans ipuçlarını buradan sunuyoruz. Bu makaleyi, MongoDB için Azure Cosmos DB API 'sindeki yavaş veya pahalı sorguların sorunlarını gidermeye yönelik bir başlangıç yeri olarak kullanmanız gerekir. Azure Cosmos DB Core (SQL) API 'sini kullanıyorsanız, bkz. [SQL API sorgusu sorun giderme kılavuzu](troubleshoot-query-performance.md) makalesi.

Azure Cosmos DB sorgu iyileştirmeleri, genel olarak aşağıdaki gibi kategorilere ayrılmıştır:

- Sorgunun Istek birimi (RU) ücretlendirmesini azaltan iyileştirmeler
- Gecikme süresini azaltan iyileştirmeler

Bir sorgunun RU ücreti düşürüyoruz, genellikle gecikmeyi de azaltabilirsiniz.

Bu makalede, [beslenme veri kümesini](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json)kullanarak yeniden oluşturabileceğiniz örnekler sağlanmaktadır.

> [!NOTE] 
> Bu makalede, MongoDB için Azure Cosmos DB "s API 'sinin 3,6 sürümünü kullandığınızı varsaymaktadır. Sürüm 3,2 ' de düzgün şekilde gerçekleştiren bazı sorgular, sürüm 3,6 ' de önemli geliştirmelere sahiptir. Bir [destek isteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)kaydederek 3,6 sürümüne yükseltin.

## <a name="use-explain-command-to-get-metrics"></a>Ölçümleri almak için $explain komutunu kullanın

Azure Cosmos DB bir sorguyu en iyileştirebilirsiniz, ilk adım her zaman sorgunuz için [ru ücreti elde](find-request-unit-charge-mongodb.md) etmek için kullanılır. Kaba bir kılavuz olarak, 50 RU 'dan büyük ücretler içeren sorgular için RU ücretini düşürmenin yollarını araştırmalısınız. 

RU ücreti almanın yanı sıra, `$explain` sorgu ve dizin kullanım ölçümlerini elde etmek için komutunu kullanmanız gerekir. Sorgu çalıştıran ve `$explain` sorgu ve dizin kullanım ölçümlerini göstermek için komutunu kullanan bir örnek aşağıda verilmiştir:

**$explain komutu:**

```
db.coll.find({foodGroup: "Baby Foods"}).explain({"executionStatistics": true })
```

**Çıktıların**

```json
{
    "stages" : [ 
        {
            "stage" : "$query",
            "timeInclusiveMS" : 905.2888,
            "timeExclusiveMS" : 905.2888,
            "in" : 362,
            "out" : 362,
            "details" : {
                "database" : "db-test",
                "collection" : "collection-test",
                "query" : {
                    "foodGroup" : {
                        "$eq" : "Baby Foods"
                    }
                },
                "pathsIndexed" : [],
                "pathsNotIndexed" : [ 
                    "foodGroup"
                ],
                "shardInformation" : [ 
                    {
                        "activityId" : "e68e6bdd-5e89-4ec5-b053-3dbbc2428140",
                        "shardKeyRangeId" : "0",
                        "durationMS" : 788.5867,
                        "preemptions" : 1,
                        "outputDocumentCount" : 362,
                        "retrievedDocumentCount" : 8618
                    }
                ],
                "queryMetrics" : {
                    "retrievedDocumentCount" : 8618,
                    "retrievedDocumentSizeBytes" : 104963042,
                    "outputDocumentCount" : 362,
                    "outputDocumentSizeBytes" : 2553535,
                    "indexHitRatio" : 0.0016802042237178,
                    "totalQueryExecutionTimeMS" : 777.72,
                    "queryPreparationTimes" : {
                        "queryCompilationTimeMS" : 0.19,
                        "logicalPlanBuildTimeMS" : 0.14,
                        "physicalPlanBuildTimeMS" : 0.09,
                        "queryOptimizationTimeMS" : 0.03
                    },
                    "indexLookupTimeMS" : 0,
                    "documentLoadTimeMS" : 687.22,
                    "vmExecutionTimeMS" : 774.09,
                    "runtimeExecutionTimes" : {
                        "queryEngineExecutionTimeMS" : 37.45,
                        "systemFunctionExecutionTimeMS" : 10.82,
                        "userDefinedFunctionExecutionTimeMS" : 0
                    },
                    "documentWriteTimeMS" : 49.42
                }
            }
        }
    ],
    "estimatedDelayFromRateLimitingInMilliseconds" : 0.0,
    "continuation" : {
        "hasMore" : false
    },
    "ok" : 1.0
}
```

`$explain`Komut çıktısı uzun olur ve sorgu yürütme hakkında ayrıntılı bilgi içerir. Genellikle, sorgu performansını en iyi duruma getirirken odaklanmanız gereken birkaç bölüm vardır:

| Ölçüm | Açıklama | 
| ------ | ----------- |
| `timeInclusiveMS` | Arka uç sorgu gecikmesi |
| `pathsIndexed` | Sorgunun kullandığı dizinleri gösterir | 
| `pathsNotIndexed` | Varsa, sorgunun kullandığı dizinleri gösterir | 
| `shardInformation` | Belirli bir [fiziksel bölümün](partition-data.md#physical-partitions) sorgu performansının Özeti | 
| `retrievedDocumentCount` | Sorgu altyapısı tarafından yüklenen belge sayısı | 
| `outputDocumentCount` | Sorgu sonuçlarında döndürülen belge sayısı | 
| `estimatedDelayFromRateLimitingInMilliseconds` | Hız sınırlaması nedeniyle tahmini ek sorgu gecikmesi | 

Sorgu ölçümlerini aldıktan sonra, `retrievedDocumentCount` sorgunuz için ile ile karşılaştırın `outputDocumentCount` . Bu makaleyi gözden geçirmek üzere ilgili bölümleri belirlemek için bu karşılaştırmayı kullanın. , `retrievedDocumentCount`  Sorgu altyapısının yüklemesi gereken belge sayısıdır. , `outputDocumentCount` Sorgunun sonuçları için gereken belge sayısıdır. , `retrievedDocumentCount`  ' Den önemli ölçüde yüksekse `outputDocumentCount` , sorgunuzun bir dizini kullanmayan ve tarama yapmak için gerekli olan en az bir bölümü vardı.

Senaryonuza yönelik ilgili sorgu iyileştirmelerini anlamak için aşağıdaki bölümlere bakın.

### <a name="querys-ru-charge-is-too-high"></a>Sorgunun RU ücreti çok yüksek

#### <a name="retrieved-document-count-is-significantly-higher-than-output-document-count"></a>Alınan belge sayısı, çıktı belge sayısından önemli ölçüde daha yüksek

- [Gerekli dizinleri dahil et.](#include-necessary-indexes)

- [Hangi toplama işlemlerinin dizini kullandığını anlayın.](#understand-which-aggregation-operations-use-the-index)

#### <a name="retrieved-document-count-is-approximately-equal-to-output-document-count"></a>Alınan belge sayısı, çıktı belge sayısına yaklaşık olarak eşit

- [Çapraz bölüm sorgularını en aza indirin.](#minimize-cross-partition-queries)

### <a name="querys-ru-charge-is-acceptable-but-latency-is-still-too-high"></a>Sorgunun RU ücreti kabul edilebilir ancak gecikme hala çok yüksek

- [Yakınlığı geliştirir.](#improve-proximity)

- [Sağlanan aktarım hızını artırın.](#increase-provisioned-throughput)

## <a name="queries-where-retrieved-document-count-exceeds-output-document-count"></a>Alınan belge sayısı, çıkış belgesi sayısını aşarsa sorgular

 , `retrievedDocumentCount` Sorgu altyapısının yüklemek için gereken belge sayısıdır. , `outputDocumentCount` Sorgu tarafından döndürülen belge sayısıdır. , `retrievedDocumentCount` ' Den önemli ölçüde yüksekse `outputDocumentCount` , sorgunuzun bir dizini kullanmayan ve tarama yapmak için gerekli olan en az bir bölümü vardı.

Aşağıda, dizin tarafından tamamen hizmet edilmemiş tarama sorgusunun bir örneği verilmiştir:

**$explain komutu:**

```
db.coll.find(
  {
    $and : [
            { "foodGroup" : "Cereal Grains and Pasta"}, 
            { "description" : "Oat bran, cooked"}
        ]
  }
).explain({"executionStatistics": true })
```

**Çıktıların**

```json
{
    "stages" : [ 
        {
            "stage" : "$query",
            "timeInclusiveMS" : 436.5716,
            "timeExclusiveMS" : 436.5716,
            "in" : 1,
            "out" : 1,
            "details" : {
                "database" : "db-test",
                "collection" : "indexing-test",
                "query" : {
                    "$and" : [ 
                        {
                            "foodGroup" : {
                                "$eq" : "Cereal Grains and Pasta"
                            }
                        }, 
                        {
                            "description" : {
                                "$eq" : "Oat bran, cooked"
                            }
                        }
                    ]
                },
                "pathsIndexed" : [],
                "pathsNotIndexed" : [ 
                    "foodGroup", 
                    "description"
                ],
                "shardInformation" : [ 
                    {
                        "activityId" : "13a5977e-a10a-4329-b68e-87e4f0081cac",
                        "shardKeyRangeId" : "0",
                        "durationMS" : 435.4867,
                        "preemptions" : 1,
                        "outputDocumentCount" : 1,
                        "retrievedDocumentCount" : 8618
                    }
                ],
                "queryMetrics" : {
                    "retrievedDocumentCount" : 8618,
                    "retrievedDocumentSizeBytes" : 104963042,
                    "outputDocumentCount" : 1,
                    "outputDocumentSizeBytes" : 6064,
                    "indexHitRatio" : 0.0,
                    "totalQueryExecutionTimeMS" : 433.64,
                    "queryPreparationTimes" : {
                        "queryCompilationTimeMS" : 0.12,
                        "logicalPlanBuildTimeMS" : 0.09,
                        "physicalPlanBuildTimeMS" : 0.1,
                        "queryOptimizationTimeMS" : 0.02
                    },
                    "indexLookupTimeMS" : 0,
                    "documentLoadTimeMS" : 387.44,
                    "vmExecutionTimeMS" : 432.93,
                    "runtimeExecutionTimes" : {
                        "queryEngineExecutionTimeMS" : 45.36,
                        "systemFunctionExecutionTimeMS" : 16.86,
                        "userDefinedFunctionExecutionTimeMS" : 0
                    },
                    "documentWriteTimeMS" : 0.13
                }
            }
        }
    ],
    "estimatedDelayFromRateLimitingInMilliseconds" : 0.0,
    "continuation" : {
        "hasMore" : false
    },
    "ok" : 1.0
}
```

`retrievedDocumentCount`(8618), `outputDocumentCount` Bu sorgunun bir belge taramasını gerektirenden (1) önemli ölçüde daha yüksektir. 

### <a name="include-necessary-indexes"></a>Gerekli dizinleri dahil et

`pathsNotIndexed`Diziyi denetlemeniz ve bu dizinleri eklemeniz gerekir. Bu örnekte, yollar `foodGroup` ve `description` dizinlenmelidir.

```json
"pathsNotIndexed" : [ 
                    "foodGroup", 
                    "description"
                ]
```

MongoDB için Azure Cosmos DB API 'sindeki en iyi yöntemler dizin oluşturma, MongoDB 'den farklıdır. MongoDB için Azure Cosmos DB API 'sinde, bileşik dizinler yalnızca birden çok özelliğe göre etkili bir şekilde sıralanması gereken sorgularda kullanılır. Birden çok özelliklerde filtre içeren sorgular varsa, bu özelliklerden her biri için tek bir alan dizini oluşturmanız gerekir. Sorgu koşulları, birden çok tek alan dizini kullanabilir.

[Joker karakter dizinleri](mongodb-indexing.md#wildcard-indexes) , dizin oluşturmayı basitleştirecek. MongoDB 'nin aksine, joker karakter dizinleri sorgu koşullarına göre birden çok alanı destekleyebilir. Her özellik için ayrı bir dizin oluşturmak yerine bir tek joker karakter dizini kullanırsanız sorgu performansında bir farklılık olmayacaktır. Tüm özellikler için joker bir dizin eklemek, tüm sorgularınızı iyileştirmek için en kolay yoldur.

Herhangi bir zamanda yazma veya okuma kullanılabilirliği üzerinde hiçbir etkisi olmayacak şekilde yeni dizinler ekleyebilirsiniz. [Dizin dönüştürme ilerlemesini izleyebilirsiniz](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-net-sdk-v3).

### <a name="understand-which-aggregation-operations-use-the-index"></a>Hangi toplama işlemlerinin Dizin kullandığını anlayın

Çoğu durumda, MongoDB için Azure Cosmos DB API 'sindeki toplama işlemleri kısmi olarak dizinleri kullanacaktır. Genellikle, sorgu altyapısı önce eşitlik ve Aralık filtrelerini uygular ve dizinleri kullanır. Bu filtreleri uyguladıktan sonra sorgu altyapısı, ek filtreleri değerlendirebilir ve gerekirse toplamı hesaplamak için kalan belgeleri yüklemeyi çare olarak gerçekleştirebilir. 

İşte bir örnek:

```
db.coll.aggregate( [
   { $match: { foodGroup: 'Fruits and Fruit Juices' } },
   {
     $group: {
        _id: "$foodGroup",
        total: { $max: "$version" }
     }
   }
] )
```

Bu durumda, dizinler aşamayı iyileştirebilirler `$match` . İçin dizin eklemek `foodGroup` , sorgu performansını önemli ölçüde iyileştirir. MongoDB 'de olduğu gibi, `$match` dizinlerin kullanımını en üst düzeye çıkarmak için toplama ardışık düzeninde erken olarak yerleştirmeniz gerekir.

MongoDB için Azure Cosmos DB API 'sinde, dizinler gerçek toplama için kullanılmaz, bu durumda `$max` . Üzerinde dizin eklemek `version` , sorgu performansını iyileştirmez.

## <a name="queries-where-retrieved-document-count-is-equal-to-output-document-count"></a>Alınan belge sayısı, çıkış belgesi sayısına eşit olan sorgular

, `retrievedDocumentCount` Yaklaşık olarak öğesine eşitse `outputDocumentCount` sorgu altyapısının çok sayıda gereksiz belgeyi taraması gerekmez.

### <a name="minimize-cross-partition-queries"></a>Çapraz bölüm sorgularını en aza indir

Azure Cosmos DB, Istek birimi ve veri depolama alanı artışına göre bağımsız kapsayıcıları ölçeklendirmek için [bölümleme](partitioning-overview.md) kullanır. Her fiziksel bölümün ayrı ve bağımsız bir dizini vardır. Sorgunuzun, kapsayıcının bölüm anahtarı ile eşleşen bir eşitlik filtresi varsa, yalnızca ilgili bölümün dizinini denetlemeniz gerekir. Bu iyileştirme, sorgunun gerektirdiği toplam ru sayısını azaltır. [Bölüm içi sorgular ve çapraz bölümleme sorguları arasındaki farklılıklar hakkında daha fazla bilgi edinin](how-to-query-container.md).

Çok sayıda sağlanan ru (30.000 ' den fazla) veya büyük miktarda veri depolanmış (yaklaşık 100 GB 'tan fazla) varsa, sorgu RU ücretlerinde önemli bir düşüş görmeniz için büyük olasılıkla çok sayıda kapsayıcısına sahip olursunuz. 

`shardInformation`Her bir fiziksel bölümün sorgu ölçümlerini anlamak için diziyi kontrol edebilirsiniz. Benzersiz değer sayısı, `shardKeyRangeId` sorgunun yürütülmesi gereken fiziksel bölüm sayısıdır. Bu örnekte, sorgu dört fiziksel bölümde yürütüldü. Yürütmenin Dizin kullanımının tamamen bağımsız olduğunu anlamak önemlidir. Diğer bir deyişle, çapraz bölümleme sorguları dizinleri kullanmaya devam edebilir.

```json
  "shardInformation" : [ 
                    {
                        "activityId" : "42f670a8-a201-4c58-8023-363ac18d9e18",
                        "shardKeyRangeId" : "5",
                        "durationMS" : 24.3859,
                        "preemptions" : 1,
                        "outputDocumentCount" : 463,
                        "retrievedDocumentCount" : 463
                    }, 
                    {
                        "activityId" : "a8bf762a-37b9-4c07-8ed4-ae49961373c0",
                        "shardKeyRangeId" : "2",
                        "durationMS" : 35.8328,
                        "preemptions" : 1,
                        "outputDocumentCount" : 905,
                        "retrievedDocumentCount" : 905
                    }, 
                    {
                        "activityId" : "3754e36b-4258-49a6-8d4d-010555628395",
                        "shardKeyRangeId" : "1",
                        "durationMS" : 67.3969,
                        "preemptions" : 1,
                        "outputDocumentCount" : 1479,
                        "retrievedDocumentCount" : 1479
                    }, 
                    {
                        "activityId" : "a69a44ee-db97-4fe9-b489-3791f3d52878",
                        "shardKeyRangeId" : "0",
                        "durationMS" : 185.1523,
                        "preemptions" : 1,
                        "outputDocumentCount" : 867,
                        "retrievedDocumentCount" : 867
                    }
                ]
```

## <a name="optimizations-that-reduce-query-latency"></a>Sorgu gecikmesini azaltan iyileştirmeler

Birçok durumda, sorgu gecikmesi hala çok yüksek olduğunda RU ücreti kabul edilebilir durumda olabilir. Aşağıdaki bölümler sorgu gecikmesini azaltmaya yönelik ipuçlarına genel bakış sunar. Aynı sorguyu aynı veri kümesinde birden çok kez çalıştırırsanız, genellikle her seferinde aynı RU ücretine sahip olur. Ancak sorgu gecikmesi sorgu yürütmeleri arasında farklılık gösterebilir.

### <a name="improve-proximity"></a>Yakınlığı geliştirme

Azure Cosmos DB hesabından farklı bir bölgeden çalıştırılan sorgular, aynı bölgede çalıştırıldıklarından daha yüksek gecikme süresine sahip olur. Örneğin, masaüstü bilgisayarınızda kod çalıştırıyorsanız, sorgunun aynı Azure bölgesindeki bir sanal makineden Azure Cosmos DB kadar (veya daha fazla) olması beklendiğinden, gecikme süresinin on veya yüzlerce milisaniyelik (veya daha fazla) olması beklenir. Verilerinizi uygulamanıza yakın bir şekilde getirebilmeniz için [Azure Cosmos DB verileri genel olarak dağıtmak](tutorial-global-distribution-mongodb.md) basit bir işlemdir.

### <a name="increase-provisioned-throughput"></a>Sağlanan aktarım hızını artır

Azure Cosmos DB, sağlanan aktarım hızı, Istek birimleri (ru) cinsinden ölçülür. 5 ' lik bir iş hacmi tüketen bir sorgunuz olduğunu düşünün. Örneğin, 1.000 ru 'yi sağlarsanız, bu sorguyu saniye başına 200 kez çalıştırabileceksiniz. Kullanılabilir yeterli üretilen iş olmadığında sorguyu çalıştırmayı denediyseniz, Azure Cosmos DB istekleri derecelendirmek için ücret uygulanır. MongoDB için Azure Cosmos DB API 'SI, kısa bir süre bekledikten sonra bu sorguyu otomatik olarak yeniden deneyecek. Kısıtlanmış istekler daha uzun sürer, bu nedenle sağlanan verimlilik artarak sorgu gecikmesi iyileştirebilirler.

Değer, `estimatedDelayFromRateLimitingInMilliseconds` aktarım hızını arttırmanız durumunda olası gecikme avantajlarının bir fikir verir.

## <a name="next-steps"></a>Sonraki adımlar

* [Sorgu performansı sorunlarını giderme (SQL API)](troubleshoot-query-performance.md)
* [MongoDB için Azure Cosmos DB API 'sinde Dizin oluşturmayı yönetme](mongodb-indexing.md)
