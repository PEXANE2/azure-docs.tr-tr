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
ms.openlocfilehash: 631a9e2d44b798404ee7567d3ccfed90628d2f8b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75432858"
---
# <a name="add-an-image-layer-to-a-map"></a>Haritaya görüntü katmanı ekleme

Bu makalede, bir görüntünün harita üzerindeki sabit bir koordinat kümesiyle nasıl kaplamanız gösterilmektedir. Haritalar üzerinde sık sık çakışan görüntü türüne örnek olarak aşağıda verilmiştir:

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

Aşağıdaki kod, haritadaki [1922 ' den bir Newark, New Jersey haritasının](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) bir görüntüsünü yer paylaşımlı olarak yer alır. Bir [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) , BIR görüntüye URL geçirerek ve `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]`biçimdeki dört köşelerin koordinatları ile oluşturulur.

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

<iframe height='500' scrolling='no' title='Basit görüntü katmanı' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile Ilgili kalem <a href='https://codepen.io/azuremaps/pen/eQodRo/'>basit görüntü katmanı</a> ' na bakın.
</iframe>

## <a name="import-a-kml-ground-overlay"></a>KML zemin kaplamasını içeri aktarma

Bu sonraki örnek, KML zemin kaplama bilgilerinin haritada bir görüntü katmanı olarak nasıl ekleneceğini gösterir. KML sol yer paylaşımları, Kuzey, Güney, Doğu ve Batı koordinatları ve bir sayaç saat yönünde döndürme sağlar, ancak görüntü katmanı görüntünün her bir köşesinde koordinatları bekler. Bu örnekteki KML zemin kaplaması Chartres Cathedral ' dir ve [Wikımedıa](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml)'dan kaynaklıdır.

Aşağıdaki kod, [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) sınıfının statik `getCoordinatesFromEdges` işlevini kullanır. Görüntünün dört köşesini Kuzey, Güney, Doğu ve Batı ve KML zemin kaplamasıyla döndürme bilgilerini hesaplar.

<br/>

<iframe height='500' scrolling='no' title='Görüntü katmanı olarak KML zemin kaplaması' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>resim katmanı olarak kalem KML zemin kaplamasını</a> görüntüleyin.
</iframe>

## <a name="customize-an-image-layer"></a>Görüntü katmanını özelleştirme

Görüntü katmanında birçok stil seçeneği vardır. İşte denemek için bir araç.

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