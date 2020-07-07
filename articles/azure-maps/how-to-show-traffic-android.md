---
title: Android eşlemesindeki trafik verilerini göster | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Android SDK kullanarak bir Haritada trafik verileri görüntüleme hakkında bilgi edineceksiniz.
author: philmea
ms.author: philmea
ms.date: 02/27/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: e5611eeb08ac370e12cf452d57a87e449fbd80da
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80335384"
---
# <a name="show-traffic-data-on-the-map-using-azure-maps-android-sdk"></a>Azure haritalar 'ı kullanarak haritadaki trafik verilerini gösterme Android SDK

Akış verileri ve olay verileri, haritada görüntülenebilen iki trafik verisi türüdür. Bu kılavuzda her iki trafik verisi türünün nasıl görüntüleneceği gösterilmektedir. Olaylar verileri, kurulumlarını, yol kapanışları ve kazalardan dolayı gibi şeyler için nokta ve hat tabanlı verilerden oluşur. Akış verileri, yolda trafik akışı hakkında ölçümleri gösterir.

## <a name="prerequisites"></a>Önkoşullar

Haritada trafiği gösterebilmeniz için önce [bir Azure hesabı](quick-demo-map-app.md#create-an-account-with-azure-maps)oluşturmanız ve [bir abonelik anahtarı edinmeniz](quick-demo-map-app.md#get-the-primary-key-for-your-account)gerekir. Ardından, [Azure Maps Android SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) yüklemeniz ve bir harita yüklemeniz gerekir.

## <a name="incidents-traffic-data"></a>Olaylar trafik verileri 

' İ çağırmak için aşağıdaki kitaplıkları içeri aktarmanız gerekir `setTraffic` `incidents` :

```java
import static com.microsoft.com.azure.maps.mapcontrol.options.TrafficOptions.incidents;
```

 Aşağıdaki kod parçacığı, Haritada trafik verilerinin nasıl görüntüleneceğini gösterir. Yöntemine bir Boole değeri geçiririz `incidents` ve bu yöntemi yöntemine geçiyoruz `setTraffic` . 

```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mapControl.getMapAsync(map - > {
        map.setTraffic(incidents(true));
    }
}
```

## <a name="flow-traffic-data"></a>Akış trafiği verileri

Çağırmak için öncelikle aşağıdaki kitaplıkları içeri aktarmanız gerekir `setTraffic` `flow` :

```java
import com.microsoft.azure.maps.mapcontrol.options.TrafficFlow;
import static com.microsoft.azure.maps.mapcontrol.options.TrafficOptions.flow;
```

Trafik akışı verilerini ayarlamak için aşağıdaki kod parçacığını kullanın. Önceki bölümde bulunan koda benzer şekilde, yöntemin dönüş değerini yöntemine geçiyoruz `flow` `setTraffic` . Öğesine geçirilebilecek dört değer vardır `flow` ve her bir değer `flow` ilgili değeri döndürecek şekilde tetiklenebilir. Dönüş değeri `flow` daha sonra öğesine bağımsız değişken olarak geçirilir `setTraffic` . Şu dört değer için aşağıdaki tabloya bakın:

| | |
| :-- | :-- |
| TrafficFlow. NONE | Harita üzerinde trafik verilerini görüntülemiyor |
| TrafficFlow. GÖRELI | Yolun serbest akış hızına göre trafik verilerini gösterir |
| TrafficFlow. RELATIVE_DELAY | Beklenen ortalama gecikmeden daha yavaş olan bölgeleri görüntüler |
| TrafficFlow. ABSOLUTE | Yolda tüm taşıtın mutlak hızını gösterir |

```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mapControl.getMapAsync(map -> {
        map.setTraffic(flow(TrafficFlow.RELATIVE)));
    }
}
```

## <a name="show-incident-traffic-data-by-clicking-a-feature"></a>Bir özelliğe tıklayarak olay trafiği verilerini göster

Belirli bir özelliğin olaylarını almak için aşağıdaki kodu kullanabilirsiniz. Bir özelliğe tıklandığında, kod mantığı olayları denetler ve olay hakkında bir ileti oluşturur. Ayrıntılar ile ekranın alt kısmında bir ileti görüntülenir.

1. İlk olarak, aşağıdaki gibi görünmesi için **kay > düzen > activity_main.xml**düzenlemeniz gerekir. , `mapcontrol_centerLat` `mapcontrol_centerLng` Ve `mapcontrol_zoom` değerlerini istediğiniz değerlerle değiştirebilirsiniz. Geri çek, yakınlaştırma düzeyi 0 ile 22 arasında bir değerdir. Yakınlaştırma düzeyinde 0, dünyanın tamamı tek bir kutucuğa sığar.

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

2. Aşağıdaki kodu **MainActivity. Java** dosyanıza ekleyin. Paket varsayılan olarak dahil edilmiştir, bu nedenle paketinizi en üstte tutduğunuzdan emin olun.

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

3. Uygulamanızda yukarıdaki kodu ekledikten sonra, bir özelliğe tıklayabilir ve trafik olaylarının ayrıntılarını görebilirsiniz. **activity_main.xml** dosyanızda kullandığınız enlem, boylam ve yakınlaştırma düzeyi değerlerine bağlı olarak, aşağıdaki görüntüye benzer sonuçlar görürsünüz:

   <center>

   ![Olay-eşleme-giden trafik](./media/how-to-show-traffic-android/android-traffic.png)

   </center>

## <a name="next-steps"></a>Sonraki adımlar

Haritanızda daha fazla veri eklemeyi öğrenmek için aşağıdaki kılavuzlara bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Kutucuk katmanı ekleme](how-to-add-tile-layer-android-map.md)

> [!div class="nextstepaction"]
> [Android haritasına şekil ekleme](how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [Özellik bilgilerini görüntüleme](display-feature-information-android.md)
