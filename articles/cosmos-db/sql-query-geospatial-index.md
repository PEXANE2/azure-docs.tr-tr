---
title: Azure Cosmos DB Jeo uzamsal verileri dizinle
description: Uzamsal verileri Azure Cosmos DB ile dizinle
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: eb0a2b2778b3217e185b9883def6eaa54674cc5b
ms.sourcegitcommit: 05a650752e9346b9836fe3ba275181369bd94cf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79137912"
---
# <a name="index-geospatial-data-with-azure-cosmos-db"></a>Azure Cosmos DB Jeo uzamsal verileri dizinle

Azure Cosmos DB veritabanı altyapısını gerçekten şema belirsiz olacak şekilde tasarlıyoruz ve JSON için birinci sınıf desteğini sunduk. Azure Cosmos DB için en iyi duruma getirilmiş yazma veritabanı altyapısı, GeoJSON standardında temsil edilen uzamsal verileri yerel olarak anlamıştır.

Bir Nutshell 'de geometri, coğrafi olmayan koordinatlardan 2B düzlemin üzerine yansıtıldıysa, daha sonra **quadtree**kullanılarak hücrelere aşamalı olarak bölünür. Bu hücreler, noktaların yerini koruyan bir **Tepbert alanı doldurma eğrisi**içindeki hücrenin konumuna göre 1G ile eşleştirilir. Ayrıca, konum verileri dizinlendiğinde, **mozaik döşeme**olarak bilinen bir işlemden geçer, diğer bir deyişle, bir konumdan kesişen tüm hücreler Azure Cosmos DB dizininde anahtar olarak tanımlanır ve saklanır. Sorgu zamanında noktaları ve Çokgenleri gibi bağımsız değişkenleri ayrıca ilgili hücre kimliği aralıkları ayıklamak için grubun Mozaik ve dizinden veri almak için kullanılır.

/* (Tüm yollar) için uzamsal dizin içeren bir dizin oluşturma ilkesi belirtirseniz, kapsayıcıda bulunan tüm veriler etkili uzamsal sorgular için dizinlenir.

> [!NOTE]
> Azure Cosmos DB noktaların, LineStrings, çokgenler ve MultiPolygon dizinlemesini destekler
>
>

## <a name="modifying-geospatial-data-type"></a>Jeo-uzamsal veri türünü değiştirme

Kapsayıcıda, `geospatialConfig` Jeo-uzamsal verilerin nasıl dizine alınacağını belirtir. Kapsayıcı başına bir `geospatialConfig` belirtmelisiniz: Coğrafya veya geometri. Belirtilmemişse, `geospatialConfig` Coğrafya veri türü için varsayılan değer olarak kullanılır. `geospatialConfig`değiştirdiğinizde, kapsayıcıdaki tüm mevcut Jeo uzamsal verilerin yeniden dizinlenmesini sağlayacak.

> [!NOTE]
> Azure Cosmos DB Şu anda .NET SDK içindeki geospatialConfig üzerinde yapılan değişiklikleri yalnızca 3,6 ve üzeri sürümlerde desteklemektedir.
>

Aşağıda, `geospatialConfig` özelliğini ayarlayarak ve bir **BoundingBox**ekleyerek Jeo-uzamsal veri türünü `geometry` olarak değiştirme örneği verilmiştir:

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

## <a name="geography-data-indexing-examples"></a>Coğrafya veri dizin oluşturma örnekleri

Aşağıdaki JSON kod parçacığında **Coğrafya** veri türü için etkin bir dizin oluşturma ilkesi gösterilmiştir. Coğrafya veri türü ile uzamsal veriler için geçerlidir ve uzamsal sorgulama için belgeler içinde bulunan coğrafi JSON noktası, Çokgen, MultiPolygon veya LineString dizimiyle dizin oluşturulur. Azure portal kullanarak dizin oluşturma ilkesini değiştiriyorsanız, kapsayıcıda uzamsal dizin oluşturmayı etkinleştirmek üzere dizin oluşturma ilkesi için aşağıdaki JSON 'u belirtebilirsiniz:

**Coğrafya uzamsal dizin oluşturma ile kapsayıcı dizin oluşturma ilkesi JSON**

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
> ' % S'konum GeoJSON değeri belge içinde hatalı biçimlendirilmiş veya geçersiz ise, ardından uzamsal sorgulama için dizine değil. Konum değerleri ST_ISVALID ve ST_ISVALIDDETAILED kullanarak doğrulayabilirsiniz.

[Dizin oluşturma Ilkesini](how-to-manage-indexing-policy.md) Azure CLI, PowerShell veya HERHANGI bir SDK kullanarak da değiştirebilirsiniz.

## <a name="geometry-data-indexing-examples"></a>Geometri veri dizin oluşturma örnekleri

Coğrafya veri türü ile benzer olan **geometri** veri türü ile, dizine eklenecek ilgili yolları ve türleri belirtmeniz gerekir. Ayrıca, belirli bir yol için dizine eklenecek istenen alanı belirtmek için dizin oluşturma ilkesi içinde bir `boundingBox` belirtmeniz gerekir. Her Jeo-uzamsal yol kendi`boundingBox`gerektirir.

Sınırlayıcı kutu aşağıdaki özelliklerden oluşur:

- **XMin**: en düşük dizinli x koordinatı
- **yMin**: en düşük dizinli y koordinatı
- **xmax**: en fazla dizinli x koordinatı
- **YMax**: en fazla dizinli y koordinatı

Geometrik veriler sonsuz olabilecek bir düzlemi kapladığı için sınırlayıcı bir kutu gereklidir. Ancak uzamsal dizinler, sınırlı bir alan gerektirir. **Coğrafya** veri türü Için, Dünya sınırı ve bir sınırlayıcı kutu ayarlamanız gerekmez.

Verilerinizin tümünü (veya çoğunu) içeren bir sınırlayıcı kutu oluşturmalısınız. Yalnızca sınırlayıcı kutunun içindeki nesneler üzerinde hesaplanan işlemler, uzamsal dizin kullanabilir. Sorgu performansını olumsuz yönde etkilediği için sınırlayıcı kutuyu gerekenden önemli ölçüde daha büyük hale memelisiniz.

**Geospatialconfig** ile **geometri** verilerinin `geometry`olarak dizinleyen örnek bir dizin oluşturma ilkesi aşağıda verilmiştir:

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

Yukarıdaki dizin oluşturma ilkesinde x koordinatları için (-10, 10) bir **sıçrama Dingbox** ve y koordinatları için (-20, 20) vardır. Yukarıdaki dizin oluşturma ilkesiyle kapsayıcı, tamamen bu bölgedeki tüm noktaları, çokgenler, MultiPolygon ve LineStrings dizinini dizinleyecek.

> [!NOTE]
> `geography` veri türü olan bir kapsayıcıya **sıçrama Dingbox** ile bir dizin oluşturma ilkesi eklemeye çalışırsanız, başarısız olur. Bir **sıçrama kutusu**eklemeden önce kapsayıcının **Geospatialconfig** öğesini `geometry` olarak değiştirmelisiniz. Veri ekleyebilir ve dizin oluşturma ilkenizin geri kalanını (yollar ve türler gibi), kapsayıcının Jeo uzamsal veri türünü seçmeden önce ya da sonra değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de Jeo-uzamsal destek kullanmaya başlamak öğrendiniz, sonra şunları yapabilirsiniz:

* [Azure Cosmos db sorgu](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB uzamsal verileri sorgulama](sql-query-geospatial-query.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Jeo-uzamsal ve coğrafi JSON konumu verileri](sql-query-geospatial-intro.md) hakkında daha fazla bilgi edinin