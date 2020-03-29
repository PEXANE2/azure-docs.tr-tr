---
title: Azure Cosmos DB Gremlin API'deki sorguları değerlendirmek için yürütme profilini kullanın
description: Yürütme profili adımını kullanarak Gremlin sorgularınızı nasıl gidereceğinizi ve geliştireceğinizi öğrenin.
services: cosmos-db
author: luisbosquez
manager: kfile
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: lbosq
ms.openlocfilehash: 5705ef4fb6aa895009d554617c968543cc3fcd63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75441846"
---
# <a name="how-to-use-the-execution-profile-step-to-evaluate-your-gremlin-queries"></a>Gremlin sorgularınızı hesaplamak için yürütme profili adımını kullanma

Bu makale, Azure Cosmos DB Gremlin API’si graf veritabanları için yürütme profilini kullanma adımı için bir genel bakış sunar. Bu adım, sorun giderme ve sorgu iyileştirmeleri için ilgili bilgileri sağlar ve Cosmos DB Gremlin API’si hesabında yürütülebilen tüm Gremlin sorgularıyla uyumludur.

Bu adımı kullanmak için Gremlin sorgunuzun sonundaki `executionProfile()` işlev çağrısını eklemeniz yeterlidir. **Gremlin sorgunuz yürütülür** ve işlemin sonucu sorgu yürütme profili ile bir JSON yanıt nesnesi döndürecek.

Örnek:

```java
    // Basic traversal
    g.V('mary').out()

    // Basic traversal with execution profile call
    g.V('mary').out().executionProfile()
```

`executionProfile()` Adımı çağırdıktan sonra, yanıt, yürütülen Gremlin adımını, toplam süresini ve deyimin sonuçlandığı Cosmos DB çalışma zamanı işleçlerini içeren bir JSON nesnesi olacaktır.

> [!NOTE]
> Yürütme Profili için bu uygulama Apache Tinkerpop belirtiminde tanımlanmamıştır. Azure Cosmos DB Gremlin API uygulamasına özgüdür.


## <a name="response-example"></a>Yanıt Örneği

Aşağıda, döndürülecek çıktının açıklamalı bir örneği verilmiştir:

> [!NOTE]
> Bu örnek, yanıtın genel yapısını açıklayan açıklamalarla açıklanır. Gerçek bir yürütmeProfil yanıtı herhangi bir yorum içermez.

```json
[
  {
    // The Gremlin statement that was executed.
    "gremlin": "g.V('mary').out().executionProfile()",

    // Amount of time in milliseconds that the entire operation took.
    "totalTime": 28,

    // An array containing metrics for each of the steps that were executed. 
    // Each Gremlin step will translate to one or more of these steps.
    // This list is sorted in order of execution.
    "metrics": [
      {
        // This operation obtains a set of Vertex objects.
        // The metrics include: time, percentTime of total execution time, resultCount, 
        // fanoutFactor, count, size (in bytes) and time.
        "name": "GetVertices",
        "time": 24,
        "annotations": {
          "percentTime": 85.71
        },
        "counts": {
          "resultCount": 2
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 2,
            "size": 696,
            "time": 0.4
          }
        ]
      },
      {
        // This operation obtains a set of Edge objects. 
        // Depending on the query, these might be directly adjacent to a set of vertices, 
        // or separate, in the case of an E() query.
        //
        // The metrics include: time, percentTime of total execution time, resultCount, 
        // fanoutFactor, count, size (in bytes) and time.
        "name": "GetEdges",
        "time": 4,
        "annotations": {
          "percentTime": 14.29
        },
        "counts": {
          "resultCount": 1
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 1,
            "size": 419,
            "time": 0.67
          }
        ]
      },
      {
        // This operation obtains the vertices that a set of edges point at.
        // The metrics include: time, percentTime of total execution time and resultCount.
        "name": "GetNeighborVertices",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      },
      {
        // This operation represents the serialization and preparation for a result from 
        // the preceding graph operations. The metrics include: time, percentTime of total 
        // execution time and resultCount.
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      }
    ]
  }
]
```

> [!NOTE]
> ExecutionProfile adımı Gremlin sorgusunu yürütecek. Bu, `addV` oluşturmayla sonuçlanacak ve sorguda belirtilen değişiklikleri gerçekleştirecek olan veya `addE`adımları içerir. Sonuç olarak, Gremlin sorgusu tarafından oluşturulan İstek Birimleri de ücretlendirilir.

## <a name="execution-profile-response-objects"></a>Yürütme profili yanıt nesneleri

Bir yürütmeProfil() işlevinin yanıtı, JSON nesnelerinin hiyerarşisini aşağıdaki yapıya sahip olarak verir:
  - **Gremlin işlem nesnesi**: Yürütülen tüm Gremlin işlemini temsil eder. Aşağıdaki özellikleri içerir.
    - `gremlin`: İdam edilen açık Gremlin beyanı.
    - `totalTime`: Milisaniye cinsinden, adımın yürütülmesinin tahakkuk ettiği süre. 
    - `metrics`: Sorguyu yerine getirmek için çalıştırılan Cosmos DB çalışma zamanı işleçlerinin her birini içeren bir dizi. Bu liste yürütme sırasına göre sıralanır.
    
  - **Cosmos DB çalışma zamanı işleçleri**: Tüm Gremlin işleminin bileşenlerinin her birini temsil eder. Bu liste yürütme sırasına göre sıralanır. Her nesne aşağıdaki özellikleri içerir:
    - `name`: Operatörün adı. Bu, değerlendirilen ve yürütülen adım türüdür. Aşağıdaki tabloda daha fazla bilgi edinin.
    - `time`: Belirli bir işleç tarafından milisaniye cinsinden verilen süre.
    - `annotations`: Çalıştırılan işleç için özel ek bilgiler içerir.
    - `annotations.percentTime`: Belirli bir işleci yürütmek için gereken toplam sürenin yüzdesi.
    - `counts`: Bu işleç tarafından depolama katmanından döndürülen nesne sayısı. Bu, içindeki `counts.resultCount` skaler değerde bulunur.
    - `storeOps`: Bir veya birden çok bölüme yayılabilecek bir depolama işlemini temsil eder.
    - `storeOps.fanoutFactor`: Bu özel depolama işleminin erişilen bölüm sayısını gösterir.
    - `storeOps.count`: Bu depolama işleminin döndürüldettiği sonuç sayısını gösterir.
    - `storeOps.size`: Belirli bir depolama işleminin sonucunun baytboyutunu temsil eder.

Cosmos DB Gremlin Çalışma Zamanı Operatörü|Açıklama
---|---
`GetVertices`| Bu adım, kalıcılık katmanından önceden belirlenmiş bir nesne kümesi elde eder. 
`GetEdges`| Bu adım, bir tepe kümesine bitişik kenarları elde eder. Bu adım, bir veya çok sayıda depolama işlemineden olabilir.
`GetNeighborVertices`| Bu adım, kenarlar kümesine bağlı vertices alır. Kenarlar, hem kaynak hem de hedef vertices'in bölüm anahtarlarını ve kimliklerini içerir.
`Coalesce`| Bu adım, `coalesce()` Gremlin adımı yürütüldüğünde iki operasyonun değerlendirilmesi için hesaplar.
`CartesianProductOperator`| Bu adım, iki veri kümesi arasında bir kartezyen ürün bilgisayara. Genellikle yüklemler `to()` kullanıldığında veya `from()` kullanıldığında çalıştırılır.
`ConstantSourceOperator`| Bu adım, sonuç olarak sabit bir değer üretmek için bir ifadeyi hesaplar.
`ProjectOperator`| Bu adım, önceki işlemlerin sonucunu kullanarak bir yanıt hazırlar ve seri hale leştirir.
`ProjectAggregation`| Bu adım, toplu işlem için bir yanıt hazırlar ve seri hale sağlar.

> [!NOTE]
> Yeni işleçler eklendikçe bu liste güncelleştirilmeye devam edecektir.

## <a name="examples-on-how-to-analyze-an-execution-profile-response"></a>Yürütme profili yanıtı nasıl çözümlenenebilen örnekler

Yürütme Profili yanıtı kullanılarak tespit edilebilen yaygın optimizasyonlara örnekler aşağıda verilmiştir:
  - Kör fan-out sorgusu.
  - Filtre uygulanmamış sorgu.

### <a name="blind-fan-out-query-patterns"></a>Kör fan-out sorgu desenleri

Bölümlenmiş bir **grafikten**aşağıdaki yürütme profili yanıtını varsayalım:

```json
[
  {
    "gremlin": "g.V('tt0093640').executionProfile()",
    "totalTime": 46,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 46,
        "annotations": {
          "percentTime": 100
        },
        "counts": {
          "resultCount": 1
        },
        "storeOps": [
          {
            "fanoutFactor": 5,
            "count": 1,
            "size": 589,
            "time": 75.61
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      }
    ]
  }
]
```

Aşağıdaki sonuçlar ondan yapılabilir:
- Gremlin deyimi deseni `g.V('id')`izlediğinden sorgu, tek bir kimlik aramasidır.
- Metrik bakılırsa, tek [bir nokta okuma işlemi için 10ms'den fazla](https://docs.microsoft.com/azure/cosmos-db/introduction#guaranteed-low-latency-at-99th-percentile-worldwide)olduğundan bu sorgunun gecikme si yüksek gibi görünüyor. `time`
- Nesneye `storeOps` bakacak olursak, bu işlemle `5`5 [bölüme](https://docs.microsoft.com/azure/cosmos-db/partition-data) erişildiği anlamına gelen `fanoutFactor` bir nesne olduğunu görebiliriz.

Bu çözümlemenin bir sonucu olarak, ilk sorgunun gerekenden daha fazla bölüme erişebildiğinizi belirleyebiliriz. Bu, sorgudaki bölümleme anahtarını yüklem olarak belirterek ele alınabilir. Bu, sorgu başına daha az gecikme ve daha az maliyet sağlar. [Grafik bölümleme](graph-partitioning.md)hakkında daha fazla bilgi edinin. Daha uygun bir `g.V('tt0093640').has('partitionKey', 't1001')`sorgu olacaktır.

### <a name="unfiltered-query-patterns"></a>Filtre uygulanmamış sorgu desenleri

Aşağıdaki iki yürütme profili yanıtını karşılaştırın. Basitlik için, bu örnekler tek bir bölümlü grafik kullanır.

Bu ilk sorgu etiketle `tweet` birlikte tüm vertices alır ve sonra komşu vertices alır:

```json
[
  {
    "gremlin": "g.V().hasLabel('tweet').out().executionProfile()",
    "totalTime": 42,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 31,
        "annotations": {
          "percentTime": 73.81
        },
        "counts": {
          "resultCount": 30
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 13,
            "size": 6819,
            "time": 1.02
          }
        ]
      },
      {
        "name": "GetEdges",
        "time": 6,
        "annotations": {
          "percentTime": 14.29
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 20,
            "size": 7950,
            "time": 1.98
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 5,
        "annotations": {
          "percentTime": 11.9
        },
        "counts": {
          "resultCount": 20
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 4,
            "size": 1070,
            "time": 1.19
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 20
        }
      }
    ]
  }
]
```

Bitişik vertices keşfetmeden önce, aynı sorguprofiline dikkat edin, ancak şimdi ek bir filtre ile: `has('lang', 'en')`

```json
[
  {
    "gremlin": "g.V().hasLabel('tweet').has('lang', 'en').out().executionProfile()",
    "totalTime": 14,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 14,
        "annotations": {
          "percentTime": 58.33
        },
        "counts": {
          "resultCount": 11
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 11,
            "size": 4807,
            "time": 1.27
          }
        ]
      },
      {
        "name": "GetEdges",
        "time": 5,
        "annotations": {
          "percentTime": 20.83
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 18,
            "size": 7159,
            "time": 1.7
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 5,
        "annotations": {
          "percentTime": 20.83
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 4,
            "size": 1070,
            "time": 1.01
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 18
        }
      }
    ]
  }
]
```

Bu iki sorgu aynı sonuca ulaşmış, ancak, bitişik öğeleri sorgulamadan önce daha büyük bir ilk veri kümesini yinelemek için gerekli olduğundan ilki daha fazla İstek Birimi gerektirir. Her iki yanıttan aşağıdaki parametreleri karşılaştırırken bu davranışın göstergelerini görebiliriz:
- Değer `metrics[0].time` ilk yanıtta daha yüksektir, bu da bu tek adımın çözülmesinin daha uzun sürdüğünü gösterir.
- İlk `metrics[0].counts.resultsCount` yanıtta da değer daha yüksektir, bu da ilk çalışan veri kümesinin daha büyük olduğunu gösterir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB'deki [desteklenen Gremlin özellikleri](gremlin-support.md) hakkında bilgi edinin. 
* [Azure Cosmos DB'deki Gremlin API'si](graph-introduction.md)hakkında daha fazla bilgi edinin.
