---
title: 'Hızlı başlangıç: Azure Maps ile bir Android uygulaması oluşturma | Microsoft Azure '
description: 'Hızlı başlangıç: Azure Maps Android SDK kullanarak bir Android uygulaması oluşturmayı öğrenin.'
author: rbrundritt
ms.author: richbrun
ms.date: 12/10/2020
ms.topic: quickstart
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: mvc
ms.openlocfilehash: 874da5ffcd2777648bb06c6e702932d64a32e59c
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97682019"
---
# <a name="quickstart-create-an-android-app-with-azure-maps"></a>Hızlı başlangıç: Azure Maps ile bir Android uygulaması oluşturma

Bu makalede, Azure haritalarını bir Android uygulamasına nasıl ekleyeceğiniz gösterilmektedir. Bu temel adımlarda size kılavuzluk eder:

* Geliştirme ortamınızı ayarlayın.
* Kendi Azure haritalar hesabınızı oluşturun.
* Uygulamada kullanmak üzere birincil Azure haritalar anahtarınızı alın.
* Projedeki Azure Maps kitaplıklarına başvurun.
* Uygulamaya bir Azure Maps denetimi ekleyin.

## <a name="prerequisites"></a>Önkoşullar

1. [Azure Portal](https://portal.azure.com)oturum açarak bir Azure Maps hesabı oluşturun. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
2. [Azure haritalar hesabı oluşturma](quick-demo-map-app.md#create-an-azure-maps-account)
3. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account). Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](how-to-manage-authentication.md).
4. [Android Studio](https://developer.android.com/studio/) Google 'Dan ücretsiz indirin.

## <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma

Aşağıdaki adımlarla yeni bir Azure Maps hesabı oluşturun:

1. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. Market 'te *Ara* kutusuna **Azure Maps** yazın.
3. *Sonuçlardan* **Azure haritaları**' nı seçin. Haritanın altında görüntülenen **Oluştur** düğmesine tıklayın.
4. **Haritalar Hesabı Oluştur** sayfasında aşağıdaki değerleri girin:
    * Bu hesap için kullanmak istediğiniz *Abonelik*.
    * Bu hesap için *Kaynak grubu* adı. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    * Yeni hesabınıza verilen *Ad*.
    * Bu hesabın *fiyatlandırma katmanı* .
    * *Lisans*’ı ve *Gizlilik Bildirimi*’ni okuyun ve onay kutusunu işaretleyerek koşulları kabul edin.
    * **Oluştur** düğmesine tıklayın.

    ![Portalda Haritalar hesabı oluşturma](media/quick-android-map/create-account.png)

## <a name="get-the-primary-key-for-your-account"></a>Hesabınızın birincil anahtarını alma

Haritalar hesabınız başarıyla oluşturulduktan sonra, Haritalar API 'Lerini sorgulamanızı sağlayan birincil anahtarı alın.

1. Portalda Haritalar hesabınızı açın.
2. Ayarlar bölümünde **kimlik doğrulaması**' nı seçin.
3. **Birincil Anahtar**’ı panonuza kopyalayın. Bu öğreticinin ilerleyen kısmında kullanmak üzere bunu yerel olarak kaydedin.

>[!NOTE]
> Azure Maps birincil anahtarı yerine Azure abonelik anahtarını kullanırsanız, haritanız düzgün şekilde işlenmez. Ayrıca, güvenlik nedeniyle, birincil ve ikincil anahtarlarınız arasında döndürme yapmanız önerilir. Anahtarları döndürmek için uygulamanızı ikincil anahtarı kullanacak şekilde güncelleştirin, dağıtın ve ardından birincil anahtarın yanındaki döngüye/Yenile düğmesine basarak yeni bir birincil anahtar oluşturun. Eski birincil anahtar devre dışı bırakılacak. Anahtar döndürme hakkında daha fazla bilgi için bkz. [anahtar döndürme ve denetleme ile Azure Key Vault ayarlama](https://docs.microsoft.com/azure/key-vault/secrets/key-rotation-log-monitoring)

![Azure portal birincil anahtar Azure Haritalar anahtarını al](media/quick-android-map/get-key.png)

## <a name="create-a-project-in-android-studio"></a>Android Studio bir proje oluşturma

İlk olarak, boş bir etkinliğe sahip yeni bir proje oluşturun. Android Studio projesi oluşturmak için aşağıdaki adımları uygulayın:

1. **Projenizi seçin** altında **telefon ve tablet**' yı seçin. Uygulamanız bu form faktörüyle çalışır.
2. **Telefon ve tablet** sekmesinde **boş etkinlik**' i seçin ve ardından **İleri**' yi seçin.
3. **Projenizi yapılandırın** altında, `API 21: Android 5.0.0 (Lollipop)` En düşük SDK olarak öğesini seçin. Bu, Azure Maps Android SDK tarafından desteklenen en eski sürümdür.
4. Varsayılanı kabul edin `Activity Name` ve `Layout Name` **son**' u seçin.

Android Studio yükleme ve yeni bir proje oluşturma hakkında daha fazla yardım için [Android Studio belgelerine](https://developer.android.com/studio/intro/) bakın.

![Android Studio 'da proje oluşturma](media/quick-android-map/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Sanal cihaz ayarlama

Android Studio, bilgisayarınızda bir sanal Android cihaz ayarlamanıza olanak sağlar. Bunun yapılması, geliştirme sırasında uygulamanızı test etmenize yardımcı olabilir. Bir sanal cihaz ayarlamak için, proje ekranınızın sağ üst köşesindeki Android sanal cihaz (AVD) Yöneticisi simgesini seçin ve ardından **sanal cihaz oluştur**' u seçin. Ayrıca, araç çubuğundan **Araçlar**  >  **Android**  >  **AVD Yöneticisi** ' ni seçerek AVD yöneticisine da ulaşabilirsiniz. **Telefonlar** kategorisinde, **Nexus 5x**' i seçin ve ardından **İleri**' yi seçin.

[Android Studio belgelerinde](https://developer.android.com/studio/run/managing-avds)bir AVD ayarlama hakkında daha fazla bilgi edinebilirsiniz.

![Android Emulator](media/quick-android-map/android-emulator.png)

## <a name="install-the-azure-maps-android-sdk"></a>Azure haritalar 'ı Android SDK

Uygulamanızı oluşturmanın bir sonraki adımı Android SDK Azure haritalar 'ı yüklemektir. SDK 'Yı yüklemek için şu adımları uygulayın:

1. En üst düzey **Build. Gradle** dosyasını açın ve aşağıdaki kodu **tüm projeler**, **depolar** bloğu bölümüne ekleyin:

    ```java
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

    3. Bağımlılıklar engellemeyi güncelleştirin ve en son Azure Maps Android SDK yeni bir uygulama bağımlılığı satırı ekleyin:

        ```java
        implementation "com.microsoft.azure.maps:mapcontrol:0.6"
        ```

        > [!Note]
        > Kodunuzun her zaman en son sürümü göstermesi için sürüm numarasını "0 +" olarak ayarlayabilirsiniz.

    4. Araç çubuğunda **dosyasına** gidin ve ardından **projeyi Gradle dosyalarıyla Eşitle**' ye tıklayın.
3. Ana etkinliğe bir harita parçası ekleyin (Res \> Düzen \> etkinliği \_main.xml):

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

    `AzureMaps`Ya da yöntemlerini kullanarak sınıftaki kimlik doğrulama bilgilerini genel olarak ayarlamak, `setSubscriptionKey` `setAadProperties` kimlik doğrulama bilgilerinizi her görünüme eklemeniz gerekmez.

    Harita denetimi, Android 'ın OpenGL yaşam döngüsünü yönetmeye yönelik kendi yaşam döngüsü yöntemlerini içerir. Bu yaşam döngüsü yöntemlerinin doğrudan içeren etkinlikten çağrılması gerekir. Uygulamanızın harita denetiminin yaşam döngüsü yöntemlerini doğru bir şekilde çağırması için, eşleme denetimini içeren etkinliğin aşağıdaki yaşam döngüsü yöntemlerini geçersiz kılmanız gerekir. Ve ilgili Map denetim yöntemini çağırmanız gerekir.

    * `onCreate(Bundle)`
    * `onStart()`
    * `onResume()`
    * `onPause()`
    * `onStop()`
    * `onDestroy()`
    * `onSaveInstanceState(Bundle)`
    * `onLowMemory()`

    **MainActivity. Java** dosyasını aşağıdaki gibi düzenleyin:

    ```Java
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

        //Alternatively use Azure Active Directory authenticate.
        //AzureMaps.setAadProperties("<Your aad clientId>", "<Your aad AppId>", "<Your aad Tenant>");
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
    }}
    ```

    > [!NOTE]
    > Yukarıdaki adımları tamamladıktan sonra, büyük olasılıkla bazı kod hakkında Android Studio uyarılar alırsınız. Bu uyarıları çözümlemek için, içinde başvurulan sınıfları içeri aktarın `MainActivity.java` .
    > `Alt`  +  `Enter` ( `Option`  +  `Return` Mac 'te) öğesini seçerek bu sınıfları otomatik olarak içeri aktarabilirsiniz.

5. Uygulamanızı derlemek için aşağıdaki grafikte gösterildiği gibi Çalıştır düğmesini seçin (veya Mac 'e basın `Control`  +  `R` ).

    ![Çalıştır’a tıklayın](media/quick-android-map/run-app.png)

Android Studio uygulamayı derlemek birkaç saniye sürer. Oluşturma işlemi tamamlandıktan sonra, uygulamanızı öykünülmüş Android cihazında test edebilirsiniz. Aşağıdakine benzer bir eşleme görmeniz gerekir:

![Android uygulamasında Azure Maps](media/quick-android-map/quickstart-android-map.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

>[!WARNING]
> [Sonraki adımlar](#next-steps) bölümünde listelenen öğreticiler, Azure haritalar 'ı hesabınızla birlikte nasıl kullanacağınızı ve yapılandıracağınızı ayrıntılandırır. Öğreticilere devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin.

Öğreticilere devam etmeyi planlamıyorsanız, kaynakları temizlemek için şu adımları uygulayın:

1. Android Studio kapatın ve oluşturduğunuz uygulamayı silin.
2. Uygulamayı bir dış cihazda test ederseniz, uygulamayı bu cihazdan kaldırın.

Azure haritalar ile geliştirmeye devam etme konusunda planlamadıysanız Android SDK:

1. Azure portal sayfasına gidin. Ana Portal sayfasından **tüm kaynaklar** ' ı seçin. Ya da sol üst köşedeki menü simgesine tıklayın. **Tüm kaynaklar**’ı seçin.
2. Azure haritalar hesabınıza tıklayın. Sayfanın üst kısmında **Sil**' e tıklayın.
3. İsteğe bağlı olarak, Android uygulamaları geliştirmeye devam etmeyi planlamıyorsanız Android Studio ' ı kaldırın.

Daha fazla kod örneği için şu kılavuzlara bakın:

* [Azure haritalar 'da kimlik doğrulamasını yönetme](how-to-manage-authentication.md)
* [Android haritalar 'da harita stillerini değiştirme](set-android-map-styles.md)
* [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)
* [Çizgi katmanı ekleme](android-map-add-line-layer.md)
* [Çokgen katmanı ekleme](how-to-add-shapes-to-android-map.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Maps hesabınızı oluşturdunuz ve bir demo uygulaması oluşturdunuz. Azure haritalar hakkında daha fazla bilgi edinmek için aşağıdaki öğreticilere göz atın:

> [!div class="nextstepaction"]
> [Coğrafi JSON verilerini Azure Maps 'e yükleme](tutorial-load-geojson-file-android.md)
