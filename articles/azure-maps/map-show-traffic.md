---
title: Azure haritalar ile trafiği gösterme | Microsoft Docs
description: Bir JavaScript eşlemesinde trafik verilerini görüntüleme
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 55bfc434082b2d5b7de193e969fc34f710657cdb
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638640"
---
# <a name="show-traffic-on-the-map"></a>Haritada trafiği göster

Bu makalede, Haritada trafik ve olay bilgilerinin nasıl gösterileceği gösterilmektedir.

## <a name="understand-the-code"></a>Kodu anlama

<iframe height='456' scrolling='no' title='Haritada trafiği gösterme' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz <a href='https://codepen.io'>. kaleme</a>Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>bir haritadaki trafiği gösterme</a> .
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur. Yönergeler için [bir harita oluşturma](map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğu haritadaki trafik akışlarını ve olayları işlemek için haritanın [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) Işlevi Içinde [settraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) işlevini kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Kod örnek sayfası](https://aka.ms/AzureMapsSamples)

Kullanıcı deneyimlerinizi geliştirin:

> [!div class="nextstepaction"]
> [Fare olayları ile harita etkileşimi](./map-events.md)

> [!div class="nextstepaction"]
> [Erişilebilir eşleme oluşturma](./map-accessibility.md)
