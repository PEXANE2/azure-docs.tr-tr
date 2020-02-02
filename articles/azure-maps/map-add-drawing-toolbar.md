---
title: Haritaya Çizim araç çubuğu ekleme | Microsoft Azure haritaları
description: Azure Haritalar Web SDK 'sını kullanarak bir haritaya Çizim araç çubuğu ekleme
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 4c89765a3bc59a37a182a2dfabf0727f95b575b8
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76933220"
---
# <a name="add-a-drawing-tools-toolbar-to-a-map"></a>Haritaya bir çizim araçları araç çubuğu ekleme

Bu makalede, çizim araçları modülünü nasıl kullanabileceğiniz ve Çizim araç çubuğunun haritada nasıl görüntüleneceği gösterilmektedir. [Drawingtoolbar](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) denetimi harita üzerine çizim araç çubuğunu ekler. Yalnızca bir ve tüm çizim araçlarıyla eşlemeler oluşturmayı ve çizim şekillerinin çizim şekillerini oluşturmayı nasıl özelleştireceğinizi öğreneceksiniz.

## <a name="add-drawing-toolbar"></a>Çizim araç çubuğu ekleme

Aşağıdaki kod, çizim Yöneticisi 'nin bir örneğini oluşturur ve araç çubuğunu haritada görüntüler.

```Javascript
//Create an instance of the drawing manager and display the drawing toolbar.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'dark'
        })
    });
```

Yukarıdaki işlevlerin tamamen çalışan kod örneği aşağıda verilmiştir:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizim araç çubuğu ekleme" src="//codepen.io/azuremaps/embed/ZEzLeRg/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/ZEzLeRg/'>Çizim araç çubuğu ekleme</a> bölümüne bakın.
</iframe>


## <a name="limit-displayed-toolbar-options"></a>Görünen araç çubuğu seçeneklerini sınırla

Aşağıdaki kod, çizim Yöneticisi 'nin bir örneğini oluşturur ve araç çubuğunu haritada yalnızca bir çokgen çizim aracıyla görüntüler. 

```Javascript
//Create an instance of the drawing manager and display the drawing toolbar with polygon drawing tool.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'light',
            buttons: ["draw-polygon"]
        })
    });
```

Yukarıdaki işlevlerin tamamen çalışan kod örneği aşağıda verilmiştir:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen çizim aracı ekleme" src="//codepen.io/azuremaps/embed/OJLWWMy/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/OJLWWMy/'>bir çokgen çizim aracı ekleme</a> kalemine bakın.
</iframe>


## <a name="change-drawing-rendering-style"></a>Çizim işleme stilini değiştir

Aşağıdaki kod, çizim yöneticisinden işleme katmanlarını alır ve çizim için işleme stilini değiştirecek seçeneklerini değiştirir. Bu durumda, işaret mavi bir işaret simgesiyle işlenir. Satırlar kırmızı ve dört piksel genişliğinde olur. Çokgenler yeşil bir Fill rengine ve turuncu bir anahatta sahip olur.

```Javascript
var layers = drawingManager.getLayers();
    layers.pointLayer.setOptions({
        iconOptions: {
            image: 'marker-blue'
        }
    });
    layers.lineLayer.setOptions({
        strokeColor: 'red',
        strokeWidth: 4
    });
    layers.polygonLayer.setOptions({
        fillColor: 'green'
    });
    layers.polygonOutlineLayer.setOptions({
        strokeColor: 'orange'
    });
```

Yukarıdaki işlevlerin tamamen çalışan kod örneği aşağıda verilmiştir:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizim işleme stilini değiştir" src="//codepen.io/azuremaps/embed/OJLWpyj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile ilgili <a href='https://codepen.io/azuremaps/pen/OJLWpyj/'>Çizim işleme stilini</a> görüntüleyin.
</iframe>


## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülünün ek özelliklerini kullanmayı öğrenin:

> [!div class="nextstepaction"]
> [Şekil verilerini al](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Çizim olaylarına tepki verme](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Etkileşim türleri ve klavye kısayolları](drawing-tools-interactions-keyboard-shortcuts.md)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Çizim Yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)
