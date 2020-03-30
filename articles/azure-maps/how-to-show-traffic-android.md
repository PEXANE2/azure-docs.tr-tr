---
title: Trafik verilerini android haritasında göster | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Android SDK'yı kullanarak trafik verilerinin haritaüzerinde nasıl görüntülenecek olduğunu öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 02/27/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: e5611eeb08ac370e12cf452d57a87e449fbd80da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335384"
---
# <a name="show-traffic-data-on-the-map-using-azure-maps-android-sdk"></a>Azure Haritalar Android SDK'yı kullanarak trafik verilerini haritada göster

Akış verileri ve olay verileri, haritada görüntülenebilen iki tür trafik verisidir. Bu kılavuz, her iki trafik veritinin nasıl görüntülenebildiğini gösterir. Olay verileri, inşaatlar, yol kapatmalar ve kazalar gibi şeyler için nokta ve çizgi tabanlı verilerden oluşur. Akış verileri, yoldaki trafik akışıyla ilgili ölçümleri gösterir.

## <a name="prerequisites"></a>Ön koşullar

Haritada trafiği gösterebilmeniz için [bir Azure Hesabı oluşturup](quick-demo-map-app.md#create-an-account-with-azure-maps)bir abonelik anahtarı [edinmeniz](quick-demo-map-app.md#get-the-primary-key-for-your-account)gerekir. Ardından, Azure Haritalar [Android SDK'sını](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) yüklemeniz ve bir harita yüklemeniz gerekir.

## <a name="incidents-traffic-data"></a>Olaylar trafik verileri 

Aramak `setTraffic` için aşağıdaki kitaplıkları almanız `incidents`gerekir:

```java
import static com.microsoft.com.azure.maps.mapcontrol.options.TrafficOptions.incidents;
```

 Aşağıdaki kod parçacığı, trafik verilerini haritada nasıl görüntülediğinizi gösterir. `incidents` Metoda boolean değerini geçiyoruz ve `setTraffic` bunu yönteme aktarıyoruz. 

```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mapControl.getMapAsync(map - > {
        map.setTraffic(incidents(true));
    }
}
```

## <a name="flow-traffic-data"></a>Akış trafiği verileri

Öncelikle aramak `setTraffic` için aşağıdaki kitaplıkları almanız gerekir: `flow`

```java
import com.microsoft.azure.maps.mapcontrol.options.TrafficFlow;
import static com.microsoft.azure.maps.mapcontrol.options.TrafficOptions.flow;
```

Trafik akışı verilerini ayarlamak için aşağıdaki kod parçacıklarını kullanın. Önceki bölümdeki koda benzer şekilde, yöntemin `flow` geri dönüş `setTraffic` değerini yönteme aktarıyoruz. 'ye `flow`geçirilebilen dört değer vardır ve `flow` her değer ilgili değeri döndürmek için tetiklenir. İade değeri `flow` daha sonra bağımsız değişken `setTraffic`olarak geçirilir. Bu dört değer için aşağıdaki tabloya bakın:

| | |
| :-- | :-- |
| TrafficFlow.NONE | Trafik verilerini haritada görüntülemez |
| Trafik Akışı.GÖRECELİ | Yolun serbest akış hızına göre trafik verilerini gösterir |
| TrafficFlow.RELATIVE_DELAY | Beklenen ortalama gecikmeden daha yavaş alanları görüntüler |
| TrafficFlow.ABSOLUTE | Yoldaki tüm araçların mutlak hızını gösterir |

```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mapControl.getMapAsync(map -> {
        map.setTraffic(flow(TrafficFlow.RELATIVE)));
    }
}
```

## <a name="show-incident-traffic-data-by-clicking-a-feature"></a>Bir özelliği tıklatarak olay trafiği verilerini gösterme

Belirli bir özellik için olayları elde etmek için aşağıdaki kodu kullanabilirsiniz. Bir özellik tıklatıldığında, kod mantığı olayları denetler ve olay hakkında bir ileti oluşturur. Ayrıntıları içeren ekranın alt kısmında bir ileti belirir.

1. İlk olarak, **activity_main.xml > > düzen res**düzenlerini düzenlemek gerekir , böylece aşağıdaki gibi görünüyor. 'yi `mapcontrol_centerLat` `mapcontrol_centerLng`ve `mapcontrol_zoom` istediğiniz değerleri değiştirebilirsiniz. Geri çağırma, yakınlaştırma düzeyi 0 ile 22 arasında bir değerdir. Zum seviyesi 0'da, tüm dünya tek bir karoya uyar.

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
           app:mapcontrol_centerLat="47.6050"
           app:mapcontrol_centerLng="-122.3344"
           app:mapcontrol_zoom="12"
           />

   </FrameLayout>
   ```

2. **MainActivity.java** dosyanıza aşağıdaki kodu ekleyin. Paket varsayılan olarak dahildir, bu nedenle paketinizi en üstte tuttuğunızdan emin olun.

   ```java
   package <yourpackagename>;
   import androidx.appcompat.app.AppCompatActivity;

   import android.os.Bundle;
   import android.widget.Toast;

   import com.microsoft.azure.maps.mapcontrol.AzureMaps;
   import com.microsoft.azure.maps.mapcontrol.MapControl;
   import com.mapbox.geojson.Feature;
   import com.microsoft.azure.maps.mapcontrol.events.OnFeatureClick;

   import com.microsoft.azure.maps.mapcontrol.options.TrafficFlow;
   import static com.microsoft.azure.maps.mapcontrol.options.TrafficOptions.flow;
   import static com.microsoft.azure.maps.mapcontrol.options.TrafficOptions.incidents;

   public class MainActivity extends AppCompatActivity {

       static {
           AzureMaps.setSubscriptionKey("Your Azure Maps Subscription Key");
       }

       MapControl mapControl;

       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);

           mapControl = findViewById(R.id.mapcontrol);

           mapControl.onCreate(savedInstanceState);

           //Wait until the map resources are ready.
           mapControl.getMapAsync(map -> {

               map.setTraffic(flow(TrafficFlow.RELATIVE));
               map.setTraffic(incidents(true));

               map.events.add((OnFeatureClick) (features) -> {

                   if (features != null && features.size() > 0) {
                       Feature incident = features.get(0);
                       if (incident.properties() != null) {


                           StringBuilder sb = new StringBuilder();
                           String incidentType = incident.getStringProperty("incidentType");
                           if (incidentType != null) {
                               sb.append(incidentType);
                           }
                           if (sb.length() > 0) sb.append("\n");
                           if ("Road Closed".equals(incidentType)) {
                               sb.append(incident.getStringProperty("from"));
                           } else {
                               String description = incident.getStringProperty("description");
                               if (description != null) {
                                   for (String word : description.split(" ")) {
                                       if (word.length() > 0) {
                                           sb.append(word.substring(0, 1).toUpperCase());
                                           if (word.length() > 1) {
                                               sb.append(word.substring(1));
                                           }
                                           sb.append(" ");
                                       }
                                   }
                               }
                           }
                           String message = sb.toString();

                           if (message.length() > 0) {
                               Toast.makeText(this,message,Toast.LENGTH_LONG).show();
                           }
                       }
                   }
               });
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

3. Yukarıdaki kodu uygulamanıza dahil ettikten sonra, bir özelliği tıklatabilir ve trafik olaylarının ayrıntılarını görebilirsiniz. **activity_main.xml** dosyanızda kullandığınız enlem, boylam ve zum düzeyi değerlerine bağlı olarak aşağıdaki resme benzer sonuçlar görürsünüz:

   <center>

   ![Olay-trafik-on-the-harita](./media/how-to-show-traffic-android/android-traffic.png)

   </center>

## <a name="next-steps"></a>Sonraki adımlar

Haritanıza nasıl daha fazla veri ekleyeceğinizi öğrenmek için aşağıdaki kılavuzları görüntüleyin:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Kutucuk katmanı ekleme](how-to-add-tile-layer-android-map.md)

> [!div class="nextstepaction"]
> [Android haritasına şekil ekleme](how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [Özellik bilgilerini görüntüleme](display-feature-information-android.md)
