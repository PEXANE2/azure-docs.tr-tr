---
title: 'Öğretici: Yeni bir Android uygulaması oluşturun'
description: Bu eğitimde, Azure Uzamsal Çapaları kullanarak yeni bir Android uygulaması oluşturmayı öğrenirsiniz.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 04/03/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: e1773ef81a5b727187a9a69ccc7ce7ad0421fb2c
ms.sourcegitcommit: 940e16ff194d5163f277f98d038833b1055a1a3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80246781"
---
# <a name="tutorial-step-by-step-instructions-to-create-a-new-android-app-using-azure-spatial-anchors"></a>Öğretici: Azure Uzamsal Çapaları kullanarak yeni bir Android uygulaması oluşturmak için adım adım talimatlar

Bu öğretici, ARCore işlevselliğini Azure Uzamsal Çapalarıyla bütünleştiren yeni bir Android uygulamasının nasıl oluşturulacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

- <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.4+</a>ile bir Windows veya macOS makinesi.
- Bir <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">geliştirici etkin</a> ve <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore yetenekli</a> Android cihaz.

## <a name="getting-started"></a>Başlarken

Android Studio'yı başlatın. Android **Studio'ya Hoş Geldiniz** penceresinde, **yeni bir Android Studio projesini başlat'ı**tıklatın. Veya, zaten açılmış bir projeniz varsa, Yeni Dosya->**Dosyası'nı**seçin. **File**

Yeni **Proje Oluştur** penceresinde, **Telefon ve Tablet** bölümünün altında Boş **Etkinlik'i**seçin ve **İleri'yi**tıklatın. Daha sonra, **Minimum API düzeyi**altında, `API 26: Android 8.0 (Oreo)`seçin ve **Dil'in** `Java`. Proje Adını konum & ve Paket adını değiştirmek isteyebilirsiniz. Diğer seçenekleri olduğu gibi bırakın. **Son**'a tıklayın. **Bileşen Yükleyici** çalışır. Bittikten sonra **Finish'e**tıklayın. Bazı işlemlerden sonra, Android Studio IDE'yi açacak.

## <a name="trying-it-out"></a>Denemek

Yeni uygulamanızı test etmek için geliştirici özellikli cihazınızı bir USB kablosuyla geliştirme makinenize bağlayın. **Çalıştır**->**'uygulamasını'** tıklatın. Dağıtım **Hedefini Seç** penceresinde cihazınızı seçin ve **Tamam'ı**tıklatın. Android Studio uygulamayı bağlı cihazınıza yükler ve başlatır. Şimdi "Merhaba Dünya!" cihazınızda çalışan uygulamada görüntülenir. **'uygulamayı' Durdur'u**->**Çalıştır'ı**tıklatın.

## <a name="integrating-_arcore_"></a>_ARCore_ Entegre

<a href="https://developers.google.com/ar/discover/" target="_blank">_ARCore,_</a> Google'ın Artırılmış Gerçeklik deneyimleri oluşturmak için oluşturduğu platformdur ve cihazınızın hareket ettikçe ve gerçek dünya hakkında kendi anlayışını oluştururken konumunu izlemesini sağlar.

Aşağıdaki `app\manifests\AndroidManifest.xml` girişleri kök `<manifest>` düğümün içine eklemek için değiştirin. Bu kod parçacığı birkaç şey yapar:

- Uygulamanızın cihazınızın kamerasına erişmesine olanak tanır.
- Ayrıca, uygulamanızın yalnızca Google Play Store'da ARCore'u destekleyen cihazlarda görünür olmasını sağlar.
- Google Play Store'u, uygulamanız yüklendiğinde, zaten yüklenmediği takdirde ARCore'u indirip yüklemek üzere yapılandıracaktır.

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera.ar" />

<application>
    ...
    <meta-data android:name="com.google.ar.core" android:value="required" />
    ...
</application>
```

Aşağıdaki `Gradle Scripts\build.gradle (Module: app)` girişi içerecek şekilde değiştirin. Bu kod, uygulamanızın ARCore sürüm 1.8'i hedeflemesini sağlar. Bu değişiklikten sonra, Gradle'den eşitlemenizi isteyen bir bildirim alabilirsiniz: **Şimdi Eşitle'yi**tıklatın.

```
dependencies {
    ...
    implementation 'com.google.ar:core:1.11.0'
    ...
}
```

## <a name="integrating-_sceneform_"></a>Sahne _Formunu_ Tümleştirme

[_Sceneform,_](https://developers.google.com/sceneform/develop/) OpenGL öğrenmek zorunda kalmadan Artırılmış Gerçeklik uygulamalarında gerçekçi 3D sahneleri oluşturmayı kolaylaştırır.

Aşağıdaki `Gradle Scripts\build.gradle (Module: app)` girişleri içerecek şekilde değiştirin. Bu kod, uygulamanızın Java 8'deki dil `Sceneform` yapılarını kullanmasına olanak sağlar ve bu da bunu gerektirir. Ayrıca, uygulamanızın kullandığı `Sceneform` ARCore sürümüyle eşleşmesi gerektiğinden, uygulamanızın sürüm 1.8'i hedeflemesini de sağlar. Bu değişiklikten sonra, Gradle'den eşitlemenizi isteyen bir bildirim alabilirsiniz: **Şimdi Eşitle'yi**tıklatın.

```
android {
    ...

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    ...
    implementation 'com.google.ar.sceneform.ux:sceneform-ux:1.11.0'
    ...
}
```

'nizi `app\res\layout\activity_main.xml`açın ve varolan `<TextView>` Hello Wolrd öğesini aşağıdaki ArFragment ile değiştirin. Bu kod, kamera akışının ekranınızda görüntülenmesine neden olarak ARCore'un hareket ederken cihazınızın konumunu izlemesine olanak tanır.

```xml
<fragment android:name="com.google.ar.sceneform.ux.ArFragment"
    android:id="@+id/ux_fragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

Uygulamanızı bir kez daha doğrulamak için cihazınıza [yeniden dağıtın.](#trying-it-out) Bu kez, kamera izinleri istenmelidir. Onaylandıktan sonra, ekranınızda kamera akışı oluşturma yı görmeniz gerekir.

## <a name="place-an-object-in-the-real-world"></a>Bir nesneyi gerçek dünyaya yerleştirme

Uygulamanızı kullanarak bir nesne & oluşturalım. İlk olarak, aşağıdaki içeri `app\java\<PackageName>\MainActivity`alma ekleyin:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=23-33)]

Ardından, sınıfınıza `MainActivity` aşağıdaki üye değişkenleri ekleyin:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=52-57)]

Ardından, yönteminize `app\java\<PackageName>\MainActivity` `onCreate()` aşağıdaki kodu ekleyin. Bu kod, kullanıcının cihazınızdaki `handleTap()`ekrana dokunduğunda algılayacak olan bir dinleyiciyi bağlar. Musluk zaten ARCore izleme tarafından kabul edilmiştir gerçek bir dünya yüzeyinde olur, dinleyici çalışacaktır.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=68-74,85&highlight=6-7)]

Son olarak, `handleTap()` birlikte her şeyi kravat aşağıdaki yöntemi ekleyin. Bir küre oluşturacak ve onu dinlenen yere yerleştirilecek. Küre başlangıçta siyah olacak, `this.recommendedSessionProgress` çünkü şu anda sıfıra ayarlanmıştır. Bu değer daha sonra ayarlanır.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=150-158,170-171,174-182,198-199)]

Uygulamanızı bir kez daha doğrulamak için cihazınıza [yeniden dağıtın.](#trying-it-out) Bu kez, ARCore'un ortamınızı tanımaya başlamasını sağlamak için cihazınızın etrafında dolaşabilirsiniz. Ardından, siyah kürenizi seçtiğiniz yüzeyin üzerine yerleştirmek & oluşturmak için ekrana dokunun.

## <a name="attach-a-local-azure-spatial-anchor"></a>Yerel bir Azure Uzamsal Çapa ekleme

Aşağıdaki `Gradle Scripts\build.gradle (Module: app)` girişi içerecek şekilde değiştirin. Bu kod, uygulamanızın Azure Uzamsal Çapalar sürüm 2.2.0'ı hedeflemesini sağlar. Bununla birlikte, Azure Uzamsal Çapalar'ın son sürümüne atıfta bulunularak işe yaraması gerekir. Sürüm notlarını [burada bulabilirsiniz.](https://github.com/Azure/azure-spatial-anchors-samples/releases)

```
dependencies {
    ...
    implementation "com.microsoft.azure.spatialanchors:spatialanchors_jni:[2.2.0]"
    implementation "com.microsoft.azure.spatialanchors:spatialanchors_java:[2.2.0]"
    ...
}
```

Sağ tıklayın `app\java\<PackageName>` -> **Yeni**->**Java Sınıfı**. _MyFirstApp_için **Adı** ayarlayın ve **superclass** _android.app.Application_için . Diğer seçenekleri olduğu gibi bırakın. **Tamam**'a tıklayın. Adlı `MyFirstApp.java` bir dosya oluşturulur. Buna aşağıdaki içe aktarma ekleyin:

```java
import com.microsoft.CloudServices;
```

Ardından, Azure Uzamsal `MyFirstApp` Bağlantı çapalarının uygulamanızın bağlamıyla başlatılmasını sağlayacak aşağıdaki kodu yeni sınıfın içine ekleyin.

```java
    @Override
    public void onCreate() {
        super.onCreate();
        CloudServices.initialize(this);
    }
```

Şimdi, `app\manifests\AndroidManifest.xml` kök düğümü içinde aşağıdaki `<application>` girişi içerecek şekilde değiştirin. Bu kod, oluşturduğunuz Uygulama sınıfını uygulamanıza bağlar.

```xml
    <application
        android:name=".MyFirstApp"
        ...
    </application>
```

Geri, `app\java\<PackageName>\MainActivity`içine aşağıdaki içeri alma ekleyin:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=33-40&highlight=2-8)]

Ardından, sınıfınıza `MainActivity` aşağıdaki üye değişkenleri ekleyin:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=57-60&highlight=3-4)]

Ardından, sınıfınızın `mainActivity` içine `initializeSession()` aşağıdaki yöntemi ekleyelim. Çağrıldıktan sonra, uygulamanızın başlatılması sırasında bir Azure Uzamsal Çapa oturumu oluşturulmasını ve düzgün bir şekilde başlatılmasını sağlar.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=89-97,146)]

Şimdi, yöntemini `initializeSession()` senin yönteminle `onCreate()` bağlayalım. Ayrıca, kamera akışınızdaki karelerin işlenmek üzere Azure Uzamsal Çapalar SDK'ya gönderilmesini sağlarız.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=68-85&highlight=9-17)]

Son olarak, yönteminize `handleTap()` aşağıdaki kodu ekleyin. Gerçek dünyaya yerleştirdiğimiz siyah küreye yerel bir Azure Uzamsal Çapa ekleyecek.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=150-158,170-182,198-199&highlight=12-13)]

Uygulamanızı bir kez daha [yeniden dağıtın.](#trying-it-out) Cihazınızın etrafında hareket ettirin, ekrana dokunun ve siyah bir küre yerleştirin. Ancak bu kez, kodunuz kürenize yerel bir Azure Uzamsal Çapa oluşturacak ve ekleyecek.

Daha fazla ilerlemeden önce, zaten sahip değilseniz bir Azure Uzamsal Çapa hesap Tanımlayıcısı ve Anahtarı oluşturmanız gerekir. Bunları elde etmek için aşağıdaki bölümü izleyin.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="upload-your-local-anchor-into-the-cloud"></a>Yerel çapanızı buluta yükleyin

Azure Uzamsal Çapalar hesap Tanımlayıcınızı ve Anahtarınızı aldıktan sonra, geri dönebilir `app\java\<PackageName>\MainActivity`ve aşağıdaki içeri leri ekleyebiliriz:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=40-45&highlight=3-6)]

Ardından, sınıfınıza `MainActivity` aşağıdaki üye değişkenleri ekleyin:

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=60-65&highlight=3-6)]

Şimdi, yönteminize `initializeSession()` aşağıdaki kodu ekleyin. İlk olarak, bu kod, uygulamanızın Azure Uzamsal Çapalar SDK'nın kamera akışınızdan kareler toplarken kaydettiği ilerlemeyi izlemesine olanak tanır. Olduğu gibi, kürenizin rengi orijinal siyahından griye dönüşmeye başlayacaktır. Daha sonra, çapanızı buluta göndermek için yeterli kare toplandıktan sonra beyaza döner. İkinci olarak, bu kod bulut arka uç ile iletişim kurmak için gerekli kimlik bilgilerini sağlayacaktır. Uygulamanızı hesap Tanımlayıcınızı ve Anahtar'ınızı kullanacak şekilde yapılandıracağınız yer burasıdır. [Uzamsal Çapalar kaynağını ayarlarken](#create-a-spatial-anchors-resource)bunları bir metin düzenleyicisine kopyaladınız.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=89-120,142-146&highlight=11-36)]

Ardından, sınıfınızın `uploadCloudAnchorAsync()` `mainActivity` içine aşağıdaki yöntemi ekleyin. Çağrıldıktan sonra, bu yöntem cihazınızdan yeterli kare toplanana kadar eş senkronize bir şekilde bekler. Bu gerçekleşir gerçekleşmez, kürenizin rengini sarıya dönüştürür ve ardından yerel Azure Uzamsal Çapanızı buluta yüklemeye başlar. Yükleme tamamlandığında, kod bir bağlantı tanımlayıcısı döndürecek.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?name=uploadCloudAnchorAsync)]

Son olarak, her şeyi birbirine bağlayalım. Yönteminizde `handleTap()` aşağıdaki kodu ekleyin. Küreniz oluşturulur `uploadCloudAnchorAsync()` oluşturulmaz yönteminizi çağırır. Yöntem döndükten sonra, aşağıdaki kod kürenize son bir güncelleştirme gerçekleştirerek rengini maviye değiştirir.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=150-158,170-199&highlight=24-37)]

Uygulamanızı bir kez daha [yeniden dağıtın.](#trying-it-out) Cihazınızın etrafında hareket edin, ekrana dokunun ve kürenizi yerleştirin. Ancak bu kez küreniz, kamera çerçeveleri toplandıkça rengini siyahtan beyaza doğru değiştirecek. Yeterli çerçeveye sahip olduğumuzda küre sarıya döner ve bulut yüklemesi başlar. Yükleme tamamlandığında küreniz maviye dönecektir. İsteğe bağlı olarak, `Logcat` uygulamanızın gönderdiği günlük iletilerini izlemek için Android Studio'nun içindeki pencereyi de kullanabilirsiniz. Örneğin, kare yakalama sırasında oturum ilerleme ve yükleme tamamlandıktan sonra bulutun döndüğü bağlantı tanımlayıcısı.

## <a name="locate-your-cloud-spatial-anchor"></a>Bulut uzamsal çapanızı bulun

Çapanızın buluta yüklenmesi, onu tekrar bulmaya hazırız. Öncelikle kodunuza aşağıdaki içeri aktarımları ekleyelim.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?range=45-48&highlight=3-4)]

Ardından, yönteminize `handleTap()` aşağıdaki kodu ekleyelim. Bu kod:

- Mevcut mavi küremizi ekrandan kaldırın.
- Azure Uzamsal Çapalar oturumumuzu yeniden başlatma. Bu eylem, bulabildiğimiz çapanın oluşturduğumuz yerel çapa yerine buluttan olmasını sağlayacaktır.
- Buluta yüklediğimiz bağlantı noktası için bir sorgu düzenleyin.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?name=handleTap&highlight=10-19)]

Şimdi, aradığımız bağlantı bulunduğunda çağrılacak kodu bağlayalım. Yönteminizin `initializeSession()` içine aşağıdaki kodu ekleyin. Bu parçacık, bulut uzamsal çapası bulunduğunda yeşil bir küre & oluşturacaktır. Ayrıca ekrana tekrar dokunmayı da etkinleştirecek, böylece tüm senaryoyu bir kez daha tekrarlayabilirsiniz: başka bir yerel bağlantı noktası oluşturun, yükleyin ve yeniden bulun.

[!code-java[MainActivity](../../../includes/spatial-anchors-new-android-app-finished.md?name=initializeSession&highlight=34-53)]

İşte bu kadar! Tüm senaryonun sonuna kadar denemek için uygulamanızı son bir kez [yeniden dağıtın.](#trying-it-out) Cihazınızın etrafında hareket edin ve siyah kürenizi yerleştirin. Ardından, küre sarıya dönene kadar kamera çerçevelerini yakalamak için cihazınızı hareket ettirmeye devam edin. Yerel çapanız yüklenir ve küreniz maviye döner. Son olarak, yerel bağlantınızın kaldırılması için ekranınıza bir kez daha dokunun ve bulut muadili için sorgu yaparız. Bulut uzamsal çapanız bulunana kadar cihazınızı hareket ettirin. Yeşil bir küre doğru konumda görünmelidir ve tüm senaryoyu tekrar & durulayabilirsiniz.

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-new-android-app-finished.md)]
