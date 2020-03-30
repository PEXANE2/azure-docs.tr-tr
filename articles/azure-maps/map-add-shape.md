---
title: Haritaya çokgen katmanı ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'daki bir haritada çokgen ve çok çokgen oluşturmayı öğreneceksiniz.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9191f054ca3c7374bcbc7bec46573289a512612c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535061"
---
# <a name="add-a-polygon-layer-to-the-map"></a>Haritaya çokgen katmanı ekleme

Bu makalede, çokgen bir `Polygon` katman `MultiPolygon` kullanarak haritadaki geometrilerin alanlarını ve özelliklerini nasıl işleyip işleyip işleyip işlediniz. Azure Haritalar Web SDK, [genişletilmiş GeoJSON şemasında](extend-geojson.md#circle)tanımlandığı şekilde Circle geometrilerinin oluşturulmasını da destekler. Bu daireler haritada işlendiğinde çokgenlere dönüştürülür. Tüm özellik geometrileri atlas ile sarılı raşitildiğinde kolayca [güncellenebilir. Şekil](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıfı.

## <a name="use-a-polygon-layer"></a>Çokgen katmanı kullanma 

Çokgen bir katman bir veri kaynağına bağlandığında ve haritaya yüklendiğinde, `MultiPolygon` alanı ve özellikleri yle birlikte `Polygon` olur. Çokgen oluşturmak için, bir veri kaynağına ekleyin ve [Çokgen Katman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) sınıfını kullanarak çokgen katmanıyla oluşturun.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a rectangular polygon.
dataSource.add(new atlas.data.Feature(
    new atlas.data.Polygon([[
        [-73.98235, 40.76799],
        [-73.95785, 40.80044],
        [-73.94928, 40.7968],
        [-73.97317, 40.76437],
        [-73.98235, 40.76799]
    ]])
));

//Create and add a polygon layer to render the polygon to the map, below the label layer.
map.layers.add(new atlas.layer.PolygonLayer(dataSource, null,{
    fillColor: 'red',
    fillOpacity: 0.7
}), 'labels');
```

Aşağıda yukarıdaki kodun tam ve çalışan örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Haritaya çokgen ekleme ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Kalem <a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>bir haritaya çokgen ekleyin.</a>
</iframe>

## <a name="use-a-polygon-and-line-layer-together"></a>Çokgen ve çizgi katmanını birlikte kullanma

Çokgenlerin anahatlarını işlemek için bir çizgi katmanı kullanılır. Aşağıdaki kod örneği önceki örnekgibi bir çokgen işler, ancak şimdi bir satır katmanı ekler. Bu satır katmanı veri kaynağına bağlı ikinci bir katmandır.  

<iframe height='500' scrolling='no' title='Çokgen ve çizgi tabakası çokgen eklemek için' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre çokgen eklemek<a href='https://codepen.io/azuremaps'>@azuremaps</a>için Kalem <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Çokgen ve satır katmanına</a> bakın .
</iframe>

## <a name="fill-a-polygon-with-a-pattern"></a>Çokgeni desenle doldurun

Çokgeni bir renkle doldurmanın yanı sıra, çokgeni doldurmak için bir görüntü deseni kullanabilirsiniz. Haritalar görüntüsü neşrin kaynaklarına bir görüntü deseni `fillPattern` yükleyin ve sonra bu görüntüyü çokgen katmanının özelliğiyle başvurun.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen dolgu deseni" src="//codepen.io/azuremaps/embed/JzQpYX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/JzQpYX/'>Çoklu dolgu desenine</a> bakın .
</iframe>


> [!TIP]
> Azure Haritalar web SDK, dolgu deseni olarak kullanabileceğiniz birkaç özelleştirilebilir resim şablonu sağlar. Daha fazla bilgi için resim şablonları belgesini [nasıl kullanılır'](how-to-use-image-templates-web-sdk.md) a bakın.

## <a name="customize-a-polygon-layer"></a>Çokgen katmanını özelleştirme

Çokgen katmanı yalnızca birkaç stil seçeneğine sahiptir. Burada bunları denemek için bir araçtır.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> 'e bakın .
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle-to-the-map"></a>Haritaya daire ekleme

Azure Haritalar, [geojson şemasının, burada](extend-geojson.md#circle)belirtildiği gibi çevreler için bir tanım sağlayan genişletilmiş bir sürümünü kullanır. Bir özellik oluşturarak haritaüzerinde bir `Point` daire oluşturulur. Bu `Point` değeri `subType` `"Circle"` olan bir özellik `radius` ve metre yarıçapı temsil eden bir sayı ile bir özellik vardır. 

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

Azure Haritalar Web SDK `Point` bu `Polygon` özellikleri özelliklere dönüştürür. Daha sonra, bu özellikler haritada aşağıdaki kod örneğinde gösterildiği gibi çokgen ve çizgi katmanları kullanılarak işlenir.

<br/>

<iframe height='500' scrolling='no' title='Haritaya daire ekleme' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Kalem <a href='https://codepen.io'>CodePen'de</a>Azure Haritalar<a href='https://codepen.io/azuremaps'>@azuremaps</a>() ile <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>haritaya daire ekleyin.</a>
</iframe>

## <a name="make-a-geometry-easy-to-update"></a>Geometriyi güncellemeyi kolaylaştırın

Bir `Shape` sınıf bir [Geometri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) veya [Özellik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) sarar ve bu özellikleri güncelleştirmeyi ve korumak kolaylaştırır. Bir şekil değişkenini anında ayarlamak için, bir geometriyi veya bir dizi özelliği şekil oluşturucuya geçirin.

```javascript
//Creating a shape by passing in a geometry and a object containing properties.
var shape1 = new atlas.Shape(new atlas.data.Point[0,0], { myProperty: 1 });

//Creating a shape using a feature.
var shape2 = new atlas.Shape(new atlas.data.Feature(new atlas.data.Point[0,0], { myProperty: 1 });
```

Aşağıdaki kod örneği, bir şekil sınıfıyla bir daire GeoJSON nesnesinin nasıl kaydırıldığını gösterir. Yarıçapın değeri şekil değiştikçe, daire haritada otomatik olarak işlenir.

<br/>

<iframe height='500' scrolling='no' title='Şekil özelliklerini güncelleştirme' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>Güncelleştirme şekil özelliklerine</a> bakın .
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [ÇokgenKatman](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [ÇokgenLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza ekleyeceğiniz daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Açılır pencere ekleme](map-add-popup.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Görüntü şablonlarını kullanma](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](map-add-line-layer.md)

Ek kaynaklar:

> [!div class="nextstepaction"]
> [Azure Haritalar GeoJSON belirtimi uzantısı](extend-geojson.md#circle)