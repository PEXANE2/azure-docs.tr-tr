---
title: Haritaya Çokgen kalıp ekleme | Microsoft Azure haritaları
description: Microsoft Azure Maps web SDK 'sına Çokgen bir katman ekleme.
author: farah-alyasari
ms.author: v-faalya
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 38a7e6c5f4c139343e735ae82616c11a224ae7ca
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77209690"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map"></a>Haritaya Çokgen kalıp ekleme

Bu makalede, Çokgen katman katmanını kullanarak `Polygon` ve `MultiPolygon` Özellik geometrileri, yükseltilmiş şekiller olarak nasıl işlenir. Azure Haritalar Web SDK 'Sı, [genişletilmiş GeoJSON şemasında](extend-geojson.md#circle)tanımlandığı şekilde daire geometrileri işlemesini destekler. Bu daireler haritada işlendiğinde çokgenlere dönüştürülebilir. Tüm özellik geometrileri, Atlas ile sarmalandıktan sonra kolayca güncelleştirilebilen olabilir [. Şekil](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıfı.

## <a name="use-a-polygon-extrusion-layer"></a>Çokgen katman kullan

[Çokgen kalıp katmanını](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest) bir veri kaynağına bağlayın. Daha sonra, bu dosyayı haritaya yükledi. Çokgen katman katmanı, bir `Polygon` ve `MultiPolygon` özelliklerinin özelliklerini yükseltilmiş şekiller olarak işler. Çokgen katman katmanının `height` ve `base` özellikleri, temel mesafeyi **Ölçü**cinsinden yükseltilmiş şeklin taban ve yüksekliğinden tanımlar. Aşağıdaki kod, bir çokgen oluşturmayı, bir veri kaynağına eklemeyi ve Çokgen katman sınıfını kullanarak nasıl işleneceğini gösterir.

> [!Note]
> Çokgen katman katmanında tanımlanan `base` değeri `height`eşit veya ondan küçük olmalıdır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Yükseltilmiş Çokgen" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>yükseltilmiş Çokgen</a> 'e bakın.</iframe>


## <a name="add-data-driven-polygons"></a>Veri odaklı çokgenler ekleme

Bir choroptath eşlemesi, Çokgen katman kullanılarak oluşturulabilir. Exkalıp katmanının `height` ve `fillColor` özelliklerini `Polygon` ve `MultiPolygon` Özellik geometrileri ' deki istatistiksel değişkenin ölçüsünün ayarlayın. Aşağıdaki kod örneğinde, durum ile popülasyon yoğunluğu ölçüsünün temelinde U. S ' nin yükseltilmiş bir choroptath Haritası gösterilmektedir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Yükseltilmiş choroptath Haritası" src="https://codepen.io/azuremaps/embed/eYYYNox?height=265&theme-id=0&default-tab=result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından gösterilen kalem ile <a href='https://codepen.io/azuremaps/pen/eYYYNox'>Haritayı Yükselt</a> sayfasına bakın.
</iframe>

## <a name="add-a-circle-to-the-map"></a>Haritaya daire ekleme

Azure Maps, coğrafi [olarak gösterildiği gibi](https://docs.microsoft.com/azure/azure-maps/extend-geojson#circle)daireler için bir tanım sağlayan geojson şemasının genişletilmiş bir sürümünü kullanır. Yükseltilmiş bir daire, bir `Circle` `subType` özelliği ile `point` özelliği ve bu yarıçapı **Ölçü**cinsinden temsil eden numaralandırılmış bir `Radius` özelliği oluşturarak haritada oluşturulabilir. Örneğin:

```Javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-105.203135, 39.664087]
    },
    "properties": {
        "subType": "Circle",
        "radius": 1000
    }
} 
```

Azure Haritalar Web SDK 'Sı, bu `Point` özelliklerini, aynı özellikler altındaki `Polygon` özelliklerine dönüştürür. Bu `Point` özellikler, aşağıdaki kod örneğinde gösterildiği gibi Çokgen katman katmanı kullanılarak haritada oluşturulabilir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Drone hava sahası Çokgen" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) için kalem <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>kuruta hava sahası poligonu</a> görüntüleyin.
</iframe>

## <a name="customize-a-polygon-extrusion-layer"></a>Çokgen kalıp katmanını özelleştirme

Çokgen katman katmanı çeşitli stil seçeneklerine sahiptir. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='PoogBRJ' src='//codepen.io/azuremaps/embed/PoogBRJ/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/PoogBRJ/'>poogbrj</a> kalemine bakın.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Poligon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Çokgen kalıp katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest)

Ek kaynaklar:

> [!div class="nextstepaction"]
> [Azure haritalar GeoJSON belirtim uzantısı](extend-geojson.md#circle)
