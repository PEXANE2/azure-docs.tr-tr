---
title: Android Maps 'e sembol katmanı ekleme | Microsoft Azure haritaları
description: Haritaya işaret eklemeyi öğrenin. Bir veri kaynağından gelen nokta tabanlı verileri içeren bir sembol katmanı eklemek için Azure Maps Android SDK kullanan bir örneğe bakın.
author: rbrundritt
ms.author: richbrun
ms.date: 2/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: edb758469a06dcb7914025ea449b9d952e939533
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097219"
---
# <a name="add-a-symbol-layer-android-sdk"></a>Sembol katmanı ekleme (Android SDK)

Bu makalede, Azure Maps Android SDK kullanarak bir harita üzerinde sembol katmanı olarak bir veri kaynağındaki nokta verilerinin nasıl işleneceğini gösterir. Sembol katmanları eşleme üzerinde bir görüntü ve metin olarak işleme noktaları.

> [!TIP]
> Sembol katmanları varsayılan olarak, bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Katmanı yalnızca nokta geometrisi özellikleri olacak şekilde sınırlamak için `filter` katmanın seçeneğini olarak ayarlayın `eq(geometryType(), "Point")` . MultiPoint özelliklerini de dahil etmek istiyorsanız `filter` katmanın seçeneğini olarak ayarlayın `any(eq(geometryType(), "Point"), eq(geometryType(), "MultiPoint"))` .

## <a name="prerequisites"></a>Önkoşullar

[Hızlı başlangıç: Android uygulama belgesi oluşturma](quick-android-map.md) ' daki adımları tamamladığınızdan emin olun. Bu makaledeki kod blokları haritalar `onReady` olay işleyicisine eklenebilir.

## <a name="add-a-symbol-layer"></a>Sembol katmanı ekleme

Haritaya bir sembol katmanı ekleyebilmeniz için önce birkaç adım gerçekleştirmeniz gerekir. İlk olarak, bir veri kaynağı oluşturun ve haritaya ekleyin. Bir sembol katmanı oluşturun. Ardından veri kaynağından veri almak için veri kaynağını sembol katmanına geçirin. Son olarak, veri kaynağına veri ekleyerek işlenecek bir şey olması gerekir.

Aşağıdaki kod, yüklendikten sonra haritaya ne ekleneceğini gösterir. Bu örnek, bir sembol katmanını kullanarak haritada tek bir nokta oluşturur.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point and add it to the data source.
source.add(Point.fromLngLat(0, 0));

//Create a symbol layer to render icons and/or text at points on the map.
SymbolLayer layer = new SymbolLayer(source);

//Add the layer to the map.
map.layers.add(layer);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a point and add it to the data source.
source.add(Point.fromLngLat(0, 0))

//Create a symbol layer to render icons and/or text at points on the map.
val layer = SymbolLayer(source)

//Add the layer to the map.
map.layers.add(layer)
```

::: zone-end

Haritaya eklenebilecek üç farklı tür Point verisi vardır:

- GeoJSON nokta geometrisi-bu nesne yalnızca bir noktanın koordinatını içerir ve başka hiçbir şey yapmaz. `Point.fromLngLat`Statik yöntem bu nesneleri kolayca oluşturmak için kullanılabilir.
- GeoJSON MultiPoint geometrisi-bu nesne birden çok noktanın koordinatlarını içerir ve başka hiçbir şey yapmaz. `MultiPoint`Bu nesneleri oluşturmak için sınıfa bir dizi işaret geçirin.
- GeoJSON özelliği-bu nesne, tüm GeoJSON geometrisinden ve geometri ile ilişkili meta verileri içeren bir özellik kümesinden oluşur.

Daha fazla bilgi için bkz. veri oluşturma ve haritaya veri ekleme hakkında [veri kaynağı belgesi oluşturma](create-data-source-android-sdk.md) .

Aşağıdaki kod örneği bir GeoJSON noktası geometrisi oluşturur ve bunu GeoJSON özelliğine geçirir ve `title` özelliklerine eklenmiş bir değer içerir. `title`Özelliği, haritada sembol simgesinin üstünde metin olarak görüntülenir.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(0, 0));

//Add a property to the feature.
feature.addStringProperty("title", "Hello World!");

//Add the feature to the data source.
source.add(feature);

//Create a symbol layer to render icons and/or text at points on the map.
SymbolLayer layer = new SymbolLayer(source, 
    //Get the title property of the feature and display it on the map.
    textField(get("title"))
);

//Add the layer to the map.
map.layers.add(layer);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a point feature.
val feature = Feature.fromGeometry(Point.fromLngLat(0, 0))

//Add a property to the feature.
feature.addStringProperty("title", "Hello World!")

//Add the feature to the data source.
source.add(feature)

//Create a symbol layer to render icons and/or text at points on the map.
val layer = SymbolLayer(
    source,  //Get the title property of the feature and display it on the map.
    textField(get("title"))
)

//Add the layer to the map.
map.layers.add(layer)
```

::: zone-end

Aşağıdaki ekran görüntüsünde, simge katmanını içeren bir simge ve metin etiketi kullanan Yukarıdaki kod bir nokta özelliğini gösterir.

![Nokta özelliği için bir simge ve metin etiketi görüntüleyen bir sembol katmanı kullanılarak işlenen nokta ile eşleme](media/how-to-add-symbol-to-android-map/android-map-pin.png)

> [!TIP]
> Varsayılan olarak, sembol katmanları örtüşen sembolleri gizleyerek sembol işlemesini iyileştirir. Yakınlaştırma sırasında gizli simgeler görünür hale gelir. Bu özelliği devre dışı bırakmak ve tüm sembolleri her zaman işlemek için, `iconAllowOverlap` ve `textAllowOverlap` seçeneklerini olarak ayarlayın `true` .

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Sembol katmanına özel simge ekleme

Sembol katmanları WebGL kullanılarak işlenir. Bu nedenle, simge görüntüleri gibi tüm kaynakların WebGL bağlamına yüklenmesi gerekir. Bu örnek, harita kaynaklarına nasıl özel bir simge ekleneceğini gösterir. Bu simge daha sonra haritada özel bir sembol ile nokta verilerini işlemek için kullanılır. `textField`Sembol katmanının özelliği bir ifadenin belirtilmesini gerektirir. Bu durumda, sıcaklık özelliğini işlemek istiyoruz. Sıcaklık bir sayı olduğundan, bir dizeye dönüştürülmesi gerekir. Ayrıca buna "°F" eklemek istiyoruz. Bu birleştirme yapmak için bir ifade kullanılabilir; `concat(Expression.toString(get("temperature")), literal("°F"))`.

::: zone pivot="programming-language-java-android"

```java
//Load a custom icon image into the image sprite of the map.
map.images.add("my-custom-icon", R.drawable.showers);

//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(-73.985708, 40.75773));

//Add a property to the feature.
feature.addNumberProperty("temperature", 64);

//Add the feature to the data source.
source.add(feature);

//Create a symbol layer to render icons and/or text at points on the map.
SymbolLayer layer = new SymbolLayer(source,
    iconImage("my-custom-icon"),
    iconSize(0.5f),

    //Get the title property of the feature and display it on the map.
    textField(concat(Expression.toString(get("temperature")), literal("°F"))),
    textOffset(new Float[]{0f, -1.5f})
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Load a custom icon image into the image sprite of the map.
map.images.add("my-custom-icon", R.drawable.showers)

//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a point feature.
val feature = Feature.fromGeometry(Point.fromLngLat(-73.985708, 40.75773))

//Add a property to the feature.
feature.addNumberProperty("temperature", 64)

//Add the feature to the data source.
source.add(feature)

//Create a symbol layer to render icons and/or text at points on the map.
val layer = SymbolLayer(
    source,
    iconImage("my-custom-icon"),
    iconSize(0.5f),  //Get the title property of the feature and display it on the map.
    textField(concat(Expression.toString(get("temperature")), literal("°F"))),
    textOffset(arrayOf(0f, -1.5f))
)
```

::: zone-end

Bu örnekte, aşağıdaki görüntü uygulamanın çizilebilir klasörüne yüklenmiştir.

| ![Hava durumu simgesi yağmur göster](media/how-to-add-symbol-to-android-map/showers.png)|
|:-----------------------------------------------------------------------:|
| showers.png                                                  |

Aşağıdaki ekran görüntüsünde, bir simge katmanıyla birlikte özel bir simge ve biçimlendirilen metin etiketi kullanarak bir nokta özelliğini yeniden izleyen kod gösterilmektedir.

![Bir nokta özelliği için özel bir simge ve biçimlendirilen metin etiketi görüntüleyen bir sembol katmanı kullanılarak işlenen nokta ile eşleme](media/how-to-add-symbol-to-android-map/android-custom-symbol-layer.png)

> [!TIP]
> Yalnızca bir sembol katmanıyla birlikte metin işlemek istediğinizde, `iconImage` simge seçeneklerinin özelliğini olarak ayarlayarak simgeyi gizleyebilirsiniz `"none"` .

## <a name="modify-symbol-colors"></a>Sembol renklerini değiştir

Azure Haritalar Android SDK, varsayılan işaretleyici simgenin önceden tanımlanmış renk çeşitlemeleri kümesiyle birlikte gelir. Örneğin, `marker-red` `iconImage` Bu katmanda işaret simgesinin kırmızı bir sürümünü işlemek için bir sembol katmanının seçeneğine geçirilebilir.

::: zone pivot="programming-language-java-android"

```java
SymbolLayer layer = new SymbolLayer(source,
    iconImage("marker-red")
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = SymbolLayer(source,
    iconImage("marker-red")
)
```

::: zone-end

Aşağıdaki tabloda, kullanılabilir tüm yerleşik simge görüntüsü adları listelenmiştir. Bu işaretçilerin hepsi, renkleri geçersiz kılabileceğiniz renk kaynaklarından çeker. Bu işaretin ana Fill rengini geçersiz kılmanın yanı sıra. Ancak, bu işaretçilerin birinin rengini geçersiz kılma söz konusu simge görüntüsünü kullanan tüm katmanlara uygulanacağını unutmayın.

| Simge resmi adı | Renk kaynağı adı |
|-----------------|---------------------|
| `marker-default` | `mapcontrol_marker_default` |
| `marker-black` | `mapcontrol_marker_black` |
| `marker-blue` | `mapcontrol_marker_blue` |
| `marker-darkblue` | `mapcontrol_marker_darkblue` |
| `marker-red` | `mapcontrol_marker_red` |
| `marker-yellow` | `mapcontrol_marker_yellow` |

Ayrıca, renk kaynağı adını kullanarak tüm işaretlerin kenarlık rengini geçersiz kılabilirsiniz `mapcontrol_marker_border` . Bu işaretçilerin renkleri, uygulamanızın dosyasında aynı ada sahip bir renk eklenerek geçersiz kılınabilir `colors.xml` . Örneğin, aşağıdaki `colors.xml` dosya varsayılan işaret rengini parlak yeşil hale getirir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="mapcontrol_marker_default">#00FF00</color>
</resources>
```

Varsayılan işaretleyici vektör XML 'in, varsayılan işaretin ek özel sürümlerini oluşturmak için değiştirebileceğiniz değiştirilmiş bir sürümü aşağıda verilmiştir. Değiştirilen sürüm, `drawable` uygulamanızın klasörüne eklenebilir ve kullanarak haritalar görüntüsünü Sprite öğesine eklenebilir ve `map.images.add` ardından bir sembol katmanıyla birlikte kullanılır.

```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="24.5dp"
    android:height="36.5dp"
    android:viewportWidth="24.5"
    android:viewportHeight="36.5">
    <path
        android:pathData="M12.25,0.25a12.2543,12.2543 0,0 0,-12 12.4937c0,6.4436 6.4879,12.1093 11.059,22.5641 0.5493,1.2563 1.3327,1.2563 1.882,0C17.7621,24.8529 24.25,19.1857 24.25,12.7437A12.2543,12.2543 0,0 0,12.25 0.25Z"
        android:strokeWidth="0.5"
        android:fillColor="@color/mapcontrol_marker_default"
        android:strokeColor="@color/mapcontrol_marker_border"/>
</vector>
```

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Özellik bilgilerini görüntüleme](display-feature-information-android.md)
