---
title: Azure haritalar 'a bir görüntü katmanı ekleme | Microsoft Docs
description: JavaScript eşlemesine görüntü katmanı ekleme
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7ea0f37e307196af4b27fd3f8fb1aa0d42443dfa
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638739"
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

## <a name="add-an-image-layer"></a>Görüntü katmanı ekleme

Bu örnek, haritadaki [1922 ' dan bir Newark New Jersey haritasının](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) bir görüntüsünün görüntüsünü nasıl kaplatagösterir.

<br/>

<iframe height='500' scrolling='no' title='Basit görüntü katmanı' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/eQodRo/'>basit görüntü katmanına</a> bakın.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğunda bir [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) , BIR görüntüye URL geçirerek ve biçimdeki `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]`dört köşelerin koordinatlarıyla oluşturulur.

## <a name="import-a-kml-ground-overlay"></a>KML zemin kaplamasını içeri aktarma

Bu örnek, KML zemin kaplama bilgilerinin haritada bir görüntü katmanı olarak nasıl ekleneceğini gösterir. KML sol yer paylaşımları, görüntü katmanının görüntünün her köşesinde koordinatları beklediği şekilde, Kuzey, Güney, Doğu ve Batı koordinatları ve saat yönünde bir döndürme sağlar. Bu örnekteki KML kii kaplama, [Wikımedıa](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml)'Dan alınan Chartres Cathedral ve kaynaklıdır.

<br/>

<iframe height='500' scrolling='no' title='Görüntü katmanı olarak KML zemin kaplaması' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Pen <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>KML zemin kaplaması</a> , <a href='https://codepen.io'>codepen</a>üzerinde Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() ile görüntü katmanı olarak.
</iframe>

Yukarıdaki kod, KML zemin kaplamasıyla Kuzey, Güney, Doğu, Batı ve döndürme bilgilerini görüntünün dört köşesini hesaplamak için `getCoordinatesFromEdges` [ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) sınıfının statik işlevini kullanır.


## <a name="customize-an-image-layer"></a>Görüntü katmanını özelleştirme

Görüntü katmanında birçok stil seçeneği vardır. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='Görüntü katmanı seçenekleri' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>resim katmanı seçeneklerine</a> bakın.
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