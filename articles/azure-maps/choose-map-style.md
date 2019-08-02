---
title: Azure haritalar 'da harita stili işlevleri | Microsoft Docs
description: Azure Maps stiliyle ilgili işlevler hakkında bilgi edinin.
author: walsehgal
ms.author: v-musehg
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 52936b14264bd4fe1846ae365e1de447d594b612
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639051"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Azure haritalar 'da harita stili seçme

Azure haritalar 'ın aralarından seçim yapabileceğiniz dört farklı harita stili vardır. Harita stilleri hakkında daha fazla bilgi için bkz. [Azure Maps 'de desteklenen harita stilleri](./supported-map-styles.md). Bu makalede, harita yükünde bir stil ayarlamak, yeni bir stil ayarlamak ve stil seçici denetimini kullanmak için stille ilgili işlevlerin nasıl kullanılacağı gösterilmektedir.

## <a name="set-style-on-map-load"></a>Harita yükünde stil ayarla

<iframe height='500' scrolling='no' title='Harita yükte stili ayarlama' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>harita yükünün stilini ayarlama</a> kalemine bakın.
</iframe>

Yukarıdaki kod bloğu, abonelik anahtarını ayarlar ve stili grayscale_dark olarak ayarlanmış bir harita nesnesi oluşturur. Harita oluşturma hakkında yönergeler için bkz. [harita oluşturma](./map-create.md) .

## <a name="update-the-style"></a>Stili Güncelleştir

<iframe height='500' scrolling='no' title='Stili güncelleştirme' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>stili güncelleştirme</a> kalemine bakın.
</iframe>

Yukarıdaki kod bloğu, abonelik anahtarını ayarlar ve stili önceden ayarlamadan bir Map nesnesi oluşturur. Harita oluşturma hakkında yönergeler için bkz. [harita oluşturma](./map-create.md) .

İkinci kod bloğu harita stilini uydu olarak ayarlamak için haritanın [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) yöntemini kullanır.

## <a name="add-the-style-picker"></a>Stil seçiciyi ekleme

<iframe height='500' scrolling='no' title='Stil seçiciyi ekleme' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Stil seçiciyi ekleme</a> kalemine bakın.
</iframe>

Yukarıdaki koddaki ilk kod bloğu, abonelik anahtarını ayarlar ve bir harita nesnesi oluşturur, harita stili önceden grayscale_dark olarak ayarlanmıştır. Harita oluşturma hakkında yönergeler için bkz. [harita oluşturma](./map-create.md) .

İkinci kod bloğu, Atlas [Stylecontrol](/javascript/api/azure-maps-control/atlas.control.stylecontrol) oluşturucusunu kullanarak bir stil Seçici oluşturur.

Stil Seçici, harita için stil seçimini sunar. Üçüncü kod bloğu haritanın [Controls. Add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) metodunu kullanarak Stil seçiciyi haritaya ekler. Stil Seçici, eşleme tam olarak yüklendikten sonra yüklendiğinden emin olmak için harita **olay dinleyicisinin** içindedir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinmek için:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Haritalarınıza denetim ekleyin:

> [!div class="nextstepaction"]
> [Harita denetimleri Ekle](./map-add-controls.md)

Harita PIN 'i ekle:

> [!div class="nextstepaction"]
> [PIN Ekle](./map-add-pin.md)
