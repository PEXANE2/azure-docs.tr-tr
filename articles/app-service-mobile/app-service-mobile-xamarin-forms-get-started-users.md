---
title: Xamarin Forms uygulamasında kimlik doğrulamaya başlayın
description: Xamarin Forms uygulamanızın kullanıcılarını AAD, Google, Facebook, Twitter ve Microsoft gibi kimlik sağlayıcılarıyla doğrulamak için Mobil Uygulamaları nasıl kullanacağınızı öğrenin.
ms.assetid: 9c55e192-c761-4ff2-8d88-72260e9f6179
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 4788aa50074016a34d906353f5b37dbba85ef104
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77458775"
---
# <a name="add-authentication-to-your-xamarin-forms-app"></a>Xamarin Forms uygulamanıza kimlik doğrulama ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Genel Bakış
Bu konu, bir App Service Mobil Uygulamasının kullanıcılarını istemci uygulamanızdan nasıl doğruladığınızı gösterir. Bu öğreticide, Uygulama Hizmeti tarafından desteklenen bir kimlik sağlayıcısını kullanarak Xamarin Forms hızlı başlatma projesine kimlik doğrulama eklersiniz. Mobil Uygulamanız tarafından başarıyla doğrulandıktan ve onaylandıktan sonra, kullanıcı kimliği değeri görüntülenir ve kısıtlı tablo verilerine erişebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici ile en iyi sonuç için, öncelikle [Bir Xamarin Forms uygulama][1] öğretici oluştur'u tamamlamanızı öneririz. Bu öğreticiyi tamamladıktan sonra, çok platformlu bir TodoList uygulaması olan bir Xamarin Forms projesine sahip olacaksınız.

İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, kimlik doğrulama uzantısı paketini projenize eklemeniz gerekir. Sunucu uzantı paketleri hakkında daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma][2]'ya bakın.

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Uygulamanızı kimlik doğrulama ve uygulama hizmetlerini yapılandırmak için kaydedin
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a><a name="redirecturl"></a>Uygulamanızı İzin Verilen Harici Yönlendirme URL'lerine ekleyin

Güvenli kimlik doğrulaması, uygulamanız için yeni bir URL düzeni tanımlamanızı gerektirir. Bu, kimlik doğrulama işlemi tamamlandıktan sonra kimlik doğrulama sisteminin uygulamanıza geri yönlendirmesine olanak tanır. Bu öğreticide, URL şeması _ekadını_ sonuna kadar kullanırız. Ancak, seçtiğiniz herhangi bir URL düzenini kullanabilirsiniz. Mobil uygulamanıza özgü olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. Azure [portalında][8]Uygulama Hizmetinizi seçin.

2. Kimlik **Doğrulama / Yetkilendirme** menüsü seçeneğini tıklatın.

3. İzin **Verilen Harici Yönlendirme URL'lerine**girin. `url_scheme_of_your_app://easyauth.callback`  Bu dizedeki **url_scheme_of_your_app,** mobil uygulamanızın URL Düzenidir.  Bir protokol için normal URL belirtimini izlemeli (yalnızca harfleri ve sayıları kullanın ve harfle başlamalıdır).  Mobil uygulama kodunuzu URL Düzeni ile birkaç yerde ayarlamanız gerektiğinden, seçtiğiniz dizeye not alabilirsiniz.

4. **Tamam**'a tıklayın.

5. **Kaydet**'e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a>İzinleri kimlik doğrulaması verilen kullanıcılarla sınırlama
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-to-the-portable-class-library"></a>Taşınabilir sınıf kitaplığına kimlik doğrulama ekleme
Mobil Uygulamalar, Uygulama Hizmeti kimlik doğrulaması olan bir kullanıcıyı oturum açmak için [MobileServiceClient'daki][4] [LoginAsync][3] uzantısı yöntemini kullanır. Bu örnek, sağlayıcının oturum açma arabirimini uygulamada görüntüleyen sunucu tarafından yönetilen bir kimlik doğrulama akışı kullanır. Daha fazla bilgi için [Sunucu tarafından yönetilen kimlik doğrulamaya][5]bakın. Üretim uygulamanızda daha iyi bir kullanıcı deneyimi sağlamak için, bunun yerine [İstemci tarafından yönetilen kimlik doğrulamasını][6]kullanmayı düşünmelisiniz.

Bir Xamarin Forms projesiyle kimlik doğrulaması yapmak için uygulama için Taşınabilir Sınıf Kitaplığı'nda bir **Kimlik Doğrulama** arabirimi tanımlayın. Ardından, kimlik doğrulamayı başlatmak için tıklattığınızda Taşınabilir Sınıf Kitaplığı'nda tanımlanan kullanıcı arabirimine **oturum** açma düğmesi ekleyin. Veriler, başarılı kimlik doğrulamasından sonra mobil uygulama arka ucundan yüklenir.

Uygulamanız tarafından desteklenen her platform için **IAuthenticate** arabirimini uygulayın.

1. Visual Studio veya Xamarin Studio'da, Portable Class Library projesi olan **Portable** ile projeden `using` App.cs açın, ardından aşağıdaki ifadeyi ekleyin:

        using System.Threading.Tasks;
2. App.cs, sınıf tanımından hemen önce aşağıdaki `IAuthenticate` arabirim tanımını `App` ekleyin.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. Arabirimi platforma özgü bir uygulamayla başlatmayı sağlamak için **Uygulama** sınıfına aşağıdaki statik üyeleri ekleyin.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. Taşınabilir Sınıf Kitaplığı projesinden TodoList.xaml'ı açın, varolan düğmeden sonra *düğmelere Panel* düzen öğesine aşağıdaki **Düğme** öğesini ekleyin:

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    Bu düğme, mobil uygulama arka ucunuzla sunucu tarafından yönetilen kimlik doğrulamasını tetikler.
5. Taşınabilir Sınıf Kitaplığı projesinden TodoList.xaml.cs açın ve `TodoList` ardından sınıfa aşağıdaki alanı ekleyin:

        // Track whether the user has authenticated.
        bool authenticated = false;
6. **OnAppearing** yöntemini aşağıdaki kodla değiştirin:

        protected override async void OnAppearing()
        {
            base.OnAppearing();

            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Bu kod, verilerin yalnızca siz doğrulandıktan sonra hizmetten yenilenmesini sağlar.
7. **Tıklanan** olay için aşağıdaki işleyiciyi **TodoList** sınıfına ekleyin:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. Değişikliklerinizi kaydedin ve hiçbir hata doğrulamayla Taşınabilir Sınıf Kitaplığı projesini yeniden oluşturun.

## <a name="add-authentication-to-the-android-app"></a>Android uygulamasına kimlik doğrulama ekleme
Bu bölümde, Android uygulama projesinde **IAuthenticate** arabiriminin nasıl uygulanacağı gösterilmektedir. Android cihazları desteklemiyorsanız bu bölümü atlayın.

1. Visual Studio veya Xamarin Studio'da **droid** projesini sağ tıklatın, ardından **StartUp Project olarak ayarlayın.**
2. Projeyi hata ayıklayıcıda başlatmak için F5 tuşuna basın ve uygulama başladıktan sonra 401 (Yetkisiz) durum kodu içeren işlenmemiş bir özel durumun yükseltilmeye başladığını doğrulayın. Arka uçtaki erişim yalnızca yetkili kullanıcılarla sınırlı olduğundan 401 kodu üretilir.
3. Android projesinde MainActivity.cs açın ve `using` aşağıdaki ifadeleri ekleyin:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. **IAuthenticate** arabirimini uygulamak için **MainActivity** sınıfını aşağıdaki gibi güncelleştirin:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. Bir **MobileServiceUser** alanı ve **IAuthenticate** arabirimi tarafından gerekli olan **Bir Authenticate** yöntemi ekleyerek **MainActivity** sınıfını aşağıdaki gibi güncelleştirin:

        // Define an authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this, 
                    MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }

    Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, [MobileServiceAuthenticationProvider][7]için farklı bir değer seçin.

6. **AndroidManifest.xml** dosyasını öğenin içine aşağıdaki `<application>` XML'yi ekleyerek güncelleyin:

    ```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
      </intent-filter>
    </activity>
    ```
    URL `{url_scheme_of_your_app}` şemanla değiştirin.
7. Aramadan önce **MainActivity** sınıfının `LoadApplication()` **OnCreate** yöntemine aşağıdaki kodu ekleyin:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Bu kod, kimlik doğrulayıcının uygulama yükedilmeden önce başlatılmasını sağlar.
8. Uygulamayı yeniden oluşturun, çalıştırın ve ardından seçtiğiniz kimlik doğrulama sağlayıcısıyla oturum açın ve kimlik doğrulaması yapılan bir kullanıcı olarak verilere erişebildiğinizi doğrulayın.

### <a name="troubleshooting"></a>Sorun giderme

**Uygulama ile çöktü`Java.Lang.NoSuchMethodError: No static method startActivity`**

Bazı durumlarda, destek paketlerindeki çakışmalar Visual stüdyosunda sadece bir uyarı olarak görüntülenir, ancak uygulama çalışma zamanında bu özel durumla birlikte çöker. Bu durumda, projenizde başvurulan tüm destek paketlerinin aynı sürüme sahip olduğundan emin olmanız gerekir. [Azure Mobile Apps NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), Android platformu için `Xamarin.Android.Support.CustomTabs` bağımlılığına sahiptir, yani projeniz daha yeni destek paketleri kullanıyorsa, çakışmaları önlemek için doğrudan gerekli sürüme sahip bu paketi yüklemeniz gerekir.

## <a name="add-authentication-to-the-ios-app"></a>iOS uygulamasına kimlik doğrulama ekleme
Bu bölümde, iOS uygulama projesinde **IAuthenticate** arabiriminin nasıl uygulanacağı gösterilmektedir. iOS aygıtlarını desteklemiyorsanız bu bölümü atlayın.

1. Visual Studio veya Xamarin Studio'da **iOS** projesini sağ tıklatın ve ardından **StartUp Project olarak ayarlayın.**
2. Projeyi hata ayıklayıcıda başlatmak için F5 tuşuna basın ve uygulama başladıktan sonra 401 (Yetkisiz) durum kodu içeren işlenmemiş bir özel durumun yükseltilmeye başladığını doğrulayın. Arka uçtaki erişim yalnızca yetkili kullanıcılarla sınırlı olduğundan 401 yanıtı üretilir.
3. iOS projesinde AppDelegate.cs açın ve `using` aşağıdaki ifadeleri ekleyin:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. **IAuthenticate** arabirimini uygulamak için **AppDelegate** sınıfını aşağıdaki gibi güncelleştirin:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. Bir **MobileServiceUser** alanı ve **IAuthenticate** arabirimi tarafından gerekli olan **Kimlik Doğrulama** yöntemini ekleyerek **AppDelegate** sınıfını aşağıdaki gibi güncelleştirin:

        // Define an authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;
                    }
                }
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertController avAlert = UIAlertController.Create("Sign-in result", message, UIAlertControllerStyle.Alert);
            avAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
            UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(avAlert, true, null);

            return success;
        }

    Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, [MobileServiceAuthenticationProvider] için farklı bir değer seçin.
    
6. **OpenUrl** yöntemiaşırı yükleyerek **AppDelegate** sınıfını aşağıdaki gibi güncelleştirin:

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }
   
7. Aramadan önce **FinishedLaunching** yöntemine `LoadApplication()`aşağıdaki kod satırını ekleyin:

        App.Init(this);

    Bu kod, uygulama yüklenmeden önce kimlik doğrulayıcının başlatılmasını sağlar.

8. Info.plist'i açın ve **bir URL Türü**ekleyin. **Tanımlayıcıyı** seçtiğiniz bir ada, **URL Şemaları'nı** uygulamanızın URL şemasına ve **Rolü** Yok'a göre ayarlayın.

9. Uygulamayı yeniden oluşturun, çalıştırın ve ardından seçtiğiniz kimlik doğrulama sağlayıcısıyla oturum açın ve kimlik doğrulaması yapılan bir kullanıcı olarak verilere erişebildiğinizi doğrulayın.

## <a name="add-authentication-to-windows-10-including-phone-app-projects"></a>Windows 10 (Telefon dahil) uygulama projelerine kimlik doğrulama ekleme
Bu bölümde, Windows 10 uygulama projelerinde **IAuthenticate** arabiriminin nasıl uygulanacağı gösterilmektedir. Aynı adımlar Evrensel Windows Platformu (UWP) projeleri için de geçerlidir, ancak **UWP** projesini (önemli değişikliklerle) kullanır. Windows aygıtlarını desteklemiyorsanız bu bölümü atlayın.

1. Visual Studio'da, **UWP** projesine sağ tıklayın, ardından **StartUp Project olarak ayarlayın.**
2. Projeyi hata ayıklayıcıda başlatmak için F5 tuşuna basın ve uygulama başladıktan sonra 401 (Yetkisiz) durum kodu içeren işlenmemiş bir özel durumun yükseltilmeye başladığını doğrulayın. 401 yanıtı, arka uçtaki erişim yalnızca yetkili kullanıcılarla sınırlı olduğundan gerçekleşir.
3. Windows uygulama projesi için MainPage.xaml.cs açın `using` ve aşağıdaki ifadeleri ekleyin:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Taşınabilir `<your_Portable_Class_Library_namespace>` sınıf kitaplığınız için ad alanıyla değiştirin.
4. **IAuthenticate** arabirimini uygulamak için **MainPage** sınıfını aşağıdaki gibi güncelleştirin:

        public sealed partial class MainPage : IAuthenticate
5. Bir **MobileServiceUser** alanı ve **IAuthenticate** arabirimi tarafından gerekli olan **Bir Authenticate** yöntemi ekleyerek **MainPage** sınıfını aşağıdaki gibi güncelleştirin:

        // Define an authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }

            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }

    Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, [MobileServiceAuthenticationProvider][7]için farklı bir değer seçin.

1. Aramadan önce **MainPage** sınıfının oluşturucuya aşağıdaki kod `LoadApplication()`satırını ekleyin:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    Taşınabilir `<your_Portable_Class_Library_namespace>` sınıf kitaplığınız için ad alanıyla değiştirin.

3. **UWP**kullanıyorsanız, **Uygulama** sınıfına aşağıdaki **OnActivated** yöntemi geçersiz kılınmasını ekleyin:

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                MobileServiceClientExtensions.ResumeWithURL(TodoItemManager.DefaultManager.CurrentClient,protocolArgs.Uri);
            }
       }

3. Package.appxmanifest'i açın ve **protokol** bildirimi ekleyin. Görüntü **adını** seçtiğiniz bir ada ve uygulamanız için URL düzenine **Ad'ı** ayarlayın.

4. Uygulamayı yeniden oluşturun, çalıştırın ve ardından seçtiğiniz kimlik doğrulama sağlayıcısıyla oturum açın ve kimlik doğrulaması yapılan bir kullanıcı olarak verilere erişebildiğinizi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi bu temel kimlik doğrulama öğretici tamamladı, aşağıdaki öğreticiler birine devam düşünün:

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-xamarin-forms-get-started-push.md)

  Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  Mobil Uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme, ağ bağlantısı olmasa bile son kullanıcıların bir mobil uygulamayla (verileri görüntüleme, ekleme veya değiştirme) etkileşimde bulunmasına olanak tanır.

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
[8]: https://portal.azure.com
