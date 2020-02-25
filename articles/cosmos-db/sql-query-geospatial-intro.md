---
title: Azure Cosmos DB Jeo uzamsal ve GeoJSON konum verileri
description: Azure Cosmos DB ve SQL API ile uzamsal nesneler oluşturmayı anlayın.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: tisande
ms.openlocfilehash: 0fe83b8e28b96f1d89a7c98cfe86a6e924f1bc49
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77566354"
---
# <a name="geospatial-and-geojson-location-data-in-azure-cosmos-db"></a>Azure Cosmos DB Jeo uzamsal ve GeoJSON konum verileri

Bu makalede, Azure Cosmos DB Jeo-uzamsal işlevler için giriş niteliğindedir. Şu anda Jeo uzamsal verileri depolama ve bunlara erişme yalnızca SQL API hesapları Azure Cosmos DB desteklenir. Jeo-uzamsal dizin oluşturma hakkındaki belgelerimizi okuduktan sonra aşağıdaki soruları cevaplayabilirsiniz:

* Azure Cosmos DB'de nasıl uzamsal veri depoluyor?
* Azure Cosmos DB'de SQL ve LINQ Jeo-uzamsal verileri nasıl sorgulama yapabilirsiniz?
* Nasıl etkinleştirmek veya Azure Cosmos DB'de uzamsal dizin oluşturmayı devre dışı?

## <a name="introduction-to-spatial-data"></a>Uzamsal veri giriş

Uzamsal veri şekli alan nesne ve konumunu açıklar. Çoğu uygulamada bunlar dünya ve Jeo-uzamsal veriler üzerinde nesnelere karşılık gelir. Uzamsal veriler, bir kişi, yer ilgi veya bir şehir veya bir gölü sınırını konumunu temsil etmek için kullanılabilir. Ortak kullanım durumları genellikle yakınlık sorguları, örneğin içeren, "tüm kahve dükkanları bulma geçerli konumunuzu."

Azure Cosmos DB SQL API 'SI **Coğrafya** veri türünü destekler. **Coğrafya** türü, bir yuvarlak dünya koordinat sistemindeki verileri temsil eder.

## <a name="supported-data-types"></a>Desteklenen veri türleri

Azure Cosmos DB [geojson belirtimi](https://tools.ietf.org/html/rfc7946)kullanılarak temsil edilen Jeo uzamsal nokta verilerinin dizinlenmesini ve sorgulanmasını destekler. Depolanabilir ve herhangi bir özel araçları veya kitaplıkları olmadan Azure Cosmos DB kullanarak sorgulanan GeoJSON veri yapıları her zaman geçerli JSON nesnesi olur.

Azure Cosmos DB, aşağıdaki uzamsal veri türlerini destekler:

- Seçeneğinin
- LineString
- Gen
- MultiPolygon

### <a name="points"></a>Noktalar

**Nokta** , alanda tek bir konum gösterir. Jeo-uzamsal verileri, adres Market, bilgi noktası, bir otomobilin ya da bir şehir olabilir tam konumu bir noktayı temsil eder.  Bir noktayı kullanarak kendi koordinat GeoJSON (ve Azure Cosmos DB) çifti veya boylam ve enlem temsil edilir.

Aşağıda nokta için bir JSON örneği verilmiştir:

**Azure Cosmos DB noktaları**

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

Uzamsal veri türleri, konum verilerini içeren bir kullanıcı profili örneğinde gösterildiği gibi bir Azure Cosmos DB belgeye gömülebilir:

**Azure Cosmos DB depolanan konum ile profili kullan**

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

### <a name="points-in-geography-coordinate-system"></a>Coğrafya koordinat sistemindeki noktaları

**Coğrafya** veri türü Için geojson belirtimi, önce boylam ve enlem Second 'u belirtir. Gibi diğer eşleme uygulamalardaki boylam ve enlem açıları olan ve derece cinsinden temsil. Boylam değerleri asal Meridyen ölçülür ve -180 derece ve 180.0 derece arasında ve enlem değerleri ekvatorun ölçülür ve 90.0 derece-90.0 derece arasında.

Azure Cosmos DB koordinatları 84 WGS başvuru sistemi temsil olarak yorumlar. Koordinat başvuru sistemleri hakkında daha fazla ayrıntı için aşağıya bakın.

### <a name="linestrings"></a>LineStrings

**LineStrings** , boşluk içinde iki veya daha fazla noktadan oluşan bir seriyi ve bunları bağlayan çizgi segmentlerini temsil eder. Jeo-uzamsal verileri LineStrings Otoyollar veya rivers temsil etmek için yaygın olarak kullanılır.

**GeoJSON 'da LineStrings**

```json
    "type":"LineString",
    "coordinates":[ [
        [ 31.8, -5 ],
        [ 31.8, -4.7 ]
    ] ]
```

### <a name="polygons"></a>Gen

**Çokgen** , kapalı bir LineString oluşturan bağlantılı noktaların bir sınırıdır. Çokgen lakes gibi doğal formations veya siyasi işletmek zorundayız Şehir ve durumlar gibi temsil etmek için yaygın olarak kullanılır. Azure Cosmos DB bir çokgen örneği aşağıda verilmiştir:

**GeoJSON 'da çokgenler**

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
> GeoJSON belirtimi geçerli çokgenler için sağlanan son koordinat çifti kapalı şekli oluşturmak için birinci ile aynı olması gerekir.
>
> İçinde bir Çokgen noktalarının saat yönünün tersi düzende belirtilmesi gerekir. Bir çokgenin belirtilen saat yönünde sırayla bölgesinde tersini temsil eder.
>
>

### <a name="multipolygons"></a>MultiPolygon

Bir **MultiPolygon** sıfır veya daha fazla poligoluşan bir dizidir. **MultiPolygon** , kenarlarından çakışamaz veya herhangi bir ortak alana sahip olamaz. Bir veya daha fazla noktaya dokunabilir.

**GeoJSON içinde MultiPolygon**

```json
{
    "type":"MultiPolygon",
    "coordinates":[ [
        [52.0, 12.0],
        [53.0, 12.0],
        [53.0, 13.0],
        [52.0, 13.0],
        [52.0, 12.0]
    ],
    [
        [50.0, 0.0],
        [51.0, 0.0],
        [51.0, 5.0],
        [50.0, 5.0],
        [50.0, 0.0]
    ] ]
}
```

## <a name="coordinate-reference-systems"></a>Başvuru koordinat sistemi

Dünya şekli düzensiz olduğundan, coğrafi bölge Jeo uzamsal verilerinin koordinatları, her biri kendi başvuru ve ölçü birimleri ile birlikte çok sayıda koordinat başvuru sisteminde (yukarı) temsil edilir. Örneğin, "ulusal kılavuz, Britanya" başvuru sistemi doğru ise, Birleşik Krallık ancak dışında bu.

Günümüzde kullanımda olan en popüler dünyayı dünya çapındaki sistem [WGS-84](https://earth-info.nga.mil/GandG/update/index.php)' dir. GPS cihazların ve Google Haritalar ve Bing haritaları API'si dahil olmak üzere birçok eşleme hizmetlerin WGS 84 kullanın. Azure Cosmos DB Coğrafya Jeo uzamsal verilerinin dizinlenmesini ve sorgulanmasını destekler ve yalnızca WGS-84 leri kullanılarak.

## <a name="creating-documents-with-spatial-data"></a>Uzamsal veri ile belge oluşturma
GeoJSON değerleri içeren belgeleri oluşturduğunuzda, bunlar olan bir uzamsal dizin kapsayıcısının dizin oluşturma ilkesini anlaşmalara uygun şekilde otomatik olarak dizine eklenir. Bir Azure Cosmos DB SDK, Python veya Node.js gibi dinamik olarak yazılan bir dilde ile çalışıyorsanız, geçerli GeoJSON oluşturmanız gerekir.

**Node. js ' de Jeo-uzamsal verilerle belge oluşturma**

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

SQL API 'Leriyle çalışıyorsanız, uygulama nesnelerinize konum bilgilerini eklemek için `Microsoft.Azure.Cosmos.Spatial` ad alanı içinde `Point`, `LineString`, `Polygon`ve `MultiPolygon` sınıflarını kullanabilirsiniz. Bu sınıfların serileştirme ve seri durumundan çıkarma uzamsal veri GeoJSON içine basitleştirmeye yardımcı olur.

**.NET 'te Jeo-uzamsal verilerle belge oluşturma**

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

Enlem ve Boylam bilgisine sahip değilseniz, ancak şehir veya ülke/bölge gibi fiziksel adreslere veya konuma sahipseniz, Bing Haritalar REST hizmetleri gibi bir coğrafi kodlama hizmeti kullanarak gerçek koordinatları arayabilirsiniz. [Burada](https://msdn.microsoft.com/library/ff701713.aspx)Bing Haritalar coğrafi kodlama hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de Jeo-uzamsal destek kullanmaya başlamak öğrendiniz, sonra şunları yapabilirsiniz:

* [Azure Cosmos db sorgu](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB uzamsal verileri sorgulama](sql-query-geospatial-query.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Ile dizin uzamsal verileri](sql-query-geospatial-index.md) hakkında daha fazla bilgi edinin