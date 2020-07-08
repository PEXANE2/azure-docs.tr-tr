---
title: Harita için veri kaynağı oluşturma | Microsoft Azure haritaları
description: Bu makalede, bir veri kaynağı oluşturmayı ve Microsoft Azure Maps web SDK 'sını kullanarak bir haritaya eklemeyi öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: codepen
ms.openlocfilehash: 7c23e659463364c5e1a497ead138abb4c696627a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85207507"
---
# <a name="create-a-data-source"></a>Veri kaynağı oluşturma

Azure Haritalar Web SDK 'Sı verileri veri kaynaklarında depolar. Veri kaynaklarını kullanmak, sorgulamak ve işlemek için veri işlemlerini iyileştirir. Şu anda iki tür veri kaynağı vardır:

**GeoJSON veri kaynağı**

Coğrafi JSON tabanlı veri kaynağı, sınıfını kullanarak verileri yerel olarak yükler ve depolar `DataSource` . GeoJSON verileri, [Atlas. Data](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data) ad alanındaki yardımcı sınıflar kullanılarak el ile oluşturulabilir veya oluşturulabilir. `DataSource`Sınıfı, yerel veya uzak coğrafi JSON dosyalarını içeri aktarmak için işlevler sağlar. Uzak GeoJSON dosyaları CORs etkin bir uç noktada barındırılmalıdır. `DataSource`Sınıfı, kümeleme noktası verileri için işlevsellik sağlar. Ve, veriler kolayca eklenebilir, kaldırılabilir ve `DataSource` sınıfla güncellenir. Aşağıdaki kod, coğrafi JSON verilerinin Azure haritalar 'da nasıl oluşturulagösterdiğini gösterir.

```Javascript
//Create raw GeoJSON object.
var rawGeoJson = {
     "type": "Feature",
     "geometry": {
         "type": "Point",
         "coordinates": [-100, 45]
     },
     "properties": {
         "custom-property": "value"
     }
};

//Create GeoJSON using helper classes (less error prone).
var geoJsonClass = new atlas.data.Feature(new atlas.data.Point([-100, 45]), {
    "custom-property": "value"
}); 
```

Oluşturulduktan sonra veri kaynakları, `map.sources` bir [sourcemanager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.sourcemanager)olan özelliği aracılığıyla haritaya eklenebilir. Aşağıdaki kod, oluşturma ve eşlemeye ekleme işlemlerinin nasıl yapılacağını gösterir `DataSource` .

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);
```

Aşağıdaki kod, GeoJSON verilerinin bir öğesine eklenebilme yollarını gösterir `DataSource` .

```Javascript
//GeoJsonData in the following code can be a single or array of GeoJSON features or geometries, a GeoJSON feature colleciton, or a single or array of atlas.Shape objects.

//Add geoJSON object to data source. 
dataSource.add(geoJsonData);

//Load geoJSON data from URL. URL should be on a CORs enabled endpoint.
dataSource.importDataFromUrl(geoJsonUrl);

//Overwrite all data in data source.
dataSource.setShapes(geoJsonData);
```

> [!TIP]
> , Bir içindeki tüm verilerin üzerine yazmak istediğinizi varsayalım `DataSource` . `clear`Then işlevlerine çağrılar yaparsanız `add` , eşleme iki kez yeniden işleyebilir ve bu da gecikmeye neden olabilir. Bunun yerine, `setShapes` veri kaynağındaki tüm verileri kaldıracak ve değiştirecek ve yalnızca haritanın tek bir yeniden işlemesini tetikleyeceği işlevini kullanın.

**Vektör kutucuk kaynağı**

Vektör kutucuk kaynağı bir vektör kutucuk katmanına nasıl erişebileceğinizi açıklar. Vektör kutucuk kaynağı oluşturmak için [Vectortilesource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource) sınıfını kullanın. Vektör döşeme katmanları döşeme katmanlarına benzerdir, ancak aynı değildir. Döşeme katmanı bir raster görüntüsüdür. Vektör döşeme katmanları, PBF biçiminde sıkıştırılmış bir dosyadır. Bu sıkıştırılmış dosya, vektör eşleme verilerini ve bir veya daha fazla katmanı içerir. Dosya, her katmanın stiline bağlı olarak işlenilerek, istemci üzerinde stil oluşturulabilir. Vektör kutucuğunda bulunan veriler, işaret, çizgi ve çokgenler biçimindeki coğrafi özellikler içerir. Raster döşeme katmanları yerine vektör kutucuğu katmanlarını kullanmanın çeşitli avantajları vardır:

 - Vektör kutucuğunun dosya boyutu genellikle denk bir raster kutucuğundan çok daha küçüktür. Bu nedenle, daha az bant genişliği kullanılır. Daha düşük gecikme süresi, daha hızlı bir harita ve daha iyi bir kullanıcı deneyimi anlamına gelir.
 - Vektör kutucukları istemcide işlendiği için, üzerinde görüntülendikleri cihazın çözünürlüğüne uyarlarlar. Sonuç olarak, işlenen haritalar Crystal Clear etiketleriyle daha iyi tanımlanmış şekilde görünür.
 - Yeni stil istemciye uygulanamadığından, vektör eşlemlerdeki verilerin stilini değiştirmek, verilerin yeniden indirilmesini gerektirmez. Buna karşılık, bir raster kutucuk katmanının stilini değiştirmenin genellikle sunucudan kutucukları yükleme ve ardından yeni stili uygulama gerekir.
 - Veriler vektör biçiminde teslim edildiğinden, verileri hazırlamak için daha az sunucu tarafı işleme gerekir. Sonuç olarak, daha yeni veriler daha hızlı kullanılabilir hale getirilebilir.

Vektör kaynağı kullanan tüm katmanların bir değer belirtmesi gerekir `sourceLayer` .

Azure haritalar, açık bir standart olan [Mapbox vektör kutucuk belirtimine](https://github.com/mapbox/vector-tile-spec)uyar.

## <a name="connecting-a-data-source-to-a-layer"></a>Bir veri kaynağını katmana bağlama

Veriler, işleme katmanları kullanılarak haritada işlenir. Tek bir veri kaynağına, bir veya daha fazla işleme katmanı tarafından başvurulabilir. Aşağıdaki işleme katmanları bir veri kaynağı gerektirir:

- [Kabarcık katmanı](map-add-bubble-layer.md) -nokta verilerini haritada ölçeklendirilen daireler olarak işler.
- [Sembol katmanı](map-add-pin.md) -nokta verilerini simge veya metin olarak işler.
- [Isı haritası katmanı](map-add-heat-map-layer.md) -nokta verilerini yoğunluk ısı haritası olarak işler.
- [Çizgi katmanı](map-add-shape.md) -bir çizgiyi işleme ve çokgen ana hattını işleme. 
- [Çokgen katmanı](map-add-shape.md) -bir çokgen alanını düz renk veya görüntü düzeniyle doldurur.

Aşağıdaki kod, bir veri kaynağının nasıl oluşturulacağını, haritaya nasıl ekleneceğini ve bir kabarcık katmanına nasıl bağlanacağını gösterir. Ve sonra, coğrafi JSON noktası verilerini uzak bir konumdan veri kaynağına aktarın. 

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a layer that defines how to render points in the data source and add it to the map.
map.layers.add(new atlas.layer.BubbleLayer(datasource));

//Load the earthquake data.
datasource.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/significant_month.geojson');
```

Bu veri kaynaklarına bağlanmayama verileri doğrudan işleme için yükleyen ek işleme katmanları vardır. 

- [Görüntü katmanı](map-add-image-layer.md) -Haritanın üstündeki tek bir görüntünün üzerine biçimlendirmeler ve köşelerini belirtilen koordinat kümesine bağlar.
- [Döşeme katmanı](map-add-tile-layer.md) -haritanın üzerine bir raster döşeme katmanı uygular.

## <a name="one-data-source-with-multiple-layers"></a>Birden çok katmanlı bir veri kaynağı

Birden çok katman tek bir veri kaynağına bağlanabilir. Bu seçeneğin yararlı olduğu birçok farklı senaryo vardır. Örneğin, bir kullanıcının Çokgen çizdiği senaryoyu düşünün. Kullanıcı haritaya işaret eklediğinden Çokgen alanını işlemeli ve doldurmalıdır. Çokgen ana hattına stil eklemek, Kullanıcı çizerken poligonun kenarlarını daha kolay görmenizi sağlar. Poligon tek bir konumu rahat bir şekilde düzenlemek için her bir konumun üzerine bir pin veya işaret gibi bir tanıtıcı ekleyebiliriz.

![Tek bir veri kaynağından birden çok katman işleme verilerini gösteren eşleme](media/create-data-source-web-sdk/multiple-layers-one-datasource.png)

Çoğu eşleme platformunda, poligon her konum için bir çokgen nesne, bir çizgi nesnesi ve bir PIN olması gerekir. Çokgen değiştirildiğinde, satırı ve PIN 'leri el ile güncelleştirmeniz gerekir, bu da hızlı bir şekilde karmaşık hale gelebilir.

Azure haritalar ile, bir veri kaynağında aşağıdaki kodda gösterildiği gibi tek bir çokgen olması yeterlidir.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a polygon and add it to the data source.
dataSource.add(new atlas.data.Polygon([[[/* Coordinates for polygon */]]]));

//Create a polygon layer to render the filled in area of the polygon.
var polygonLayer = new atlas.layer.PolygonLayer(dataSource, 'myPolygonLayer', {
     fillColor: 'rgba(255,165,0,0.2)'
});

//Create a line layer for greater control of rendering the outline of the polygon.
var lineLayer = new atlas.layer.LineLayer(dataSource, 'myLineLayer', {
     color: 'orange',
     width: 2
});

//Create a bubble layer to render the vertices of the polygon as scaled circles.
var bubbleLayer = new atlas.layer.BubbleLayer(dataSource, 'myBubbleLayer', {
     color: 'orange',
     radius: 5,
     outlineColor: 'white',
     outlineWidth: 2
});

//Add all layers to the map.
map.layers.add([polygonLayer, lineLayer, bubbleLayer]);
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [DataSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VectorTileSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VectorTileSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.vectortilesourceoptions?view=azure-maps-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Açılır pencere ekleme](map-add-popup.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](map-add-pin.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](map-add-shape.md)

> [!div class="nextstepaction"]
> [Isı haritası ekleme](map-add-heat-map-layer.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)