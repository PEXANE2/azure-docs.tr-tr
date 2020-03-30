---
title: Haritaya Resim katmanı ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak bir resmin haritaya nasıl yer üstü yatıracağınız hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 69bf41f9d88081b9a416b9bee91e8650a84f12c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77209724"
---
# <a name="add-an-image-layer-to-a-map"></a>Haritaya resim katmanı ekleme

Bu makalede, bir görüntüyü sabit bir koordinat kümesine nasıl yerleştirirsiniz gösterilmektedir. Haritalarda yer alabilen farklı resim türlerine birkaç örnek aşağıda verilmiştir:

* İnsansız hava araçlarından çekilen görüntüler
* Bina kat planları
* Geçmiş veya diğer özel harita görüntüleri
* İş yerlerinin planları
* Hava radarı görüntüleri

> [!TIP]
> [ImageLayer,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) bir resmi haritaya kaplamanın kolay bir yoludur. Tarayıcıların büyük bir görüntü yüklemede zorluk çekebileceğini unutmayın. Bu durumda, görüntünüzü kutucuklara bölmeyi ve bunları Bir [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)olarak haritaya yüklemeyi düşünün.

Görüntü katmanı aşağıdaki görüntü biçimlerini destekler:

- JPEG
- PNG
- BMP
- GIF (animasyon yok)

## <a name="add-an-image-layer"></a>Görüntü katmanı ekleme

Aşağıdaki [kod, 1922'de newark, New Jersey haritasının](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) bir resmini haritaya bindirmektedir. [ImageLayer,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) bir görüntüye URL geçirerek oluşturulur ve biçimdeki `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]`dört köşenin koordinatları.

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

Burada, önceki kodun tam çalışan kod örneği vereyim.

<br/>

<iframe height='500' scrolling='no' title='Basit Görüntü Katmanı' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Basit Resim <a href='https://codepen.io/azuremaps/pen/eQodRo/'>Katmanına</a> bakın .
</iframe>

## <a name="import-a-kml-file-as-ground-overlay"></a>Zemin kaplaması olarak bir KML dosyaalma

Bu örnek, haritaüzerinde bir görüntü katmanı olarak KML yer kaplaması bilgilerinin nasıl ekleyeceğini gösterir. KML yer kaplamaları kuzey, güney, doğu ve batı koordinatları sağlar ve saat yönünün tersine döner. Ancak, görüntü katmanı görüntünün her köşesi için koordinatlar bekler. Bu örnekteki KML yer kaplaması Chartres katedrali içindir ve [Wikimedia'dan](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml)temin edilebistir.

Kod, `getCoordinatesFromEdges` [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) sınıfındaki statik işlevi kullanır. KML yer kaplamasının kuzey, güney, doğu, batı ve dönüş bilgilerini kullanarak görüntünün dört köşesini hesaplar.

<br/>

<iframe height='500' scrolling='no' title='Görüntü Katmanı Olarak KML Yer Kaplaması' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Kalem <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>KML Yer Kaplamasını</a> <a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Haritalar'a göre Görüntü Katmanı olarak görün ( )
</iframe>

## <a name="customize-an-image-layer"></a>Resim katmanını özelleştirme

Görüntü katmanıbirçok stil seçeneği vardır. İşte bunları denemek için bir araçtır.

<br/>

<iframe height='700' scrolling='no' title='Görüntü Katmanı Seçenekleri' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>CodePen'de Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Görüntü Katmanı <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>Seçenekleri</a> <a href='https://codepen.io'>'ne</a>bakın .
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [ImageLayerSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Kutucuk katmanı ekleme](./map-add-tile-layer.md)