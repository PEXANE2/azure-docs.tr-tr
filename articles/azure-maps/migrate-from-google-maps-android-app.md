---
title: 'Öğretici: Bir Android uygulamasını geçir | Microsoft Azure Haritaları'
description: Bir Android uygulamasını Google Haritalar'dan Microsoft Azure Haritalar'a nasıl geçirebilirsiniz?
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 9514398ec6a84becd1283e4b0975804101b64086
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77209741"
---
# <a name="migrate-an-android-app-from-google-maps"></a>Android uygulamasını Google Haritalar'dan geçirme

Azure Haritalar Android SDK,Web SDK'ya benzer bir API arabirimine sahiptir. Bu SDK'lardan biriyle geliştirdiyseniz, aynı kavramların, en iyi uygulamaların ve mimarilerin çoğu geçerlidir.

Azure Haritalar Android SDK API 21 minimum Android sürümünü destekler: Android 5.0.0 (Lollipop).

Tüm örnekler Java'da verilmiştir; ancak Kotlin'i Azure Maps Android SDK ile kullanabilirsiniz.

Azure Haritalar tarafından Android SDK ile geliştirme hakkında daha fazla bilgi için [Azure Haritalar Android SDK için Nasıl YapIlir kılavuzlarına](how-to-use-android-map-control-library.md)bakın.

## <a name="load-a-map"></a>Harita yükleme

Google veya Azure Haritalar'ı kullanarak bir Android uygulamasında harita yüklemek de benzer adımlardan oluşur. SDK'yı kullanırken şunları kullanmalısınız:

- Her iki platforma da erişmek için bir API veya abonelik anahtarı alın.
- Haritanın nerede işlenmeli ve nasıl oluşturulacağını belirtmek için bir Aktiviteye biraz XML ekleyin.
- Harita görünümünü içeren Etkinlik'ten harita sınıfındaki ilgili yöntemlere kadar tüm yaşam döngüsü yöntemlerini geçersiz kılın. Özellikle, aşağıdaki yöntemleri geçersiz kılmanız gerekir:
    - `onCreate(Bundle)`
    - `onStart()`
    - `onResume()`
    - `onPause()`
    - `onStop()`
    - `onDestroy()`
    - `onSaveInstanceState(Bundle)`
    - `onLowMemory()`
- Haritaya erişmeye ve programlamaya çalışmadan önce haritanın hazır olmasını bekleyin.

**Önce: Google Haritalar**

Android için Google Maps SDK'yı kullanarak bir harita görüntülemek için aşağıdaki adımlar yapılır:

1.  Google Play hizmetlerinin yüklü olduğundan emin olun.
2.  Modülün **gradle.build** dosyasına Google Haritalar hizmeti için bir bağımlılık ekleyin: 

    `implementation 'com.google.android.gms:play-services-maps:17.0.0'`

1.  Google Maps **\_\_api.xml** dosyasının uygulama bölümüne bir Google Haritalar API anahtarı ekleyin:
    
    ```xml
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR_GOOGLE_MAPS_KEY"/>
    ```

1.  Ana aktiviteye bir harita parçası ekleyin:

    ```xml
    <com.google.android.gms.maps.MapView
            android:id="@+id/myMap"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>
    ```

1.  **MainActivity.java** dosyasında, Google Haritalar SDK'yı almanız gerekir. Harita görünümünü içeren etkinlikten tüm yaşam döngüsü yöntemlerini harita sınıfındaki ilgili lere iletin. Yöntemi `MapView` kullanarak harita parçasından bir örnek alın. `getMapAsync(OnMapReadyCallback)` Otomatik `MapView` olarak haritalar sistemi ve görünümü başharf. **MainActivity.java** dosyasını aşağıdaki gibi edin:

    ```java
    import com.google.android.gms.maps.GoogleMap;
    import com.google.android.gms.maps.MapView;
    import com.google.android.gms.maps.OnMapReadyCallback;
    
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    
    public class MainActivity extends AppCompatActivity implements OnMapReadyCallback {
    
        MapView mapView;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapView = findViewById(R.id.myMap);
    
            mapView.onCreate(savedInstanceState);
            mapView.getMapAsync(this);
        }
    
        @Override
    
        public void onMapReady(GoogleMap map) {
            //Add your post map load code here.
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapView.onResume();
        }
    
        @Override
        protected void onStart(){
            super.onStart();
            mapView.onStart();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapView.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapView.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapView.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapView.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapView.onSaveInstanceState(outState);
        }
    }
    ```

Bir uygulamayı çalıştırdığınızda, harita denetimi aşağıdaki resimdeki gibi yüklenir.

<center>

![Basit Google Haritalar](media/migrate-google-maps-android-app/simple-google-maps.png)</center>

**Sonra: Azure Haritalar**

Android için Azure Maps SDK'yı kullanarak bir harita görüntülemek için aşağıdaki adımların yapılması gerekir:

1. Üst düzey **build.gradle** dosyasını açın ve **tüm projeler** blok bölümüne aşağıdaki kodu ekleyin:

    ```JAVA
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. **Uygulamanızı/build.gradle'nizi** güncelleyin ve aşağıdaki kodu ekleyin:
    
    1. Projenizin **minSdkVersion'unAPI** 21 veya daha yüksek olduğundan emin olun.

    2. Android bölümüne aşağıdaki kodu ekleyin:

        ```java
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Bağımlılık bloğunu güncelleştirin. En son Azure Haritalar Android SDK için yeni bir uygulama bağımlılık satırı ekleyin:

        ```java
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

        > [!Note]
        > Azure Haritalar Android SDK düzenli olarak yükseltilir ve geliştirilir. En son Azure Haritalar sürüm numarasını almak için [Android harita denetimiyle başlarken](how-to-use-android-map-control-library.md) görebilirsiniz. Ayrıca, sürüm numarasını "0,2"den "0+"ya ayarlayabilirsiniz ve kodunuzu her zaman en son sürüme yönlendirebilirsiniz.
    
    4. Araç çubuğunda **Dosya'ya** gidin ve ardından **Gradle Dosyalarıyla Projeyi Eşitle'yi**tıklatın.
3. Ana aktiviteye bir harita parçası \> \> ekleyin\_(kaynak düzeni etkinliği main.xml):
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            />
    </FrameLayout>
    ```

4. **MainActivity.java** dosyasında şunları yapmanız gerekir:
    
    * Azure Haritalar SDK'sını içe aktarım
    * Azure Haritalar kimlik doğrulama bilgilerinizi ayarlama
    * **onCreate** yönteminde harita denetimi örneğini alma

     Sınıftaki `AzureMaps` kimlik doğrulama bilgilerini `setSubscriptionKey` veya `setAadProperties` yöntemleri kullanarak ayarlayın. Bu genel güncelleştirme, kimlik doğrulama bilgilerinizi her görünüme eklediğinizden emin olun.

    Harita denetimi, Android'in OpenGL yaşam döngüsünü yönetmek için kendi yaşam döngüsü yöntemlerini içerir. Bu yöntemler doğrudan içerdiği Etkinlik'ten çağrılmalıdır. Harita denetiminin yaşam döngüsü yöntemlerini doğru şekilde aramak için, etkinlikte harita denetimini içeren aşağıdaki yaşam döngüsü yöntemlerini geçersiz kılmanız gerekir. İlgili harita kontrol yöntemini arayın.

    * `onCreate(Bundle)` 
    * `onStart()` 
    * `onResume()` 
    * `onPause()` 
    * `onStop()` 
    * `onDestroy()` 
    * `onSaveInstanceState(Bundle)` 
    * `onLowMemory()`

    **MainActivity.java** dosyasını aşağıdaki gibi edin:
    
    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
        
        static {
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);
    
            //Wait until the map resources are ready.
            mapControl.onReady(map -> {
                //Add your post map load code here.
    
            });
        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        protected void onStart(){
            super.onStart();
            mapControl.onStart();
        }

        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }

        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }

        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }

        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    }
    ```

Uygulamanızı çalıştırarsanız, harita denetimi aşağıdaki resimdeki gibi yüklenir.

<center>

![Basit Azure Haritaları](media/migrate-google-maps-android-app/simple-azure-maps.png)</center>

Azure Haritalar denetiminin daha fazla uzaklaştırmayı desteklediğine ve daha fazla dünya görüşü sağladığına dikkat edin.

> [!TIP]
> Windows makinesinde Android emülatörü kullanıyorsanız, harita OpenGL ve yazılım hızlandırılmış grafik oluşturma yla çakışmaları nedeniyle işlemeyebilir. Aşağıdaki, bazı insanlar için, bu sorunu çözmek için çalıştı. AVD Yöneticisi'ni açın ve sanal aygıtı seçin. **Yapılandırmayı Doğrula** panelinde aşağı kaydırın. Taklit **Performans** bölümünde, **Grafik** seçeneğini **Donanım**olarak ayarlayın.

## <a name="localizing-the-map"></a>Haritayı yerelleştirme

Hedef kitleniz birden fazla ülkeye yayılmışsa veya farklı diller konuşuyorsa yerelleştirme önemlidir.

**Önce: Google Haritalar**

Haritanın dilini ayarlamak `onCreate` için yönteme aşağıdaki kodu ekleyin. Haritanın bağlam görünümünü ayarlamadan önce kod eklenmelidir. "Fr" dil kodu dili Fransızca ile sınırlar.

```java
String languageToLoad = "fr";
Locale locale = new Locale(languageToLoad);
Locale.setDefault(locale);
Configuration config = new Configuration();
config.locale = locale;
getBaseContext().getResources().updateConfiguration(config,
        getBaseContext().getResources().getDisplayMetrics());
```

Burada dil "fr" olarak ayarlanmış Google Maps bir örnektir.

<center>

![Google Haritalar yerelleştirme](media/migrate-google-maps-android-app/google-maps-localization.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar, dili ve haritanın bölgesel görünümünü ayarlamak için üç farklı yol sağlar. İlk seçenek, dil ve bölgesel görünüm bilgilerini `AzureMaps` sınıfa aktarmaktır. Bu seçenek, `setLanguage` genel `setView` olarak statik ve yöntemleri kullanır. Anlamı, varsayılan dil ve bölgesel görünüm, uygulamanızda yüklenen tüm Azure Haritalar denetimlerinde ayarlanır. Bu örnekte Fransızca "fr-FR" dil kodunu kullanarak ayarlar.

```java
static {
    //Set your Azure Maps Key.
    AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");

    //Set the language to be used by Azure Maps.
    AzureMaps.setLanguage("fr-FR");

    //Set the regional view to be used by Azure Maps.
    AzureMaps.setView("auto");
}
```

İkinci seçenek, dili geçmek ve bilgileri harita denetimi XML koduna görüntülemektir.

```xml
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_language="fr-FR"
    app:mapcontrol_view="auto"
    />
```

Üçüncü seçenek, haritalar `setStyle` yöntemini kullanarak dil ve bölgesel harita görünümünü programlamaktır. Bu seçenek, kodun yürütüldeğinde dili ve bölgesel görünümü güncelleştirir.

```java
mapControl.onReady(map -> {
    map.setStyle(StyleOptions.language("fr-FR"));
    map.setStyle(StyleOptions.view("auto"));
});
```

Burada "fr-FR" olarak ayarlanmış dil ile Azure Haritalar bir örnektir.

<center>

![Azure Haritalar yerelleştirme](media/migrate-google-maps-android-app/azure-maps-localization.png)</center>

Desteklenen dillerin tam listesini gözden [geçirin.](supported-languages.md)

## <a name="setting-the-map-view"></a>Harita görünümünü ayarlama

Hem Azure Haritalar'daki hem de Google Haritalar'daki dinamik haritalar, uygun yöntemler çağırılarak programlı bir şekilde yeni coğrafi konumlara taşınabilir. Haritayı uydu havadan görüntülerini gösterelim, haritayı koordinatları olan bir konuma ortalayalım ve yakınlaştırma düzeyini değiştirelim. Bu örnek için enlem kullanacağız: 35.0272, boylam: -111.0225 ve zoom düzeyi 15.

**Önce: Google Haritalar**

Google Haritalar harita denetiminin kamerası `moveCamera` bu yöntem kullanılarak programlanabilir. Yöntem, `moveCamera` haritanın merkezini ve yakınlaştırma düzeyini belirtmenizi sağlar. Yöntem, `setMapType` görüntülenecek harita türünü değiştirir.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.moveCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(35.0272, -111.0225), 15));
    mapView.setMapType(GoogleMap.MAP_TYPE_SATELLITE);
}
```

<center>

![Google Haritalar görünümü ayarlı](media/migrate-google-maps-android-app/google-maps-set-view.png)</center>

> [!NOTE]
> Google Haritalar boyutları 256 piksel, Azure Haritalar ise daha büyük bir 512 piksel kutucuk kullanır. Bu, Azure Haritalar'ın Google Haritalar'la aynı harita alanını yüklemek için ihtiyaç duyduğu ağ isteklerinin sayısını azaltır. Google Haritalar'da bir haritayla aynı görüntülenebilir alanı elde etmek için, Azure Haritalar'ı kullanırken Google Haritalar'da kullanılan yakınlaştırma düzeyini tek tek çıkarmanız gerekir. 

**Sonra: Azure Haritalar**

Daha önce de belirtildiği gibi, Azure Haritalar'da aynı görüntülenebilir alana ulaşmak için Google Haritalar'da kullanılan yakınlaştırma düzeyini bir kişi çıkarın. Bu durumda, 14 zoom düzeyi kullanın.

İlk harita görünümü, harita denetiminde XML özniteliklerinde ayarlanabilir.

```xml
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_cameraLat="35.0272"
    app:mapcontrol_cameraLng="-111.0225"
    app:mapcontrol_zoom="14"
    app:mapcontrol_style="satellite"
    />
```

Harita görünümü haritalar `setCamera` ve `setStyle` yöntemler kullanılarak programlanabilir.

```java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(35.0272, -111.0225), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![Azure Haritalar görünümü ayarlı](media/migrate-google-maps-android-app/azure-maps-set-view.png)</center>

**Ek kaynaklar:**

- [Desteklenen eşleme stilleri](supported-map-styles.md)

## <a name="adding-a-marker"></a>İşaretçi ekleme

Nokta verileri genellikle haritadaki bir resim kullanılarak işlenir. Bu görüntülerişaretçiler, pıcıkonlar, iğneler veya semboller olarak adlandırılır. Aşağıdaki örnekler, işaretverilerini haritaüzerinde enlemde belirteçolarak işler: 51.5, boylam: -0.2.

**Önce: Google Haritalar**

Google Haritalar ile işaretleyiciler haritalar `addMarker` yöntemi kullanılarak eklenir.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.addMarker(new MarkerOptions().position(new LatLng(47.64, -122.33)));
}
```

<center>

![Google Haritalar işareti](media/migrate-google-maps-android-app/google-maps-marker.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar'da, verileri önce bir veri kaynağına ekleyerek haritadaki nokta verilerini işleyin. Sonra, bu veri kaynağını bir sembol katmanına bağlamak. Veri kaynağı, harita denetimindeki uzamsal verilerin yönetimini optimize eder. Sembol katmanı, görüntü veya metin olarak kullanarak nokta verilerinin nasıl işlenir olduğunu belirtir.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create a point feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

    //Create a symbol layer and add it to the map.
    map.layers.add(new SymbolLayer(dataSource));
});
```

<center>

![Azure Haritalar işaretçisi](media/migrate-google-maps-android-app/azure-maps-marker.png)</center>

## <a name="adding-a-custom-marker"></a>Özel işaretçi ekleme

Özel görüntüler, haritadaki noktaları temsil etmek için kullanılabilir. Aşağıdaki örneklerdeki harita, haritada bir noktayı görüntülemek için özel bir resim kullanır. Nokta enlem: 51.5 ve boylam: -0.2. Bağlantı işaretçinin konumunu dengeler, böylece retam simgesinin noktası haritadaki doğru konumla hizalanır.

<center>

![sarı pushpin görüntü](media/migrate-google-maps-web-app/ylw_pushpin.png)<br/>
ylw\_pushpin.png</center>

Her iki örnekte de, yukarıdaki resim uygulama kaynaklarının çizilebilir klasörüne eklenir.

**Önce: Google Haritalar**

Google Haritalar'da işaretçiler için özel resimler kullanılabilir. İşaretleyici `icon` seçeneğini kullanarak özel görüntüler yükleyin. Görüntünün noktasını koordinatla hizalamak için `anchor` seçeneği kullanın. Çapa görüntünün boyutlarına göredir. Bu durumda, çapa 0,2 birim genişliğinde ve 1 birim yüksekliğindedir.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.addMarker(new MarkerOptions().position(new LatLng(47.64, -122.33))
    .icon(BitmapDescriptorFactory.fromResource(R.drawable.ylw_pushpin))
    .anchor(0.2f, 1f));
}
```

<center>

![Google Haritalar özel işaretçisi](media/migrate-google-maps-android-app/google-maps-custom-marker.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar'daki sembol katmanları özel görüntüleri destekler, ancak önce görüntünün harita kaynaklarına yüklenmesi ve benzersiz bir kimlik atanması gerekir. Daha sonra, sembol katmanıbu kimliği referans gerekir. `iconOffset` Seçeneği kullanarak görüntüdeki doğru noktaya hizalamak için sembolü ofset. Simge ofset piksel. Varsayılan olarak, ofset görüntünün alt merkezine göredir, ancak bu ofset `iconAnchor` değeri seçeneği kullanılarak ayarlanabilir. Bu örnek, `iconAnchor` `"center"`seçeneğini . Görüntüyü beş piksel sağa, 15 pikseli de rendeleme görüntüsünün noktasına hizalamak için taşımak için bir simge ofset kullanır.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create a point feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

    //Load the custom image icon into the map resources.
    map.images.add("my-yellow-pin", R.drawable.ylw_pushpin);

    //Create a symbol that uses the custom image icon and add it to the map.
    map.layers.add(new SymbolLayer(dataSource,
        iconImage("my-yellow-pin"),
        iconAnchor(AnchorType.CENTER
        iconOffset(new Float[]{5f, -15f})));
});
```

<center>

![Azure Haritalar özel işaretçisi](media/migrate-google-maps-android-app/azure-maps-custom-marker.png)</center>

## <a name="adding-a-polyline"></a>Polyline ekleme

Çok çizgiler, haritadaki bir satırı veya yolu temsil etmek için kullanılır. Aşağıdaki örnekler, haritada kesik li bir poliçizginin nasıl oluşturulabildiğini gösterir.

**Önce: Google Haritalar**

Google Haritalar'da `PolylineOptions` sınıfı kullanarak bir poliçizgi işleyin. `addPolyline` Yöntemi kullanarak haritaya polyline ekleyin. `color` Kontur rengini seçeneği kullanarak ayarlayın. `width` Kontur genişliğini seçeneği kullanarak ayarlayın. `pattern` Seçeneği kullanarak bir kontur çizgi dizisi ekleyin.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    //Create the options for the polyline.
    PolylineOptions lineOptions = new PolylineOptions()
            .add(new LatLng(46, -123))
            .add(new LatLng(49, -122))
            .add(new LatLng(46, -121))
            .color(Color.RED)
            .width(10f)
            .pattern(Arrays.<PatternItem>asList(
                    new Dash(30f), new Gap(30f)));

    //Add the polyline to the map.
    Polyline polyline = mapView.addPolyline(lineOptions);
}
```

<center>

![Google Haritalar polyline](media/migrate-google-maps-android-app/google-maps-polyline.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar'da çok `LineString` satırlar çağrılır veya `MultiLineString` nesneler. Bu nesneleri bir veri kaynağına ekleyin ve bir satır katmanı kullanarak işleyin. `strokeWidth` Kontur genişliğini seçeneği kullanarak ayarlayın. `strokeDashArray` Seçeneği kullanarak bir kontur çizgi dizisi ekleyin.

Azure Haritalar Web SDK'daki kontur genişliği ve tire dizisi "piksel" birimleri, Google Haritalar hizmetiyle aynıdır. Her ikisi de aynı sonuçları üretmek için aynı değerleri kabul.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create an array of points.
    List<Point> points = Arrays.asList(
        Point.fromLngLat(-123, 46),
        Point.fromLngLat(-122, 49),
        Point.fromLngLat(-121, 46));

    //Create a LineString feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(LineString.fromLngLats(points)));

    //Create a line layer and add it to the map.
    map.layers.add(new LineLayer(dataSource,
        strokeColor("red"),
        strokeWidth(4f),
        strokeDashArray(new Float[]{3f, 3f})));
});
```

<center>

![Azure Haritalar polyline](media/migrate-google-maps-android-app/azure-maps-polyline.png)</center>

## <a name="adding-a-polygon"></a>Çokgen ekleme

Çokgenler haritaüzerinde bir alanı temsil etmek için kullanılır. Sonraki örnekler, çokgenin nasıl oluşturulabildiğini gösterir. Bu çokgen haritanın merkez koordinatına dayalı bir üçgen oluşturur.

**Önce: Google Haritalar**

Google Haritalar ile `PolygonOptions` sınıfı kullanarak bir çokgen işleyin. `addPolygon` Yöntemi kullanarak haritaya çokgen ekleyin. Dolgu ve kontur renklerini sırasıyla `fillColor` seçenekleri `strokeColor` kullanarak ayarlayın. `strokeWidth` Kontur genişliğini seçeneği kullanarak ayarlayın.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    //Create the options for the polygon.
    PolygonOptions polygonOptions = new PolygonOptions()
            .add(new LatLng(46, -123))
            .add(new LatLng(49, -122))
            .add(new LatLng(46, -121))
            .add(new LatLng(46, -123))  //Close the polygon.
            .fillColor(Color.argb(128, 0, 128, 0))
            .strokeColor(Color.RED)
            .strokeWidth(5f);

    //Add the polygon to the map.
    Polygon polygon = mapView.addPolygon(polygonOptions);
}
```

<center>

![Google Haritalar çokgen](media/migrate-google-maps-android-app/google-maps-polygon.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar'da, bir veri kaynağına nesneler ekleyin `Polygon` ve `MultiPolygon` bunları katmanlar kullanarak haritada işleyin. Çokgen bir tabakada çokgenin alanını işleyin. Çizgi katmanı kullanarak çokgenin anahatlarını işleyin. Kontur rengini ve `strokeColor` genişliğini `strokeWidth` ve seçenekleri kullanarak ayarlayın.

Azure Haritalar Web SDK'daki kontur genişliği ve tire dizisi "piksel" birimleri Google Haritalar'daki ilgili birimlerle hizalanır. Her ikisi de aynı değerleri kabul ve aynı sonuçları üretmek.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create an array of point arrays to create polygon rings.
    List<List<Point>> rings = Arrays.asList(Arrays.asList(
        Point.fromLngLat(-123, 46),
        Point.fromLngLat(-122, 49),
        Point.fromLngLat(-121, 46),
        Point.fromLngLat(-123, 46)));

    //Create a Polygon feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Polygon.fromLngLats(rings)));

    //Add a polygon layer for rendering the polygon area.
    map.layers.add(new PolygonLayer(dataSource,
        fillColor("green"),
        fillOpacity(0.5f)));

    //Add a line layer for rendering the polygon outline.
    map.layers.add(new LineLayer(dataSource,
        strokeColor("red"),
        strokeWidth(2f)));
});
```

<center>

![Azure Haritalar çokgen](media/migrate-google-maps-android-app/azure-maps-polygon.png)</center>

## <a name="overlay-a-tile-layer"></a>Döşeme katmanını kaplama

 Haritalar döşeme sistemiyle uyumlu küçük karo görüntülere ayrılmış katman görüntülerini kaplamak için Döşeme katmanlarını kullanın. Bu yaklaşım, katman görüntüleri veya büyük veri kümeleri böldürme ortak bir yoludur. Döşeme katmanları, Google Haritalar'da Resim kaplamaları olarak bilinir.

Aşağıdaki örnekler Iowa State Üniversitesi Iowa Çevre Mesonet bir hava radar kiremit tabakası bindirme. Döşemeler 256 piksel boyutundadır.

**Önce: Google Haritalar**

Google Haritalar ile haritanın üzerine bir döşeme katmanı kaplanabilir. `TileOverlayOptions` Sınıfı kullan. Yöntemi kullanarak haritaya döşeme katmanı `addTileLauer` ekleyin. Döşemeleri yarı saydam yapmak `transparency` için seçenek 0,2 veya %20 saydam olarak ayarlanır.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    //Create the options for the tile layer.
    TileOverlayOptions tileLayer = new TileOverlayOptions()
        .tileProvider(new UrlTileProvider(256, 256) {
            @Override
            public URL getTileUrl(int x, int y, int zoom) {

                try {
                    //Define the URL pattern for the tile images.
                    return new URL(String.format("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/%d/%d/%d.png", zoom, x, y));
                }catch (MalformedURLException e) {
                    throw new AssertionError(e);
                }
            }
        }).transparency(0.2f);

    //Add the tile layer to the map.
    mapView.addTileOverlay(tileLayer);
}
```

<center>

![Google Haritalar döşeme katmanı](media/migrate-google-maps-android-app/google-maps-tile-layer.png)</center>

**Sonra: Azure Haritalar**

Bir döşeme katmanı haritaya diğer katmanlarda olduğu gibi benzer bir şekilde eklenebilir. X, y ve yakınlaştırma yer tutucuları olan biçimlendirilmiş bir URL; `{x}`, `{y}` `{z}` , sırasıyla, katmana kutucuklara nerede erişeceğini söylemek için kullanılır. Ayrıca, Azure Haritalar'daki `{quadkey}`döşeme `{bbox-epsg-3857}`katmanları `{subdomain}` ve yer tutucuları destekler. Kiremit tabakasını yarı saydam hale getirmek için 0,8 opaklık değeri kullanılır. Opaklık ve saydamlık, benzer olsa da, ters değerleri kullanır. Her iki seçenek arasında dönüştürme yapmak için, değerlerini bir numaradan çıkarın.

> [!TIP]
> Azure Haritalar'da, temel harita katmanları da dahil olmak üzere katmanları diğer katmanların altında işlemek uygundur. Ayrıca, harita etiketlerinin altındaki döşeme katmanlarını kolayca okunması için işlemek genellikle istenir. Yöntem, `map.layers.add` yeni katmanı eklemek için katmanın kimliği olan ikinci bir parametre alır. Harita etiketlerinin altına bir döşeme katmanı eklemek için aşağıdaki kod kullanılabilir:`map.layers.add(myTileLayer, "labels");`

```java
mapControl.onReady(map -> {
    //Add a tile layer to the map, below the map labels.
    map.layers.add(new TileLayer(
        tileUrl("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png"),
        opacity(0.8f),
        tileSize(256)
    ), "labels");
});
```

<center>

![Azure Haritalar döşeme katmanı](media/migrate-google-maps-android-app/azure-maps-tile-layer.png)</center>

## <a name="show-traffic"></a>Trafiği gösterme

Hem Azure Haritalar hem de Google haritalar trafik verilerini kaplama seçeneklerine sahiptir.

**Önce: Google Haritalar**

Google Haritalar ile trafik akışı verileri, haritanın `setTrafficEnabled` yöntemine uygun olarak geçerek haritanın üzerine yerlenebilir.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.setTrafficEnabled(true);
}
```

<center>

![Google Haritalar trafiği](media/migrate-google-maps-android-app/google-maps-traffic.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar, trafiği görüntülemek için birkaç farklı seçenek sunar. Yol kapatmalar ve kazalar gibi trafik olayları haritada simgeler olarak görüntülenebilir. Trafik akışı ve renk kodlu yollar harita üzerinde yer kaplanmış olabilir. Renkler, deftere nakledilen hız sınırına göre normal beklenen gecikmeye veya mutlak gecikmeye göre değiştirilebilir. Azure Haritalar'daki olay verileri her dakika güncelleştirilir ve akış verileri her iki dakikada bir güncelleştirilir.

```java
mapControl.onReady(map -> {
    map.setTraffic(
        incidents(true),
        flow(TrafficFlow.RELATIVE));
});
```

<center>

![Azure Haritalar trafiği](media/migrate-google-maps-android-app/azure-maps-traffic.png)</center>

## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar Android SDK hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Android harita denetimi nasıl kullanılır?](how-to-use-android-map-control-library.md)
