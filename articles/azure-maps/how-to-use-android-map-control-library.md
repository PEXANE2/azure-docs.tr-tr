---
title: Android harita denetimi ile çalışmaya başlama | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Android SDK kullanarak Android harita denetimiyle nasıl başlayacağınızı öğreneceksiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: fbb81764262d98a401a26cd089e53ad37007050c
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198199"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Azure haritalar 'ı kullanmaya başlama Android SDK

Azure Maps Android SDK, Android için bir vektör eşleme kitaplığıdır. Bu makale, Azure Maps Android SDK yükleme ve eşleme yükleme işlemlerinde size rehberlik eder.

## <a name="prerequisites"></a>Önkoşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma

Bu makaledeki yordamları tamamlayabilmeniz için, önce S1 fiyatlandırma katmanında [bir Azure Maps hesabı oluşturmanız](quick-demo-map-app.md#create-an-account-with-azure-maps) ve hesabınız için [birincil anahtar almanız](quick-demo-map-app.md#get-the-primary-key-for-your-account) gerekir.

Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).

### <a name="download-android-studio"></a>Android Studio indir

Azure Haritalar Android SDK yüklemeden önce Android Studio indirin ve boş bir etkinliğe sahip bir proje oluşturun. Android Studio Google 'dan ücretsiz olarak [indirebilirsiniz](https://developer.android.com/studio/) . 

## <a name="create-a-project-in-android-studio"></a>Android Studio bir proje oluşturma

İlk olarak, boş bir etkinliğe sahip yeni bir proje oluşturun. Android Studio projesi oluşturmak için aşağıdaki adımları uygulayın:

1. **Projenizi seçin**altında **telefon ve tablet**' yı seçin. Uygulamanız bu form faktörüyle çalışır.
2. **Telefon ve tablet** sekmesinde **boş etkinlik**' i seçin ve ardından **İleri**' yi seçin.
3. **Projenizi yapılandırın**altında, en düşük SDK olarak `API 21: Android 5.0.0 (Lollipop)` ' yi seçin. Bu, Azure Maps Android SDK tarafından desteklenen en eski sürümdür.
4. Varsayılan `Activity Name` ve `Layout Name` kabul edin ve **son**' u seçin.

Android Studio yükleme ve yeni bir proje oluşturma hakkında daha fazla yardım için [Android Studio belgelerine](https://developer.android.com/studio/intro/) bakın.

![Android Studio 'da proje oluşturma ](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Sanal cihaz ayarlama

Android Studio, bilgisayarınızda bir sanal Android cihaz ayarlamanıza olanak sağlar. Bunun yapılması, geliştirme sırasında uygulamanızı test etmenize yardımcı olabilir. Bir sanal cihaz ayarlamak için, proje ekranınızın sağ üst köşesindeki Android sanal cihaz (AVD) Yöneticisi simgesini seçin ve ardından **sanal cihaz oluştur**' u seçin. Ayrıca araç çubuğundan **araçlar** > **Android** > **AVD Yöneticisi** ' ni seçerek AVD yöneticisine da ulaşabilirsiniz. **Telefonlar** kategorisinde, **Nexus 5x**' i seçin ve ardından **İleri**' yi seçin.

[Android Studio belgelerinde](https://developer.android.com/studio/run/managing-avds)bir AVD ayarlama hakkında daha fazla bilgi edinebilirsiniz.

![Android Emulator](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-the-azure-maps-android-sdk"></a>Azure haritalar 'ı Android SDK

Uygulamanızı oluşturmanın bir sonraki adımı Android SDK Azure haritalar 'ı yüklemektir. SDK 'Yı yüklemek için şu adımları uygulayın:

1. En üst düzey **Build. Gradle** dosyasını açın ve aşağıdaki kodu **tüm projeler**, **depolar** bloğu bölümüne ekleyin:

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. **Uygulamanızı/Build. Gradle** öğesini güncelleştirin ve aşağıdaki kodu buna ekleyin:
    
    1. Projenizin **Minsdkversion** özelliğinin API 21 veya daha yüksek olduğundan emin olun.

    2. Aşağıdaki kodu Android bölümüne ekleyin:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Bağımlılıklar engellemeyi güncelleştirin ve en son Azure Maps Android SDK yeni bir uygulama bağımlılığı satırı ekleyin:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```
    
    4. Araç çubuğunda **dosyasına** gidin ve ardından **projeyi Gradle dosyalarıyla Eşitle**' ye tıklayın.
3. Ana etkinliğe (Res \> düzeni \> Activity\_Main. xml) bir harita parçası ekleyin:
    
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

    Harita denetimi, Android 'ın OpenGL yaşam döngüsünü yönetmeye yönelik kendi yaşam döngüsü yöntemlerini içerir. Bu yaşam döngüsü yöntemlerinin doğrudan içeren etkinlikten çağrılması gerekir. Uygulamanızın harita denetiminin yaşam döngüsü yöntemlerini doğru bir şekilde çağırması için, eşleme denetimini içeren etkinliğin aşağıdaki yaşam döngüsü yöntemlerini geçersiz kılmanız gerekir. Ve ilgili Map denetim yöntemini çağırmanız gerekir. 

    * onCreate (paket) 
    * onStart () 
    * Onözgeçmişi () 
    * onPause () 
    * onStop () 
    * onDestroy () 
    * Onsaveınstancestate (paket) 
    * onLowMemory () 

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

## <a name="import-classes"></a>Sınıfları içeri aktar

Yukarıdaki adımları tamamladıktan sonra, büyük olasılıkla bazı kod hakkında Android Studio uyarılar alırsınız. Bu uyarıları çözümlemek için `MainActivity.java`başvurulan sınıfları içeri aktarın.

Alt + Enter (bir Mac 'te seçenek + Return) seçeneğini belirleyerek bu sınıfları otomatik olarak içeri aktarabilirsiniz.

Uygulamanızı derlemek için aşağıdaki grafikte gösterildiği gibi Çalıştır düğmesini seçin (veya Mac üzerinde Control + R tuşlarına basın).

![Çalıştır’a tıklayın](./media/how-to-use-android-map-control-library/run-app.png)

Android Studio uygulamayı derlemek birkaç saniye sürer. Oluşturma işlemi tamamlandıktan sonra, uygulamanızı öykünülmüş Android cihazında test edebilirsiniz. Aşağıdakine benzer bir eşleme görmeniz gerekir:

<center>

Android uygulamasında Azure Maps ![](./media/how-to-use-android-map-control-library/android-map.png)</center>

## <a name="localizing-the-map"></a>Haritayı yerelleştirme

Azure Haritalar Android SDK haritanın dilini ve bölgesel görünümünü ayarlamanın üç farklı yolunu sağlar. Aşağıdaki kod, dilin Fransızca ("fr-FR") ve bölgesel görünümün "Auto" olarak nasıl ayarlanacağını gösterir. 

İlk seçenek, statik `setLanguage` ve `setView` yöntemlerini genel olarak kullanarak dili geçirmektir ve bölgesel bilgileri `AzureMaps` sınıfına görüntüler. Bu işlem, uygulamanızda yüklü olan tüm Azure haritalar denetimlerinde varsayılan dili ve bölgesel görünümü ayarlar.

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

İkinci seçenek, dili geçirmektir ve harita denetim XML dosyasına bilgileri görüntüler.

```XML
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_language="fr-FR"
    app:mapcontrol_view="auto"
    />
```

Üçüncü seçenek, haritalar `setStyle` yöntemi kullanılarak eşlemenin dil ve bölgesel görünümünü programlı olarak ayarlamanıza olanak sağlar. Bu işlem, eşlemenin dil ve bölgesel görünümünü değiştirmek için herhangi bir zamanda yapılabilir.

```Java
mapControl.onReady(map -> {
    map.setStyle(StyleOptions.language("fr-FR"));
    map.setStyle(StyleOptions.view("auto"));
});
```

Dil "fr-FR" olarak ayarlanmış ve bölgesel görünüm "Auto" olarak ayarlanmış bir Azure Maps örneği aşağıda verilmiştir.

<center>

Azure haritalar ![, resimleri Fransızca](./media/how-to-use-android-map-control-library/android-localization.png)
göstermek için eşleyin </center>

Desteklenen dillerin ve bölgesel görünümlerin tamamen listesi [burada](supported-languages.md)belgelenmiştir.

## <a name="next-steps"></a>Sonraki adımlar

Haritada kaplama verileri eklemeyi öğrenin:

> [!div class="nextstepaction"]
> [Android haritasına sembol katmanı ekleme](https://review.docs.microsoft.com/azure/azure-maps/how-to-add-symbol-to-android-map)

> [!div class="nextstepaction"]
> [Android haritasına şekil ekleme](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)

> [!div class="nextstepaction"]
> [Android haritalar 'da harita stillerini değiştirme](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)
