---
title: Haritaya Çokgen kalıp ekleme | Microsoft Azure haritaları
description: Microsoft Azure Maps web SDK 'sına Çokgen bir katman ekleme.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 36914240caf3c1321dfa0102bd87cb29173f8b1d
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911063"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map"></a>Haritaya Çokgen kalıp ekleme

Bu makalede, `Polygon` ve `MultiPolygon` Özellik geometrileri, haritada yükseltilmiş şekiller olarak oluşturmak için Çokgen kalıp katmanını nasıl kullanabileceğiniz gösterilmektedir. Azure Haritalar Web SDK 'Sı Ayrıca, [genişletilmiş GeoJSON şemasında](extend-geojson.md#circle)tanımlandığı şekilde daire geometrileri oluşturulmasını destekler. Bu daireler haritada işlendiğinde çokgenler halinde dönüştürülür. Tüm özellik geometrileri, Atlas ile sarmalandıktan sonra da kolayca güncelleştirilebilen olabilir [. Şekil](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıfı.


## <a name="use-a-polygon-extrusion-layer"></a>Çokgen katman kullan

Bir [Çokgen kalıp katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest) veri kaynağına bağlıyken ve haritada yüklendiğinde, bir `Polygon` ve `MultiPolygon` özelliklerinin özelliklerini yükseltilmiş şekiller olarak işler. Çokgen katman katmanının `height` ve `base` özellikleri, temel mesafeyi **Ölçü**cinsinden yükseltilmiş şeklin taban ve yüksekliğinden tanımlar. Aşağıdaki kod, bir çokgen oluşturmayı, bir veri kaynağına eklemeyi ve Çokgen katman sınıfını kullanarak nasıl işleneceğini gösterir.

> [!Note]
> Çokgen katman katmanında tanımlanan `base` değeri `height`eşit veya ondan küçük olmalıdır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Yükseltilmiş Çokgen" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>yükseltilmiş Çokgen</a> 'e bakın.</iframe>


## <a name="add-data-driven-multipolygons"></a>Veri temelli MultiPolygon ekleme

Bir choroptath eşlemesi, `height` ve `fillColor` özelliklerinin `Polygon` ve `MultiPolygon` Özellik geometrileri tarafından istatistiksel değişken ölçüyle orantılı olarak ayarlanarak Çokgen katman katmanı kullanılarak oluşturulabilir. Aşağıdaki kod örneğinde, durum ile popülasyon yoğunluğu ölçüsünün temelinde U. S ' nin yükseltilmiş bir choroptath Haritası gösterilmektedir.

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

Azure Haritalar Web SDK 'Sı bu `Point` özelliklerini, aşağıdaki kod örneğinde gösterildiği gibi, çokgen tarafından kullanılan `Polygon` özelliklere dönüştürür ve haritada oluşturulabilir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Drone hava sahası Çokgen" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) için kalem <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>kuruta hava sahası poligonu</a> görüntüleyin.
</iframe>

## <a name="customize-a-polygon-extrusion-layer"></a>Çokgen kalıp katmanını özelleştirme

Çokgen katman birçok stil seçeneğini katmana kat. İşte deneyebileceğiniz bir araç.

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
