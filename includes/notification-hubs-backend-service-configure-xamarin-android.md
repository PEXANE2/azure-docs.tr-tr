---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: c919cfce3d868a81f28eb8172314e9639387e933
ms.sourcegitcommit: e04a66514b21019f117a4ddb23f22c7c016da126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85111987"
---
### <a name="validate-package-name-and-permissions"></a>Paket adı ve izinlerini doğrula

1. **Pushdemo. Android**' de, **derleme** bölümündeki **Proje seçenekleri** sonra **Android uygulaması** ' nı açın.

1. **Paket adının** [Firebase konsol](https://console.firebase.google.com) **pushdemo** projesinde kullandığınız değerle eşleşip eşleşmediğini denetleyin. **Paket adı** biçimiydi ``com.<organization>.pushdemo`` .

1. **En düşük Android sürümünü** **ANDROID 8,0 (API düzeyi 26)** ve **hedef Android sürümünü** en son **API düzeyine**ayarlayın.

    > [!NOTE]
    > Bu öğreticinin amaçları doğrultusunda yalnızca **API düzeyi 26 ve üzeri** çalıştıran cihazlar desteklenir, ancak daha eski sürümleri çalıştıran cihazları desteklemek için genişletebilirsiniz.

1. **Gerekli izinler**altında **Internet** ve **READ_PHONE_STATE** izinlerinin etkinleştirildiğinden emin olun.

1. **Tamam 'a** tıklayın

### <a name="add-the-xamarin-google-play-services-base-and-xamarinfirebasemessaging-packages"></a>Xamarin Google Play Hizmetleri Base ve Xamarin. Firebase. Messaging paketlerini ekleyin

1. **Pushdemo. Android**dosyasında, **Denetim**  +  **paketleri** klasörüne**tıklayın** ve ardından **NuGet Paketlerini Yönet...** seçeneğini belirleyin.

1. **Xamarin. GooglePlayServices. Base** ( **temel değil)** araması yapın ve bunun işaretli olduğundan emin olun.

1. **Xamarin. Firebase. Messaging** için arama yapın ve bunun işaretli olduğundan emin olun.

1. **Paket Ekle**' ye tıklayın ve **Lisans koşullarını**kabul etmek Isteyip istemediğiniz sorulduğunda **kabul et** ' e tıklayın.

### <a name="add-the-google-services-json-file"></a>Google Services JSON dosyasını ekleme

1. **Denetim**  +  Projeye **tıklayın** `PushDemo.Android` , ardından **Ekle** menüsünde **mevcut dosya...** öğesini seçin.

1. [Firebase konsolunda](https://console.firebase.google.com) **pushdemo** projesini ayarlarken daha önce indirdiğiniz dosyada *google-services.js* seçin ve ardından **Aç**' a tıklayın.

1. İstendiğinde, **dosyayı dizine kopyalamayı**seçin.

1. **Denetim**  +  Projenin içinden dosya *google-services.js* **' ne tıklayın** `PushDemo.Android` , sonra **GoogleServicesJson** **Yapı eylemi**olarak ayarlandığından emin olun.

### <a name="handle-push-notifications-for-android"></a>Android için anında iletme bildirimlerini işleme

1. **Denetim**  +  **Click** `PushDemo.Android` Projeye tıklayın, **Ekle** menüsünde **Yeni klasör** ' i seçin ve ardından **klasör adı**olarak *Hizmetleri* kullanarak **Ekle** ' ye tıklayın.

1. **Denetim**  +  **Hizmetler** klasörüne **tıklayın** ve ardından **Ekle** menüsünde **yeni dosya..** . öğesini seçin.

1. **Genel**  >  **boş sınıf**' ı seçin, **ad**için *DeviceInstallationService.cs* girin ve ardından aşağıdaki uygulamayı eklemek için **Yeni** ' ye tıklayın.

    ```csharp
    using System;
    using Android.App;
    using Android.Gms.Common;
    using PushDemo.Models;
    using PushDemo.Services;
    using static Android.Provider.Settings;

    namespace PushDemo.Droid.Services
    {
        public class DeviceInstallationService : IDeviceInstallationService
        {
            public string Token { get; set; }

            public bool NotificationsSupported
                => GoogleApiAvailability.Instance
                    .IsGooglePlayServicesAvailable(Application.Context) == ConnectionResult.Success;

            public string GetDeviceId()
                => Secure.GetString(Application.Context.ContentResolver, Secure.AndroidId);

            public DeviceInstallation GetDeviceInstallation(params string[] tags)
            {
                if (!NotificationsSupported)
                    throw new Exception(GetPlayServicesError());

                var installation = new DeviceInstallation
                {
                    InstallationId = GetDeviceId(),
                    Platform = "fcm",
                    PushChannel = Token
                };

                installation.Tags.AddRange(tags);

                return installation;
            }

            string GetPlayServicesError()
            {
                int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(Application.Context);

                if (resultCode != ConnectionResult.Success)
                    return GoogleApiAvailability.Instance.IsUserResolvableError(resultCode) ?
                               GoogleApiAvailability.Instance.GetErrorString(resultCode) :
                               "This device is not supported";

                return "An error occurred preventing the use of push notifications";
            }
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf, Bildirim Hub 'ı kayıt yükünün bir parçası olarak benzersiz bir KIMLIK ( [Secure. AndroidId](https://docs.microsoft.com/dotnet/api/android.provider.settings.secure.androidid?view=xamarin-android-sdk-9)kullanarak) sağlar.

1. *PushNotificationFirebaseMessagingService.cs*adlı **Hizmetler** klasörüne başka bir **boş sınıf** ekleyin ve ardından aşağıdaki uygulamayı ekleyin.

    ```csharp
    using Android.App;
    using Android.Content;
    using Firebase.Messaging;
    using PushDemo.Services;

    namespace PushDemo.Droid.Services
    {
        [Service]
        [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
        public class PushNotificationFirebaseMessagingService : FirebaseMessagingService
        {
            IPushDemoNotificationActionService _notificationActionService;
            INotificationRegistrationService _notificationRegistrationService;
            IDeviceInstallationService _deviceInstallationService;

            IPushDemoNotificationActionService NotificationActionService
                => _notificationActionService ??
                    (_notificationActionService =
                    ServiceContainer.Resolve<IPushDemoNotificationActionService>());

            INotificationRegistrationService NotificationRegistrationService
                => _notificationRegistrationService ??
                    (_notificationRegistrationService =
                    ServiceContainer.Resolve<INotificationRegistrationService>());

            IDeviceInstallationService DeviceInstallationService
                => _deviceInstallationService ??
                    (_deviceInstallationService =
                    ServiceContainer.Resolve<IDeviceInstallationService>());

            public override void OnNewToken(string token)
            {
                DeviceInstallationService.Token = token;

                NotificationRegistrationService.RefreshRegistrationAsync()
                    .ContinueWith((task) => { if (task.IsFaulted) throw task.Exception; });
            }

            public override void OnMessageReceived(RemoteMessage message)
            {
                if(message.Data.TryGetValue("action", out var messageAction))
                    NotificationActionService.TriggerAction(messageAction);
            }
        }
    }
    ```

1. **MainActivity.cs**' de, aşağıdaki ad alanlarının dosyanın en üstüne eklendiğinden emin olun.

    ```csharp
    using System;
    using Android.App;
    using Android.Content;
    using Android.Content.PM;
    using Android.OS;
    using Android.Runtime;
    using Firebase.Iid;
    using PushDemo.Droid.Services;
    using PushDemo.Services;
    ```

1. **MainActivity.cs**' de, **Launchmode** 'u **singletop** olarak ayarlayın. bu nedenle **MainActivity** açıldığında yeniden oluşturulmaz.

    ```csharp
    [Activity(
        Label = "PushDemo",
        LaunchMode = LaunchMode.SingleTop,
        Icon = "@mipmap/icon",
        Theme = "@style/MainTheme",
        MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    ```

1. **Ipushnotificationactionservice** ve **ıdeviceınstalservıce** uygulamalarına bir başvuru depolamak için özel özellikler ve karşılık gelen yedekleme alanları ekleyin.

    ```csharp
    IPushDemoNotificationActionService _notificationActionService;
    IDeviceInstallationService _deviceInstallationService;

    IPushDemoNotificationActionService NotificationActionService
        => _notificationActionService ??
            (_notificationActionService =
            ServiceContainer.Resolve<IPushDemoNotificationActionService>());

    IDeviceInstallationService DeviceInstallationService
        => _deviceInstallationService ??
            (_deviceInstallationService =
            ServiceContainer.Resolve<IDeviceInstallationService>());
    ```

1. **Firebase** belirtecini almak ve depolamak Için **ıonbaşarılı dinleyicisi** arabirimini uygulayın.

    ```csharp
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity, Android.Gms.Tasks.IOnSuccessListener
    {
        ...

        public void OnSuccess(Java.Lang.Object result)
            => DeviceInstallationService.Token =
                result.Class.GetMethod("getToken").Invoke(result).ToString();
    }
    ```

1. **Belirli bir** amaca *göre adlandırılmış bir*ek değer olup olmadığını kontrol edecek **processnotificationactions** adlı yeni bir yöntem ekleyin. Bu eylemi, **ıpushdemonocertificate Ationactionservice** uygulamasını kullanarak koşullu olarak tetikleyin.

    ```csharp
    void ProcessNotificationActions(Intent intent)
    {
        try
        {
            if (intent?.HasExtra("action") == true)
            {
                var action = intent.GetStringExtra("action");

                if (!string.IsNullOrEmpty(action))
                    NotificationActionService.TriggerAction(action);
            }
        }
        catch (Exception ex)
        {
            System.Diagnostics.Debug.WriteLine(ex.Message);
        }
    }
    ```

1. **Checkentfornotificationactions** metodunu çağırmak Için **Onnewınkatlanmış** yöntemini geçersiz kılın.

    ```csharp
    protected override void OnNewIntent(Intent intent)
    {
        base.OnNewIntent(intent);
        ProcessNotificationActions(intent);
    }
    ```

    > [!NOTE]
    > **Etkinliğin** **Launchmode** 'u **singletop**olarak ayarlandığı Için, bir **Amaç** , **OnCreate** yöntemi yerine **onnewıntes** yöntemi aracılığıyla var olan **etkinlik** örneğine gönderilir ve bu nedenle hem **OnCreate** hem de **onnewıntes** yöntemlerinde gelen bir amacı işlemeniz gerekir.

1. **OnCreate** `Bootstrap.Begin` `base.OnCreate` **Ideviceınstalınstall hizmeti**'nin platforma özgü uygulamasını geçirme çağrısından sonra doğrudan çağrı yapmak için OnCreate yöntemini güncelleştirin.

    ```csharp
    Bootstrap.Begin(() => new DeviceInstallationService());
    ```

1. Aynı yöntemde, **Ateınstanceıd** 'Yi **firebaseapp** örneğinde koşullu olarak çağırın, çağrısından hemen sonra `Bootstrap.Begin` **ıonbaşarıyla dinleyicisi**olarak **MainActivity** ekleyin.

    ```csharp
    if (DeviceInstallationService.NotificationsSupported)
    {
        FirebaseInstanceId.GetInstance(Firebase.FirebaseApp.Instance)
            .GetInstanceId()
            .AddOnSuccessListener(this);
    }
    ```

1. Hala **OnCreate**içinde, geçerli amaca geçirme çağrısından hemen sonra **processnotificationactions** öğesini çağırın `LoadApplication` . **Intent**

    ```cs
    ...

    LoadApplication(new App());

    ProcessNotificationActions(Intent);
    ```

> [!NOTE]
> Uygulamayı her çalıştırdığınız zaman yeniden kaydetmeniz ve anında iletme bildirimleri almaya devam etmek için bir hata ayıklama oturumundan durdurmanız gerekir.
