---
title: Azure Cosmos DB Gremlin API veri bölümleme
description: Bölümlenmiş bir grafik Azure Cosmos DB'de nasıl kullanabileceğinizi öğrenin. Bu makalede ayrıca gereksinimleri ve bölümlenmiş bir grafik için en iyi uygulamaları açıklar.
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 06/24/2019
ms.custom: seodec18
ms.openlocfilehash: 44d3b7c2b9e23b90f696162747d9728b18fb7d3f
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77623366"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Bölümlenmiş bir grafik Azure Cosmos DB içinde kullanma

Azure Cosmos DB Gremlin API anahtar özelliklerini de yatay ölçeklendirme aracılığıyla büyük ölçekli grafikleri işlemek yeteneğidir. Kapsayıcıları, depolama ve aktarım hızı bakımından bağımsız olarak ölçeklendirebilirsiniz. Azure Cosmos DB'de graf verilerini depolamak için otomatik olarak ölçeklendirilebilir kapsayıcılar oluşturabilirsiniz. Veriler, belirtilen **bölüm anahtarına**göre otomatik olarak dengelenir.

Kapsayıcının boyutunun 20 GB 'den fazlasını depolaması bekleniyorsa veya saniyede 10.000 ' den fazla istek birimi (ru) ayırmak istiyorsanız **bölümlendirme gerekir** . [Azure Cosmos DB bölümleme mekanizmasından](partition-data.md) aynı genel ilkeler aşağıda açıklanan bazı grafiğe özgü iyileştirmeler ile uygulanır.

![Grafik bölümleme.](./media/graph-partitioning/graph-partitioning.png)

## <a name="graph-partitioning-mechanism"></a>Grafik bölümleme mekanizması

Aşağıdaki kılavuzlar Azure Cosmos DB ' de bölümleme stratejisinin nasıl çalıştığını açıklamaktadır:

- **Her iki köşe ve kenar da JSON belgeleri olarak depolanır**.

- Köşeler **bir bölüm anahtarı gerektirir**. Bu anahtar, hangi bölüme bir karma algoritma köşe depolanacak belirler. Bölüm anahtarı özellik adı, yeni bir kapsayıcı oluşturulurken tanımlanır ve şu biçimdedir: `/partitioning-key-name`.

- **Kenarlar, kaynak köşelerine sahip olacak**. Diğer bir deyişle, her köşe için bölüm anahtarını giden kenarlarını birlikte depolandıkları tanımlar. Bu iyileştirme, grafik sorgularında `out()` kardinaliteyi kullanırken, bölümler arası sorgulardan kaçınmak için yapılır.

- **Kenarlar işaret ettikleri köşelerin başvurularını içerir**. Tüm kenarlar, işaret ettikleri köşelerin bölüm anahtarları ve kimlikleriyle birlikte depolanır. Bu hesaplama, tüm `out()` yön sorgularının her zaman kapsamlı bölümlenmiş bir sorgu olmasını sağlar ve bu durum, geçici bir çapraz bölümlü sorgu değildir. 

- **Grafik sorgularının bir bölüm anahtarı belirtmesi gerekir**. Tek bir köşe seçildiğinde mümkün olduğunda yatay Azure Cosmos DB'de bölümleme tam avantajından yararlanmak için bölüm anahtarı belirtilmesi gerekir. Bölümlenmiş bir grafikte bir veya daha fazla köşe seçmeye yönelik sorgular şunlardır:

    - `/id` ve `/label` Gremlin API 'sindeki bir kapsayıcı için bölüm anahtarları olarak desteklenmez.


    - KIMLIĞE göre bir köşe seçip **`.has()` adımını kullanarak bölüm anahtarı özelliğini belirtin**: 
    
        ```java
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - **Bölüm anahtarı değeri ve kimliği dahil bir tanımlama grubu belirterek**köşe seçme: 
    
        ```java
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - **Bölüm anahtarı değerleri ve kimlikleri için bir dizi tanımlama dizisi**belirtme:
    
        ```java
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Kimlikleri olan bir köşe kümesi seçme ve **bölüm anahtarı değerlerinin bir listesini belirtme**: 
    
        ```java
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

    - Bir sorgunun başlangıcında **bölüm stratejisi** kullanma ve Gremlin sorgusunun geri kalanının kapsamı için bir bölüm belirtme: 
    
        ```java
        g.withStrategies(PartitionStrategy.build().partitionKey('partitionKey').readPartitions('partitionKey_value').create()).V()
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Bölümlenmiş bir grafik kullanırken en iyi yöntemler

Sınırsız kapsayıcılar ile bölümlenmiş grafiklerini kullanarak, performans ve ölçeklenebilirlik sağlamak için aşağıdaki yönergeleri kullanın:

- **Bir köşeyi sorgularken bölüm anahtarı değerini her zaman belirtin**. Köşe bilinen bir bölümün dışında alma performans elde etmek için bir yoldur. Kenarlar, hedef köşelerine başvuru KIMLIĞI ve bölüm anahtarı içerdiğinden sonraki tüm bitişik Bitişiklik işlemleri her zaman bir bölüm kapsamına alınır.

- **Mümkün olduğunda kenarları sorgularken giden yönü kullanın**. Yukarıda belirtildiği gibi kendi kaynak köşe giden yönde ile kenarlar depolanır. Bu nedenle bu düzendeki aklınızda sorgular ve verileri tasarlanırsa bölümler arası sorgular için maksimum olasılığını en aza indirilir. Aksine, `in()` sorgusu her zaman pahalı bir fan sorgusu olacaktır.

- **Verileri bölümler arasında eşit olarak dağıtan bir bölüm anahtarı seçin**. Bu karar, çözümün veri modeli üzerinde yoğun bir şekilde bağlıdır. [Azure Cosmos DB bölümleyip ölçeklendirerek](partition-data.md)uygun bölüm anahtarı oluşturma hakkında daha fazla bilgi edinin.

- **Bir bölümün sınırları içinde veri almak için sorguları iyileştirin**. Sorgulama desenleri için uygun bir bölümleme stratejisi hizalanabilir. Tek bölümden veri elde sorguları mümkün olan en iyi performansı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Ardından bu makaleleri okuyun geçebilirsiniz:

* Azure Cosmos DB bölümünde [bölüm ve ölçek](partition-data.md)hakkında bilgi edinin.
* [GREMLIN API 'de Gremlin desteği](gremlin-support.md)hakkında bilgi edinin.
* [Gremlin API 'Sine giriş](graph-introduction.md)hakkında bilgi edinin.
