---
title: UWP uygulamanıza kimlik doğrulama ekleme
description: Evrensel Windows Platformu (UWP) uygulamanızın kullanıcılarını AAD, Google, Facebook, Twitter ve Microsoft gibi kimlik sağlayıcılarıyla doğrulamak için Azure App Service Mobile Apps'ı nasıl kullanacağınızı öğrenin.
ms.assetid: 6cffd951-893e-4ce5-97ac-86e3f5ad9466
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 32d4313b345964a2db13d68e83f81756a4acf0d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77458945"
---
# <a name="add-authentication-to-your-windows-app"></a>Windows uygulamanıza kimlik doğrulama ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Genel Bakış
Bu konu, mobil uygulamanıza bulut tabanlı kimlik doğrulamayı nasıl ekleyeceğinizi gösterir. Bu eğitimde, Azure Uygulama Hizmeti tarafından desteklenen bir kimlik sağlayıcısı nı kullanarak Mobil Uygulamalar için Evrensel Windows Platformu (UWP) hızlı başlatma projesine kimlik doğrulama eklersiniz. Mobil Uygulama arka ucunuz tarafından başarıyla doğrulanmış ve yetkilendirilen kullanıcı kimliği değeri görüntülenir.

Bu öğretici, Mobil Uygulamalar hızlı başlatma dayanmaktadır. Öncelikle öğreticiyi tamamlamanız gerekir [Mobil Uygulamalar ile başlayın.](app-service-mobile-windows-store-dotnet-get-started.md)

## <a name="register-your-app-for-authentication-and-configure-the-app-service"></a><a name="register"></a>Uygulamanızı kimlik doğrulama için kaydedin ve Uygulama Hizmetini yapılandırın
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a><a name="redirecturl"></a>Uygulamanızı İzin Verilen Harici Yönlendirme URL'lerine ekleyin

Güvenli kimlik doğrulaması, uygulamanız için yeni bir URL düzeni tanımlamanızı gerektirir. Bu, kimlik doğrulama işlemi tamamlandıktan sonra kimlik doğrulama sisteminin uygulamanıza geri yönlendirmesine olanak tanır. Bu öğreticide, URL şeması _ekadını_ sonuna kadar kullanırız. Ancak, seçtiğiniz herhangi bir URL düzenini kullanabilirsiniz. Mobil uygulamanıza özgü olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. Azure [portalında](https://ms.portal.azure.com)Uygulama Hizmetinizi seçin.

2. Kimlik **Doğrulama / Yetkilendirme** menüsü seçeneğini tıklatın.

3. İzin **Verilen Harici Yönlendirme URL'lerine**girin. `url_scheme_of_your_app://easyauth.callback`  Bu dizedeki **url_scheme_of_your_app,** mobil uygulamanızın URL Düzenidir.  Bir protokol için normal URL belirtimini izlemeli (yalnızca harfleri ve sayıları kullanın ve harfle başlamalıdır).  Mobil uygulama kodunuzu URL Düzeni ile birkaç yerde ayarlamanız gerektiğinden, seçtiğiniz dizeye not alabilirsiniz.

4. **Kaydet**'e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a><a name="permissions"></a>İzinleri kimlik doğrulaması verilen kullanıcılarla sınırlama
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Şimdi, arka uca anonim erişimin devre dışı bırakıldığını doğrulayabilirsiniz. UWP uygulama projesi başlangıç projesi olarak ayarlanmışken, uygulamayı dağıtın ve çalıştırın; uygulama başladıktan sonra 401 (Yetkisiz) durum kodu içeren işlenmemiş bir özel durum ortaya sürüldü. Bu durum, uygulamanın Mobil Uygulama Kodunuza kimliği doğrulanmamış bir kullanıcı olarak erişmeye çalıştığından, ancak *TodoItem* tablosu artık kimlik doğrulaması gerektirdiğinden olur.

Ardından, Uygulama Hizmetinizden kaynak istemeden önce uygulamayı kullanıcıların kimliğini doğrulamak için güncellersiniz.

## <a name="add-authentication-to-the-app"></a><a name="add-authentication"></a>Uygulamaya kimlik doğrulama ekleme
1. UWP uygulama proje dosyasında MainPage.xaml.cs ve aşağıdaki kod parçacığı ekleyin:
   
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                message =
                    string.Format("You are now signed in - {0}", user.UserId);
   
                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }
   
    Bu kod, kullanıcının Facebook girişiyle kimlik doğruluğunu doğrular. Facebook dışında bir kimlik sağlayıcısı kullanıyorsanız, yukarıdaki **MobileServiceAuthenticationProvider** değerini sağlayıcınızın değeriyle değiştirin.
2. **MainPage.xaml.cs'da OnNavigatedTo()** yöntemini değiştirin. Ardından, uygulamaya kimlik doğrulamasını tetikleyen bir **Oturum Açma** düğmesi eklersiniz.

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. MainPage.xaml.cs aşağıdaki kod parçacığı ekleyin:
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. MainPage.xaml proje dosyasını açın, **Kaydet** düğmesini tanımlayan öğeyi bulun ve aşağıdaki kodla değiştirin:
   
        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>
5. App.xaml.cs aşağıdaki kod parçacığı ekleyin:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                Frame content = Window.Current.Content as Frame;
                if (content.Content.GetType() == typeof(MainPage))
                {
                    content.Navigate(typeof(MainPage), protocolArgs.Uri);
                }
            }
            Window.Current.Activate();
            base.OnActivated(args);
        }
6. Package.appxmanifest dosyasını açın, Kullanılabilir **Bildirimler** açılır listesinde **Bildirimler'e**gidin, **Protokol'ü** seçin ve **Ekle** düğmesini tıklatın. Şimdi **Protokol** bildiriminin **özelliklerini** yapılandırın. **Display adına**, görüntülemek istediğiniz adı uygulamanızın kullanıcılarına ekleyin. **Ad**olarak , {url_scheme_of_your_app}'nizi ekleyin.
7. Uygulamayı çalıştırmak için F5 tuşuna basın, **Oturum Aç** düğmesini tıklayın ve seçtiğiniz kimlik sağlayıcıyla uygulamada oturum açın. Oturum açmanız başarılı olduktan sonra, uygulama hatasız çalışır ve arka uçunuzu sorgulayıp verilerde güncellemeler yapabilirsiniz.

## <a name="store-the-authentication-token-on-the-client"></a><a name="tokens"></a>Kimlik doğrulama belirtecisini istemcide depolama
Önceki örnekte, istemcinin uygulama her başlatıldığında hem kimlik sağlayıcısına hem de Uygulama Hizmetiyle iletişime geçmesini gerektiren standart bir oturum açma gösterilmektedir. Bu yöntem verimsiz olmasının tek nedeni, birçok müşteri uygulamanızı aynı anda başlatmaya çalışırsa, kullanımla ilgili sorunlarla da karşınıza çıkabilir. Daha iyi bir yaklaşım, Uygulama Hizmetiniz tarafından döndürülen yetkilendirme belirteci önbelleğe almak ve sağlayıcı tabanlı oturum açma kullanmadan önce bunu kullanmaya çalışmaktır.

> [!NOTE]
> İstemci tarafından yönetilen veya hizmet tarafından yönetilen kimlik doğrulaması kullanıp kullanmadığınıza bakılmaksızın App Services tarafından verilen belirteci önbelleğe alabilirsiniz. Bu öğretici, hizmet tarafından yönetilen kimlik doğrulaması kullanır.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Sonraki adımlar
Şimdi bu temel kimlik doğrulama öğretici tamamladı, aşağıdaki öğreticiler birine devam düşünün:

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Mobil Uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların, ağ bağlantısı yokken dahi, mobil uygulama ile etkileşim kurmalarına &mdash;veri görüntüleme, ekleme ya da değiştirme&mdash; olanak tanır.

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
