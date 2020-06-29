---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 1dc491084f65bc90397b0897de6b6cfe4f2fd410
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448757"
---
### <a name="configure-infoplist-and-entitlementsplist"></a>Info. plist ve yetkilendirmeler. plist yapılandırma

1. **Visual Studio**tercihlerinde **Apple geliştirici hesabınızda** oturum açtığınızdan emin olun  >  **...**  >  **Yayımlama**  >  **Apple geliştirici hesapları** ve uygun *sertifika* ve *sağlama profili* indirildi. Bu varlıkları önceki adımların bir parçası olarak oluşturmuş olmanız gerekir.

1. **Pushdemo. iOS**' de, **Info. plist** dosyasını açın ve **paketleme Liıdentifier** 'ın [Apple geliştirici portalındaki](https://developer.apple.com)ilgili sağlama profili için kullanılan değerle eşleştiğinden emin olun. **Paketlemeli bir tanımlayıcı** biçimiydi ``com.<organization>.PushDemo`` .

1. Aynı dosyada, **En düşük sistem sürümünü** **13,0**olarak ayarlayın.

    > [!NOTE]
    > Bu öğreticinin amaçları doğrultusunda yalnızca **iOS 13,0 ve üzeri** çalıştıran cihazlar desteklenir, ancak daha eski sürümleri çalıştıran cihazları desteklemek için genişletebilirsiniz.

1. **Pushdemo. iOS** Için **proje seçeneklerini** açın (projeye çift tıklayın).

1. **Proje seçenekleri**' nde, **> iOS paket Imzalama**altında, geliştirici hesabınızın **ekip**altında seçildiğinden emin olun. Sonra, "imzalamayı otomatik olarak Yönet" ' in seçildiğinden ve Imza sertifikanız ve sağlama profilinizin otomatik olarak seçildiğinden emin olun.

    > [!NOTE]
    > *Imzalama sertifikanız* ve *sağlama profiliniz* otomatik olarak seçilmemişse, **el ile sağlama**' yı seçin ve ardından **paket imzalama seçenekleri**' ne tıklayın. *Ekibinizin* **imzalama kimliği** Için seçildiğinden ve hem **hata ayıklama** hem de *PushDemo* **Sürüm** yapılandırmalarının **sağlama profili** için, her iki **durumda da** **iPhone** 'un seçili olduğundan emin olun.

1. **Pushdemo. iOS**' de, **yetkilendirmeler. plist** ' i açın ve **Yetkilendirme sekmesinde görüntülendiğinde** **anında iletme bildirimlerini etkinleştir** ' in işaretli olduğundan emin olun. Ardından, **kaynak** sekmesinde görüntülendiğinde **AP ortamı** ayarının **geliştirme** olarak ayarlandığından emin olun.

### <a name="handle-push-notifications-for-ios"></a>İOS için anında iletme bildirimlerini işle

1. **Denetim**  +  **Pushdemo. iOS** projesine tıklayın, **Ekle** menüsünde **Yeni klasör** **'** i seçin ve ardından **klasör adı**olarak *Hizmetleri* kullanarak **Ekle** ' ye tıklayın.

1. **Denetim**  +  **Hizmetler** klasörüne **tıklayın** ve ardından **Ekle** menüsünde **yeni dosya..** . öğesini seçin.

1. **Genel**  >  **boş sınıf**' ı seçin, **ad**için *DeviceInstallationService.cs* girin ve ardından aşağıdaki uygulamayı eklemek için **Yeni** ' ye tıklayın.

    ```csharp
    using System;
    using PushDemo.Models;
    using PushDemo.Services;
    using UIKit;

    namespace PushDemo.iOS.Services
    {
        public class DeviceInstallationService : IDeviceInstallationService
        {
            const int SupportedVersionMajor = 13;
            const int SupportedVersionMinor = 0;

            public string Token { get; set; }

            public bool NotificationsSupported
                => UIDevice.CurrentDevice.CheckSystemVersion(SupportedVersionMajor, SupportedVersionMinor);

            public string GetDeviceId()
                => UIDevice.CurrentDevice.IdentifierForVendor.ToString();

            public DeviceInstallation GetDeviceInstallation(params string[] tags)
            {
                if (!NotificationsSupported)
                    throw new Exception(GetNotificationsSupportError());

                if (string.isNullOrWhitespace(Token))
                    throw new Exception("Unable to resolve token for APNS");

                var installation = new DeviceInstallation
                {
                    InstallationId = GetDeviceId(),
                    Platform = "apns",
                    PushChannel = Token
                };

                installation.Tags.AddRange(tags);

                return installation;
            }

            string GetNotificationsSupportError()
            {
                if (!NotificationsSupported)
                    return $"This app only supports notifications on iOS {SupportedVersionMajor}.{SupportedVersionMinor} and above. You are running {UIDevice.CurrentDevice.SystemVersion}.";

                if (Token == null)
                    return $"This app can support notifications but you must enable this in your settings.";


                return "An error occurred preventing the use of push notifications";
            }
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf benzersiz bir KIMLIK ( [UIDevice. ıdentifierforvendor](https://docs.microsoft.com/dotnet/api/uikit.uidevice.identifierforvendor?view=xamarin-ios-sdk-12) değeri kullanılarak) ve Bildirim Hub 'ı kayıt yükünü sağlar.

1. *Uzantılar* adlı **Pushdemo. iOS** projesine yeni bir klasör ekleyin ve ardından *NSDataExtensions.cs* adlı klasöre aşağıdaki uygulamayla **boş bir sınıf** ekleyin.

    ```csharp
    using System.Text;
    using Foundation;

    namespace PushDemo.iOS.Extensions
    {
        internal static class NSDataExtensions
        {
            internal static string ToHexString(this NSData data)
            {
                var bytes = data.ToArray();

                if (bytes == null)
                    return null;

                StringBuilder sb = new StringBuilder(bytes.Length * 2);

                foreach (byte b in bytes)
                    sb.AppendFormat("{0:x2}", b);

                return sb.ToString().ToUpperInvariant();
            }
        }
    }
    ```

1. **AppDelegate.cs**' de, aşağıdaki ad alanlarının dosyanın en üstüne eklendiğinden emin olun.

    ```csharp
    using System;
    using System.Diagnostics;
    using System.Threading.Tasks;
    using Foundation;
    using PushDemo.iOS.Extensions;
    using PushDemo.iOS.Services;
    using PushDemo.Services;
    using UIKit;
    using UserNotifications;
    using Xamarin.Essentials;
    ```

1. **Ipushdemonocertificate. Service**, **ınocertificate Ationregistrationservice**ve **ıdeviceınstalservıce** uygulamalarına bir başvuru depolamak için özel özellikler ve ilgili yedekleme alanları ekleyin.

    ```csharp
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
    ```

1. Kullanıcı bildirim ayarlarını kaydetmek ve ardından **APNs**ile uzak bildirimler için **RegisterForRemoteNotifications** yöntemini ekleyin.

    ```csharp
    void RegisterForRemoteNotifications()
    {
        MainThread.BeginInvokeOnMainThread(() =>
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert |
                UIUserNotificationType.Badge |
                UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();
        });
    }
    ```

1. Özellik değerini ayarlamak **için, '** . ' `IDeviceInstallationService.Token` . Kaydı yenileyin ve en son depolanmasından bu yana güncelleştirilmiş olan cihaz belirtecini önbelleğe alma.

    ```csharp
    Task CompleteRegistrationAsync(NSData deviceToken)
    {
        DeviceInstallationService.Token = deviceToken.ToHexString();
        return NotificationRegistrationService.RefreshRegistrationAsync();
    }
    ```

1. **Nsdictionary** bildirim verilerini Işlemek ve **notificationactionservice. TriggerAction**öğesini koşullu olarak çağırmak için **processnotificationactions** metodunu ekleyin.

    ```csharp
    void ProcessNotificationActions(NSDictionary userInfo)
    {
        if (userInfo == null)
            return;

        try
        {
            var actionValue = userInfo.ObjectForKey(new NSString("action")) as NSString;

            if (!string.IsNullOrWhiteSpace(actionValue?.Description))
                NotificationActionService.TriggerAction(actionValue.Description);
        }
        catch (Exception ex)
        {
            Debug.WriteLine(ex.Message);
        }
    }
    ```

1. **RegisteredForRemoteNotifications** yöntemini, **devicetoken** bağımsız değişkenini bir IBir **ıistrationasync** metoduna geçirerek geçersiz kılın.

    ```csharp
    public override void RegisteredForRemoteNotifications(
        UIApplication application,
        NSData deviceToken)
        => CompleteRegistrationAsync(deviceToken).ContinueWith((task)
            => { if (task.IsFaulted) throw task.Exception; });
    ```

1. **UserInfo** bağımsız değişkenini **processnotificationactions** yöntemine geçirerek **ReceivedRemoteNotification** yöntemini geçersiz kılın.

    ```csharp
    public override void ReceivedRemoteNotification(
        UIApplication application,
        NSDictionary userInfo)
        => ProcessNotificationActions(userInfo);
    ```

1. Hatayı günlüğe kaydetmek için **FailedToRegisterForRemoteNotifications** yöntemini geçersiz kılın.

    ```csharp
    public override void FailedToRegisterForRemoteNotifications(
        UIApplication application,
        NSError error)
        => Debug.WriteLine(error.Description);
    ```

    > [!NOTE]
    > Bu çok fazla yer tutucudur. Üretim senaryolarında doğru günlük kaydı ve hata işlemeyi uygulamak isteyeceksiniz.

1. **FinishedLaunching** `Bootstrap.Begin` `Forms.Init` **Ideviceınstalınstall hizmeti**'nin platforma özgü uygulamasını geçirme çağrısından sonra doğrudan çağrı yapmak için sonlandırhedbaşlatılıyor yöntemini güncelleştirin.

    ```csharp
    Bootstrap.Begin(() => new DeviceInstallationService());
    ```

1. Aynı yöntemde, koşullu olarak kimlik doğrulaması yapın ve uzak bildirimler için hemen sonrasında kaydolun `Bootstrap.Begin` .

    ```csharp
    if (DeviceInstallationService.NotificationsSupported)
    {
        UNUserNotificationCenter.Current.RequestAuthorization(
                UNAuthorizationOptions.Alert |
                UNAuthorizationOptions.Badge |
                UNAuthorizationOptions.Sound,
                (approvalGranted, error) =>
                {
                    if (approvalGranted && error == null)
                        RegisterForRemoteNotifications();
                });
    }
    ```

1. Hala, **Processhedbaşlatırken**çağrısı, **ProcessNotificationActions** `LoadApplication` **seçenek** bağımsız değişkeni, elde edilen **UserInfo** nesnesine geçirilen **Uıapplication. launchoptions remotenocertificate Ationkey** öğesini içeriyorsa, öğesine yapılan çağrıdan hemen sonra processnotificationactions çağırın.

    ```csharp
    using (var userInfo = options?.ObjectForKey(
        UIApplication.LaunchOptionsRemoteNotificationKey) as NSDictionary)
            ProcessNotificationActions(userInfo);
    ```
