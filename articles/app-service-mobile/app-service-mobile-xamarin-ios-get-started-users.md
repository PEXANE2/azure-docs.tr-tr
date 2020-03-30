---
title: Xamarin iOS'ta kimlik doğrulamaya başlayın
description: Xamarin iOS uygulamanızın kullanıcılarını AAD, Google, Facebook, Twitter ve Microsoft gibi kimlik sağlayıcılarıyla doğrulamak için Mobil Uygulamaları nasıl kullanacağınızı öğrenin.
ms.assetid: 180cc61b-19c5-48bf-a16c-7181aef3eacc
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 05e936accbcd5c6fa2760c4f8682d907557f23b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461326"
---
# <a name="add-authentication-to-your-xamarinios-app"></a>Xamarin.iOS uygulamanıza kimlik doğrulama ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Genel Bakış

Bu konu, bir App Service Mobil Uygulamasının kullanıcılarını istemci uygulamanızdan nasıl doğruladığınızı gösterir. Bu eğitimde, App Service tarafından desteklenen bir kimlik sağlayıcısını kullanarak Xamarin.iOS hızlı başlatma projesine kimlik doğrulama eklersiniz. Mobil Uygulamanız tarafından başarıyla doğrulandıktan ve onaylandıktan sonra, kullanıcı kimliği değeri görüntülenir ve kısıtlı tablo verilerine erişebilirsiniz.

Öncelikle [bir Xamarin.iOS uygulaması oluştur]eğitimini tamamlamanız gerekir. İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, kimlik doğrulama uzantısı paketini projenize eklemeniz gerekir. Sunucu uzantı paketleri hakkında daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)'ya bakın.

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Uygulamanızı kimlik doğrulama ve uygulama hizmetlerini yapılandırmak için kaydedin
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a>Uygulamanızı İzin Verilen Harici Yönlendirme URL'lerine ekleyin

Güvenli kimlik doğrulaması, uygulamanız için yeni bir URL düzeni tanımlamanızı gerektirir. Bu, kimlik doğrulama işlemi tamamlandıktan sonra kimlik doğrulama sisteminin uygulamanıza geri yönlendirmesine olanak tanır. Bu öğreticide, URL şeması _ekadını_ sonuna kadar kullanırız. Ancak, seçtiğiniz herhangi bir URL düzenini kullanabilirsiniz. Mobil uygulamanıza özgü olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. Azure [portalında](https://portal.azure.com/)Uygulama Hizmetinizi seçin.

2. Kimlik **Doğrulama / Yetkilendirme** menüsü seçeneğini tıklatın.

3. İzin **Verilen Harici Yönlendirme URL'lerine**girin. `url_scheme_of_your_app://easyauth.callback`  Bu dizedeki **url_scheme_of_your_app,** mobil uygulamanızın URL Düzenidir.  Bir protokol için normal URL belirtimini izlemeli (yalnızca harfleri ve sayıları kullanın ve harfle başlamalıdır).  Mobil uygulama kodunuzu URL Düzeni ile birkaç yerde ayarlamanız gerektiğinden, seçtiğiniz dizeye not alabilirsiniz.

4. **Tamam**'a tıklayın.

5. **Kaydet**'e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a>İzinleri kimlik doğrulaması verilen kullanıcılarla sınırlama
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* Visual Studio veya Xamarin Studio'da, istemci projesini bir aygıt veya emülatör üzerinde çalıştırın. Uygulama başladıktan sonra 401 (Yetkisiz) durum kodu içeren işlenmemiş bir özel durum ortaya çıkın. Hata ayıklama nın konsoluna günlüğe kaydedilir. Visual Studio'da, çıkış penceresindeki arızayı görmelisiniz.

    Bu yetkisiz hata, uygulamanın Mobil Uygulamanıza kimlik doğrulaması yapılamayan bir kullanıcı olarak geri seve erişmeye çalıştığı için gerçekleşir. *TodoItem* tablosu artık kimlik doğrulaması gerektirir.

Ardından, kimlik doğrulaması yapılan bir kullanıcıyla Mobil Uygulama arka ucundan kaynak istemek için istemci uygulamasını güncellersiniz.

## <a name="add-authentication-to-the-app"></a>Uygulamaya kimlik doğrulama ekleme
Bu bölümde, verileri görüntülemeden önce uygulamayı bir giriş ekranı görüntülemek için değiştirirsiniz. Uygulama başlatıldığında, Uygulama Hizmetinize bağlanmaz ve herhangi bir veri görüntülemez. Kullanıcı yenileme jestini ilk kez yaptıktan sonra, oturum açma ekranı görüntülenir; başarılı girişten sonra todo öğeleri listesi görüntülenir.

1. İstemci projesinde, **dosyayı QSTodoService.cs** açın ve `MobileServiceUser` aşağıdaki ifadesini kullanarak ve QSTodoService sınıfına erişimci ile ekleyin:

    ```csharp
    using UIKit;

    // Logged in user
    private MobileServiceUser user;
    public MobileServiceUser User { get { return user; } }
    ```

2. **QSTodoService'e** **kimlik doğrulama** adlı yeni yöntem aşağıdaki tanımla ekleyin:

    ```csharp
    public async Task Authenticate(UIViewController view)
    {
        try
        {
            AppDelegate.ResumeWithURL = url => url.Scheme == "{url_scheme_of_your_app}" && client.ResumeWithURL(url);
            user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
        }
    }
    ```

    > [!NOTE]
    > Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, **loginAsync'e** aktarılan değeri aşağıdakilerden biriyle değiştirin: _MicrosoftAccount_, _Twitter_, _Google_, veya _WindowsAzureActiveDirectory_.

3. Açık **QSTodoListViewController.cs**. **ViewDidLoad'ın** meton tanımını değiştirerek sonuna yakın **RefreshAsync()** çağrısını kaldırarak:

    ```csharp
    public override async void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        todoService = QSTodoService.DefaultService;
        await todoService.InitializeStoreAsync();

        RefreshControl.ValueChanged += async (sender, e) => {
            await RefreshAsync();
        }

        // Comment out the call to RefreshAsync
        // await RefreshAsync();
    }
    ```

4. **Kullanıcı** özelliği null ise kimlik doğrulaması için **RefreshAsync** yöntemini değiştirin. Yöntem tanımının üst kısmında aşağıdaki kodu ekleyin:

    ```csharp
    // start of RefreshAsync method
    if (todoService.User == null) {
        await QSTodoService.DefaultService.Authenticate(this);
        if (todoService.User == null) {
            Console.WriteLine("couldn't login!!");
            return;
        }
    }
    // rest of RefreshAsync method
    ```

5. **açık AppDelegate.cs,** aşağıdaki yöntemi ekleyin:

    ```csharp
    public static Func<NSUrl, bool> ResumeWithURL;

    public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
    {
        return ResumeWithURL != null && ResumeWithURL(url);
    }
    ```

6. **Info.plist** dosyasını açın, **Gelişmiş** bölümündeki **URL Türleri'ne** gidin. Şimdi URL Türü'nüzün **Tanımlayıcısını** ve **URL Şemalarını** yapılandırın ve **URL Türü Ekle'yi**tıklatın. **URL Düzenleri** {url_scheme_of_your_app}'inizle aynı olmalıdır.
7. Mac için Mac Host veya Visual Studio'nuza bağlı Visual Studio'da, istemci projesini bir aygıtı veya emülatöre yönelik olarak çalıştırın. Uygulamanın veri görüntülemediğini doğrulayın.

    Giriş ekranının görünmesine neden olacak öğelerin listesini aşağı çekerek yenileme hareketi gerçekleştirin. Geçerli kimlik bilgilerini başarıyla girdikten sonra, uygulama todo öğeleri listesini görüntüler ve verilerde güncelleştirmeler yapabilirsiniz.

<!-- URLs. -->
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Yeni bir Xamarin.iOS uygulaması oluşturma]: app-service-mobile-xamarin-ios-get-started.md
