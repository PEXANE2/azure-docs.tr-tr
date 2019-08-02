---
title: Azure haritalar ile harita oluşturma | Microsoft Docs
description: JavaScript eşlemesi oluşturma
author: jingjing-z
ms.author: jinzh
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 8a4f67290e93d8b296added9023fe9b6947ba02c
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638689"
---
# <a name="create-a-map"></a>Harita oluşturma

Bu makalede harita oluşturma ve Haritayı canlandırma yolları gösterilmektedir.  

## <a name="understand-the-code"></a>Kodu anlama

Bir harita oluşturabileceğiniz iki yol vardır. Orta nokta ve yakınlaştırma düzeyini belirterek haritanın kamerasını ayarlayabilir veya Güney ve kuzeydoğu sınırlayıcı noktalarını belirterek haritanın kamera sınırlarını ayarlayabilirsiniz.

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Kamerayı ayarlama

<iframe height='500' scrolling='no' title='CameraOptions aracılığıyla harita oluşturma' src='//codepen.io/azuremaps/embed/qxKBMN/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure konum tabanlı hizmetler (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>bir harita `CameraOptions` oluşturma </a>kalemine bakın.
</iframe>

Yukarıdaki kodda, ile bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) oluşturulur `new atlas.Map()` ve merkez ve yakınlaştırma ayarlanır. Merkez ve yakınlaştırma düzeyi gibi harita özellikleri [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions)'in bir parçasıdır.

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Kamera sınırlarını ayarlama

<iframe height='500' scrolling='no' title='CameraBoundsOptions aracılığıyla harita oluşturma' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>bir harita `CameraBoundsOptions` oluşturma </a>kalemine bakın.
</iframe>

Yukarıdaki kodda, ile `new atlas.Map()`bir [Map nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) oluşturulur. Gibi eşleme özellikleri `CameraBoundsOptions` , Map sınıfının [setcamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) işlevi aracılığıyla tanımlanabilir. Sınırlar ve doldurma özellikleri kullanılarak `setCamera`ayarlanır.

### <a name="animate-map-view"></a>Harita görünümüne animasyon ekleme

<iframe height='500' scrolling='no' title='Harita görünümüne animasyon ekleme' src='//codepen.io/azuremaps/embed/WayvbO/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure haritalar (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile ilgili <a href='https://codepen.io/azuremaps/pen/WayvbO/'>harita görünümü</a> kalemine bakın.
</iframe>

Yukarıdaki kodda, ilk kod bloğu aracılığıyla `new atlas.Map()`bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) oluşturur. Merkez ve yakınlaştırma düzeyi gibi harita özellikleri [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions)'in bir parçasıdır. `CameraOptions`Map oluşturucusunun veya Map sınıfının [Setcamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) aracılığıyla tanımlanabilir. [Harita stili](https://docs.microsoft.com/azure/azure-maps/supported-map-styles) olarak `road`ayarlanır.

İkinci kod bloğu, [Setcamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) Işlevi aracılığıyla [animasyon seçeneklerini](/javascript/api/azure-maps-control/atlas.animationoptions) tanımlayarak harita görünümündeki değişikliği canlandıran bir animasyon ekleme işlevi oluşturur. İşlev, her tıklama üzerinde rastgele bir yakınlaştırma düzeyi oluşturmak için ' Haritayı canlandır ' düğmesi tarafından tetiklenir.

## <a name="try-out-the-code"></a>Kodu deneyin

Yukarıdaki örnek koda göz atın. Sol taraftaki **js sekmesinde** JavaScript kodunu düzenleyebilir ve sağ taraftaki **sonuç sekmesinde** harita görünümü değişiklikleri ' ne bakabilirsiniz. Ayrıca, **birlikte Düzenle** düğmesine tıklayıp kodu CodePen ' de düzenleyebilirsiniz.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Uygulamanıza işlevsellik eklemek için bkz. kod örnekleri:

> [!div class="nextstepaction"]
> [Harita stili seçin](choose-map-style.md)

> [!div class="nextstepaction"]
> [Harita denetimleri Ekle](map-add-controls.md)
