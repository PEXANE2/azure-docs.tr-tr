---
title: Azure Cosmos DB 'de dizin oluşturma
description: Azure Cosmos DB ' de dizin oluşturmanın nasıl çalıştığını anlayın.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: thweiss
ms.openlocfilehash: c8e21ea89f3e23709d636ab8af4716bff76d7217
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479279"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Azure Cosmos DB Dizin oluşturma-genel bakış

Azure Cosmos DB, şema veya dizin yönetimiyle uğraşmak zorunda kalmadan uygulamanızda yineleme yapmanızı sağlayan şemadan bağımsız bir veritabanıdır. Varsayılan olarak, Azure Cosmos DB herhangi bir şemayı tanımlamaya veya ikincil dizinleri yapılandırmaya gerek kalmadan, [kapsayıcıdaki](databases-containers-items.md#azure-cosmos-containers) tüm öğeler için her özelliği otomatik olarak dizine ekler.

Bu makalenin amacı, Azure Cosmos DB verileri nasıl dizinleyen ve sorgu performansını artırmak için dizinleri nasıl kullandığını açıklamaktır. [Dizin oluşturma ilkelerinin](index-policy.md)nasıl özelleştirileceğine ilişkin araştırmadan önce bu bölümden gitmeniz önerilir.

## <a name="from-items-to-trees"></a>Öğelerden ağaçlara

Bir öğe kapsayıcıda her depolandığında, içeriği bir JSON belgesi olarak yansıtıldıktan sonra ağaç gösterimine dönüştürülür. Bu, söz konusu öğenin her özelliğinin bir ağaçta düğüm olarak temsil edildiği anlamına gelir. Sözde kök düğüm, öğenin tüm ilk düzey özelliklerine üst öğe olarak oluşturulur. Yaprak düğümleri bir öğe tarafından taşınan gerçek skaler değerleri içerir.

Örnek olarak, şu öğeyi göz önünde bulundurun:

    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 },
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }

Aşağıdaki ağaç tarafından temsil edilir:

![Ağaç olarak temsil edilen önceki öğe](./media/index-overview/item-as-tree.png)

Dizilerin ağaçta nasıl kodlandığını unutmayın: dizideki her giriş dizideki bu girdinin diziniyle etiketlenmiş bir ara düğüm alır (0, 1 vb.).

## <a name="from-trees-to-property-paths"></a>Ağaçlardan Özellik yollarına

Azure Cosmos DB öğeleri ağaçlara dönüştürmesinin nedeni, özelliklerin bu ağaçlar içindeki yollarıyla başvurulmasını sağlar. Bir özelliğin yolunu almak için, ağacı kök düğümden bu özelliğe çapraz geçiş yapabilir ve her bir çapraz düğüm etiketini birleştirebilirsiniz.

Yukarıda açıklanan örnek öğeden her bir özelliğin yolları aşağıda verilmiştir:

    /locations/0/country: "Germany"
    /locations/0/city: "Berlin"
    /locations/1/country: "France"
    /locations/1/city: "Paris"
    /headquarters/country: "Belgium"
    /headquarters/employees: 250
    /exports/0/city: "Moscow"
    /exports/1/city: "Athens"

Bir öğe yazıldığında, Azure Cosmos DB her bir özelliğin yolunu ve karşılık gelen değerini etkin bir şekilde dizine ekler.

## <a name="index-kinds"></a>Dizin türleri

Azure Cosmos DB Şu anda üç tür dizini desteklemektedir:

**Aralık** Dizin türü için kullanılır:

- Eşitlik sorguları:

    ```sql
   SELECT * FROM container c WHERE c.property = 'value'
    ```

- Aralık sorguları:

   ```sql
   SELECT * FROM container c WHERE c.property > 'value'
   ``` 
  ( `>` ,`<=` ,,,`!=`,,,, için geçerlidir) `<` `>=`

- `ORDER BY`lardır

   ```sql 
   SELECT * FROM container c ORDER BY c.property
   ```

- `JOIN`lardır

   ```sql
   SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'
   ```

Aralık dizinleri, skaler değerlerde (dize veya sayı) kullanılabilir.

**Uzamsal** Dizin türü için kullanılır:

- Jeo-uzamsal uzaklık sorguları: 

   ```sql
   SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40
   ```

- Sorgular içindeki Jeo uzamsal: 

   ```sql
   SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] } })
   ```

Uzamsal dizinler, doğru biçimli [geojson](geospatial.md) nesnelerinde kullanılabilir. Noktaları, LineStrings ve çokgenler Şu anda destekleniyor.

**Bileşik** Dizin türü için kullanılır:

- `ORDER BY`birden çok özelliklerde sorgular: 

   ```sql
   SELECT * FROM container c ORDER BY c.firstName, c.lastName
   ```

## <a name="querying-with-indexes"></a>Dizinlerle sorgulama

Verileri dizinlerken ayıklanan yollar, bir sorgu işlenirken dizinde arama yapmayı kolaylaştırır. Dizinli yolların listesiyle `WHERE` bir sorgunun yan tümcesini eşleştirerek sorgu koşulunun çok çabuk eşleşen öğelerini tanımlamak mümkündür.

Örneğin, aşağıdaki sorguyu göz önünde bulundurun: `SELECT location FROM location IN company.locations WHERE location.country = 'France'`. Sorgu koşulu (herhangi bir konumda ülke olarak "Fransa" bulunduğu öğeler üzerinde filtreleme) aşağıdaki kırmızı renkle eşleşen yol ile eşleşir:

![Ağaç içindeki belirli bir yolu eşleştirme](./media/index-overview/matching-path.png)

> [!NOTE]
> Tek `ORDER BY` bir özelliğe göre siparişlerin *her zaman* bir Aralık dizinine ihtiyacı olan ve başvurduğu yolun bir tane yoksa başarısız olacağı bir yan tümce. Benzer şekilde, birden `ORDER BY` çok sorgunun *her zaman* bileşik dizine ihtiyacı vardır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde dizin oluşturma hakkında daha fazla bilgi edinin:

- [Dizin oluşturma ilkesi](index-policy.md)
- [Dizin oluşturma ilkesini yönetme](how-to-manage-indexing-policy.md)
