---
title: Haritaya Kabarcık katmanı ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak haritaya Kabarcık katmanı ekleme hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7ae11734eb804715f3eb1b5edcb02fc328dafec8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77208565"
---
# <a name="add-a-bubble-layer-to-a-map"></a>Haritaya kabarcık katmanı ekleme

Bu makalede, bir veri kaynağından gelen nokta verilerini haritaüzerinde kabarcık katmanı olarak nasıl işlendirilebildiğiniz gösterilmektedir. Kabarcık katmanları, sabit bir piksel yarıçapı olan haritaüzerinde noktaları daireler halinde işler. 

> [!TIP]
> Kabarcık katmanları varsayılan olarak bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Yalnızca nokta geometrisi özelliklerini işleyen katmanı sınırlamak `filter` için, katmanın `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` özelliğini ayarlamak veya `['==', ['geometry-type'], 'Point']` MultiPoint özelliklerini de eklemek istiyorsanız.

## <a name="add-a-bubble-layer"></a>Baloncuk katmanı ekleme

Aşağıdaki kod bir veri kaynağına bir dizi nokta yükler. Daha sonra, veri noktaları bir [kabarcık katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)na bağlanır. Kabarcık katmanı beş piksel ve beyaz bir dolgu rengi ile her kabarcık yarıçapı işler. Ve, mavi bir kontur rengi ve altı piksel bir kontur genişliği. 

```javascript
//Add point locations.
var points = [
    new atlas.data.Point([-73.985708, 40.75773]),
    new atlas.data.Point([-73.985600, 40.76542]),
    new atlas.data.Point([-73.985550, 40.77900]),
    new atlas.data.Point([-73.975550, 40.74859]),
    new atlas.data.Point([-73.968900, 40.78859])
];

//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Add multiple points to the data source.
dataSource.add(points);

//Create a bubble layer to render the filled in area of the circle, and add it to the map.
map.layers.add(new atlas.layer.BubbleLayer(dataSource, null, {
    radius: 5,
    strokeColor: "#4288f7",
    strokeWidth: 6, 
    color: "white" 
}));
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='BubbleLayer DataSource' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>CodePen'deki Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer DataSource</a> <a href='https://codepen.io'>'a</a>bakın .
</iframe>

## <a name="show-labels-with-a-bubble-layer"></a>Etiketleri kabarcık katmanıyla gösterme

Bu kod, haritada bir noktayı işlemek için kabarcık katmanını nasıl kullanacağınızı gösterir. Ve, bir etiket işlemek için bir sembol katmanı nasıl kullanılır. Sembol katmanısimgesini gizlemek için `image` `'none'`simge seçeneklerinin özelliğini .

<br/>

<iframe height='500' scrolling='no' title='Çok Katmanlı Veri Kaynağı' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>CodePen'deki Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>MultiLayer DataSource</a> <a href='https://codepen.io'>'a</a>bakın .
</iframe>

## <a name="customize-a-bubble-layer"></a>Kabarcık katmanını özelleştirme

Kabarcık katmanı yalnızca birkaç stil seçeneğine sahiptir. Burada bunları denemek için bir araçtır.

<br/>

<iframe height='700' scrolling='no' title='Kabarcık Katman Seçenekleri' src='//codepen.io/azuremaps/embed/eQxbGm/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>CodePen'de Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/eQxbGm/'>Kabarcığı Katmanı Seçenekleri</a> <a href='https://codepen.io'>'ne</a>bakın .
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [BubbleLayerSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](map-add-pin.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)
