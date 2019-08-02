---
title: Azure haritalar 'da harita denetimleri ekleme | Microsoft Docs
description: Azure haritalar 'daki haritaya yakınlaştırma denetimi, sıklık denetimi, döndürme denetimi ve stil Seçici ekleme.
author: walsehgal
ms.author: v-musehg
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 7a504b8df199a3a461d5eb4e5b7238462b4c438f
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638762"
---
# <a name="add-map-controls-to-azure-maps"></a>Azure haritalar 'a harita denetimleri ekleme

Bu makalede harita denetimlerini haritaya nasıl ekleyeceğiniz gösterilir. Ayrıca, tüm denetimlerle ve [Stil seçiciyle](https://docs.microsoft.com/azure/azure-maps/choose-map-style)bir harita oluşturmayı öğreneceksiniz.

## <a name="add-zoom-control"></a>Yakınlaştırma denetimi Ekle

<iframe height='500' scrolling='no' title='Yakınlaştırma denetimi ekleme' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>yakınlaştırma denetimi ekleme</a> kalemine bakın.
</iframe>

İlk kod bloğu, anonim kimlik doğrulama mekanizmasını kullanarak bir harita nesnesi oluşturur. Harita oluşturma hakkında yönergeler için bkz. [harita oluşturma](./map-create.md) .

Yakınlaştırma denetimi haritanın yakınlaştırmasını ve dışına çıkan özelliği ekler. İkinci kod bloğu, Atlas [Zoomcontrol](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) kullanarak bir yakınlaştırma denetim nesnesi oluşturur ve haritanın [Controls. Add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) metodunu kullanarak haritaya ekler. Yakınlaştırma denetimi, eşleme tam olarak yüklendikten sonra yüklendiğinden emin olmak için harita **olay dinleyicisinin** içindedir.

## <a name="add-pitch-control"></a>Aralıklı Denetim Ekle

<iframe height='500' scrolling='no' title='Aralıklı denetim ekleme' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>bir aralıklı denetim ekleme</a> kalemine bakın.
</iframe>

İlk kod bloğu, anonim kimlik doğrulama mekanizmasını kullanarak bir harita nesnesi oluşturur. Harita oluşturma hakkında yönergeler için bkz. [harita oluşturma](./map-create.md) .

Aralıklı denetim, haritanın perdesini değiştirme yeteneğini ekler. İkinci kod bloğu, Atlas \ ' [denetimini](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) kullanarak bir sıklık denetim nesnesi oluşturur ve haritanın [Controls. Add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) metodunu kullanarak haritaya ekler. Sıklık denetimi, eşleme tam olarak yüklendikten sonra yüklendiğinden emin olmak için harita **olay dinleyicisinde** yer alabilir.

## <a name="add-compass-control"></a>Pusula denetimi Ekle

<iframe height='500' scrolling='no' title='Döndürme denetimi ekleme' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>bir döndürme denetimi ekleme</a> kalemine bakın.
</iframe>

İlk kod bloğu, anonim kimlik doğrulama mekanizmasını kullanarak bir harita nesnesi oluşturur. Harita oluşturma hakkında yönergeler için bkz. [harita oluşturma](./map-create.md) .

İkinci kod bloğu, Atlas [pusula denetimini](/javascript/api/azure-maps-control/atlas.control.compasscontrol)kullanarak bir pusula denetim nesnesi oluşturur. Ayrıca, Map 'in [Controls. Add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) metodunu kullanarak da pusula denetimini haritaya ekler. Pusula denetimi, eşleme tam olarak yüklendikten sonra yüklendiğinden emin olmak için harita **olay dinleyicisi** içindedir.

## <a name="a-map-with-all-controls"></a>Tüm denetimlerle bir eşleme

<iframe height='500' scrolling='no' title='Tüm denetimlerle bir harita' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>Tüm denetimlerle Haritayı</a> görüntüleyin.
</iframe>

İlk kod bloğu, anonim kimlik doğrulama mekanizmasını kullanarak bir harita nesnesi oluşturur. Harita oluşturma hakkında yönergeler için bkz. [harita oluşturma](./map-create.md) .

İkinci kod bloğu, Atlas [CompassControl](/javascript/api/azure-maps-control/atlas.control.compasscontrol) kullanarak bir pusula denetim nesnesi oluşturur ve haritanın [Controls. Add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) metodunu kullanarak haritaya ekler.

Üçüncü kod bloğu, Atlas [Zoomcontrol](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) kullanarak bir yakınlaştırma denetim nesnesi oluşturur ve haritanın [Controls. Add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) metodunu kullanarak haritaya ekler.

Dördüncü kod bloğu, Atlas \ ' [denetimini](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) kullanarak bir aralıklı denetim nesnesi oluşturur ve haritanın [Controls. Add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) metodunu kullanarak Map 'e ekler.

Son kod bloğu, Atlas [Stylecontrol](/javascript/api/azure-maps-control/atlas.control.stylecontrol) kullanarak bir stil Seçici nesnesi oluşturur ve haritanın [Controls. Add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) metodunu kullanarak Map 'e ekler. Tüm denetim nesneleri, eşleme tam olarak yüklendikten sonra yüklendiklerinden emin olmak için harita **olay dinleyicisi** içine eklenir.

Betikteki Denetim nesnelerinin sırası haritada görünme sırasını belirler. Haritadaki denetimlerin sırasını değiştirmek için, komut dosyasında sırasını değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)

Tam kod için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [PIN Ekle](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Açılan pencere Ekle](./map-add-popup.md)
