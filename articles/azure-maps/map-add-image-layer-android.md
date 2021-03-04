---
title: Android haritasına görüntü katmanı ekleme | Microsoft Azure haritaları
description: Bir haritaya görüntü eklemeyi öğrenin. Görüntü katmanlarını özelleştirmek ve sabit koordinat kümelerinde resimleri kaplama halinde görüntülemek için bkz. Azure Maps Android SDK nasıl kullanılır.
author: rbrundritt
ms.author: richbrun
ms.date: 02/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: e1d99297c0357039606149bdf7e5a526258fc7c5
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102054944"
---
# <a name="add-an-image-layer-to-a-map-android-sdk"></a>Haritaya görüntü katmanı ekleme (Android SDK)

Bu makalede bir görüntünün sabit bir koordinat kümesiyle nasıl kaplama yapılacağı gösterilir. Haritalar üzerinde kaplama olabilecek farklı görüntü türlerine birkaç örnek aşağıda verilmiştir:

* Dronlarla 'den yakalanan görüntüler
* Floorplanlar oluşturma
* Geçmiş veya diğer özel harita görüntüleri
* İş sitelerinin şemaları

> [!TIP]
> Görüntü katmanı, bir harita üzerindeki görüntüyü kaplama için kolay bir yoldur. Büyük görüntülerin çok fazla bellek tükettiği ve potansiyel olarak performans sorunlarına neden olabileceğini unutmayın. Bu durumda, görüntünüzü kutucuklara bölmek ve bir kutucuk katmanı olarak haritaya yüklemek için göz önünde bulundurun.

## <a name="add-an-image-layer"></a>Görüntü katmanı ekleme

Aşağıdaki kod, haritadaki [1922 ' den bir Newark, New Jersey haritasının](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) bir görüntüsünü yer paylaşımlı olarak yer alır. Bu görüntü, `drawable` projenin klasörüne eklenir. Görüntü katmanı, biçimdeki dört köşelerin görüntüsü ve koordinatları ayarlanarak oluşturulur `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]` . Katmanın altına görüntü katmanları eklemek `label` çoğunlukla tercih edilir.

::: zone pivot="programming-language-java-android"

```java
//Create an image layer.
ImageLayer layer = new ImageLayer(
    imageCoordinates(
        new Position[] {
            new Position(-74.22655, 40.773941), //Top Left Corner
            new Position(-74.12544, 40.773941), //Top Right Corner
            new Position(-74.12544, 40.712216), //Bottom Right Corner
            new Position(-74.22655, 40.712216)  //Bottom Left Corner
        }
    ),
    setImage(R.drawable.newark_nj_1922)
);

//Add the image layer to the map, below the label layer.
map.layers.add(layer, "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create an image layer.
val layer = ImageLayer(
    imageCoordinates(
        arrayOf<Position>(
            Position(-74.22655, 40.773941),  //Top Left Corner
            Position(-74.12544, 40.773941),  //Top Right Corner
            Position(-74.12544, 40.712216),  //Bottom Right Corner
            Position(-74.22655, 40.712216)   //Bottom Left Corner
        )
    ),
    setImage(R.drawable.newark_nj_1922)
)

//Add the image layer to the map, below the label layer.
map.layers.add(layer, "labels")
```

::: zone-end

Alternatif olarak, çevrimiçi ortamda barındırılan bir görüntünün URL 'SI de belirtilebilir. Ancak senaryonuz izin veriyorsa, görüntü `drawable` yerel olarak kullanılabilir olacağı ve indirilmeyeceği için daha hızlı yükleneceği için bu görüntüyü projeler klasörünüze ekleyin.

::: zone pivot="programming-language-java-android"

```java
//Create an image layer.
ImageLayer layer = new ImageLayer(
    imageCoordinates(
        new Position[] {
            new Position(-74.22655, 40.773941), //Top Left Corner
            new Position(-74.12544, 40.773941), //Top Right Corner
            new Position(-74.12544, 40.712216), //Bottom Right Corner
            new Position(-74.22655, 40.712216)  //Bottom Left Corner
        }
    ),
    setUrl("https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg")
);

//Add the image layer to the map, below the label layer.
map.layers.add(layer, "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create an image layer.
val layer = ImageLayer(
    imageCoordinates(
        arrayOf<Position>(
            Position(-74.22655, 40.773941),  //Top Left Corner
            Position(-74.12544, 40.773941),  //Top Right Corner
            Position(-74.12544, 40.712216),  //Bottom Right Corner
            Position(-74.22655, 40.712216) //Bottom Left Corner
        )
    ),
    setUrl("https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg")
)

//Add the image layer to the map, below the label layer.
map.layers.add(layer, "labels")
```

::: zone-end

Aşağıdaki ekran görüntüsünde, bir görüntü katmanını kullanarak 1922 arasında kaplama olan Newark, New Jersey bir harita gösterilmektedir.

![Bir görüntü katmanı kullanarak 1922 arasında yer olan Newark, New Jersey Haritası](media/map-add-image-layer-android/android-image-layer.gif)

## <a name="import-a-kml-file-as-ground-overlay"></a>KML dosyasını arka plan kaplaması olarak içeri aktarma

Bu örnek, KML zemin kaplama bilgilerinin haritada görüntü katmanı olarak nasıl ekleneceğini gösterir. KML zemin Yerpaylaşımları, Kuzey, Güney, Doğu ve Batı koordinatları ve saat yönünde bir döndürme sağlar. Ancak, görüntü katmanı görüntünün her köşesi için koordinatları bekler. Bu örnekteki KML zemin kaplaması, Chartres Cathedral içindir ve [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml)'dan kaynaklıdır.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<kml xmlns="http://www.opengis.net/kml/2.2" xmlns:gx="http://www.google.com/kml/ext/2.2" xmlns:kml="http://www.opengis.net/kml/2.2" xmlns:atom="http://www.w3.org/2005/Atom">
<GroundOverlay>
    <name>Map of Chartres cathedral</name>
    <Icon>
        <href>https://upload.wikimedia.org/wikipedia/commons/thumb/e/e3/Chartres.svg/1600px-Chartres.svg.png</href>
        <viewBoundScale>0.75</viewBoundScale>
    </Icon>
    <LatLonBox>
        <north>48.44820923628113</north>
        <south>48.44737203258976</south>
        <east>1.488833825534365</east>
        <west>1.486788581643038</west>
        <rotation>46.44067597839695</rotation>
    </LatLonBox>
</GroundOverlay>
</kml>
```

Kod sınıfından statik yöntemi kullanır `getCoordinatesFromEdges` `ImageLayer` . Bu yöntem, KML zemin kaplamanın Kuzey, Güney, Doğu, Batı ve döndürme bilgilerini kullanarak görüntünün dört köşesini hesaplar.

::: zone pivot="programming-language-java-android"

```java
//Calculate the corner coordinates of the ground overlay.
Position[] corners = ImageLayer.getCoordinatesFromEdges(
    //North, south, east, west
    48.44820923628113, 48.44737203258976, 1.488833825534365, 1.486788581643038,

    //KML rotations are counter-clockwise, subtract from 360 to make them clockwise.
    360 - 46.44067597839695
);

//Create an image layer.
ImageLayer layer = new ImageLayer(
    imageCoordinates(corners),
    setUrl("https://upload.wikimedia.org/wikipedia/commons/thumb/e/e3/Chartres.svg/1600px-Chartres.svg.png")
);

//Add the image layer to the map, below the label layer.
map.layers.add(layer, "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Calculate the corner coordinates of the ground overlay.
val corners: Array<Position> =
    ImageLayer.getCoordinatesFromEdges( //North, south, east, west
        48.44820923628113,
        48.44737203258976,
        1.488833825534365,
        1.486788581643038,  //KML rotations are counter-clockwise, subtract from 360 to make them clockwise.
        360 - 46.44067597839695
    )

//Create an image layer.
val layer = ImageLayer(
    imageCoordinates(corners),
    setUrl("https://upload.wikimedia.org/wikipedia/commons/thumb/e/e3/Chartres.svg/1600px-Chartres.svg.png")
)

//Add the image layer to the map, below the label layer.
map.layers.add(layer, "labels")
```

::: zone-end

Aşağıdaki ekran görüntüsünde, bir görüntü katmanını kullanarak yer kaplayan KML zemin kaplaması içeren bir harita gösterilmektedir.

![Görüntü katmanını kullanarak yer kaplayan bir KML zemin kaplaması ile eşleme](media/map-add-image-layer-android/android-ground-overlay.jpg)

> [!TIP]
> `getPixels` `getPositions` Konumlandırılmış görüntü katmanının coğrafi koordinatları ve yerel resim piksel koordinatları arasında dönüştürme yapmak için görüntü katmanı sınıfının ve yöntemlerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Haritada görüntü kaplama yolları hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın.

> [!div class="nextstepaction"]
> [Kutucuk katmanı ekleme](how-to-add-tile-layer-android-map.md)