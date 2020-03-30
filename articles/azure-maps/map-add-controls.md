---
title: Haritaya denetim ekleme | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar'daki bir haritaya yakınlaştırma denetimi, adım denetimi, döndürme denetimi ve stil seçici ekleme nasıl eklenir.
author: philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 094dc9fd01ec71f378a173a2b4fa64cc672d7c97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334555"
---
# <a name="add-controls-to-a-map"></a>Haritaya denetim ekleme

Bu makalede, bir haritaya denetimleri nasıl ekleyeceğiniz gösterilmektedir. Ayrıca, tüm denetimleri ve stil [seçiciile](https://docs.microsoft.com/azure/azure-maps/choose-map-style)bir harita oluşturmayı da öğreneceksiniz.

## <a name="add-zoom-control"></a>Yakınlaştırma denetimi ekleme

Yakınlaştırma denetimi, haritayı yakınlaştırmak ve uzaklaştırmak için düğmeler ekler. Aşağıdaki kod örneği [ZoomControl](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) sınıfının bir örneğini oluşturur ve haritanın sağ alt köşesine ekler.

```javascript
//Construct a zoom control and add it to the map.
map.controls.add(new atlas.control.ZoomControl(), {
    position: 'bottom-right'
});
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Yakınlaştırma denetimi ekleme' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>yakınlaştırma denetimi ekleme</a> kalemine bakın.
</iframe>

## <a name="add-pitch-control"></a>Pitch control ekle

Bir adım kontrolü ufka göre harita için perde yatırma katıyor düğmeler ekler. Aşağıdaki kod örneği [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) sınıfının bir örneğini oluşturur. PitchControl'i haritanın sağ üst köşesine ekler.

```javascript
//Construct a pitch control and add it to the map.
map.controls.add(new atlas.control.PitchControl(), {
    position: 'top-right'
});
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Pitch control ekleme' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>tarafından <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>bir adım denetimi ekleme</a> kalemine bakın.
</iframe>

## <a name="add-compass-control"></a>Pusula denetimi ekleme

Pusula denetimi, haritayı döndürmek için bir düğme ekler. Aşağıdaki kod örneği [Pusula Denetimi](/javascript/api/azure-maps-control/atlas.control.compasscontrol) sınıfının bir örneğini oluşturur ve haritanın sol alt köşesine ekler.

```javascript
//Construct a compass control and add it to the map.
map.controls.add(new atlas.control.Compass(), {
    position: 'bottom-left'
});
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Döndürme denetimi ekleme' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>tarafından <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>döndürme denetimi ekleme</a> kalemine bakın.
</iframe>

## <a name="a-map-with-all-controls"></a>Tüm denetimleri içeren bir harita

Birden çok denetim bir diziye konulabilir ve haritaya aynı anda eklenebilir ve geliştirmeyi kolaylaştırmak için haritanın aynı alanına yerleştirebilir. Aşağıdaki, bu yaklaşımı kullanarak haritaya standart gezinti denetimleri ekler.

```javascript
map.controls.add([
    new atlas.control.ZoomControl(),
    new atlas.control.CompassControl(),
    new atlas.control.PitchControl(),
    new atlas.control.StyleControl()
], {
    position: "top-right"
});
```

Aşağıdaki kod örneği, haritanın sağ üst köşesine yakınlaştırma, pusula, pitch ve stil seçici denetimlerini ekler. Otomatik olarak nasıl istiflediklerine dikkat edin. Komut dosyasındaki denetim nesnelerinin sırası, haritada görünme sırasını belirler. Haritadaki denetimlerin sırasını değiştirmek için dizideki sıralarını değiştirebilirsiniz.

<br/>

<iframe height='500' scrolling='no' title='Tüm kontrolleri içeren bir harita' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar tarafından yapılan<a href='https://codepen.io/azuremaps'>@azuremaps</a>tüm denetimlerin yer ettiği Kalem Bir <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>haritasına</a> bakın .
</iframe>

Stil seçici denetimi [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) sınıfı tarafından tanımlanır. Stil seçici denetimini kullanma hakkında daha fazla bilgi için [bkz.](choose-map-style.md)

## <a name="customize-controls"></a>Denetimleri özelleştirme

Burada denetimleri özelleştirmek için çeşitli seçenekleri test etmek için bir araçtır.

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Navigasyon kontrol seçenekleri" src="//codepen.io/azuremaps/embed/LwBZMx/?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre Kalem <a href='https://codepen.io/azuremaps/pen/LwBZMx/'>Gezintisi denetim seçeneklerine</a> bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

Özelleştirilmiş gezinti denetimleri oluşturmak istiyorsanız, `atlas.Control` sınıftan yayılan bir sınıf oluşturun veya bir HTML öğesi oluşturun ve harita divinin üzerine yerleştirin. Bu UI denetiminin `setCamera` haritayı taşımak için haritalar işlevini aramasını sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Pusula Kontrolü](/javascript/api/azure-maps-control/atlas.control.compasscontrol)

> [!div class="nextstepaction"]
> [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) 

> [!div class="nextstepaction"]
> [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) 

> [!div class="nextstepaction"]
> [Yakınlaştırma Denetimi](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) 

Tam kod için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Pin ekleme](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Açılır pencere ekleme](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](map-add-shape.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer.md)

