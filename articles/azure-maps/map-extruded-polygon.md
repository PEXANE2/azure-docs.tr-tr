---
title: Haritaya çokgen ekstrüzyon katmanı ekleme | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar Web SDK'ya çokgen ekstrüzyon katmanı ekleme.
author: philmea
ms.author: philmea
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7405098bd4924333aafcd1c285eb2f37bb1d4f75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334552"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map"></a>Haritaya çokgen ekstrüzyon katmanı ekleme

Bu makalede, çokgen ekstrüzyon katmanının alanları `Polygon` işlemek `MultiPolygon` ve geometrileri ekstrüde şekiller olarak nasıl kullanacağınızı gösterilmektedir. Azure Haritalar Web SDK genişletilmiş [GeoJSON şemasında](extend-geojson.md#circle)tanımlandığı gibi Circle geometrilerinin işlenmesini destekler. Bu daireler haritada işlendiğinde çokgenlere dönüştürülebilir. Tüm özellik geometrileri atlasla sarılıolduğunda kolayca [güncellenebilir. Şekil](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) sınıfı.

## <a name="use-a-polygon-extrusion-layer"></a>Çokgen ekstrüzyon katmanı kullanma

[Çokgen ekstrüzyon katmanını](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest) bir veri kaynağına bağlayın. Sonra, haritaya yükledi. Çokgen ekstrüzyon katmanı, a `Polygon` ve `MultiPolygon` özelliklerin alanlarını ekstrüzyonlu şekiller olarak işler. Çokgen ekstrüzyon tabakasının `height` özellikleri ve `base` özellikleri, ekstrüzyon şeklinin yerden taban uzaklığı ve yüksekliği metre olarak tanımlar. **meters** Aşağıdaki kod, çokgenin nasıl oluşturulup bir veri kaynağına ekleyeceğini ve Çokgen ekstrüzyon katmanı sınıfını kullanarak nasıl işleyeceğini gösterir.

> [!Note]
> Çokgen ekstrüzyon tabakasında tanımlanan `base` `height`değer, .'ın .

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Ekstrüde poligon" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar tarafından <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>Ekstrüzyon Ekstrüzyon</a> 'a bakın (<a href='https://codepen.io/azuremaps'>@azuremaps</a>)</iframe>


## <a name="add-data-driven-polygons"></a>Veri odaklı çokgenler ekleme

Çokgen ekstrüzyon tabakası kullanılarak bir korekin haritası oluşturulabilir. Ekstrüzyon tabakasının `height` özelliklerini `fillColor` `Polygon` ve özellik `MultiPolygon` geometrileri deki istatistiksel değişkenin ölçümüne ayarlayın. Aşağıdaki kod örneği, nüfus yoğunluğunun eyalete göre ölçülmelerine dayalı olarak ABD'nin ekstrüde bir korektit haritasını gösterir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Ekstrüzyon choropleth haritası" src="https://codepen.io/azuremaps/embed/eYYYNox?height=265&theme-id=0&default-tab=result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar'ın<a href='https://codepen.io/azuremaps'>@azuremaps</a>Kalem <a href='https://codepen.io/azuremaps/pen/eYYYNox'>Ekstrüzyon</a> korecetveline bakın.
</iframe>

## <a name="add-a-circle-to-the-map"></a>Haritaya daire ekleme

Azure Haritalar, GeoJSON şemasının genişletilmiş bir sürümünü kullanır ve [burada](https://docs.microsoft.com/azure/azure-maps/extend-geojson#circle)belirtildiği gibi çevreler için bir tanım sağlar. Ekstrüde bir daire metre yarıçapı temsil `point` eden bir `subType` özellik `Circle` ve numaralanmış `Radius` bir özellik oluşturarak harita üzerinde oluşturulabilir. **meters** Örnek:

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

Azure Haritalar Web SDK `Point` bu `Polygon` özellikleri kaputun altındaki özelliklere dönüştürür. Bu `Point` özellikler, aşağıdaki kod örneğinde gösterildiği gibi çokgen ekstrüzyon katmanı kullanılarak haritaüzerinde işlenebilir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Drone hava sahası çokgen" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar tarafından Kalem<a href='https://codepen.io/azuremaps'>@azuremaps</a>Drone hava sahası <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>çokgenine</a> bakın .
</iframe>

## <a name="customize-a-polygon-extrusion-layer"></a>Çokgen ekstrüzyon katmanını özelleştirme

Çokgen Ekstrüzyon katmanı çeşitli şekillendirme seçenekleri vardır. Burada bunları denemek için bir araçtır.

<br/>

<iframe height='700' scrolling='no' title='PoogBRJ' src='//codepen.io/azuremaps/embed/PoogBRJ/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/PoogBRJ/'>PoogBRJ</a> 'e bakın .
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [poligon ekstrüzyon tabakası](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest)

Ek kaynaklar:

> [!div class="nextstepaction"]
> [Azure Haritalar GeoJSON belirtimi uzantısı](extend-geojson.md#circle)
