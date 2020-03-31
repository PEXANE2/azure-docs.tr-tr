---
title: Haritaya çizim araç çubuğu ekleme | Microsoft Azure Haritaları
description: Azure Haritalar Web SDK'yı kullanarak haritaya çizim araç çubuğu ekleme
author: philmea
ms.author: philmea
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: bebf1ddfbca3aec5a551193609381cf3510bc3ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334495"
---
# <a name="add-a-drawing-tools-toolbar-to-a-map"></a>Haritaya çizim araçları araç çubuğu ekleme

Bu makalede, Çizim Araçları modülünün nasıl kullanılacağı ve çizim araç çubuğunun haritaüzerinde nasıl görüntülenebildiğiniz gösterilmektedir. [DrawingToolbar](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) denetimi haritaya çizim araç çubuğunu ekler. Yalnızca bir ve tüm çizim araçlarıyla harita oluşturmayı ve çizim yöneticisindeki çizim şekillerinin işlenmesini nasıl özelleştireceğinizi öğreneceksiniz.

## <a name="add-drawing-toolbar"></a>Çizim araç çubuğu ekleme

Aşağıdaki kod çizim yöneticisinin bir örneğini oluşturur ve araç çubuğunu haritada görüntüler.

```javascript
//Create an instance of the drawing manager and display the drawing toolbar.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'dark'
        })
    });
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizim araç çubuğu ekleme" src="//codepen.io/azuremaps/embed/ZEzLeRg/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Ekleme çizim araç <a href='https://codepen.io/azuremaps/pen/ZEzLeRg/'>çubuğuna</a> bakın ( )
</iframe>


## <a name="limit-displayed-toolbar-options"></a>Görüntülenen araç çubuğu seçeneklerini sınırlama

Aşağıdaki kod çizim yöneticisinin bir örneğini oluşturur ve araç çubuğunu haritada sadece çokgen çizim aracıyla görüntüler. 

```javascript
//Create an instance of the drawing manager and display the drawing toolbar with polygon drawing tool.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'light',
            buttons: ["draw-polygon"]
        })
    });
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen çizim aracı ekleme" src="//codepen.io/azuremaps/embed/OJLWWMy/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Bkz. Kalem <a href='https://codepen.io'>CodePen'de</a>Azure Haritalar<a href='https://codepen.io/azuremaps'>@azuremaps</a>'a göre <a href='https://codepen.io/azuremaps/pen/OJLWWMy/'>çokgen çizim aracı ekleyin</a> ( )
</iframe>


## <a name="change-drawing-rendering-style"></a>Çizim oluşturma stilini değiştirme

Çizilen şekillerin `drawingManager.getLayers()` stili, işlevi kullanarak çizim yöneticisinin temel katmanları alınarak ve daha sonra tek tek katmanlarda seçenekler ayarlayarak özelleştirilebilir. Bir şekli düzenlerken koordinatlar için görünen sürükleme tutamaçları HTML işaretçileridir. Sürükleme tutamaçlarının stili, HTML işaretçisi seçeneklerini `dragHandleStyle` çizim `secondaryDragHandleStyle` yöneticisinin seçeneklerine ve seçeneklerine geçirerek özelleştirilebilir.  

Aşağıdaki kod çizim yöneticisinden işleme katmanları alır ve çizim için render stilini değiştirmek için seçeneklerini değiştirir. Bu durumda, noktalar mavi bir işaretçi simgesiyle işlenir. Çizgiler kırmızı ve dört piksel genişliğinde olacaktır. Çokgenler yeşil dolgu rengi ve turuncu bir anahat olacaktır. Daha sonra, sürükleme tutamaçlarının stillerini kare simgeler olarak değiştirir. 

```javascript
//Get rendering layers of drawing manager.
var layers = drawingManager.getLayers();

//Change the icon rendered for points.
layers.pointLayer.setOptions({
    iconOptions: {
        image: 'marker-blue'
    }
});

//Change the color and width of lines.
layers.lineLayer.setOptions({
    strokeColor: 'red',
    strokeWidth: 4
});

//Change fill color of polygons.
layers.polygonLayer.setOptions({
    fillColor: 'green'
});

//Change the color of polygon outlines.
layers.polygonOutlineLayer.setOptions({
    strokeColor: 'orange'
});

//Update the style of the drag handles that appear when editting.
drawingManager.setOptions({
    //Primary drag handle that represents coordinates in the shape.
    dragHandleStyle: {
        anchor: 'center',
        htmlContent: '<svg width="15" height="15" viewBox="0 0 15 15" xmlns="http://www.w3.org/2000/svg" style="cursor:pointer"><rect x="0" y="0" width="15" height="15" style="stroke:black;fill:white;stroke-width:4px;"/></svg>',
        draggable: true
    },

    //Secondary drag hanle that represents mid-point coordinates that users can grab to add new cooridnates in the middle of segments.
    secondaryDragHandleStyle: {
        anchor: 'center',
        htmlContent: '<svg width="10" height="10" viewBox="0 0 10 10" xmlns="http://www.w3.org/2000/svg" style="cursor:pointer"><rect x="0" y="0" width="10" height="10" style="stroke:white;fill:black;stroke-width:4px;"/></svg>',
        draggable: true
    }
});  
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizim oluşturma stilini değiştirme" src="//codepen.io/azuremaps/embed/OJLWpyj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Değişikliği çizim oluşturma <a href='https://codepen.io/azuremaps/pen/OJLWpyj/'>stiline</a> bakın .
</iframe>


## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülünün ek özelliklerini nasıl kullanacağınızı öğrenin:

> [!div class="nextstepaction"]
> [Şekil verilerini alma](map-get-shape-data.md)

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
> [Çizim yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)
