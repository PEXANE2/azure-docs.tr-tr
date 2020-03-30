---
title: Belirli cihazlara bildirim gönderme (Evrensel Windows Platformu) | Microsoft Docs
description: Bir Evrensel Windows Platformu uygulamasına son dakika haberleri göndermek için kayıtta etiketlerle birlikte Azure Notification Hubs kullanın.
services: notification-hubs
documentationcenter: windows
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/30/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/22/2019
ms.openlocfilehash: 9151870836b1a616a79e54275ed185a425c11f0c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72385613"
---
# <a name="tutorial-send-notifications-to-specific-devices-running-universal-windows-platform-applications"></a>Öğretici: Evrensel Windows Platformu uygulamaları çalıştıran belirli aygıtlara bildirim gönderme

[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Genel Bakış

Bu öğretici, son dakika haber bildirimlerini yayınlamak için Azure Bildirim Hub'larını nasıl kullanacağınızı gösterir. Bu öğretici, Windows Mağazası veya Windows Phone 8.1 (Silverlight olmayan) uygulamaları kapsar. Windows Phone 8.1 Silverlight'ı hedefliyorsanız, [Azure Bildirim Hub'larını kullanarak belirli Windows Phone aygıtlarına anında iletme bildirimleri'ne](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md)bakın.

Bu eğitimde, Evrensel Windows Platformu (UWP) uygulaması çalıştıran belirli Windows aygıtlarına bildirim leri iletmek için Azure Bildirim Hub'larını nasıl kullanacağınızı öğrenirsiniz. Öğreticiyi tamamladıktan sonra, ilgilendiğiniz son dakika haber kategorileri için kaydolabilirsiniz. Yalnızca bu kategoriler için anında iletme bildirimleri alırsınız.

Yayın senaryolarını etkinleştirmek için, bildirim hub'ında bir kayıt oluştururken bir veya daha fazla *etiket* ekleyin. Bildirimler bir etikete gönderildiğinde, etiketiçin kayıtlı tüm aygıtlar bildirimi alır. Etiketler hakkında daha fazla bilgi için [Yönlendirme ve etiket ifadeleri'ne](notification-hubs-tags-segment-push-message.md)bakın.

> [!NOTE]
> Windows Mağazası ve Windows Phone proje sürümleri 8.1 ve önceki sürümleri Visual Studio 2019'da desteklenmez. Daha fazla bilgi için [Visual Studio 2019 Platform Hedefleme ve Uyumluluk](/visualstudio/releases/2019/compatibility)bilgisine bakın.

Bu öğreticide, aşağıdaki görevleri yaparsınız:

> [!div class="checklist"]
> * Mobil uygulamaya kategori seçimi ekleme
> * Bildirimlere kaydolma
> * Etiketli bildirimler gönderme
> * Uygulamayı çalıştırma ve bildirimler oluşturma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce [Öğretici: Azure Notification Hubs kullanarak Evrensel Windows Platformu uygulamalarına bildirimler gönderme][get-started] öğreticisini tamamlayın.  

## <a name="add-category-selection-to-the-app"></a>Uygulamaya kategori seçimi ekleme

İlk adım, mevcut ana sayfanıza kullanıcının kaydolmak için kategorileri seçebileceği UI öğeleri eklemektir. Seçilen kategoriler cihazda depolanır. Uygulama başlatıldığında, bildirim merkezinizde seçili kategorileretiket olarak bir aygıt kaydı oluşturur.

1. *MainPage.xaml* proje dosyasını açın ve aşağıdaki kodu `Grid` öğeye kopyalayın:

    ```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
        <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
    </Grid>
    ```

1. **Çözüm Gezgini'nde**projeyi sağ tıklatın,**Sınıf** **Ekle'yi** > seçin. **Yeni Öğe Ekle'de**sınıf *Bildirimleri'ni*adlandırın ve **Ekle'yi**seçin. Gerekirse, `public` sınıf tanımına değiştirici ekleyin.

1. Yeni dosyaya aşağıdaki `using` ifadeleri ekleyin:

    ```csharp
    using Windows.Networking.PushNotifications;
    using Microsoft.WindowsAzure.Messaging;
    using Windows.Storage;
    using System.Threading.Tasks;
    ```

1. Aşağıdaki kodu yeni `Notifications` sınıfa kopyalayın:

    ```csharp
    private NotificationHub hub;

    public Notifications(string hubName, string listenConnectionString)
    {
        hub = new NotificationHub(hubName, listenConnectionString);
    }

    public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
    {
        ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
        return await SubscribeToCategories(categories);
    }

    public IEnumerable<string> RetrieveCategories()
    {
        var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
        return categories != null ? categories.Split(','): new string[0];
    }

    public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        if (categories == null)
        {
            categories = RetrieveCategories();
        }

        // Using a template registration to support notifications across platforms.
        // Any template notifications that contain messageParam and a corresponding tag expression
        // will be delivered for this registration.

        const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

        return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                categories);
    }
    ```

    Bu sınıf, bu cihazın alması gereken haber kategorilerini depolamak için yerel depolamayı kullanır. `RegisterNativeAsync` Yöntemi çağırmak yerine, `RegisterTemplateAsync` şablon kaydı kullanarak kategorilere kaydolmak için arayın.

    Birden fazla şablon kaydetmek istiyorsanız, *örneğin, basit WNSTemplateExample*gibi bir şablon adı sağlayın. Şablonları güncelleştirebilmeniz veya silebilmeniz için adlandırmanız gerekir. Tost bildirimleri için bir tane ve fayans lar için bir şablon kaydetmek için birden fazla şablon kaydedebilirsiniz.

    >[!NOTE]
    > Bildirim Hub'ları ile aygıt aynı etiketi kullanarak birden çok şablon kaydedebilir. Bu durumda, etiketi hedefleyen gelen bir ileti, aygıta her şablon için bir tane olmak üzere birden çok bildirimin gönderilmesiyle sonuçlanır. Bu işlem, aynı iletiyi hem rozet hem de bir Windows Mağazası uygulamasında tost bildirimi gibi birden çok görsel bildirimde görüntülemenize olanak tanır.

    Daha fazla bilgi için bkz. [Şablonlar](notification-hubs-templates-cross-platform-push-messages.md).

1. *App.xaml.cs* proje dosyasında, `App` sınıfa aşağıdaki özelliği ekleyin:

    ```csharp
    public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
    ```

    Bu özelliği bir `Notifications` örnek oluşturmak ve erişmek için kullanırsınız.

    Kod içindeki `<hub name>` ve `<connection string with listen access>` yer tutucularını, daha önce edindiğiniz bildirim hub'ı adınız ve **DefaultListenSharedAccessSignature** bağlantı dizeniz ile değiştirin.

   > [!NOTE]
   > Bir istemci uygulaması ile dağıtılmış kimlik bilgileri genellikle güvenli olmadığından yalnızca istemci uygulamanızla *dinleme* erişimi için anahtarı dağıtın. Dinleme erişimi ile uygulamanızın bildirimlere kaydolmasını sağlar, ancak mevcut kayıtlar değiştirilemez ve bildirimler gönderilemez. Tam erişim anahtarı, güvenli bir arka uç hizmetinde bildirimler göndermek ve mevcut kayıtları değiştirmek için kullanılır.

1. *MainPage.xaml.cs* dosyasına aşağıdaki satırı ekleyin:

    ```csharp
    using Windows.UI.Popups;
    ```

1. *MainPage.xaml.cs* dosyasına aşağıdaki yöntemi ekleyin:

    ```csharp
    private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
    {
        var categories = new HashSet<string>();
        if (WorldToggle.IsOn) categories.Add("World");
        if (PoliticsToggle.IsOn) categories.Add("Politics");
        if (BusinessToggle.IsOn) categories.Add("Business");
        if (TechnologyToggle.IsOn) categories.Add("Technology");
        if (ScienceToggle.IsOn) categories.Add("Science");
        if (SportsToggle.IsOn) categories.Add("Sports");

        var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

        var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    ```

    Bu yöntem, kategorilerin bir listesini `Notifications` oluşturur ve listeyi yerel depolama alanında depolamak için sınıfı kullanır. Ayrıca ilgili etiketleri bildirim hub’ınıza kaydeder. Kategoriler değiştiğinde, kayıt yeni kategorilerle yeniden oluşturulur.

Uygulamanız artık cihazın yerel depolama alanında bir kategori kümesini depolayabilir. Uygulama, kullanıcılar kategori seçimini her değiştirdiğinde bildirim hub’ına kaydolur.

## <a name="register-for-notifications"></a>Bildirimlere kaydolma

Bu bölümde, yerel depolama alanında depoladığınız kategorileri kullanarak başlatma sırasında bildirim hub’ına kaydolursunuz.

> [!NOTE]
> Windows Bildirim Hizmeti (WNS) tarafından atanan kanal URI’si her zaman değişebileceğinden, bildirim hatalarını önlemek için sık sık bildirimlere kaydolmanız gerekir. Bu örnek, uygulama her başlatıldığında bildirimlere kaydolur. Örneğin, sık sık çalıştırdığınız uygulamalarda, önceki kayıttan bu yana bir günden az bir süre geçtiyse bant genişliğini korumak için büyük olasılıkla kaydı atlayabilirsiniz.

1. Kategorilere `notifications` göre abone olmak için sınıfı kullanmak için *App.xaml.cs* `InitNotificationsAsync` dosyasını açın ve yöntemi güncelleştirin.

    ```csharp
    // *** Remove or comment out these lines ***
    //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    //var hub = new NotificationHub("your hub name", "your listen connection string");
    //var result = await hub.RegisterNativeAsync(channel.Uri);

    var result = await notifications.SubscribeToCategories();
    ```

    Bu işlem, uygulama başlatıldığında kategorileri yerel depolama dan almalarını sağlar. Daha sonra bu kategorilerin kaydolmasını ister. Bu [yöntemi, Azure Bildirim Hub'ları öğreticisini kullanarak Evrensel Windows Platformu uygulamalarına bildirim gönder'in][get-started] bir parçası olarak oluşturdunuz. `InitNotificationsAsync`
2. *MainPage.xaml.cs* proje dosyasında, `OnNavigatedTo` yönteme aşağıdaki kodu ekleyin:

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        var categories = ((App)Application.Current).notifications.RetrieveCategories();

        if (categories.Contains("World")) WorldToggle.IsOn = true;
        if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
        if (categories.Contains("Business")) BusinessToggle.IsOn = true;
        if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
        if (categories.Contains("Science")) ScienceToggle.IsOn = true;
        if (categories.Contains("Sports")) SportsToggle.IsOn = true;
    }
    ```

    Bu kod, önceden kaydedilmiş kategorilerin durumuna göre ana sayfayı güncelleştirir.

Uygulamanız artık tamamlandı. Aygıt yerel depolama alanında bir dizi kategori depolayabilir. Kullanıcılar kategori seçimini değiştirdiğinde, kaydedilen kategoriler bildirim merkezine kaydolmak için kullanılır. Sonraki bölümde, bu uygulamaya kategori bildirimleri gönderebilen bir arka uç tanımlayacaksınız.

## <a name="run-the-uwp-app"></a>UWP uygulamasını çalıştırın

1. Visual Studio’da F5 tuşunu seçerek uygulamayı derleyin ve başlatın. Uygulama kullanıcı arabirimi, abone olunacak kategorileri seçmenize olanak sağlayan iki durumlu düğmeler sağlar.

   ![Son Dakika Haberleri uygulaması](./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breaking-news.png)

1. Bir veya daha fazla kategori geçişini etkinleştirin ve ardından **Abone Ol'u**seçin.

   Uygulama, seçilen kategorileri etiketlere dönüştürür ve bildirim hub’ından seçilen etiketler için yeni bir cihaz kaydı ister. Uygulama kayıtlı kategorileri bir iletişim kutusunda görüntüler.

    ![Kategori iki durumlu düğmeleri ve Abone ol düğmesi](./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast.png)

## <a name="create-a-console-app-to-send-tagged-notifications"></a>Etiketli bildirimlergöndermek için konsol uygulaması oluşturma

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-console-app-to-send-tagged-notifications"></a>Etiketli bildirimlergöndermek için konsol uygulamasını çalıştırın

Önceki bölümde oluşturulan uygulamayı çalıştırın. Seçili kategorilere ait bildirimler, bildirim olarak görünür.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, son dakika haberlerini kategoriye göre yayınlamayı öğrendiniz. Arka uç uygulaması, etiketlenmiş bildirimleri, bu etiket için bildirim almak için kaydolan aygıtlara iter. Hangi cihazı kullandıklarından bağımsız olarak belirli kullanıcılara bildirim nasıl ileteceklerini öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Azure Bildirim Hub'larını kullanarak Windows uygulamalarına yerelleştirilmiş bildirimler iletme](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- URLs.-->
[get-started]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: https://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[wns object]: https://go.microsoft.com/fwlink/p/?LinkId=260591
