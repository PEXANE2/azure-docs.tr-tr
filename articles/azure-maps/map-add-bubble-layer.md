---
title: Haritaya kabarcık katmanı ekleme | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Web SDK 'sını kullanarak bir haritaya kabarcık katmanı ekleme hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7ae11734eb804715f3eb1b5edcb02fc328dafec8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77208565"
---
# <a name="add-a-bubble-layer-to-a-map"></a>Haritaya kabarcık katmanı ekleme

Bu makalede, bir veri kaynağındaki nokta verilerinin haritada kabarcık katmanı olarak nasıl oluşturulduğu gösterilir. Kabarcık katmanları, bir sabit piksel yarıçapı ile haritada daire olarak işleme noktaları. 

> [!TIP]
> Kabarcık katmanları varsayılan olarak bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Katmanı yalnızca nokta geometrisi özelliklerinin oluşturduğu şekilde sınırlamak için `filter` katmanın özelliğini olarak ayarlayın `['==', ['geometry-type'], 'Point']` ve `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` MultiPoint özelliklerini de dahil etmek istiyorsanız.

## <a name="add-a-bubble-layer"></a>Baloncuk katmanı ekleme

Aşağıdaki kod, bir dizi noktayı bir veri kaynağına yükler. Ardından, veri noktalarını bir [kabarcık katmanına](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)bağlar. Kabarcık katmanı, her kabarcığun yarıçapını beş piksel ve bir de beyaz dolgulu bir renk oluşturur. Ve, mavi renkli bir kontur rengi ve altı piksellik bir kontur kalınlığı. 

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

Aşağıda, yukarıdaki işlevselliğin tamamen çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='BubbleLayer veri kaynağı' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen üzerinde Azure Maps () tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer veri kaynağına</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="show-labels-with-a-bubble-layer"></a>Kabarcık katmanı içeren etiketleri göster

Bu kod, haritada bir noktayı işlemek için balon katmanını nasıl kullanacağınızı gösterir. Ve bir etiketi işlemek için bir sembol katmanını kullanma. Sembol katmanının simgesini gizlemek için, `image` simge seçeneklerinin özelliğini olarak ayarlayın `'none'` .

<br/>

<iframe height='500' scrolling='no' title='Çoklu katman veri kaynağı' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen üzerinde Azure Maps () tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>Multilayer veri kaynağına</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

## <a name="customize-a-bubble-layer"></a>Balon katmanını özelleştirme

Kabarcık katmanının yalnızca birkaç stil seçeneği vardır. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='Kabarcık katmanı seçenekleri' src='//codepen.io/azuremaps/embed/eQxbGm/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/eQxbGm/'>kabarcık katmanı seçeneklerine</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](map-add-pin.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)
