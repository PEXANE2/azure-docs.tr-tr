---
title: Azure haritalar 'ı kullanarak haritaya bir sembol katmanı ekleme Android SDK
description: Haritaya işaret eklemeyi öğrenin. Bir veri kaynağından gelen nokta tabanlı verileri içeren bir sembol katmanını eklemek için Microsoft Azure haritaları Android SDK kullanan bir örneğe bakın.
author: anastasia-ms
ms.author: v-stharr
ms.date: 11/24/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 300a7968b2072459d6d7709e4d89388e1bcf59f3
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96531216"
---
# <a name="add-a-symbol-layer-to-a-map-using-azure-maps-android-sdk"></a>Azure haritalar 'ı kullanarak haritaya bir sembol katmanı ekleme Android SDK

Bu makalede, Azure Maps Android SDK kullanarak bir harita üzerinde sembol katmanı olarak bir veri kaynağındaki nokta verilerinin nasıl işleneceğini gösterir.

## <a name="prerequisites"></a>Önkoşullar

1. [Azure haritalar hesabı oluşturma](quick-demo-map-app.md#create-an-azure-maps-account)
2. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account).
3. [Azure haritalar Android SDK](./how-to-use-android-map-control-library.md)indirin ve yükleyin.

## <a name="add-a-symbol-layer"></a>Sembol katmanı ekleme

Sembol katmanını kullanarak haritada bir işaretleyici eklemek için aşağıdaki adımları izleyin:

1. **res**  >  **layout**  >  Şu XML gibi görünmesi için res Düzen **activity_main.xml** düzenleyin:
    
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
            app:mapcontrol_centerLat="47.64"
            app:mapcontrol_centerLng="-122.33"
            app:mapcontrol_zoom="12"
            />

    </FrameLayout>
    ```

2. Aşağıdaki kod parçacığını sınıfınızın **OnCreate ()** yöntemine kopyalayın `MainActivity.java` .

    ```Java
    mapControl.onReady(map -> {
    
        //Create a data source and add it to the map.
        DataSource dataSource = new DataSource();
        map.sources.add(dataSource);
    
        //Create a point feature and add it to the data source.
        dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
    
        //Add a red custom image icon to the map resources.
        map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
    
        //Create a symbol layer and add it to the map.
        map.layers.add(new SymbolLayer(dataSource,
            iconImage("my-icon")));
        });
    
    ```
    
    Yukarıdaki kod parçacığını ekledikten sonra `MainActivity.java` aşağıdaki gibi görünmelidir:
    
    ```Java
    package com.example.myapplication;
    
    import android.app.Activity;
    import android.os.Bundle;
    import com.mapbox.geojson.Feature;
    import com.mapbox.geojson.Point;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;
    import static com.microsoft.azure.maps.mapcontrol.options.SymbolLayerOptions.iconImage;
    public class MainActivity extends AppCompatActivity {
        
        static{
                AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
            }
    
        MapControl mapControl;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapControl = findViewById(R.id.mapcontrol);
    
            mapControl.onCreate(savedInstanceState);
    
            mapControl.onReady(map -> {
    
                //Create a data source and add it to the map.
                DataSource dataSource = new DataSource();
                map.sources.add(dataSource);
            
                //Create a point feature and add it to the data source.
                dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
            
                //Add a custom image icon to the map resources.
                map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            
                //Create a symbol layer and add it to the map.
                map.layers.add(new SymbolLayer(dataSource,
                    iconImage("my-icon")));
            });
        }
    
        @Override
        public void onStart() {
            super.onStart();
            mapControl.onStart();
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
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

Uygulamayı çalıştırdığınızda, haritada aşağıda gösterildiği gibi bir işaret görürsünüz:

![Android eşleme PIN 'i](./media/how-to-add-symbol-to-android-map/android-map-pin.png)

> [!TIP]
> Varsayılan olarak, sembol katmanları örtüşen sembolleri gizleyerek sembol işlemesini iyileştirir. Yakınlaştırma sırasında gizli simgeler görünür hale gelir. Bu özelliği devre dışı bırakmak ve tüm sembolleri her zaman işlemek için `iconAllowOverlap` seçeneğini olarak ayarlayın `true` .

## <a name="next-steps"></a>Sonraki adımlar

Haritanızda daha fazla veri eklemek için bkz.:

> [!div class="nextstepaction"]
> [Android haritasına şekil ekleme](./how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [Özellik bilgilerini görüntüleme](display-feature-information-android.md)