---
title: Azure haritalar ile şekil ekleme | Microsoft Docs
description: JavaScript eşlemesine şekil ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 377cfbf93e45872e34242f9cd2432195b6ba6f92
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68849111"
---
# <a name="add-a-shape-to-a-map"></a>Haritaya şekil ekleme

Bu makalede çizgi ve çokgen katmanları kullanılarak haritada geometrilerin nasıl oluşturulduğu gösterilir. Azure Haritalar Web SDK 'Sı Ayrıca, [genişletilmiş GeoJSON şemasında](extend-geojson.md#circle)tanımlandığı şekilde daire geometrileri oluşturulmasını destekler. [Şekil](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıfıyla sarıldıysanız tüm özellik geometrileri de kolayca güncelleştirilemeyebilir.

<a id="addALine"></a>

## <a name="add-lines-to-the-map"></a>Haritaya satır ekleme

`LineString`ve `MultiLineString` özellikleri haritadaki yolları ve anahatları temsil etmek için kullanılır.

### <a name="add-a-line"></a>Çizgi ekle

<iframe height='500' scrolling='no' title='Haritaya çizgi ekleme' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/qomaKv/'>bir haritaya çizgi ekleme</a> kalemine bakın.
</iframe>

Yukarıdaki kodda bulunan ilk kod bloğu bir Map nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğunda, [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı kullanılarak bir veri kaynağı nesnesi oluşturulur. Bir [LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest) nesnesi oluşturulup veri kaynağına eklenir.

[Linelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) , [veri kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)Sarmalanan çizgi nesnelerini işler. Son kod bloğu oluşturur ve haritaya bir çizgi katmanı ekler. [Linelayeroptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)'da bir çizgi katmanının özelliklerine bakın. Veri kaynağı ve çizgi katmanı oluşturulur ve eşleme tam olarak yüklendikten sonra çizginin görüntülendiğinden emin olmak için [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) içinde haritaya eklenir.

### <a name="add-symbols-along-a-line"></a>Çizgi üzerinde semboller ekleme

Bu örnek, haritada bir çizgi üzerinde ok simgelerinin nasıl ekleneceğini gösterir. Sembol katmanını kullanırken, "yerleştirme" seçeneğini "satır" olarak ayarlayın, bu, simgeleri satır üzerinde işler ve simgeleri döndürür (0 derece = sağ).

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Oku çizgi üzerinde göster" src="//codepen.io/azuremaps/embed/drBJwX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile birlikte, kalemin <a href='https://codepen.io/azuremaps/pen/drBJwX/'>göster okuna</a> bakın.
</iframe>

> [!TIP]
> Azure Haritalar Web SDK 'Sı, sembol katmanıyla birlikte kullanabileceğiniz çeşitli özelleştirilebilir görüntü şablonları sağlar. Daha fazla bilgi için bkz. [görüntü şablonlarını kullanma](how-to-use-image-templates-web-sdk.md) belgesi.

### <a name="line-stroke-gradient"></a>Çizgiye vuruş gradyanı ekleme

Bir çizgiye tek bir kontur rengi uygulayabilmenin yanı sıra, bir çizgi segmentinden diğerine geçişi göstermek için bir renk degradesi olan bir satırı da doldurabilirsiniz. Örneğin, çizgi degradeleri zaman ve mesafe üzerinde yapılan değişiklikleri veya bağlı bir nesne satırındaki farklı sıcaklıkları temsil etmek için kullanılabilir. Bu özelliği bir satıra uygulamak için veri kaynağı `lineMetrics` seçeneğinin true olarak ayarlanmış olması gerekir ve ardından çizgi `strokeColor` seçeneğine bir renk gradyanı ifadesi geçirilebilir. Vuruş gradyanı ifadesi, hesaplanan satır ölçümlerini ifade `['line-progress']` eden veri ifadesine başvurmalıdır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Kontur degradesi olan çizgi" src="//codepen.io/azuremaps/embed/wZwWJZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/wZwWJZ/'>vuruş degradesi ile kalem hattına</a> bakın.
</iframe>

### <a name="customize-a-line-layer"></a>Çizgi katmanını özelleştirme

Çizgi katmanı çeşitli stil oluşturma seçenekleri. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='Çizgi katmanı seçenekleri' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>çizgisi katmanı seçeneklerine</a> bakın.
</iframe>

<a id="addAPolygon"></a>

## <a name="add-a-polygon-to-the-map"></a>Haritaya Çokgen ekleme

`Polygon`ve `MultiPolygon` özellikler genellikle haritadaki bir alanı temsil etmek için kullanılır. 

### <a name="use-a-polygon-layer"></a>Çokgen katmanı kullanma 

Çokgen katmanı, çokgenin alanını işler. 

<iframe height='500' scrolling='no' title='Haritaya Çokgen ekleme ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>haritaya Çokgen ekleme</a> kalemine bakın.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğunda, [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı kullanılarak bir veri kaynağı nesnesi oluşturulur. Bir koordinat dizisinden bir [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) oluşturulur ve veri kaynağına eklenir. 

Bir [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) , haritadaki veri [kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) kaydırılan verileri işler. Son kod bloğu oluşturur ve haritaya bir çokgen katmanı ekler. [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)adresindeki Çokgen katmanının özelliklerine bakın. Veri kaynağı ve çokgen katmanı, eşleme tam olarak yüklendikten sonra çokgenin görüntülendiğinden emin olmak için oluşturulur ve [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) içindeki haritaya eklenir.

### <a name="use-a-polygon-and-line-layer-together"></a>Bir çokgen ve çizgi katmanını birlikte kullanma

Bir çokgen ana hattını işlemek için çizgi katmanı kullanılabilir. 

<iframe height='500' scrolling='no' title='Çokgen eklenecek Çokgen ve çizgi katmanı' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Çokgen eklemek için kalem poligonu ve çizgi katmanını</a> inceleyin.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğunda, [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı kullanılarak bir veri kaynağı nesnesi oluşturulur. Bir koordinat dizisinden bir [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) oluşturulur ve veri kaynağına eklenir. 

Bir [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) , haritadaki veri [kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) kaydırılan verileri işler. [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)adresindeki Çokgen katmanının özelliklerine bakın. [Linelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) bir satır dizisidir. [Linelayeroptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)'da bir çizgi katmanının özelliklerine bakın. Üçüncü kod bloğu Çokgen ve çizgi katmanları oluşturur.

Son kod bloğu, haritada Çokgen ve çizgi katmanları ekler. Veri kaynağı ve Katmanlar oluşturulur ve eşleme tam olarak yüklendikten sonra poligonun görüntülendiğinden emin olmak için [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) içindeki haritaya eklenir.

> [!TIP]
> Çizgi katmanları, varsayılan olarak çokgenler koordinatlarını ve bir veri kaynağındaki satırları de işler. Katmanı yalnızca, LineString özelliklerinin, `filter` `['==', ['geometry-type'], 'LineString']` katman özelliğini olarak ayarlayın veya `['any', ['==', ['geometry-type'], 'LineString'], ['==', ['geometry-type'], 'MultiLineString']]` çok kanallı dize özelliklerini de dahil etmek istiyorsanız, bu katmanı sınırlamak için.

### <a name="fill-a-polygon-with-a-pattern"></a>Çokgeni bir desenli doldur

Bir görüntüyü renklerle bir çokgen doldurmanın yanı sıra, bir resim deseninin de kullanılabilir olması. Haritalar görüntüsü Sprite kaynaklarına bir resim kalıbı yükleyin ve ardından bu görüntüye Çokgen katmanının `fillPattern` özelliği ile başvurun.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen dolgusu kalıbı" src="//codepen.io/azuremaps/embed/JzQpYX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/JzQpYX/'>Çokgen dolgusu düzenine</a> bakın.
</iframe>


> [!TIP]
> Azure Haritalar Web SDK 'Sı, Fill desenleri olarak kullanabileceğiniz çeşitli özelleştirilebilir görüntü şablonları sağlar. Daha fazla bilgi için bkz. [görüntü şablonlarını kullanma](how-to-use-image-templates-web-sdk.md) belgesi.

### <a name="customize-a-polygon-layer"></a>Çokgen katmanını özelleştirme

Çokgen katmanın yalnızca birkaç stil seçeneği vardır. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da bkz. Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() ile kalem <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> .
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle-to-the-map"></a>Haritaya daire ekleme

Azure Maps, coğrafi olarak gösterildiği gibi daireler için bir tanım sağlayan GeoJSON şemasının genişletilmiş bir sürümünü kullanır [](extend-geojson.md#circle). Bir daire `Point` , değerine sahip bir `subType` `"Circle"` özelliği ve `radius` ölçü cinsinden yarıçapı temsil eden bir sayı olan bir özelliği oluşturarak haritada oluşturulabilir. Örneğin:

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}  
```

Azure Haritalar Web SDK 'sı, bu `Point` `Polygon` özellikleri kapakların altında özelliklerine dönüştürür ve burada gösterildiği gibi Çokgen ve çizgi katmanları kullanılarak haritada oluşturulabilir.

<iframe height='500' scrolling='no' title='Haritaya daire ekleme' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>haritaya bir daire ekleme</a> kalemine bakın.
</iframe>

Yukarıdaki kodda bulunan ilk kod bloğu bir Map nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğunda, [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı kullanılarak bir veri kaynağı nesnesi oluşturulur. Daire bir [nokta](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) `subType` [özelliğidir](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) ve bir özelliğine `"Circle"` `radius` sahiptir ve ölçüm içindeki bir özellik değeri. `subType` Bir`"Circle"` Point özelliği bir veri kaynağına eklendiğinde, harita içindeki dairesel bir çokgen içine dönüştürülür.

Bir [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) , haritadaki veri [kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) kaydırılan verileri işler. Son kod bloğu oluşturur ve haritaya bir çokgen katmanı ekler. [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)adresindeki Çokgen katmanının özelliklerine bakın. Veri kaynağı ve çokgen katmanı, eşleme tam olarak yüklendikten sonra dairenin görüntülendiğinden emin olmak için oluşturulur ve [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) içindeki haritaya eklenir.

## <a name="make-a-geometry-easy-to-update"></a>Bir geometriyi güncellemenin kolay olmasını sağlama

Bir `Shape` sınıf bir [geometriyi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) veya [özelliği](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) sarmalayan ve bunların güncelleştirilmesini ve bakımını kolaylaştırır.
`new Shape(data: Feature<data.Geometry, any>)`bir şekil nesnesi oluşturur ve belirtilen özellik ile başlatır.

<br/>

<iframe height='500' scrolling='no' title='Şekil özelliklerini güncelleştir' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>güncelleştirme Şekil özelliklerine</a> bakın.
</iframe>

Yukarıdaki ilk kod bloğu bir harita nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

Nokta, sınıfının nokta [özelliğidir](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) . [](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) İkinci kod bloğu, HTML kaydırıcı öğesi için yarıçap değerini başlatır ve ardından bir [Şekil](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıfı nesnesinde bir nokta nesnesi oluşturur ve kaydırır.

Üçüncü kod bloğu, HTML aralığı kaydırıcı öğesinden değeri alan bir işlev oluşturur ve şekil sınıfı [AddProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) metodunu kullanarak yarıçap değerini değiştirir.

Dördüncü kod bloğunda veri kaynağı nesnesi, [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfı kullanılarak oluşturulur. Daha sonra bu nokta veri kaynağına eklenir.

Bir [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) , haritadaki veri [kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) kaydırılan verileri işler. Üçüncü kod bloğu bir Çokgen katman oluşturur. [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)adresindeki Çokgen katmanının özelliklerine bakın. Veri kaynağı, tıklama olayı işleyicisi ve çokgen katmanı, eşleme tam olarak yüklendikten sonra noktanın görüntülendiğinden emin olmak için oluşturulur ve [olay işleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) içindeki haritaya eklenir.

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklenecek daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Görüntü şablonlarını kullanma](how-to-use-image-templates-web-sdk.md)