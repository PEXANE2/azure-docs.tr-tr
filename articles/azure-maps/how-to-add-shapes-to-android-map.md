---
title: Android Maps 'e çokgen katmanı ekleme | Microsoft Azure haritaları
description: Haritalara çokgenler veya daireler eklemeyi öğrenin. Geometrik şekilleri özelleştirmek ve bunların güncelleştirilmesini ve bakımını kolaylaştırmak için Azure Haritalar Android SDK kullanma konusuna bakın.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 1712cedab9cef23108fcc48b8e09bdc3e33065c4
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97679485"
---
# <a name="add-a-polygon-layer-to-the-map-android-sdk"></a>Haritaya çokgen katmanı ekleme (Android SDK)

Bu makalede, `Polygon` `MultiPolygon` bir çokgen katmanı kullanarak haritada ve özellik geometrileri arasındaki alanların nasıl işleneceğini gösterir.

## <a name="prerequisites"></a>Ön koşullar

[Hızlı başlangıç: Android uygulama belgesi oluşturma](quick-android-map.md) ' daki adımları tamamladığınızdan emin olun. Bu makaledeki kod blokları haritalar `onReady` olay işleyicisine eklenebilir.

## <a name="use-a-polygon-layer"></a>Çokgen katmanı kullanma

Bir çokgen katmanı bir veri kaynağına bağlıyken ve haritada yüklendiğinde, alanı `Polygon` ve `MultiPolygon` özellikleri işler. Bir çokgen oluşturmak için bir veri kaynağına ekleyin ve sınıfı kullanarak bir Çokgen katman ile oluşturun `PolygonLayer` .

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source, 
    fillColor("red"),
    fillOpacity(0.7f)
), "labels");
```

Aşağıdaki ekran görüntüsünde, bir çokgen katmanı kullanarak bir çokgen alanını işleyen Yukarıdaki kod gösterilmektedir.

![Fill alanı işlenmiş olan Çokgen](media/how-to-add-shapes-to-android-map/android-polygon-layer.png)

## <a name="use-a-polygon-and-line-layer-together"></a>Bir çokgen ve çizgi katmanını birlikte kullanma

Çokgenler ana hattını işlemek için çizgi katmanı kullanılır. Aşağıdaki kod örneği, önceki örnekte olduğu gibi bir çokgen oluşturur, ancak şimdi bir çizgi katmanı ekler. Bu çizgi katmanı, veri kaynağına bağlı ikinci bir katmandır.  

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source,
    fillColor("rgba(0, 200, 200, 0.5)")
), "labels");

//Create and add a line layer to render the outline of the polygon.
map.layers.add(new LineLayer(source,
    strokeColor("red"),
    strokeWidth(2f)
));
```

Aşağıdaki ekran görüntüsünde, bir çizgi katmanı kullanılarak oluşturulan ana hattı ile bir çokgen işlenen Yukarıdaki kod gösterilmektedir.

![Kendi Fill alanı ve ana hattı işlenmiş Çokgen](media/how-to-add-shapes-to-android-map/android-polygon-and-line-layer.png)

> [!TIP]
> Bir çizgi katmanıyla bir çokgen ana hattını oluştururken, her bir nokta dizisinin aynı başlangıç ve bitiş noktasına sahip olması gibi çokgenler içindeki tüm halkaları kapatmayı unutmayın. Bu yapılmazsa, çizgi katmanı, çokgenin son noktasını ilk noktaya bağlayamayabilir.

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](android-map-add-line-layer.md)
