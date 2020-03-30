---
title: Haritaya HTML İşaretçisi ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak haritaya html işaretçisi ekleme hakkında bilgi edineceksiniz.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 2700d42c25d58911fb275ad9ce6c5610cd22624d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536777"
---
# <a name="add-html-markers-to-the-map"></a>Haritaya HTML işaretçileri ekleme

Bu makalede, html işaretçisi olarak eşleme dosyası gibi özel bir HTML nasıl ekleyeceğiniz gösterilmektedir.

> [!NOTE]
> HTML İşaretçileri veri kaynaklarına bağlanmaz. Bunun yerine konum bilgileri doğrudan işaretçiye eklenir ve `markers` işaretçi [htmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)olan haritalar özelliğine eklenir.

> [!IMPORTANT]
> Görüntüleme için WebGL kullanan Azure Haritalar Web denetimindeki çoğu katmanın aksine, HTML İşaretçileri işleme için geleneksel DOM öğelerini kullanır. Bu nedenle, bir sayfaya ne kadar çok HTML işaretçisi eklenirse, o kadar çok DOM öğesi vardır. Birkaç yüz HTML işaretçisi ekledikten sonra performans bozulabilir. Daha büyük veri kümeleri için verilerinizi kümelemeyi veya Bir Sembol veya Kabarcık katmanı kullanmayı düşünün.

## <a name="add-an-html-marker"></a>HTML işaretçisi ekleme

[HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) sınıfıvarsayılan bir stili vardır. İşaretçinin renk ve metin seçeneklerini ayarlayarak işaretçiyi özelleştirebilirsiniz. HTML işaretçisınıfının varsayılan stili, yer `{color}` tutucusu `{text}` ve yeri tutucusu olan bir SVG şablonudur. Hızlı özelleştirme için HTML işaretçisi seçeneklerindeki renk ve metin özelliklerini ayarlayın. 

Aşağıdaki kod bir HTML işaretçisi oluşturur ve renk özelliğini "DodgerBlue" ve metin özelliğini "10" olarak ayarlar. İşaretçiye bir açılır pencere `click` eklenir ve olay açılır pencerenin görünürlüğünü geçişyapmak için kullanılır.

```javascript
//Create an HTML marker and add it to the map.
var marker = new atlas.HtmlMarker({
    color: 'DodgerBlue',
    text: '10',
    position: [0, 0],
    popup: new atlas.Popup({
        content: '<div style="padding:10px">Hello World</div>',
        pixelOffset: [0, -30]
    })
});

map.markers.add(marker);

//Add a click event to toggle the popup.
map.events.add('click',marker, () => {
    marker.togglePopup();
});
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Haritaya HTML İşaretçisi ekleme' src='//codepen.io/azuremaps/embed/MVoeVw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Kalem <a href='https://codepen.io'>CodePen'de</a>Azure Haritalar<a href='https://codepen.io/azuremaps'>@azuremaps</a>() ile <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>bir haritaya HTML İşaretçisi ekleyin.</a>
</iframe>

## <a name="create-svg-templated-html-marker"></a>SVG şablonuyla HTML işaretçisi oluşturma

Html `htmlContent` işaretçisinin varsayılanı, yer klasörleri `{color}` ve `{text}` içinde bulunan bir SVG şablonudur. Özel SVG dizeleri oluşturabilir ve bu aynı yer tutucuları SVG'nize ekleyebilir, bu işaretçinin seçeneklerini `color` ve `text` seçeneklerini SVG'nizde güncelleyebilirsiniz.

<br/>

<iframe height='500' scrolling='no' title='Özel SVG Şablonlu HTML İşaretleyici' src='//codepen.io/azuremaps/embed/LXqMWx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>CodePen'de Azure Haritalar <a href='https://codepen.io/azuremaps/pen/LXqMWx/'>'a göre Özel SVG Şablonu olan</a> Kalem HTML İşaretçisi<a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>'ne</a>bakın ( )
</iframe>

> [!TIP]
> Azure Haritalar web SDK, HTML işaretçileri ile kullanılabilecek birkaç SVG görüntü şablonu sağlar. Daha fazla bilgi için resim şablonları belgesini [nasıl kullanılır'](how-to-use-image-templates-web-sdk.md) a bakın.

## <a name="add-a-css-styled-html-marker"></a>CSS tarzı HTML işaretçisi ekleme

HTML işaretçileri yararlarından biri CSS kullanılarak elde edilebilir birçok büyük özelleştirmeler olmasıdır. Bu örnekte, HtmlMarker içeriği yerine ve darbeler içine düşer animasyonlu bir pin oluşturmak HTML ve CSS oluşur.

<br/>

<iframe height='500' scrolling='no' title='HTML Veri Kaynağı' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a göre Kalem HTML <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>DataSource</a> ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>konusuna bakın.
</iframe>

## <a name="draggable-html-markers"></a>Sürülebilir HTML işaretçileri

Bu örnek, html işaretçisinin nasıl sürülebilir hale getirilebildiğini gösterir. HTML işaretçileri `dragstart`ve `dragend` olayları destekler. `drag`

<br/>

<iframe height='500' scrolling='no' title='Sürülebilir HTML İşaretçisi' src='//codepen.io/azuremaps/embed/wQZoEV/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/wQZoEV/'>Sürülebilir HTML İşaretçisi</a> () bakın.
</iframe>

## <a name="add-mouse-events-to-html-markers"></a>HTML işaretçilerine fare olayları ekleme

Bu örnekler, html işaretçisine fare ve sürükleme olaylarının nasıl ekleyeceğini gösterir.

<br/>

<iframe height='500' scrolling='no' title='HTML İşaretleyicilerine Fare Olayları Ekleme' src='//codepen.io/azuremaps/embed/RqOKRz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile <a href='https://codepen.io/azuremaps/pen/RqOKRz/'>HTML İşaretçilere Fare Olayları Ekleyen</a> Kaleme bakın.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)

Haritalarınıza ekleyeceğiniz daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Görüntü şablonlarını kullanma](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](./map-add-bubble-layer.md)
