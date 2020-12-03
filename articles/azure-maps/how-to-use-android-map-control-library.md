---
title: Azure haritalar 'ı kullanmaya başlama Android SDK
description: Microsoft Azure Maps Android SDK hakkında bilgi sahibi olun. Android Studio bir proje oluşturmayı, SDK 'yı yüklemeyi ve etkileşimli bir harita oluşturmayı öğrenin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 11/18/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 1da003bb8d285dbedde87cbc6cd4708fda2dc38b
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96531270"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Azure haritalar 'ı kullanmaya başlama Android SDK

Azure Maps Android SDK, Android için bir vektör eşleme kitaplığıdır. Bu makale, Azure Maps Android SDK yükleme ve eşleme yükleme işlemlerinde size rehberlik eder.

## <a name="prerequisites"></a>Önkoşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma

1. [Azure haritalar hesabı oluşturma](quick-demo-map-app.md#create-an-azure-maps-account)
2. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account).
Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).
3. [Google Android Studio indirin ve yükleyin](https://developer.android.com/studio/).

## <a name="create-a-project-in-android-studio"></a>Android Studio bir proje oluşturma

Android Studio projesi oluşturmak için aşağıdaki adımları uygulayın:

1. Android Studio başlatın.
2. **+ Yeni proje oluştur**' a tıklayın.
3. **Telefon ve tablet** sekmesinde **boş etkinlik**' e tıklayın. **İleri**’ye tıklayın.
4. **Projenizi yapılandırın** altında, `API 21: Android 5.0.0 (Lollipop)` En düşük SDK olarak öğesini seçin.
5. `Java`Dil olarak seçin.
6. Proje için varsayılanı kabul edin `Name` . **Finish (Son)** düğmesine tıklayın.

Android Studio yükleme ve yeni bir proje oluşturma hakkında daha fazla yardım için [Android Studio belgelerine](https://developer.android.com/studio/intro/) bakın.

![Android Studio 'da proje oluşturma ](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-device"></a>Bir cihaz kurma

Geliştirme sırasında uygulamanızı test etmek için bir Android telefonu veya bir Android öykünücüsü kullanabilirsiniz.

Bir AVD (Android sanal cihaz) ayarlama hakkında daha fazla bilgi edinmek için [Android Studio belgelerine](https://developer.android.com/studio/run/managing-avds)bakın.

## <a name="install-the-azure-maps-android-sdk"></a>Azure haritalar 'ı Android SDK

Uygulamanızı oluşturmanın bir sonraki adımı Android SDK Azure haritalar 'ı yüklemektir.

SDK 'Yı yüklemek için şu adımları uygulayın:

1. Proje sekmesinde **Gradle Scripts**' i genişletin. **Build. Gradle öğesini açın (proje: My_Application)** ve **tüm projelere** ve bölümüne aşağıdaki kodu ekleyin `repositories` :

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. **Build. Gradle öğesini açın (modül: My_Application)**.

3. Bölümünde **Minsdkversion** 'ın `defaultConfig` API 21 veya daha yüksek bir sürümde olduğundan emin olun.

4. Aşağıdaki kodu Android bölümüne ekleyin:

    ```
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    ```

5. Aşağıdaki kodu `dependencies` bölümüne ekleyin:

    ```
    implementation "com.microsoft.azure.maps:mapcontrol:0.6"
    ```

6. Ana araç çubuğunda **Dosya** ' ya tıklayın ve ardından **projeyi Gradle dosyalarıyla Eşitle**' yi seçin.

7. `res > layout > activity_main.xml` dosyasını açın. `Code`Sağ üst köşedeki Görünüm ' e tıklayın. Aşağıdaki XML öğesini öğesinin içine ekleyin `<androidx.constraintlayout.widget.ConstraintLayout>` .
    
    ```XML
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

8. Dosyasında şunları `java > com.example.myapplication > MainActivity.java` yapmanız gerekir:

    * Azure Maps SDK için içeri aktarmalar ekleyin.
    * Azure haritalar kimlik doğrulama bilgilerinizi ayarlayın.
    * **OnCreate** yönteminde Map denetim örneğini alın.

    Her uygulama görünümüne yönelik kimlik doğrulama bilgilerini ekleme zorunluluğunu ortadan kaldırmak için, çağırarak kimlik doğrulama bilgilerini küresel olarak ayarlayacağız `AzureMaps.setSubscriptionKey` . Ayrıca `AzureMaps.setAadProperties` , Azure Active Directory kullanarak kimlik doğrulaması yapmak istiyorsanız öğesini çağırabilirsiniz.

    Harita denetimi MainActivity sınıfının aşağıdaki yaşam döngüsü yöntemlerini geçersiz kılar. Bu yöntemler, Android 'in OpenGL yaşam döngüsünü yönetmekten sorumludur.

    * onCreate (paket)
    * onStart ()
    * Onözgeçmişi ()
    * onPause ()
    * onStop ()
    * onDestroy ()
    * Onsaveınstancestate (paket)
    * onLowMemory ()

    `MainActivity.java`Dosyayı aşağıdaki gibi düzenleyin:

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

>[!WARNING]
>Android Studio gerekli sınıfları içeri aktarmayabilir.  Sonuç olarak, kodun çözümlenemeyecek bazı başvuruları olacaktır. Gerekli sınıfları içeri aktarmak için, çözülmemiş her başvurunun üzerine gelin ve `Alt + Enter` (seçenek + bir Mac üzerinde dön) tuşuna basın.

Android Studio uygulamayı derlemek birkaç saniye sürer. Oluşturma işlemi tamamlandıktan sonra, uygulamanızı öykünülmüş Android cihazında test edebilirsiniz. Aşağıdakine benzer bir eşleme görmeniz gerekir:

:::image type="content" source="./media/how-to-use-android-map-control-library/android-map.png" border="true" alt-text="Android uygulamasında Azure Maps":::

## <a name="localizing-the-map"></a>Haritayı yerelleştirme

Azure Haritalar Android SDK, eşlemenin dilini ve bölgesel ayarlarını ayarlamanın üç farklı yolunu sağlar.

1. AzureMaps sınıfında statik yöntemler çağırarak dil ve bölge ayarlarını belirleyin.

    ```Java
    static {
        //Set your Azure Maps Key.
        AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");

        //Set the language to be used by Azure Maps.
        AzureMaps.setLanguage("fr-FR");

        //Set the regional view.
        AzureMaps.setLanguage("Auto");
    
    }
    ```

2. Harita denetim XML dosyasında dil ve bölge ayarlarını tanımlayın.

    ```XML
    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/myMap"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_language="fr-FR"
        app:mapcontrol_view="Auto"
        />
    ```

3. Harita denetimindeki yöntemleri çağırarak dil ve bölge ayarlarını belirleyin. Bu seçenek çalışma zamanı sırasında ayarları değiştirmenize izin verir.

    ```Java
    mapControl.onReady(map -> {
        map.setStyle(StyleOptions.language("fr-FR"));
        map.setStyle(StyleOptions.view("Auto"));
    
    });
    ```

Dil olarak ayarlanan Azure Maps örneği aşağıda verilmiştir `fr-FR` .

:::image type="content" source="./media/how-to-use-android-map-control-library/android-localization.png" border="true" alt-text="Azure haritalar, etiketleri Fransızca olarak gösteren harita resmi":::

Desteklenen dillerin ve bölgesel görünümlerin tamamen listesi [burada](supported-languages.md)belgelenmiştir.

## <a name="navigating-the-map"></a>Haritada gezinme

Haritanın Yakınlaştırılıp Yakınlaştırılıp döndürülmemiş, döndürülebileceği ve açık bir şekilde birçok farklı yolu vardır. Haritada gezinmek için kullanabileceğiniz farklı yollar aşağıda verilmiştir.

**Haritayı Yakınlaştır**

- Parmakların ölçeğini yakınlaştırmak veya yaymak için iki parmağınızla ve Pinç ile Haritayı dokunarak bir araya geçirin.
- Bir düzey yakınlaştırmak için haritaya çift dokunun.
- Haritayı bir düzey büyütmek için iki parmağınızla çift dokunun.
- İki kez dokunun; İkinci tap haritada parmak izi tutun ve yakınlaştırıp yakınlaştırmak için yukarı veya aşağı sürükleyin.

**Haritayı kaydır**

- Haritayı dokunarak istediğiniz yönde sürükleyin.

**Haritayı döndürme**

- İki parmağınızla haritaya dokunun ve döndürün.

**Haritayı sıklık**

- İki parmağınızla Haritayı dokunarak yukarı veya aşağı sürükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Haritada kaplama verileri eklemeyi öğrenin:

> [!div class="nextstepaction"]
> [Android haritasına sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Android haritasına şekil ekleme](./how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [Android haritalar 'da harita stillerini değiştirme](./set-android-map-styles.md)