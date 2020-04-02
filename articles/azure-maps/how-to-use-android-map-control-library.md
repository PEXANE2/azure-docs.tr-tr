---
title: Android harita kontrolü ile başlarken | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Maps Android SDK'yı kullanarak Android harita denetimine nasıl başacağınızı öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 6e0f0f311b7ec8adae6ddb25e01046141adadfa4
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548548"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Azure Haritalar Android SDK ile başlarken

Azure Haritalar Android SDK, Android için bir vektör haritası kitaplığıdır. Bu makale, Azure Haritalar Android SDK'yı yükleme ve harita yükleme işlemlerinde size yol gösterir.

## <a name="prerequisites"></a>Ön koşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma

Bu makaledeki yordamları tamamlamak için öncelikle S1 fiyatlandırma katmanında [bir Azure Haritalar hesabı oluşturmanız](quick-demo-map-app.md#create-an-account-with-azure-maps) ve hesabınızın birincil [anahtarını almanız](quick-demo-map-app.md#get-the-primary-key-for-your-account) gerekir.

Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın.

### <a name="download-android-studio"></a>Android Studio'u İndir

Android Studio'yu indirin ve Azure Haritalar Android SDK'yı yüklemeden önce boş bir etkinlikle bir proje oluşturun. Android [Studio'u](https://developer.android.com/studio/) Google'dan ücretsiz olarak indirebilirsiniz. 

## <a name="create-a-project-in-android-studio"></a>Android Studio'da proje oluşturma

İlk olarak, boş bir etkinlikle yeni bir proje oluşturun. Bir Android Studio projesi oluşturmak için şu adımları tamamlayın:

1. **Projenizi seçin**, **Telefon ve Tablet**seçin. Başvurunuz bu form faktörü üzerinde çalışacaktır.
2. Telefon **ve Tablet** sekmesinde Boş **Etkinlik'i**seçin ve ardından **İleri'yi**seçin.
3. **Projenizi yapılandırın,** minimum SDK olarak seçin. `API 21: Android 5.0.0 (Lollipop)` Bu, Azure Haritalar Android SDK tarafından desteklenen en eski sürümüdür.
4. Varsayılanı `Activity Name` kabul `Layout Name` edin ve **Finish'i**seçin.

Android Studio'yu yükleme ve yeni bir proje oluşturma konusunda daha fazla yardım için [Android Studio belgelerine](https://developer.android.com/studio/intro/) bakın.

![Android stüdyosunda proje oluşturma ](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Sanal aygıt ayarlama

Android Studio bilgisayarınızda sanal bir Android cihaz kurmanızı sağlar. Bunu yapmak, geliştirme sırasında uygulamanızı test yardımcı olabilir. Sanal bir cihaz kurmak için proje ekranınızın sağ üst köşesindeki Android Sanal Cihaz (AVD) Yöneticisi simgesini seçin ve ardından **Sanal Aygıt Oluştur'u**seçin. Araç çubuğundan **Araçlar** > **Android** > **AVD Manager'ı** seçerek AVD Yöneticisi'ne de ulaşabilirsiniz. **Telefonlar** **kategorisinde, Nexus 5X'i**seçin ve **ardından İleri'yi**seçin.

[Android Studio belgelerinde](https://developer.android.com/studio/run/managing-avds)AVD kurma hakkında daha fazla bilgi edinebilirsiniz.

![Android Emülatör](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-the-azure-maps-android-sdk"></a>Azure Haritalar Android SDK'yı yükleyin

Uygulamanızı oluşturmanın bir sonraki adımı Azure Haritalar Android SDK'yı yüklemektir. SDK'yı yüklemek için aşağıdaki adımları tamamlayın:

1. Üst düzey **build.gradle** dosyasını açın ve **tüm projeler,** **depolar** blok bölümüne aşağıdaki kodu ekleyin:

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. **Uygulamanızı/build.gradle'nizi** güncelleyin ve aşağıdaki kodu ekleyin:
    
    1. Projenizin **minSdkVersion'unAPI** 21 veya daha yüksek olduğundan emin olun.

    2. Android bölümüne aşağıdaki kodu ekleyin:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Bağımlılıkblokunuzu güncelleştirin ve en son Azure Haritalar Android SDK için yeni bir uygulama bağımlılık çizgisi ekleyin:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```
    
    4. Araç çubuğunda **Dosya'ya** gidin ve ardından **Gradle Dosyalarıyla Projeyi Eşitle'yi**tıklatın.
3. Ana aktiviteye bir harita parçası \> ekleyin \> \_(res düzen etkinliği main.xml):
    
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
    
    * Azure Haritalar SDK için içeri alma ekleme
    * Azure Haritalar kimlik doğrulama bilgilerinizi ayarlama
    * **onCreate** yönteminde harita denetimi örneğini alma

    Kimlik doğrulama bilgilerini `AzureMaps` genel olarak sınıfa `setSubscriptionKey` `setAadProperties` veya yöntemleri kullanarak ayarlamak, kimlik doğrulama bilgilerinizi her görünüme eklemek zorunda kalmamanızı sağlar. 

    Harita denetimi, Android'in OpenGL yaşam döngüsünü yönetmek için kendi yaşam döngüsü yöntemlerini içerir. Bu yaşam döngüsü yöntemleri doğrudan içeren Etkinlik'ten çağrılmalıdır. Uygulamanızın harita denetiminin yaşam döngüsü yöntemlerini doğru şekilde araması için, harita denetimini içeren Etkinlik'te aşağıdaki yaşam döngüsü yöntemlerini geçersiz kılmanız gerekir. Ve, ilgili harita kontrol yöntemini aramanız gerekir. 

    * onCreate(Paket) 
    * onBaşlangıç() 
    * onÖzgeçmiş() 
    * onPause() 
    * onStop() 
    * onDestroy() 
    * onSaveInstanceState(Paket) 
    * onLowMemory() 

    **MainActivity.java** dosyasını aşağıdaki gibi edin:
    
    ```java
    package com.example.myapplication;

    //For older versions use: import android.support.v7.app.AppCompatActivity; 
    import androidx.appcompat.app.AppCompatActivity;
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

## <a name="import-classes"></a>Alma sınıfları

Önceki adımları tamamladıktan sonra, büyük olasılıkla Android Studio'dan bazı kodlar hakkında uyarılar alırsınız. Bu uyarıları gidermek için başvurulan `MainActivity.java`sınıfları içeri aktarın

Alt+Enter (Mac'te Seçenek+İade) seçeneğini seçerek bu sınıfları otomatik olarak içe aktarabilirsiniz.

Uygulamanızı oluşturmak için aşağıdaki grafikte gösterildiği gibi çalıştır düğmesini (veya Mac'te Control+R tuşuna basın) seçin.

![Çalıştır’a tıklayın](./media/how-to-use-android-map-control-library/run-app.png)

Android Studio uygulama oluşturmak için birkaç saniye sürer. Yapı tamamlandıktan sonra, emulated Android cihazda uygulamanızı test edebilirsiniz. Bunun gibi bir harita görmelisiniz:

<center>

![Android uygulamasında Azure Haritalar](./media/how-to-use-android-map-control-library/android-map.png)</center>

## <a name="localizing-the-map"></a>Haritayı yerelleştirme

Azure Haritalar Android SDK, haritanın dilini ve bölgesel görünümünü belirlemenin üç farklı yolunu sağlar. Aşağıdaki kod, dili Fransızca ("fr-FR") ve bölgesel görünümün "otomatik" olarak nasıl ayarlanışını gösterir. 

İlk seçenek, dili aktarmak ve bölgesel bilgileri `AzureMaps` genel olarak `setLanguage` `setView` statik ve yöntemleri kullanarak sınıfa görüntülemektir. Bu, uygulamanızda yüklenen tüm Azure Haritalar denetimlerinde varsayılan dili ve bölgesel görünümü ayarlar.

```Java
static {
    //Set your Azure Maps Key.
    AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");

    //Set the language to be used by Azure Maps.
    AzureMaps.setLanguage("fr-FR");

    //Set the regional view to be used by Azure Maps.
    AzureMaps.setView("auto");
}
```

İkinci seçenek, dili geçmek ve bilgileri harita denetimi XML'e görüntülemektir.

```XML
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_language="fr-FR"
    app:mapcontrol_view="auto"
    />
```

Üçüncü seçenek, haritalar `setStyle` yöntemini kullanarak haritanın dilini ve bölgesel görünümünü programlı olarak ayarlamaktır. Bu, haritanın dilini ve bölgesel görünümünü değiştirmek için herhangi bir zamanda yapılabilir.

```Java
mapControl.onReady(map -> {
    map.setStyle(StyleOptions.language("fr-FR"));
    map.setStyle(StyleOptions.view("auto"));
});
```

Burada , "fr-FR" olarak ayarlanmış dil ve "otomatik" olarak ayarlanmış bölgesel görünüme sahip Azure Haritaları örneği verilmiştir.

<center>

![Azure Haritalar, Fransızca etiket gösteren harita görüntüsü](./media/how-to-use-android-map-control-library/android-localization.png)
</center>

Desteklenen dillerin ve bölgesel görünümlerin tam listesi [burada](supported-languages.md)belgelenmiştir.

## <a name="navigating-the-map"></a>Haritada gezinme

Haritanın yakınlaştırılabildiği, kaydırılabildiği, döndürülebileceği ve pitched edilebildiği birkaç farklı yol vardır. Aşağıdaki ayrıntıları harita gezinmek için tüm farklı yolları.

**Haritayı yakınlaştır**

- Haritaya iki parmağınızla dokunun ve yakınlaştırmak için parmaklarınızı uzaklaştırmak veya yaymak için birbirine sıkıştırın.
- Bir seviyeyi yakınlaştırmak için haritaya çift dokunun.
- Haritayı bir seviye yi uzaklaştırmak için iki parmağınızla çift dokunun.
- İki kez dokunun; ikinci dokunuşta parmağınızı haritada tutun ve yakınlaştırmak için yukarı sürükleyin veya uzaklaştırmak için aşağı sürükleyin.

**Haritayı kaydır**

- Haritaya dokunun ve herhangi bir yöne sürükleyin.

**Haritayı döndürme**

- Haritaya iki parmağınızla dokunun ve döndürün.

**Haritayı pitch**

- Haritaya iki parmağınızla dokunun ve yukarı veya aşağı sürükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Haritaya yer kaplama sı nasıl ekleyeceğinizi öğrenin:

> [!div class="nextstepaction"]
> [Android haritasına sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Android haritasına şekil ekleme](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)

> [!div class="nextstepaction"]
> [Android haritalarda harita stillerini değiştirme](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)
