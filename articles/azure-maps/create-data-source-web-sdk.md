---
title: Harita için veri kaynağı oluşturma | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak bir veri kaynağı oluşturmayı ve haritaya nasıl ekleyeceğinizi öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: codepen
ms.openlocfilehash: 1675d63fd3a65beda46042f4a78535bb4e066e62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190226"
---
# <a name="create-a-data-source"></a>Veri kaynağı oluşturma

Azure Haritalar Web SDK verileri veri kaynaklarında depolar. Veri kaynaklarının kullanılması, sorgulama ve işleme için veri işlemlerini optimize eder. Şu anda iki tür veri kaynağı vardır:

**GeoJSON veri kaynağı**

GeoJSON tabanlı veri kaynağı yükü ve `DataSource` sınıfı kullanarak verileri yerel olarak depolar. GeoJSON [verileri, atlas.data](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data) ad alanındaki yardımcı sınıflar kullanılarak el ile oluşturulabilir veya oluşturulabilir. Sınıf `DataSource` yerel veya uzak GeoJSON dosyalarını almak için işlevler sağlar. Uzak GeoJSON dosyaları CORs etkin bir bitiş noktasında barındırılmalıdır. Sınıf, `DataSource` nokta verilerini kümeleme için işlevsellik sağlar. Ayrıca, veriler `DataSource` sınıfla kolayca eklenebilir, kaldırılabilir ve güncellenebilir.


> [!TIP]
> Tüm verilerin üzerine yazmak istediğinizi `DataSource`varsaalım. `clear` O zaman `add` işlevleri ne zaman aramaları yaparsanız, harita iki kez yeniden işleyebilir ve bu da biraz gecikmeye neden olabilir. Bunun yerine, veri kaynağındaki `setShapes` tüm verileri kaldıracak ve değiştirecek ve yalnızca haritanın tek bir yeniden işlemesini tetikleyen işlevi kullanın.

**Vektör döşeme kaynağı**

Vektör döşemesi kaynağı, vektör döşeme katmanına nasıl erişilir açıklar. Vektör döşeme kaynağını anlık olarak kullanmak için [VectorTileSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource) sınıfını kullanın. Vektör döşeme katmanları döşeme katmanlarına benzer, ancak aynı değildir. Çini tabakası bir raster görüntüdür. Vektör döşeme katmanları PBF formatında sıkıştırılmış bir dosyadır. Bu sıkıştırılmış dosya vektör eşlemi verileri ve bir veya daha fazla katman içerir. Dosya, her katmanın stiline bağlı olarak istemcide işlenebilir ve şekillendirilebilir. Vektör döşemesindeki veriler, noktalar, çizgiler ve çokgenler şeklinde coğrafi özellikler içerir. Raster kiremit katmanları yerine vektör kiremit katmanları kullanmanın çeşitli avantajları vardır:

 - Vektör döşemesinin dosya boyutu genellikle eşdeğer bir raster döşemesinden çok daha küçüktür. Bu nedenle, daha az bant genişliği kullanılır. Daha düşük gecikme, daha hızlı bir harita ve daha iyi bir kullanıcı deneyimi anlamına gelir.
 - İstemci üzerinde vektör döşemeleri işlendiğinden, görüntülenmekte oldukları aygıtın çözünürlüğüne uyum sağlarlar. Sonuç olarak, işlenen haritalar kristal berraklığında etiketlerle daha iyi tanımlanmış görünür.
 - Yeni stil istemciye uygulanabileceğinden, vektör eşlemlerinde verilerin stilini değiştirmek için verilerin yeniden indirilmesi gerekmez. Buna karşılık, bir raster döşeme katmanı stilini değiştirme genellikle sunucudan fayans yükleme sonra yeni stil uygulayarak gerektirir.
 - Veriler vektör biçiminde teslim olduğundan, verileri hazırlamak için daha az sunucu tarafı işleme sevesi gerekir. Sonuç olarak, yeni veriler daha hızlı kullanılabilir hale getirilebilir.

Vektör kaynağı kullanan tüm katmanlar `sourceLayer` bir değer belirtmelidir.

Oluşturulduktan sonra, veri kaynakları bir `map.sources` [SourceManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.sourcemanager)olan özellik üzerinden haritaya eklenebilir. Aşağıdaki kod, bir şeyin `DataSource` nasıl oluşturulup eşe nasıl ekleyeceğini gösterir.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);
```

Azure Haritalar, açık bir standart olan [Mapbox Vektör Döşeme Belirtimine](https://github.com/mapbox/vector-tile-spec)bağlıdır.

## <a name="connecting-a-data-source-to-a-layer"></a>Bir veri kaynağını katmana bağlama

Veriler, oluşturma katmanları kullanılarak haritaüzerinde işlenir. Tek bir veri kaynağı, bir veya daha fazla görüntüleme katmanı tarafından başvurulabilir. Aşağıdaki işleme katmanları bir veri kaynağı gerektirir:

- [Kabarcık katmanı](map-add-bubble-layer.md) - haritaüzerinde ölçeklenmiş daireler olarak nokta verileri işler.
- [Sembol katmanı](map-add-pin.md) - nokta verilerini simge veya metin olarak işler.
- [Isı haritası katmanı](map-add-heat-map-layer.md) - nokta verilerini yoğunluk ısı haritası olarak işler.
- [Satır katmanı](map-add-shape.md) - bir çizgi işlemek ve çokgenlerin anahat işlemek. 
- [Çokgen katmanı - çokgenin](map-add-shape.md) alanını düz bir renk veya görüntü deseniyle doldurur.

Aşağıdaki kod, bir veri kaynağının nasıl oluşturulup haritaya ekleyeceğini ve bir kabarcık katmanına nasıl bağlanılabildiğini gösterir. Ve sonra, Uzak bir konumdan veri kaynağına GeoJSON nokta verilerini aktarın. 

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a layer that defines how to render points in the data source and add it to the map.
map.layers.add(new atlas.layer.BubbleLayer(datasource));

//Load the earthquake data.
datasource.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/significant_month.geojson');
```

Bu veri kaynaklarına bağlanmayan ek işleme katmanları vardır, ancak verileri işlemek için doğrudan yüklerler. 

- [Resim katmanı](map-add-image-layer.md) - haritanın üstüne tek bir görüntüyü bindirmeve köşelerini belirtilen koordinatlar kümesine bağlar.
- [Döşeme katmanı](map-add-tile-layer.md) - haritanın üstüne bir raster kiremit katmanı üst üste bindirer.

## <a name="one-data-source-with-multiple-layers"></a>Birden çok katmanı olan bir veri kaynağı

Birden çok katman tek bir veri kaynağına bağlanabilir. Bu seçeneğin yararlı olduğu birçok farklı senaryo vardır. Örneğin, kullanıcının çokgen çizdiği senaryoyu göz önünde bulundurun. Kullanıcı haritaya puan eklerken çokgen alanını oluşturmalı ve doldurmalıyız. Çokgenin anahatlarını ortaya çıkarmak için stilde bir çizgi eklemek, kullanıcı çizerken çokgenin kenarlarını daha kolay görmesini sağlar. Çokgendeki tek bir konumu rahatlıkla yönetmek için, her pozisyonun üzerine bir iğne veya işaretçi gibi bir tutamaç ekleyebiliriz.

![Tek bir veri kaynağından veri işleyen birden çok katmanı gösteren harita](media/create-data-source-web-sdk/multiple-layers-one-datasource.png)

Çoğu eşleme platformunda, çokgen nesnesi, bir çizgi nesnesi ve çokgendeki her konum için bir iğne gerekir. Çokgen değiştirildiğinde, hızla karmaşık hale gelebilen satırı ve pimleri el ile güncelleştirmeniz gerekir.

Azure Haritalar'da tek ihtiyacınız olan, aşağıdaki kodda gösterildiği gibi bir veri kaynağında tek bir çokgendir.

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
> [Datasource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [DataSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VektörTileKaynak](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VektörTileSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.vectortilesourceoptions?view=azure-maps-typescript-latest)

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