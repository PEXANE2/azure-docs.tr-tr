---
title: Azure haritalar 'a HTML Işaretleyicisi ekleme | Microsoft Docs
description: JavaScript eşlemesine HTML Işaretleyicisi ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 65777a87cd042a4d8b3b14255dbf99241cd42e18
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638758"
---
# <a name="add-html-markers-to-the-map"></a>Haritaya HTML işaretçileri ekleyin

Bu makalede, bir HTML Işaretçisi olarak haritaya bir görüntü dosyası gibi özel bir HTML nasıl ekleyeceğiniz gösterilir.

> [!NOTE]
> HTML Işaretçileri veri kaynaklarına bağlanamaz. Bunun yerine, konum bilgisi doğrudan işaretçiye eklenir ve işaret, bir [htmlmarkermanager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)olan `markers` Maps özelliğine eklenir.

> [!IMPORTANT]
> Azure Haritalar Web denetimindeki birçok katmanın farklı olarak, işlemek için WebGL kullanan çoğu katmanın aksine, HTML Işaretçileri işleme için geleneksel DOM öğeleri kullanır. Bu nedenle, daha fazla HTML işaretleyicisi bir sayfa eklendikçe, daha fazla DOM öğesi vardır. Birkaç yüz HTML işaretleyicisi eklendikten sonra performans düşebilir. Daha büyük veri kümeleri için verilerinizi kümelendirmeyi veya sembol ya da kabarcık katmanı kullanmayı düşünün.

## <a name="add-an-html-marker"></a>HTML işaretleyicisi ekleme

Htmlişaretleyici sınıfının varsayılan bir stili vardır. İşaretin renk ve metin seçeneklerini ayarlayarak işaretçiyi özelleştirebilirsiniz. Htmlişaretleyici sınıfının varsayılan stili, renk ve metin yer tutucusu olan bir SVG şablonudur. Hızlı özelleştirme için Htmlişaretleyici seçeneklerinde Color ve Text özelliklerini ayarlayın. 

<br/>

<iframe height='500' scrolling='no' title='Haritaya HTML Işaretleyicisi ekleme' src='//codepen.io/azuremaps/embed/MVoeVw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>bir haritaya HTML işaretleyicisi ekleme</a> kalemine bakın.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğu [harita sınıfının](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) [işaretçiler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#markers) özelliğini kullanarak haritaya bir [htmlişaretleyici](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) ekler. , Eşleme tam olarak yüklendikten sonra görüntülendiğinden emin olmak için, [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) işlevi Içindeki haritaya htmlişaretleyici eklenir.

## <a name="create-svg-templated-html-marker"></a>SVG şablonu oluşturma HTML işaretleyicisi

HTML işaretleyicisinin varsayılan değer `htmlContent` , yer klasörleri `{color}` ve `{text}` içindeki bir SVG şablonudur. Özel SVG dizeleri oluşturabilir ve bu aynı yer tutucuları SVG 'ye ekleyebilirsiniz ve bu nedenle, işaretleyicinin `color` ve `text` seçeneklerini ayarlayarak, bu yer tutucuları SVG 'de güncelleştirin.

<br/>

<iframe height='500' scrolling='no' title='Özel SVG şablonuyla HTML Işaretleyicisi' src='//codepen.io/azuremaps/embed/LXqMWx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/LXqMWx/'>özel SVG şablonuyla kalem HTML işaretine</a> bakın.
</iframe>

## <a name="add-a-css-styled-html-marker"></a>CSS stilli HTML işaretleyicisi ekleme

HTML işaretlerinin avantajlarından biri, CSS kullanılarak erişilebilecek çok sayıda harika özelleştirme olabilir. Bu örnekte, Htmlmarkın içeriği, bir hareketli PIN oluşturan ve pulsuz olan HTML ve CSS 'den oluşur.

<br/>

<iframe height='500' scrolling='no' title='HTML veri kaynağı' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>HTML veri kaynağına</a> bakın.
</iframe>

## <a name="draggable-html-markers"></a>Sürüklenebilir HTML işaretçileri

Bu örnek, bir HTML işaretleyici sürüklenebilir nasıl yapılacağını gösterir. HTML işaretçileri desteği `drag` `dragstart` ve`dragend` olayları.

<br/>

<iframe height='500' scrolling='no' title='Sürüklenebilir HTML Işaretleyicisi' src='//codepen.io/azuremaps/embed/wQZoEV/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/wQZoEV/'>sürüklenebilir HTML işaretine</a> bakın.
</iframe>

## <a name="add-mouse-events-to-html-markers"></a>HTML işaretçilerine fare olayları ekleme

Bu örnekler, bir HTML işaretine fare ve sürükleme olaylarının nasıl ekleneceğini gösterir.

<br/>

<iframe height='500' scrolling='no' title='HTML Işaretçilerine fare olayları ekleme' src='//codepen.io/azuremaps/embed/RqOKRz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/RqOKRz/'>HTML işaretçilerine fare olayları ekleme</a> kalemine bakın.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Htmlişaretleyici](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)

Haritalarınıza eklenecek daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Kabarcık katmanı ekleme](./map-add-bubble-layer.md)