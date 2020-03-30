---
title: UWP uygulamanıza anında iletme bildirimleri ekleme
description: Evrensel Windows Platformu (UWP) uygulamanıza anında bildirim göndermek için Azure App Service Mobile Apps ve Azure Bildirim Hub'larını nasıl kullanacağınızı öğrenin.
ms.assetid: 6de1b9d4-bd28-43e4-8db4-94cd3b187aa3
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 6f200e9649a00bfe890d46f86e62404f1a7e844f
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366295"
---
# <a name="add-push-notifications-to-your-windows-app"></a>Windows uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, [windows hızlı başlatma](app-service-mobile-windows-store-dotnet-get-started.md) projesine anında iletme bildirimleri eklersiniz, böylece her kayıt ekildiğinde aygıta bir anında iletme bildirimi gönderilir.

İndirilen hızlı başlangıç sunucusu projesini kullanmazsanız, anında iletme bildirimi uzantısı paketine ihtiyacınız olacaktır. Daha fazla bilgi için [Azure Mobile Apps için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) bilgisine bakın.

## <a name="configure-a-notification-hub"></a><a name="configure-hub"></a>Bildirim Hub'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a>Anında iletme bildirimleri için uygulamanızı kaydetme

Uygulamanızı Microsoft Mağazası'na göndermeniz, ardından sunucu projenizi [Windows Push Bildirim Hizmetleri (WNS)](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview) ile tümleştirecek şekilde yapılandırmanız gerekir.

1. Visual Studio Solution Explorer'da UWP uygulama projesine sağ tıklayın, Mağaza ile **Ilişkilendir** > **Uygulamasını tıklatın...**

    ![Uygulamayı Microsoft Mağazası ile ilişkilendirme](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)

2. Sihirbazda, **İleri'yi**tıklatın , Microsoft hesabınızla oturum açın, **Rezerv'de**uygulamanız için bir ad yazın, ardından **Rezerv'i**tıklatın.
3. Uygulama kaydı başarıyla oluşturulduktan sonra yeni uygulama adını seçin, **İleri'yi**tıklatın ve ardından **Ilişkilendir'i**tıklatın. Bu, gerekli Microsoft Mağazası kayıt bilgilerini uygulama bildirimine ekler.
4. [Uygulama Kayıt Portalı'na](https://apps.dev.microsoft.com/) gidin ve Microsoft hesabınızla oturum açın. Önceki adımda ilişkilendirdiğiniz Windows Mağazası uygulamasını tıklatın.
5. Kayıt sayfasında, **Uygulama sırları** ve **paket SID**altında değeri bir not yapmak , bir sonraki mobil uygulama arka plan yapılandırmak için kullanacağız.

    ![Uygulamayı Microsoft Mağazası ile ilişkilendirme](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > Gizli anahtar ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın. **Uygulama Kimliği,** Microsoft Hesabı kimlik doğrulamasını yapılandırmak için gizli olarak kullanılır.

[App Center](https://docs.microsoft.com/appcenter/sdk/push/uwp#prerequisite---register-your-app-for-windows-notification-services-wns) ayrıca anında iletme bildirimleri için UWP uygulamalarını yapılandırmak için talimatlara sahiptir.

## <a name="configure-the-backend-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için arka ucu yapılandırma

[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <a name="update-the-server-to-send-push-notifications"></a><a id="update-service"></a>Anında iletme bildirimleri göndermek için sunucuyu güncelleştirme

[Aşağıdaki ,.NET arka uç](#dotnet) veya [Node.js arka uçile](#nodejs)eşleşen arka uç proje türünüzle&mdash;eşleşen yordamı kullanın.

### <a name="net-backend-project"></a><a name="dotnet"></a>.NET arka uç projesi

1. Visual Studio'da sunucu projesini sağ tıklatın ve **NuGet Paketlerini Yönet'i**tıklatın, Microsoft.Azure.NotificationHub'ları arayın, ardından **Yükle'yi**tıklatın. Bu, Bildirim Hub'ları istemci kitaplığını yükler.
2. **Denetleyicileri**genişletin, TodoItemController.cs açın ve aşağıdaki ifadeleri ekleyin:

    ```csharp
    using System.Collections.Generic;
    using Microsoft.Azure.NotificationHubs;
    using Microsoft.Azure.Mobile.Server.Config;
    ```

3. **PostTodoItem** yönteminde, **InsertAsync'e**çağrıdan sonra aşağıdaki kodu ekleyin:

    ```csharp
    // Get the settings for the server project.
    HttpConfiguration config = this.Configuration;
    MobileAppSettingsDictionary settings =
        this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

    // Get the Notification Hubs credentials for the Mobile App.
    string notificationHubName = settings.NotificationHubName;
    string notificationHubConnection = settings
        .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

    // Create the notification hub client.
    NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

    // Define a WNS payload
    var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                            + item.Text + @"</text></binding></visual></toast>";
    try
    {
        // Send the push notification.
        var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

        // Write the success result to the logs.
        config.Services.GetTraceWriter().Info(result.State.ToString());
    }
    catch (System.Exception ex)
    {
        // Write the failure result to the logs.
        config.Services.GetTraceWriter()
            .Error(ex.Message, null, "Push.SendAsync Error");
    }
    ```

    Bu kod, bildirim hub'ına yeni bir öğe yerleştirildikten sonra anında iletme bildirimi göndermesini söyler.

4. Sunucu projesini yeniden yayımlayın.

### <a name="nodejs-backend-project"></a><a name="nodejs"></a>Node.js arka uç projesi
1. Arka uç projenizi ayarlayın.
2. Todoitem.js dosyasındaki varolan kodu aşağıdakilerle değiştirin:

    ```javascript
    var azureMobileApps = require('azure-mobile-apps'),
    promises = require('azure-mobile-apps/src/utilities/promises'),
    logger = require('azure-mobile-apps/src/logger');

    var table = azureMobileApps.table();

    table.insert(function (context) {
    // For more information about the Notification Hubs JavaScript SDK,
    // see https://aka.ms/nodejshubs
    logger.info('Running TodoItem.insert');

    // Define the WNS payload that contains the new item Text.
    var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                + context.item.text + "</text></binding></visual></toast>";

    // Execute the insert.  The insert returns the results as a Promise,
    // Do the push as a post-execute action within the promise flow.
    return context.execute()
        .then(function (results) {
            // Only do the push if configured
            if (context.push) {
                // Send a WNS native toast notification.
                context.push.wns.sendToast(null, payload, function (error) {
                    if (error) {
                        logger.error('Error while sending push notification: ', error);
                    } else {
                        logger.info('Push notification sent successfully!');
                    }
                });
            }
            // Don't forget to return the results from the context.execute()
            return results;
        })
        .catch(function (error) {
            logger.error('Error while running context.execute: ', error);
        });
    });

    module.exports = table;
    ```

    Bu, yeni bir todo öğesi eklendiğinde item.text içeren bir WNS tost bildirimi gönderir.

3. Dosyayı yerel bilgisayarınızda düzenlerken sunucu projesini yeniden yayımlayın.

## <a name="add-push-notifications-to-your-app"></a><a id="update-app"></a>Uygulamanıza anında iletme bildirimleri ekleme
Ardından, uygulamanızın başlangıç anında iletme bildirimleri için kaydolması gerekir. Kimlik doğrulamasını zaten etkinleştirdiyseniz, anında iletme bildirimleri için kaydolmaya çalışmadan önce kullanıcının giriş yaptığından emin olun.

1. proje **App.xaml.cs** dosyasını açın `using` ve aşağıdaki ifadeleri ekleyin:

    ```csharp
    using System.Threading.Tasks;
    using Windows.Networking.PushNotifications;
    ```

2. Aynı dosyada, **Uygulama** sınıfına aşağıdaki **InitNotificationsAsync** yöntem tanımını ekleyin:

    ```csharp
    private async Task InitNotificationsAsync()
    {
        // Get a channel URI from WNS.
        var channel = await PushNotificationChannelManager
            .CreatePushNotificationChannelForApplicationAsync();

        // Register the channel URI with Notification Hubs.
        await App.MobileService.GetPush().RegisterAsync(channel.Uri);
    }
    ```

    Bu kod, WNS'den uygulama için ChannelURI'yi alır ve bu ChannelURI'yi App Service Mobile App'inize kaydeder.

3. **App.xaml.cs'da** **Başlatılan Başlatılmış** olay işleyicisinin üst kısmında, yöntem tanımına **async** değiştirici ekleyin ve aşağıdaki örnekte olduğu gibi yeni **InitNotificationsAsync** yöntemine aşağıdaki çağrıyı ekleyin:

    ```csharp
    protected async override void OnLaunched(LaunchActivatedEventArgs e)
    {
        await InitNotificationsAsync();

        // ...
    }
    ```

    Bu, kısa ömürlü ChannelURI'nin uygulama her başlatılında kaydedildiğini garanti eder.

4. UWP uygulama projenizi yeniden oluşturun. Uygulamanız şimdi bildirim almaya hazırdır.

## <a name="test-push-notifications-in-your-app"></a><a id="test"></a>Uygulamanızdaki anında iletme bildirimlerini test edin

[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <a name="next-steps"></a><a id="more"></a>Sonraki adımlar

Anında iletme bildirimleri hakkında daha fazla bilgi edinin:

* [Azure Mobil Uygulamaları için yönetilen istemci nasıl kullanılır?](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications) Şablonlar, platformlar arası itme ler ve yerelleştirilmiş itmeler gönderme esnekliği sağlar. Şablonları nasıl kaydedin.
* [Anında iletme bildirimi sorunlarını tanılama](../notification-hubs/notification-hubs-push-notification-fixer.md) Bildirimlerin düşmesinin veya aygıtlara düşmemelerinin çeşitli nedenleri vardır. Bu konu, anında iletme bildirimi hatalarının temel nedenini nasıl çözümleyip çözersiniz gösterir.

Aşağıdaki öğreticilerden birine devam etmeyi düşünün:

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-windows-store-dotnet-get-started-users.md) Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-windows-store-dotnet-get-started-offline-data.md) Mobil Uygulama arka uçunu kullanarak uygulamanızı çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların, ağ bağlantısı yokken dahi, mobil uygulama ile etkileşim kurmalarına &mdash;veri görüntüleme, ekleme ya da değiştirme&mdash; olanak tanır.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
