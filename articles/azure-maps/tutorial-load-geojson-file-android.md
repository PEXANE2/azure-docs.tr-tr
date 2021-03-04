---
title: "Öğretici: coğrafi JSON verilerini Azure Maps 'e yükleme Android SDK | Microsoft Azure haritaları"
description: Coğrafi JSON veri dosyasını Azure Maps Android harita SDK 'sına yükleme hakkında öğretici.
author: rbrundritt
ms.author: richbrun
ms.date: 12/10/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: 8300a7c120ce816c8068a88fa69f4f978fa664ca
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102034515"
---
# <a name="tutorial-load-geojson-data-into-azure-maps-android-sdk"></a>Öğretici: coğrafi JSON verilerini Azure Maps 'e yükleme Android SDK

Bu öğretici, Azure Maps Android SDK konum verilerinin coğrafi json dosyasını içeri aktarma işleminde size rehberlik eder. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Android uygulamasına Azure haritaları ekleyin.
> * Bir veri kaynağı oluşturun ve yerel bir dosyadan veya Web 'den bir GeoJSON dosyasında yükleyin.
> * Haritada verileri görüntüleyin.

## <a name="prerequisites"></a>Önkoşullar

1. [Hızlı başlangıç: Android uygulaması oluşturma](quick-android-map.md)' yı doldurun. Bu öğretici, bu hızlı başlangıçta kullanılan kodu genişletecektir.
2. Ilgi çekici geojson dosyasının [örnek noktalarını](https://raw.githubusercontent.com/Azure-Samples/AzureMapsCodeSamples/master/AzureMapsCodeSamples/Common/data/geojson/SamplePoiDataSet.json) indirin.

### <a name="import-geojson-data-from-web-or-assets-folder"></a>Web veya varlıklar klasöründen GeoJSON verilerini içeri aktar

Çoğu GeoJSON dosyası, içindeki tüm verileri sarın `FeatureCollection` . Bu göz önünde bulundurularak, GeoJSON dosyaları bir dize olarak uygulamaya yüklenirse, bu, `fromJson` dizeyi, eşlemenin eklenebileceği bir geojson nesnesi olarak seri durumdan çıkarabilen özellik koleksiyonunun statik yöntemine iletilebilir `FeatureCollection` .

Aşağıdaki adımlarda, coğrafi json dosyasını uygulamaya aktarma ve coğrafi JSON nesnesi olarak serisini kaldırma işlemleri gösterilmektedir `FeatureCollection` .

1. Hızlı başlangıç: aşağıdaki adımlar bu uygulamanın üstünde derleme yaparken [Android uygulaması oluşturma](quick-android-map.md) ' yı doldurun.
2. Android Studio 'nun Proje panelinde **uygulama** klasörüne sağ tıklayıp öğesine gidin `New > Folder > Assets Folder` .
3. Ilgi çekici geojson dosyasının [örnek noktalarını](https://raw.githubusercontent.com/Azure-Samples/AzureMapsCodeSamples/master/AzureMapsCodeSamples/Common/data/geojson/SamplePoiDataSet.json) sürükleyin ve varlıklar klasörüne bırakın.

::: zone pivot="programming-language-java-android"

4. **Utils. Java** adlı yeni bir dosya oluşturun ve bu dosyaya aşağıdaki kodu ekleyin. Bu kod `importData` , bir dosyayı `assets` uygulamanın klasöründen veya bir URL 'yi bir dize olarak kullanarak Web 'den veya basit bir geri çağırma yöntemi kullanarak Kullanıcı arabirimi iş parçacığına geri döndüren adlı statik bir yöntem sağlar.

    ```java
    //Modify the package name as needed to align with your application.
    package com.example.myapplication;
    
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

5. **MainActivity. Java** dosyasına gidin ve olay için geri çağırma içine aşağıdaki kodu ekleyin `mapControl.onReady` , bu yöntem içinde bulunur `onCreate` . Bu kod, dosyadaki **SamplePoiDataSet.js** bir dize olarak okumak için içeri aktarma yardımcı programını kullanır, ardından sınıfın statik yöntemini kullanarak bir özellik koleksiyonu olarak serileştirir `fromJson` `FeatureCollection` . Bu kod ayrıca, özellik koleksiyonundaki tüm veriler için sınırlayıcı kutu alanını hesaplar ve bunu, verilerin üzerine odaklanmak üzere haritanın kamerayı ayarlamak için kullanır.

    ```java
    //Create a data source and add it to the map.
    DataSource source = new DataSource();
    map.sources.add(source);
    
    //Import the GeoJSON data and add it to the data source.
    Utils.importData("SamplePoiDataSet.json",
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

                //Padding added to account for pixel size of rendered points.
                padding(20)
            );
        });
    ```

6. Coğrafi JSON verilerini bir veri kaynağına yüklemek için kodu kullanarak, artık bu verilerin haritada nasıl görüntüleneceğini belirtmemiz gerekir. Nokta verileri için birkaç farklı işleme katmanı vardır; [Kabarcık katmanı](map-add-bubble-layer-android.md), [sembol katmanı](how-to-add-symbol-to-android-map.md)ve [ısı haritası katmanı](map-add-heat-map-layer-android.md) en yaygın kullanılan katmanlardır. `mapControl.onReady`Verileri içeri aktarma kodundan sonra olay için geri çağırmada bir kabarcık katmanındaki verileri işlemek için aşağıdaki kodu ekleyin.

    ```java
    //Create a layer and add it to the map.
    BubbleLayer layer = new BubbleLayer(source);
    map.layers.add(layer);
    ```

::: zone-end

::: zone pivot="programming-language-kotlin"

4. **Utils. kt** adlı yeni bir dosya oluşturun ve bu dosyaya aşağıdaki kodu ekleyin. Bu kod `importData` , bir dosyayı `assets` uygulamanın klasöründen veya bir URL 'yi bir dize olarak kullanarak Web 'den veya basit bir geri çağırma yöntemi kullanarak Kullanıcı arabirimi iş parçacığına geri döndüren adlı statik bir yöntem sağlar.

    ```kotlin
    //Modify the package name as needed to align with your application.
    package com.example.myapplication;

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

5. **MainActivity. kt** dosyasına gidin ve olay için geri çağırma içine aşağıdaki kodu ekleyin `mapControl.onReady` , bu yöntem içinde bulunur `onCreate` . Bu kod, dosyadaki **SamplePoiDataSet.js** bir dize olarak okumak için içeri aktarma yardımcı programını kullanır, ardından sınıfın statik yöntemini kullanarak bir özellik koleksiyonu olarak serileştirir `fromJson` `FeatureCollection` . Bu kod ayrıca, özellik koleksiyonundaki tüm veriler için sınırlayıcı kutu alanını hesaplar ve bunu, verilerin üzerine odaklanmak üzere haritanın kamerayı ayarlamak için kullanır.

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

6. Coğrafi JSON verilerini bir veri kaynağına yüklemek için kodu kullanarak, artık bu verilerin haritada nasıl görüntüleneceğini belirtmemiz gerekir. Nokta verileri için birkaç farklı işleme katmanı vardır; [Kabarcık katmanı](map-add-bubble-layer-android.md), [sembol katmanı](how-to-add-symbol-to-android-map.md)ve [ısı haritası katmanı](map-add-heat-map-layer-android.md) en yaygın kullanılan katmanlardır. `mapControl.onReady`Verileri içeri aktarma kodundan sonra olay için geri çağırmada bir kabarcık katmanındaki verileri işlemek için aşağıdaki kodu ekleyin.

    ```kotlin
    //Create a layer and add it to the map.
    val layer = new BubbleLayer(source)
    map.layers.add(layer)
    ```

::: zone-end

7. Uygulamayı çalıştırın. Bir harita, coğrafi JSON dosyasındaki her konum için daire içine alınır ve ABD üzerine yoğunlaşacaktır.

    ![Bir GeoJSON dosyasındaki verilerle USA eşlemi](media/tutorial-load-geojson-file-android/android-import-geojson.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiden kaynakları temizlemek için aşağıdaki adımları uygulayın:

1. Android Studio kapatın ve oluşturduğunuz uygulamayı silin.
2. Uygulamayı bir dış cihazda test ederseniz, uygulamayı bu cihazdan kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla kod örneği ve etkileşimli bir kodlama deneyimi için:

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Özellik bilgilerini görüntüleme](display-feature-information-android.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](how-to-add-shapes-to-android-map.md)
