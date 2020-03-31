---
title: Xamarin.Forms uygulamanıza anında iletme bildirimleri ekleme
description: Xamarin.Forms uygulamalarınız için çok platformlu anında iletme bildirimleri göndermek için Azure hizmetlerini nasıl kullanacağınızı öğrenin.
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: f23ac2d693492695c398893c103d5a77a0e93129
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461479"
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Xamarin.Forms uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, [Xamarin.Forms hızlı başlangıç](app-service-mobile-xamarin-forms-get-started.md)sonucu tüm projelere anında iletme bildirimleri ekleyin. Bu, bir kayıt her ekildiğinde tüm platformlar arası istemcilere bir anında iletme bildirimi gönderildiği anlamına gelir.

İndirilen hızlı başlangıç sunucusu projesini kullanmazsanız, anında iletme bildirimi uzantısı paketine ihtiyacınız olacaktır. Daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)'ya bakın.

## <a name="prerequisites"></a>Ön koşullar

iOS için bir [Apple Geliştirici Programı üyeliğine](https://developer.apple.com/programs/ios/) ve fiziksel bir iOS aygıtına ihtiyacınız olacaktır. [iOS simülatörü anında iletme bildirimlerini desteklemiyor.](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html)

## <a name="configure-a-notification-hub"></a><a name="configure-hub"></a>Bildirim hub’ını yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için sunucu projesini güncelleştirme

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a>Android projesini yapılandırma ve çalıştırma (isteğe bağlı)

Android için Xamarin.Forms Droid projesi için anında iletme bildirimlerini etkinleştirmek için bu bölümü tamamlayın.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Firebase Bulut Mesajlaşmasını Etkinleştir (FCM)

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a>FCM kullanarak anında iletme istekleri göndermek için Mobil Uygulamaları arka uçta yapılandırın

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a>Android projesine anında iletme bildirimleri ekleme

FCM ile yapılandırılan arka uçla, FCM'ye kaydolmak için istemciye bileşenler ve kodlar ekleyebilirsiniz. Ayrıca Mobil Uygulamalar arka uç üzerinden Azure Bildirim Hub'ları ile anında iletme bildirimleri için kayıt ve bildirimler ilerler.

1. **Droid** projesinde, **NuGet Paketlerini Yönet> Doğru**Tıklama Referanslar ... .
1. NuGet Package Manager penceresinde **Xamarin.Firebase.Messaging** paketini arayın ve projeye ekleyin.
1. **Droid** projesinin proje özelliklerinde, uygulamayı Android sürüm 7.0 veya daha yüksek kullanarak derlemek üzere ayarlayın.
1. Firebase konsolundan indirilen **google-services.json** dosyasını **Droid** projesinin köküne ekleyin ve **googleServicesJson'a**yapı eylemini ayarlayın. Daha fazla bilgi için bkz: [Google Hizmetleri JSON Dosyasını Ekle.](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/#Add_the_Google_Services_JSON_File)

#### <a name="registering-with-firebase-cloud-messaging"></a>Firebase Cloud Messaging ile kaydolma

1. **AndroidManifest.xml** dosyasını açın ve aşağıdaki `<receiver>` öğelerini `<application>` öğesine ekleyin:

    ```xml
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
        </intent-filter>
    </receiver>
    ```

#### <a name="implementing-the-firebase-instance-id-service"></a>Firebase Instance ID Hizmetinin Uygulanması

1. **Droid** projesine adlı `FirebaseRegistrationService`yeni bir sınıf ekleyin ve `using` aşağıdaki ifadelerin dosyanın üst kısmında bulunduğundan emin olun:

    ```csharp
    using System.Threading.Tasks;
    using Android.App;
    using Android.Util;
    using Firebase.Iid;
    using Microsoft.WindowsAzure.MobileServices;
    ```

1. Boş `FirebaseRegistrationService` sınıfı aşağıdaki kodla değiştirin:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class FirebaseRegistrationService : FirebaseInstanceIdService
    {
        const string TAG = "FirebaseRegistrationService";

        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationTokenToAzureNotificationHub(refreshedToken);
        }

        void SendRegistrationTokenToAzureNotificationHub(string token)
        {
            // Update notification hub registration
            Task.Run(async () =>
            {
                await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
            });
        }
    }
    ```

    Sınıf, `FirebaseRegistrationService` fcm'e erişim için uygulama yetkisi veren güvenlik belirteçleri oluşturmakla yükümlüdür. Yöntem, `OnTokenRefresh` uygulama FCM'den bir kayıt jetonu aldığında çağrılır. Yöntem, FCM tarafından eşzamanlı `FirebaseInstanceId.Instance.Token` olarak güncelleştirilen özellikten belirteci alır. Belirteç `OnTokenRefresh` yalnızca uygulama yüklendiğinde veya kaldırıldığında, kullanıcı uygulama verilerini sildiğinde, uygulama Örnek Kimliğini sildiğinde veya belirteç güvenliği tehlikeye girdiğinden, yöntem sık sık çağrılır. Buna ek olarak, FCM Instance ID hizmeti, uygulamanın belirtecidüzenli olarak her 6 ayda bir düzenli olarak yenilenmesini talep eder.

    Yöntem, `OnTokenRefresh` kullanıcının `SendRegistrationTokenToAzureNotificationHub` kayıt belirteciyle Azure Bildirim Hub'ı ile ilişkilendirmek için kullanılan yöntemi de çağırır.

#### <a name="registering-with-the-azure-notification-hub"></a>Azure Bildirim Merkezi'ne kaydolmak

1. **Droid** projesine adlı `AzureNotificationHubService`yeni bir sınıf ekleyin ve `using` aşağıdaki ifadelerin dosyanın üst kısmında bulunduğundan emin olun:

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Android.Util;
    using Microsoft.WindowsAzure.MobileServices;
    using Newtonsoft.Json.Linq;
    ```

1. Boş `AzureNotificationHubService` sınıfı aşağıdaki kodla değiştirin:

    ```csharp
    public class AzureNotificationHubService
    {
        const string TAG = "AzureNotificationHubService";

        public static async Task RegisterAsync(Push push, string token)
        {
            try
            {
                const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBody}
                };

                await push.RegisterAsync(token, templates);
                Log.Info("Push Installation Id: ", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
            }
        }
    }
    ```

    Yöntem, `RegisterAsync` JSON olarak basit bir bildirim iletisi şablonu oluşturur ve Firebase kayıt belirteci kullanarak bildirim merkezinden şablon bildirimleri almak için kaydolur. Bu, Azure Bildirim Hub'ından gönderilen bildirimlerin kayıt belirteci tarafından temsil edilen aygıtı hedeflemesini sağlar.

#### <a name="displaying-the-contents-of-a-push-notification"></a>Anında İtme Bildiriminin İçeriğini Görüntüleme

1. **Droid** projesine adlı `FirebaseNotificationService`yeni bir sınıf ekleyin ve `using` aşağıdaki ifadelerin dosyanın üst kısmında bulunduğundan emin olun:

    ```csharp
    using Android.App;
    using Android.Content;
    using Android.Media;
    using Android.Support.V7.App;
    using Android.Util;
    using Firebase.Messaging;
    ```

1. Boş `FirebaseNotificationService` sınıfı aşağıdaki kodla değiştirin:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class FirebaseNotificationService : FirebaseMessagingService
    {
        const string TAG = "FirebaseNotificationService";

        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);

            // Pull message body out of the template
            var messageBody = message.Data["message"];
            if (string.IsNullOrWhiteSpace(messageBody))
                return;

            Log.Debug(TAG, "Notification message body: " + messageBody);
            SendNotification(messageBody);
        }

        void SendNotification(string messageBody)
        {
            var intent = new Intent(this, typeof(MainActivity));
            intent.AddFlags(ActivityFlags.ClearTop);
            //Unique request code to avoid PendingIntent collision.
            var requestCode = new Random().Next();
            var pendingIntent = PendingIntent.GetActivity(this, requestCode, intent, PendingIntentFlags.OneShot);
            var notificationBuilder = new NotificationCompat.Builder(this)
                .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle("New Todo Item")
                .SetContentText(messageBody)
                .SetContentIntent(pendingIntent)
                .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
                .SetAutoCancel(true);

            var notificationManager = NotificationManager.FromContext(this);
            notificationManager.Notify(0, notificationBuilder.Build());
        }
    }
    ```

    Bir `OnMessageReceived` uygulama FCM'den bir bildirim aldığında çağrılan yöntem, ileti içeriğini ayıklar `SendNotification` ve yöntemi çağırır. Bu yöntem, bildirimin bildirim alanında görünmesiyle ileti içeriğini uygulama çalışırken başlatılan yerel bir bildirime dönüştürür.

Şimdi, bir Android cihaz veya emülatör üzerinde çalışan uygulamada test push bildirimleri hazırsınız.

### <a name="test-push-notifications-in-your-android-app"></a>Android uygulamanızda anında iletme bildirimlerini test edin

İlk iki adım yalnızca bir emülatör üzerinde test yaparken gereklidir.

1. Google Play Hizmetleri ile yapılandırılan bir aygıta veya emülatöre dağıtıldığınızı veya hata ayıklama yaptığından emin olun. Bu, **Yürüt** uygulamalarının aygıta veya emülatöre yüklü olup olmadığını kontrol ederek doğrulanabilir.
2. **Uygulamalar** > **Ayarları** > **Hesap Ekle'ye**tıklayarak Android cihazına bir Google hesabı ekleyin. Ardından, aygıta varolan bir Google hesabı eklemek veya yeni bir hesap oluşturmak için istemleri izleyin.
3. Visual Studio veya Xamarin Studio'da **Droid** projesine sağ tıklayın ve **başlangıç projesi olarak ayarla'yı**tıklatın.
4. Projeyi oluşturmak ve uygulamayı Android cihazınızda veya emülatörünüzü başlatmak için **Çalıştır'ı** tıklatın.
5. Uygulamada, bir görev yazın ve sonra artı**+**( ) simgesini tıklatın.
6. Bir öğe eklendiğinde bildirimin alındığını doğrulayın.

## <a name="configure-and-run-the-ios-project-optional"></a>iOS projesini yapılandırma ve çalıştırma (isteğe bağlı)

Bu bölüm iOS cihazları için Xamarin iOS projesi çalıştırmaya yöneliktir. iOS cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

[!INCLUDE [Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a>APNS için bildirim merkezini yapılandırma

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Ardından, Xamarin Studio veya Visual Studio'daki iOS proje ayarını yapılandıracak.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a>iOS uygulamanıza anında iletme bildirimleri ekleme

1. **iOS** projesinde, AppDelegate.cs açın ve kod dosyasının üst bölümüne aşağıdaki deyimi ekleyin.

    ```csharp
    using Newtonsoft.Json.Linq;
    ```

2. **AppDelegate** sınıfında, bildirimler için kaydolmak için **RegisteredForRemoteBildirimler** olayı için bir geçersiz kılma ekleyin:

    ```csharp
    public override void RegisteredForRemoteNotifications(UIApplication application,
        NSData deviceToken)
    {
        const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

        JObject templates = new JObject();
        templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

        // Register for push with your mobile app
        Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
        push.RegisterAsync(deviceToken, templates);
    }
    ```

3. **AppDelegate'de** **DidReceiveRemoteNotification** olay işleyicisi için aşağıdaki geçersiz kılmayı da ekleyin:

    ```csharp
    public override void DidReceiveRemoteNotification(UIApplication application,
        NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
    {
        NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

        string alert = string.Empty;
        if (aps.ContainsKey(new NSString("alert")))
            alert = (aps[new NSString("alert")] as NSString).ToString();

        //show alert
        if (!string.IsNullOrEmpty(alert))
        {
            UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
            avAlert.Show();
        }
    }
    ```

    Bu yöntem, uygulama çalışırken gelen bildirimleri işler.

4. **AppDelegate** sınıfında, **FinishedLaunching** yöntemine aşağıdaki kodu ekleyin:

    ```csharp
    // Register for push notifications.
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert
        | UIUserNotificationType.Badge
        | UIUserNotificationType.Sound,
        new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ```

    Bu, uzak bildirimler ve push kaydı istekleri için destek sağlar.

Uygulamanız artık anında iletme bildirimlerini destekleyecek şekilde güncelleştirildi.

#### <a name="test-push-notifications-in-your-ios-app"></a>iOS uygulamanızda anında iletme bildirimlerini test edin

1. iOS projesini sağ tıklatın ve **Başlangıç Projesi olarak ayarla'yı**tıklatın.
2. Projeyi oluşturmak ve uygulamayı bir iOS aygıtında başlatmak için Visual Studio'da **Çalıştır** düğmesine veya **F5'e** basın. Ardından anında **iletme** bildirimlerini kabul etmek için Tamam'ı tıklatın.

   > [!NOTE]
   > Uygulamanızdan gelen anında iletme bildirimlerini açıkça kabul etmelisiniz. Bu istek yalnızca uygulamailk kez çalıştırılır.

3. Uygulamada, bir görev yazın ve sonra artı**+**( ) simgesini tıklatın.
4. Bildirimin alındığını doğrulayın ve bildirimi kapatmak için **Tamam'ı** tıklatın.

## <a name="configure-and-run-windows-projects-optional"></a>Windows projelerini yapılandırma ve çalıştırma (isteğe bağlı)

Bu bölüm Windows cihazları için Xamarin.Forms WinApp ve WinPhone81 projelerini çalıştırmak içindir. Bu adımlar, Evrensel Windows Platformu (UWP) projelerini de destekler. Windows cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Anında iletme bildirimleri için Windows uygulamanızı Windows Bildirim Hizmeti (WNS) ile kaydedin

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a>WNS için bildirim merkezini yapılandırma

[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a>Windows uygulamanıza anında iletme bildirimleri ekleme

1. Visual Studio'da, bir Windows projesinde **App.xaml.cs** açın ve aşağıdaki ifadeleri ekleyin.

    ```csharp
    using Newtonsoft.Json.Linq;
    using Microsoft.WindowsAzure.MobileServices;
    using System.Threading.Tasks;
    using Windows.Networking.PushNotifications;
    using <your_TodoItemManager_portable_class_namespace>;
    ```

    Sınıfı `<your_TodoItemManager_portable_class_namespace>` içeren taşınabilir projenizin ad `TodoItemManager` alanıyla değiştirin.

2. App.xaml.cs olarak, aşağıdaki **InitNotificationsAsync** yöntemini ekleyin:

    ```csharp
    private async Task InitNotificationsAsync()
    {
        var channel = await PushNotificationChannelManager
            .CreatePushNotificationChannelForApplicationAsync();

        const string templateBodyWNS =
            "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

        JObject headers = new JObject();
        headers["X-WNS-Type"] = "wns/toast";

        JObject templates = new JObject();
        templates["genericMessage"] = new JObject
        {
            {"body", templateBodyWNS},
            {"headers", headers} // Needed for WNS.
        };

        await TodoItemManager.DefaultManager.CurrentClient.GetPush()
            .RegisterAsync(channel.Uri, templates);
    }
    ```

    Bu yöntem anında iletme bildirimi kanalını alır ve bildirim merkezinizden şablon bildirimleri almak için bir şablon kaydeder. İletiyi destekleyen bir şablon *bildirimiParam* bu istemciye teslim edilecektir.

3. App.xaml.cs, değiştirici ekleyerek **Başlatılan Olay** Işleyicisi `async` yöntem tanımını güncelleştirin. Ardından yöntemin sonuna aşağıdaki kod satırını ekleyin:

    ```csharp
    await InitNotificationsAsync();
    ```

    Bu, uygulama her başlatılında anında iletme bildirimi kaydının oluşturulmasını veya yenilenmesini sağlar. WNS push kanalının her zaman etkin olmasını garanti etmek için bunu yapmak önemlidir.  

4. Visual Studio için Solution Explorer'da **Package.appxmanifest** dosyasını açın ve **Bildirimler**altında **Tost'u** **Evet** olarak ayarlayın.
5. Uygulamayı oluşturun ve hiçbir hatanız olmadığını doğrulayın. Müşteri uygulamanız artık Mobil Uygulamalar arka uçtan gelen şablon bildirimleri için kaydolmalıdır. Çözümünüzdeki her Windows projesi için bu bölümü tekrarlayın.

#### <a name="test-push-notifications-in-your-windows-app"></a>Windows uygulamanızda anında iletme bildirimlerini test etme

1. Visual Studio'da bir Windows projesine sağ tıklayın ve **başlangıç projesi olarak ayarla'yı**tıklatın.
2. Projeyi oluşturmak ve uygulamayı başlatmak için **Çalıştır** düğmesine basın.
3. Uygulamada, yeni bir todoöğe için bir ad yazın ve**+** eklemek için artı ( ) simgesine tıklayın.
4. Öğe eklendiğinde bildirimin alındığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Anında iletme bildirimleri hakkında daha fazla bilgi edinebilirsiniz:

* [Azure Mobil Uygulamalarından Anında Ödeme Gönderme](https://developer.xamarin.com/guides/xamarin-forms/cloud-services/push-notifications/azure/)
* [Firebase Cloud Messaging](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/firebase-cloud-messaging/)
* [Firebase Bulut Mesajlaşma ile Uzaktan Bildirimler](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/)
* [Anında iletme bildirimi sorunlarını tanılama](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Bildirimlerin düşmesinin veya aygıtlara düşmemelerinin çeşitli nedenleri vardır. Bu konu, anında iletme bildirimi hatalarının temel nedenini nasıl çözümleyip çözersiniz gösterir.

Ayrıca aşağıdaki öğreticilerden birine devam edebilirsiniz:

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-xamarin-forms-get-started-users.md)  
  Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme yle, kullanıcılar ağ&mdash;bağlantısı olmasa bile bir&mdash;mobil uygulamayı görüntüleme, ekleme veya değiştirme ile etkileşimkurabilir.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: https://go.microsoft.com/fwlink/p/?LinkId=272333
