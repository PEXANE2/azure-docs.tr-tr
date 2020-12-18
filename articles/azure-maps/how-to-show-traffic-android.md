---
title: Android haritalar üzerinde trafik verilerini göster | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Android SDK kullanarak bir Haritada trafik verileri görüntüleme hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 12/04/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 113f39ac2976b870c9e07851cdd0919e2578940f
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97680466"
---
# <a name="show-traffic-data-on-the-map-android-sdk"></a>Haritadaki trafik verilerini göster (Android SDK)

Akış verileri ve olay verileri, haritada görüntülenebilen iki trafik verisi türüdür. Bu kılavuzda her iki trafik verisi türünün nasıl görüntüleneceği gösterilmektedir. Olaylar verileri, kurulumlarını, yol kapanışları ve kazalardan dolayı gibi şeyler için nokta ve hat tabanlı verilerden oluşur. Akış verileri, yolda trafik akışı hakkında ölçümleri gösterir.

## <a name="prerequisites"></a>Ön koşullar

[Hızlı başlangıç: Android uygulama belgesi oluşturma](quick-android-map.md) ' daki adımları tamamladığınızdan emin olun. Bu makaledeki kod blokları haritalar `onReady` olay işleyicisine eklenebilir.

## <a name="show-traffic-on-the-map"></a>Haritada trafiği göster

Azure haritalar 'da kullanılabilen iki tür trafik verisi vardır:

- Olay verileri-oluşturma, yol kapanışları ve kazalardan dolayı gibi şeyler için nokta ve hat tabanlı verilerden oluşur.
- Akış verileri-yollar üzerindeki trafik akışı hakkında ölçümler sağlar. Genellikle trafik akışı verileri, yolların renklendirmek için kullanılır. Renkler, akışı yavaşlatan, hız sınırına veya başka bir ölçüye göre ne kadar trafik altına göre belirlenir. Haritanın trafik seçeneğine geçirilebilecek dört değer vardır `flow` .

    |Akış değeri | Açıklama|
    | :-- | :-- |
    | TrafficFlow. NONE | Harita üzerinde trafik verilerini görüntülemiyor |
    | TrafficFlow. GÖRELI | Yolun serbest akış hızına göre trafik verilerini gösterir |
    | TrafficFlow.RELATIVE_DELAY | Beklenen ortalama gecikmeden daha yavaş olan bölgeleri görüntüler |
    | TrafficFlow. ABSOLUTE | Yolda tüm taşıtın mutlak hızını gösterir |

Aşağıdaki kod, Haritada trafik verilerinin nasıl görüntüleneceğini gösterir.

```java
//Show traffic on the map using the traffic options.
map.setTraffic(
    incidents(true),
    flow(TrafficFlow.RELATIVE)
);
```

Aşağıdaki ekran görüntüsünde, haritada gerçek zamanlı trafik bilgilerinin bulunduğu Yukarıdaki kod gösterilmektedir.

![Gerçek zamanlı trafik bilgilerini gösteren harita](media/how-to-show-traffic-android/android-show-traffic.png)

## <a name="get-traffic-incident-details"></a>Trafik olay ayrıntılarını al

Bir trafik olayıyla ilgili ayrıntılar, haritada olayı göstermek için kullanılan özelliğin özellikleri içinde mevcuttur. Trafik olayları, Azure Maps Trafik olayı vektör kutucuk hizmeti kullanılarak haritaya eklenir. [Burada belgelendiği](https://developer.tomtom.com/traffic-api/traffic-api-documentation-traffic-incidents/vector-incident-tiles)durumlarda bu vektör kutucuklarında verilerin biçimi. Aşağıdaki kod, haritaya bir tıklama olayı ekler ve tıklanan trafik olayı özelliğini alır ve bazı ayrıntıların bulunduğu bir bildirim iletisi görüntüler.

```java
//Show traffic information on the map.
map.setTraffic(
    incidents(true),
    flow(TrafficFlow.RELATIVE)
);

//Add a click event to the map.
map.events.add((OnFeatureClick) (features) -> {

    if (features != null && features.size() > 0) {
        Feature incident = features.get(0);

        //Ensure that the clicked feature is an traffic incident feature.
        if (incident.properties() != null && incident.hasProperty("incidentType")) {

            StringBuilder sb = new StringBuilder();
            String incidentType = incident.getStringProperty("incidentType");

            if (incidentType != null) {
                sb.append(incidentType);
            }

            if (sb.length() > 0) {
                sb.append("\n");
            }

            //If the road is closed, find out where it is closed from.
            if ("Road Closed".equals(incidentType)) {
                String from = incident.getStringProperty("from");

                if (from != null) {
                    sb.append(from);
                }
            } else {
                //Get the description of the traffic incident.
                String description = incident.getStringProperty("description");

                if (description != null) {
                    sb.append(description);
                }
            }

            String message = sb.toString();

            if (message.length() > 0) {
                Toast.makeText(this, message, Toast.LENGTH_LONG).show();
            }
        }
    }
});
```

Aşağıdaki ekran görüntüsünde, olay ayrıntılarını görüntüleyen bir bildirim iletisi ile haritada gerçek zamanlı trafik bilgilerinin bulunduğu Yukarıdaki kod gösterilmektedir.

![Olay ayrıntılarını görüntüleyen bir bildirim iletisi ile gerçek zamanlı trafik bilgilerini gösteren harita](media/how-to-show-traffic-android/android-traffic-details.png)

## <a name="next-steps"></a>Sonraki adımlar

Haritanızda daha fazla veri eklemeyi öğrenmek için aşağıdaki kılavuzlara bakın:

> [!div class="nextstepaction"]
> [Kutucuk katmanı ekleme](how-to-add-tile-layer-android-map.md)
