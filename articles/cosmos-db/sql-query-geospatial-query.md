---
title: Azure Cosmos DB ile coğrafi verileri sorgulama
description: Azure Cosmos DB ile uzamsal verileri sorgulama
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: 08b12bd9d35aaa61c79d35a55068983cdc0f1b83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77566328"
---
# <a name="querying-geospatial-data-with-azure-cosmos-db"></a>Azure Cosmos DB ile coğrafi verileri sorgulama

Bu makalede, SQL ve LINQ kullanarak Azure Cosmos DB'deki coğrafi verilerin nasıl sorgulanır. Şu anda jeouzamsal verilerin depolanması ve erişilmesi yalnızca Azure Cosmos DB SQL API hesapları tarafından desteklenir. Azure Cosmos DB, coğrafi sorgu için aşağıdaki Açık Jeouzamsal Konsorsiyum (OGC) yerleşik işlevlerini destekler. SQL dilindeki yerleşik işlevlerin tamamı hakkında daha fazla bilgi için [Azure Cosmos DB'deki Sorgu Sistemi Fonksiyonları'na](sql-query-system-functions.md)bakın.

## <a name="spatial-sql-built-in-functions"></a>Uzamsal SQL yerleşik işlevleri

Azure Cosmos DB'de sorgulama için yararlı olan jeouzamsal sistem işlevlerinin bir listesi aşağıda veda edilmiştir:

|**Kullanım**|**Açıklama**|
|---|---|
| ST_DISTANCE (spatial_expr, spatial_expr) | İki GeoJSON Noktası, Çokgen veya LineString ifadesi arasındaki mesafeyi döndürür.|
|ST_WITHIN (spatial_expr, spatial_expr) | İlk GeoJSON nesnesinin (Nokta, Çokgen veya LineString) ikinci GeoJSON nesnesi (Nokta, Çokgen veya LineString) içinde olup olmadığını belirten bir Boolean ifadesi döndürür.|
|ST_INTERSECTS (spatial_expr, spatial_expr)| Belirtilen iki GeoJSON nesnesinin (Nokta, Çokgen veya LineString) kesişip kesişmediğini belirten bir Boolean ifadesi döndürür.|
|ST_ISVALID| Belirtilen GeoJSON Point, Polygon veya LineString ifadesinin geçerli olup olmadığını belirten bir Boolean değeri döndürür.|
| ST_ISVALIDDETAILED| Belirtilen GeoJSON Point, Polygon veya LineString ifadesi geçerliyse Boolean değeri içeren bir JSON değeri döndürür. Geçersizse, nedeni dize değeri olarak döndürür.|

Uzamsal işlevler, uzamsal verilere karşı yakınlık sorguları gerçekleştirmek için kullanılabilir. Örneğin, yerleşik işlevi kullanarak belirtilen konuma 30 km mesafedeki tüm aile `ST_DISTANCE` belgelerini döndüren bir sorgu aşağıda verilmiştir.

**Sorgu**

```sql
    SELECT f.id
    FROM Families f
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000
```

**Sonuç -ları**

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Dizin oluşturma ilkenize uzamsal dizin ekleme eklerseniz, dizin aracılığıyla "mesafe sorguları" verimli bir şekilde sunulur. Uzamsal indeksleme hakkında daha fazla bilgi için [jeouzamsal indeksleme'ye](sql-query-geospatial-index.md)bakın. Belirtilen yollar için uzamsal dizin yoksa, sorgu kapsayıcının bir tarar.

`ST_WITHIN`bir noktanın Çokgen içinde olup olmadığını kontrol etmek için kullanılabilir. Genellikle Çokgenler posta kodları, durum sınırları veya doğal oluşumlar gibi sınırları temsil etmek için kullanılır. Yine dizin oluşturma ilkenize uzamsal dizin ekleme eklerseniz, "içinde" sorguları dizin aracılığıyla verimli bir şekilde sunulacaktır.

Çokgen bağımsız `ST_WITHIN` değişkenleri yalnızca tek bir halka içerebilir, yani Çokgenler içlerinde delik içermemelidir.

**Sorgu**

```sql
    SELECT *
    FROM Families f
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon',
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })
```

**Sonuç -ları**

```json
    [{
      "id": "WakefieldFamily",
    }]
```

> [!NOTE]
> Azure Cosmos DB sorgusunda eşleşmesi olmayan türlerin nasıl çalıştığına benzer şekilde, bağımsız değişkende belirtilen konum değeri yanlış bilgilendirilmiş veya geçersizse, **tanımlanmamış** ve sorgu sonuçlarından atlanacak değerlendirilen belgeyi değerlendirir. Sorgunuz sonuç döndürmezse, uzamsal türün neden geçersiz olduğunu hata ayıklamak için çalıştırın. `ST_ISVALIDDETAILED`
>
>

Azure Cosmos DB ayrıca ters sorgular gerçekleştirmeyi de destekler, diğer bir deyişle Azure Cosmos DB'de çokgenleri veya satırları dizine ekleyebilir, ardından belirli bir nokta içeren alanları sorgulayabilirsiniz. Bu desen, örneğin bir kamyonun belirli bir alana girdiğini veya ayrıldığını belirlemek için lojistiğinde yaygın olarak kullanılır.

**Sorgu**

```sql
    SELECT *
    FROM Areas a
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)
```

**Sonuç -ları**

```json
    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon",
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]
```

`ST_ISVALID`ve `ST_ISVALIDDETAILED` uzamsal bir nesnenin geçerli olup olmadığını kontrol etmek için kullanılabilir. Örneğin, aşağıdaki sorgu, aralık dışı enlem değeri (-132,8) olan bir noktanın geçerliliğini denetler. `ST_ISVALID`yalnızca bir Boolean değerini `ST_ISVALIDDETAILED` döndürür ve Boolean'ı ve geçersiz kabul edilmesinin nedenini içeren bir dize döndürür.

**Sorgu**

```sql
    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })
```

**Sonuç -ları**

```json
    [{
      "$1": false
    }]
```

Bu işlevler çokgenleri doğrulamak için de kullanılabilir. Örneğin, burada kapalı `ST_ISVALIDDETAILED` olmayan bir Çokgen doğrulamak için kullanın.

**Sorgu**

```sql
    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})
```

**Sonuç -ları**

```json
    [{
       "$1": { 
            "valid": false, 
            "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a Polygon must have the same start and end points." 
          }
    }]
```

## <a name="linq-querying-in-the-net-sdk"></a>.NET SDK'da LINQ sorgulama

SQL .NET SDK ayrıca saplama yöntemlerini `Distance()` ve `Within()` LINQ ifadeleri içinde kullanım için de sağlayıcılar. SQL LINQ sağlayıcısı bu yöntemi eşdeğer SQL yerleşik işlev çağrılarına çevirir (sırasıyla ST_DISTANCE ve ST_WITHIN).

Aşağıda, DEĞERI BELIRTILEN noktanın 30 km yarıçapı içinde olan `location` Azure Cosmos kapsayıcısındaki tüm belgeleri LINQ kullanarak bulan bir LINQ sorgusu örneği verilmiştir.

**Mesafe için LINQ sorgusu**

```csharp
    foreach (UserProfile user in container.GetItemLinqQueryable<UserProfile>(allowSynchronousQueryExecution: true)
        .Where(u => u.ProfileType == "Public" && u.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }
```

Benzer şekilde, burada belirtilen kutu / Çokgen içinde olan `location` tüm belgeleri bulmak için bir sorgu.

**Within için LINQ sorgusu**

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

Azure Cosmos DB'de coğrafi destekle nasıl başladığınızı öğrendiğiniz için, bir sonraki

* [Azure Cosmos DB Sorgusu](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB'deki Geospatial ve GeoJSON konum verileri](sql-query-geospatial-intro.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB ile uzamsal verileri Dizin](sql-query-geospatial-index.md) le ilgili daha fazla bilgi edinin
