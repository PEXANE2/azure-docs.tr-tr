---
title: Android haritalara döşeme katmanı ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Android SDK'yı kullanarak haritaüzerinde bir döşeme katmanının nasıl oluşturulacağını öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: f98598bd1307bb1b46ff23814780c5f809b9ac90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335574"
---
# <a name="add-a-tile-layer-to-a-map-using-the-azure-maps-android-sdk"></a>Azure Haritalar Android SDK'yı kullanarak haritaya döşeme katmanı ekleme

Bu makalede, Azure Haritalar Android SDK'yı kullanarak haritaüzerinde bir döşeme katmanınasıl oluşturulabileceğiniz gösterilmektedir. Kutucuk katmanları, Görüntüleri Azure Haritalar temel harita kutucuklarının üzerine eklemenize olanak tanır. Azure Haritalar döşeme sistemi hakkında daha fazla bilgiyi [Yakınlaştırma düzeyleri ve döşeme ızgarası](zoom-levels-and-tile-grid.md) belgelerinde bulabilirsiniz.

Bir Kiremit katmanı, bir sunucudan kutucuklarda yüklenir. Bu görüntüler, döşeme katmanının anladığı bir adlandırma kuralı kullanılarak sunucudaki diğer görüntüler gibi önceden işlenebilir ve depolanabilir. Veya, bu görüntüler gerçek zamanlı yakın görüntüleri üreten dinamik bir hizmet ile işlenebilir. Azure Haritalar TileLayer sınıfı tarafından desteklenen üç farklı döşeme hizmeti adlandırma kuralı vardır:

* X, Y, Zoom gösterimi - Zum düzeyine bağlı olarak, x sütundur ve y döşeme ızgarasındaki döşemenin satır konumudur.
* Quadkey gösterimi - X, y, zoom bilgileri bir döşeme için benzersiz bir tanımlayıcı olan tek bir dize değerine yakınlaştırın.
* Sınırlayıcı Kutu - Bağlama kutusu koordinatları, Web `{west},{south},{east},{north}` [Haritalama Hizmetleri (WMS)](https://www.opengeospatial.org/standards/wms)tarafından yaygın olarak kullanılan biçimde bir görüntüyü belirtmek için kullanılabilir.

> [!TIP]
> Bir TileLayer harita üzerinde büyük veri kümeleri görselleştirmek için harika bir yoldur. Görüntüden yalnızca bir döşeme katmanı oluşturulamaz, aynı zamanda vektör verileri de bir döşeme katmanı olarak işlenebilir. Vektör verilerini bir döşeme katmanı olarak işleyerek, harita denetiminin yalnızca dosya boyutu nda temsil ettikleri vektör verilerinden çok daha küçük olabilecek kutucukları yüklemesi gerekir. Bu teknik, harita üzerinde milyonlarca veri satırı işlemek için gereken birçok kişi tarafından kullanılır.

Döşeme katmanına geçirilen döşeme URL'si, bir TileJSON kaynağının http/https URL'si veya aşağıdaki parametreleri kullanan bir döşeme URL şablonu olmalıdır: 

* `{x}`- Kiremitin X konumu. Ayrıca `{y}` ihtiyaçları `{z}`ve .
* `{y}`- Kiremitin Y konumu. Ayrıca `{x}` ihtiyaçları `{z}`ve .
* `{z}`- Döşemenin yakınlaştırma seviyesi. Ayrıca `{x}` ihtiyaçları `{y}`ve .
* `{quadkey}`- Bing Maps döşeme sistemi adlandırma kuralına dayalı döşeme dörtanahtar tanımlayıcısı.
* `{bbox-epsg-3857}`- EPSG 3857 `{west},{south},{east},{north}` Mekansal Referans Sistemi formatında bir sınırlayıcı kutu dizesi.
* `{subdomain}`- Alt etki alanı değeri belirtilirse, alt etki alanı değerleri için bir yer tutucu.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki işlemi tamamlamak için, bir harita yüklemek için [Azure Haritalar Android SDK'yı](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) yüklemeniz gerekir.


## <a name="add-a-tile-layer-to-the-map"></a>Haritaya döşeme katmanı ekleme

 Bu örnek, bir kutucuk kümesini işaret eden bir döşeme katmanının nasıl oluşturulutur gösteriş gösterir. Bu karolar "x, y, zoom" döşeme sistemini kullanır. Bu kiremit tabakasının kaynağı [Iowa Devlet Üniversitesi Iowa Çevre Mesonet](https://mesonet.agron.iastate.edu/ogc/)bir hava radar bindirme olduğunu. 

Aşağıdaki adımları izleyerek haritaya bir döşeme katmanı ekleyebilirsiniz.

1. Aşağıdaki gibi görünüyor böylece **activity_main.xml > > düzen reit** edin:

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
            app:mapcontrol_centerLat="40.75"
            app:mapcontrol_centerLng="-99.47"
            app:mapcontrol_zoom="3"
            />
    
    </FrameLayout>
    ```

2. Aşağıdaki kod parçacıklarını sınıfınızın `MainActivity.java` **onCreate()** yöntemine kopyalayın.

    ```Java
    mapControl.onReady(map -> {
        //Add a tile layer to the map, below the map labels.
        map.layers.add(new TileLayer(
            tileUrl("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png"),
            opacity(0.8f),
            tileSize(256)
        ), "labels");
    });
    ```
    
    Yukarıdaki kod snippet, **onReady()** geri arama yöntemini kullanarak bir Azure Haritalar harita denetimi örneği alır. Daha sonra bir `TileLayer` nesne oluşturur ve seçeneğine biçimlendirilmiş bir `tileUrl` **xyz** döşemesi URL'sini geçirir. Katmanın opaklığı ayarlanır `0.8` ve kullanılan kutucuk hizmetindeki döşemeler 256 piksel karo olduğundan, `tileSize` bu bilgiler seçeneğine aktarılır. Döşeme katmanı daha sonra haritalar katman yöneticisine geçirilir.

    Yukarıdaki kod parçacığı ekledikten sonra, `MainActivity.java` aşağıdaki gibi görünmelidir:
    
    ```Java
    package com.example.myapplication;

    import android.app.Activity;
    import android.os.Bundle;
    import android.support.v7.app.AppCompatActivity;
    import com.microsoft.azure.maps.mapcontrol.layer.TileLayer;
    import java.util.Arrays;
    import java.util.List;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import static com.microsoft.azure.maps.mapcontrol.options.TileLayerOptions.tileSize;
    import static com.microsoft.azure.maps.mapcontrol.options.TileLayerOptions.tileUrl;
        
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

                //Add a tile layer to the map, below the map labels.
                map.layers.add(new TileLayer(
                    tileUrl("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png"),
                    opacity(0.8f),
                    tileSize(256)
                ), "labels");
            });    
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

Uygulamanızı şimdi çalıştırıyorsanız, haritada aşağıda görüldüğü gibi bir satır görmeniz gerekir:

<center>

![Android harita çizgisi](./media/how-to-add-tile-layer-android-map/xyz-tile-layer-android.png)</center>

## <a name="next-steps"></a>Sonraki adımlar

Harita stilleri ayarlama yolları hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın

> [!div class="nextstepaction"]
> [Android haritalarda harita stillerini değiştirme](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)