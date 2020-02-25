---
title: Jeo uzamsal verileri Azure Cosmos DB sorgulama
description: Uzamsal verileri Azure Cosmos DB ile sorgulama
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: 08b12bd9d35aaa61c79d35a55068983cdc0f1b83
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77566328"
---
# <a name="querying-geospatial-data-with-azure-cosmos-db"></a>Jeo uzamsal verileri Azure Cosmos DB sorgulama

Bu makalede, SQL ve LINQ kullanarak Azure Cosmos DB Jeo uzamsal verileri sorgulama ele alınacaktır. Şu anda Jeo uzamsal verileri depolama ve bunlara erişme yalnızca SQL API hesapları Azure Cosmos DB desteklenir. Azure Cosmos DB, Jeo-uzamsal sorgulamak için aşağıdaki açık Jeo-uzamsal Consortium (OGC) yerleşik işlevleri destekler. SQL dilinde yerleşik işlevlerin tam kümesi hakkında daha fazla bilgi için, bkz. [Azure Cosmos db sorgu sistemi işlevleri](sql-query-system-functions.md).

## <a name="spatial-sql-built-in-functions"></a>Uzamsal SQL yerleşik işlevler

Aşağıda, Azure Cosmos DB sorgulamak için yararlı olan Jeo-uzamsal sistem işlevlerinin bir listesi verilmiştir:

|**Kullanım**|**Açıklama**|
|---|---|
| ST_DISTANCE (spatial_expr, spatial_expr) | İki GeoJSON noktası, çokgen veya LineString ifadeler uzaklığı döndürür.|
|ST_WITHIN (spatial_expr, spatial_expr) | İlk GeoJSON nesne (noktası, çokgen veya LineString) ikinci GeoJSON nesne içinde (noktası, çokgen veya LineString) olup olmadığını gösteren bir Boole ifadesi döndürür.|
|ST_INTERSECTS (spatial_expr, spatial_expr)| İki belirtilen GeoJSON nesnesi (noktası, çokgen veya LineString) kesişen olup olmadığını belirten bir Boole ifadesi döndürür.|
|ST_ISVALID| Belirtilen GeoJSON noktası, çokgen veya LineString ifade geçerli olup olmadığını gösteren bir Boole değeri döndürür.|
| ST_ISVALIDDETAILED| Belirtilen GeoJSON noktası, çokgen veya LineString ifadesi geçerliyse, Boole değeri içeren bir JSON değeri döndürür. Geçersiz ise, nedeni bir dize değeri olarak döndürür.|

Uzamsal İşlevler, uzamsal veri yakınlık sorguları gerçekleştirmek için kullanılabilir. Örneğin, `ST_DISTANCE` yerleşik işlevini kullanarak belirtilen konumun 30 km dahilinde olan tüm aile belgelerini döndüren bir sorgu aşağıda verilmiştir.

**Sorgu**

```sql
    SELECT f.id
    FROM Families f
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000
```

**Sonuçlar**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Uzamsal dizin oluşturma ilkenizi dizin oluşturma dahil, "uzaklık sorguları" verimli bir şekilde dizini alabilecektir. Uzamsal dizin oluşturma hakkında daha fazla bilgi için bkz. [Jeo uzamsal dizin oluşturma](sql-query-geospatial-index.md). Belirtilen yollar için bir uzamsal dizininiz yoksa, sorgu kapsayıcının taramasını yapılır.

`ST_WITHIN`, bir noktanın çokgen içinde olup olmadığını denetlemek için kullanılabilir. Yaygın olarak çokgenler, posta kodları, durumu sınırları veya doğal formations gibi sınırlarını temsil etmek için kullanılır. Uzamsal dizin oluşturma ilkenizi dizin oluşturma dahil, yeniden ardından "içinde" sorguları verimli bir şekilde dizini alabilecektir.

`ST_WITHIN` Çokgen bağımsız değişkenleri yalnızca tek bir halka içerebilir, diğer bir deyişle, çokgenler içinde boşluklar içermemelidir.

**Sorgu**

```sql
    SELECT *
    FROM Families f
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon',
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })
```

**Sonuçlar**

```json
    [{
      "id": "WakefieldFamily",
    }]
```

> [!NOTE]
> Eşleşmeyen türlerin Azure Cosmos DB sorgusunda çalışmasına benzer şekilde, iki bağımsız değişkende belirtilen konum değeri hatalı biçimlendirilmiş veya geçersiz ise, **tanımsız** olarak değerlendirilir ve sorgu sonuçlarından atlanacak şekilde değerlendirilen belgeyi kabul edilir. Sorgunuz hiçbir sonuç döndürürse, uzamsal türün neden geçersiz olduğunu ayıklamak için `ST_ISVALIDDETAILED` çalıştırın.
>
>

Azure Cosmos DB ayrıca Ters sorgular gerçekleştirdiğini destekler, diğer bir deyişle, çokgenler veya Azure Cosmos DB'de satırları dizin sonra belirli bir noktaya içeren alanlar için sorgu. Bu Düzen yaygın olarak Lojistik bir kamyon girdiğinde veya belirlenen bir alan bırakır, örneğin, tanımlamak için kullanılır.

**Sorgu**

```sql
    SELECT *
    FROM Areas a
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)
```

**Sonuçlar**

```json
    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon",
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]
```

`ST_ISVALID` ve `ST_ISVALIDDETAILED`, bir uzamsal nesnenin geçerli olup olmadığını denetlemek için kullanılabilir. Örneğin, aşağıdaki sorguyu dışı aralık enlem değeri (-132.8) ile bir noktası geçerliliğini denetler. `ST_ISVALID` yalnızca bir Boolean değer döndürür ve `ST_ISVALIDDETAILED`, Boolean ve neden geçersiz kabul edildiği nedenini içeren bir dize döndürür.

**Sorgu**

```sql
    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })
```

**Sonuçlar**

```json
    [{
      "$1": false
    }]
```

Bu işlevler, çokgenler doğrulamak için de kullanılabilir. Örneğin, burada kapalı olmayan bir çokgeni doğrulamak için `ST_ISVALIDDETAILED` kullanırız.

**Sorgu**

```sql
    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})
```

**Sonuçlar**

```json
    [{
       "$1": { 
            "valid": false, 
            "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a Polygon must have the same start and end points." 
          }
    }]
```

## <a name="linq-querying-in-the-net-sdk"></a>.NET SDK 'da LINQ sorgulama

SQL .NET SDK Ayrıca, LINQ ifadelerinde kullanım için `Distance()` ve `Within()` sağlayıcıları saplama yöntemlerine sahiptir. Bu yöntem SQL LINQ sağlayıcısı çevirir eşdeğer SQL yerleşik işlev çağrıları çağrı (ST_DISTANCE ve ST_WITHIN sırasıyla).

Aşağıda, Azure Cosmos kapsayıcısındaki, `location` değeri, LINQ kullanarak belirtilen noktanın bir yarıçapı içinde olan tüm belgeleri bulan bir LINQ sorgusuna örnek verilmiştir.

**Mesafe için LINQ sorgusu**

```csharp
    foreach (UserProfile user in container.GetItemLinqQueryable<UserProfile>(allowSynchronousQueryExecution: true)
        .Where(u => u.ProfileType == "Public" && u.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }
```

Benzer şekilde, `location` belirtilen kutu/çokgen içinde olan tüm belgeleri bulmaya yönelik bir sorgu aşağıda verilmiştir.

**Içinde için LINQ sorgusu**

```csharp
    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in container.GetItemLinqQueryable<UserProfile>(allowSynchronousQueryExecution: true)
         .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de Jeo-uzamsal destek kullanmaya başlamak öğrendiniz, sonra şunları yapabilirsiniz:

* [Azure Cosmos db sorgu](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Jeo-uzamsal ve coğrafi JSON konumu verileri](sql-query-geospatial-intro.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Ile dizin uzamsal verileri](sql-query-geospatial-index.md) hakkında daha fazla bilgi edinin
