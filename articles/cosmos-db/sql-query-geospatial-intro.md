---
title: Azure Cosmos DB'deki coğrafi ve GeoJSON konum verileri
description: Azure Cosmos DB ve SQL API ile uzamsal nesnelerin nasıl oluşturulup oluşturultamasını öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: 59c8b31dcc8594d2cafb2db7832e290b01026f60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367593"
---
# <a name="geospatial-and-geojson-location-data-in-azure-cosmos-db"></a>Azure Cosmos DB'deki coğrafi ve GeoJSON konum verileri

Bu makale, Azure Cosmos DB'deki coğrafi işlevselliğin bir girişidir. Şu anda jeouzamsal verilerin depolanması ve erişilmesi yalnızca Azure Cosmos DB SQL API hesapları tarafından desteklenir. Jeouzamsal indeksleme ile ilgili belgelerimizi okuduktan sonra aşağıdaki soruları yanıtlayabilirsiniz:

* Azure Cosmos DB'de uzamsal verileri nasıl depolarım?
* SQL ve LINQ'da Azure Cosmos DB'deki jeouzamsal verileri nasıl sorgulayabilirim?
* Azure Cosmos DB'de uzamsal dizin oluşturmayı nasıl etkinleştirebilirim veya devre dışı alabiliyorum?

## <a name="introduction-to-spatial-data"></a>Uzamsal verilere giriş

Uzamsal veriler uzaydaki nesnelerin konumunu ve şeklini açıklar. Çoğu uygulamada, bu dünya ve jeouzamsal veri nesneleri karşılık gelir. Uzamsal veriler bir kişinin konumunu, ilgi çekici bir yeri veya bir şehrin veya gölün sınırını temsil etmek için kullanılabilir. Yaygın kullanım örnekleri genellikle yakınlık sorguları içerir, örneğin, "geçerli konumumun yakınındaki tüm kafeleri bulun."

Azure Cosmos DB'nin SQL API'si iki uzamsal veri türünü destekler: **geometri** veri türü ve **coğrafya** veri türü.

- **Geometri** türü Öklid (düz) koordinat sistemindeki verileri temsil eder
- **Coğrafya** türü, yuvarlak dünya koordinat sistemindeki verileri temsil eder.

## <a name="supported-data-types"></a>Desteklenen veri türleri

Azure Cosmos DB, [GeoJSON belirtimi](https://tools.ietf.org/html/rfc7946)kullanılarak temsil edilen coğrafi nokta verilerinin dizinoluşturmasını ve sorgulanmasını destekler. GeoJSON veri yapıları her zaman geçerli JSON nesneleridir, bu nedenle azure cosmos DB kullanılarak özel araçlar veya kitaplıklar olmadan depolanabilir ve sorgulanabilir.

Azure Cosmos DB aşağıdaki uzamsal veri türlerini destekler:

- Nokta
- Linestring
- Çokgen
- Multipolygon

### <a name="points"></a>Noktalar

Bir **Nokta** uzayda tek bir konumu gösterir. Coğrafi verilerde, bir Nokta bir bakkal, bir büfe, bir otomobil veya bir şehrin sokak adresi olabilir tam konumunu temsil eder.  Bir nokta GeoJSON 'da (ve Azure Cosmos DB) koordinat çiftini veya boylam ve enlemini kullanarak temsil edilir.

Burada bir nokta için bir örnek JSON's:

**Azure Cosmos DB'deki Noktalar**

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

Uzamsal veri türleri, konum verilerini içeren bir kullanıcı profili örneğiörneğinde gösterildiği gibi bir Azure Cosmos DB belgesine katıştırılabilir:

**Azure Cosmos DB'de Depolanan Konumla Profili Kullanma**

```json
{
    "id":"cosmosdb-profile",
    "screen_name":"@CosmosDB",
    "city":"Redmond",
    "topics":[ "global", "distributed" ],
    "location":{
        "type":"Point",
        "coordinates":[ 31.9, -4.8 ]
    }
}
```

### <a name="points-in-a-geometry-coordinate-system"></a>Geometri koordinat sistemindeki noktalar

**Geometri** veri türü için GeoJSON belirtimi yatay ekseni birinci ve dikey ekseni ikinci olarak belirtir.

### <a name="points-in-a-geography-coordinate-system"></a>Coğrafya koordinat sistemindeki noktalar

**Coğrafya** veri türü için GeoJSON belirtimi boylam birinci ve enlem ikinci belirtir. Diğer haritalama uygulamalarında olduğu gibi, boylam ve enlem açıları ve derece açısından temsil edilir. Boylam değerleri Prime Meridyen'den ölçülür ve -180 derece ile 180,0 derece arasında, enlem değerleri ise ekvatordan ölçülür ve -90.0 derece ile 90.0 derece arasındadır.

Azure Cosmos DB koordinatları WGS-84 başvuru sistemine göre temsil edildiği şekilde yorumlar. Koordinat başvuru sistemleri hakkında daha fazla bilgi için aşağıya bakın.

### <a name="linestrings"></a>Linestrings

**LineStrings** uzayda iki veya daha fazla nokta ve onları bağlayan çizgi segmentleri bir dizi temsil eder. Jeouzamsal verilerde, LineString'ler genellikle otoyolları veya nehirleri temsil etmek için kullanılır.

**GeoJSON'da LineStrings**

```json
    "type":"LineString",
    "coordinates":[ [
        [ 31.8, -5 ],
        [ 31.8, -4.7 ]
    ] ]
```

### <a name="polygons"></a>Çokgen

**Çokgen,** kapalı bir LineString oluşturan bağlı noktaların sınırıdır. Çokgenler genellikle göller veya şehirler ve devletler gibi siyasi yargı gibi doğal oluşumları temsil etmek için kullanılır. Azure Cosmos DB'deki Çokgen örneği aşağıda verilmiştir:

**GeoJSON'da çokgenler**

```json
{
    "type":"Polygon",
    "coordinates":[ [
        [ 31.8, -5 ],
        [ 31.8, -4.7 ],
        [ 32, -4.7 ],
        [ 32, -5 ],
        [ 31.8, -5 ]
    ] ]
}
```

> [!NOTE]
> GeoJSON belirtimi, geçerli Çokgenler için, kapalı bir şekil oluşturmak için sağlanan son koordinat çiftinin ilki ile aynı olmasını gerektirir.
>
> Çokgen içindeki noktalar saat yönünün tersine belirtilmelidir. Saat yönünde belirtilen bir Çokgen, içindeki bölgenin tersini temsil eder.
>
>

### <a name="multipolygons"></a>MultiPolygons

**MultiPolygon** sıfır veya daha fazla Çokgenler bir dizi. **MultiPolygons** taraf örtüşemez veya herhangi bir ortak alana sahip olamaz. Bir veya daha fazla noktada dokunabilirler.

**GeoJSON'da Çok Yönlüler**

```json
{
    "type":"MultiPolygon",
    "coordinates":[[[
        [52.0, 12.0],
        [53.0, 12.0],
        [53.0, 13.0],
        [52.0, 13.0],
        [52.0, 12.0]
        ]],
        [[
        [50.0, 0.0],
        [51.0, 0.0],
        [51.0, 5.0],
        [50.0, 5.0],
        [50.0, 0.0]
        ]]]
}
```

## <a name="coordinate-reference-systems"></a>Koordinat referans sistemleri

Yeryüzünün şekli düzensiz olduğundan, coğrafya jeouzamsal verilerinin koordinatları, her biri kendi referans çerçeveleri ve ölçü birimleri ile birçok koordinat referans sisteminde (CRS) temsil edilmektedir. Örneğin, "National Grid of Britain" bir referans sistemi Birleşik Krallık için doğru, ama dışında değil.

Bugün kullanılan en popüler CRS Dünya Jeodezik Sistemi [WGS-84](https://earth-info.nga.mil/GandG/update/index.php)olduğunu. GPS cihazları ve Google Haritalar ve Bing Haritalar API'ları da dahil olmak üzere birçok haritalama hizmeti WGS-84 kullanır. Azure Cosmos DB, yalnızca WGS-84 CRS kullanarak coğrafya jeouzamsal verilerinin dizinlenmesini ve sorgulanmasını destekler.

## <a name="creating-documents-with-spatial-data"></a>Uzamsal verilerle belge oluşturma
GeoJSON değerlerini içeren belgeler oluşturduğunuzda, bunlar kapsayıcının dizin oluşturma ilkesine uygun olarak otomatik olarak uzamsal dizinle dizine eklenir. Python veya Node.js gibi dinamik olarak yazılan bir dilde bir Azure Cosmos DB SDK ile çalışıyorsanız, geçerli Bir GeoJSON oluşturmanız gerekir.

**Düğüm.js'de Jeouzamsal verilerle belge oluşturma**

```javascript
var userProfileDocument = {
    "id":"cosmosdb",
    "location":{
        "type":"Point",
        "coordinates":[ -122.12, 47.66 ]
    }
};

client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
    // additional code within the callback
});
```

SQL API'leri ile çalışıyorsanız, uygulama `Point`nesnelerinizin `Polygon`yer `MultiPolygon` bilgilerini `Microsoft.Azure.Cosmos.Spatial` yerleştirmek için ad alanı içindeki , ve `LineString`sınıfları kullanabilirsiniz. Bu sınıflar, uzamsal verilerin GeoJSON'a serileştirilmesini ve deserialization'ını basitleştirmeye yardımcı olur.

**.NET'te Jeouzamsal verilerle belge oluşturma**

```csharp
using Microsoft.Azure.Cosmos.Spatial;

public class UserProfile
{
    [JsonProperty("id")]
    public string id { get; set; }

    [JsonProperty("location")]
    public Point Location { get; set; }

    // More properties
}

await container.CreateItemAsync( new UserProfile
    {
        id = "cosmosdb",
        Location = new Point (-122.12, 47.66)
    });
```

Enlem ve boylam bilgileriniz yoksa, ancak şehir veya ülke/bölge gibi fiziksel adresleriniz veya konum adınız varsa, Bing Maps REST Services gibi bir coğrafi kodlama hizmetini kullanarak gerçek koordinatları alabilirsiniz. Bing Haritalar coğrafi kodlama hakkında daha fazla bilgiyi [buradan edinebilirsiniz.](https://msdn.microsoft.com/library/ff701713.aspx)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de coğrafi destekle nasıl başladığınızı öğrendiğiniz için, bir sonraki

* [Azure Cosmos DB Sorgusu](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB ile uzamsal verileri sorgulama](sql-query-geospatial-query.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB ile uzamsal verileri Dizin](sql-query-geospatial-index.md) le ilgili daha fazla bilgi edinin