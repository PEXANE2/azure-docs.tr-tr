---
title: Azure haritalar 'a balon katmanı ekleme | Microsoft Docs
description: JavaScript eşlemesine kabarcık katmanı ekleme
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 516e4f35c88ae9c0e2d63e8a4ee40eb57c05ac29
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639047"
---
# <a name="add-a-bubble-layer-to-a-map"></a>Haritaya kabarcık katmanı ekleme

Bu makalede, bir veri kaynağındaki nokta verilerini haritada kabarcık katmanı olarak nasıl işleyebilmeniz gösterilmektedir. Kabarcık katmanları, sabit piksel yarıçapı ile haritada daire olarak işleme noktaları. 

> [!TIP]
> Kabarcık katmanları varsayılan olarak bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Katmanı yalnızca nokta geometrisi özelliklerinin oluşturduğu şekilde sınırlamak için `filter` `['==', ['geometry-type'], 'Point']` katmanın özelliğini olarak ayarlayın ve `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` MultiPoint özelliklerini de dahil etmek istiyorsanız.

## <a name="add-a-bubble-layer"></a>Baloncuk katmanı ekleme

<iframe height='500' scrolling='no' title='BubbleLayer veri kaynağı' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer veri kaynağına</a> bakın.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğunda, bir [nokta](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) nesneleri dizisi tanımlanır ve [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) nesnesine eklenir.

[Kabarcık katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) , [veri kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) , haritada daire olarak Sarmalanan nokta tabanlı verileri işler. Son kod bloğu bir kabarcık katmanı oluşturur ve haritaya ekler. [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions)adresindeki kabarcık katmanının özelliklerine bakın.

Nokta nesneleri dizisi, veri kaynağı ve kabarcık katmanı oluşturulur ve eşleme tam yüklendikten sonra dairenin görüntülendiğinden emin olmak için [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlevi içindeki haritaya eklenir.

## <a name="show-labels-with-a-bubble-layer"></a>Kabarcık katmanı içeren etiketleri göster

<iframe height='500' scrolling='no' title='Çoklu katman veri kaynağı' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>Multilayer veri kaynağına</a> bakın.
</iframe>

Yukarıdaki kod, haritada verileri görselleştirmeyi ve etiketlemesini gösterir. Yukarıdaki ilk kod bloğu bir harita nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğu, bir [nokta](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) nesnesi oluşturur. Daha sonra veri [kaynağı sınıfını kullanarak](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) bir veri kaynağı nesnesi oluşturur ve noktayı veri kaynağına ekler.

[Kabarcık katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) , [veri kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) , haritada daire olarak Sarmalanan nokta tabanlı verileri işler. Üçüncü kod bloğu bir kabarcık katmanı oluşturup haritaya ekler. [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions)adresindeki kabarcık katmanının özelliklerine bakın.

Bir [sembol katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) , [veri kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) kaydırılan nokta tabanlı verileri haritada semboller olarak oluşturmak için metin veya simgeleri kullanır. Son kod bloğu, kabarcık için metin etiketini oluşturan haritaya bir sembol katmanı oluşturur ve ekler. Bkz. [Symbollayeroptions](/javascript/api/azure-maps-control/atlas.symbollayeroptions)'da bir sembol katmanının özelliklerine bakın.

Veri kaynağı ve Katmanlar oluşturulur ve eşleme tam olarak yüklendikten sonra verilerin görüntülenmesini sağlamak için [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlevi içindeki haritaya eklenir.

## <a name="customize-a-bubble-layer"></a>Balon katmanını özelleştirme

Kabarcık katmanının yalnızca birkaç stil seçeneği vardır. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='Kabarcık katmanı seçenekleri' src='//codepen.io/azuremaps/embed/eQxbGm/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/eQxbGm/'>kabarcık katmanı seçeneklerine</a> bakın.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](map-add-pin.md)

> [!div class="nextstepaction"]
> [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)