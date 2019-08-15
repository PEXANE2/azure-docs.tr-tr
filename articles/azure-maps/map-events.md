---
title: Azure haritalar ile fare olaylarını işleme | Microsoft Docs
description: Harita olayları ile etkileşimli Web SDK eşlemesi yapma
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 37a3fc3178fe5caeacedfd355a6065ee189a5890
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976540"
---
# <a name="interact-with-the-map---mouse-events"></a>Harita-fare olaylarıyla etkileşim kurma

Bu makalede haritadaki olayları ve haritanın farklı katmanlarını vurgulamak için [harita sınıfı olayları](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?#events) özelliğinin nasıl kullanılacağı gösterilir. Ayrıca, bir HTML işaretleyicisi ile etkileşim kurarken olayları vurgulamak için harita sınıfı olayları özelliğini nasıl kullanacağınızı gösterir.

## <a name="interact-with-the-map"></a>Harita ile etkileşim kurma

Aşağıdaki haritada yürütün ve sağ tarafta vurgulanan ilgili fare olaylarına bakın. JavaScript kodunu görüntülemek ve düzenlemek için **js sekmesine** tıklayabilirsiniz. Ayrıca, **birlikte Düzenle** ' yi tıklatın ve kodu CodePen ' da düzenleyebilirsiniz.

<br/>

<iframe height='600' scrolling='no' title='Harita ile etkileşim kurma – fare olayları' src='//codepen.io/azuremaps/embed/bLZEWd/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. kalemin <a href='https://codepen.io/azuremaps/pen/bLZEWd/'>harita – fare olayları ile</a> Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io'>codepen</a>.
</iframe>

## <a name="interact-with-map-layers"></a>Harita katmanları ile etkileşim kurma

Aşağıdaki kod, sembol katmanıyla etkileşime geçerek harekete geçen olayların adını vurgular. Sembol, kabarcık, çizgi ve çokgen katmanı, hepsi aynı olay kümesini destekler. Isı haritası ve döşeme katmanları bu olayların hiçbirini desteklemez.

<br/>

<iframe height='600' scrolling='no' title='Harita-katman olayları ile etkileşim kurma' src='//codepen.io/azuremaps/embed/bQRRPE/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da <a href='https://codepen.io/azuremaps/pen/bQRRPE/'>harita-katman olayları</a> (Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile etkileşim kurma kalemine bakın.
</iframe>

## <a name="interact-with-html-marker"></a>HTML Işaretleyicisi ile etkileşim kurma

Aşağıdaki kod, bir HTML işaretine JavaScript harita olayları ekler. Ayrıca, HTML işaretleyicisi ile etkileşimde bulunana kadar harekete geçen olayların adını vurgular.

<br/>

<iframe height='500' scrolling='no' title='Map-HTML Işaretleyici olayları ile etkileşim kurma' src='//codepen.io/azuremaps/embed/VVzKJY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/VVzKJY/'>harita-HTML işaretçisi olayları ile etkileşime</a> geçen kaleme bakın.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Azure haritalar Hizmetleri modülünü kullanma](./how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)
