---
title: Microsoft kimlik platformu UWP başlıyor | Azure
description: Evrensel Windows Platformu uygulamalarının (UWP) Microsoft kimlik platformu bitiş noktası tarafından erişim belirteçleri gerektiren bir API'yi nasıl çağırabileceği.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 12/13/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 205f4a27a7903bc5a1da3fc12e3d4a02b23f58cf
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80990931"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Evrensel Windows Platformu uygulamasından Microsoft Graph API'yi (XAML) arayın

> [!div renderon="docs"]

Bu kılavuz, yerel bir Evrensel Windows Platformu (UWP) uygulamasının erişim jetonunu nasıl isteyebileceğini açıklar. Uygulama daha sonra Microsoft Graph API çağırır. Kılavuz, Microsoft kimlik platformu bitiş noktasından erişim belirteçleri gerektiren diğer API'ler için de geçerlidir.

Bu kılavuzun sonunda, uygulamanız kişisel hesapları kullanarak korumalı bir API çağırır. Örnekler outlook.com, live.com ve diğerleridir. Uygulamanız ayrıca Azure Etkin Dizini (Azure AD) olan herhangi bir şirket veya kuruluşun iş ve okul hesaplarını da çağırır.

>[!NOTE]
> Bu kılavuz, Evrensel Windows Platformu geliştirme yüklü Visual Studio gerektirir. Bkz. Visual Studio'yı evrensel Windows Platformu uygulamaları geliştirmek için indirmek ve yapılandırmak için yönergeler için [hazırlanın.](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)

>[!NOTE]
> Microsoft kimlik platformunda yeniyseniz, [Evrensel Windows Platformu (UWP) uygulamasından Microsoft Graph API'yi Ara](quickstart-v2-uwp.md)ile başlamanızı öneririz.

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](./media/tutorial-v2-windows-uwp/uwp-intro.svg)

Bu kılavuz, Microsoft Graph API sorgulayan örnek bir UWP uygulaması oluşturur. Bu senaryo için, Yetkilendirme üstbilgisi kullanılarak HTTP isteklerine bir belirteç eklenir. Microsoft Kimlik Doğrulama Kitaplığı (MSAL), belirteç satın almalarını ve yenilemeleri işler.

## <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuzda aşağıdaki NuGet paketi kullanVet

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library|

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölümde, Windows Desktop .NET uygulamasını (XAML) Microsoft ile Oturum Açma ile tümleştirmek için adım adım yönergeler sağlanıyor. Ardından uygulama, Microsoft Graph API gibi belirteç gerektiren Web API'lerini sorgulayabilir.

Bu kılavuz, Grafik API'sini sorgulayan bir düğme ve oturum dan çıkma düğmesini görüntüleyen bir uygulama oluşturur. Ayrıca, aramaların sonuçlarını içeren metin kutularını da görüntüler.

> [!NOTE]
> Bu örneğin Visual Studio projesini oluşturmak yerine indirmek istiyor musunuz? [Bir proje indirin](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip) ve kod örneğini çalıştırmadan önce yapılandırmak için [uygulama kayıt](#register-your-application "başvuru kayıt adımı") adımına atlayın.

### <a name="create-your-application"></a>Uygulamanızı oluşturma

1. Visual Studio'u açın ve **yeni bir proje oluştur'u**seçin.
1. **Yeni bir proje oluştur'da**C# için Boş Uygulama **(Evrensel Windows)** seçin ve **İleri'yi**seçin.
1. **Yeni projenizi yapılandırın,** uygulamayı adlandırın ve **Oluştur'u**seçin.
1. İstenirse, **Yeni Evrensel Windows Platform Projesi'nde** **Hedef** ve **Minimum** sürümler için herhangi bir sürümü seçin ve **Tamam'ı**seçin.

   ![Minimum ve Hedef sürümleri](./media/tutorial-v2-windows-uwp/select-uwp-target-minimum.png)

### <a name="add-microsoft-authentication-library-to-your-project"></a>Projenize Microsoft Kimlik Doğrulama Kitaplığı ekleme

1. Visual Studio'da **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'nu**seçin.
1. **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu kopyalayıp yapıştırın:

    ```powershell
    Install-Package Microsoft.Identity.Client
    ```

   > [!NOTE]
   > Bu komut [Microsoft Kimlik Doğrulama Kitaplığı](https://aka.ms/msal-net)yükler. MSAL, Microsoft kimlik platformu tarafından korunan API'lere erişen kullanıcı belirteçlerini edinir, önbelleğe eder ve yeniler.

### <a name="create-your-applications-ui"></a>Uygulamanızın Kullanıcı Tarafından Kullanıcı Tarafından Kullanıcı Tarafından Karşıla'nın oluşturulması

Visual Studio, proje şablonunuzun bir parçası olarak *MainPage.xaml* oluşturur. Bu dosyayı açın ve ardından uygulamanızın **Kılavuz** düğümunu aşağıdaki kodla değiştirin:

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

### <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>Microsoft Graph API için belirteç almak için MSAL'ı kullanın

Bu bölümde, Microsoft Graph API için bir belirteç almak için MSAL nasıl kullanılacağı nı gösterir. *MainPage.xaml.cs* dosyasında değişiklik yapın.

1. *MainPage.xaml.cs*olarak, aşağıdaki başvuruları ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    using System.Diagnostics;
    using System.Threading.Tasks;
    ```

1. Sınıfınızı `MainPage` aşağıdaki kodla değiştirin:

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
        // - the content of Tenant with the information about the accounts allowed to sign in in your application:
        //   - for Work or School account in your org, use your tenant ID, or domain
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
    }
    ```

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma<a name="more-information"></a>

Yöntem, `AcquireTokenInteractive` kullanıcıların oturum açmasını gerektiren bir pencereyle sonuçlanır. Uygulamalar genellikle kullanıcıların korumalı bir kaynağa erişmek için etkileşimli olarak ilk kez oturum açmalarını gerektirir. Ayrıca, bir belirteci elde etmek için sessiz bir işlem başarısız olduğunda oturum açmaları gerekebilir. Buna bir örnek, bir kullanıcının parolanın süresinin dolmasıdır.

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntem, `AcquireTokenSilent` herhangi bir kullanıcı etkileşimi olmadan belirteç satın almave yenilemeleri işler. İlk `AcquireTokenInteractive` kez çalıştırıladıktan ve kullanıcıdan kimlik `AcquireTokenSilent` bilgilerini istedikten sonra, daha sonraki aramalar için belirteçleri istemek için yöntemi kullanın. Bu yöntem sessizce belirteçleri elde eder. MSAL belirteç önbelleği ve yenileme işler.

Sonunda, `AcquireTokenSilent` yöntem başarısız olur. Hata nedenleri arasında, başka bir aygıtta parolalarını imzalayan veya değiştiren bir kullanıcı yer almaktadır. MSAL sorunun etkileşimli bir eylem gerektirdiğini algıladığında, bir `MsalUiRequiredException` özel durum oluşturur. Uygulamanız bu özel durumu iki şekilde işleyebilir:

* Başvurunuz `AcquireTokenInteractive` hemen arar. Bu çağrı, kullanıcının oturum açmasını istenmesine neden olur. Normalde, kullanıcı için kullanılabilir çevrimdışı içerik bulunmayan çevrimiçi uygulamalar için bu yaklaşımı kullanın. Bu kılavuzlu kurulum tarafından oluşturulan örnek deseni izler. Örneği ilk çalıştırdığınızda iş başında görürsünüz.

   Hiçbir kullanıcı uygulamayı kullanmadığından, `accounts.FirstOrDefault()` null değeri içerdiğinden `MsalUiRequiredException` ve bir özel durum atamadığı için.

   Örnekteki kod daha sonra çağırarak `AcquireTokenInteractive`özel durumu işler. Bu çağrı, kullanıcının oturum açmasını istenmesine neden olur.

* Uygulamanız, kullanıcılara oturum açmaları gerektiğini gösteren görsel bir gösterge sunar. Daha sonra oturum açmaları için doğru zamanı seçebilirler. Uygulama daha sonra `AcquireTokenSilent` yeniden deneyebilirsiniz. Kullanıcılar kesintiye uğramadan diğer uygulama işlevlerini kullanabilecekse bu yaklaşımı kullanın. Uygulamada çevrimdışı içeriğin kullanılabilir olduğu bir örnektir. Bu durumda, kullanıcılar ne zaman oturum açmaya karar verebilir. Ağ geçici olarak `AcquireTokenSilent` kullanılamadıktan sonra uygulama yeniden denilebilir.

### <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Yeni elde ettiğiniz belirteci kullanarak Microsoft Graph API'yi arayın

*MainPage.xaml.cs*aşağıdaki yeni yöntemi ekleyin:

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

 Bu yöntem, `GET` bir `Authorization` üstbilgi kullanarak Grafik API bir istek yapar.

#### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Korunan API'ye karşı REST çağrısı yapma hakkında daha fazla bilgi

Bu örnek uygulamada, `GetHttpContentWithToken` yöntem bir `GET` belirteç gerektiren korumalı bir kaynağa karşı bir HTTP isteği yapmak. Daha sonra yöntem içeriği arayana döndürür. Bu **yöntem, http yetkilendirme** üstbilgisinde edinilmiş belirteci ekler. Bu örnek için kaynak, kullanıcının profil bilgilerini görüntüleyen Microsoft Graph API **me** bitiş noktasıdır.

### <a name="add-a-method-to-sign-out-the-user"></a>Kullanıcıyı oturum dank etmek için bir yöntem ekleme

Kullanıcıyı oturum dışı batmak için *MainPage.xaml.cs*aşağıdaki yöntemi ekleyin:

   ```csharp
   /// <summary>
   /// Sign out the current user
   /// </summary>
   private async void SignOutButton_Click(object sender, RoutedEventArgs e)
   {
       IEnumerable<IAccount> accounts = await PublicClientApp.GetAccountsAsync().ConfigureAwait(false);
       IAccount firstAccount = accounts.FirstOrDefault();

       try
       {
           await PublicClientApp.RemoveAsync(firstAccount).ConfigureAwait(false);
           await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
           {
               ResultText.Text = "User has signed out";
               this.CallGraphButton.Visibility = Visibility.Visible;
                   this.SignOutButton.Visibility = Visibility.Collapsed;
               });
           }
           catch (MsalException ex)
           {
               ResultText.Text = $"Error signing out user: {ex.Message}";
           }
       }
   ```

> [!NOTE]
> MSAL.NET belirteçleri elde etmek veya hesapları işlemek için eşzamanlı yöntemler kullanır. UI iş parçacığındaki UI eylemlerini desteklemeniz gerekir. Bu `Dispatcher.RunAsync` arama ve aramak için önlemler `ConfigureAwait(false)`nedenidir.

#### <a name="more-information-about-signing-out"></a>Oturumdan çıkma hakkında daha fazla bilgi<a name="more-information-on-sign-out"></a>

Yöntem, `SignOutButton_Click` kullanıcıyı MSAL kullanıcı önbelleğinden kaldırır. Bu yöntem, MSAL'a geçerli kullanıcıyı unutmasını etkin bir şekilde bildirir. Bir belirteci elde etmek için gelecekteki bir istek yalnızca etkileşimli yse başarılı olur.

Bu örnekteki uygulama tek bir kullanıcıyı destekler. MSAL, kullanıcının birden fazla hesapta oturum açabileceği senaryoları destekler. Bir örnek, kullanıcının birden fazla hesabı olan bir e-posta uygulamasıdır.

### <a name="display-basic-token-information"></a>Temel belirteç bilgilerini görüntüleme

Belirteç le ilgili temel bilgileri görüntülemek için *MainPage.xaml.cs* aşağıdaki yöntemi ekleyin:

   ```csharp
   /// <summary>
   /// Display basic information contained in the token. Needs to be called from the UI thread.
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

#### <a name="more-information"></a>Daha fazla bilgi<a name="more-information-1"></a>

**OpenID Connect** kullanılarak elde edilen kimlik belirteçleri, kullanıcıyla ilgili küçük bir bilgi alt kümesi de içerir. `DisplayBasicTokenInfo`belirtecinde bulunan temel bilgileri görüntüler. Bu bilgiler, kullanıcının görüntü adını ve kimliğini içerir. Ayrıca belirteç ve erişim belirteci kendisini temsil eden dize son kullanma tarihini içerir. Microsoft Graph **API'yi birkaç** kez ara seçeneğini belirlerseniz, aynı belirteci daha sonraki istekler için yeniden kullanıldığını görürsünüz. Ayrıca, MSAL belirteci yenileme zamanının geldiğinde son kullanma tarihinin uzatıldığını da görebilirsiniz.

### <a name="display-message"></a>İletiyi görüntüleme

*MainPage.xaml.cs*aşağıdaki yeni yöntemi ekleyin:

   ```csharp
   /// <summary>
   /// Displays a message in the ResultText. Can be called from any thread.
   /// </summary>
   private async Task DisplayMessageAsync(string message)
   {
        await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
            () =>
            {
                ResultText.Text = message;
            });
        }
   ```

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Şimdi başvurunuzu kaydetmeniz gerekir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Azure Active Directory** > **App kayıtlarını**seçin.
1. **Yeni kayıt**seçin. *UWP-App-calling-MSGraph*gibi uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
1. **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında (örneğin Skype, Xbox) Hesapları**seçin ve devam etmek için **Kaydol'u** seçin.
1. Genel bakış sayfasında, **Uygulama (istemci) kimlik** değerini bulun ve kopyalayın. Visual Studio'ya geri dön, *MainPage.xaml.cs*açın `ClientId` ve değerini bu değerle değiştirin.

Uygulamanız için kimlik doğrulamasını yapılandırın:

1. [Azure portalında](https://portal.azure.com), **Yönet**altında, **Kimlik Doğrulama'yı**seçin.
1. Yönlendirme **URI'lerinde** | **kamu istemcileri (mobil, masaüstü)** bölümü için **https://login.microsoftonline.com/common/oauth2/nativeclient**Önerilen Yönlendirme URL'leri, kontrol edin.
1. **Kaydet'i**seçin.

Uygulamanız için API izinlerini yapılandırın:

1. **Yönet**altında, **API izinlerini**seçin.
1. **İzin ekle'yi** seçin ve ardından **Microsoft API'lerini**seçtiğinizden emin olun.
1. **Microsoft Graph'ı**seçin.
1. **Temsilcileştirilmiş izinleri**seçin, *Kullanıcı'yı arayın.Kullanıcı.Read'in* seçildiğini okuyun ve doğrulayın. **User.Read**
1. Herhangi bir değişiklik yaptıysanız, bunları kaydetmek için **İzin Ekle'yi** seçin.

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Federe etki alanlarında tümleşik kimlik doğrulamasını etkinleştirme (isteğe bağlı)

Federe Azure AD etki alanıyla kullanıldığında Windows Tümleşik Kimlik Doğrulama'yı etkinleştirmek için uygulama bildiriminin ek özellikler sağlaması gerekir. Visual Studio'daki başvurunuza geri dön.

1. *Açık Paket.appxmanifest*.
1. **Yetenekleri** seçin ve aşağıdaki ayarları etkinleştirin:

   * **Kurumsal Kimlik Doğrulama**
   * **Özel Ağlar (İstemci & Sunucusu)**
   * **Paylaşılan Kullanıcı Sertifikaları**

> [!IMPORTANT]
> [Tümleşik Windows Kimlik Doğrulama](https://aka.ms/msal-net-iwa) bu örnek için varsayılan olarak yapılandırılmamıştır. İsteyen veya `Enterprise Authentication` `Shared User Certificates` yetenek isteyen uygulamalar, Windows Mağazası tarafından daha yüksek bir doğrulama düzeyi gerektirir. Ayrıca, tüm geliştiriciler daha yüksek doğrulama düzeyini gerçekleştirmek istemez. Bu ayarı yalnızca federe Azure AD etki alanı yla Windows Tümleşik Kimlik Doğrulaması'na ihtiyacınız varsa etkinleştirin.

## <a name="test-your-code"></a>Kodunuza test etme

Uygulamanızı test etmek için Visual Studio'da projenizi çalıştırmak için F5'i seçin. Ana pencereniz görüntülenir:

![Uygulamanın kullanıcı arabirimi](./media/tutorial-v2-windows-uwp/testapp-ui-vs2019.png)

Sınamaya hazır olduğunuzda, **Microsoft Graph API'sini arayın'ı**seçin. Oturum açabilmek için Azure AD kuruluş hesabı veya live.com veya outlook.com gibi bir Microsoft hesabı kullanın. Bir kullanıcı bunu ilk kez çalıştırınca, uygulama kullanıcıdan oturum açmasını isteyen bir pencere görüntüler.

### <a name="consent"></a>Izni

Başvurunuzda ilk oturum açğınızda, aşağıdakilere benzer bir onay ekranı yla birlikte sunulur. Erişimi açıkça kabul etmek için **Evet'i** seçin:

![Erişim onay ekranı](./media/tutorial-v2-windows-uwp/consentscreen-vs2019.png)

### <a name="expected-results"></a>Beklenen sonuçlar

**API Çağrı Sonuçları** ekranında Microsoft Graph API çağrısı tarafından döndürülen kullanıcı profili bilgilerini görürsünüz:

![API Çağrı Sonuçları ekranı](./media/tutorial-v2-windows-uwp/uwp-results-screen-vs2019.png)

Ayrıca, Belirteç Bilgileri kutusu `AcquireTokenInteractive` aracılığıyla `AcquireTokenSilent` veya **token Info** kutusunda edinilen belirteç le ilgili temel bilgileri de görürsünüz:

|Özellik  |Biçimlendir  |Açıklama |
|---------|---------|---------|
|`Username` |`user@domain.com` |Kullanıcıyı tanımlayan kullanıcı adı.|
|`Token Expires` |`DateTime` |Belirteç süresinin dolduğu saat. MSAL, belirteci gerektiği gibi yenileyerek son kullanma tarihini uzatar.|

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API, kapsamın bir kullanıcının profilini okumasını `user.read` gerektirir. Bu kapsam, Uygulama Kayıt Portalı'na kayıtlı olan her uygulamada varsayılan olarak eklenir. Microsoft Graph için diğer API'ler ve arka uç sunucunuz için özel API'ler ek kapsamlar gerektirebilir. Örneğin, Microsoft Graph API `Calendars.Read` kullanıcının takvimlerini listelemek için kapsamı gerektirir.

Bir uygulama bağlamında kullanıcının takvimlerine erişmek için, `Calendars.Read` uygulama kayıt bilgilerine temsilci izni ekleyin. Ardından `Calendars.Read` kapsamı `acquireTokenSilent` aramaya ekleyin.

> [!NOTE]
> Kapsam sayısını artırdıkça kullanıcılardan ek izinler istenebilir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="issue-1"></a>Sorun 1

Federe bir Azure AD etki alanında uygulamanızda oturum açışı yaptığınızda aşağıdaki hata iletilerinden birini alırsınız:

* İstekte geçerli bir istemci sertifikası bulunamadı.
* Kullanıcının sertifika deposunda geçerli sertifika bulunamadı.
* Farklı bir kimlik doğrulama yöntemi seçmeyi yeniden deneyin.

Neden: Kuruluş ve sertifika yetenekleri etkinleştirildi.

Çözüm: [Federe etki alanlarında (isteğe bağlı) tümleşik kimlik doğrulamasını etkinleştir](#enable-integrated-authentication-on-federated-domains-optional)adımlarını izleyin.

### <a name="issue-2"></a>Sorun 2

Federal [etki alanlarında tümleşik kimlik doğrulamasını](#enable-integrated-authentication-on-federated-domains-optional) etkinleştirin ve windows hello'yu windows 10 bilgisayarında kullanarak çok faktörlü kimlik doğrulamayapılandırılmış bir ortamda oturum açmayı deneyin. Sertifika listesi sunulur. Ancak, PIN'inizi kullanmayı seçerseniz, PIN penceresi hiçbir zaman sunulmaz.

Neden: Bu sorun, Windows 10 masaüstünde çalışan UWP uygulamalarında web kimlik doğrulama aracısının bilinen bir sınırlamasıdır. Windows 10 Mobile'da iyi çalışır.

Geçici Çözüm: **Diğer seçeneklerle Oturum Aç'ı**seçin. Ardından **kullanıcı adı ve parolayla Oturum Aç'ı**seçin. **Parolanızı oluştur'u**seçin. Ardından telefon kimlik doğrulama işleminden geçirin.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
