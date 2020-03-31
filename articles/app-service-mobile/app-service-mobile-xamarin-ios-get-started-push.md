---
title: Xamarin.iOS uygulamanıza anında iletme bildirimleri ekleme
description: Xamarin.iOS uygulamanıza anında iletme bildirimleri göndermek için Azure Uygulama Hizmetini nasıl kullanacağınızı öğrenin.
ms.assetid: 2921214a-49f8-45e1-a306-a85ce21defca
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: f9c70491d06f61931ebabda859ff3a86ed035b44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249288"
---
# <a name="add-push-notifications-to-your-xamarinios-app"></a>Xamarin.iOS Uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, [Xamarin.iOS hızlı başlatma](app-service-mobile-xamarin-ios-get-started.md) projesine anında iletme bildirimleri eklersiniz, böylece her kayıt ekildiğinde aygıta bir anında iletme bildirimi gönderilir.

İndirilen hızlı başlangıç sunucusu projesini kullanmazsanız, anında iletme bildirimi uzantısı paketine ihtiyacınız olacaktır. Daha fazla bilgi için [Azure Mobile Apps için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) bilgisine bakın.

## <a name="prerequisites"></a>Ön koşullar

* [Xamarin.iOS hızlı başlatma](app-service-mobile-xamarin-ios-get-started.md) eğitimini tamamlayın.
* Fiziksel bir iOS aygıtı. Anında iletme bildirimleri iOS simülatörü tarafından desteklenmez.

## <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Uygulamayı Apple'ın geliştirici portalına anında iletme bildirimleri için kaydedin

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-to-send-push-notifications"></a>Anında iletme bildirimleri gönderecek şekilde Mobil Uygulamanızı yapılandırın

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için sunucu projesini güncelleştirme

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a>Xamarin.iOS projenizi yapılandırın

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-to-your-app"></a>Uygulamanıza anında iletme bildirimleri ekleme

1. **QSTodoService'te,** **AppDelegate'in** mobil istemciyi edinebilmeleri için aşağıdaki özelliği ekleyin:

    ```csharp
    public MobileServiceClient GetClient {
        get
        {
            return client;
        }
        private set
        {
            client = value;
        }
    }
    ```

2. Aşağıdaki `using` ifadeyi **AppDelegate.cs** dosyasının üstüne ekleyin.

    ```csharp
    using Microsoft.WindowsAzure.MobileServices;
    using Newtonsoft.Json.Linq;
    ```

3. **AppDelegate'de,** **FinishedLaunching** olayını geçersiz kılın:

   ```csharp
    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // registers for push for iOS8
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

        return true;
    }
    ```

4. Aynı dosyada, `RegisteredForRemoteNotifications` olayı geçersiz kılın. Bu kodda, sunucu tarafından desteklenen tüm platformlarda gönderilecek basit bir şablon bildirimi için kaydolabilirsiniz.

    Bildirim Hub'ları içeren şablonlar hakkında daha fazla bilgi için [Şablonlar'a](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md)bakın.

    ```csharp
    public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
    {
        MobileServiceClient client = QSTodoService.DefaultService.GetClient;

        const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

        JObject templates = new JObject();
        templates["genericMessage"] = new JObject
        {
            {"body", templateBodyAPNS}
        };

        // Register for push with your mobile app
        var push = client.GetPush();
        push.RegisterAsync(deviceToken, templates);
    }
    ```

5. Ardından, **DidReceivedRemoteNotification** olayını geçersiz kılın:

   ```csharp
    public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
    {
        NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

        string alert = string.Empty;
        if (aps.ContainsKey(new NSString("alert")))
            alert = (aps [new NSString("alert")] as NSString).ToString();

        //show alert
        if (!string.IsNullOrEmpty(alert))
        {
            UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
            avAlert.Show();
        }
    }
    ```

Uygulamanız artık anında iletme bildirimlerini destekleyecek şekilde güncelleştirildi.

## <a name="test-push-notifications-in-your-app"></a><a name="test"></a>Uygulamanızdaki anında iletme bildirimlerini test edin

1. Projeyi oluşturmak için **Çalıştır** düğmesine basın ve uygulamayı iOS özellikli bir cihazda başlatın, ardından anında iletme bildirimlerini kabul etmek için **Tamam'ı** tıklatın.

   > [!NOTE]
   > Uygulamanızdan gelen anında iletme bildirimlerini açıkça kabul etmelisiniz. Bu istek yalnızca uygulamailk kez çalıştırılır.

2. Uygulamada, bir görev yazın ve sonra artı**+**( ) simgesini tıklatın.
3. Bildirimin alındığını doğrulayın ve bildirimi kapatmak için **Tamam'ı** tıklatın.
4. Adımı 2'yi tekrarlayın ve uygulamayı hemen kapatın, ardından bir bildirimin gösterildiğini doğrulayın.

Bu öğreticiyi başarıyla tamamladınız.
