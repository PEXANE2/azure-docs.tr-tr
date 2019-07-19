---
title: Microsoft Identity platform UWP Başlarken | Mavisi
description: Evrensel Windows Platformu uygulamalar (UWP), Microsoft Identity platform uç noktası tarafından erişim belirteçleri gerektiren bir API 'YI nasıl çağırabilirler.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dae3f9f2f4a9f6966d3453123773887c244cf4e2
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334062"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Evrensel Windows Platformu uygulamasından Microsoft Graph API çağrısı (XAML)

> [!div renderon="docs"]

Bu kılavuzda, yerel bir Evrensel Windows Platformu (UWP) uygulamasının bir erişim belirteci isteme ve sonra Microsoft Graph API 'yi çağırma işlemleri açıklanmaktadır. Kılavuz, Microsoft Identity platform uç noktasından erişim belirteçleri gerektiren diğer API 'Ler için de geçerlidir.

Bu kılavuzun sonunda, uygulamanız kişisel hesapları kullanarak korumalı bir API çağırır. Örnekler şunlardır outlook.com, live.com ve diğerleri. Uygulamanız Ayrıca, Azure Active Directory (Azure AD) olan herhangi bir şirketten veya kuruluştan iş ve okul hesaplarını da çağırır.

>[!NOTE]
> Bu kılavuzda Evrensel Windows Platformu geliştirme yüklü Visual Studio 2017 gerekir. Evrensel Windows Platformu uygulamalar geliştirmek için Visual Studio 'Yu indirme ve yapılandırma [yönergeleri bölümüne bakın](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) .

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](./media/tutorial-v2-windows-uwp/uwp-intro.svg)

Bu kılavuz, Microsoft Identity platform uç noktasından belirteçleri kabul eden Microsoft Graph API 'sini veya Web API 'sini sorgulayan bir örnek UWP uygulaması oluşturur. Bu senaryo için, yetkilendirme üst bilgisi aracılığıyla HTTP isteklerine bir belirteç eklenir. Microsoft kimlik doğrulama kitaplığı (MSAL) belirteç alma ve yenileme işlemleri gerçekleştirir.

## <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuzda aşağıdaki NuGet paketleri kullanılmaktadır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library|

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölümde, *Microsoft Ile oturum açma*Ile bir Windows Masaüstü .NET uygulaması 'Nı (XAML) bütünleştirmek için adım adım yönergeler sağlanmaktadır. Daha sonra, Microsoft Graph API gibi bir belirteç gerektiren Web API 'Lerini sorgulayabilir.

Bu kılavuz Graph API, oturum kapatma düğmesini ve çağrıların sonuçlarını görüntüleyen metin kutularını sorgulayan bir düğme görüntüleyen bir uygulama oluşturur.

> [!NOTE]
> Bunun yerine bu örnek Visual Studio projesini indirmek istiyor musunuz? [Bir proje indirin](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip) ve kod örneğini çalıştırılmadan önce yapılandırmak için [uygulama kaydı](#register-your-application "uygulama kaydı adımı") adımına atlayın.

### <a name="create-your-application"></a>Uygulamanızı oluşturma

1. Visual Studio'da **dosya** > **yeni** > **proje**.
2. **Şablonlar**altında **görsel C#** ' i seçin.
3. **Boş Uygulama (Evrensel Windows)** seçeneğini belirleyin.
4. Uygulamayı adlandırın ve **Tamam**' ı seçin.
5. İstenirse, **hedef** ve **En düşük** sürümler için herhangi bir sürüm seçin ve **Tamam**' ı seçin.

    >![En düşük ve hedef sürümler](./media/tutorial-v2-windows-uwp/vs-minimum-target.png)

## <a name="add-microsoft-authentication-library-to-your-project"></a>Projenize Microsoft kimlik doğrulama Kitaplığı ekleyin
1. Visual Studio’da **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**’nu seçin.
2. Aşağıdaki komutu kopyalayıp **Paket Yöneticisi konsol** penceresine yapıştırın:

    ```powershell
    Install-Package Microsoft.Identity.Client -IncludePrerelease
    ```

> [!NOTE]
> Bu komut, [Microsoft kimlik doğrulama kitaplığı](https://aka.ms/msal-net)'nı kurar. MSAL Microsoft Identity platform tarafından korunan API 'Lere erişen Kullanıcı belirteçlerini alır, önbelleğe alır ve yeniler.

## <a name="create-your-applications-ui"></a>Uygulamanızın Kullanıcı arabirimini oluşturma

Bir **MainPage. xaml** dosyası, proje şablonunuzun bir parçası olarak otomatik olarak oluşturulur. Bu dosyayı açın ve ardından yönergeleri izleyin:

* Uygulamanızın **Grid** düğümünü şu kodla değiştirin:

    ```xml
    <Grid>
        <StackPanel Background="Azure">
            <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
                <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
                <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
            </StackPanel>
            <TextBlock Text="API Call Results" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
            <TextBlock Text="Token Info" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
        </StackPanel>
    </Grid>
    ```
    
## <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>Microsoft Graph API belirteci almak için MSAL kullanma

Bu bölümde, Microsoft Graph API 'sinin bir belirtecini almak için MSAL nasıl kullanılacağı gösterilmektedir.

1.  **MainPage.xaml.cs**' de, msal için başvuruyu sınıfına ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. <code>MainPage</code> Sınıfınızın kodunu aşağıdaki kodla değiştirin:

    ```csharp
    public sealed partial class MainPage : Page
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };

        // Below are the clientId (Application Id) of your app registration and the tenant information. 
        // You have to replace:
        // - the content of ClientID with the Application Id for your app registration
        // - Te content of Tenant by the information about the accounts allowed to sign-in in your application:
        //   - For Work or School account in your org, use your tenant ID, or domain
        //   - for any Work or School accounts, use organizations
        //   - for any Work or School accounts, or Microsoft personal account, use common
        //   - for Microsoft Personal account, use consumers
        private const string ClientId = "0b8b0665-bc13-4fdc-bd72-e0227b9fc011";        

        public IPublicClientApplication PublicClientApp { get; } 

        public MainPage()
        {
          this.InitializeComponent();

          PublicClientApp = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AadAuthorityAudience.AzureAdAndPersonalMicrosoftAccount)
                .WithLogging((level, message, containsPii) =>
                {
                    Debug.WriteLine($"MSAL: {level} {message} ");
                }, LogLevel.Warning, enablePiiLogging:false,enableDefaultPlatformLogging:true)
                .WithUseCorporateNetwork(true)
                .Build();
        }

        /// <summary>
        /// Call AcquireTokenInteractive - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
         AuthenticationResult authResult = null;
         ResultText.Text = string.Empty;
         TokenInfoText.Text = string.Empty;

         // It's good practice to not do work on the UI thread, so use ConfigureAwait(false) whenever possible.            
         IEnumerable<IAccount> accounts = await PublicClientApp.GetAccountsAsync().ConfigureAwait(false); 
         IAccount firstAccount = accounts.FirstOrDefault();

         try
         {
          authResult = await PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                                  .ExecuteAsync();
         }
         catch (MsalUiRequiredException ex)
         {
          // A MsalUiRequiredException happened on AcquireTokenSilent.
          // This indicates you need to call AcquireTokenInteractive to acquire a token
          System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

          try
          {
           authResult = await PublicClientApp.AcquireTokenInteractive(scopes)
                                                      .ExecuteAsync()
                                                      .ConfigureAwait(false);
           }
           catch (MsalException msalex)
           {
            await DisplayMessageAsync($"Error Acquiring Token:{System.Environment.NewLine}{msalex}");
           }
          }
          catch (Exception ex)
          {
           await DisplayMessageAsync($"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}");
           return;
          }

          if (authResult != null)
          {
           var content = await GetHttpContentWithToken(graphAPIEndpoint,
                                                       authResult.AccessToken).ConfigureAwait(false);

           // Go back to the UI thread to make changes to the UI
           await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
           {
            ResultText.Text = content;
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
           });
          }
        }
    ```

### <a name="more-information"></a>Daha fazla bilgi

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Yöntemine yapılan bir çağrı `AcquireTokenInteractive` , kullanıcılardan oturum açmasını isteyen bir pencereyle sonuçlanır. Uygulamalar genellikle kullanıcıların, korunan kaynağa ilk kez erişmesi gerektiğinde etkileşimli olarak oturum açmasını gerektirir. Ayrıca, bir belirteci almak için sessiz bir işlem başarısız olduğunda oturum açması gerekebilir. Bir Kullanıcı parolasının ne zaman dolduğunu bir örnektir.

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntemi `AcquireTokenSilent` , Kullanıcı etkileşimi olmadan belirteç alma ve yenileme işlemleri gerçekleştirir. İlk `AcquireTokenInteractive` kez yürütüldükten sonra ve kullanıcıdan kimlik bilgileri istendiğinde `AcquireTokenSilent` , bu yöntem belirteçleri sessizce elde ettiğinden sonraki çağrılar için belirteç istemek üzere kullanılmalıdır. MSAL, belirteç önbelleğini ve yenilemeyi işleyecek.

Sonuç olarak, `AcquireTokenSilent` yöntem başarısız olur. Hatanın nedeni, kullanıcıların başka bir cihazda oturum açmış veya parolalarını değiştirmiş olması olabilir. MSAL, sorunun etkileşimli bir eylem gerektirerek çözümlenemeyeceğini algıladığında, bir `MsalUiRequiredException` özel durum harekete geçirilir. Uygulamanız bu özel durumu iki şekilde işleyebilir:

* `AcquireTokenInteractive` Anında bir çağrı yapabilir. Bu çağrı kullanıcıdan oturum açmasını ister. Normalde, bu model Kullanıcı için kullanılabilir çevrimdışı içerik bulunmayan çevrimiçi uygulamalarda kullanılır. Bu Kılavuzlu kurulum tarafından oluşturulan örnek, düzeni izler. Örneği ilk kez çalıştırdığınızda eylem olarak görürsünüz.
  * Hiçbir Kullanıcı uygulamayı kullanmadığından, `accounts.FirstOrDefault()` null bir değer içerir ve bir `MsalUiRequiredException` özel durum oluşturulur.
  * Daha sonra örnekteki kod, çağırarak `AcquireTokenInteractive`özel durumu işler. Bu çağrı kullanıcıdan oturum açmasını ister.

* Bunun yerine, etkileşimli bir oturum açma işlemi için kullanıcılara görsel bir gösterge sunar. Ardından, oturum açmak için doğru zamanı seçebilirsiniz. Ya da uygulama daha sonra `AcquireTokenSilent` yeniden deneyebilir. Genellikle, bu model kullanıcılar diğer uygulama işlevlerini kesintiye uğramadan kullanbiliyor olduğunda kullanılır. Uygulamada çevrimdışı içerik kullanılabilir olduğunda örnek bir örnektir. Bu durumda, kullanıcılar korunan kaynağa erişmek veya güncel olmayan bilgileri yenilemek için ne zaman oturum açmak istediğine karar verebilir. Aksi takdirde uygulama, geçici olarak devre dışı `AcquireTokenSilent` olduktan sonra ağ geri yüklendiğinde yeniden denemeye karar verebilir.

## <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Yeni Edindiğiniz belirteci kullanarak Microsoft Graph API çağrısı yapın

* Aşağıdaki yeni yöntemi **MainPage.xaml.cs**öğesine ekleyin. Bu yöntem, bir `GET` `Authorization` üst bilgi kullanarak Graph API karşı bir istek yapmak için kullanılır:

    ```csharp
    /// <summary>
    /// Perform an HTTP GET request to a URL using an HTTP Authorization header
    /// </summary>
    /// <param name="url">The URL</param>
    /// <param name="token">The token</param>
    /// <returns>String containing the results of the GET operation</returns>
    public async Task<string> GetHttpContentWithToken(string url, string token)
    {
        var httpClient = new System.Net.Http.HttpClient();
        System.Net.Http.HttpResponseMessage response;
        try
        {
            var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
            // Add the token in Authorization header
            request.Headers.Authorization = 
              new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
            response = await httpClient.SendAsync(request);
            var content = await response.Content.ReadAsStringAsync();
            return content;
        }
        catch (Exception ex)
        {
            return ex.ToString();
        }
    }
    ```

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Korunan bir API 'ye karşı REST çağrısı yapma hakkında daha fazla bilgi

Bu örnek uygulamada, `GetHttpContentWithToken` yöntemi, bir belirteç gerektiren korumalı bir kaynağa karşı http `GET` isteği oluşturmak için kullanılır. Sonra yöntem, içeriği çağırana döndürür. Bu yöntem, alınan belirteci **http yetkilendirme** üst bilgisine ekler. Bu örnekte, kaynak, kullanıcının profil bilgilerini görüntüleyen Microsoft Graph API **Me** uç noktasıdır.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Kullanıcının oturumunu kapatmak için bir yöntem ekleyin

* Kullanıcının oturumunu kapatmak için, **MainPage.xaml.cs**'e aşağıdaki yöntemi ekleyin:

    ```csharp
    /// <summary>
    /// Sign out the current user
    /// </summary>
    private async void SignOutButton_Click(object sender, RoutedEventArgs e)
    {
        IEnumerable<IAccount> accounts = await PublicClientApp.GetAccountsAsync
                                                              .ConfigureAwait(false);
        IAccount firstAccount = accounts.FirstOrDefault();

        try
        {
            await PublicClientApp.RemoveAsync(firstAccount).ConfigureAwait(false);
            await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
            {
                ResultText.Text = "User has signed-out";
                this.CallGraphButton.Visibility = Visibility.Visible;
                    this.SignOutButton.Visibility = Visibility.Collapsed;
                });
            }
            catch (MsalException ex)
            {
                ResultText.Text = $"Error signing-out user: {ex.Message}";
            }
        }
    ```

> [!NOTE]
> MSAL.net, belirteçleri almak veya hesapları yönetmek için zaman uyumsuz yöntemler kullanır ve bu nedenle, Kullanıcı arabirimi iş parçacığında UI ile yapılan eylemleri, bu `Dispatcher.RunAsync`nedenle ve çağrısı yapılacak önlemleri uygulamanız gerekir`ConfigureAwait(false)`

### <a name="more-information-on-sign-out"></a>Oturum kapatma hakkında daha fazla bilgi

`SignOutButton_Click` Yöntemi, kullanıcıyı msal Kullanıcı önbelleğinden kaldırır. Bu yöntem, MSAL 'in geçerli kullanıcıyı unutmasını etkin bir şekilde söyler. Ardından, belirteç alma isteği yalnızca etkileşimli olması durumunda başarılı olur.
Bu örnekteki uygulama, tek bir kullanıcıyı destekler. Ancak MSAL, aynı anda birden fazla hesabın imzalanabilmesi için senaryoları destekler. Örneğin, bir kullanıcının birkaç hesabı bulunan bir e-posta uygulaması.

## <a name="display-basic-token-information"></a>Temel belirteç bilgilerini görüntüle

* Belirteç hakkındaki temel bilgileri göstermek için **MainPage.xaml.cs** 'e aşağıdaki yöntemi ekleyin:

    ```csharp
    /// <summary>
    /// Display basic information contained in the token. Needs to be called from the UI thead.
    /// </summary>
    private void DisplayBasicTokenInfo(AuthenticationResult authResult)
    {
        TokenInfoText.Text = "";
        if (authResult != null)
        {
            TokenInfoText.Text += $"User Name: {authResult.Account.Username}" + Environment.NewLine;
            TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        }
    }
    ```

### <a name="more-information"></a>Daha fazla bilgi

**OpenID Connect** aracılığıyla alınan kimlik belirteçleri, kullanıcıya yönelik bir bilgi alt kümesini de içerir. `DisplayBasicTokenInfo`belirteçte bulunan temel bilgileri görüntüler. Kullanıcının görünen adı ve KIMLIĞI, belirtecin sona erme tarihi ve erişim belirtecinin kendisini temsil eden dize verilebilir. **MICROSOFT Graph API** düğmesini birkaç kez çağır ' ı seçerseniz, sonraki istekler için aynı belirtecin yeniden kullanılmış olduğunu görürsünüz. Ayrıca MSAL, belirtecin yenileme zamanına karar verdiğinde, süre sonu genişletilmiş tarihini de görebilirsiniz.

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Şimdi uygulamanızı Microsoft uygulama kayıt portalı 'na kaydetmeniz gerekir:

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısında mevcutsa, sayfanın üstündeki menüdeki sağ üst `Directory + Subscription` köşedeki ' ı seçin ve Portal oturumunuzu istenen Azure AD kiracısına geçirin.
1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
1. **Yeni kayıt**seçeneğini belirleyin.
   - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `UWP-App-calling-MSGraph`.
   - **Desteklenen hesap türleri** bölümünde, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında (ör. Skype, Xbox, Outlook.com) hesaplar**' ı seçin.
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamaya **genel bakış** sayfasında, **uygulama (istemci) kimlik** değerini bulun ve daha sonra için kaydedin. Visual Studio 'ya geri dönün, **MainPage.xaml.cs**açın ve ClientID değerini yeni KAYDETTIĞINIZ uygulama kimliğiyle değiştirin:
1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
   1. Yeniden yönlendirme **URI** 'leri bölümünde, yeniden yönlendirme URI 'leri listesinde:
   1. **Tür** sütununda **ortak istemci (mobil & Masaüstü)** öğesini seçin.
   1. `urn:ietf:wg:oauth:2.0:oob` **Yeniden yönlendirme URI** sütununa girin.
1. **Kaydet**’i seçin.
1. Uygulama için sayfa listesinde, **API izinleri** ' ni seçin.
   - **Izin Ekle** düğmesine tıklayın ve ardından
   - **MICROSOFT API** 'sinin sekmesinin seçildiğinden emin olun
   - *Yaygın olarak kullanılan Microsoft API 'leri* bölümünde, **Microsoft Graph** ' ye tıklayın.
   - **Temsilci izinleri** bölümünde, doğru izinlerin işaretli olduğundan emin olun: **User. Read**. Gerekirse arama kutusunu kullanın.
   - **Izin Ekle** düğmesini seçin

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Federasyon etki alanlarında tümleşik kimlik doğrulamasını etkinleştirme (isteğe bağlı)

Windows ile tümleşik kimlik doğrulamasını, Federe bir Azure AD etki alanı ile kullanıldığında etkinleştirmek için, uygulama bildiriminin ek özellikleri etkinleştirmesi gerekir:

1. **Package. appxmanifest**öğesine çift tıklayın.
2. **Yetenekler** sekmesini seçin ve aşağıdaki ayarların etkinleştirildiğinden emin olun:

    - Kurumsal kimlik doğrulama
    - Özel ağlar (Istemci & sunucusu)
    - Paylaşılan Kullanıcı sertifikaları

> [!IMPORTANT]
> [Tümleşik Windows kimlik doğrulaması](https://aka.ms/msal-net-iwa) Bu örnek için varsayılan olarak yapılandırılmamıştır. *Kurumsal kimlik doğrulaması* veya *Paylaşılan Kullanıcı sertifikaları* özelliği ısteyen uygulamalar, Windows Mağazası tarafından daha yüksek bir doğrulama düzeyi gerektirir. Ayrıca, tüm geliştiriciler daha yüksek doğrulama düzeyini gerçekleştirmek istemekdedir. Bu ayarı yalnızca, Federe bir Azure AD etki alanı ile Windows tümleşik kimlik doğrulaması gerekiyorsa etkinleştirin.

## <a name="test-your-code"></a>Kodunuzu test etme

Uygulamanızı test etmek için F5 ' i seçerek projenizi Visual Studio 'da çalıştırın. Ana pencereniz görüntülenir:

![Uygulamanın kullanıcı arabirimi](./media/tutorial-v2-windows-uwp/testapp-ui.png)

Sınamaya hazırsanız **MICROSOFT Graph API çağrısı**' nı seçin. Ardından, oturum açmak için bir Azure AD kuruluş hesabı veya live.com veya outlook.com gibi bir Microsoft hesabı kullanın. İlk kez karşılaşırsanız, kullanıcıdan oturum açmasını isteyen bir pencere görürsünüz:

![Oturum açma sayfası](./media/tutorial-v2-windows-uwp/sign-in-page.png)

### <a name="consent"></a>İzniniz

Uygulamanızda ilk kez oturum açtığınızda aşağıdakine benzer bir izin ekranı sunulur. Erişime açık olarak izin vermek için **Evet** ' i seçin:

![Erişim izni ekranı](./media/tutorial-v2-windows-uwp/consentscreen.png)

### <a name="expected-results"></a>Beklenen sonuçlar

**API çağrı sonuçları** EKRANıNDA Microsoft Graph API çağrısı tarafından döndürülen Kullanıcı profili bilgilerini görürsünüz:

![API çağrı sonuçları ekranı](./media/tutorial-v2-windows-uwp/uwp-results-screen.PNG)

Ayrıca, `AcquireTokenInteractive` **belirteç bilgisi** kutusunda veya `AcquireTokenSilent` aracılığıyla alınan belirteçle ilgili temel bilgileri görürsünüz:

|Özellik  |Biçimi  |Açıklama |
|---------|---------|---------|
|**Kullanıcı Adı** |<span>user@domain.com</span> |Kullanıcıyı tanımlayan Kullanıcı adı.|
|**Belirtecin süresi doluyor** |Datetime |Belirtecin süresinin dolacağı zaman. MSAL, belirteci gerektiği şekilde yenileyerek sona erme tarihini genişletir.|

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API 'SI, kullanıcının profilini okumak için *Kullanıcı. Read* kapsamını gerektirir. Bu kapsam, uygulama kayıt portalı 'nda kayıtlı olan her uygulamada varsayılan olarak otomatik olarak eklenir. Microsoft Graph için diğer API 'Ler ve arka uç sunucunuz için özel API 'Ler, ek kapsamlar gerektirebilir. Microsoft Graph API 'SI, kullanıcıların takvimlerini listelemek için *takvimler. Read* Scope 'ı gerektirir.

Kullanıcının takvimlerine bir uygulama bağlamında erişmek için, *takvimler.* uygulama kayıt bilgilerine, temsilci olarak oku iznini ekleyin. Ardından *takvimlere. Read* kapsamını `acquireTokenSilent` çağrıya ekleyin.

> [!NOTE]
> Kapsam sayısını artırdıkça kullanıcılara ek yarışlar istenebilir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="issue-1"></a>Sorun 1

Uygulamanızda bir federasyon Azure AD etki alanında oturum açtığınızda aşağıdaki hata iletilerinden birini alırsınız:

* İstekte geçerli bir istemci sertifikası bulunamadı.
* Kullanıcının sertifika deposunda geçerli sertifika bulunamadı.
* Farklı bir kimlik doğrulama yöntemi seçmeyi yeniden deneyin.

**Sağlamak** Kuruluş ve sertifika özellikleri etkin değil.

**Çözümden** [Federasyon etki alanlarında tümleşik kimlik doğrulaması](#enable-integrated-authentication-on-federated-domains-optional)bölümündeki adımları izleyin.

### <a name="issue-2"></a>Sorun 2

[Federasyon etki alanlarında tümleşik kimlik doğrulamasını](#enable-integrated-authentication-on-federated-domains-optional) etkinleştirir ve Multi-Factor Authentication yapılandırılmış bir ortamda oturum açmak için Windows 10 bilgisayarında Windows Hello 'yu kullanmayı deneyin. Sertifika listesi sunulur. Ancak PIN 'inizi kullanmayı seçerseniz, PIN penceresi hiçbir şekilde sunulmaz.

**Sağlamak** Bu sorun, Windows 10 Masaüstü 'nde çalışan UWP uygulamalarında Web kimlik doğrulama aracısının bilinen bir sınırlamasıdır. Windows 10 Mobile üzerinde sorunsuz bir şekilde çalışıyor.

**Sorunu** **Diğer seçeneklerle oturum aç '** ı seçin. Ardından **Kullanıcı adı ve parolayla oturum aç '** ı seçin. **Parolanızı belirtin**' i seçin. Ardından telefon kimlik doğrulama sürecini gözden geçin.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
