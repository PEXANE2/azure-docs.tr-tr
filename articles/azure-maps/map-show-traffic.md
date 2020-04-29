---
title: Bir haritada trafiği göster | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Web SDK 'sını kullanarak bir Haritada trafik verilerini görüntülemeyi öğreneceksiniz.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9bd5ae462013924a46a3da8400719e83abae3424
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79534823"
---
# <a name="show-traffic-on-the-map"></a>Haritada trafiği göster

Azure haritalar 'da kullanılabilen iki tür trafik verisi vardır:

- Olay verileri-oluşturma, yol kapanışları ve kazalardan dolayı gibi şeyler için nokta ve hat tabanlı verilerden oluşur.
- Akış verileri-yollar üzerindeki trafik akışı hakkında ölçümler sağlar. Genellikle trafik akışı verileri, yolların renklendirmek için kullanılır. Renkler, akışı yavaşlatan, hız sınırına veya başka bir ölçüye göre ne kadar trafik altına göre belirlenir. Azure haritalar 'daki trafik akışı verileri üç farklı ölçüm ölçümüne sahiptir:
    - `relative`-yolun ücretsiz akış hızına göre değişir.
    - `absolute`-yolda tüm taşıtlardan oluşan mutlak hızdır.
    - `relative-delay`-beklenen ortalama gecikmeden daha yavaş olan bölgeleri görüntüler.

Aşağıdaki kod, Haritada trafik verilerinin nasıl görüntüleneceğini gösterir.

```javascript
//Show traffic on the map using the traffic options.
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

Aşağıda, yukarıdaki işlevselliğin tamamen çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Haritada trafiği gösterme' src='//codepen.io/azuremaps/embed/WMLRPw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz <a href='https://codepen.io'>. kaleme</a>Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>bir haritadaki trafiği gösterme</a> .
</iframe>

## <a name="traffic-overlay-options"></a>Trafik kaplama seçenekleri

Aşağıdaki araç, işleme değişikliğini görmek için farklı trafik kaplama ayarları arasında geçiş yapmanızı sağlar. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Trafik kaplama seçenekleri" src="//codepen.io/azuremaps/embed/RwbPqRY/?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/RwbPqRY/'>trafiği kaplama seçeneklerine</a> bakın.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [TrafficOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.trafficoptions)

Kullanıcı deneyimlerinizi geliştirin:

> [!div class="nextstepaction"]
> [Fare olayları ile harita etkileşimi](map-events.md)

> [!div class="nextstepaction"]
> [Erişilebilir eşleme oluşturma](map-accessibility.md)

> [!div class="nextstepaction"]
> [Kod örnek sayfası](https://aka.ms/AzureMapsSamples)
