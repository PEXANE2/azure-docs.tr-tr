---
title: Azure haritalar 'a bir görüntü katmanı ekleme | Microsoft Docs
description: Azure Maps web SDK 'sına bir görüntü katmanı ekleme.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: fadaaf7c64b11a6d6d94c68234f8288d1b3f8d07
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74480493"
---
# <a name="add-an-image-layer-to-a-map"></a>Haritaya görüntü katmanı ekleme

Bu makalede, bir görüntüyü haritada sabit bir koordinat kümesiyle nasıl kaplabir şekilde kullanabileceğiniz gösterilmektedir. Haritadaki bir görüntünün fazla yerleşimi tamamlandığında çok sayıda senaryo vardır. Aşağıda, haritalar üzerinde yer alan görüntülerin türüne birkaç örnek verilmiştir;

* Drones 'den yakalanan görüntüler.
* Floorplanlar oluşturma.
* Geçmiş veya diğer özel harita görüntüleri.
* İş sitelerinin planları.
* Hava durumu radar görüntüleri.

> [!TIP]
> Bir [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) , bir harita üzerindeki görüntüyü kaplama için hızlı bir yoldur. Ancak, görüntü büyükse, tarayıcı onu yüklemek için çıkabilir. Bu durumda, görüntünüzü kutucuklara bölmek ve bir [Tilelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)olarak haritaya yüklemek için göz önünde bulundurun.

Görüntü katmanı aşağıdaki görüntü biçimlerini destekler:

- JPEG
- PNG
- BMP
- GIF (animasyon yok)

## <a name="add-an-image-layer"></a>Görüntü katmanı ekleme

Aşağıdaki kodda, haritadaki 1922 ' den bir [Newark New Jersey haritasının](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) bir görüntüsü yer alır. Bir [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) , BIR görüntüye URL geçirerek ve `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]`biçimindeki dört köşelerin koordinatları koordinatlarıyla oluşturulur.

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

Aşağıda, yukarıdaki işlevselliğin tamamen çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Basit görüntü katmanı' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile Ilgili kalem <a href='https://codepen.io/azuremaps/pen/eQodRo/'>basit görüntü katmanı</a> ' na bakın.
</iframe>

## <a name="import-a-kml-ground-overlay"></a>KML zemin kaplamasını içeri aktarma

Bu örnek, KML zemin kaplama bilgilerinin haritada bir görüntü katmanı olarak nasıl ekleneceğini gösterir. KML zemin Yerpaylaşımları, Kuzey, Güney, Doğu ve Batı koordinatları ve saat yönünde bir döndürme sağlar, ancak görüntü katmanı görüntünün her bir köşesinde koordinatları bekler. Bu örnekteki KML kii kaplama, [Wikımedıa](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml)'Dan alınan Chartres Cathedral ve kaynaklıdır.

Aşağıdaki kod, KML zemin kaplamasıyla Kuzey, Güney, Doğu, Batı ve döndürme bilgilerini görüntünün dört köşesini hesaplamak için [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) sınıfının statik `getCoordinatesFromEdges` işlevini kullanır.

<br/>

<iframe height='500' scrolling='no' title='Görüntü katmanı olarak KML zemin kaplaması' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>resim katmanı olarak kalem KML zemin kaplamasını</a> görüntüleyin.
</iframe>

## <a name="customize-an-image-layer"></a>Görüntü katmanını özelleştirme

Görüntü katmanında birçok stil seçeneği vardır. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='Görüntü katmanı seçenekleri' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>resim katmanı seçeneklerine</a> bakın.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Imagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Imagelayeroptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Döşeme katmanı Ekle](./map-add-tile-layer.md)