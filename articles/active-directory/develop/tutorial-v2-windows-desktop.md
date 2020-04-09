---
title: Microsoft kimlik platformu Windows masaüstü ile başlayın
description: Bir Windows Desktop .NET (XAML) uygulaması nın nasıl bir erişim jetonu alabilirsiniz ve Microsoft kimlik platformu tarafından korunan bir API çağırabilirsiniz.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 12/12/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 3cca48c44add92b935e3a21f35e38bb4990ff9de
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886424"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Windows Desktop uygulamasından Microsoft Graph API'sini arayın

Bu kılavuz, yerel bir Windows Desktop .NET (XAML) uygulamasının Microsoft Graph API'yi aramak için nasıl bir erişim belirteci kullandığını gösterir. Uygulama ayrıca geliştiriciler v2.0 bitiş noktası için bir Microsoft kimlik platformundan erişim belirteçleri gerektiren diğer API'lere de erişebilir. Bu platform eskiden Azure AD olarak adlandırılmıştır.

Kılavuzu tamamladığınızda, uygulamanız kişisel hesapları (outlook.com, live.com ve diğerleri dahil) kullanan korumalı bir API'yi arayabilir. Uygulama ayrıca Azure Etkin Dizini kullanan herhangi bir şirket veya kuruluşun iş ve okul hesaplarını da kullanır.  

> [!NOTE]
> Kılavuz, Visual Studio 2015 Update 3, Visual Studio 2017 veya Visual Studio 2019'u gerektirir. Bu sürümlerden hiç yok mu? [Visual Studio 2019'u ücretsiz olarak indirin.](https://www.visualstudio.com/downloads/)

>[!NOTE]
> Microsoft kimlik platformunda yeniyseniz, bir belirteç edinme ile başlamanızı [ve bir Windows masaüstü uygulamasından Microsoft Graph API'yi aramanızı](quickstart-v2-windows-desktop.md)öneririz.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulama nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.svg)

Bu kılavuzla oluşturduğunuz örnek uygulama, Microsoft Graph API'sini veya Microsoft kimlik platformu bitiş noktasından gelen belirteçleri kabul eden bir Web API'sini sorgulayan bir Windows Desktop uygulamasına olanak tanır. Bu senaryo için, Yetkilendirme üstbilgisi aracılığıyla HTTP isteklerine bir belirteç eklersiniz. Microsoft Kimlik Doğrulama Kitaplığı (MSAL), belirteç edinme ve yenileme işlemlerini işler.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Korumalı Web API'lerine erişmek için belirteç edinimi işleme

Kullanıcının kimliği doğrulandıktan sonra, örnek uygulama Microsoft Graph API'sini veya geliştiriciler için Microsoft kimlik platformu tarafından güvenli bir Web API'sini sorgulamak için kullanabileceğiniz bir belirteç alır.

Microsoft Graph gibi API'ler, belirli kaynaklara erişime izin vermek için bir belirteç gerektirir. Örneğin, bir kullanıcının profilini okumak, bir kullanıcının takvimine erişmek veya e-posta göndermek için bir belirteç gereklidir. Uygulamanız, API kapsamlarını belirterek bu kaynaklara erişmek için MSAL'ı kullanarak bir erişim belirteci isteyebilir. Bu erişim belirteci, korumalı kaynağa karşı yapılan her arama için HTTP Yetkilendirme üstbilgisine eklenir.

MSAL, uygulamanızın gerekmemesi için önbelleğe alma ve yenileme erişim belirteçlerini yönetir.

## <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuzda aşağıdaki NuGet paketleri kullanışı:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Kimlik Doğrulama Kitaplığı (MSAL.NET)|

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölümde, uygulamanın belirteç gerektiren Web API'lerini sorgulayabilmesi için Bir Windows Desktop .NET uygulamasını (XAML) *Microsoft ile Oturum Açma* ile nasıl tümleştireceğinigösteren yeni bir proje oluşturursunuz.

Bu kılavuzla oluşturduğunuz uygulama, grafik çağırmak için kullanılan bir düğmeyi, sonuçları ekranda göstermek için bir alan ve oturum açma düğmesini görüntüler.

> [!NOTE]
> Bunun yerine bu örneğin Visual Studio projesini indirmeyi mi tercih edersiniz? [Bir proje indirin](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/msal3x.zip)ve çalıştırmadan önce kod örneğini yapılandırmak için [Yapılandırma adımına](#register-your-application) atlayın.
>

Uygulamanızı oluşturmak için aşağıdakileri yapın:

1. Visual Studio'da **Dosya** > **Yeni** > **Projesi'ni**seçin.
2. Şablonlar altında, **Visual C#** seçeneğini **belirleyin.**
3. Kullandığınız Visual Studio sürümünün sürümüne bağlı olarak **WPF App'ı (.NET Framework)** seçin.

## <a name="add-msal-to-your-project"></a>Projenize MSAL Ekleyin

1. Visual Studio'da **Araçlar** > **NuGet Paket Yöneticisi**> **Paket Yöneticisi Konsolu'nu**seçin.
2. Paket Yöneticisi Konsolu penceresinde aşağıdaki Azure PowerShell komutunu yapıştırın:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

    > [!NOTE] 
    > Bu komut Microsoft Kimlik Doğrulama Kitaplığı yükler. MSAL, Azure Active Directory v2.0 tarafından korunan API'lere erişmek için kullanılan kullanıcı belirteçlerini edinme, önbelleğe alma ve yenileme işlemlerini işler.
    >

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Başvurunuzu iki şekilde de kaydedebilirsiniz.

### <a name="option-1-express-mode"></a>Seçenek 1: Ekspres modu

Aşağıdakileri yaparak başvurunuzu hızlı bir şekilde kaydedebilirsiniz:
1. [Azure portalına](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/WinDesktopQuickstartPage/sourceType/docs)gidin - Uygulama Kaydı .
1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.

### <a name="option-2-advanced-mode"></a>Seçenek 2: Gelişmiş mod

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:
1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
1. **Yeni kayıt**seçin.
   - **Ad** bölümüne, örneğin `Win-App-calling-MsGraph`uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
   - **Desteklenen hesap türleri** bölümünde **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları (ör. Skype, Xbox, Outlook.com)** seçeneğini belirtin.
   - Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
   1. Yönlendirme **URI'leri** bölümünde, Yönlendirme URI'leri listesinde:
   1. **TYPE** sütununda **Ortak istemci/yerel (mobil & masaüstü)** seçeneğini belirleyin.
   1. **REDIRECT URI** sütununa,`https://login.microsoftonline.com/common/oauth2/nativeclient`
1. **Kaydol**’u seçin.
1. Visual Studio'ya gidin, *App.xaml.cs* dosyasını `Enter_the_Application_Id_here` açın ve daha sonra aşağıdaki kod snippet'ini yeni kaydettiğiniz ve kopyaladığınız uygulama kimliğiyle değiştirin.

    ```csharp
    private static string ClientId = "Enter_the_Application_Id_here";
    ```

## <a name="add-the-code-to-initialize-msal"></a>MSAL'ı başlatmak için kodu ekleme

Bu adımda, belirteçlerin işlenmesi gibi MSAL ile etkileşimi işlemek için bir sınıf oluşturursunuz.

1. *App.xaml.cs* dosyasını açın ve sonra MSAL için başvuruyu sınıfa ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```
   <!-- Workaround for Docs conversion bug -->

2. Uygulama sınıfını aşağıdaki güncelleştirmeler:

    ```csharp
    public partial class App : Application
    {
        static App()
        {
            _clientApp = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AzureCloudInstance.AzurePublic, Tenant)
                .Build();
        }

        // Below are the clientId (Application Id) of your app registration and the tenant information. 
        // You have to replace:
        // - the content of ClientID with the Application Id for your app registration
        // - the content of Tenant by the information about the accounts allowed to sign-in in your application:
        //   - For Work or School account in your org, use your tenant ID, or domain
        //   - for any Work or School accounts, use `organizations`
        //   - for any Work or School accounts, or Microsoft personal account, use `common`
        //   - for Microsoft Personal account, use consumers
        private static string ClientId = "0b8b0665-bc13-4fdc-bd72-e0227b9fc011";

        private static string Tenant = "common";

        private static IPublicClientApplication _clientApp ;

        public static IPublicClientApplication PublicClientApp { get { return _clientApp; } }
    }
    ```

## <a name="create-the-application-ui"></a>Uygulama Kullanıcı Birası oluşturma

Bu bölümde, bir uygulamanın Microsoft Graph gibi korumalı bir arka uç sunucusunu nasıl sorgulayabileceği gösterilmektedir. 

Proje şablonunuzun bir parçası olarak otomatik olarak bir *MainWindow.xaml* dosyası oluşturulmalıdır. Bu dosyayı açın ve ardından uygulamanızın * \<Grid>* düğümunu aşağıdaki kodla değiştirin:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```

## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Microsoft Graph API için bir belirteç almak için MSAL'ı kullanın

Bu bölümde, Microsoft Graph API için bir belirteç almak için MSAL'ı kullanırsınız.

1. *MainWindow.xaml.cs* dosyasında, MSAL için başvuruyu sınıfa ekleyin:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. `MainWindow` Sınıf kodunu aşağıdakilerle değiştirin:

    ```csharp
    public partial class MainWindow : Window
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };


        public MainWindow()
        {
            InitializeComponent();
        }

      /// <summary>
        /// Call AcquireToken - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;
            var app = App.PublicClientApp;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;

            var accounts = await app.GetAccountsAsync();
            var firstAccount = accounts.FirstOrDefault();

            try
            {
                authResult = await app.AcquireTokenSilent(scopes, firstAccount)
                    .ExecuteAsync();
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilent.
                // This indicates you need to call AcquireTokenInteractive to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

                try
                {
                    authResult = await app.AcquireTokenInteractive(scopes)
                        .WithAccount(accounts.FirstOrDefault())
                        .WithPrompt(Prompt.SelectAccount)
                        .ExecuteAsync();
                }
                catch (MsalException msalex)
                {
                    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
                }
            }
            catch (Exception ex)
            {
                ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
                return;
            }

            if (authResult != null)
            {
                ResultText.Text = await GetHttpContentWithToken(graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Daha fazla bilgi

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Yöntemin `AcquireTokenInteractive` çağrılması, kullanıcıların oturum açmasını gerektiren bir pencereyle sonuçlanır. Uygulamalar genellikle kullanıcıların korumalı bir kaynağa ilk kez etkileşimli olarak oturum açmalarını gerektirir. Ayrıca, bir belirteç elde etmek için sessiz bir işlem başarısız olduğunda (örneğin, bir kullanıcının parolasının süresi dolduğunda) oturum açmaları gerekebilir.

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Yöntem, `AcquireTokenSilent` herhangi bir kullanıcı etkileşimi olmadan belirteç satın almave yenilemeleri işler. İlk `AcquireTokenInteractive` kez yürütüldükten `AcquireTokenSilent` sonra, sonraki aramalar için korumalı kaynaklara erişen belirteçleri elde etmek için kullanılan olağan yöntemdir, çünkü belirteçleri isteme veya yenileme çağrıları sessizce yapılır.

Sonunda, `AcquireTokenSilent` yöntem başarısız olur. Hata nedeni, kullanıcının başka bir aygıtta parolasını oturum açmış veya değiştirmiş olması olabilir. MSAL, sorunun etkileşimli bir eylem gerekerek çözülebileceğini algıladığında, `MsalUiRequiredException` bir özel durum oluşturur. Uygulamanız bu özel durumu iki şekilde işleyebilir:

* Hemen karşı `AcquireTokenInteractive` bir arama yapabilir. Bu çağrı, kullanıcının oturum açmasını istenmesine neden olur. Bu desen genellikle kullanıcı için kullanılabilir çevrimdışı içerik bulunmayan çevrimiçi uygulamalarda kullanılır. Bu kılavuzlu kurulum tarafından oluşturulan örnek, örneği ilk kez çalıştırdığınızda eylemde görebileceğiniz bu deseni izler. 

* Hiçbir kullanıcı uygulamayı kullanmadığından, `PublicClientApp.Users.FirstOrDefault()` null değeri içerdiğinden ve bir `MsalUiRequiredException` özel durum atıldığından. 

* Örnekteki kod daha sonra arayarak `AcquireTokenInteractive`özel durumu işler ve bu da kullanıcının oturum açmasını istenir.

* Bunun yerine, oturum açmaiçin doğru zamanı seçebilmeleri için kullanıcılara etkileşimli oturum açmanın gerekli olduğuna dair görsel bir gösterge sunabilir. Veya uygulama daha `AcquireTokenSilent` sonra yeniden deneyebilirsiniz. Bu desen, kullanıcılar diğer uygulama işlevlerini kesintiye uğramadan kullanabiliyorsa (örneğin, uygulamada çevrimdışı içerik kullanılabilirolduğunda) sıklıkla kullanılır. Bu durumda, kullanıcılar korumalı kaynağa erişmek veya güncelliğini yitirmiş bilgileri yenilemek için ne zaman oturum açmaya karar verebilirler. Alternatif olarak, uygulama geçici olarak `AcquireTokenSilent` kullanılamayan ağ geri yüklendiğinde yeniden denemeye karar verebilir.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Yeni elde ettiğiniz belirteci kullanarak Microsoft Graph API'yi arayın

Aşağıdaki yeni yöntemi `MainWindow.xaml.cs`ekleyin. Yöntem, Bir Yetkilendirme `GET` üstbilgi kullanarak Grafik API karşı bir istek yapmak için kullanılır:

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
        //Add the token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
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

<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Korunan API'ye karşı REST çağrısı yapma hakkında daha fazla bilgi

Bu örnek uygulamada, belirteç gerektiren `GET` korumalı bir kaynağa karşı bir HTTP isteği nde bulunmak ve ardından içeriği arayana iade etmek için `GetHttpContentWithToken` yöntemi kullanırsınız. Bu yöntem, http yetkilendirme üstbilgisinde edinilmiş belirteci ekler. Bu örnek için kaynak, kullanıcının profil bilgilerini görüntüleyen Microsoft Graph API *me* bitiş noktasıdır.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Kullanıcıyı oturum alabilmek için yöntem ekleme

Bir kullanıcıyı oturum dışı batmak `MainWindow.xaml.cs` için dosyanıza aşağıdaki yöntemi ekleyin:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private async void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    var accounts = await App.PublicClientApp.GetAccountsAsync(); 

    if (accounts.Any())
    {
        try
        {
            await App.PublicClientApp.RemoveAsync(accounts.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Kullanıcı oturum açma hakkında daha fazla bilgi

Yöntem, `SignOutButton_Click` kullanıcıları MSAL kullanıcı önbelleğinden kaldırır ve bu da MSAL'a geçerli kullanıcıyı unutmasını söyler, böylece gelecekteki bir belirteç elde etme isteği yalnızca etkileşimli olması için yapıldığında başarılı olur.

Bu örnekteki uygulama tek kullanıcıları desteklese de, MSAL birden çok hesabın aynı anda oturum açmış olabileceği senaryoları destekler. Bir örnek, kullanıcının birden çok hesabı olan bir e-posta uygulamasıdır.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Temel belirteç bilgilerini görüntüleme

Belirteç le ilgili temel bilgileri görüntülemek için *MainWindow.xaml.cs* dosyanıza aşağıdaki yöntemi ekleyin:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Username: {authResult.Account.Username}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Daha fazla bilgi

Microsoft Graph API'yi aramak için kullanılan erişim belirtecine ek olarak, kullanıcı giriş yaptıktan sonra, MSAL bir kimlik belirteci de alır. Bu belirteç, kullanıcılarla ilgili küçük bir bilgi alt kümesi içerir. Yöntem, `DisplayBasicTokenInfo` belirteçte bulunan temel bilgileri görüntüler. Örneğin, kullanıcının görüntü adının ve kimliğinin yanı sıra belirteç son kullanma tarihini ve erişim belirtecinin kendisini temsil eden dizesini görüntüler. Microsoft Graph *API'yi birden* çok kez arayabilir ve aynı belirteçlerin sonraki istekler için yeniden kullanıldığını görebilirsiniz. Ayrıca, MSAL belirteci yenileme zamanının geldiğinde son kullanma tarihinin uzatıldığını da görebilirsiniz.
<!--end-collapse-->

[!INCLUDE [5. Test and Validate](../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]

Microsoft kimlik platformlarını geliştirmemize yardımcı olun. Kısa bir iki soruluk anketi tamamlayarak bize ne düşündüğünüzü söyleyin.

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
