---
title: Apache Cordova uygulamasına anında iletme bildirimleri ekleme
description: Apache Cordova uygulamanıza anında iletme bildirimleri göndermek için Mobil Uygulamaları nasıl kullanacağınızı öğrenin.
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 08260437076728421cb6fa393f481d27b95b1782
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461615"
---
# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Apache Cordova uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, [Apache Cordova hızlı başlatma][5] projesine anında iletme bildirimleri eklersiniz, böylece her kayıt ekildiğinde aygıta bir anında iletme bildirimi gönderilir.

İndirilen quickstart sunucu projesini kullanmıyorsanız, anında iletme bildirimi uzantısı paketine ihtiyacınız olur. Daha fazla bilgi için, [Mobil Uygulamalar için .NET arka uç sunucusu SDK ile çalışma'ya][1]bakın.

## <a name="prerequisites"></a><a name="prerequisites"></a>Ön koşullar

Bu öğretici, Visual Studio 2015 ile geliştirilmiş bir Apache Cordova uygulamanız olduğunu varsayar. Bu cihaz Google Android Emülatörü, Bir Android cihazı, bir Windows aygıtı veya bir iOS aygıtında çalıştırılmalıdır.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Visual Studio Community 2015][2] veya sonrası bilgisayar
* [Apache Cordova için Visual Studio Araçları][4]
* Etkin bir [Azure hesabı][3]
* Tamamlanmış [bir Apache Cordova quickstart][5] projesi
* (Android) Doğrulanmış e-posta adresine sahip bir [Google hesabı][6]
* (iOS) [Apple Geliştirici Programı üyeliği][7] ve iOS aygıtı (iOS Simulator anında iletme bildirimlerini desteklemez)
* (Windows) Microsoft [Mağazası Geliştirici Hesabı][8] ve Windows 10 aygıtı

## <a name="configure-a-notification-hub"></a><a name="configure-hub"></a>Bildirim hub’ını yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Bu bölümdeki adımları gösteren bir video izleyin.][9]

## <a name="update-the-server-project"></a>Sunucu projesini güncelleştirme

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="modify-your-cordova-app"></a><a name="add-push-to-app"></a>Cordova uygulamanızı değiştirin

Apache Cordova uygulama projenizin anında iletme bildirimlerini işlemeye hazır olduğundan emin olmak için Cordova push eklentisini ve platforma özel itme hizmetlerini yükleyin.

#### <a name="update-the-cordova-version-in-your-project"></a>Projenizdeki Cordova sürümünü güncelleştirin.

Projenizde Apache Cordova'nın sürüm 6.1.1'den önceki bir sürümü kullanıyorsa, istemci projesini güncelleştirin. Projeyi güncelleştirmek için aşağıdaki adımları izleyin:

* Yapılandırma tasarımcısını açmak için `config.xml`sağ tıklatın.
* **Platformlar** sekmesini seçin.
* **Cordova CLI** metin kutusunda **6.1.1'i**seçin. 
* Projeyi güncelleştirmek için **Yapı'yı**seçin ve ardından **Çözüm Oluştur'u**seçin.

#### <a name="install-the-push-plugin"></a>Push eklentisini yükleme

Apache Cordova uygulamaları aygıt veya ağ özelliklerini yerel olarak işlemez.  Bu özellikler, [npm][10] veya GitHub'da yayınlanan eklentiler tarafından sağlanır. `phonegap-plugin-push` Eklenti ağ anında iletme bildirimlerini işler.

Push eklentisini aşağıdaki yollardan birine yükleyebilirsiniz:

**Komut isteminden:**

Şu komutu çalıştırın:

    cordova plugin add phonegap-plugin-push

**Visual Studio içinden:**

1. Çözüm Gezgini'nde `config.xml` dosyayı açın. Ardından, **Eklentiler** > **Özel**seçin. Ardından yükleme kaynağı olarak **Git'i** seçin.

2. Kaynak `https://github.com/phonegap/phonegap-plugin-push` olarak girin.

    ![Solution Explorer'da config.xml dosyasını açın][img1]

3. Yükleme kaynağının yanındaki oku seçin.

4. **SENDER_ID,** Google Geliştirici Konsolu projesi için zaten sayısal bir proje kimliğiniz varsa, buraya ekleyebilirsiniz. Aksi takdirde, 777777 gibi bir yer tutucu değeri girin. Android'i hedefliyorsanız, bu değeri daha sonra config.xml dosyasında güncelleştirebilirsiniz.

    >[!NOTE]
    >Sürüm 2.0.0 itibariyle, gönderen kimliğini yapılandırmak için google-services.json'Un projenizin kök klasörüne yüklenmesi gerekir. Daha fazla bilgi için [yükleme belgelerine bakın.](https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md)

5. **Ekle'yi**seçin.

Push eklentisi artık yüklendi.

#### <a name="install-the-device-plugin"></a>Aygıt eklentisini yükleme

Push eklentisini yüklemek için kullandığınız yordamı uygulayın. Çekirdek eklentileri listesinden Aygıt eklentisini ekleyin. (Bulmak **için, Eklentiler** > **Çekirdek**seçin.) Platform adını elde etmek için bu eklentiye ihtiyacınız var.

#### <a name="register-your-device-when-the-application-starts"></a>Uygulama başladığında cihazınızı kaydettirin 

Başlangıçta, Android için bazı minimal kod lar içerir. Daha sonra uygulamayı iOS veya Windows 10'da çalışacak şekilde değiştirebilirsiniz.

1. Oturum açma işlemi için geri arama sırasında **ForPushBildirims'i kaydetmek** için bir çağrı ekleyin. Alternatif olarak, **onDeviceReady** yönteminin en altında ekleyebilirsiniz:

    ```javascript
    // Log in to the service.
    client.login('google')
        .then(function () {
            // Create a table reference.
            todoItemTable = client.getTable('todoitem');

            // Refresh the todoItems.
            refreshDisplay();

            // Wire up the UI Event Handler for the Add Item.
            $('#add-item').submit(addItemHandler);
            $('#refresh').on('click', refreshDisplay);

                // Added to register for push notifications.
            registerForPushNotifications();

        }, handleError);
    ```

    Bu örnek, kimlik doğrulama başarılı olduktan sonra **registerForPushBildirimlers'i** aramayı gösterir. Gerektiği sıklıkta `registerForPushNotifications()` arayabilirsiniz.

2. Yeni **registerForPushBildirimlers** yöntemini aşağıdaki gibi ekleyin:

    ```javascript
    // Register for push notifications. Requires that phonegap-plugin-push be installed.
    var pushRegistration = null;
    function registerForPushNotifications() {
        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

    // Handle the registration event.
    pushRegistration.on('registration', function (data) {
        // Get the native platform of the device.
        var platform = device.platform;
        // Get the handle returned during registration.
        var handle = data.registrationId;
        // Set the device-specific message template.
        if (platform == 'android' || platform == 'Android') {
            // Register for GCM notifications.
            client.push.register('gcm', handle, {
                mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
            });
        } else if (device.platform === 'iOS') {
            // Register for notifications.
            client.push.register('apns', handle, {
                mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
            });
        } else if (device.platform === 'windows') {
            // Register for WNS notifications.
            client.push.register('wns', handle, {
                myTemplate: {
                    body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                    headers: { 'X-WNS-Type': 'wns/toast' } }
            });
        }
    });

    pushRegistration.on('notification', function (data, d2) {
        alert('Push Received: ' + data.message);
    });

    pushRegistration.on('error', handleError);
    }
    ```
3. (Android) Önceki kodda, Google `Your_Project_ID` Geliştirici Konsolu'ndan uygulamanızın sayısal [Google Developer Console][18]proje kimliğiyle değiştirin.

## <a name="optional-configure-and-run-the-app-on-android"></a>(İsteğe bağlı) Uygulamayı Android'de yapılandırma ve çalıştırma

Android için anında iletme bildirimlerini etkinleştirmek için bu bölümü tamamlayın.

#### <a name="enable-firebase-cloud-messaging"></a><a name="enable-gcm"></a>Firebase Cloud Messaging'i etkinleştirme

Başlangıçta Google Android platformını hedeflediğiniz için Firebase Cloud Messaging'i etkinleştirmeniz gerekir.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-the-mobile-app-back-end-to-send-push-requests-using-fcm"></a><a name="configure-backend"></a>FCM kullanarak anında iletme istekleri göndermek için Mobil Uygulamayı arka uçta yapılandırın

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Android için Cordova uygulamanızı yapılandırın

Cordova uygulamanızda **config.xml'i**açın. Ardından, `Your_Project_ID` Google Geliştirici Konsolu'ndan uygulamanız [Google Developer Console][18]için sayısal proje kimliği ile değiştirin.

```xml
<plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
    <variable name="SENDER_ID" value="Your_Project_ID" />
</plugin>
```

Açık **index.js**. Ardından sayısal proje kimliğinizi kullanmak için kodu güncelleştirin.

```javascript
pushRegistration = PushNotification.init({
    android: { senderID: 'Your_Project_ID' },
    ios: { alert: 'true', badge: 'true', sound: 'true' },
    wns: {}
});
```

#### <a name="configure-your-android-device-for-usb-debugging"></a><a name="configure-device"></a>Usb hata ayıklama için Android cihazınızı yapılandırın

Uygulamanızı Android Cihazınıza dağıtmadan önce USB hata ayıklamayı etkinleştirmeniz gerekir. Android telefonunuzda aşağıdaki adımları izleyin:

1. **Ayarlar** > **Hakkında telefona**gidin. Ardından geliştirici modu etkinleştirilene kadar **Yapı numarasına** dokunun (yaklaşık yedi kez).
2. **Ayarlar** > **Geliştirici Seçenekleri**geri, USB hata **ayıklama**etkinleştirin. Ardından Android telefonunuzu bir USB kablosuyla geliştirme bilgisayarınıza bağlayın.

Bunu Android 6.0 (Marshmallow) çalıştıran bir Google Nexus 5X cihazı nı kullanarak test ettik. Ancak, teknikler herhangi bir modern Android sürümü arasında yaygındır.

#### <a name="install-google-play-services"></a>Google Play Hizmetlerini Yükleyin

Push eklentisi anında iletme bildirimleri için Android Google Play Hizmetleri'ne dayanır.

1. Visual Studio'da **Araçlar** > **Android Android** > **SDK Manager'ı**seçin. Ardından **Ekstralar** klasörünü genişletin. Aşağıdaki SDK'ların her birinin yüklü olduğundan emin olmak için uygun kutuları işaretleyin:

   * Android 2.3 veya üzeri
   * Google Depo revizyonu 27 veya daha yüksek
   * Google Play Hizmetleri 9.0.2 veya üzeri

2. **Paketleri Yükle'yi**seçin. Ardından yüklemenin bitmesini bekleyin.

Geçerli gerekli kitaplıklar [phonegap-plugin-push yükleme belgelerinde][19]listelenir.

#### <a name="test-push-notifications-in-the-app-on-android"></a>Android'de uygulamada anında iletme bildirimlerini test edin

Artık uygulamayı çalıştırarak ve Öğeleri TodoItem tablosuna ekleyerek anında iletme bildirimlerini test edebilirsiniz. Aynı arka ucu kullandığınız sürece aynı aygıttan veya ikinci bir aygıttan test edebilirsiniz. Cordova uygulamanızı Android platformunda aşağıdaki yollardan biriyle test edin:

* *Fiziksel bir cihazda:* Android cihazınızı bir USB kablosuyla geliştirme bilgisayarınıza takın.  Yerine **Google Android Emülatörü**, **Cihaz**seçin. Visual Studio uygulamayı aygıta dağıtır ve uygulamayı çalıştırAr. Daha sonra aygıttaki uygulamayla etkileşimkurabilirsiniz.

  [Mobizen][20] gibi ekran paylaşım uygulamaları Android uygulamaları geliştirmede size yardımcı olabilir. Mobizen, Android ekranınızı bilgisayarınızdaki bir web tarayıcısına göre projeleri.

* *Bir Android emülatör üzerinde:* Emülatör kullanırken gereken ek yapılandırma adımları vardır.

    Android Sanal Cihaz (AVD) yöneticisinde gösterildiği gibi, Hedef olarak Google API'leri ayarlanan sanal bir cihaza dağıtım yaptığınızdan emin olun.

    ![Android Sanal Cihaz Yöneticisi](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Daha hızlı bir x86 emülatörü kullanmak istiyorsanız, [HAXM sürücüsünü yükleyin][11]ve emülatörün kullanımını yapılandırın.

    **Uygulamalar** > **Ayarları** > **Ekle hesabını**seçerek Android cihazına bir Google hesabı ekleyin. Ardından istemleri izleyin.

    ![Android cihazına Google hesabı ekleme](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Todolist uygulamasını eskisi gibi çalıştırın ve yeni bir todo öğesi ekleyin. Bu kez, bildirim alanında bir bildirim simgesi görüntülenir. Bildirimin tam metnini görüntülemek için bildirim çekmecesini açabilirsiniz.

    ![Bildirimi görüntüleme](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(İsteğe bağlı) iOS'ta yapılandırma ve çalıştırma

Bu bölüm, cordova projesini iOS aygıtlarında çalıştırmak içindir. iOS aygıtlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="install-and-run-the-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>iOS uzaktan yapı aracısını Mac veya bulut hizmetine yükleme ve çalıştırma

Visual Studio'yu kullanarak iOS'ta bir Cordova uygulamasını çalıştırmadan önce, uzaktan yapı aracısını yüklemek ve çalıştırmak için [iOS kurulum kılavuzundaki][12] adımları gözden geçirin.

uygulamayı iOS için oluşturabileceğinizden emin olun. Visual Studio'dan iOS için uygulama oluşturmak için kurulum kılavuzundaki adımlar gereklidir. Mac'iniz yoksa, MacInCloud gibi bir hizmette uzaktan yapı aracısını kullanarak iOS için oluşturabilirsiniz. Daha fazla bilgi için [bkz.][21]

> [!NOTE]
> iOS'taki push eklentisini kullanmak için Xcode 7 veya daha büyük olması gerekir.

#### <a name="find-the-id-to-use-as-your-app-id"></a>Uygulama Kimliğiniz olarak kullanılacak kimliği bulun

Uygulamanızı anında iletme bildirimleri için kaydetmeden önce, Cordova uygulamanızda `id` config.xml'i açın, widget öğesindeki öznitelik değerini bulun ve daha sonra kullanmak üzere kopyalayın. Aşağıdaki XML'de, kimlik `io.cordova.myapp7777777`.

```xml
<widget defaultlocale="en-US" id="io.cordova.myapp7777777"
    version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="https://www.w3.org/ns/widgets"
    xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">
```

Daha sonra, Apple'ın geliştirici portalında bir Uygulama Kimliği oluştururken bu tanımlayıcıyı kullanın. Geliştirici portalında farklı bir Uygulama Kimliği oluşturursanız, bu eğitimde daha sonra birkaç ek adım atmanız gerekir. Widget öğesindeki kimliğin geliştirici portalındaki Uygulama Kimliği ile eşleşmesi gerekir.

#### <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Uygulamayı Apple'ın geliştirici portalına anında iletme bildirimleri için kaydedin

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Benzer adımları gösteren bir video izleyin](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için Azure'u yapılandırma

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a>Uygulama Kimliğinizin Cordova uygulamanızla eşleştiğini doğrulayın

Apple Geliştirici Hesabınızda oluşturduğunuz Uygulama Kimliği, config.xml dosyasındaki widget öğesinin kimliğiyle zaten eşleşiyorsa, bu adımı atlayabilirsiniz. Ancak, adlar eşleşmiyorsa, aşağıdaki adımları izleyin:

1. Projenizdeki platformlar klasörünü silin.
2. Projenizdeki eklentiler klasörünü silin.
3. Projenizdeki node_modules klasörünü silin.
4. Apple geliştirici hesabınızda oluşturduğunuz uygulama kimliğini kullanmak için config.xml dosyasındaki widget öğesinin kimlik özniteliğini güncelleştirin.
5. Projenizi yeniden oluşturun.

##### <a name="test-push-notifications-in-your-ios-app"></a>iOS uygulamanızda anında iletme bildirimlerini test edin

1. Visual Studio'da, **iOS'un** dağıtım hedefi olarak seçildiğinden emin olun. Ardından bağlı iOS aygıtınızdaki anında iletme bildirimlerini çalıştırmak için **Aygıt'ı** seçin.

    Anında iletme bildirimlerini iTunes ile bilgisayarınıza bağlı bir iOS aygıtında çalıştırabilirsiniz. iOS Simulator anında iletme bildirimlerini desteklemez.

2. Projeyi oluşturmak ve uygulamayı bir iOS aygıtında başlatmak için Visual Studio'da **Çalıştır** düğmesini veya **F5'i** seçin. Ardından anında **iletme** bildirimlerini kabul etmek için Tamam'ı seçin.

   > [!NOTE]
   > Uygulama ilk çalıştırma sırasında anında iletme bildirimleri için onay ister.

3. Uygulamada, bir görev yazın ve sonra artı **(+)** simgesini seçin.
4. Bildirimin alındığını doğrulayın. Ardından bildirimi kapatmak için **Tamam'ı** seçin.

## <a name="optional-configure-and-run-on-windows"></a>(İsteğe bağlı) Windows'da yapılandırma ve çalıştırma

Bu bölümde, Windows 10 cihazlarda Apache Cordova uygulama projesinin nasıl çalıştırılabildiği açıklanmaktadır (PhoneGap push eklentisi Windows 10'da desteklenir). Windows aygıtlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>Windows uygulamanızı anında iletme bildirimleri için WNS ile kaydedin

Visual Studio'da Mağaza seçeneklerini kullanmak için, **Windows-x64** veya **Windows-x86**gibi Çözüm Platformları listesinden bir Windows hedefi seçin. (Anında iletme bildirimleri için **Windows-AnyCPU'dan** kaçının.)

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Benzer adımları gösteren bir video izleyin][13]

#### <a name="configure-the-notification-hub-for-wns"></a>WNS için bildirim merkezini yapılandırma

[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Windows anında iletme bildirimlerini destekleyecek şekilde Cordova uygulamanızı yapılandırın

**Config.xml'e**sağ tıklayarak yapılandırma tasarımcısını açın. Ardından **Tasarımcıyı Görüntüle'yi**seçin. Ardından, **Windows** sekmesini seçin ve ardından **Windows Hedef Sürümü'nün**altında **Windows 10'u** seçin.

Varsayılan (hata ayıklama) yapınızda anında iletme bildirimlerini desteklemek için **build.json** dosyasını açın. Ardından "sürüm" yapılandırmasını hata ayıklama yapılandırmanıza kopyalayın.

```json
"windows": {
    "release": {
        "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
        "publisherId": "CN=yourpublisherID"
    }
}
```

Güncelleştirmeden sonra **build.json** dosyası aşağıdaki kodu içermelidir:

```json
"windows": {
    "release": {
        "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
        "publisherId": "CN=yourpublisherID"
        },
    "debug": {
        "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
        "publisherId": "CN=yourpublisherID"
        }
    }
```

Uygulamayı oluşturun ve hiçbir hatanız olmadığını doğrulayın. Müşteri uygulamanız artık Mobil Uygulamalar'dan gelen bildirimler için kayıt olmalıdır. Çözümünüzdeki her Windows projesi için bu bölümü tekrarlayın.

#### <a name="test-push-notifications-in-your-windows-app"></a>Windows uygulamanızda anında iletme bildirimlerini test etme

Visual Studio'da, **Windows-x64** veya **Windows-x86**gibi dağıtım hedefi olarak bir Windows platformunun seçildiğinden emin olun. Uygulamayı Visual Studio'ya ev sahipliği yapan bir Windows 10 pc'de çalıştırmak için **Yerel Makine'yi**seçin.

1. Projeyi oluşturmak ve uygulamayı başlatmak için **Çalıştır** düğmesini seçin.

2. Uygulamada, yeni bir todoöğe için bir ad yazın ve eklemek için artı **(+)** simgesini seçin.

Öğe eklendiğinde bildirimin alındığını doğrulayın.

## <a name="next-steps"></a><a name="next-steps"></a>Sonraki adımlar

* Anında iletme bildirimleri hakkında bilgi edinmek için [Bildirim Hub'ları][17] hakkında bilgi edinin.
* Bunu zaten yapmadıysanız, Apache Cordova uygulamanıza [kimlik doğrulama ekleyerek][14] öğreticiye devam edin.

Aşağıdaki SDK'ları nasıl kullanacağınızı öğrenin:

* [Apache Cordova SDK][15]
* [ASP.NET Sunucusu SDK][1]
* [Node.js Sunucusu SDK][16]

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: https://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: https://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: https://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: https://taco.visualstudio.com/en-us/docs/build_ios_cloud/
