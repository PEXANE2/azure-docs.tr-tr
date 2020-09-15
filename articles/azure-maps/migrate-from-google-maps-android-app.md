---
title: 'Öğretici: Android uygulaması geçirme | Microsoft Azure haritaları'
description: Android uygulamasını Google Maps 'tan Microsoft Azure Maps 'a geçirme.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: fe67364ef51248d04cbc6095eb691ffe255fa02c
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90085884"
---
# <a name="migrate-an-android-app-from-google-maps"></a>Google Maps 'tan Android uygulaması geçirme

Azure Haritalar Android SDK, Web SDK 'sına benzer bir API arabirimine sahiptir. Bu SDK 'Lardan biriyle geliştirdiyseniz, aynı kavramların, en iyi uygulamaların ve mimarilerin birçoğu geçerlidir.

Azure Maps Android SDK, API 21 ' in en düşük Android sürümünü destekler: Android 5.0.0 (Lollipop).

Tüm örnekler Java 'da verilmiştir; Ancak, Azure Maps Android SDK Kotlin kullanabilirsiniz.

Azure haritalar tarafından Android SDK ile geliştirme hakkında daha fazla bilgi için bkz. [Azure Maps Için nasıl yapılır kılavuzlarıyla Android SDK](how-to-use-android-map-control-library.md).

## <a name="load-a-map"></a>Harita yükleme

Google veya Azure Maps kullanarak bir Android uygulamasına harita yükleme, benzer adımlardan oluşur. SDK kullanırken, şunları yapmanız gerekir:

- Her iki platforma de erişmek için bir API veya abonelik anahtarı alın.
- Haritanın nerede işleneceğini ve nasıl düzenleneceğini belirtmek için bir etkinliğe bazı XML ekleyin.
- Eşleme görünümünü içeren etkinlikten tüm yaşam döngüsü yöntemlerini Map sınıfında karşılık gelen yöntemlere geçersiz kılın. Özellikle, aşağıdaki yöntemleri geçersiz kılmanız gerekir:
    - `onCreate(Bundle)`
    - `onStart()`
    - `onResume()`
    - `onPause()`
    - `onStop()`
    - `onDestroy()`
    - `onSaveInstanceState(Bundle)`
    - `onLowMemory()`
- Eşlemeye ve programa erişmeye çalışmadan önce haritanın hazırlanmaya başlamasını bekleyin.

**Önce: Google Maps**

Android için Google Maps SDK 'sını kullanarak bir Haritayı göstermek için aşağıdaki adımlar yapılır:

1.  Google Play hizmetlerinin yüklü olduğundan emin olun.
2.  Google Maps hizmeti için modülün **Gradle. Build** dosyasına bir bağımlılık ekleyin: 

    `implementation 'com.google.android.gms:play-services-maps:17.0.0'`

1.  Google Maps  ** \_ \_api.xml** dosyasının uygulama bölümünde Google Maps API anahtarı ekleyin:
    
    ```xml
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR_GOOGLE_MAPS_KEY"/>
    ```

1.  Ana etkinliğe bir harita parçası ekleyin:

    ```xml
    <com.google.android.gms.maps.MapView
            android:id="@+id/myMap"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>
    ```

1.  **MainActivity. Java** dosyasında Google Maps SDK 'sını içeri aktarmanız gerekir. Harita görünümünü içeren etkinlikten tüm yaşam döngüsü yöntemlerini Map sınıfındaki ilgili olanlarla iletin. `MapView`Yöntemini kullanarak harita parçasındaki bir örnek alın `getMapAsync(OnMapReadyCallback)` . , `MapView` Haritalar sistemini ve görünümünü otomatik olarak başlatır. **MainActivity. Java** dosyasını aşağıdaki gibi düzenleyin:

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

Bir uygulamayı çalıştırdığınızda, eşleme denetimi aşağıdaki görüntüde olduğu gibi yüklenir.

<center>

![Basit Google haritaları](media/migrate-google-maps-android-app/simple-google-maps.png)</center>

**Sonrasında: Azure Maps**

Android için Azure Maps SDK 'sını kullanarak bir Haritayı göstermek için aşağıdaki adımların gerçekleştirilmesi gerekir:

1. En üst düzey **Build. Gradle** dosyasını açın ve aşağıdaki kodu **tüm projeler** engelleme bölümüne ekleyin:

    ```JAVA
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. **Uygulamanızı/Build. Gradle** öğesini güncelleştirin ve aşağıdaki kodu buna ekleyin:
    
    1. Projenizin **Minsdkversion** özelliğinin API 21 veya daha yüksek olduğundan emin olun.

    2. Aşağıdaki kodu Android bölümüne ekleyin:

        ```java
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Bağımlılıklar bloğunu güncelleştirin. En son Azure Maps Android SDK için yeni bir uygulama bağımlılığı satırı ekleyin:

        ```java
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

        > [!Note]
        > Azure Haritalar Android SDK düzenli olarak yükseltilir ve geliştirilir. En son Azure haritaları sürüm numarasını almak için [Android eşleme ile çalışmaya başlama denetimini](how-to-use-android-map-control-library.md) görebilirsiniz. Ayrıca, kodunuzun her zaman en son sürümü göstermesi için "0,2" olan sürüm numarasını "0 +" olarak ayarlayabilirsiniz.
    
    4. Araç çubuğunda **dosyasına** gidin ve ardından **projeyi Gradle dosyalarıyla Eşitle**' ye tıklayın.
3. Ana etkinliğe bir harita parçası ekleyin (kaynaklar \> Düzen \> etkinliği \_main.xml):
    
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

4. **MainActivity. Java** dosyasında şunları yapmanız gerekir:
    
    * Azure Haritalar SDK 'sını içeri aktarır
    * Azure haritalar kimlik doğrulama bilgilerinizi ayarlama
    * **OnCreate** yönteminde Map denetim örneğini al

     `AzureMaps`Veya yöntemlerini kullanarak sınıfındaki kimlik doğrulama bilgilerini ayarlayın `setSubscriptionKey` `setAadProperties` . Bu genel güncelleştirme, kimlik doğrulama bilgilerinizi her görünüme eklemediğinizden emin olun.

    Harita denetimi, Android 'ın OpenGL yaşam döngüsünü yönetmeye yönelik kendi yaşam döngüsü yöntemlerini içerir. Bu yöntemlerin doğrudan içerilen etkinlikten çağrılması gerekir. Harita denetiminin yaşam döngüsü yöntemlerini doğru şekilde çağırmak için, eşleme denetimini içeren etkinliğin aşağıdaki yaşam döngüsü yöntemlerini geçersiz kılmanız gerekir. İlgili harita denetim yöntemini çağırın.

    * `onCreate(Bundle)` 
    * `onStart()` 
    * `onResume()` 
    * `onPause()` 
    * `onStop()` 
    * `onDestroy()` 
    * `onSaveInstanceState(Bundle)` 
    * `onLowMemory()`

    **MainActivity. Java** dosyasını aşağıdaki gibi düzenleyin:
    
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

Uygulamanızı çalıştırırsanız, eşleme denetimi aşağıdaki görüntüde olduğu gibi yüklenir.

<center>

![Basit Azure haritaları](media/migrate-google-maps-android-app/simple-azure-maps.png)</center>

Azure haritalar denetiminin daha fazla yakınlaştırmayı desteklediğini ve bir dünya görünümü daha fazlasını sağladığını unutmayın.

> [!TIP]
> Bir Windows makinesinde Android öykünücüsü kullanıyorsanız, OpenGL ve yazılım hızlandırılmış grafik işleme ile çakışmalar nedeniyle eşleme işlenmeyebilir. Aşağıdakiler, bazı kişilerin bu sorunu çözmesi için çalışmıştır. AVD yöneticisini açın ve düzenlenecek sanal cihazı seçin. **Yapılandırmayı Doğrula** panelinde aşağı kaydırın. **Öykünülmüş performans** bölümünde **grafik** seçeneğini **donanım**olarak ayarlayın.

## <a name="localizing-the-map"></a>Haritayı yerelleştirme

Hedef kitleniz birden fazla ülkeye/bölgeye yayıldığında veya farklı dillerde konuşyayıldığında yerelleştirme önemlidir.

**Önce: Google Maps**

`onCreate`Eşleme dilini ayarlamak için yöntemine aşağıdaki kodu ekleyin. Haritanın bağlam görünümü ayarlamadan önce kodun eklenmesi gerekir. "Fr" dil kodu, dili Fransızca olarak sınırlandırır.

```java
String languageToLoad = "fr";
Locale locale = new Locale(languageToLoad);
Locale.setDefault(locale);
Configuration config = new Configuration();
config.locale = locale;
getBaseContext().getResources().updateConfiguration(config,
        getBaseContext().getResources().getDisplayMetrics());
```

Dilde "fr" olarak ayarlanan Google Maps örneğine bir örnek aşağıda verilmiştir.

<center>

![Google Haritalar yerelleştirme](media/migrate-google-maps-android-app/google-maps-localization.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar, eşlemenin dilini ve bölgesel görünümünü ayarlamak için üç farklı yol sunar. İlk seçenek, dil ve bölgesel görünüm bilgilerini sınıfa geçirmektir `AzureMaps` . Bu seçenek, `setLanguage` `setView` genel olarak statik ve yöntemleri kullanır. Anlamı, varsayılan dil ve bölgesel görünüm, uygulamanızda yüklü olan tüm Azure haritalar denetimleri arasında ayarlanır. Bu örnek, "fr-FR" dil kodunu kullanarak Fransızca ayarlar.

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

İkinci seçenek, dili geçirmektir ve harita denetimi XML koduna bilgileri görüntüler.

```xml
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_language="fr-FR"
    app:mapcontrol_view="auto"
    />
```

Üçüncü seçenek, eşleme yöntemi kullanılarak dili ve bölgesel harita görünümünü programlayadilleridir `setStyle` . Bu seçenek, dil ve bölgesel görünümü kod her yürütüldüğünde güncelleştirir.

```java
mapControl.onReady(map -> {
    map.setStyle(StyleOptions.language("fr-FR"));
    map.setStyle(StyleOptions.view("auto"));
});
```

Bu dilin "fr-FR" olarak ayarlandığı Azure Maps örneği aşağıda verilmiştir.

<center>

![Azure haritalar yerelleştirme](media/migrate-google-maps-android-app/azure-maps-localization.png)</center>

[Desteklenen dillerin](supported-languages.md)tüm listesini gözden geçirin.

## <a name="setting-the-map-view"></a>Harita görünümü ayarlanıyor

Azure haritalar ve Google Maps içindeki dinamik haritalar, uygun yöntemleri çağırarak program aracılığıyla yeni coğrafi konumlara taşınabilir. Haritada uydu havadan görüntüleme, Haritayı koordinatlarla bir konum üzerine ortalayın ve yakınlaştırma düzeyini değiştirin. Bu örnekte, Enlem: 35,0272, Boylam:-111,0225 ve 15 ' in yakınlaştırma düzeyini kullanacağız.

**Önce: Google Maps**

Google Maps harita denetimi Kamerası, yöntemi kullanılarak programlı bir şekilde taşınabilir `moveCamera` . `moveCamera`Yöntemi, haritanın merkezini ve yakınlaştırma düzeyini belirtmenize olanak tanır. `setMapType`Yöntemi, harita türünü görüntülenecek şekilde değiştirir.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.moveCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(35.0272, -111.0225), 15));
    mapView.setMapType(GoogleMap.MAP_TYPE_SATELLITE);
}
```

<center>

![Google Maps kümesi görünümü](media/migrate-google-maps-android-app/google-maps-set-view.png)</center>

> [!NOTE]
> Azure haritalar daha büyük bir 512 piksel kutucuğu kullandığında, Google Maps boyutlarda 256 piksel olan kutucukları kullanır. Bu, Google Maps ile aynı harita alanını yüklemek için Azure haritalar tarafından gerek duyulan ağ isteklerinin sayısını azaltır. Google Maps 'lerdeki aynı görüntülenebilen alanı bir eşleme olarak elde etmek için, Google Maps 'ta kullanılan yakınlaştırma düzeyini Azure haritalar kullanırken bir tane olarak çıkarmanız gerekir. 

**Sonrasında: Azure Maps**

Daha önce belirtildiği gibi, Azure Maps 'ta aynı görüntülenebilir alan elde etmek için Google Maps 'ta kullanılan yakınlaştırma düzeyini bir tane olarak çıkar. Bu durumda, 14 yakınlaştırma düzeyi kullanın.

İlk harita görünümü harita denetimindeki XML özniteliklerinde ayarlanabilir.

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

Harita görünümü haritalar `setCamera` ve yöntemler kullanılarak programlanabilir `setStyle` .

```java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(35.0272, -111.0225), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![Azure haritalar kümesi görünümü](media/migrate-google-maps-android-app/azure-maps-set-view.png)</center>

**Ek kaynaklar:**

- [Desteklenen eşleme stilleri](supported-map-styles.md)

## <a name="adding-a-marker"></a>İşaretleyici ekleme

Nokta verileri genellikle haritadaki bir görüntü kullanılarak işlenir. Bu görüntüler işaretçiler, Pushpin, pin veya semboller olarak adlandırılır. Aşağıdaki örneklerde, nokta verileri, eşleme üzerinde, Enlem: 51,5, Boylam:-0,2 konumunda işaretçiler olarak işlenir.

**Önce: Google Maps**

Google Maps ile işaretçiler, Maps yöntemi kullanılarak eklenir `addMarker` .

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.addMarker(new MarkerOptions().position(new LatLng(47.64, -122.33)));
}
```

<center>

![Google Haritalar işaretçisi](media/migrate-google-maps-android-app/google-maps-marker.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da, önce verileri bir veri kaynağına ekleyerek haritada nokta verileri işleme. Ardından, bu veri kaynağını bir sembol katmanına bağlama. Veri kaynağı, harita denetimindeki uzamsal verilerin yönetimini iyileştirir. Sembol katmanı, nokta verilerinin görüntü veya metin olarak kullanılarak nasıl işleneceğini belirtir.

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

![Azure haritalar işaretleyicisi](media/migrate-google-maps-android-app/azure-maps-marker.png)</center>

## <a name="adding-a-custom-marker"></a>Özel işaretleyici ekleme

Özel görüntüler, bir haritadaki noktaları temsil etmek için kullanılabilir. Aşağıdaki örneklerde yer alan eşleme, haritada bir noktayı göstermek için özel bir görüntü kullanır. Bu nokta Enlem: 51,5 ve Boylam:-0,2 ' dir. Tutturucu, işaretin konumunu, raptiye simgesinin noktasının haritada doğru konuma göre hizalanmasını sağlayacak şekilde kaydırır.

<center>

![sarı raptiye resmi](media/migrate-google-maps-web-app/yellow-pushpin.png)<br/>
yellow-pushpin.png</center>

Her iki örnekte de yukarıdaki görüntü, uygulamalar kaynaklarının çizilebilir klasörüne eklenir.

**Önce: Google Maps**

Google Maps ile, işaretçiler için özel görüntüler kullanılabilir. İşaretleyici seçeneğini kullanarak özel görüntüleri yükleyin `icon` . Görüntünün noktasını koordinatya hizalamak için `anchor` seçeneğini kullanın. Tutturucu, görüntünün boyutlarına göredir. Bu durumda, bağlayıcı 0,2 birim genişliğinde ve 1 birim yüksekliğinde olur.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.addMarker(new MarkerOptions().position(new LatLng(47.64, -122.33))
    .icon(BitmapDescriptorFactory.fromResource(R.drawable.yellow-pushpin))
    .anchor(0.2f, 1f));
}
```

<center>

![Google Maps özel işaretçisi](media/migrate-google-maps-android-app/google-maps-custom-marker.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'daki sembol katmanları özel görüntüleri destekler, ancak önce görüntünün harita kaynaklarına yüklenmesi ve benzersiz bir KIMLIK atanması gerekir. Ardından, sembol katmanının bu KIMLIĞE başvurması gerekir. Seçeneği kullanarak görüntüdeki doğru noktaya hizalamak için simgeyi kaydırın `iconOffset` . Simge boşluğu piksel cinsinden olur. Varsayılan olarak, fark görüntünün alt merkezine göredir, ancak bu fark değeri seçeneği kullanılarak ayarlanabilir `iconAnchor` . Bu örnek `iconAnchor` seçeneğini olarak ayarlar `"center"` . Bir simge sapmasını kullanarak, raptiye görüntüsünün noktasıyla hizalamak için görüntüyü beş piksel sağa ve 15 piksele taşıyın.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create a point feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

    //Load the custom image icon into the map resources.
    map.images.add("my-yellow-pin", R.drawable.yellow_pushpin);

    //Create a symbol that uses the custom image icon and add it to the map.
    map.layers.add(new SymbolLayer(dataSource,
        iconImage("my-yellow-pin"),
        iconAnchor(AnchorType.CENTER
        iconOffset(new Float[]{5f, -15f})));
});
```

<center>

![Azure haritalar özel işaretçisi](media/migrate-google-maps-android-app/azure-maps-custom-marker.png)</center>

## <a name="adding-a-polyline"></a>Çoklu çizgi ekleme

Polylines, haritada bir çizgiyi veya yolu göstermek için kullanılır. Aşağıdaki örneklerde, haritada kesik çizgili bir çoklu çizginin nasıl oluşturulacağı gösterilmektedir.

**Önce: Google Maps**

Google Maps ile sınıfını kullanarak bir çoklu çizgi işleme koyun `PolylineOptions` . Yöntemini kullanarak, çoklu çizgiyi haritaya ekleyin `addPolyline` . Seçeneğini kullanarak kontur rengini ayarlayın `color` . Seçeneğini kullanarak kontur genişliğini ayarlayın `width` . Seçeneğini kullanarak bir vuruş tire dizisi ekleyin `pattern` .

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

![Google Haritalar çoklu çizgi](media/migrate-google-maps-android-app/google-maps-polyline.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da çoklu çizgilerin çizgisini, `LineString` veya nesneleri olarak adlandırılır `MultiLineString` . Bu nesneleri bir veri kaynağına ekleyin ve çizgi katmanı kullanarak bunları işleme koyun. Seçeneğini kullanarak kontur genişliğini ayarlayın `strokeWidth` . Seçeneğini kullanarak bir vuruş tire dizisi ekleyin `strokeDashArray` .

Azure Haritalar Web SDK 'sindeki kontur genişliği ve Dash dizisi "piksel" birimleri Google Maps hizmetindeki ile aynıdır. Aynı sonuçları oluşturmak için her iki aynı değeri de kabul edin.

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

![Azure haritalar çoklu çizgi](media/migrate-google-maps-android-app/azure-maps-polyline.png)</center>

## <a name="adding-a-polygon"></a>Çokgen ekleme

Çokgenler, haritadaki bir alanı temsil etmek için kullanılır. Sonraki örneklerde bir çokgen oluşturma gösterilmektedir. Bu Çokgen haritanın orta koordinasyonu temelinde bir üçgen oluşturur.

**Önce: Google Maps**

Google Maps ile sınıfı kullanarak bir çokgen işleme `PolygonOptions` . Yöntemini kullanarak çokgeni haritaya ekleyin `addPolygon` . Sırasıyla ve seçeneklerini kullanarak Fill ve Stroke renklerini `fillColor` ayarlayın `strokeColor` . Seçeneğini kullanarak kontur genişliğini ayarlayın `strokeWidth` .

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

![Google Haritalar Çokgen](media/migrate-google-maps-android-app/google-maps-polygon.png)</center>

**Sonrasında: Azure Maps**

Azure Maps ' de `Polygon` `MultiPolygon` bir veri kaynağına nesneler ekleyin ve bunları katman kullanarak haritada işleme koyun. Çokgen katmanındaki bir çokgen alanını işleme. Bir çizgi katmanını kullanarak bir çokgen ana hattını işleme. Ve seçeneklerini kullanarak kontur rengini ve genişliğini ayarlayın `strokeColor` `strokeWidth` .

Azure Haritalar Web SDK 'sindeki kontur genişliği ve kesik çizgi dizisi "piksel" birimleri Google Maps 'taki ilgili birimlerle hizalanır. Her ikisi de aynı değerleri kabul edin ve aynı sonuçları üretir.

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

![Azure haritalar Çokgen](media/migrate-google-maps-android-app/azure-maps-polygon.png)</center>

## <a name="overlay-a-tile-layer"></a>Döşeme katmanını kaplama

 Harita döşeme sistemiyle birlikte hizalanmış, daha küçük döşenmiş görüntülerle ayrılmış katman görüntülerinin kaplaması için döşeme katmanlarını kullanın. Bu yaklaşım, katman görüntülerini veya büyük veri kümelerini fazla kullanmanın yaygın bir yoludur. Döşeme katmanları, Google Maps 'ta görüntü Yerpaylaşımları olarak bilinir.

Aşağıdaki örnekler, Iowa çevresel Mesonet of Iowa çevre bir hava durumu radar kutucuk katmanını kaplamış. Kutucuklar 256 piksel boyutlardır.

**Önce: Google Maps**

Google Maps ile, bir döşeme katmanı haritanın üstünde yer alabilir. Sınıfını kullanın `TileOverlayOptions` . Yöntemi kullanarak haritaya kutucuk katmanını ekleyin `addTileLauer` . Kutucukları yarı saydam hale getirmek için, `transparency` seçenek 0,2 olarak veya %20 saydam olarak ayarlanır.

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

![Google Maps kutucuk katmanı](media/migrate-google-maps-android-app/google-maps-tile-layer.png)</center>

**Sonrasında: Azure Maps**

Bir kutucuk katmanı, diğer herhangi bir katmanda benzer bir şekilde haritaya eklenebilir. X, y ve zoom yer tutucuları olan biçimli bir URL; `{x}`, `{y}` `{z}` sırasıyla katmana nerede erişacaklarını bildirmek için kullanılır. Ayrıca, Azure haritalar 'daki döşeme katmanları `{quadkey}` , `{bbox-epsg-3857}` ve `{subdomain}` yer tutucuları destekler. Kutucuk katmanını yarı şeffaf hale getirmek için 0,8 opaklık değeri kullanılır. Opaklık ve saydamlık, benzer olmasına karşın ters çevrilmiş değerler kullanır. Her iki seçenek arasında dönüştürmek için, değerlerini bir sayıdan çıkarın.

> [!TIP]
> Azure haritalar 'da, temel harita katmanları dahil olmak üzere diğer katmanların altında katmanları işlemek kullanışlı olur. Ayrıca, daha kolay okunabilmeleri için harita etiketlerinin altında döşeme katmanlarını işlemek tercih edilir. `map.layers.add`Yöntemi, aşağıdaki yeni katmanın ekleneceği katmanın kimliği olan ikinci bir parametreyi alır. Harita etiketlerinin altına bir kutucuk katmanı eklemek için aşağıdaki kod kullanılabilir: `map.layers.add(myTileLayer, "labels");`

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

![Azure haritalar kutucuk katmanı](media/migrate-google-maps-android-app/azure-maps-tile-layer.png)</center>

## <a name="show-traffic"></a>Trafiği gösterme

Hem Azure Maps hem de Google Maps, trafik verilerinin yer paylaşımı için seçeneklere sahiptir.

**Önce: Google Maps**

Google Maps ile trafik akışı verileri haritanın en üstünde, eşlemenin yöntemine doğru geçirerek kaplama bulunabilir `setTrafficEnabled` .

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.setTrafficEnabled(true);
}
```

<center>

![Google Haritalar trafiği](media/migrate-google-maps-android-app/google-maps-traffic.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar, trafiği görüntülemek için çeşitli farklı seçenekler sağlar. Yol kapanışları ve kazalardan dolayı gibi trafik olayları haritada simgeler olarak görüntülenebilir. Trafik akışı ve renk kodlu yollar haritada yer alabilir. Renkler, normal beklenen gecikmeye veya mutlak gecikmeye göre, postalanan hız sınırına göre görüntülenecek şekilde değiştirilebilir. Azure haritalar 'daki olay verileri dakikada bir güncelleştirilir ve akış verileri her iki dakikada bir güncelleştirilir.

```java
mapControl.onReady(map -> {
    map.setTraffic(
        incidents(true),
        flow(TrafficFlow.RELATIVE));
});
```

<center>

![Azure haritalar trafiği](media/migrate-google-maps-android-app/azure-maps-traffic.png)</center>

## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar Android SDK hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Android harita denetimini kullanma](how-to-use-android-map-control-library.md)
