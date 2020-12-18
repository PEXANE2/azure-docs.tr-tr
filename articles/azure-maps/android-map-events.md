---
title: Android haritalar 'da harita olaylarını işleme | Microsoft Azure haritaları
description: Kullanıcılar eşlemelerle etkileşim kurarken hangi olayların tetiklediği hakkında bilgi edinin. Desteklenen tüm harita olaylarının listesini görüntüleyin. Olayları işlemek için Azure Maps Android SDK kullanma konusuna bakın.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 818d33947fa079a130c3009a34dcb9b72ad52f91
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681669"
---
# <a name="interact-with-the-map-android-sdk"></a>Eşlemele etkileşim kurma (Android SDK)

Bu makalede haritalar olayları yöneticisini nasıl kullanabileceğiniz gösterilmektedir.

## <a name="interact-with-the-map"></a>Harita ile etkileşim kurma

Map, tüm olaylarını özelliği aracılığıyla yönetir `events` . Aşağıdaki tabloda desteklenen tüm harita olayları listelenmektedir.

| Olay                  | Olay işleyicisi biçimi | Açıklama |
|------------------------|----------------------|-------------|
| `OnCameraIdle`         | `()`                 | <p>Eşleme "boşta" durumuna girmeden önce oluşturulan son çerçeveden sonra harekete geçirilir:<ul><li>Devam eden bir kamera geçişi yok.</li><li>Şu anda istenen tüm kutucuklar yüklendi.</li><li>Tüm belirme/geçiş animasyonları tamamlandı.</li></ul></p> |
| `OnCameraMove`         | `()`                 | Bir görünümden diğerine animasyon eklenmiş bir geçiş sırasında, Kullanıcı etkileşimi ya da yöntemlerinin sonucu olarak tekrar tekrar tetiklenir. |
| `OnCameraMoveCanceled` | `()`                 | Kameraya bir taşıma isteği iptal edildiğinde harekete geçirilir. |
| `OnCameraMoveStarted`  | `(int reason)`       | Eşleme, Kullanıcı etkileşimi ya da yöntemlerinin sonucu olarak bir görünümden diğerine geçiş yapmaya başlamadan hemen önce tetiklenir. `reason`Olay dinleyicisinin bağımsız değişkeni, kamera hareketinin nasıl başlatılmasının ayrıntılarını sağlayan bir tamsayı değeri döndürür. Olası nedenlerinin listesi aşağıda verilmiştir:<ul><li>1: hareket</li><li>2: Geliştirici animasyonu</li><li>3: API animasyonu</li></ul>   |
| `OnClick`              | `(double lat, double lon)` | Eşleme basıldığında ve haritada aynı noktada bırakıldığında harekete geçirilir. |
| `OnFeatureClick`       | `(List<Feature>)`    | Eşleme basıldığında harekete geçirilir ve bir özellikte aynı noktada serbest bırakılır.  |
| `OnLongClick`          | `(double lat, double lon)` | Eşleme basıldığında harekete geçirilir, bir süre tutulur ve sonra haritada aynı noktada serbest bırakılır. |
| `OnLongFeatureClick `  | `(List<Feature>)`    | Eşleme basıldığında harekete geçirilir, bir süre tutulur ve sonra bir özellikte aynı noktada serbest bırakılır. |
| `OnReady`              | `(AzureMap map)`     | Eşleme başlangıçta yüklendiğinde veya uygulama yönü değiştiğinde ve gereken en az sayıda eşleme kaynağı yüklendiğinde ve eşleme, program aracılığıyla ile etkileşim için hazırlanmaya hazırsa tetiklenir. |

Aşağıdaki kod,, `OnClick` `OnFeatureClick` ve olaylarının haritaya nasıl ekleneceğini gösterir `OnCameraMove` .

```java
map.events.add((OnClick) (lat, lon) -> {
    //Map clicked.
});

map.events.add((OnFeatureClick) (features) -> {
    //Feature clicked.
});

map.events.add((OnCameraMove) () -> {
    //Map camera moved.
});
```

Daha fazla bilgi için harita ve tetikleme olaylarıyla nasıl etkileşim kuracağınızı öğrenmek için [harita](how-to-use-android-map-control-library.md#navigating-the-map) belgelerine bakın.

## <a name="scope-feature-events-to-layer"></a>Katman özelliği olaylarını katmana kapsama

`OnFeatureClick`Veya `OnLongFeatureClick` olaylarını haritaya eklerken, BIR katman kimliği ikinci bir parametre olarak geçirilebilir. Bir katman KIMLIĞI geçirildiğinde, olay yalnızca bu katmanda olay gerçekleşirse harekete geçirilir. Katmanlara kapsamlı olaylar sembol, kabarcık, çizgi ve çokgen katmanları tarafından desteklenir.

```java
//Create a data source.
DataSource source = new DataSource();
map.sources.add(source);

//Add data to the data source.
source.add(Point.fromLngLat(0, 0));

//Create a layer and add it to the map.
BubbleLayer layer = new BubbleLayer(source);
map.layers.add(layer);

//Add a feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add((OnFeatureClick) (features) -> {
    //One or more features clicked.
}, layer.getId());

//Add a long feature click event to the map and pass the layer ID to limit the event to the specified layer.
map.events.add((OnLongFeatureClick) (features) -> {
    //One or more features long clicked.
}, layer.getId());
```

## <a name="next-steps"></a>Sonraki adımlar

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Haritada gezinme](how-to-use-android-map-control-library.md#navigating-the-map)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](how-to-add-shapes-to-android-map.md)
