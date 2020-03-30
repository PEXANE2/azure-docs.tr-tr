---
title: Azure Bildirim Hub'ları Windows için Güvenli Push
description: Azure'da güvenli anında iletme bildirimleri gönderme yi öğrenin. .NET API kullanarak C# dilinde yazılan kod örnekleri.
documentationcenter: windows
author: sethmanheim
manager: femila
editor: jwargo
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: db42cf7f886855af77073963e6f04ac088ca5612
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75530740"
---
# <a name="securely-push-notifications-from-azure-notification-hubs"></a>Azure Bildirim Hub'larından güvenli bir şekilde anında iletme bildirimleri

> [!div class="op_single_selector"]
> * [Windows Evrensel](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

## <a name="overview"></a>Genel Bakış

Microsoft Azure'da anında bildirim desteği, mobil cihazlar için hem tüketici hem de kurumsal uygulamalar için anında iletme bildirimlerinin uygulanmasını büyük ölçüde kolaylaştıran, kullanımı kolay, çok platformlu, ölçeklenmiş bir anında iletme altyapısına erişmenizi sağlar Platform.

Düzenleyici veya güvenlik kısıtlamaları nedeniyle, bazen bir uygulama bildirime standart anında iletme bildirimi altyapısı aracılığıyla aktarılamayan bir şey eklemek isteyebilir. Bu öğretici, istemci aygıtı ve uygulama arka ucu arasında güvenli, kimlik doğrulaması yla bağlantı yoluyla hassas bilgileri göndererek aynı deneyimi nasıl elde edilebiz açıklanır.

Yüksek bir düzeyde, akış aşağıdaki gibidir:

1. Uygulama arka uç:
   * Güvenli yükü arka uç veritabanında saklar.
   * Bu bildirimin kimliğini aygıta gönderir (güvenli bilgi gönderilmez).
2. Bildirim alırken cihazdaki uygulama:
   * Aygıt, güvenli yükü isteyen arka uçla bağlantı kurur.
   * Uygulama, yükü cihazda bir bildirim olarak gösterebilir.

Önceki akışta (ve bu öğreticide) aygıtın, kullanıcı oturum açtıktan sonra yerel depolama alanında bir kimlik doğrulama belirteci depoladığını varsaymak önemlidir. Bu, aygıt bu belirteci kullanarak bildirimin güvenli yükünü alabildiği için tamamen sorunsuz bir deneyim garanti eder. Uygulamanız cihazda kimlik doğrulama belirteçleri depolamazveya bu belirteçlerin süresi dolmuşsa, bildirimin alınmasından sonra cihaz uygulaması kullanıcıdan uygulamayı başlatmasını isteyen genel bir bildirim görüntülemelidir. Uygulama daha sonra kullanıcının kimliğini doğrular ve bildirim yükünü gösterir.

Bu Güvenli Push öğreticisi, anında iletme bildiriminin güvenli bir şekilde nasıl gönderilebildiğini gösterir. Öğretici, [Kullanıcıları Bildir](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) öğreticisine göre inşa eder, bu nedenle önce bu öğreticideki adımları tamamlamanız gerekir.

> [!NOTE]
> Bu öğretici, bildirim hub'ınızı [Bildirim Hub'larıyla Başlarken'de (Windows Mağazası)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)açıklandığı şekilde oluşturduğunuzu ve yapılandırdığınızı varsayar.
> Ayrıca, Windows Phone 8.1'in Windows (Windows Phone değil) kimlik bilgileri gerektirdiğini ve arka plan görevlerinin Windows Phone 8.0 veya Silverlight 8.1'de çalışmadığını unutmayın. Windows Mağazası uygulamaları için, yalnızca uygulama kilit ekranı etkinse (Appmanifest'teki onay kutusunu tıklatın) arka plan görevi aracılığıyla bildirim alabilirsiniz.

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Windows Phone Projesini Değiştirme

1. **NotifyUserWindowsPhone** projesinde, itme arka planı görevini kaydetmek için App.xaml.cs için aşağıdaki kodu ekleyin. `OnLaunched()` yönteminin sonuna aşağıdaki kod satırını ekleyin:

    ```csharp
    RegisterBackgroundTask();
    ```
2. Hala App.xaml.cs, `OnLaunched()` yöntemden hemen sonra aşağıdaki kodu ekleyin:

    ```csharp
    private async void RegisterBackgroundTask()
    {
        if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
        {
            var result = await BackgroundExecutionManager.RequestAccessAsync();
            var builder = new BackgroundTaskBuilder();

            builder.Name = "PushBackgroundTask";
            builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
            builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
            BackgroundTaskRegistration task = builder.Register();
        }
    }
    ```
3. App.xaml.cs dosyasının üst kısmında aşağıdaki `using` ifadeleri ekleyin:

    ```csharp
    using Windows.Networking.PushNotifications;
    using Windows.ApplicationModel.Background;
    ```
4. Visual Studio'daki **Dosya** menüsünde **Tümünü Kaydet**'e tıklayın.

## <a name="create-the-push-background-component"></a>Push Arka Plan Bileşenini Oluşturma

Bir sonraki adım itme arka plan bileşeni ni oluşturmaktır.

1. Çözüm Gezgini'nde, çözümün üst düzey düğümüne sağ tıklayın (Bu durumda**Solution SecurePush),** sonra **Ekle'yi**tıklatın, ardından **Yeni Proje'yi**tıklatın.
2. **Mağaza Uygulamalarını**Genişletin, ardından Windows **Phone Apps'ı**tıklatın, ardından **Windows Runtime Bileşeni'ni (Windows Phone)** tıklatın. Projeye **PushBackgroundComponent**adını ver ve projeyi oluşturmak için **Tamam'ı** tıklatın.

    ![][12]
3. Solution Explorer'da **PushBackgroundComponent (Windows Phone 8.1)** projesine sağ tıklayın, ardından **Ekle'yi**tıklatın, ardından **Sınıf'ı**tıklatın. Yeni sınıfı `PushBackgroundTask.cs`adlandırın. Sınıfı oluşturmak için **Ekle'yi** tıklatın.
4. Ad alanı tanımının `PushBackgroundComponent` tüm içeriğini aşağıdaki kodla değiştirin ve `{back-end endpoint}` arka uçunuzu dağıtırken yer tutucuyu elde edilen arka uç ucuyla değiştirin:

    ```csharp
    public sealed class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public sealed class PushBackgroundTask : IBackgroundTask
        {
            private string GET_URL = "{back-end endpoint}/api/notifications/";

            async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
            {
                // Store the content received from the notification so it can be retrieved from the UI.
                RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                var notificationId = raw.Content;

                // retrieve content
                BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                var httpClient = new HttpClient();
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                ShowToast(notification);

                deferral.Complete();
            }

            private void ShowToast(Notification notification)
            {
                ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                ToastNotification toast = new ToastNotification(toastXml);
                ToastNotificationManager.CreateToastNotifier().Show(toast);
            }
        }
    ```
5. Solution Explorer'da **PushBackgroundComponent (Windows Phone 8.1)** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet'i**tıklatın.
6. Sol taraftaki **Çevrimiçi** öğesine tıklayın.
7. **Arama** kutusuna **Http İstemcisi** yazın.
8. Sonuç listesinde Microsoft **HTTP İstemci Kitaplıkları'nı**tıklatın ve sonra **Yükle'yi**tıklatın. Yüklemeyi tamamlayın.
9. NuGet **Arama** kutusuna **Json.net** yazın. **Json.NET** paketi yükleyin ve NuGet Paket Yöneticisi penceresini kapatın.
10. Dosyanın `using` üst kısmında aşağıdaki ifadeleri ekleyin: `PushBackgroundTask.cs`

    ```csharp
    using Windows.ApplicationModel.Background;
    using Windows.Networking.PushNotifications;
    using System.Net.Http;
    using Windows.Storage;
    using System.Net.Http.Headers;
    using Newtonsoft.Json;
    using Windows.UI.Notifications;
    using Windows.Data.Xml.Dom;
    ```
11. Solution Explorer'da, **NotifyUserWindowsPhone (Windows Phone 8.1)** projesinde, **Başvurular'a**sağ tıklayın, ardından **Başvuru Ekle'yi tıklatın...**. Başvuru Yöneticisi iletişim kutusunda, **PushBackgroundComponent'un**yanındaki kutuyu işaretleyin ve ardından **Tamam'ı**tıklatın.
12. Solution Explorer'da, **NotifyUserWindowsPhone (Windows Phone 8.1)** projesinde **Package.appxmanifest'e** çift tıklayın. **Bildirimler**altında , **Tost** **Evet için yetenekli** ayarlayın.

    ![][3]
13. Hala **Package.appxmanifest**, üst yakınındaki **Bildirimler** menüsüne tıklayın. Kullanılabilir **Bildirimler** açılır açılır durumda, **Arka Plan Görevleri'ni**tıklatın ve sonra **Ekle'yi**tıklatın.
14. **Package.appxmanifest'** de , **Özellikler**altında, Push bildirimini kontrol **edin.**
15. **Package.appxmanifest'de,** **Uygulama Ayarları** **altında, Giriş Noktası** alanına **PushBackgroundComponent yazın.**

    ![][13]
16. **Dosya** menüsünden **Tümünü Kaydet**'e tıklayın.

## <a name="run-the-application"></a>Uygulamayı Çalıştır

Uygulamayı çalıştırmak için aşağıdakileri yapın:

1. Visual Studio'da **AppBackend** Web API uygulamasını çalıştırın. ASP.NET bir web sayfası görüntülenir.
2. Visual Studio'da **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone uygulamasını çalıştırın. Windows Phone emülatörü uygulamayı otomatik olarak çalıştırır ve yükler.
3. **NotifyUserWindowsPhone** uygulaması Kullanıcı Arabirimi'ne bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değerolmalıdır.
4. **NotifyUserWindowsPhone** uygulaması Kullanıcı Arabirimi'nde **Giriş Yap'ı tıklatın ve kaydolun.** Ardından **Gönder tuşuna**bas.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
