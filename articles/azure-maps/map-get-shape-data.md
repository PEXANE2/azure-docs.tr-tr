---
title: Haritadaki şekillerden veri alma | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak haritaüzerinde şekil verilerinin nasıl çizilen alınabildiğini öğrenin.
author: philmea
ms.author: philmea
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 88db018575f92e777223f383c65cd6db51ba515a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334404"
---
# <a name="get-shape-data"></a>Şekil verilerini alma

Bu makalede, haritaüzerinde çizilen şekillerin verilerini nasıl elde edebilirsiniz. Çizim [yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#getsource--)içinde **drawingManager.getSource()** işlevini kullanırız. Çizilmiş bir şeklin geojson verilerini ayıklamak ve başka bir yerde kullanmak istediğinizde çeşitli senaryolar vardır.  


## <a name="get-data-from-drawn-shape"></a>Çizilen şekle veri alma

Aşağıdaki işlev, çizilen şeklin kaynak verilerini alır ve ekrana çıkar. 

```Javascript
function getDrawnShapes() {
    var source = drawingManager.getSource();

    document.getElementById('CodeOutput').value = JSON.stringify(source.toJson(), null, '    ');
}
```

Aşağıda işlevselliği sınamak için bir şekil çizebileceğiniz tam çalışan kod örneği verilmiştir:

<br/>

<iframe height="686" title="Şekil verilerini alma" src="//codepen.io/azuremaps/embed/xxKgBVz/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Kalem Verisi<a href='https://codepen.io/azuremaps'>@azuremaps</a>'ne göre Şekil <a href='https://codepen.io/azuremaps/pen/xxKgBVz/'>Verilerini</a> Azure Haritaları 'na ( ) görün.
</iframe>


## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülünün ek özelliklerini nasıl kullanacağınızı öğrenin:

> [!div class="nextstepaction"]
> [Çizim olaylarına tepki verme](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Etkileşim türleri ve klavye kısayolları](drawing-tools-interactions-keyboard-shortcuts.md)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Çizim yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
