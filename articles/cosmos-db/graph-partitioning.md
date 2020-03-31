---
title: Azure Cosmos DB Gremlin API'de veri bölümleme
description: Azure Cosmos DB'de bölümlenmiş bir grafiği nasıl kullanabileceğinizi öğrenin. Bu makalede, bölümlenmiş bir grafik için gereksinimleri ve en iyi uygulamaları açıklar.
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 06/24/2019
ms.custom: seodec18
ms.openlocfilehash: 44d3b7c2b9e23b90f696162747d9728b18fb7d3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77623366"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Azure Cosmos DB'de bölümlenmiş graf kullanma

Azure Cosmos DB'deki Gremlin API'sinin temel özelliklerinden biri, büyük ölçekli grafikleri yatay ölçekleme yoluyla işleyebilme özelliğidir. Kapsayıcılar depolama ve iş artışı açısından bağımsız olarak ölçeklendirilebilir. Azure Cosmos DB'de grafik verilerini depolamak için otomatik olarak ölçeklendirilebilen kapsayıcılar oluşturabilirsiniz. Veriler, belirtilen **bölüm tuşuna**göre otomatik olarak dengelenir.

Kapsayıcının 20 GB'tan fazla boyutu nda depolaması bekleniyorsa veya saniyede 10.000'den fazla istek birimi (RUs) ayırmak istiyorsanız **bölümleme gereklidir.** [Azure Cosmos DB bölümleme mekanizmasının](partition-data.md) aynı genel ilkeleri, aşağıda açıklanan birkaç grafiğe özgü optimizasyonla birlikte geçerlidir.

![Grafik bölümleme.](./media/graph-partitioning/graph-partitioning.png)

## <a name="graph-partitioning-mechanism"></a>Grafik bölümleme mekanizması

Aşağıdaki yönergeler, Azure Cosmos DB'deki bölümleme stratejisinin nasıl çalıştığını açıklar:

- **Hem tepe ler hem de kenarlar JSON belgeleri olarak depolanır.**

- **Vertices bir bölüm anahtarı gerektirir.** Bu anahtar, tepe noktasının karma algoritması aracılığıyla hangi bölümde depolanacağını belirler. Bölüm anahtarı özellik adı yeni bir kapsayıcı oluştururken tanımlanır `/partitioning-key-name`ve bir biçimi vardır: .

- **Kenarlar kendi kaynak tepe noktası ile depolanır.** Başka bir deyişle, her tepe noktası için bölüm anahtarı, giden kenarlarıyla birlikte nerede depolandıklarını tanımlar. Bu optimizasyon grafik sorgularında `out()` önemli liğini kullanırken çapraz bölüm sorguları önlemek için yapılır.

- **Kenarlar işaret ettikleri tepe noktalarına başvurular içerir.** Tüm kenarlar, işaret ettikleri tepe uçlarının bölüm tuşları ve disleri ile depolanır. Bu hesaplama, tüm `out()` yön sorgularının her zaman bir kapsamlı bölümlenmiş sorgu olmasını sağlar, kör bir çapraz bölümsorgusu değil. 

- **Grafik sorguları bir bölüm anahtarı belirtmeniz gerekir.** Azure Cosmos DB'deki yatay bölümlemeden tam olarak yararlanmak için, tek bir tepe noktası seçildiğinde ve mümkün olduğunda bölüm anahtarı belirtilmelidir. Bölümlenmiş bir grafikte bir veya birden çok vertices seçmek için sorgular şunlardır:

    - `/id`ve `/label` Gremlin API'deki bir kapsayıcı için bölüm anahtarları olarak desteklenmez.


    - Kimlik tarafından bir tepe noktası seçerek, bölüm **anahtar özelliğini belirtmek için `.has()` adımı kullanarak:** 
    
        ```java
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Bölüm anahtar değeri **ve kimliği de dahil olmak üzere bir tuple belirterek bir**tepe noktası seçme: 
    
        ```java
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - **Bölüm anahtar değerleri ve d'lerinin bir dizi tuples**belirtilmesi:
    
        ```java
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Kimliklerini içeren bir vertices kümesi ni seçme ve **bölüm anahtar değerlerinin listesini belirtme:** 
    
        ```java
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

    - Bir sorgunun başında **Bölümleme stratejisini** kullanma ve Gremlin sorgusunun geri kalanının kapsamı için bir bölüm belirtme: 
    
        ```java
        g.withStrategies(PartitionStrategy.build().partitionKey('partitionKey').readPartitions('partitionKey_value').create()).V()
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Bölümlenmiş grafik kullanırken en iyi uygulamalar

Sınırsız kapsayıcılı bölümlenmiş grafikler kullanırken performans ve ölçeklenebilirlik sağlamak için aşağıdaki yönergeleri kullanın:

- **Bir tepe noktası sorgularken her zaman bölüm anahtar değerini belirtin.** Bilinen bir bölümden tepe noktası elde etmek, performans elde etmenin bir yoludur. Kenarlar hedef tepe tabanlarına referans kimliği ve bölüm anahtarı içerdiğinden, sonraki tüm adjacency işlemleri her zaman bir bölüme yöneliktir.

- **Mümkün olduğunda kenarları sorgularken giden yönü kullanın.** Yukarıda belirtildiği gibi, kenarlar giden yönde kendi kaynak vertices ile saklanır. Böylece, veriler ve sorgular bu desen göz önünde bulundurularak tasarlandığında, bölümler arası sorgulara başvurma şansı en aza indirilir. Aksine, `in()` sorgu her zaman pahalı bir fan-out sorgu olacaktır.

- **Verileri bölümler arasında eşit olarak dağıtacak bir bölüm anahtarı seçin.** Bu karar büyük ölçüde çözümün veri modeline bağlıdır. [Azure Cosmos DB'de Bölümleme ve ölçeklendirmede](partition-data.md)uygun bir bölüm anahtarı oluşturma hakkında daha fazla bilgi edinin.

- **Bir bölümün sınırları içinde veri elde etmek için sorguları en iyi duruma getirin.** En iyi bölümleme stratejisi sorgu desenleri hizalanmış olacaktır. Tek bir bölümden veri alan sorgular mümkün olan en iyi performansı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra aşağıdaki makaleleri okumaya devam edebilirsiniz:

* Azure [Cosmos DB'de Bölümleme ve ölçeklendirme](partition-data.md)hakkında bilgi edinin.
* [Gremlin API'deki Gremlin desteği](gremlin-support.md)hakkında bilgi edinin.
* [Gremlin API'ye Giriş](graph-introduction.md)hakkında bilgi edinin.
