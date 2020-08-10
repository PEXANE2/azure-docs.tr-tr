---
title: Bir haritada trafiği göster | Microsoft Azure haritaları
description: Haritalar 'a trafik verileri ekleme hakkında bilgi edinin. Akış verileri hakkında bilgi edinin ve haritalara olay verileri ve akış verileri eklemek için Azure Maps web SDK 'sını kullanma konusuna bakın.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-javascript
ms.openlocfilehash: 063fbd2ad4f2f5d427fd2cb39b8ce9b231eba374
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88036439"
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

<iframe height='500' scrolling='no' title='Haritada trafiği gösterme' src='//codepen.io/azuremaps/embed/WMLRPw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. kaleme Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>bir haritadaki trafiği gösterme</a> <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

## <a name="traffic-overlay-options"></a>Trafik kaplama seçenekleri

Aşağıdaki araç, işleme değişikliğini görmek için farklı trafik kaplama ayarları arasında geçiş yapmanızı sağlar. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Trafik kaplama seçenekleri" src="//codepen.io/azuremaps/embed/RwbPqRY/?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Codepen 'da Azure Maps () tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/RwbPqRY/'>trafiği kaplama seçeneklerine</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
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
