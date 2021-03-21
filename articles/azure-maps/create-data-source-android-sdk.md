---
title: Android Maps için veri kaynağı oluşturma | Microsoft Azure haritaları
description: 'Bir harita için veri kaynağı oluşturmayı öğrenin. Azure Haritalar Android SDK kullandığı veri kaynakları hakkında bilgi edinin: GeoJSON kaynakları ve vektör kutucukları.'
author: rbrundritt
ms.author: richbrun
ms.date: 2/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: e870134e2ecd431aa3e5c02638120027f0d47df2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102101469"
---
# <a name="create-a-data-source-android-sdk"></a>Veri kaynağı oluşturma (Android SDK)

Azure haritalar, verileri veri kaynaklarında depolar Android SDK. Veri kaynaklarını kullanmak, sorgulamak ve işlemek için veri işlemlerini iyileştirir. Şu anda iki tür veri kaynağı vardır:

- **Geojson kaynağı**: coğrafi konum verilerini yerel olarak geojson biçiminde yönetir. Küçük ve orta ölçekli veri kümeleri için iyi (yüzlerce yüz binlerce şekil).
- **Vektör kutucuk kaynağı**: haritalar döşeme sistemine bağlı olarak, geçerli harita görünümü için vektör kutucukları olarak biçimlendirilen verileri yükler. Büyük ve çok büyük veri kümeleri (milyonlarca veya milyarlarca şekil) için idealdir.

## <a name="geojson-data-source"></a>GeoJSON veri kaynağı

Azure Maps, birincil veri modellerinden biri olarak GeoJSON kullanır. GeoJSON, Jeo uzamsal verileri JSON biçiminde göstermek için açık bir jeo-uzamsal standart yoludur. Azure haritalar 'da bulunan GeoJSON sınıfları kolay bir şekilde oluşturmak ve coğrafi JSON verilerini seri hale getirmek için Android SDK. Sınıfında GeoJSON verilerini yükleyin ve depolayın `DataSource` ve katmanları kullanarak işleme yapın. Aşağıdaki kod, coğrafi JSON nesnelerinin Azure haritalar 'da nasıl oluşturulagösterdiğini gösterir.

::: zone pivot="programming-language-java-android"

```java
/*
    Raw GeoJSON feature
    
    {
         "type": "Feature",
         "geometry": {
             "type": "Point",
             "coordinates": [-100, 45]
         },
         "properties": {
             "custom-property": "value"
         }
    }

*/

//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(-100, 45));

//Add a property to the feature.
feature.addStringProperty("custom-property", "value");

//Add the feature to the data source.
source.add(feature);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
/*
    Raw GeoJSON feature
    
    {
         "type": "Feature",
         "geometry": {
             "type": "Point",
             "coordinates": [-100, 45]
         },
         "properties": {
             "custom-property": "value"
         }
    }

*/

//Create a point feature.
val feature = Feature.fromGeometry(Point.fromLngLat(-100, 45))

//Add a property to the feature.
feature.addStringProperty("custom-property", "value")

//Add the feature to the data source.
source.add(feature)
```

::: zone-end

Alternatif olarak, özellikler aşağıda gösterildiği gibi, ilk olarak bir Jsonnesnesine yüklenebilir ve bu özellik oluşturulduğunda özelliğe geçirilir.

::: zone pivot="programming-language-java-android"

```java
//Create a JsonObject to store properties for the feature.
JsonObject properties = new JsonObject();
properties.addProperty("custom-property", "value");

Feature feature = Feature.fromGeometry(Point.fromLngLat(-100, 45), properties);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a JsonObject to store properties for the feature.
val properties = JsonObject()
properties.addProperty("custom-property", "value")

val feature = Feature.fromGeometry(Point.fromLngLat(-100, 45), properties)
```

::: zone-end

Bir GeoJSON özelliği oluşturulduktan sonra haritanın özelliği aracılığıyla haritaya bir veri kaynağı eklenebilir `sources` . Aşağıdaki kod, oluşturma `DataSource` , haritaya ekleme ve veri kaynağına özellik ekleme işlemlerinin nasıl yapılacağını gösterir.

```javascript
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Add GeoJSON feature to the data source.
source.add(feature);
```

Aşağıdaki kod, GeoJSON özelliği, Korturecollection ve geometriler oluşturmak için çeşitli yollar gösterir.

::: zone pivot="programming-language-java-android"

```java
//GeoJSON Point Geometry
Point point = Point.fromLngLat(LONGITUDE, LATITUDE);

//GeoJSON Point Geometry
LineString linestring = LineString.fromLngLats(PointList);

//GeoJSON Polygon Geometry
Polygon polygon = Polygon.fromLngLats(listOfPointList);
 
Polygon polygonFromOuterInner = Polygon.fromOuterInner(outerLineStringObject,innerLineStringObject);

//GeoJSON MultiPoint Geometry
MultiPoint multiPoint = MultiPoint.fromLngLats(PointList);

//GeoJSON MultiLineString Geometry
MultiLineString multiLineStringFromLngLat = MultiLineString.fromLngLats(listOfPointList);

MultiLineString multiLineString = MultiLineString.fromLineString(singleLineString);

//GeoJSON MultiPolygon Geometry
MultiPolygon multiPolygon = MultiPolygon.fromLngLats(listOflistOfPointList);

MultiPolygon multiPolygonFromPolygon = MultiPolygon.fromPolygon(polygon);

MultiPolygon multiPolygonFromPolygons = MultiPolygon.fromPolygons(PolygonList);

//GeoJSON Feature
Feature pointFeature = Feature.fromGeometry(Point.fromLngLat(LONGITUDE, LATITUDE));
 
//GeoJSON FeatureCollection 
FeatureCollection featureCollectionFromSingleFeature = FeatureCollection.fromFeature(pointFeature);
 
FeatureCollection featureCollection = FeatureCollection.fromFeatures(listOfFeatures);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//GeoJSON Point Geometry
val point = Point.fromLngLat(LONGITUDE, LATITUDE)

//GeoJSON Point Geometry
val linestring = LineString.fromLngLats(PointList)

//GeoJSON Polygon Geometry
val polygon = Polygon.fromLngLats(listOfPointList)

val polygonFromOuterInner = Polygon.fromOuterInner(outerLineStringObject, innerLineStringObject)

//GeoJSON MultiPoint Geometry
val multiPoint = MultiPoint.fromLngLats(PointList)

//GeoJSON MultiLineString Geometry
val multiLineStringFromLngLat = MultiLineString.fromLngLats(listOfPointList)

val multiLineString = MultiLineString.fromLineString(singleLineString)

//GeoJSON MultiPolygon Geometry
val multiPolygon = MultiPolygon.fromLngLats(listOflistOfPointList)

val multiPolygonFromPolygon = MultiPolygon.fromPolygon(polygon)

val multiPolygonFromPolygons = MultiPolygon.fromPolygons(PolygonList)

//GeoJSON Feature
val pointFeature = Feature.fromGeometry(Point.fromLngLat(LONGITUDE, LATITUDE))

//GeoJSON FeatureCollection 
val featureCollectionFromSingleFeature = FeatureCollection.fromFeature(pointFeature)

val featureCollection = FeatureCollection.fromFeatures(listOfFeatures)
```

::: zone-end

### <a name="serialize-and-deserialize-geojson"></a>Coğrafi JSON seri hale getirme ve serisini kaldırma

Özellik koleksiyonu, özellik ve geometri sınıflarının hepsi `fromJson()` `toJson()` , serileştirme ile ilgili yardımcı olan ve statik yöntemlere sahiptir. Yöntemi aracılığıyla geçirilen biçimlendirilen geçerli JSON dizesi `fromJson()` geometri nesnesini oluşturur. Bu `fromJson()` Yöntem ayrıca Gson veya diğer serileştirme/seri kaldırma stratejilerini kullanabileceğiniz anlamına gelir. Aşağıdaki kod, bir strıngimi GeoJSON özelliğinin nasıl alınacağını ve özellik sınıfına nasıl seri hale getireceğini gösterir ve ardından onu bir coğrafi JSON dizesine geri serileştirin.

::: zone pivot="programming-language-java-android"

```java
//Take a stringified GeoJSON object.
String GeoJSON_STRING = "{"
    + "      \"type\": \"Feature\","            
    + "      \"geometry\": {"
    + "            \"type\": \"Point\""
    + "            \"coordinates\": [-100, 45]"
    + "      },"
    + "      \"properties\": {"
    + "            \"custom-property\": \"value\""
    + "      },"
    + "}";

//Deserialize the JSON string into a feature.
Feature feature = Feature.fromJson(GeoJSON_STRING);
 
//Serialize a feature collection to a string.
String featureString = feature.toJson();
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Take a stringified GeoJSON object.
val GeoJSON_STRING = ("{"
        + "      \"type\": \"Feature\","
        + "      \"geometry\": {"
        + "            \"type\": \"Point\""
        + "            \"coordinates\": [-100, 45]"
        + "      },"
        + "      \"properties\": {"
        + "            \"custom-property\": \"value\""
        + "      },"
        + "}")

//Deserialize the JSON string into a feature.
val feature = Feature.fromJson(GeoJSON_STRING)

//Serialize a feature collection to a string.
val featureString = feature.toJson()
```

::: zone-end

### <a name="import-geojson-data-from-web-or-assets-folder"></a>Web veya varlıklar klasöründen GeoJSON verilerini içeri aktar

Çoğu GeoJSON dosyası bir Korturecollection içerir. GeoJSON dosyalarını dizeler olarak okuyun ve `FeatureCollection.fromJson` Bu yöntemi seri durumdan çıkarmak için kullandınız.

Aşağıdaki kod, Web veya yerel varlıklar klasöründen bir dize olarak veri aktarmak ve geri çağırma işlevi aracılığıyla UI iş parçacığına döndürmek için yeniden kullanılabilir bir sınıftır.

::: zone pivot="programming-language-java-android"

```java
import android.content.Context;
import android.os.Handler;
import android.os.Looper;
import android.webkit.URLUtil;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.URL;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

import javax.net.ssl.HttpsURLConnection;

public class Utils {

    interface SimpleCallback {
        void notify(String result);
    }

    /**
     * Imports data from a web url or asset file name and returns it to a callback.
     * @param urlOrFileName A web url or asset file name that points to data to load.
     * @param context The context of the app.
     * @param callback The callback function to return the data to.
     */
    public static void importData(String urlOrFileName, Context context, SimpleCallback callback){
        importData(urlOrFileName, context, callback, null);
    }
    
    /**
     * Imports data from a web url or asset file name and returns it to a callback.
     * @param urlOrFileName A web url or asset file name that points to data to load.
     * @param context The context of the app.
     * @param callback The callback function to return the data to.
     * @param error A callback function to return errors to.
     */
    public static void importData(String urlOrFileName, Context context, SimpleCallback callback, SimpleCallback error){
        if(urlOrFileName != null && callback != null) {
            ExecutorService executor = Executors.newSingleThreadExecutor();
            Handler handler = new Handler(Looper.getMainLooper());

            executor.execute(() -> {
                String data = null;

                try {

                    if(URLUtil.isNetworkUrl(urlOrFileName)){
                        data = importFromWeb(urlOrFileName);
                    } else {
                        //Assume file is in assets folder.
                        data = importFromAssets(context, urlOrFileName);
                    }

                    final String result = data;

                    handler.post(() -> {
                        //Ensure the resulting data string is not null or empty.
                        if (result != null && !result.isEmpty()) {
                            callback.notify(result);
                        } else {
                            error.notify("No data imported.");
                        }
                    });
                } catch(Exception e) {
                    if(error != null){
                        error.notify(e.getMessage());
                    }
                }
            });
        }
    }

    /**
     * Imports data from an assets file as a string.
     * @param context The context of the app.
     * @param fileName The asset file name.
     * @return
     * @throws IOException
     */
    private static String importFromAssets(Context context, String fileName) throws IOException {
        InputStream stream = null;

        try {
            stream = context.getAssets().open(fileName);

            if(stream != null) {
                return readStreamAsString(stream);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // Close Stream and disconnect HTTPS connection.
            if (stream != null) {
                stream.close();
            }
        }

        return null;
    }

    /**
     * Imports data from the web as a string.
     * @param url URL to the data.
     * @return
     * @throws IOException
     */
    private static String importFromWeb(String url) throws IOException {
        InputStream stream = null;
        HttpsURLConnection connection = null;
        String result = null;

        try {
            connection = (HttpsURLConnection) new URL(url).openConnection();

            //For this use case, set HTTP method to GET.
            connection.setRequestMethod("GET");

            //Open communications link (network traffic occurs here).
            connection.connect();

            int responseCode = connection.getResponseCode();
            if (responseCode != HttpsURLConnection.HTTP_OK) {
                throw new IOException("HTTP error code: " + responseCode);
            }

            //Retrieve the response body as an InputStream.
            stream = connection.getInputStream();

            if (stream != null) {
                return readStreamAsString(stream);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // Close Stream and disconnect HTTPS connection.
            if (stream != null) {
                stream.close();
            }
            if (connection != null) {
                connection.disconnect();
            }
        }

        return result;
    }

    /**
     * Reads an input stream as a string.
     * @param stream Stream to convert.
     * @return
     * @throws IOException
     */
    private static String readStreamAsString(InputStream stream) throws IOException {
        //Convert the contents of an InputStream to a String.
        BufferedReader in = new BufferedReader(new InputStreamReader(stream, "UTF-8"));

        String inputLine;
        StringBuffer response = new StringBuffer();

        while ((inputLine = in.readLine()) != null) {
            response.append(inputLine);
        }

        in.close();

        return response.toString();
    }
}
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
import android.content.Context
import android.os.Handler
import android.os.Looper
import android.webkit.URLUtil
import java.net.URL
import java.util.concurrent.ExecutorService
import java.util.concurrent.Executors

class Utils {
    companion object {

        /**
            * Imports data from a web url or asset file name and returns it to a callback.
            * @param urlOrFileName A web url or asset file name that points to data to load.
            * @param context The context of the app.
            * @param callback The callback function to return the data to.
            */
        fun importData(urlOrFileName: String?, context: Context, callback: (String?) -> Unit) {
            importData(urlOrFileName, context, callback, null)
        }

        /**
            * Imports data from a web url or asset file name and returns it to a callback.
            * @param urlOrFileName A web url or asset file name that points to data to load.
            * @param context The context of the app.
            * @param callback The callback function to return the data to.
            * @param error A callback function to return errors to.
            */
        public fun importData(urlOrFileName: String?, context: Context, callback: (String?) -> Unit, error: ((String?) -> Unit)?) {
            if (urlOrFileName != null && callback != null) {
                val executor: ExecutorService = Executors.newSingleThreadExecutor()
                val handler = Handler(Looper.getMainLooper())
                executor.execute {
                    var data: String? = null
                    
                    try {
                        data = if (URLUtil.isNetworkUrl(urlOrFileName)) {
                            URL(urlOrFileName).readText()
                        } else { //Assume file is in assets folder.
                            context.assets.open(urlOrFileName).bufferedReader().use{
                                it.readText()
                            }
                        }

                        handler.post {
                            //Ensure the resulting data string is not null or empty.
                            if (data != null && !data.isEmpty()) {
                                callback(data)
                            } else {
                                error!!("No data imported.")
                            }
                        }
                    } catch (e: Exception) {
                        error!!(e.message)
                    }
                }
            }
        }
    }
}
```

::: zone-end

Aşağıdaki kod, coğrafi JSON verilerini bir dize olarak içeri aktarmak ve bir geri çağırma yoluyla UI iş parçacığına döndürmek için bu yardımcı programın nasıl kullanılacağını gösterir. Geri çağırmada, dize verileri GeoJSON Özellik koleksiyonunda seri hale getirilebilir ve veri kaynağına eklenebilir. İsteğe bağlı olarak, haritalar kamerasını verilere odaklanmak için güncelleştirin.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource dataSource = new DataSource();
map.sources.add(dataSource);

//Import the geojson data and add it to the data source.
Utils.importData("URL_or_FilePath_to_GeoJSON_data",
    this,
    (String result) -> {
        //Parse the data as a GeoJSON Feature Collection.
        FeatureCollection fc = FeatureCollection.fromJson(result);

        //Add the feature collection to the data source.
        dataSource.add(fc);

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
DataSource source = new DataSource();
map.sources.add(source);

//Import the GeoJSON data and add it to the data source.
Utils.importData("SamplePoiDataSet.json", this) { 
    result: String? ->
        //Parse the data as a GeoJSON Feature Collection.
            val fc = FeatureCollection.fromJson(result!!)

        //Add the feature collection to the data source.
        source.add(fc)

        //Optionally, update the maps camera to focus in on the data.

        //Calculate the bounding box of all the data in the Feature Collection.
        val bbox = MapMath.fromData(fc);

        //Update the maps camera so it is focused on the data.
        map.setCamera(
            bounds(bbox),

            //Padding added to account for pixel size of rendered points.
            padding(20)
        )
    }
```

::: zone-end

## <a name="vector-tile-source"></a>Vektör kutucuk kaynağı

Vektör kutucuk kaynağı bir vektör kutucuk katmanına nasıl erişebileceğinizi açıklar. `VectorTileSource`Vektör kutucuk kaynağı oluşturmak için sınıfını kullanın. Vektör döşeme katmanları döşeme katmanlarına benzerdir, ancak aynı değildir. Döşeme katmanı bir raster görüntüsüdür. Vektör döşeme katmanları, **PBF** biçiminde sıkıştırılmış bir dosyadır. Bu sıkıştırılmış dosya, vektör eşleme verilerini ve bir veya daha fazla katmanı içerir. Dosya, her katmanın stiline bağlı olarak işlenilerek, istemci üzerinde stil oluşturulabilir. Vektör kutucuğunda bulunan veriler, işaret, çizgi ve çokgenler biçimindeki coğrafi özellikler içerir. Raster döşeme katmanları yerine vektör kutucuğu katmanlarını kullanmanın çeşitli avantajları vardır:

- Vektör kutucuğunun dosya boyutu genellikle denk bir raster kutucuğundan çok daha küçüktür. Bu nedenle, daha az bant genişliği kullanılır. Daha düşük gecikme süresi, daha hızlı bir harita ve daha iyi bir kullanıcı deneyimi anlamına gelir.
- Vektör kutucukları istemcide işlendiği için, üzerinde görüntülendikleri cihazın çözünürlüğüne uyarlarlar. Sonuç olarak, işlenen haritalar Crystal Clear etiketleriyle daha iyi tanımlanmış şekilde görünür.
- Yeni stil istemciye uygulanamadığından, vektör eşlemlerdeki verilerin stilini değiştirmek, verilerin yeniden indirilmesini gerektirmez. Buna karşılık, bir raster kutucuk katmanının stilini değiştirmenin genellikle sunucudan kutucukları yükleme ve ardından yeni stili uygulama gerekir.
- Veriler vektör biçiminde teslim edildiğinden, verileri hazırlamak için daha az sunucu tarafı işleme gerekir. Sonuç olarak, daha yeni veriler daha hızlı kullanılabilir hale getirilebilir.

Azure haritalar, açık bir standart olan [Mapbox vektör kutucuk belirtimine](https://github.com/mapbox/vector-tile-spec)uyar. Azure Maps, platformun bir parçası olarak aşağıdaki vektör kutucukları hizmetlerini sağlar:

- Yol kutucukları [belge](/rest/api/maps/renderv2/getmaptilepreview)  |  [verileri biçimi ayrıntıları](https://developer.tomtom.com/maps-api/maps-api-documentation-vector/tile)
- Trafik olayları [belge](/rest/api/maps/traffic/gettrafficincidenttile)  |  [verileri biçimi ayrıntıları](https://developer.tomtom.com/traffic-api/traffic-api-documentation-traffic-incidents/vector-incident-tiles)
- Trafik akışı [belgeleri](/rest/api/maps/traffic/gettrafficflowtile)  |  [veri biçimi ayrıntıları](https://developer.tomtom.com/traffic-api/traffic-api-documentation-traffic-flow/vector-flow-tiles)
- Azure haritalar Oluşturucusu Ayrıca özel vektör kutucuklarının oluşturma ve [kutucuk oluşturma v2](/rest/api/maps/renderv2/getmaptilepreview) aracılığıyla erişilebilir olmasını sağlar

> [!TIP]
> Azure haritalar işleme hizmeti 'nden Web SDK 'Sı ile vektör veya raster görüntü kutucukları kullanırken, `atlas.microsoft.com` yer tutucu ile değiştirebilirsiniz `azmapsdomain.invalid` . Bu yer tutucu, eşleme tarafından kullanılan aynı etki alanıyla değiştirilmelidir ve aynı kimlik doğrulama ayrıntılarını da otomatik olarak ekler. Bu, Azure Active Directory kimlik doğrulaması kullanırken işleme hizmeti ile kimlik doğrulamasını büyük ölçüde basitleştirir.

Haritadaki bir vektör kutucuk kaynağından verileri göstermek için, kaynağı veri işleme katmanlarından birine bağlayın. Vektör kaynağı kullanan tüm katmanların seçeneklerde bir değer belirtmesi gerekir `sourceLayer` . Aşağıdaki kod, Azure Maps trafik akışı vektör kutucuk hizmetini bir vektör kutucuk kaynağı olarak yükler ve bir çizgi katmanını kullanarak haritada görüntüler. Bu vektör kutucuk kaynağı kaynak katmanda "trafik akışı" adlı tek bir veri kümesine sahiptir. Bu veri kümesindeki satır verilerinde, `traffic_level` rengi seçmek ve çizgilerin boyutunu ölçeklendirmek için bu kodda kullanılan adlı bir özellik vardır.

::: zone pivot="programming-language-java-android"

```java
//Formatted URL to the traffic flow vector tiles, with the maps subscription key appended to it.
String trafficFlowUrl = "https://azmapsdomain.invalid/traffic/flow/tile/pbf?api-version=1.0&style=relative&zoom={z}&x={x}&y={y}";

//Create a vector tile source and add it to the map.
VectorTileSource source = new VectorTileSource(
    tiles(new String[] { trafficFlowUrl }),
    maxSourceZoom(22)
);
map.sources.add(source);

//Create a layer for traffic flow lines.
LineLayer layer = new LineLayer(source,
    //The name of the data layer within the data source to pass into this rendering layer.
    sourceLayer("Traffic flow"),

    //Color the roads based on the traffic_level property.
    strokeColor(
        interpolate(
            linear(),
            get("traffic_level"),
            stop(0, color(Color.RED)),
            stop(0.33, color(Color.YELLOW)),
            stop(0.66, color(Color.GREEN))
        )
    ),

    //Scale the width of roads based on the traffic_level property.
    strokeWidth(
        interpolate(
            linear(),
            get("traffic_level"),
            stop(0, 6),
            stop(1,1)
        )
    )
);

//Add the traffic flow layer below the labels to make the map clearer.
map.layers.add(layer, "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Formatted URL to the traffic flow vector tiles, with the maps subscription key appended to it.
val trafficFlowUrl = "https://azmapsdomain.invalid/traffic/flow/tile/pbf?api-version=1.0&style=relative&zoom={z}&x={x}&y={y}"

//Create a vector tile source and add it to the map.
val source = VectorTileSource(
    tiles(arrayOf(trafficFlowUrl)),
    maxSourceZoom(22)
)
map.sources.add(source)

//Create a layer for traffic flow lines.
val layer = LineLayer(
    source,  //The name of the data layer within the data source to pass into this rendering layer.
    sourceLayer("Traffic flow"),  //Color the roads based on the traffic_level property.
    strokeColor(
        interpolate(
            linear(),
            get("traffic_level"),
            stop(0, color(Color.RED)),
            stop(0.33, color(Color.YELLOW)),
            stop(0.66, color(Color.GREEN))
        )
    ),  //Scale the width of roads based on the traffic_level property.
    strokeWidth(
        interpolate(
            linear(),
            get("traffic_level"),
            stop(0, 6),
            stop(1, 1)
        )
    )
)

//Add the traffic flow layer below the labels to make the map clearer.
map.layers.add(layer, "labels")
```

::: zone-end

![Trafik akışı düzeylerini gösteren renk kodlu yol satırlarıyla eşle](media/create-data-source-android-sdk/android-vector-tile-source-line-layer.png)

## <a name="connecting-a-data-source-to-a-layer"></a>Bir veri kaynağını katmana bağlama

Veriler, işleme katmanları kullanılarak haritada işlenir. Tek bir veri kaynağına, bir veya daha fazla işleme katmanı tarafından başvurulabilir. Aşağıdaki işleme katmanları bir veri kaynağı gerektirir:

- [Kabarcık katmanı](map-add-bubble-layer-android.md) -nokta verilerini haritada ölçeklendirilen daireler olarak işler.
- [Sembol katmanı](how-to-add-symbol-to-android-map.md) -nokta verilerini simge veya metin olarak işler.
- [Isı haritası katmanı](map-add-heat-map-layer-android.md) -nokta verilerini yoğunluk ısı haritası olarak işler.
- [Çizgi katmanı](android-map-add-line-layer.md) -bir çizgiyi işleme ve çokgen ana hattını işleme.
- [Çokgen katmanı](how-to-add-shapes-to-android-map.md) -bir çokgen alanını düz renk veya görüntü düzeniyle doldurur.

Aşağıdaki kod, bir veri kaynağının nasıl oluşturulacağını, haritaya nasıl ekleneceğini ve bir kabarcık katmanına nasıl bağlanacağını gösterir. Ve sonra, coğrafi JSON noktası verilerini uzak bir konumdan veri kaynağına aktarın.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a layer that defines how to render points in the data source and add it to the map.
BubbleLayer layer = new BubbleLayer(source);
map.layers.add(layer);

//Import the geojson data and add it to the data source.
Utils.importData("URL_or_FilePath_to_GeoJSON_data",
    this,
    (String result) -> {
        //Parse the data as a GeoJSON Feature Collection.
        FeatureCollection fc = FeatureCollection.fromJson(result);

        //Add the feature collection to the data source.
        dataSource.add(fc);

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

//Create a layer that defines how to render points in the data source and add it to the map.
val layer = BubbleLayer(source)
map.layers.add(layer)

//Import the geojson data and add it to the data source.
Utils.importData("URL_or_FilePath_to_GeoJSON_data", this) { 
    result: String? ->
        //Parse the data as a GeoJSON Feature Collection.
        val fc = FeatureCollection.fromJson(result!!)
    
        //Add the feature collection to the data source.
        dataSource.add(fc)
    
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

Bu veri kaynaklarına bağlanmayama verileri doğrudan işleme için yükleyen ek işleme katmanları vardır.

- [Döşeme katmanı](how-to-add-tile-layer-android-map.md) -haritanın üzerine bir raster döşeme katmanı uygular.

## <a name="one-data-source-with-multiple-layers"></a>Birden çok katmanlı bir veri kaynağı

Birden çok katman tek bir veri kaynağına bağlanabilir. Bu seçeneğin yararlı olduğu birçok farklı senaryo vardır. Örneğin, bir kullanıcının Çokgen çizdiği senaryoyu düşünün. Kullanıcı haritaya işaret eklediğinden Çokgen alanını işlemeli ve doldurmalıdır. Çokgen ana hattına stil eklemek, Kullanıcı çizerken poligonun kenarlarını daha kolay görmenizi sağlar. Poligon tek bir konumu rahat bir şekilde düzenlemek için her bir konumun üzerine bir pin veya işaret gibi bir tanıtıcı ekleyebiliriz.

![Tek bir veri kaynağından birden çok katman işleme verilerini gösteren eşleme](media/create-data-source-android-sdk/multiple-layers-one-datasource.png)

Çoğu eşleme platformunda, poligon her konum için bir çokgen nesne, bir çizgi nesnesi ve bir PIN olması gerekir. Çokgen değiştirildiğinde, satırı ve PIN 'leri el ile güncelleştirmeniz gerekir, bu da hızlı bir şekilde karmaşık hale gelebilir.

Azure haritalar ile, bir veri kaynağında aşağıdaki kodda gösterildiği gibi tek bir çokgen olması yeterlidir.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a polygon and add it to the data source.
source.add(Polygon.fromLngLats(/* List of points */));

//Create a polygon layer to render the filled in area of the polygon.
PolygonLayer polygonLayer = new PolygonLayer(source,
    fillColor("rgba(255,165,0,0.2)")
);

//Create a line layer for greater control of rendering the outline of the polygon.
LineLayer lineLayer = new LineLayer(source,
    strokeColor("orange"),
    strokeWidth(2f)
);

//Create a bubble layer to render the vertices of the polygon as scaled circles.
BubbleLayer bubbleLayer = new BubbleLayer(source,
    bubbleColor("orange"),
    bubbleRadius(5f),
    bubbleStrokeColor("white"),
    bubbleStrokeWidth(2f)
);

//Add all layers to the map.
map.layers.add(new Layer[] { polygonLayer, lineLayer, bubbleLayer });
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a polygon and add it to the data source.
source.add(Polygon.fromLngLats())

//Create a polygon layer to render the filled in area of the polygon.
val polygonLayer = PolygonLayer(
    source,
    fillColor("rgba(255,165,0,0.2)")
)

//Create a line layer for greater control of rendering the outline of the polygon.
val lineLayer = LineLayer(
    source,
    strokeColor("orange"),
    strokeWidth(2f)
)

//Create a bubble layer to render the vertices of the polygon as scaled circles.
val bubbleLayer = BubbleLayer(
    source,
    bubbleColor("orange"),
    bubbleRadius(5f),
    bubbleStrokeColor("white"),
    bubbleStrokeWidth(2f)
)

//Add all layers to the map.
map.layers.add(arrayOf<Layer>(polygonLayer, lineLayer, bubbleLayer))
```

::: zone-end

> [!TIP]
> Yöntemi kullanılarak haritaya katman eklerken `map.layers.add` , var olan bir KATMANıN kimliği veya örneği ikinci bir parametre olarak geçirilebilir. Bu, eşlemenin mevcut katmanın altına eklenen yeni katmanı eklemesini söyler. Bu yöntem, katman KIMLIĞINI geçirmenin yanı sıra aşağıdaki değerleri de destekler.
>
> - `"labels"` -Harita etiketi katmanlarının altına yeni katmanı ekler.
> - `"transit"` -Harita yolu ve aktarım katmanlarının altına yeni katmanı ekler.

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [Isı haritası ekleme](map-add-heat-map-layer-android.md)

> [!div class="nextstepaction"]
> [Web SDK kodu örnekleri](/samples/browse/?products=azure-maps)