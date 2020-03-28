---
title: Azure Notification Hubs kullanarak Evrensel Windows Platformu uygulamalarına bildirimler gönderme | Microsoft Docs
description: Windows Evrensel Platform uygulamasına bildirim leri iletmek için Azure Bildirim Hub'larını nasıl kullanacağınıza yalın olarak yararlanın.
services: notification-hubs
documentationcenter: windows
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 12/05/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 12/04/2019
ms.openlocfilehash: f78f24ee68545b386169e29a5a52ccc572849ad7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80127067"
---
# <a name="tutorial-send-notifications-to-universal-windows-platform-apps-using-azure-notification-hubs"></a>Öğretici: Azure Bildirim Hub'larını kullanarak Evrensel Windows Platformu uygulamalarına bildirim gönderme

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Bu öğreticide, bir Evrensel Windows Platformu (UWP) uygulamasına anında iletme bildirimleri göndermek için bir bildirim hub’ı oluşturursunuz. Windows Anında Bildirim Hizmeti’ni (WNS) kullanarak anında iletme bildirimleri alan boş bir Windows Mağazası uygulaması oluşturursunuz. Ardından, uygulamanızı çalıştıran tüm aygıtlara anında iletme bildirimleri yayınlamak için bildirim hub'ınızı kullanırsınız.

> [!NOTE]
> Bu öğreticinin tamamlanan kodunu [GitHub](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/UwpSample)'da bulabilirsiniz.

Aşağıdaki adımları atarsınız:

> [!div class="checklist"]
> * Windows Mağazası’nda bir uygulama oluşturma
> * Bildirim hub’ı oluşturma
> * Örnek bir Windows uygulaması oluşturma
> * Test bildirimleri gönderme

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği.** Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir Azure hesabı oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Microsoft Visual Studio 2017 veya sonrası. Bu öğreticideki örnek, [Visual Studio 2019'u kullansın.](https://www.visualstudio.com/products)
- [UWP uygulama geliştirme araçlarının yüklü olması](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
- Etkin bir Windows Mağazası hesabı
- **Uygulamalardan ve diğer gönderenlerden bildirim alın'ın** etkinleştirildiğinden onaylayın. 
    - Bilgisayarınızda **ayarlar** pencereyi başlatın.
    - **Sistem** döşemesini seçin.
    - Sol menüden **Bildirimler & eylemleri** seçin. 
    - **Uygulamalardan ve diğer gönderenlerden bildirim alın'ın** etkin olduğunu doğrulayın. Etkin değilse etkinleştirin.

Bu öğreticiyi tamamlamak UWP uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

## <a name="create-an-app-in-windows-store"></a>Windows Mağazası’nda bir uygulama oluşturma

UWP uygulamalarına anında iletme bildirimleri göndermek için uygulamanızı Windows Mağazası ile ilişkilendirin. Daha sonra, WNS ile tümleştirmek için bildirim hub'ınızı yapılandırın.

1. [Windows Geliştirme Merkezi](https://partner.microsoft.com/dashboard/windows/first-run-experience)’ne gidin, Microsoft hesabınızla oturum açın ve ardından **Yeni uygulama oluştur**’u seçin.

    ![Yeni uygulama düğmesi](./media/notification-hubs-windows-store-dotnet-get-started/windows-store-new-app-button.png)
2. Uygulamanız için bir ad yazın ve ardından **Ürün adını ayır**’ı seçin. Bunu yaptığınızda uygulamanız için yeni bir Windows Mağazası kaydı oluşturulur.

    ![Uygulama adını depolama](./media/notification-hubs-windows-store-dotnet-get-started/store-app-name.png)
3. **Ürün yönetimini genişletin,** **WNS/MPNS'yi**seçin ve ardından **Canlı Hizmetler sitesini**seçin. Microsoft hesabınızda oturum açın. Uygulama kayıt sayfası yeni bir sekmede açılır. Alternatif olarak, doğrudan [Uygulamalarım](https://apps.dev.microsoft.com) sayfasına gidebilir ve bu sayfaya ulaşmak için uygulama adınızı seçebilirsiniz.

    ![WNS MPNS sayfası](./media/notification-hubs-windows-store-dotnet-get-started/wns-mpns-page.png)
4. Uygulama **Sırları** parolasını ve **Paket güvenlik tanımlayıcısını (SID)** not edin.

    >[!WARNING]
    >Uygulama gizli anahtarı ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın.

## <a name="create-a-notification-hub"></a>Bildirim Merkezi Oluşturma

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-wns-settings-for-the-hub"></a>Hub için WNS ayarlarını yapılandırma

1. BILDIRIM **AYARLARI** kategorisinde **Windows (WNS)** seçeneğini belirleyin.
2. Önceki bölümde not ettiğiniz **Paket SID'si** ve **Güvenlik Anahtarı** değerlerini girin.
3. Araç çubuğunda **Kaydet** seçeneğine tıklayın.

    ![Paket SID'si ve Güvenlik Anahtarı kutuları](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Bildirim hub'ınız WNS ile birlikte çalışacak şekilde yapılandırıldı. Uygulamanızı kaydetmek ve bildirim göndermek için gerekli bağlantı dizelerine sahipsiniz.

## <a name="create-a-sample-windows-app"></a>Örnek bir Windows uygulaması oluşturma

1. Visual Studio'da **Dosya** menüsünü açın, **Yeni'yi**seçin ve ardından **Project'i**seçin.
2. Yeni **proje oluştur** iletişim kutusunda aşağıdaki adımları tamamlayın:

    1. Üstteki arama kutusuna **Windows Universal**yazın.
    2. Arama sonuçlarında **Boş Uygulama (Evrensel Windows) seçeneğini**belirleyin ve ardından **İleri'yi**seçin.

       ![Yeni Proje iletişim kutusu](./media/notification-hubs-windows-store-dotnet-get-started/new-project-dialog.png)

    3. Yeni proje iletişim **günlüğüne Yapılandırma'da** bir **Proje adı**ve proje dosyaları **için** konum girin.
    4. **Oluştur'u**seçin.

3. **Hedef** ve **en düşük** platform sürümleri için varsayılan değerleri kabul edin ve **Tamam**’ı seçin.
4. Solution Explorer'da Windows Mağazası uygulama projesine sağ tıklayın, **Yayımla'yı**seçin ve ardından **Mağaza ile Uygulamayı Ilişkilendir'i**seçin. **Uygulamanızı Windows Mağazası ile ilişkilendirin** sihirbazı görüntülenir.
5. Sihirbazda Microsoft hesabınızla oturum açın.
6. 2. adımda kaydettiğiniz uygulamayı seçin, **İleri**'yi ve ardından **İlişkilendir**'i seçin. Bunu yaptığınızda uygulama bildirimine gerekli Windows Mağazası kayıt bilgileri eklenir.
7. Visual Studio'da çözüme sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'i seçin. **NuGet Paketlerini Yönet** penceresi açılır.
8. Arama kutusuna **WindowsAzure.Messaging.Managed** yazın, **Yükle**'yi seçin ve kullanım koşullarını kabul edin.

    ![NuGet Paketlerini Yönet penceresi][20]

    Bu eylem [Microsoft.Azure.NotificationHubs NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs) kullanarak Windows için Azure Notification Hubs kitaplığına bir başvuru indirir, yükler ve ekler.
9. Proje `App.xaml.cs` dosyasını açın ve aşağıdaki ifadeleri ekleyin:

    ```csharp
    using Windows.Networking.PushNotifications;
    using Microsoft.WindowsAzure.Messaging;
    using Windows.UI.Popups;
    ```

10. Projenin `App.xaml.cs` dosyasında sınıfı bulun `App` ve aşağıdaki `InitNotificationsAsync` yöntem tanımını ekleyin. Azure `<your hub name>` portalında oluşturduğunuz bildirim hub'ının adıyla değiştirin `DefaultListenSharedAccessSignature` ve bildirim merkezinizin Access **Polices** sayfasındaki bağlantı dizesiyle değiştirin: `<Your DefaultListenSharedAccessSignature connection string>`

    ```csharp
    private async void InitNotificationsAsync()
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
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

    Bu kod, WNS'den uygulamanın kanal URI'sini alır ve ardından bu kanal URI'sini bildirim hub'ınıza kaydeder.

    >[!NOTE]
    > Yer `hub name` tutucuyu Azure portalında görünen bildirim merkezinin adı ile değiştirin. Ayrıca, bağlantı dizesini `DefaultListenSharedAccessSignature` yer tutucuyu önceki bir bölümdeki bildirim merkezinizin **Access Polices** sayfasından elde ettiğiniz bağlantı dizesiyle değiştirin.

11. Içinde olay işleyicisi `OnLaunched` `App.xaml.cs`üst kısmında, yeni `InitNotificationsAsync` yöntem için aşağıdaki çağrıekleyin:

    ```csharp
    InitNotificationsAsync();
    ```

    Bu eylem, uygulama her başlatında URI kanalının bildirim merkezinizde kayıtlı olduğunu garanti eder.

12. Uygulamayı çalıştırmak için klavyenin **F5** tuşuna basın. Kayıt anahtarını içeren bir iletişim kutusu görüntülenir. İletişim kutusunu kapatmak için **Tamam'ı**tıklatın.

    ![Kayıt başarılı](./media/notification-hubs-windows-store-dotnet-get-started/registration-successful.png)

Uygulamanız şimdi bildirim almaya hazırdır.

## <a name="send-test-notifications"></a>Test bildirimleri gönderme

[Azure portalından](https://portal.azure.com/) bildirim göndererek uygulamanızda bildirim alma testi gerçekleştirebilirsiniz.

1. Azure portalında, Genel Bakış sekmesine geçin ve araç çubuğunda **Test Gönderimi** seçeneğini belirleyin.

    ![Test Gönderimi düğmesi](./media/notification-hubs-windows-store-dotnet-get-started/test-send-button.png)
2. **Test Gönderimi** penceresinde aşağıdaki eylemleri gerçekleştirin:
    1. **Platformlar** için **Windows**’u seçin.
    2. **Bildirim Türü** için **Bildirim**’i seçin.
    3. **Gönder**’i seçin.

        ![Test Gönderimi bölmesi](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)
3. Pencerenin en altındaki **Sonuç** listesinde Gönderme işleminin sonucuna bakın. Bir uyarı iletisi de görürsünüz.

    ![Gönderme işleminin sonucu](./media/notification-hubs-windows-store-dotnet-get-started/result-of-send.png)
4. Şu bildirim iletisini görürsünüz: Masaüstünüzde **test iletisi**.

    ![Bildirim iletisi](./media/notification-hubs-windows-store-dotnet-get-started/test-notification-message.png)

## <a name="next-steps"></a>Sonraki adımlar
Portalı veya konsol uygulamasını kullanarak tüm Windows aygıtlarınıza yayın bildirimleri gönderdiniz. Belirli cihazlara nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Belirli cihazlara anında iletme bildirimleri gönderme](
notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[toast catalog]: https://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: https://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: https://msdn.microsoft.com/library/windows/apps/hh779719.aspx
