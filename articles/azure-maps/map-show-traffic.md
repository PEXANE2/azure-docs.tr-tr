---
title: Haritada trafiği göster | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak trafik verilerinin haritaüzerinde nasıl görüntülenecek olduğunu öğreneceksiniz.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9bd5ae462013924a46a3da8400719e83abae3424
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534823"
---
# <a name="show-traffic-on-the-map"></a>Haritada trafiği göster

Azure Haritalar'da kullanılabilen iki tür trafik verisi vardır:

- Olay verileri - inşaat, yol kapatma ve kaza gibi şeyler için nokta ve çizgi tabanlı verilerden oluşur.
- Akış verileri - yollarda trafik akışı ölçümleri sağlar. Genellikle, trafik akışı verileri yolları renklendirmek için kullanılır. Renkler, hız sınırına veya başka bir metretmeye göre trafiğin akışı ne kadar yavaşlattığına bağlıdır. Azure Haritalar'daki trafik akışı verilerinin üç farklı ölçüm ölçümü vardır:
    - `relative`- yolun serbest akış hızına göredir.
    - `absolute`- yoldaki tüm araçların mutlak hızıdır.
    - `relative-delay`- beklenen ortalama gecikmeden daha yavaş alanları görüntüler.

Aşağıdaki kod, trafik verilerinin haritada nasıl görüntülenebildiğini gösterir.

```javascript
//Show traffic on the map using the traffic options.
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Haritada trafiği göster' src='//codepen.io/azuremaps/embed/WMLRPw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile haritadaki Kalem <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Göster trafiğini</a> görün.
</iframe>

## <a name="traffic-overlay-options"></a>Trafik bindirme seçenekleri

Aşağıdaki araç, işlemenin nasıl değiştiğini görmek için farklı trafik bindirme ayarları arasında geçiş yapmanızı sağlar. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Trafik bindirme seçenekleri" src="//codepen.io/azuremaps/embed/RwbPqRY/?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/RwbPqRY/'>Trafiği bindirme seçeneklerine</a> bakın .
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [Trafik Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.trafficoptions)

Kullanıcı deneyimlerinizi geliştirin:

> [!div class="nextstepaction"]
> [Fare olayları ile harita etkileşimi](map-events.md)

> [!div class="nextstepaction"]
> [Erişilebilir bir harita oluşturma](map-accessibility.md)

> [!div class="nextstepaction"]
> [Kod örnek sayfası](https://aka.ms/AzureMapsSamples)
