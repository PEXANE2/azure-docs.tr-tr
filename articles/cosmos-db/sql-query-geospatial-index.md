---
title: Azure Cosmos DB ile coğrafi verileri dizinle
description: Azure Cosmos DB ile uzamsal verileri dizine
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: eb0a2b2778b3217e185b9883def6eaa54674cc5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137912"
---
# <a name="index-geospatial-data-with-azure-cosmos-db"></a>Azure Cosmos DB ile coğrafi verileri dizinle

Azure Cosmos DB'nin veritabanı motorini gerçekten şema agnostik olacak ve JSON için birinci sınıf destek sağlayacak şekilde tasarladık. Azure Cosmos DB'nin yazma optimize veritabanı altyapısı, GeoJSON standardında temsil edilen uzamsal verileri yerel olarak anlar.

Özetle, geometri jeodezik koordinatlardan 2B düzleme yansıtılır ve daha sonra kademeli olarak bir **dörtağaç**kullanarak hücrelere bölünür. Bu hücreler, noktaların yerelliğini koruyan **Hilbert boşluk dolum eğrisi**içindeki hücrenin konumuna göre 1D'ye eşlenir. Ayrıca, konum verileri dizine eklendiğinde, **tessellation**olarak bilinen bir işlemden geçer, yani bir konumla kesişen tüm hücreler Azure Cosmos DB dizininde anahtar olarak tanımlanır ve depolanır. Sorgu sırasında, nokta ve Çokgenler gibi bağımsız değişkenler de ilgili hücre kimliği aralıklarını ayıklamak için tessellated, daha sonra dizin veri almak için kullanılır.

/* (tüm yollar) için uzamsal dizini içeren bir dizin oluşturma ilkesi belirtirseniz, kapsayıcıda bulunan tüm veriler verimli uzamsal sorgular için dizine eklenir.

> [!NOTE]
> Azure Cosmos DB Puan, LineString, Çokgenler ve Çok Gen'lerin dizinesini destekler
>
>

## <a name="modifying-geospatial-data-type"></a>Jeouzamsal veri türünü değiştirme

Kapsayıcınızda, jeouzamsal verilerin nasıl dizine alınacağını `geospatialConfig` belirtir. Kapsayıcı başına `geospatialConfig` bir tane belirtmelisiniz: coğrafya veya geometri. Belirtilmemişse, `geospatialConfig` coğrafya veri türü varsayılan olacaktır. Modiyi `geospatialConfig`değiştirdiğinizde, kapsayıcıdaki tüm varolan jeouzamsal veriler yeniden dizine olur.

> [!NOTE]
> Azure Cosmos DB şu anda .NET SDK'daki geospatialConfig değişikliklerini yalnızca 3.6 ve üzeri sürümlerde desteklemektedir.
>

Burada `geometry` `geospatialConfig` özelliği ayarlayarak ve bir **sınırlayıcı Kutusu**ekleyerek jeouzamsal veri türünü değiştirmek için bir örnektir:

```csharp
    //Retrieve the container's details
    ContainerResponse containerResponse = await client.GetContainer("db", "spatial").ReadContainerAsync();
    //Set GeospatialConfig to Geometry
    GeospatialConfig geospatialConfig = new GeospatialConfig(GeospatialType.Geometry);
    containerResponse.Resource.GeospatialConfig = geospatialConfig;
    // Add a spatial index including the required boundingBox
    SpatialPath spatialPath = new SpatialPath
            {  
                Path = "/locations/*",
                BoundingBox = new BoundingBoxProperties(){
                    Xmin = 0,
                    Ymin = 0,
                    Xmax = 10,
                    Ymax = 10
                }
            };
    spatialPath.SpatialTypes.Add(SpatialType.Point);
    spatialPath.SpatialTypes.Add(SpatialType.LineString);
    spatialPath.SpatialTypes.Add(SpatialType.Polygon);
    spatialPath.SpatialTypes.Add(SpatialType.MultiPolygon);

    containerResponse.Resource.IndexingPolicy.SpatialIndexes.Add(spatialPath);

    // Update container with changes
    await client.GetContainer("db", "spatial").ReplaceContainerAsync(containerResponse.Resource);
```

## <a name="geography-data-indexing-examples"></a>Coğrafya veri dizini örnekleri

Aşağıdaki JSON snippet, **coğrafya** veri türü için uzamsal dizin oluşturma etkin leştirilmiş bir dizin oluşturma ilkesi ni gösterir. Bu coğrafya veri türü ile uzamsal veriler için geçerlidir ve herhangi bir GeoJSON Point, Polygon, MultiPolygon veya LineString uzamsal sorgu için belgeler içinde bulunan dizin olacaktır. Azure portalını kullanarak dizin oluşturma ilkesini değiştiriyorsanız, kapsayıcınızda uzamsal dizin oluşturmayı etkinleştirmek için dizin oluşturma ilkesi için aşağıdaki JSON'u belirtebilirsiniz:

**Coğrafya mekansal indeksleme ile konteyner indeksleme ilkesi JSON**

```json
    {
       "automatic":true,
       "indexingMode":"Consistent",
        "includedPaths": [
        {
            "path": "/*"
        }
        ],
        "spatialIndexes": [
        {
            "path": "/*",
            "types": [
                "Point",
                "Polygon",
                "MultiPolygon",
                "LineString"
            ]
        }
    ],
       "excludedPaths":[]
    }
```

> [!NOTE]
> Belgeiçindeki konum GeoJSON değeri yanlış bilgilendirilmiş veya geçersizse, uzamsal sorgu için dizine alınmaz. ST_ISVALID ve ST_ISVALIDDETAILED kullanarak konum değerlerini doğrulayabilirsiniz.

Azure CLI, PowerShell veya herhangi bir SDK'yı kullanarak [dizin oluşturma ilkesini de değiştirebilirsiniz.](how-to-manage-indexing-policy.md)

## <a name="geometry-data-indexing-examples"></a>Geometri veri dizini örnekleri

Coğrafya veri türüne benzer **geometri** veri türünde, dizin için ilgili yolları ve türleri belirtmeniz gerekir. Ayrıca, bu belirli yol `boundingBox` için dizine eklenmesi istenen alanı belirtmek için dizin oluşturma ilkesi içinde bir de belirtmeniz gerekir. Her jeouzamsal`boundingBox`yol kendi gerektirir.

Sınırlayıcı kutu aşağıdaki özelliklerden oluşur:

- **xmin**: minimum endeksli x koordinat
- **ymin**: minimum endeksli y koordinatı
- **xmax**: maksimum endeksli x koordinat
- **ymax**: maksimum endeksli y koordinatı

Geometrik veriler sonsuz olabilecek bir düzlemi kapladığı için sınırlayıcı bir kutu gereklidir. Ancak uzamsal dizinler sınırlı bir alan gerektirir. **Coğrafya** veri türü için, Dünya sınırdır ve bir sınırlandırma kutusu ayarlamanız gerekmez.

Verilerinizin tümünün (veya çoğunun) tümünün (veya çoğunun) içeren bir sınırlayıcı kutusu oluşturmanız gerekir. Yalnızca sınırlayıcı kutunun içinde olan nesneler üzerinde hesaplanan işlemler uzamsal dizini kullanabilir. Sınırlayıcı kutusunu gerekenden önemli ölçüde daha büyük yapmamalısınız, çünkü bu sorgu performansını olumsuz etkileyecektir.

Geometri **verilerini** **geospatialConfig** ile indeksleyen bir örnek `geometry`indeksleme ilkesi aşağıda verilmiştir:

```json
 {
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": [
        {
            "path": "/\"_etag\"/?"
        }
    ],
    "spatialIndexes": [
        {
            "path": "/locations/*",
            "types": [
                "Point",
                "LineString",
                "Polygon",
                "MultiPolygon"
            ],
            "boundingBox": {
                "xmin": -10,
                "ymin": -20,
                "xmax": 10,
                "ymax": 20
            }
        }
    ]
}
```

Yukarıdaki dizin oluşturma ilkesi, x koordinatları için (-10, 10) ve y koordinatları için (-20, 20) **bir sınırlayıcı Kutusu** vardır. Yukarıdaki dizin oluşturma ilkesine sahip kapsayıcı, tamamen bu bölge içinde olan tüm Noktaları, Çokgenleri, ÇokGenleri ve LineString'leri dizine dizine alacaktır.

> [!NOTE]
> Veri türüne sahip bir kapsayıcıya **sınırlayıcı Kutu** içeren bir dizin oluşturma ilkesi eklemeye çalışırsanız, başarısız olur. `geography` Bir **sınırlayıcı Kutusu**eklemeden önce **konteynerin geospatialConfig** değiştirmek `geometry` gerekir. Kapsayıcının jeouzamsal veri türünü seçmeden önce veya sonra veri ekleyebilir ve dizin oluşturma ilkenizin geri kalanını (yollar ve türler gibi) değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de coğrafi destekle nasıl başladığınızı öğrendiğiniz için, bir sonraki

* [Azure Cosmos DB Sorgusu](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB ile uzamsal verileri sorgulama](sql-query-geospatial-query.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB'deki Geospatial ve GeoJSON konum verileri](sql-query-geospatial-intro.md) hakkında daha fazla bilgi edinin