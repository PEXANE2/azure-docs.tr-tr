---
title: Jeo uzamsal verileri Azure Cosmos DB sorgulama
description: Uzamsal verileri Azure Cosmos DB ile sorgulama
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: 08b12bd9d35aaa61c79d35a55068983cdc0f1b83
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77566328"
---
# <a name="querying-geospatial-data-with-azure-cosmos-db"></a>Jeo uzamsal verileri Azure Cosmos DB sorgulama

Bu makalede, SQL ve LINQ kullanarak Azure Cosmos DB Jeo uzamsal verileri sorgulama ele alınacaktır. Şu anda Jeo uzamsal verileri depolama ve bunlara erişme yalnızca SQL API hesapları Azure Cosmos DB desteklenir. Azure Cosmos DB, Jeo-uzamsal sorgulama için aşağıdaki Open Geospatial Consortium (OGC) yerleşik işlevlerini destekler. SQL dilinde yerleşik işlevlerin tam kümesi hakkında daha fazla bilgi için, bkz. [Azure Cosmos db sorgu sistemi işlevleri](sql-query-system-functions.md).

## <a name="spatial-sql-built-in-functions"></a>Uzamsal SQL yerleşik işlevleri

Aşağıda, Azure Cosmos DB sorgulamak için yararlı olan Jeo-uzamsal sistem işlevlerinin bir listesi verilmiştir:

|**Kullanımıyla**|**Açıklama**|
|---|---|
| ST_DISTANCE (spatial_expr, spatial_expr) | İki GeoJSON noktası, çokgen veya LineString ifadeleri arasındaki mesafeyi döndürür.|
|ST_WITHIN (spatial_expr, spatial_expr) | İlk GeoJSON nesnesinin (nokta, çokgen veya LineString) ikinci GeoJSON nesnesi (Point, çokgen veya LineString) içinde olup olmadığını gösteren bir Boole ifadesi döndürür.|
|ST_INTERSECTS (spatial_expr, spatial_expr)| Belirtilen iki coğrafi JSON nesnesinin (nokta, çokgen veya LineString) kesişip kesişmediğini gösteren bir Boole ifadesi döndürür.|
|ST_ISVALID| Belirtilen GeoJSON noktası, çokgen veya LineString ifadesinin geçerli olup olmadığını gösteren bir Boole değeri döndürür.|
| ST_ISVALIDDETAILED| Belirtilen GeoJSON noktası, çokgen veya LineString ifadesi geçerliyse, Boole değeri içeren bir JSON değeri döndürür. Geçersiz ise, nedeni bir dize değeri olarak döndürür.|

Uzamsal işlevler, uzamsal verilere karşı yakınlık sorguları gerçekleştirmek için kullanılabilir. Örneğin, `ST_DISTANCE` yerleşik işlevi kullanılarak belirtilen konumun 30 km içinde olan tüm aile belgelerini döndüren bir sorgu aşağıda verilmiştir.

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

Dizin oluşturma ilkenize uzamsal dizin oluşturma eklerseniz, "uzaklık sorguları" Dizin aracılığıyla verimli bir şekilde sunulacaktır. Uzamsal dizin oluşturma hakkında daha fazla bilgi için bkz. [Jeo uzamsal dizin oluşturma](sql-query-geospatial-index.md). Belirtilen yollar için bir uzamsal dizininiz yoksa, sorgu kapsayıcının taramasını yapılır.

`ST_WITHIN`bir noktanın bir çokgen içinde olup olmadığını denetlemek için kullanılabilir. Genellikle çokgenler, ZIP kodları, durum sınırları veya doğal biçimlendirme gibi sınırları temsil etmek için kullanılır. Dizin oluşturma ilkenize uzamsal dizin oluşturma eklerseniz, "içindeki" sorgular dizin aracılığıyla verimli bir şekilde sunulacaktır.

İçindeki `ST_WITHIN` Çokgen bağımsız değişkenleri yalnızca tek bir halka içerebilir, diğer bir deyişle, çokgenlerin içerdikleri delikleri içermemesi gerekir.

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
> Eşleşmeyen türlerin Azure Cosmos DB sorgusunda çalışmasına benzer şekilde, iki bağımsız değişkende belirtilen konum değeri hatalı biçimlendirilmiş veya geçersiz ise, **tanımsız** olarak değerlendirilir ve sorgu sonuçlarından atlanacak şekilde değerlendirilen belgeyi kabul edilir. Sorgunuz hiçbir sonuç döndürürse, uzamsal türün neden `ST_ISVALIDDETAILED` geçersiz olduğunu ayıklamak için öğesini çalıştırın.
>
>

Azure Cosmos DB ayrıca ters sorgular gerçekleştirmeyi destekler, diğer bir deyişle, Azure Cosmos DB çokgenler veya satırları dizinlerken, belirtilen bir noktayı içeren alanlara yönelik sorgu oluşturabilirsiniz. Bu model, örneğin, bir kamyonun belirlenmiş bir alana girdiğinde veya ayrıldığında tanımlanması için genellikle lojistik 'da kullanılır.

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

`ST_ISVALID`ve `ST_ISVALIDDETAILED` , bir uzamsal nesnenin geçerli olup olmadığını denetlemek için kullanılabilir. Örneğin, aşağıdaki sorgu Aralık dışı bir enlem değeri (-132,8) ile bir noktanın geçerliliğini denetler. `ST_ISVALID`yalnızca bir Boole değeri döndürür ve `ST_ISVALIDDETAILED` Boolean ve neden geçersiz olarak kabul edildiği nedenini içeren bir dize döndürür.

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

Bu işlevler, çokgenler doğrulamak için de kullanılabilir. Örneğin, burada kapatılmayan bir `ST_ISVALIDDETAILED` çokgeni doğrulamak için kullanırız.

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

SQL .NET SDK, Ayrıca, LINQ ifadeleri `Distance()` içinde `Within()` kullanılmak üzere saplama yöntemleri ve kullanım için sağlayıcıları. SQL LINQ sağlayıcısı bu yöntemi, eşdeğer SQL yerleşik işlev çağrılarına çevirir (ST_DISTANCE ve ST_WITHIN sırasıyla).

Aşağıda, Azure Cosmos kapsayıcısındaki, `location` değeri LINQ kullanarak belirtilen noktanın bir yarıçapı içinde olan tüm belgeleri bulan bir LINQ sorgusu örneği verilmiştir.

**Mesafe için LINQ sorgusu**

```csharp
    foreach (UserProfile user in container.GetItemLinqQueryable<UserProfile>(allowSynchronousQueryExecution: true)
        .Where(u => u.ProfileType == "Public" && u.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }
```

Benzer şekilde, belirtilen kutu/çokgen içinde olan `location` tüm belgeleri bulmaya yönelik bir sorgu aşağıda verilmiştir.

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

Artık Azure Cosmos DB Jeo-uzamsal desteği kullanmaya nasıl başladığınıza öğrendiğinize göre şunları yapabilirsiniz:

* [Azure Cosmos db sorgu](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Jeo-uzamsal ve coğrafi JSON konumu verileri](sql-query-geospatial-intro.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Ile dizin uzamsal verileri](sql-query-geospatial-index.md) hakkında daha fazla bilgi edinin
