---
title: Haritadaki şekillerden veri al | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Web SDK 'sını kullanarak bir haritada çizilmiş şekil verilerini alma hakkında bilgi edinin.
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: ca1b9854b118d359167873313f1fcb11bfecee52
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911004"
---
# <a name="get-shape-data"></a>Şekil verilerini alma

Bu makalede, [Çizim Yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#getsource--) **Drawingmanager. GetSource ()** işlevini kullanarak haritada çizilmiş şekil verilerini alma işlemi gösterilmektedir. Çizilmiş bir şeklin coğrafi JSON verilerini ayıklamak ve başka bir yerde kullanmak isteyebileceğiniz çeşitli senaryolar olabilir.  


## <a name="get-data-from-drawn-shape"></a>Çizilen şekilden verileri al

Aşağıdaki işlev, çizilen şeklin kaynak verilerini alır ve ekranda çıktı. 

```Javascript
function getDrawnShapes() {
    var source = drawingManager.getSource();

    document.getElementById('CodeOutput').value = JSON.stringify(source.toJson(), null, '    ');
}
```

Aşağıda, işlevleri test etmek üzere bir şekil ekleyebileceğiniz, çalışan tüm kod örneği verilmiştir:

<br/>

<iframe height="686" title="Şekil verilerini alma" src="//codepen.io/azuremaps/embed/xxKgBVz/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile <a href='https://codepen.io/azuremaps/pen/xxKgBVz/'>şekil verilerini alın</a> kalemine bakın.
</iframe>


## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülünün ek özelliklerini kullanmayı öğrenin:

> [!div class="nextstepaction"]
> [Çizim olaylarına tepki verme](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Etkileşim türleri ve klavye kısayolları](drawing-tools-interactions-keyboard-shortcuts.md)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Çizim Yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
