---
title: Bildirim Hub'ları kuruluş itme mimarisi
description: Azure Bildirim Hub'larını kurumsal bir ortamda kullanma hakkında bilgi edinin
services: notification-hubs
documentationcenter: ''
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 0104547a432f7f78d74731e11926bcd82088cef7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76264042"
---
# <a name="enterprise-push-architectural-guidance"></a>Kurumsal gönderim mimari kılavuzu

Günümüzde işletmeler, son kullanıcıları (harici) veya çalışanlar (dahili) için mobil uygulamalar oluşturmaya doğru kademeli olarak ilerliyor. Ana bilgisayarlar veya mobil uygulama mimarisine entegre edilmesi gereken bazı LoB uygulamaları gibi mevcut arka uç sistemlerine sahiptirler. Bu kılavuz, ortak senaryolara olası bir çözüm önererek bu tümleştirmenin en iyi nasıl yapılabildiği hakkında dır.

Sık karşılaşılan bir gereksinim, arka uç sistemlerinde ilgi çekici bir durum oluştuğunda, kullanıcılara mobil uygulamaları üzerinden anında iletme bildirimi göndermektir. Örneğin, iPhone'da bankanın bankacılık uygulamasına sahip olan bir banka müşterisi, hesaptan belirli bir tutarın üzerinde bir borç veya Windows Phone'da bütçe onay uygulaması olan finans departmanından bir çalışanın istediği bir intranet senaryosu yapıldığında bilgilendirilmek istiyor onay talebi alındığı zaman bilgilendirilmelidir.

Banka hesabı veya onay işleme kullanıcıya bir itme başlatması gereken bazı arka uç sisteminde yapılması muhtemeldir. Bir olay bir bildirimi tetiklediğinde itmek için aynı tür bir mantık oluşturması gereken birden çok arka uç sistemi olabilir. Buradaki karmaşıklık, son kullanıcıların farklı bildirimlere abone olabileceği ve hatta birden fazla mobil uygulama olabileceği tek bir itme sistemiyle birlikte birkaç arka uç sistemini entegre etmede yatmaktadır. Örneğin, bir mobil uygulamanın birden çok arka uç sisteminden bildirim almak isteyebileceği intranet mobil uygulamalar. Arka uç sistemleri bilmiyorum ya da itme semantik / teknoloji bilmek gerekir bu yüzden burada ortak bir çözüm geleneksel ilgi herhangi bir olay için arka uç sistemleri anketler ve istemciye push mesajları göndermekten sorumludur bir bileşeni tanıtmak olmuştur.

Daha iyi bir çözüm, çözümü ölçeklenebilir hale getirirken karmaşıklığı azaltan Azure Hizmet Veri Servisi - Konu/Abonelik modelini kullanmaktır.

Çözümün genel mimarisi aşağıdadır (birden fazla mobil uygulamayla genelleştirilmiştir, ancak yalnızca bir mobil uygulama olduğunda eşit derecede uygulanabilir)

## <a name="architecture"></a>Mimari

![][1]

Bu mimari diyagramdaki anahtar parça, bir konu/abonelik programlama modeli [(Service Bus Pub/Sub programlamada]daha fazla) sağlayan Azure Hizmet Veri Tos'udur. Bu durumda, Mobil arka uç (genellikle Azure [Mobil Hizmeti], mobil uygulamalara bir itme başlatır) olan alıcı, doğrudan arka uç sistemlerinden ileti almaz, bunun [yerine, mobil]arka uç bir veya daha fazla arka uç sisteminden ileti almasına olanak tanıyan Azure Hizmet Veri Servisi tarafından sağlanan bir ara soyutlama katmanı alır. Bir Hizmet Veri Servisi Konusu arka uç sistemlerinin her biri için oluşturulmalıdır, örneğin, Hesap, İk, Finans, temelde ilgi "konular" olan, anında iletme bildirimi olarak gönderilmek üzere mesajları başlatır. Arka uç sistemleri bu konulara ileti gönderir. Mobil Arka Uç, Bir Hizmet Veri Aracı aboneliği oluşturarak bu tür konulara abone olabilir. Bu, ilgili arka uç sisteminden bir bildirim almak için mobil arka uç hakkı verir. Mobil arka uç aboneliklerindeki iletileri dinlemeye devam eder ve bir ileti gelir gelmez geri döner ve bildirim merkezine bildirim olarak gönderir. Bildirim hub'ları daha sonra iletiyi mobil uygulamaya iletilir. Önemli bileşenlerin listesi aşağıda veda edilmiştir:

1. Arka uç sistemleri (LoB/Legacy sistemleri)
   * Hizmet Veri Servisi Konusu Oluşturur
   * Mesaj Gönderir
1. Mobil arka uç
   * Hizmet Aboneliği Oluşturur
   * İleti alır (Arka uç sisteminden)
   * İstemcilere bildirim gönderir (Azure Bildirim Hub'ı aracılığıyla)
1. Mobile Uygulama
   * Bildirimleri alır ve görüntüler

### <a name="benefits"></a>Avantajlar

1. Alıcı (Bildirim Hub'ı üzerinden mobil uygulama/hizmet) ve gönderen (arka uç sistemleri) arasındaki ayrım, ek arka uç sistemlerinin en az değişiklikle tümleştirilmesini sağlar.
1. Ayrıca, birden çok mobil uygulamanın bir veya daha fazla arka uç sisteminden etkinlik alabilme senaryosunu da yapar.  

## <a name="sample"></a>Örnek

### <a name="prerequisites"></a>Ön koşullar

Kavramları nyanıtır ve yapılandırma adımlarını & ortak oluşturmayı tanımak için aşağıdaki öğreticileri tamamlayın:

1. [Service Bus Pub/Sub programming] - Bu öğretici, Service Bus Topics/Subscriptions ile çalışmanın ayrıntılarını, konuları/abonelikleri içeren bir ad alanıoluşturmanın ayrıntılarını, onlardan ileti & nasıl gönderilebildiğini açıklar.
2. [Bildirim Hub'ları - Windows Universal öğreticisi] - Bu öğretici, bir Windows Mağazası uygulamasının nasıl ayarlanır ve bildirimleri kaydetmek ve sonra almak için Bildirim Hub'larını nasıl kullanacağınızı açıklar.

### <a name="sample-code"></a>Örnek kod

Tam örnek kodu [Bildirim Hub Örnekleri]mevcuttur. Üç bileşene ayrılır:

1. **EnterprisePushBackendSystem**

    a. Bu proje **WindowsAzure.ServiceBus** NuGet paketini kullanır ve [Service Bus Pub/Sub programlamayı]temel alır.

    b. Bu uygulama mobil uygulamaya teslim edilecek mesajı başlatan bir LoB sistemi simüle etmek için basit bir C# konsol uygulamasıdır.

    ```csharp
    static void Main(string[] args)
    {
        string connectionString =
            CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

        // Create the topic
        CreateTopic(connectionString);

        // Send message
        SendMessage(connectionString);
    }
    ```

    c. `CreateTopic`Servis Veri Servisi konusunu oluşturmak için kullanılır.

    ```csharp
    public static void CreateTopic(string connectionString)
    {
        // Create the topic if it does not exist already

        var namespaceManager =
            NamespaceManager.CreateFromConnectionString(connectionString);

        if (!namespaceManager.TopicExists(sampleTopic))
        {
            namespaceManager.CreateTopic(sampleTopic);
        }
    }
    ```

    d. `SendMessage`iletileri bu Hizmet Veri Servisi Konusuna göndermek için kullanılır. Bu kod, yalnızca, örneğin amacı için konuya düzenli olarak rasgele iletiler gönderir. Normalde bir olay oluştuğunda ileti gönderen bir arka uç sistemi vardır.

    ```csharp
    public static void SendMessage(string connectionString)
    {
        TopicClient client =
            TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

        // Sends random messages every 10 seconds to the topic
        string[] messages =
        {
            "Employee Id '{0}' has joined.",
            "Employee Id '{0}' has left.",
            "Employee Id '{0}' has switched to a different team."
        };

        while (true)
        {
            Random rnd = new Random();
            string employeeId = rnd.Next(10000, 99999).ToString();
            string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

            // Send Notification
            BrokeredMessage message = new BrokeredMessage(notification);
            client.Send(message);

            Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

            System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
        }
    }
    ```
2. **Alma ve Gönderme Bildirimi**

    a. Bu proje *WindowsAzure.ServiceBus* ve **Microsoft.Web.WebJobs.Publish** NuGet paketlerini kullanır ve [Service Bus Pub/Sub programlamayı]temel alır.

    b. Aşağıdaki konsol uygulaması, LoB/arka uç sistemlerinden gelen iletileri dinlemek için sürekli çalışması gerekdiğinden Azure [WebJob] olarak çalışır. Bu uygulama Mobil arka uç bir parçasıdır.

    ```csharp
    static void Main(string[] args)
    {
        string connectionString =
                 CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

        // Create the subscription that receives messages
        CreateSubscription(connectionString);

        // Receive message
        ReceiveMessageAndSendNotification(connectionString);
    }
    ```

    c. `CreateSubscription`arka uç sisteminin ileti gönderdiği konu için Bir Hizmet Veri Servisi aboneliği oluşturmak için kullanılır. İş senaryosuna bağlı olarak, bu bileşen ilgili konulara bir veya daha fazla abonelik oluşturur (örneğin, bazıları İk sisteminden, bazıları Finans sisteminden vb. iletiler alıyor olabilir)

    ```csharp
    static void CreateSubscription(string connectionString)
    {
        // Create the subscription if it does not exist already
        var namespaceManager =
            NamespaceManager.CreateFromConnectionString(connectionString);

        if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
        {
            namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
        }
    }
    ```

    d. `ReceiveMessageAndSendNotification`aboneliğini kullanarak konuyla ilgili iletiyi okumak için kullanılır ve okuma başarılı olursa, Azure Bildirim Hub'larını kullanarak mobil uygulamaya gönderilecek bir bildirim (örnek senaryoda Windows yerel tost bildirimi) hazırlayın.

    ```csharp
    static void ReceiveMessageAndSendNotification(string connectionString)
    {
        // Initialize the Notification Hub
        string hubConnectionString = CloudConfigurationManager.GetSetting
                ("Microsoft.NotificationHub.ConnectionString");
        hub = NotificationHubClient.CreateClientFromConnectionString
                (hubConnectionString, "enterprisepushservicehub");

        SubscriptionClient Client =
            SubscriptionClient.CreateFromConnectionString
                    (connectionString, sampleTopic, sampleSubscription);

        Client.Receive();

        // Continuously process messages received from the subscription
        while (true)
        {
            BrokeredMessage message = Client.Receive();
            var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

            if (message != null)
            {
                try
                {
                    Console.WriteLine(message.MessageId);
                    Console.WriteLine(message.SequenceNumber);
                    string messageBody = message.GetBody<string>();
                    Console.WriteLine("Body: " + messageBody + "\n");

                    toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                    SendNotificationAsync(toastMessage);

                    // Remove message from subscription
                    message.Complete();
                }
                catch (Exception)
                {
                    // Indicate a problem, unlock message in subscription
                    message.Abandon();
                }
            }
        }
    }
    static async void SendNotificationAsync(string message)
    {
        await hub.SendWindowsNativeNotificationAsync(message);
    }
    ```

    e. Bu uygulamayı **WebJob**olarak yayınlamak için Visual Studio'daki çözüme sağ tıklayın ve **WebJob olarak yayınla'yı** seçin

    ![][2]

    f. Yayımlama profilinizi seçin ve bu WebJob'u barındıran ve Web Sitesini Yayımladıktan sonra **Yayımlayan'ı**barındıran yeni bir Azure Web Sitesi oluşturun.

    ![][3]

    g. İşi "Sürekli Çalıştır" olarak yapılandırın, böylece [Azure portalında] oturum açtığınızda aşağıdaki gibi bir şey görmeniz gerekir:

    ![][4]

3. **EnterprisePushMobileApp**

    a. Bu uygulama, Mobil arka uçünüzün bir parçası olarak çalışan WebJob'tan tost bildirimleri alan ve görüntüleyen bir Windows Mağazası uygulamasıdır. Bu kod [Bildirim Hub'larına]dayanır - Windows Universal öğretici.  

    b. Uygulamanızın tost bildirimleri almak için etkinleştirildiğinden emin olun.

    c. Aşağıdaki Bildirim Hub'ları kayıt kodunun Uygulama başlangıcında çağrıldığından emin `HubName` olun `DefaultListenSharedAccessSignature` (aşağıdaki değerleri ve değerleri değiştirdikten sonra:

    ```csharp
    private async void InitNotificationsAsync()
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
        var result = await hub.RegisterNativeAsync(channel.Uri);

        // Displays the registration ID so you know it was successful
        if (result.RegistrationId != null)
        {
            var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

### <a name="running-the-sample"></a>Örneği çalıştırma

1. Webİş'inizin başarılı bir şekilde çalıştığından ve sürekli olarak çalışacak şekilde zamanlandığından emin olun.
2. Windows Mağazası uygulamasını başlatan **EnterprisePushMobileApp'ı**çalıştırın.
3. LoB arka ucunu taklit eden ve mesaj göndermeye başlayan **EnterprisePushBackendSystem** konsol uygulamasını çalıştırın ve aşağıdaki resim gibi görünen tost bildirimlerini görmeniz gerekir:

    ![][5]

4. İletiler ilk olarak, Web İşinizde Servis Veri Cemi abonelikleri tarafından izlenen Hizmet Veri Mesuli konulara gönderilmiştir. Bir ileti alındıktan sonra bir bildirim oluşturuldu ve mobil uygulamaya gönderildi. Web İşiniz için [Azure portalındaki] Günlükler bağlantısına gittiğinizde işlemi onaylamak için WebJob günlüklerine bakabilirsiniz:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Bildirim Merkezi Örnekleri]: https://github.com/Azure/azure-notificationhubs-samples
[Azure Mobil Hizmet]: https://azure.microsoft.com/documentation/services/mobile-services/
[Azure Servis Veri Servisi]: https://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Servis Otobüsü Pub / Alt programlama]: https://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: ../app-service/webjobs-create.md
[Bildirim Hub'ları - Windows Universal öğretici]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure portalında]: https://portal.azure.com/
