---
title: Android Maps 'e kutucuk katmanı ekleme | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Android SDK kullanarak bir haritada kutucuk katmanının nasıl işleneceğini öğreneceksiniz.
author: anastasia-ms
ms.author: v-stharr
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: e7480ba3befaf560eace2cae4ef4a822012373a2
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87126984"
---
# <a name="add-a-tile-layer-to-a-map-using-the-azure-maps-android-sdk"></a>Azure haritalar 'ı kullanarak haritaya kutucuk katmanı ekleme Android SDK

Bu makalede, Azure Maps Android SDK kullanarak bir haritada kutucuk katmanının nasıl işleneceğini gösterilmektedir. Döşeme katmanları, Azure Maps temel harita kutucuklarının üzerine görüntü eklemenize olanak tanır. Azure haritalar döşeme sistemi hakkında daha fazla bilgi [yakınlaştırma düzeyleri ve kutucuk Kılavuzu](zoom-levels-and-tile-grid.md) belgelerinde bulunabilir.

Döşeme katmanı bir sunucudan kutucukları yükler. Bu görüntüler, kutucuk katmanının anladığı adlandırma kuralını kullanarak, bir sunucu üzerindeki herhangi bir görüntü gibi önceden işlenebilir ve depolanabilir. Ya da bu görüntüler gerçek zamanlı görüntüleri üreten dinamik bir hizmetle birlikte oluşturulabilir. Azure haritalar TileLayer sınıfı tarafından desteklenen üç farklı kutucuk hizmeti adlandırma kuralı vardır:

* X, Y, yakınlaştırma gösterimi-yakınlaştırma düzeyine göre x, sütun ise döşeme kılavuzundaki döşemenin satır konumudur.
* Quadkey gösterimi-x, y, zoom bilgilerini bir kutucuk için benzersiz bir tanımlayıcı olan tek bir dize değerine birleşimi.
* Sınırlayıcı kutusu-sınırlayıcı kutu koordinatları, `{west},{south},{east},{north}` [Web eşleme HIZMETLERI (WMS)](https://www.opengeospatial.org/standards/wms)tarafından yaygın olarak kullanılan biçimde bir görüntü belirtmek için kullanılabilir.

> [!TIP]
> Bir TileLayer, haritada büyük veri kümelerini görselleştirmenin harika bir yoludur. Bir görüntüden yalnızca bir kutucuk katmanı oluşturulmayabilir, ancak vektör verileri de kutucuk katmanı olarak da oluşturulabilir. Vektör verilerini kutucuk katmanı olarak işleyerek, harita denetiminin yalnızca dosya boyutunda, temsil ettikleri vektör verilerinden çok daha küçük olabilen kutucukları yüklemesi gerekir. Bu teknik, haritada milyonlarca veri satırı oluşturması gereken birçok kişi tarafından kullanılır.

Döşeme katmanına geçirilen kutucuk URL 'si, bir TileJSON kaynağına veya aşağıdaki parametreleri kullanan bir kutucuk URL şablonuna yönelik bir http/https URL 'SI olmalıdır: 

* `{x}`-X kutucuğunun konumu. Ayrıca `{y}` , ve gerektirir `{z}` .
* `{y}`-Kutucuğun Y konumu. Ayrıca `{x}` , ve gerektirir `{z}` .
* `{z}`-Kutucuğun yakınlaştırma düzeyi. Ayrıca `{x}` , ve gerektirir `{y}` .
* `{quadkey}`-Bing Haritalar kutucuk sistem adlandırma kuralına bağlı olarak, quadkey tanımlayıcısını Döşe.
* `{bbox-epsg-3857}`- `{west},{south},{east},{north}` EPSG 3857 uzamsal başvuru sisteminde biçim içeren bir sınırlayıcı kutu dizesi.
* `{subdomain}`-Alt etki alanı değeri belirtilmişse alt etki alanı değerleri için bir yer tutucu.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki işlemi gerçekleştirmek için, bir harita yüklemek üzere [Azure Maps Android SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) yüklemeniz gerekir.


## <a name="add-a-tile-layer-to-the-map"></a>Haritaya kutucuk katmanı ekleyin

 Bu örnek, bir kutucuk kümesini işaret eden döşeme katmanının nasıl oluşturulacağını gösterir. Bu Kutucuklar, "x, y, Zoom" Döşeme sistemini kullanır. Bu kutucuk katmanının kaynağı, [Iowa çevresel Mesonet 'in Iowa çevre](https://mesonet.agron.iastate.edu/ogc/)bir hava durumu radar kaplamasıyla. 

Aşağıdaki adımları izleyerek haritaya bir kutucuk katmanı ekleyebilirsiniz.

1. **Kaynak > düzeni > activity_main.xml** aşağıdaki gibi görünmesi için düzenleyin:

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

2. Aşağıdaki kod parçacığını, sınıfınızın **OnCreate ()** metoduna kopyalayın `MainActivity.java` .

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
    
    Yukarıdaki kod parçacığı ilk olarak, **Onready ()** geri çağırma yöntemini kullanarak bir Azure haritalar harita denetim örneği edinir. Daha sonra bir `TileLayer` nesnesi oluşturur ve biçimlendirilen bir **xyz** kutucuğu URL 'sini seçeneğe geçirir `tileUrl` . Katman opaklığı olarak ayarlanır `0.8` ve kutucuk hizmetinden kullanılan kutucuklar 256 piksel döşeme olduğundan bu bilgiler `tileSize` seçeneğe geçirilir. Kutucuk katmanı daha sonra Haritalar katman yöneticisine geçirilir.

    Yukarıdaki kod parçacığını ekledikten sonra `MainActivity.java` aşağıdaki gibi görünmelidir:
    
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

Uygulamanızı Şimdi çalıştırırsanız, haritada aşağıda görüldüğü gibi bir satır görmeniz gerekir:

<center>

![Android eşleme satırı](./media/how-to-add-tile-layer-android-map/xyz-tile-layer-android.png)</center>

## <a name="next-steps"></a>Sonraki adımlar

Harita stillerini ayarlama yolları hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın

> [!div class="nextstepaction"]
> [Android haritalar 'da harita stillerini değiştirme](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)