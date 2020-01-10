---
title: Azure haritalar 'da harita stili seçme
titleSuffix: Azure Maps
description: Azure Maps stiliyle ilgili işlevler hakkında bilgi edinin.
author: walsehgal
ms.author: v-musehg
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: e1d3d1b5904eded9f35c5ba628bea6426e7afaf0
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75531556"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Azure haritalar 'da harita stili seçme

[Azure haritalar 'da desteklenen harita stillerinin](./supported-map-styles.md) BIRÇOĞU Web SDK 'da bulunabilir. Bu makalede, harita yükünde bir stil ayarlamak, yeni bir stil ayarlamak ve stil seçici denetimini kullanmak için stille ilgili işlevlerin nasıl kullanılacağı gösterilmektedir.

## <a name="set-style-on-map-load"></a>Harita yükünde stil ayarla

Aşağıdaki kodda, eşlemenin `style` seçeneği başlatma sırasında `grayscale_dark` olarak ayarlanmıştır.

<br/>

<iframe height='500' scrolling='no' title='Harita yükte stili ayarlama' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>harita yükünün stilini ayarlama</a> kalemine bakın.
</iframe>

## <a name="update-the-style"></a>Stili Güncelleştir

Aşağıdaki kodda, bir harita örneği yüklendikten sonra harita stili, eşlemenin [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) işlevi kullanılarak `satellite` `road` güncelleştirilir.

<br/>

<iframe height='500' scrolling='no' title='Stili güncelleştirme' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>stili güncelleştirme</a> kalemine bakın.
</iframe>

## <a name="add-the-style-picker"></a>Stil seçiciyi ekleme

Aşağıdaki kod, kullanıcının farklı harita stilleri arasında kolayca geçiş yapabilmesi için haritaya bir [Stylecontrol](/javascript/api/azure-maps-control/atlas.control.stylecontrol) ekler. 

<br/>

<iframe height='500' scrolling='no' title='Stil seçiciyi ekleme' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Stil seçiciyi ekleme</a> kalemine bakın.
</iframe>

> [!TIP]
> Varsayılan olarak, varsayılan olarak, Azure Maps 'in S0 fiyatlandırma katmanını kullanırken stil Seçici denetimine sunulan tüm stiller listelenir. Bu listedeki stil sayısını azaltmak isterseniz, listede görünmesini istediğiniz stillerin dizisini stil seçicisinin `mapStyle` seçeneğine geçirin. S1 kullanıyorsanız ve tüm kullanılabilir stilleri göstermek istiyorsanız, stil seçicinin `mapStyles` seçeneğini `"all"`olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinmek için:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Haritalar için denetimler ekleyin:

> [!div class="nextstepaction"]
> [Harita denetimleri Ekle](map-add-controls.md)

> [!div class="nextstepaction"]
> [PIN Ekle](map-add-pin.md)
