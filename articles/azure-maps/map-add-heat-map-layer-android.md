---
title: Android Maps 'a bir ısı haritası katmanı ekleme | Microsoft Azure haritaları
description: Isı haritası oluşturmayı öğrenin. Haritaya bir ısı haritası katmanı eklemek için bkz. Azure Mapsandroıd SDK 'sını kullanma. Isı haritası katmanlarını özelleştirmeyi öğrenin.
author: rbrundritt
ms.author: richbrun
ms.date: 02/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: fce2c2d007f92c43e763826f9345f773324e885e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102100194"
---
# <a name="add-a-heat-map-layer-android-sdk"></a>Isı haritası katmanı ekleme (Android SDK)

Nokta yoğunluğu haritaları olarak da bilinen ısı haritaları, veri görselleştirmesinin bir türüdür. Bunlar, bir aralıktaki renkler kullanılarak verilerin yoğunluğunu göstermek ve bir haritada "etkin noktalar" verilerini göstermek için kullanılırlar. Isı haritaları çok sayıda noktayla veri kümelerini işlemek için harika bir yoldur. 

Sembol, harita alanının çoğunu kapsayabilen on binlerce noktayı işleme. Bu durum büyük olasılıkla çakışan birçok sembol ile sonuçlanır. Verileri daha iyi anlamak daha zor hale gelir. Bununla birlikte, bir ısı haritası olarak aynı veri kümesini görselleştirmede, her bir veri noktasının yoğunluğunu ve göreli yoğunluğunu görmeyi kolaylaştırır.

Isı haritalarını birçok farklı senaryoda kullanabilirsiniz; örneğin:

- **Sıcaklık verileri**: sıcaklığın iki veri noktası arasında olduğu yaklaşık bir değer sağlar.
- **Gürültü sensörlerine yönelik veriler**: yalnızca sensörün olduğu gürültü şiddetini göstermez, ancak aynı zamanda bir mesafeden fazla bakış elde etmek için de bilgi sağlayabilir. Herhangi bir sitedeki gürültü düzeyi yüksek olmayabilir. Birden çok sensörden gürültü kapsam alanı çakışırsa, bu örtüşen alanın daha fazla gürültü düzeyiyle karşılaşması mümkündür. Bu nedenle örtüşen alan, ısı haritasında görünür olur.
- **GPS izleme**: her bir veri noktasının yoğunluğu hızlı temel alınarak ağırlıklı bir yükseklik eşlemesi olarak hızı içerir. Örneğin, bu işlev bir aracın hızlandırmakta olduğunu görmek için bir yol sağlar.

> [!TIP]
> Isı haritası katmanları varsayılan olarak bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Katmanı yalnızca nokta geometrisi özellikleri olacak şekilde sınırlamak için `filter` katmanın seçeneğini olarak ayarlayın `eq(geometryType(), "Point")` . MultiPoint özelliklerini de dahil etmek istiyorsanız `filter` katmanın seçeneğini olarak ayarlayın `any(eq(geometryType(), "Point"), eq(geometryType(), "MultiPoint"))` .

</br>

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Heat-Maps-and-Image-Overlays-in-Azure-Maps/player?format=ny]

## <a name="prerequisites"></a>Önkoşullar

[Hızlı başlangıç: Android uygulama belgesi oluşturma](quick-android-map.md) ' daki adımları tamamladığınızdan emin olun. Bu makaledeki kod blokları haritalar `onReady` olay işleyicisine eklenebilir.

## <a name="add-a-heat-map-layer"></a>Isı haritası katmanı ekleme

Bir veri kaynağını bir ısı haritası olarak işlemek için, veri kaynağınızı sınıfın bir örneğine geçirin `HeatMapLayer` ve haritaya ekleyin.

Aşağıdaki kod örneği, deprem 'in bir coğrafi JSON akışını son haftadan yükler ve bunları bir ısı haritası olarak işler. Her veri noktası, tüm yakınlaştırma düzeylerinde 10 piksellik bir yarıçap ile işlenir. Daha iyi bir kullanıcı deneyimi sağlamak için, ısı Haritası etiket katmanının altında olduğundan etiketlerin net bir şekilde görünür kalmasını sağlar. Bu örnekteki veriler, [USGS deprem Hazarlar programından](https://earthquake.usgs.gov/)yapılır. Bu örnek, [veri kaynağı oluşturma](create-data-source-android-sdk.md) belgesinde sunulan veri içeri aktarma yardımcı programı kod bloğunu kullanarak Web 'Den geojson verilerini yükler.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a heat map layer.
HeatMapLayer layer = new HeatMapLayer(source,
  heatmapRadius(10f),
  heatmapOpacity(0.8f)
);

//Add the layer to the map, below the labels.
map.layers.add(layer, "labels");

//Import the geojson data and add it to the data source.
Utils.importData("https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson",
    this,
    (String result) -> {
        //Parse the data as a GeoJSON Feature Collection.
        FeatureCollection fc = FeatureCollection.fromJson(result);

        //Add the feature collection to the data source.
        source.add(fc);

        //Optionally, update the maps camera to focus in on the data.

        //Calculate the bounding box of all the data in the Feature Collection.
        BoundingBox bbox = MapMath.fromData(fc);

        //Update the maps camera so it is focused on the data.
        map.setCamera(
            bounds(bbox),
            padding(20));
    });
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a heat map layer.
val layer = HeatMapLayer(
    source,
    heatmapRadius(10f),
    heatmapOpacity(0.8f)
)

//Add the layer to the map, below the labels.
map.layers.add(layer, "labels")

//Import the geojson data and add it to the data source.
Utils.importData("https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson",
    this
) { result: String? ->
    //Parse the data as a GeoJSON Feature Collection.
    val fc = FeatureCollection.fromJson(result!!)

    //Add the feature collection to the data source.
    source.add(fc)

    //Optionally, update the maps camera to focus in on the data.
    //Calculate the bounding box of all the data in the Feature Collection.
    val bbox = MapMath.fromData(fc)

    //Update the maps camera so it is focused on the data.
    map.setCamera(
        bounds(bbox),
        padding(20)
    )
}
```

::: zone-end

Aşağıdaki ekran görüntüsünde, yukarıdaki kodu kullanarak bir ısı haritası yükleyen bir harita gösterilmektedir.

![Son Deprem ısı haritası katmanıyla eşle](media/map-add-heat-map-layer-android/android-heat-map-layer.png)

## <a name="customize-the-heat-map-layer"></a>Isı haritası katmanını özelleştirme

Önceki örnek, yarıçap ve opaklık seçeneklerini ayarlayarak ısı haritasını özelleştirdi. Isı haritası katmanı, aşağıdakiler dahil olmak üzere çeşitli özelleştirme seçenekleri sunar:

- `heatmapRadius`: Her bir veri noktasının işlenmesi için bir piksel yarıçapı tanımlar. Yarıçapı sabit bir sayı veya bir ifade olarak ayarlayabilirsiniz. Bir ifade kullanarak, önizleme düzeyine göre yarıçapı ölçeklendirebilir ve haritadaki tutarlı bir uzamsal alanı (örneğin, 5-mil yarıçap) temsil edebilirsiniz.
- `heatmapColor`: Isı haritasının renk oluşturma şeklini belirtir. Renk gradyanı, ısı haritaları 'nın ortak bir özelliğidir. Bir ifadeyle efekti elde edebilirsiniz `interpolate` . Ayrıca, `step` ısı haritasını renklendirime için bir ifade kullanabilirsiniz ve yoğunluğu, bir dağılım veya radar stil eşlemesine benzeyen aralıklarda görsel parçalara ayırır. Bu renk paletleri, minimum değeri en yüksek yoğunluk değerine göre tanımlar.

  Isı haritaları için renk değerlerini değer üzerinde bir ifade olarak belirtirsiniz `heatmapDensity` . "Ilişkilendirme" ifadesinin 0 dizini veya "sözcük üzerinde" ifadesinin varsayılan rengi üzerinde hiçbir verilerin tanımlanmadığı alanın rengi. Bu değeri, bir arka plan rengi tanımlamak için kullanabilirsiniz. Genellikle, bu değer saydam veya yarı saydam bir siyah olarak ayarlanır. 

  Renk ifadelerinin örnekleri aşağıda verilmiştir:

  | Enterpolasyon rengi ifadesi | Basamaklı renk ifadesi |
  |--------------------------------|--------------------------|
  | enterpolasyon<br/>&nbsp;&nbsp;&nbsp;&nbsp;doğrusal (), <br/>&nbsp;&nbsp;&nbsp;&nbsp;Heatmapyoğunluğu (),<br/>&nbsp;&nbsp;&nbsp;&nbsp;Durdur (0, renk (Color. saydam)),<br/>&nbsp;&nbsp;&nbsp;&nbsp;Durdur (0,01, Color (Color. MACENTA)),<br/>&nbsp;&nbsp;&nbsp;&nbsp;Durdur (0,5, Color (parseColor ("#fb00fb")))),<br/>&nbsp;&nbsp;&nbsp;&nbsp;Durdur (1, renk (parseColor ("#00c3ff"))))<br/>)` | ından<br/>&nbsp;&nbsp;&nbsp;&nbsp;Heatmapyoğunluğu (),<br/>&nbsp;&nbsp;&nbsp;&nbsp;renk (renkli. saydam),<br/>&nbsp;&nbsp;&nbsp;&nbsp;Durdur (0,01, Color (parseColor ("#000080")))),<br/>&nbsp;&nbsp;&nbsp;&nbsp;Durdur (0,25, Color (parseColor ("#000080")))),<br/>&nbsp;&nbsp;&nbsp;&nbsp;Durdur (0,5, Color (Color. YEŞIL)),<br/>&nbsp;&nbsp;&nbsp;&nbsp;Durdur (0,5, Color (Color. sarı)),<br/>&nbsp;&nbsp;&nbsp;&nbsp;Durdur (1, renk (renkli. kırmızı))<br/>) |

- `heatmapOpacity`: Isı haritası katmanının donuk veya saydam şeklini belirtir.
- `heatmapIntensity`: Heatmap 'in genel yoğunluğunu artırmak için her bir veri noktasının ağırlığına bir çarpan uygular. Veri noktalarının ağırlığına bir farklılık oluşmasına neden olur, bu da görselleştirmeyi kolaylaştırır.
- `heatmapWeight`: Varsayılan olarak, tüm veri noktalarında 1 ağırlığı vardır ve eşit olarak ağırlıklı şekilde tanımlanır. Ağırlık seçeneği bir çarpan olarak davranır ve bunu sayı veya bir ifade olarak ayarlayabilirsiniz. Bir sayı ağırlık olarak ayarlandıysa, her bir veri noktasının haritaya iki kez yerleştirilmesi denklik olur. Örneğin, ağırlık ise `2` yoğunluğu iki katına çıkarır. Weight seçeneğini bir sayı olarak ayarlamak, ısı haritasını, yoğunluk seçeneğini kullanmak için benzer bir şekilde işler.

  Ancak, bir ifade kullanırsanız her bir veri noktasının ağırlığı her bir veri noktasının özelliklerine bağlı olabilir. Örneğin, her bir veri noktasının bir deprem temsil ettiğini varsayın. Büyüklük değeri her bir deprem veri noktası için önemli bir ölçümdür. Deprem her zaman gerçekleşir, ancak en az büyüklüğü düşüktür ve fark edilmez. Her bir veri noktasına ağırlığı atamak için bir ifadede büyüklük değerini kullanın. Ağırlığı atamak için büyüklük değerini kullanarak, ısı haritası içinde deprem 'in önemini daha iyi bir şekilde temsil edersiniz.
- `minZoom` ve `maxZoom` : katmanın gösterilmesi gereken yakınlaştırma düzeyi aralığı.
- `filter`: Kaynaktan alınan ve katmanda işlenen bir filtre ifadesi.
- `sourceLayer`: Katmana bağlı veri kaynağı bir vektör kutucuk kaynağı ise, vektör kutucukları içindeki bir kaynak katmanın belirtilmesi gerekir.
- `visible`: Katmanı gizler veya gösterir.

Aşağıda, yumuşak bir renk gradyanı oluşturmak için bir Oluşturucu ilişkilendirme ifadesinin kullanıldığı bir ısı haritası örneği verilmiştir. `mag`Verilerde tanımlanan özellik, her bir veri noktasının ağırlığını veya uygunluğunu ayarlamak için bir üstel ilişkilendirme ile birlikte kullanılır.

::: zone pivot="programming-language-java-android"

```java
HeatMapLayer layer = new HeatMapLayer(source,
    heatmapRadius(10f),

    //A linear interpolation is used to create a smooth color gradient based on the heat map density.
    heatmapColor(
        interpolate(
            linear(),
            heatmapDensity(),
            stop(0, color(Color.TRANSPARENT)),
            stop(0.01, color(Color.BLACK)),
            stop(0.25, color(Color.MAGENTA)),
            stop(0.5, color(Color.RED)),
            stop(0.75, color(Color.YELLOW)),
            stop(1, color(Color.WHITE))
        )
    ),

    //Using an exponential interpolation since earthquake magnitudes are on an exponential scale.
    heatmapWeight(
       interpolate(
            exponential(2),
            get("mag"),
            stop(0,0),

            //Any earthquake above a magnitude of 6 will have a weight of 1
            stop(6, 1)
       )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = HeatMapLayer(source,
    heatmapRadius(10f),

    //A linear interpolation is used to create a smooth color gradient based on the heat map density.
    heatmapColor(
        interpolate(
            linear(),
            heatmapDensity(),
            stop(0, color(Color.TRANSPARENT)),
            stop(0.01, color(Color.BLACK)),
            stop(0.25, color(Color.MAGENTA)),
            stop(0.5, color(Color.RED)),
            stop(0.75, color(Color.YELLOW)),
            stop(1, color(Color.WHITE))
        )
    ),

    //Using an exponential interpolation since earthquake magnitudes are on an exponential scale.
    heatmapWeight(
       interpolate(
            exponential(2),
            get("mag"),
            stop(0,0),

            //Any earthquake above a magnitude of 6 will have a weight of 1
            stop(6, 1)
       )
    )
)
```

::: zone-end

Aşağıdaki ekran görüntüsünde, önceki ısı haritası örneğinde aynı verileri kullanan Yukarıdaki özel ısı haritası katmanı gösterilmektedir.

![Son Deprem özel ısı haritası katmanıyla eşle](media/map-add-heat-map-layer-android/android-custom-heat-map-layer.png)

## <a name="consistent-zoomable-heat-map"></a>Tutarlı Zoomable ısı haritası

Varsayılan olarak, ısı haritası katmanında işlenen veri noktalarının yarıçapının tüm yakınlaştırma düzeyleri için sabit bir piksel yarıçapı vardır. Haritayı yakınlaştırdığınızda, veriler birlikte toplar ve ısı haritası katmanı farklı görünür. Aşağıdaki videoda, bir harita yakınlaştırılırken bir piksel yarıçapını tuttuğu ısı haritasının varsayılan davranışı gösterilmektedir.

![Tutarlı bir piksel boyutunu gösteren bir ısı haritası katmanıyla birlikte harita yakınlaştırması gösteren animasyon](media/map-add-heat-map-layer-android/android-heat-map-layer-zoom.gif)

Her bir `zoom` yakınlaştırma düzeyi için yarıçapı ölçeklendirmek üzere bir ifade kullanın, örneğin her bir veri noktası haritanın aynı fiziksel alanını içerir. Bu ifade, ısı haritası katmanının daha statik ve tutarlı görünmesini sağlar. Haritanın her yakınlaştırma düzeyinin, önceki yakınlaştırma düzeyiyle dikey ve yatay olarak iki piksel daha vardır.

Her yakınlaştırma düzeyiyle çift olacak şekilde yarıçap ölçekleme, tüm yakınlaştırma düzeylerinde tutarlı görünen bir ısı haritası oluşturur. Bu ölçeklendirmeyi uygulamak için, `zoom` `exponential interpolation` En düşük yakınlaştırma düzeyi için piksel yarıçapı ve aşağıdaki örnekte gösterildiği gibi hesaplanan en yüksek yakınlaştırma düzeyi için ölçeklendirilmiş bir yarıçap ile birlikte bir taban 2 ifadesiyle kullanın `2 * Math.pow(2, minZoom - maxZoom)` . Isı haritasının yakınlaştırma düzeyiyle nasıl ölçeklendirçalıştığını görmek için haritayı yakınlaştırın.

::: zone pivot="programming-language-java-android"

```java
HeatMapLayer layer = new HeatMapLayer(source,
  heatmapRadius(
    interpolate(
      exponential(2),
      zoom(),

      //For zoom level 1 set the radius to 2 pixels.
      stop(1, 2f),

      //Between zoom level 1 and 19, exponentially scale the radius from 2 pixels to 2 * (maxZoom - minZoom)^2 pixels.
      stop(19, Math.pow(2, 19 - 1) * 2f)
    )
  ),
  heatmapOpacity(0.75f)
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = HeatMapLayer(source,
  heatmapRadius(
    interpolate(
      exponential(2),
      zoom(),

      //For zoom level 1 set the radius to 2 pixels.
      stop(1, 2f),

      //Between zoom level 1 and 19, exponentially scale the radius from 2 pixels to 2 * (maxZoom - minZoom)^2 pixels.
      stop(19, Math.pow(2.0, 19 - 1.0) * 2f)
    )
  ),
  heatmapOpacity(0.75f)
)
```

::: zone-end

Aşağıdaki videoda, harita yakınlaştırma düzeylerinde tutarlı bir ısı haritası oluşturma işlemi oluşturmak için Haritayı yakınlaştırırken, yukarıdaki kodu çalıştıran bir harita gösterilmektedir.

![Tutarlı bir jeo-uzamsal boyutu gösteren bir ısı haritası katmanıyla birlikte harita yakınlaştırması gösteren animasyon](media/map-add-heat-map-layer-android/android-consistent-zoomable-heat-map-layer.gif)

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklenecek daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-android-sdk.md)
