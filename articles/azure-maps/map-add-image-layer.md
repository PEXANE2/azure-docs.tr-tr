---
title: Haritaya görüntü katmanı ekleme | Microsoft Azure haritaları
description: Bu makalede, harita üzerinde Microsoft Azure haritaları Web SDK 'sını kullanarak bir görüntünün nasıl kaplayacağınızı öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 69bf41f9d88081b9a416b9bee91e8650a84f12c7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77209724"
---
# <a name="add-an-image-layer-to-a-map"></a>Haritaya görüntü katmanı ekleme

Bu makalede bir görüntünün sabit bir koordinat kümesiyle nasıl kaplama yapılacağı gösterilir. Haritalar üzerinde kaplama olabilecek farklı görüntü türlerine birkaç örnek aşağıda verilmiştir:

* Dronlarla 'den yakalanan görüntüler
* Floorplanlar oluşturma
* Geçmiş veya diğer özel harita görüntüleri
* İş sitelerinin şemaları
* Hava durumu radar resimleri

> [!TIP]
> Bir [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) , bir harita üzerindeki görüntüyü kaplama için kolay bir yoldur. Tarayıcıların büyük bir görüntüyü yükleme güçlüğü olabileceğini unutmayın. Bu durumda, görüntünüzü kutucuklara bölmek ve bir [Tilelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)olarak haritaya yüklemek göz önünde bulundurun.

Görüntü katmanı aşağıdaki görüntü biçimlerini destekler:

- JPEG
- PNG
- BMP
- GIF (animasyon yok)

## <a name="add-an-image-layer"></a>Görüntü katmanı ekleme

Aşağıdaki kod, haritadaki [1922 ' den bir Newark, New Jersey haritasının](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) bir görüntüsünü yer paylaşımlı olarak yer alır. Bir [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) , BIR görüntüye URL geçirerek ve biçimdeki dört köşelerin koordinatları ile oluşturulur `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]` .

```javascript
//Create an image layer and add it to the map.
map.layers.add(new atlas.layer.ImageLayer({
    url: 'newark_nj_1922.jpg',
    coordinates: [
        [-74.22655, 40.773941], //Top Left Corner
        [-74.12544, 40.773941], //Top Right Corner
        [-74.12544, 40.712216], //Bottom Right Corner
        [-74.22655, 40.712216]  //Bottom Left Corner
    ]
}));
```

Önceki kodun çalışan kod örneğinin tamamı aşağıda verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Basit görüntü katmanı' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/eQodRo/'>basit görüntü katmanına</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="import-a-kml-file-as-ground-overlay"></a>KML dosyasını arka plan kaplaması olarak içeri aktarma

Bu örnek, KML zemin kaplama bilgilerinin haritada görüntü katmanı olarak nasıl ekleneceğini gösterir. KML zemin Yerpaylaşımları, Kuzey, Güney, Doğu ve Batı koordinatları ve saat yönünde bir döndürme sağlar. Ancak, görüntü katmanı görüntünün her köşesi için koordinatları bekler. Bu örnekteki KML zemin kaplaması, Chartres Cathedral içindir ve [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml)'dan kaynaklıdır.

Kod, `getCoordinatesFromEdges` [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) sınıfından statik işlevi kullanır. KML zemin kaplamanın Kuzey, Güney, Doğu, Batı ve döndürme bilgilerini kullanarak görüntünün dört köşesini hesaplar.

<br/>

<iframe height='500' scrolling='no' title='Görüntü katmanı olarak KML zemin kaplaması' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Pen <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>KML zemin kaplaması</a> , <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps () ile görüntü katmanı olarak.
</iframe>

## <a name="customize-an-image-layer"></a>Görüntü katmanını özelleştirme

Görüntü katmanında birçok stil seçeneği vardır. İşte denemek için bir araç.

<br/>

<iframe height='700' scrolling='no' title='Görüntü katmanı seçenekleri' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>resim katmanı seçeneklerine</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Imagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Imagelayeroptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Kutucuk katmanı ekleme](./map-add-tile-layer.md)