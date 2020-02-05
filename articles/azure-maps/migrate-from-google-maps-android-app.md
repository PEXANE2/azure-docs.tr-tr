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
ms.openlocfilehash: 6e54d8ea44b6c322f311cc1baeb6ca3ab6715aee
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989969"
---
# <a name="migrate-an-android-app-from-google-maps"></a>Google Maps 'tan Android uygulaması geçirme

Azure Haritalar Android SDK, Web SDK 'sına benzer bir API arabirimine sahiptir. Bu SDK 'Lardan biriyle geliştirdiyseniz, aynı kavramların, en iyi uygulamaların ve mimarilerin birçoğu geçerlidir. Bilginizi birinden diğerine kolayca aktarabilmeniz gerekir.

Azure Maps Android SDK, API 21 ' in en düşük Android sürümünü destekler: Android 5.0.0 (Lollipop).

Tüm örnekler Java 'da verilmiştir, ancak Kotlin Azure Maps Android SDK ile de kullanılabilir.

Azure haritalar tarafından Android SDK ile geliştirme hakkında daha fazla bilgi için bkz. [Azure Maps Için nasıl yapılır kılavuzlarıyla Android SDK](how-to-use-android-map-control-library.md).

## <a name="load-a-map"></a>Harita yükleme

Google veya Azure Maps kullanarak bir Android uygulamasında bir haritanın yüklenmesi aynı adımlardan birçoğundan oluşur. SDK kullanırken, şunları yapmanız gerekir:

- Her iki platforma de erişmek için bir API veya abonelik anahtarı alın.
- Haritanın nerede işleneceğini ve nasıl düzenleneceğini belirtmek için bir etkinliğe bazı XML ekleyin.
- Harita görünümünü içeren etkinlikten tüm yaşam döngüsü yöntemlerini Map sınıfındaki ilgili olanlarla iletin. Özellikle, aşağıdaki yöntemleri geçersiz kılmanız gerekir:
    - `onCreate(Bundle)`
    - `onStart()`
    - `onResume()`
    - `onPause()`
    - `onStop()`
    - `onDestroy()`
    - `onSaveInstanceState(Bundle)`
    - `onLowMemory()`
- Program aracılığıyla erişmeye çalışmadan önce haritanın hazırlanmaya başlamasını bekleyin.

**Önce: Google Maps**

Android için Google Maps SDK 'sını kullanarak bir Haritayı göstermek için aşağıdaki adımlar yapılır:

1.  Google Play hizmetlerinin yüklü olduğundan emin olun.
2.  Google Maps hizmeti için modülün **Gradle. Build** dosyasına bir bağımlılık ekleyin: 

    `implementation 'com.google.android.gms:play-services-maps:17.0.0'`

1.  Google **\_Maps\_API. xml** dosyasının uygulama bölümünde Google Maps API anahtarı ekleyin:
    
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

1.  MainActivity. Java dosyasında, Google Maps SDK için içeri aktarmalar eklemeniz gerekir. Harita görünümünü içeren etkinlikten tüm yaşam döngüsü yöntemlerini Map sınıfındaki ilgili olanlarla iletin. `MapView` bir örnek, `getMapAsync(OnMapReadyCallback)` yöntemi kullanılarak harita parçasından alınabilir. `MapView`, haritalar sistemini ve görünümünü otomatik olarak başlatır. **MainActivity. Java** dosyasını aşağıdaki gibi düzenleyin:

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

Bir uygulamayı çalıştırdığınızda, eşleme denetimi aşağıdaki şekilde yüklenir.

<center>

Basit Google Maps ![](media/migrate-google-maps-android-app/simple-google-maps.png)</center>

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

        ```JAVA
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Bağımlılıklar engellemeyi güncelleştirin ve en son Azure Maps Android SDK yeni bir uygulama bağımlılığı satırı ekleyin:

        ```JAVA
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

        > [!Note]
        > Azure Haritalar Android SDK düzenli olarak yükseltilmekte ve geliştirilir. En son Azure Maps sürüm numarasını almak için [Android harita denetimini kullanmaya](how-to-use-android-map-control-library.md) başlama belgesine bakabilirsiniz. Ayrıca, kodunuzun her zaman en son sürümü göstermesi için "0,2" olan sürüm numarasını "0 +" olarak ayarlayabilirsiniz.
    
    4. Araç çubuğunda **dosyasına** gidin ve ardından **projeyi Gradle dosyalarıyla Eşitle**' ye tıklayın.
3. Ana etkinliğe bir harita parçası ekleyin (kaynak \> düzen \> etkinlik\_Main. xml):
    
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
    
    * Azure Maps SDK için içeri aktarmalar ekleme
    * Azure haritalar kimlik doğrulama bilgilerinizi ayarlama
    * **OnCreate** yönteminde Map denetim örneğini al

    `setSubscriptionKey` veya `setAadProperties` yöntemler kullanılarak `AzureMaps` sınıftaki kimlik doğrulama bilgilerini genel olarak ayarlamak, kimlik doğrulama bilgilerinizi her görünüme eklemeniz gerekmez. 

    Harita denetimi, Android 'in OpenGL yaşam döngüsünü yönetmeye yönelik kendi yaşam döngüsü yöntemlerini içerir. Bu, doğrudan içeren etkinlikten çağrılmalıdır. Harita denetiminin yaşam döngüsü yöntemlerini doğru şekilde çağırmak için, Map denetimini içeren etkinliğin aşağıdaki yaşam döngüsü yöntemlerini geçersiz kılmanız ve ilgili Map denetim yöntemini çağırmanız gerekir. 

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

Uygulamanızı çalıştırırsanız, eşleme denetimi aşağıdaki gibi yüklenir.

<center>

![basit Azure Maps](media/migrate-google-maps-android-app/simple-azure-maps.png)</center>

Azure haritalar denetiminin daha fazla yakınlaştırmayı desteklediğini ve bir dünya görünümü daha fazlasını sağladığını unutmayın.

> [!TIP]
> Windows 'da bir Android öykünücüsü kullanıyorsanız, OpenGL, OpenGL ve yazılım hızlandırılmış grafik işleme ile çakışmalar nedeniyle işlenemiyor olabilir. Aşağıda, bu sorunu çözmek için bazıları çalıştı. AVD yöneticisini açın ve düzenlenecek sanal cihazı seçin. **Yapılandırmayı Doğrula** panelini aşağı kaydırın. **Öykünülmüş performans** bölümünde **grafik** seçeneğini **donanım**olarak ayarlayın.

## <a name="localizing-the-map"></a>Haritayı yerelleştirme

Hedef kitleniz birden çok ülkede yayıldığında veya farklı dillerde konuşabilçalışıyorsa, yerelleştirme önemlidir.

**Önce: Google Maps**

Aşağıdaki kodu ekleyerek, eşlemenin dili ana etkinliğin `onCreate` yönteminde ayarlanabilir. Haritanın bağlam görünümü ayarlamadan önce kodun eklenmesi gerekir. Aşağıda "fr" dil kodunu kullanarak dili Fransızca ile sınırlandırır.

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

![Google Maps yerelleştirme](media/migrate-google-maps-android-app/google-maps-localization.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar haritanın dilini ve bölgesel görünümünü ayarlamanın üç farklı yolunu sağlar. İlk seçenek, statik `setLanguage` ve `setView` yöntemlerini genel olarak kullanarak dil ve bölgesel görünüm bilgilerini `AzureMaps` sınıfına geçirmektir. Bu işlem, uygulamanıza yüklenen tüm Azure haritalar denetimlerinde varsayılan dil ve bölgesel görünüm ayarlar. Aşağıda, "fr-FR" dil kodunu kullanarak dili Fransızca ile sınırlandırır.

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

İkinci seçenek, dili geçirmektir ve harita denetim XML dosyasına bilgileri görüntüler.

```xml
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_language="fr-FR"
    app:mapcontrol_view="auto"
    />
```

Üçüncü seçenek, haritalar `setStyle` yöntemi kullanılarak eşlemenin dil ve bölgesel görünümünü programlı olarak ayarlamanıza olanak sağlar. Bu seçenek, eşlemenin dil ve bölgesel görünümünü değiştirmek için herhangi bir zamanda ayarlanabilir.

```java
mapControl.onReady(map -> {
    map.setStyle(StyleOptions.language("fr-FR"));
    map.setStyle(StyleOptions.view("auto"));
});
```

Bu dilin "fr-FR" olarak ayarlandığı Azure Maps örneği aşağıda verilmiştir.

<center>

![Azure Maps yerelleştirme](media/migrate-google-maps-android-app/azure-maps-localization.png)</center>

Desteklenen dillerin ve bölgesel görünümlerin tamamen listesi [burada](supported-languages.md)belgelenmiştir.

## <a name="setting-the-map-view"></a>Harita görünümü ayarlanıyor

Azure haritalar ve Google Maps içindeki dinamik haritalar, uygun yöntemleri çağırarak program aracılığıyla yeni coğrafi konumlara taşınabilir. Aşağıdaki örneklerde, haritanın uydu havadan görüntüleme, Haritayı koordinatlarla bir konum üzerine (Enlem: 35,0272, Boylam:-111,0225) nasıl görüntüleyeceği ve Google Maps 'ta yakınlaştırma düzeyini 15 olarak değiştirme işlemlerinin nasıl yapılacağı gösterilmektedir.

> [!NOTE]
> Azure haritalar daha büyük bir 512 piksel kutucuğu kullandığında, Google Maps boyutlarda 256 piksel olan kutucukları kullanır. Bu, Google Maps ile aynı harita alanını yüklemek için Azure haritalar tarafından gerek duyulan ağ isteklerinin sayısını azaltır. Ancak, Tile Pyramids 'in harita denetimlerinde çalışma yöntemi nedeniyle, Azure haritalar 'daki daha büyük Kutucuklar, Google Maps 'ta bir eşleme olarak aynı görüntülenebilir alana ulaşmak için, Azure haritalar kullanırken, Google Maps 'ta kullanılan yakınlaştırma düzeyini 1 ' e çıkarmanız gerektiği anlamına gelir. 

**Önce: Google Maps**

Google Maps harita denetimi Kamerası, eşlemenin merkezini ve yakınlaştırma düzeyini belirtmenize olanak tanıyan `moveCamera` yöntemi kullanılarak programlı bir şekilde taşınabilir. `setMapType` yöntemi, görüntülenen haritanın türünü değiştirmek için kullanılabilir.

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

**Sonrasında: Azure Maps**

Daha önce belirtildiği gibi, Azure haritalar 'da görüntülenebilen aynı alana ulaşmak için, Google Maps 'ta kullanılan yakınlaştırma düzeyini bir tane olarak çıkarın, bu durumda 14 yakınlaştırma düzeyi kullanılır.

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

Harita görünümü, haritalar `setCamera` ve `setStyle` yöntemleri kullanılarak programlı bir şekilde güncelleştirebilir.

```java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(35.0272, -111.0225), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![Azure Maps kümesi görünümü](media/migrate-google-maps-android-app/azure-maps-set-view.png)</center>

**Ek kaynaklar:**

- [Desteklenen harita stilleri](supported-map-styles.md)

## <a name="adding-a-marker"></a>İşaretleyici ekleme

Nokta verileri genellikle haritadaki bir görüntü kullanılarak haritada işlenir. Bu görüntüler genellikle işaretçiler, Pushpin, pin veya semboller olarak adlandırılır. Aşağıdaki örneklerde, nokta verileri haritadaki (Enlem: 51,5, Boylam:-0,2) işaretçiler olarak işlenir.

**Önce: Google Maps**

Google Maps ile, işaretçiler `addMarker` yöntemi kullanılarak işaretçiler eklenir.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.addMarker(new MarkerOptions().position(new LatLng(47.64, -122.33)));
}
```

<center>

![Google Maps işaretleyici](media/migrate-google-maps-android-app/google-maps-marker.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da, önce verileri bir veri kaynağına ekleyerek haritada veri noktası oluşturulabilir. Ardından, bu veri kaynağını bir sembol katmanına bağlama. Veri kaynağı, harita denetimindeki uzamsal verilerin yönetimini iyileştirir. Sembol katmanı, nokta verilerinin görüntü ve/veya metin olarak kullanılarak nasıl işleneceğini belirtir.

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

![Azure haritaları işaretçisi](media/migrate-google-maps-android-app/azure-maps-marker.png)</center>

## <a name="adding-a-custom-marker"></a>Özel işaretleyici ekleme

Özel görüntüler, bir haritadaki noktaları temsil etmek için kullanılabilir. Aşağıdaki örneklerde yer alan eşleme, haritada bir noktayı göstermek için özel bir görüntü kullanır. Bu nokta Enlem: 51,5 ve Boylam:-0,2 ' dir. Harita, işaretçinin konumunu, raptiye simgesinin noktasının haritada doğru konuma göre hizalanmasını sağlayacak şekilde kaydırır.

<center>

sarı iğne görüntüsünü ![](media/migrate-google-maps-web-app/ylw_pushpin.png)<br/>
YLW\_iğne. png</center>

Her iki örnekte de yukarıdaki görüntü, uygulamalar kaynaklarının çizilebilir klasörüne eklenir.

**Önce: Google Maps**

Google Maps ile, işaretçiler için özel görüntüler kullanılabilir. İşaretin `icon` seçeneğini kullanarak özel görüntüleri yükleyin. Görüntünün noktasını koordinatya hizalamak için `anchor` seçeneğini kullanın. Tutturucu, bu durumda 0,2 birimleri genişliğinde ve 1 birim yüksekliğinde olan görüntünün boyutlarına göredir.

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

![Google Maps özel işaretleyici](media/migrate-google-maps-android-app/google-maps-custom-marker.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'daki sembol katmanları özel görüntüleri destekler, ancak önce görüntünün harita kaynaklarına yüklenmesi ve benzersiz bir KIMLIK atanması gerekir. Sembol katmanı daha sonra bu KIMLIĞE başvurabilir. `iconOffset` seçeneğini kullanarak görüntüde doğru noktaya hizalamak için simgeyi kaydırın. Simge boşluğu piksel cinsinden olur. Varsayılan olarak, fark görüntünün alt merkezine göredir, ancak bu fark değeri `iconAnchor` seçeneği kullanılarak ayarlanabilir. Bu örnek, `iconAnchor` seçeneğini `"center"` olarak ayarlar ve bir simge sapmasını kullanarak, raptiye görüntüsünün noktasıyla hizalamak için görüntüyü beş piksel sağa ve 15 piksele taşıyın.

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

![Azure Maps özel işaretleyici](media/migrate-google-maps-android-app/azure-maps-custom-marker.png)</center>

## <a name="adding-a-polyline"></a>Çoklu çizgi ekleme

Polylines, haritada bir çizgiyi veya yolu göstermek için kullanılır. Aşağıdaki örneklerde, haritada kesik çizgili bir çoklu çizginin nasıl oluşturulacağı gösterilmektedir.

**Önce: Google Maps**

Google Maps ile `PolylineOptions` sınıfı kullanılarak bir çoklu çizgi oluşturulabilir ve `addPolyline` yöntemi kullanılarak haritaya eklenebilir. Kontur rengi `color` seçeneği kullanılarak ayarlanabilir, vuruş genişliği Genişlik seçeneği kullanılarak ayarlanır ve `pattern` seçeneği kullanılarak vuruş kesik çizgi dizisi ayarlanabilir.

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

![Google Maps çoklu çizgi](media/migrate-google-maps-android-app/google-maps-polyline.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da çoklu çizgilerin çizgisini `LineString` veya `MultiLineString` nesneleri olarak adlandırılır. Bu nesneler bir veri kaynağına eklenebilir ve çizgi katmanı kullanılarak oluşturulabilir. Kontur genişliği ve Dash dizisi "piksel" birimleri, ' deki Azure Maps web SDK 'Sı ile hizalanır ve her iki SDK 'daki aynı değerlerin kullanılması aynı sonuçları üretir.

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

![Azure Maps çoklu çizgi](media/migrate-google-maps-android-app/azure-maps-polyline.png)</center>

## <a name="adding-a-polygon"></a>Çokgen ekleme

Çokgenler, haritadaki bir alanı temsil etmek için kullanılır. Aşağıdaki örneklerde haritanın orta koordinatına göre bir üçgen oluşturan bir çokgen oluşturma gösterilmektedir.

**Önce: Google Maps**

Google Maps ile `PolygonOptions` sınıfı kullanılarak bir çokgen oluşturulabilir ve `addPolygon` yöntemi kullanılarak haritaya eklenebilir. `fillColor` ve `strokeColor` seçeneği kullanılarak Fill ve Stroke renkleri ayarlanabilir, kontur genişliği `strokeWidth` seçeneği kullanılarak ayarlanır.

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

![Google Maps Çokgen](media/migrate-google-maps-android-app/google-maps-polygon.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da, `Polygon` ve `MultiPolygon` nesneleri bir veri kaynağına eklenebilir ve katmanlar kullanılarak haritada işlenebilir. Çokgenin alanı çokgen katmanında işlenebilir. Bir çokgenin ana hattı bir çizgi katmanı kullanılarak oluşturulabilir. Kontur genişliği ve Dash dizisi "piksel" birimleri, ' deki Azure Maps web SDK 'Sı ile hizalanır ve her iki SDK 'daki aynı değerlerin kullanılması aynı sonuçları üretir.

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

Azure haritalar Çokgen ![](media/migrate-google-maps-android-app/azure-maps-polygon.png)</center>

## <a name="overlay-a-tile-layer"></a>Döşeme katmanını kaplama

 Döşeme katmanları, haritalar döşeme sistemiyle hizalamış, daha küçük döşenmiş görüntülerle ayrılmış katman görüntülerinin yerdeğiştirmesine olanak tanır. Bu yaklaşım, katman görüntülerini veya büyük veri kümelerini fazla kullanmanın yaygın bir yoludur. Döşeme katmanları, Google Maps 'ta görüntü Yerpaylaşımları olarak bilinir.

Aşağıdaki örnekler, Iowa çevresel Mesonet of Iowa çevre bir hava durumu radar kutucuk katmanını kaplamış. Kutucuklar 256 piksel boyutlardır.

**Önce: Google Maps**

Google Maps ile, bir kutucuk katmanı haritanın üstünde bulunabilir `TileOverlayOptions` sınıfını kullanır ve `addTileLauer` yöntemi kullanılarak haritaya eklenebilir. Kutucukları yarı saydam hale getirmek için `transparency` seçeneği 0,2 veya saydam olarak ayarlanır.

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

Bir kutucuk katmanı, diğer herhangi bir katmanda benzer bir şekilde haritaya eklenebilir. X, y ve zoom yer tutucuları olan biçimli bir URL; `{x}`, `{y}`, sırasıyla `{z}`, kutucuklara nerede erişebileceğini bildirmek için kullanılır. Azure haritalar 'daki döşeme katmanları `{quadkey}`, `{bbox-epsg-3857}`ve `{subdomain}` yer tutucuları de destekler. Kutucuk katmanını yarı şeffaf hale getirmek için 0,8 opaklık değeri kullanılır. Opaklık ve saydamlık, benzer olmasına karşın ters çevrilmiş değerler kullanır. Aralarında dönüştürme yapmak için, değerlerini bir sayıdan çıkarın.

> [!TIP]
> Azure haritalar katmanları, temel harita katmanları dahil olmak üzere diğer katmanların altına kolayca işlenebilirler. Genellikle, daha kolay okunabilmeleri için harita etiketlerinin altında döşeme katmanlarını işlemek tercih edilir. `map.layers.add` yöntemi, aşağıdaki yeni katmanın ekleneceği katmanın kimliği olan ikinci bir parametreyi alır. Harita etiketlerinin altına bir kutucuk katmanı eklemek için aşağıdaki kod kullanılabilir: `map.layers.add(myTileLayer, "labels");`

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

Azure haritalar kutucuk katmanını ![](media/migrate-google-maps-android-app/azure-maps-tile-layer.png)</center>

## <a name="show-traffic"></a>Trafiği göster

Trafik verileri hem Azure Maps hem de Google Maps 'ta yer alabilir.

**Önce: Google Maps**

Google Maps ile trafik akışı verileri haritanın `setTrafficEnabled` yöntemine doğru geçirerek eşlemenin en üstünde yer alabilir.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.setTrafficEnabled(true);
}
```

<center>

Google Maps trafiğini ![](media/migrate-google-maps-android-app/google-maps-traffic.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar, trafiği görüntülemek için çeşitli farklı seçenekler sağlar. Yol kapanışları ve kazalardan dolayı gibi trafik olayları haritada simgeler olarak görüntülenebilir. Trafik akışı, renk kodlu yollar, haritada yer alabilir. Renkler, normal beklenen gecikmeye veya mutlak gecikmeye göre, postalanan hız sınırına göre görüntülenecek şekilde değiştirilebilir. Azure haritalar 'daki olay verileri dakikada bir güncelleştirilir ve akış verileri her iki dakikada bir güncelleştirilir.

```java
mapControl.onReady(map -> {
    map.setTraffic(
        incidents(true),
        flow(TrafficFlow.RELATIVE));
});
```

<center>

Azure haritalar trafiğini ![](media/migrate-google-maps-android-app/azure-maps-traffic.png)</center>

## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar Android SDK hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Android harita denetimini kullanma](how-to-use-android-map-control-library.md)
