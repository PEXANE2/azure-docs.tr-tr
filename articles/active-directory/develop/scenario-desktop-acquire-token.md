---
title: Web API(masaüstü uygulaması) aramak için bir belirteç edinin | Azure
titleSuffix: Microsoft identity platform
description: Uygulama için bir belirteç edinmek için web API'lerini çağıran bir masaüstü uygulaması oluşturmayı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: a5942a9d614bbb06fadb1d4b16d4c68c007434c7
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885327"
---
# <a name="desktop-app-that-calls-web-apis-acquire-a-token"></a>Web API'lerini çağıran masaüstü uygulaması: Bir belirteç edinin

Ortak istemci uygulamasının bir örneğini oluşturduktan sonra, bu uygulamayı web API'sını aramak için kullanacağınız bir belirteç elde etmek için kullanırsınız.

## <a name="recommended-pattern"></a>Önerilen desen

Web API tarafından `scopes`tanımlanır. Uygulamanızda sağladığınız deneyim ne olursa olsun, kullanılacak model:

- Sistematik olarak çağırarak `AcquireTokenSilent`belirteç önbelleğinden bir belirteç almaya çalışır.
- Bu çağrı başarısız olursa, burada `AcquireToken` `AcquireTokenXX`'ile temsil edilen, kullanmak istediğiniz akışı kullanın.

# <a name="net"></a>[.NET](#tab/dotnet)

### <a name="in-msalnet"></a>MSAL.NET yılında

```csharp
AuthenticationResult result;
var accounts = await app.GetAccountsAsync();
IAccount account = ChooseAccount(accounts); // for instance accounts.FirstOrDefault
                                            // if the app manages is at most one account  
try
{
 result = await app.AcquireTokenSilent(scopes, account)
                   .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
  result = await app.AcquireTokenXX(scopes, account)
                    .WithOptionalParameterXXX(parameter)
                    .ExecuteAsync();
}
```

# <a name="java"></a>[Java](#tab/java)

```java

Set<IAccount> accountsInCache = pca.getAccounts().join();
// Take first account in the cache. In a production application, you would filter
// accountsInCache to get the right account for the user authenticating.
IAccount account = accountsInCache.iterator().next();

IAuthenticationResult result;
try {
    SilentParameters silentParameters =
            SilentParameters
                    .builder(SCOPE, account)
                    .build();

    // try to acquire token silently. This call will fail since the token cache
    // does not have any data for the user you are trying to acquire a token for
    result = pca.acquireTokenSilently(silentParameters).join();
} catch (Exception ex) {
    if (ex.getCause() instanceof MsalException) {

        InteractiveRequestParameters parameters = InteractiveRequestParameters
                .builder(new URI("http://localhost"))
                .scopes(SCOPE)
                .build();

        // Try to acquire a token interactively with system browser. If successful, you should see
        // the token and account information printed out to console
        result = pca.acquireToken(parameters).join();
    } else {
        // Handle other exceptions accordingly
        throw ex;
    }
}
return result;

```

# <a name="python"></a>[Python](#tab/python)

```Python
result = None

# Firstly, check the cache to see if this end user has signed in before
accounts = app.get_accounts(username=config["username"])
if accounts:
    result = app.acquire_token_silent(config["scope"], account=accounts[0])

if not result:
    result = app.acquire_token_by_xxx(scopes=config["scope"])
```

# <a name="macos"></a>[Macos](#tab/macOS)

### <a name="in-msal-for-ios-and-macos"></a>iOS ve macOS için MSAL'da

Amaç-C:

```objc
MSALAccount *account = [application accountForIdentifier:accountIdentifier error:nil];

MSALSilentTokenParameters *silentParams = [[MSALSilentTokenParameters alloc] initWithScopes:scopes account:account];
[application acquireTokenSilentWithParameters:silentParams completionBlock:^(MSALResult *result, NSError *error) {

    // Check the error
    if (error && [error.domain isEqual:MSALErrorDomain] && error.code == MSALErrorInteractionRequired)
    {
        // Interactive auth will be required, call acquireTokenWithParameters:error:
    }
}];
```
Swift:

```swift
guard let account = try? application.account(forIdentifier: accountIdentifier) else { return }
let silentParameters = MSALSilentTokenParameters(scopes: scopes, account: account)
application.acquireTokenSilent(with: silentParameters) { (result, error) in

    guard let authResult = result, error == nil else {

    let nsError = error! as NSError

        if (nsError.domain == MSALErrorDomain &&
            nsError.code == MSALError.interactionRequired.rawValue) {

            // Interactive auth will be required, call acquireToken()
            return
        }
        return
    }
}
```
---

Aşağıda, bir masaüstü uygulamasında belirteçler edinmenin çeşitli yolları veremleri veremleri veremleri veremleri veremleri veremleri veremleri aşağıda veda edebilirsiniz.

## <a name="acquire-a-token-interactively"></a>Etkileşimli bir belirteç edinme

Aşağıdaki örnekte, kullanıcının microsoft graph ile profilini okumak için etkileşimli olarak bir belirteç almak için en az kod gösterilmektedir.

# <a name="net"></a>[.NET](#tab/dotnet)
### <a name="in-msalnet"></a>MSAL.NET yılında

```csharp
string[] scopes = new string[] {"user.read"};
var app = PublicClientApplicationBuilder.Create(clientId).Build();
var accounts = await app.GetAccountsAsync();
AuthenticationResult result;
try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
             .ExecuteAsync();
}
catch(MsalUiRequiredException)
{
 result = await app.AcquireTokenInteractive(scopes)
             .ExecuteAsync();
}
```

### <a name="mandatory-parameters"></a>Zorunlu parametreler

`AcquireTokenInteractive`belirteç gerektiren kapsamları tanımlayan dizeleri numaralandırma içeren yalnızca bir zorunlu parametresi ``scopes``vardır. Belirteç Microsoft Graph içinse, gerekli kapsamlar "İzinler" adlı bölümdeki her Microsoft Graph API'sinin API başvurusunda bulunabilir. Örneğin, [kullanıcının ilgili kişilerini listelemek](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_contacts)için "User.Read", "Contacts.Read" kapsamı kullanılmalıdır. Daha fazla bilgi için [Microsoft Graph izinleri başvurusuna](https://developer.microsoft.com/graph/docs/concepts/permissions_reference)bakın.

Android'de, belirteç etkileşimden sonra `.WithParentActivityOrWindow`ana aktiviteye geri dönebilmek için gösterildiği gibi üst etkinliği de kullanarak üst etkinliği belirtmeniz gerekir. Belirtmezseniz, ararken `.ExecuteAsync()`bir özel durum atılır.

### <a name="specific-optional-parameters-in-msalnet"></a>MSAL.NET'da belirli isteğe bağlı parametreler

#### <a name="withparentactivityorwindow"></a>WithParentActivityOrWindow

UI etkileşimli olduğu için önemlidir. `AcquireTokenInteractive`destekleyen platformlar için ana Kullanıcı Arabirimi'ni belirtebilecek belirli bir isteğe bağlı parametrevardır. Bir masaüstü uygulamasında kullanıldığında, `.WithParentActivityOrWindow` platforma bağlı olarak farklı bir türü vardır.

```csharp
// net45
WithParentActivityOrWindow(IntPtr windowPtr)
WithParentActivityOrWindow(IWin32Window window)

// Mac
WithParentActivityOrWindow(NSWindow window)

// .Net Standard (this will be on all platforms at runtime, but only on NetStandard at build time)
WithParentActivityOrWindow(object parent).
```

Açıklamalar:

- .NET Standard'da `object` beklenen `Activity` ler `UIViewController` Android'de, `NSWindow` iOS'ta, MAC'te ve `IWin32Window` windows'da dır. `IntPr`
- Windows'da, gömülü `AcquireTokenInteractive` tarayıcının uygun Kullanıcı Arabirimi eşitleme bağlamını alması için Kullanıcı Arabirimi iş parçacığından aramanız gerekir. UI iş parçacığından arama maksama, iletilerin ui ile düzgün pompalamamalarına ve kilitlenme senaryolarına neden olabilir. UI iş parçacığı üzerinde değilseniz, UI iş parçacığı Microsoft Kimlik Doğrulama Kitaplıkları (MSALs) arama yolu wpf `Dispatcher` üzerinde kullanmaktır.
- WPF kullanıyorsanız, WPF denetiminden bir pencere almak için `WindowInteropHelper.Handle` sınıfı kullanabilirsiniz. Sonra arama bir WPF kontrolünden (`this`):

  ```csharp
  result = await app.AcquireTokenInteractive(scopes)
                    .WithParentActivityOrWindow(new WindowInteropHelper(this).Handle)
                    .ExecuteAsync();
  ```

#### <a name="withprompt"></a>WithPrompt

`WithPrompt()`bir istem belirterek kullanıcı ile etkileşimi denetlemek için kullanılır.

<img src="https://user-images.githubusercontent.com/13203188/53438042-3fb85700-39ff-11e9-9a9e-1ff9874197b3.png" width="25%" />

Sınıf aşağıdaki sabitleri tanımlar:

- ``SelectAccount``STS'yi, kullanıcının oturumyaptığı hesapları içeren hesap seçimi iletişim kutusunu sunmaya zorlar. Bu seçenek, uygulama geliştiricileri kullanıcıların farklı kimlikler arasında seçim yapmak istediğinizde yararlıdır. Bu seçenek, MSAL'ı kimlik sağlayıcısına göndermeye ``prompt=select_account`` iter. Bu seçenek varsayılandır. Bu hesap ve kullanıcı için bir oturum varlığı gibi kullanılabilir bilgilere dayalı mümkün olan en iyi deneyimi sağlayan iyi bir iş yok. Bunu yapmak için iyi bir neden yoksa bunu değiştirmeyin.
- ``Consent``daha önce izin verilmiş olsa bile, uygulama geliştiricisinin kullanıcıdan rızası istenmesi için zorlamasına olanak tanır. Bu durumda, MSAL `prompt=consent` kimlik sağlayıcısına gönderir. Bu seçenek, kuruluş yönetiminin, uygulama her kullanıldığında kullanıcıya onay iletişim kutusu yla birlikte sunulmasını talep ettiği bazı güvenlik odaklı uygulamalarda kullanılabilir.
- ``ForceLogin``bu kullanıcı istemi gerekli olmasa bile, uygulama geliştiricisinin hizmet tarafından kimlik bilgileri için kullanıcıdan istenmesini sağlar. Bu seçenek, bir belirteç edinme başarısız olursa kullanıcının yeniden oturum açmasına izin vermek için yararlı olabilir. Bu durumda, MSAL `prompt=login` kimlik sağlayıcısına gönderir. Bazen, kuruluş yönetiminin bir uygulamanın belirli bölümlerine her erişici olduğunda kullanıcının yeniden imzalamasını gerektirdiği güvenlik odaklı uygulamalarda kullanılır.
- ``Never``(.NET 4.5 ve Yalnızca WinRT için) kullanıcıyı istesin, bunun yerine gizli gömülü web görünümünde depolanan çerezi kullanmaya çalışır. Daha fazla bilgi için MSAL.NET web görünümleri'ne bakın. Bu seçeneği kullanmak başarısız olabilir. Bu durumda, `AcquireTokenInteractive` bir UI etkileşimi gerekli olduğunu bildirmek için bir özel durum atar. Başka bir `Prompt` parametre kullanmanız gerekir.
- ``NoPrompt``kimlik sağlayıcısına herhangi bir istem göndermez. Bu seçenek yalnızca Azure Etkin Dizin (Azure AD) B2C profil oluşturma ilkeleri için yararlıdır. Daha fazla bilgi için [Azure AD B2C ayrıntılarına](https://aka.ms/msal-net-b2c-specificities)bakın.

#### <a name="withextrascopetoconsent"></a>Withextrascopetoconsent

Bu değiştirici, kullanıcının çeşitli kaynaklara önceden onay vermek istediğiniz ve normalde MSAL.NET/the Microsoft kimlik platformuyla normalde kullanılan artımlı onayı kullanmak istemediğiniz gelişmiş bir senaryoda kullanılır. Daha fazla bilgi için [bkz.](scenario-desktop-production.md#have-the-user-consent-upfront-for-several-resources)

```csharp
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

#### <a name="withcustomwebui"></a>IleCustomWebUi

Web UI, tarayıcıyı çağırmak için bir mekanizmadır. Bu mekanizma özel bir UI WebBrowser denetimi veya tarayıcıyı açmayı temsilciye devretmenin bir yolu olabilir.
MSAL çoğu platform için web Kullanıcı Arabirimi uygulamaları sağlar, ancak tarayıcıyı kendiniz barındırmak isteyebileceğiniz durumlar vardır:

- Masaüstü bilgisayarlarda Blazor, Unity ve Mono gibi MSAL tarafından açıkça kapsanmamış platformlar.
- Kullanıcı ui uygulamanızı test etmek ve Selenium ile kullanılabilecek otomatik bir tarayıcı kullanmak istiyorsunuz.
- MSAL'ı çalıştıran tarayıcı ve uygulama ayrı işlemlerdedir.

##### <a name="at-a-glance"></a>Bir bakışta

Bunu başarmak için, son `start Url`kullanıcının kullanıcı adı gibi öğeleri girebilmesi için tercih edilen bir tarayıcıda görüntülenmesi gereken MSAL'a verirsiniz.
Kimlik doğrulama sı bittikten sonra uygulamanızın Azure `end Url`AD tarafından sağlanan bir kod içeren MSAL'a geri geçmesi gerekir.
Ev sahibi `end Url` her `redirectUri`zaman . Durdurmak `end Url`için, aşağıdaki şeylerden birini yapın:

- Monitör tarayıcısı vurulana kadar `redirect Url` yönlendirir.
- Tarayıcının izlediğiniz bir URL'ye yönlendirmesini sağlar.

##### <a name="withcustomwebui-is-an-extensibility-point"></a>WithCustomWebUi bir genişletilebilirlik noktasıdır

`WithCustomWebUi`kamu istemcisi uygulamalarında kendi kullanıcı arabirimi sağlamak için kullanabileceğiniz bir genişletilebilirlik noktasıdır. Ayrıca, kullanıcının kimlik sağlayıcısının /Authorize bitiş noktasından geçmesine ve oturum açmalarına ve onay vermelerine izin verebilirsiniz. MSAL.NET daha sonra kimlik doğrulama kodunu kullanabilir ve bir belirteç alabilir. Örneğin, Visual Studio'da elektron uygulamalarının (örneğin, Visual Studio Feedback) web etkileşimini sağlaması, ancak işin çoğunu MSAL.NET bırakması için kullanılır. UI otomasyonu sağlamak istiyorsanız da kullanabilirsiniz. Kamu istemcisi uygulamalarında MSAL.NET, güvenliğe saygı duyulandan emin olmak için Kod Değişimi Için Kanıt Anahtarı (PKCE) standardını kullanır. Kodu yalnızca MSAL.NET kullanabilir. Daha fazla bilgi için, [OAuth Public Clients tarafından Kod Değişimi için Kanıt Anahtarı RFC 7636 - Proof Key](https://tools.ietf.org/html/rfc7636)'e bakın.

  ```csharp
  using Microsoft.Identity.Client.Extensions;
  ```

##### <a name="use-withcustomwebui"></a>WithCustomWebUi kullanın

Kullanmak `.WithCustomWebUI`için aşağıdaki adımları izleyin.

  1. `ICustomWebUi` arabirimini gerçekleştirin. Daha fazla bilgi için [bu web sitesine](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/src/Microsoft.Identity.Client/Extensibility/ICustomWebUI.cs#L32-L70)bakın. Bir `AcquireAuthorizationCodeAsync`yöntem uygulayın ve MSAL.NET tarafından hesaplanan yetkilendirme kodu URL'sini kabul edin. Ardından, kullanıcının kimlik sağlayıcısıyla etkileşimi geçmesine izin verin ve kimlik sağlayıcısının yetkilendirme koduyla birlikte uygulamanızı geri çağıracağı URL'yi geri döndürün. Eğer sorunlarınız varsa, uygulama `MsalExtensionException` güzel MSAL ile işbirliği için bir istisna atmalıdır.
  2. Aramanızda, `AcquireTokenInteractive` özel `.WithCustomUI()` web kullanıcı arabirimi örneğini geçen değiştiriciyi kullanın.

     ```csharp
     result = await app.AcquireTokenInteractive(scopes)
                       .WithCustomWebUi(yourCustomWebUI)
                       .ExecuteAsync();
     ```

##### <a name="examples-of-implementation-of-icustomwebui-in-test-automation-seleniumwebui"></a>Test otomasyonunda ICustomWebUi uygulama örnekleri: SeleniumWebUI

MSAL.NET ekibi bu genişletilebilirlik mekanizmasını kullanmak için UI testlerini yeniden yazdı. Eğer ilgileniyorsanız, MSAL.NET kaynak kodustaki [SeleniumWebUI](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/tests/Microsoft.Identity.Test.Integration/Infrastructure/SeleniumWebUI.cs#L15-L160) sınıfına bakın.

##### <a name="provide-a-great-experience-with-systemwebviewoptions"></a>SystemWebViewOptions ile harika bir deneyim yaşayın

MSAL.NET 4.1'den [`SystemWebViewOptions`](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.systemwebviewoptions?view=azure-dotnet)itibaren şunları belirtebilirsiniz:

- Uri (`BrowserRedirectError`) veya html parçası görüntülemek`HtmlMessageError`için gitmek için ( ) sistem web tarayıcısında oturum açma veya izin hataları durumunda.
- Uri gitmek için`BrowserRedirectSuccess`( ) veya html`HtmlMessageSuccess`parçası görüntülemek için ( ) başarılı oturum açma veya rıza durumunda.
- Sistem tarayıcısını başlatmak için çalışacak eylem. Temsilciyi `OpenBrowserAsync` ayarlayarak kendi uygulamanızı sağlayabilirsiniz. Sınıf ayrıca iki tarayıcı için varsayılan `OpenWithEdgeBrowserAsync` bir `OpenWithChromeEdgeBrowserAsync` uygulama sağlar: ve Microsoft Edge ve [Microsoft Edge on Chromium](https://www.windowscentral.com/faq-edge-chromium)için ( sırasıyla).

Bu yapıyı kullanmak için aşağıdaki örnek gibi bir şey yazın:

```csharp
IPublicClientApplication app;
...

options = new SystemWebViewOptions
{
 HtmlMessageError = "<b>Sign-in failed. You can close this tab ...</b>",
 BrowserRedirectSuccess = "https://contoso.com/help-for-my-awesome-commandline-tool.html"
};

var result = app.AcquireTokenInteractive(scopes)
                .WithEmbeddedWebView(false)       // The default in .NET Core
                .WithSystemWebViewOptions(options)
                .Build();
```

#### <a name="other-optional-parameters"></a>Diğer isteğe bağlı parametreler

Diğer tüm isteğe bağlı parametreler `AcquireTokenInteractive`hakkında daha fazla bilgi için [bkz: AcquireTokenInteractiveParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokeninteractiveparameterbuilder?view=azure-dotnet-preview#methods).

# <a name="java"></a>[Java](#tab/java)

```java
private static IAuthenticationResult acquireTokenInteractive() throws Exception {

    // Load token cache from file and initialize token cache aspect. The token cache will have
    // dummy data, so the acquireTokenSilently call will fail.
    TokenCacheAspect tokenCacheAspect = new TokenCacheAspect("sample_cache.json");

    PublicClientApplication pca = PublicClientApplication.builder(CLIENT_ID)
            .authority(AUTHORITY)
            .setTokenCacheAccessAspect(tokenCacheAspect)
            .build();

    Set<IAccount> accountsInCache = pca.getAccounts().join();
    // Take first account in the cache. In a production application, you would filter
    // accountsInCache to get the right account for the user authenticating.
    IAccount account = accountsInCache.iterator().next();

    IAuthenticationResult result;
    try {
        SilentParameters silentParameters =
                SilentParameters
                        .builder(SCOPE, account)
                        .build();

        // try to acquire token silently. This call will fail since the token cache
        // does not have any data for the user you are trying to acquire a token for
        result = pca.acquireTokenSilently(silentParameters).join();
    } catch (Exception ex) {
        if (ex.getCause() instanceof MsalException) {

            InteractiveRequestParameters parameters = InteractiveRequestParameters
                    .builder(new URI("http://localhost"))
                    .scopes(SCOPE)
                    .build();

            // Try to acquire a token interactively with system browser. If successful, you should see
            // the token and account information printed out to console
            result = pca.acquireToken(parameters).join();
        } else {
            // Handle other exceptions accordingly
            throw ex;
        }
    }
    return result;
}
```

# <a name="python"></a>[Python](#tab/python)

MSAL Python doğrudan etkileşimli bir edinme belirteç yöntemi sağlamaz. Bunun yerine, bir yetkilendirme kodu elde etmek için kullanıcı etkileşim akışının uygulanmasında bir yetkilendirme isteği göndermek için uygulama gerektirir. Bu kod daha sonra `acquire_token_by_authorization_code` belirteci almak için yönteme geçirilebilir.

```Python
result = None

# Firstly, check the cache to see if this end user has signed in before
accounts = app.get_accounts(username=config["username"])
if accounts:
    result = app.acquire_token_silent(config["scope"], account=accounts[0])

if not result:
    result = app.acquire_token_by_authorization_code(
         request.args['code'],
         scopes=config["scope"])    

```

# <a name="macos"></a>[Macos](#tab/macOS)

### <a name="in-msal-for-ios-and-macos"></a>iOS ve macOS için MSAL'da

Amaç-C:

```objc
MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopes webviewParameters:[MSALWebviewParameters new]];
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) {
    if (!error)
    {
        // You'll want to get the account identifier to retrieve and reuse the account
        // for later acquireToken calls
        NSString *accountIdentifier = result.account.identifier;

        NSString *accessToken = result.accessToken;
    }
}];
```

Swift:

```swift
let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopes, webviewParameters: MSALWebviewParameters())
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in

    guard let authResult = result, error == nil else {
        print(error!.localizedDescription)
        return
    }

    // Get access token from result
    let accessToken = authResult.accessToken
})
```
---

## <a name="integrated-windows-authentication"></a>Tümleşik Windows Kimlik Doğrulaması

Etki alanı veya Azure AD'ye katılan bir makinede etki alanı kullanıcısını oturum açabilmek için Tümleşik Windows Kimlik Doğrulaması (IWA) kullanın.

### <a name="constraints"></a>Kısıtlamalar

- Tümleşik Windows Kimlik Doğrulama, yalnızca *federe+* kullanıcılar için, yani Active Directory'de oluşturulan ve Azure AD tarafından desteklenen kullanıcılar için kullanılabilir. *Yönetilen* kullanıcılar olarak bilinen Active Directory desteği olmadan doğrudan Azure AD'de oluşturulan kullanıcılar bu kimlik doğrulama akışını kullanamaz. Bu sınırlama kullanıcı adı ve parola akışını etkilemez.
- IWA , .NET Framework, .NET Core ve Universal Windows Platform (UWP) platformları için yazılmış uygulamalar içindir.
- IWA çok faktörlü kimlik doğrulamasını (MFA) atlamaz. MFA yapılandırılırsa, MFA'nın kullanıcı etkileşimi gerektirdiğinden, MFA sorunu gerekiyorsa IWA başarısız olabilir.
  > [!NOTE]
  > Bu biraz zor. IWA etkileşimli değildir, ancak MFA kullanıcı etkileşimi gerektirir. Kimlik sağlayıcısının MFA'nın gerçekleştirilmesini ne zaman istediğini, kiracı yöneticinin ne zaman gerçekleştirileceğini denetlemezsiniz. Gözlemlerimize göre, farklı bir ülkeden oturum açtığınızda, VPN ile kurumsal bir ağa bağlanmadığınızda ve hatta bazen VPN üzerinden bağlandığınızda bile MFA gereklidir. Belirleyici kurallar beklemeyin. Azure AD, MFA gerekip gerekip gerekmeden sürekli olarak öğrenmek için AI kullanır. IWA başarısız olursa, etkileşimli kimlik doğrulama veya aygıt kodu akışı gibi bir kullanıcı istemine geri dön.

- Yetki nin `PublicClientApplicationBuilder` geçmesi gerekenler:
  - Formun `https://login.microsoftonline.com/{tenant}/`kiracı, kiracı `tenant` kimliğini temsil eden GUID veya kiracı ile ilişkili bir etki alanı olduğu.
  - Herhangi bir iş ve `https://login.microsoftonline.com/organizations/`okul hesapları için: .
  - Microsoft kişisel hesapları desteklenmez. /ortak veya /tüketici kiracılarını kullanamazsınız.

- Tümleşik Windows Kimlik Doğrulama sessiz bir akış olduğundan:
  - Uygulamanızın kullanıcısı daha önce uygulamayı kullanmayı kabul etmiş olmalıdır.
  - Veya, kiracı yönetici daha önce kiracı tüm kullanıcılara uygulamayı kullanmak için izin vermiş olmalıdır.
  - Başka bir deyişle:
    - Geliştirici olarak Azure portalındaki **Hibe** düğmesini kendiniz seçtiniz.
    - Veya, bir kiracı yönetici, uygulama için kaydın **API izinleri** sekmesinde **{kiracı etki alanı} düğmesi için Yönetici Yitirme/Iptal onayı'nı** seçti. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-permissions-to-access-web-apis)
    - Veya, kullanıcıların uygulamayı kabul etmesi için bir yol sağladınız. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-individual-user-consent)
    - Veya, kiracı yöneticinin başvuruyu kabul etmesi için bir yol sağladınız. Daha fazla bilgi için Yönetici [onayına](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant)bakın.

- Bu akış .NET masaüstü, .NET Core ve UWP uygulamaları için etkinleştirilir.

Onay hakkında daha fazla bilgi için [Microsoft kimlik platformu izinlerine ve iznine](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent)bakın.

### <a name="learn-how-to-use-it"></a>Nasıl kullanılacağını öğrenin

# <a name="net"></a>[.NET](#tab/dotnet)

MSAL.NET olarak şunları kullanmanız gerekir:

```csharp
AcquireTokenByIntegratedWindowsAuth(IEnumerable<string> scopes)
```

Normalde sadece bir parametreye`scopes`ihtiyacınız vardır ( ). Windows yöneticinizin ilkeleri ayarlama şekline bağlı olarak, Windows makinenizdeki uygulamaların oturum açmış kullanıcıyı aramasına izin verilmeyebilir. Bu durumda, ikinci bir `.WithUsername()`yöntem kullanın ve oturum açmış kullanıcının kullanıcı adını upn biçimi `joe@contoso.com`olarak geçirin, örneğin. .NET Core'da, .NET Core platformu kullanıcı adını işletim sistemi için isteyemediğinden, yalnızca kullanıcı adını alan aşırı yük kullanılabilir.

Aşağıdaki örnek, alabileceğiniz özel durumlar ve bunların azaltıcı durumları ile ilgili açıklamalar la birlikte en güncel örneği sunar.

```csharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app = PublicClientApplicationBuilder
      .Create(clientId)
      .WithAuthority(authority)
      .Build();

 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
      .ExecuteAsync();
 }
 else
 {
  try
  {
   result = await app.AcquireTokenByIntegratedWindowsAuth(scopes)
      .ExecuteAsync(CancellationToken.None);
  }
  catch (MsalUiRequiredException ex)
  {
   // MsalUiRequiredException: AADSTS65001: The user or administrator has not consented to use the application
   // with ID '{appId}' named '{appName}'.Send an interactive authorization request for this user and resource.

   // you need to get user consent first. This can be done, if you are not using .NET Core (which does not have any Web UI)
   // by doing (once only) an AcquireToken interactive.

   // If you are using .NET core or don't want to do an AcquireTokenInteractive, you might want to suggest the user to navigate
   // to a URL to consent: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read

   // AADSTS50079: The user is required to use multi-factor authentication.
   // There is no mitigation - if MFA is configured for your tenant and AAD decides to enforce it,
   // you need to fallback to an interactive flows such as AcquireTokenInteractive or AcquireTokenByDeviceCode
   }
   catch (MsalServiceException ex)
   {
    // Kind of errors you could have (in ex.Message)

    // MsalServiceException: AADSTS90010: The grant type is not supported over the /common or /consumers endpoints. Please use the /organizations or tenant-specific endpoint.
    // you used common.
    // Mitigation: as explained in the message from Azure AD, the authority needs to be tenanted or otherwise organizations

    // MsalServiceException: AADSTS70002: The request body must contain the following parameter: 'client_secret or client_assertion'.
    // Explanation: this can happen if your application was not registered as a public client application in Azure AD
    // Mitigation: in the Azure portal, edit the manifest for your application and set the `allowPublicClient` to `true`
   }
   catch (MsalClientException ex)
   {
      // Error Code: unknown_user Message: Could not identify logged in user
      // Explanation: the library was unable to query the current Windows logged-in user or this user is not AD or AAD
      // joined (work-place joined users are not supported).

      // Mitigation 1: on UWP, check that the application has the following capabilities: Enterprise Authentication,
      // Private Networks (Client and Server), User Account Information

      // Mitigation 2: Implement your own logic to fetch the username (e.g. john@contoso.com) and use the
      // AcquireTokenByIntegratedWindowsAuth form that takes in the username

      // Error Code: integrated_windows_auth_not_supported_managed_user
      // Explanation: This method relies on an a protocol exposed by Active Directory (AD). If a user was created in Azure
      // Active Directory without AD backing ("managed" user), this method will fail. Users created in AD and backed by
      // AAD ("federated" users) can benefit from this non-interactive method of authentication.
      // Mitigation: Use interactive authentication
   }
 }


 Console.WriteLine(result.Account.Username);
}
```

EdinmeTokenByIntegratedWindowsAuthentication olası değiştiriciler listesi için, [AcquireTokenByIntegratedWindowsAuthParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokenbyintegratedwindowsauthparameterbuilder?view=azure-dotnet-preview#methods)bakın.

# <a name="java"></a>[Java](#tab/java)

Bu ekstre [MSAL Java dev örneklerinden.](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/public-client/)

```Java
private static IAuthenticationResult acquireTokenIwa() throws Exception {

    // Load token cache from file and initialize token cache aspect. The token cache will have
    // dummy data, so the acquireTokenSilently call will fail.
    TokenCacheAspect tokenCacheAspect = new TokenCacheAspect("sample_cache.json");

    PublicClientApplication pca = PublicClientApplication.builder(CLIENT_ID)
            .authority(AUTHORITY)
            .setTokenCacheAccessAspect(tokenCacheAspect)
            .build();

    Set<IAccount> accountsInCache = pca.getAccounts().join();
    // Take first account in the cache. In a production application, you would filter
    // accountsInCache to get the right account for the user authenticating.
    IAccount account = accountsInCache.iterator().next();

    IAuthenticationResult result;
    try {
        SilentParameters silentParameters =
                SilentParameters
                        .builder(SCOPE, account)
                        .build();

        // try to acquire token silently. This call will fail since the token cache
        // does not have any data for the user you are trying to acquire a token for
        result = pca.acquireTokenSilently(silentParameters).join();
    } catch (Exception ex) {
        if (ex.getCause() instanceof MsalException) {

            IntegratedWindowsAuthenticationParameters parameters =
                    IntegratedWindowsAuthenticationParameters
                            .builder(SCOPE, USER_NAME)
                            .build();

            // Try to acquire a IWA. You will need to generate a Kerberos ticket.
            // If successful, you should see the token and account information printed out to
            // console
            result = pca.acquireToken(parameters).join();
        } else {
            // Handle other exceptions accordingly
            throw ex;
        }
    }
    return result;
}
```

# <a name="python"></a>[Python](#tab/python)

Bu akış henüz MSAL Python'da desteklenmedi.

# <a name="macos"></a>[Macos](#tab/macOS)

Bu akış MacOS için geçerli değildir.

---

## <a name="username-and-password"></a>Kullanıcı adı ve parola

Ayrıca, kullanıcı adı ve parola sağlayarak bir belirteç elde edebilirsiniz. Bu akış sınırlıdır ve önerilmez, ancak yine de gerekli olan kullanım durumları vardır.

### <a name="this-flow-isnt-recommended"></a>Bu akış önerilmez

Uygulamanızın bir kullanıcıdan parolalarını istemesi güvenli olmadığından, bu akış *önerilmez.* Daha fazla bilgi için, [büyüyen parola sorununun çözümü nedir?](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/) Windows etki alanı birleştirilmiş makinelerde sessizce bir belirteç elde etmek için tercih edilen akış [Tümleşik Windows Kimlik Doğrulama.](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Integrated-Windows-Authentication) Ayrıca aygıt [kodu akışını](https://aka.ms/msal-net-device-code-flow)kullanabilirsiniz.

> [!NOTE]
> Bir kullanıcı adı ve parola kullanmak, DevOps senaryoları gibi bazı durumlarda yararlıdır. Ancak, kendi kullanıcı arabiriminizi sağladığınız etkileşimli senaryolarda bir kullanıcı adı ve parola kullanmak istiyorsanız, kullanıcı adı ve parolayı nasıl uzaklaştıracağınızı düşünün. Bir kullanıcı adı ve parola kullanarak, bir dizi şeyden vazgeçersiniz:
>
> - Modern kimliğin temel ilkeleri. Paylaşılan bir sır ele geçirilebildiği için parola şifrelenebilir ve yeniden oynatılabilir. Şifresiz ile bağdaşmaz.
> - MFA yapması gereken kullanıcılar etkileşim olmadığı için oturum açamıyor.
> - Kullanıcılar tek oturum açamaz (SSO).

### <a name="constraints"></a>Kısıtlamalar

Aşağıdaki kısıtlamalar da geçerlidir:

- Kullanıcı adı ve parola akışı koşullu erişim ve çok faktörlü kimlik doğrulama ile uyumlu değildir. Sonuç olarak, uygulamanız, kiracı yöneticisinin çok faktörlü kimlik doğrulaması gerektirdiği bir Azure AD kiracısında çalışıyorsa, bu akışı kullanamazsınız. Birçok kuruluş bunu yapar.
- Yalnızca iş ve okul hesapları için çalışır (MSA için değil).
- Akış .NET masaüstü ve .NET Core'da kullanılabilir, ancak UWP'de kullanılamaz.

### <a name="b2c-specifics"></a>B2C özellikleri

Daha fazla bilgi için [B2C'li Kaynak Sahibi Parola Kimlik Bilgileri'ne (ROPC)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics#resource-owner-password-credentials-ropc-with-b2c)bakın.

### <a name="use-it"></a>Kullanın

# <a name="net"></a>[.NET](#tab/dotnet)

`IPublicClientApplication`yöntemini `AcquireTokenByUsernamePassword`içerir.

Aşağıdaki örnek basitleştirilmiş bir durum sunar.

```csharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app;
 app = PublicClientApplicationBuilder.Create(clientId)
       .WithAuthority(authority)
       .Build();
 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
                    .ExecuteAsync();
 }
 else
 {
  try
  {
   var securePassword = new SecureString();
   foreach (char c in "dummy")        // you should fetch the password
    securePassword.AppendChar(c);  // keystroke by keystroke

   result = await app.AcquireTokenByUsernamePassword(scopes,
                                                    "joe@contoso.com",
                                                     securePassword)
                      .ExecuteAsync();
  }
  catch(MsalException)
  {
   // See details below
  }
 }
 Console.WriteLine(result.Account.Username);
}
```

Aşağıdaki örnek, alabileceğiniz özel durumlar ve bunların azaltıcı durumları ile ilgili açıklamalar la birlikte en güncel örneği sunar.

```csharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app;
 app = PublicClientApplicationBuilder.Create(clientId)
                                   .WithAuthority(authority)
                                   .Build();
 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
                    .ExecuteAync();
 }
 else
 {
  try
  {
   var securePassword = new SecureString();
   foreach (char c in "dummy")        // you should fetch the password keystroke
    securePassword.AppendChar(c);  // by keystroke

   result = await app.AcquireTokenByUsernamePassword(scopes,
                                                    "joe@contoso.com",
                                                    securePassword)
                    .ExecuteAsync();
  }
  catch (MsalUiRequiredException ex) when (ex.Message.Contains("AADSTS65001"))
  {
   // Here are the kind of error messages you could have, and possible mitigations

   // ------------------------------------------------------------------------
   // MsalUiRequiredException: AADSTS65001: The user or administrator has not consented to use the application
   // with ID '{appId}' named '{appName}'. Send an interactive authorization request for this user and resource.

   // Mitigation: you need to get user consent first. This can be done either statically (through the portal),
   /// or dynamically (but this requires an interaction with Azure AD, which is not possible with
   // the username/password flow)
   // Statically: in the portal by doing the following in the "API permissions" tab of the application registration:
   // 1. Click "Add a permission" and add all the delegated permissions corresponding to the scopes you want (for instance
   // User.Read and User.ReadBasic.All)
   // 2. Click "Grant/revoke admin consent for <tenant>") and click "yes".
   // Dynamically, if you are not using .NET Core (which does not have any Web UI) by
   // calling (once only) AcquireTokenInteractive.
   // remember that Username/password is for public client applications that is desktop/mobile applications.
   // If you are using .NET core or don't want to call AcquireTokenInteractive, you might want to:
   // - use device code flow (See https://aka.ms/msal-net-device-code-flow)
   // - or suggest the user to navigate to a URL to consent: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ErrorCode: invalid_grant
   // SubError: basic_action
   // MsalUiRequiredException: AADSTS50079: The user is required to use multi-factor authentication.
   // The tenant admin for your organization has chosen to oblige users to perform multi-factor authentication.
   // Mitigation: none for this flow
   // Your application cannot use the Username/Password grant.
   // Like in the previous case, you might want to use an interactive flow (AcquireTokenInteractive()),
   // or Device Code Flow instead.
   // Note this is one of the reason why using username/password is not recommended;
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ex.ErrorCode: invalid_grant
   // subError: null
   // Message = "AADSTS70002: Error validating credentials.
   // AADSTS50126: Invalid username or password
   // In the case of a managed user (user from an Azure AD tenant opposed to a
   // federated user, which would be owned
   // in another IdP through ADFS), the user has entered the wrong password
   // Mitigation: ask the user to re-enter the password
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ex.ErrorCode: invalid_grant
   // subError: null
   // MsalServiceException: ADSTS50034: To sign into this application the account must be added to
   // the {domainName} directory.
   // or The user account does not exist in the {domainName} directory. To sign into this application,
   // the account must be added to the directory.
   // The user was not found in the directory
   // Explanation: wrong username
   // Mitigation: ask the user to re-enter the username.
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "invalid_request")
  {
   // ------------------------------------------------------------------------
   // AADSTS90010: The grant type is not supported over the /common or /consumers endpoints.
   // Please use the /organizations or tenant-specific endpoint.
   // you used common.
   // Mitigation: as explained in the message from Azure AD, the authority you use in the application needs
   // to be tenanted or otherwise "organizations". change the
   // "Tenant": property in the appsettings.json to be a GUID (tenant Id), or domain name (contoso.com)
   // if such a domain is registered with your tenant
   // or "organizations", if you want this application to sign-in users in any Work and School accounts.
   // ------------------------------------------------------------------------

  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "unauthorized_client")
  {
   // ------------------------------------------------------------------------
   // AADSTS700016: Application with identifier '{clientId}' was not found in the directory '{domain}'.
   // This can happen if the application has not been installed by the administrator of the tenant or consented
   // to by any user in the tenant.
   // You may have sent your authentication request to the wrong tenant
   // Cause: The clientId in the appsettings.json might be wrong
   // Mitigation: check the clientId and the app registration
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "invalid_client")
  {
   // ------------------------------------------------------------------------
   // AADSTS70002: The request body must contain the following parameter: 'client_secret or client_assertion'.
   // Explanation: this can happen if your application was not registered as a public client application in Azure AD
   // Mitigation: in the Azure portal, edit the manifest for your application and set the `allowPublicClient` to `true`
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException)
  {
   throw;
  }

  catch (MsalClientException ex) when (ex.ErrorCode == "unknown_user_type")
  {
   // Message = "Unsupported User Type 'Unknown'. Please see https://aka.ms/msal-net-up"
   // The user is not recognized as a managed user, or a federated user. Azure AD was not
   // able to identify the IdP that needs to process the user
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "user_realm_discovery_failed")
  {
   // The user is not recognized as a managed user, or a federated user. Azure AD was not
   // able to identify the IdP that needs to process the user. That's for instance the case
   // if you use a phone number
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "unknown_user")
  {
   // the username was probably empty
   // ex.Message = "Could not identify the user logged into the OS. See https://aka.ms/msal-net-iwa for details."
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "parsing_wstrust_response_failed")
  {
   // ------------------------------------------------------------------------
   // In the case of a Federated user (that is owned by a federated IdP, as opposed to a managed user owned in an Azure AD tenant)
   // ID3242: The security token could not be authenticated or authorized.
   // The user does not exist or has entered the wrong password
   // ------------------------------------------------------------------------
  }
 }

 Console.WriteLine(result.Account.Username);
}
```

Uygulanabilecek tüm değiştiriciler hakkında daha fazla `AcquireTokenByUsernamePassword`bilgi için, [AcquireTokenByUsernamePasswordParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokenbyusernamepasswordparameterbuilder?view=azure-dotnet-preview#methods)bakın .

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki özü [MSAL Java dev örnekleri.](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/public-client/)

```Java
private static IAuthenticationResult acquireTokenUsernamePassword() throws Exception {

    // Load token cache from file and initialize token cache aspect. The token cache will have
    // dummy data, so the acquireTokenSilently call will fail.
    TokenCacheAspect tokenCacheAspect = new TokenCacheAspect("sample_cache.json");

    PublicClientApplication pca = PublicClientApplication.builder(CLIENT_ID)
            .authority(AUTHORITY)
            .setTokenCacheAccessAspect(tokenCacheAspect)
            .build();

    Set<IAccount> accountsInCache = pca.getAccounts().join();
    // Take first account in the cache. In a production application, you would filter
    // accountsInCache to get the right account for the user authenticating.
    IAccount account = accountsInCache.iterator().next();

    IAuthenticationResult result;
    try {
        SilentParameters silentParameters =
                SilentParameters
                        .builder(SCOPE, account)
                        .build();
        // try to acquire token silently. This call will fail since the token cache
        // does not have any data for the user you are trying to acquire a token for
        result = pca.acquireTokenSilently(silentParameters).join();
    } catch (Exception ex) {
        if (ex.getCause() instanceof MsalException) {

            UserNamePasswordParameters parameters =
                    UserNamePasswordParameters
                            .builder(SCOPE, USER_NAME, USER_PASSWORD.toCharArray())
                            .build();
            // Try to acquire a token via username/password. If successful, you should see
            // the token and account information printed out to console
            result = pca.acquireToken(parameters).join();
        } else {
            // Handle other exceptions accordingly
            throw ex;
        }
    }
    return result;
}
```

# <a name="python"></a>[Python](#tab/python)

Bu ekstre [MSAL Python dev örneklerinden.](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/dev/sample/)

```Python
# Create a preferably long-lived app instance which maintains a token cache.
app = msal.PublicClientApplication(
    config["client_id"], authority=config["authority"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )

# The pattern to acquire a token looks like this.
result = None

# Firstly, check the cache to see if this end user has signed in before
accounts = app.get_accounts(username=config["username"])
if accounts:
    logging.info("Account(s) exists in cache, probably with token too. Let's try.")
    result = app.acquire_token_silent(config["scope"], account=accounts[0])

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    # See this page for constraints of Username Password Flow.
    # https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki/Username-Password-Authentication
    result = app.acquire_token_by_username_password(
        config["username"], config["password"], scopes=config["scope"])
```

# <a name="macos"></a>[Macos](#tab/macOS)

Bu akış macOS için MSAL'da desteklenmez.

---

## <a name="command-line-tool-without-a-web-browser"></a>Web tarayıcısı olmayan komut satırı aracı

### <a name="device-code-flow"></a>Cihaz kodu akışı

Web denetimleri olmayan bir komut satırı aracı yazıyorsanız ve önceki akışları kullanamıyor veya kullanmak istemiyorsanız, aygıt kodu akışını kullanmanız gerekir.

Azure AD ile etkileşimli kimlik doğrulaması için bir web tarayıcısı gerektirir. Daha fazla bilgi için [bkz.](https://aka.ms/msal-net-uses-web-browser) Web tarayıcısı sağlamayan aygıtlarda veya işletim sistemlerindeki kullanıcıların kimliğini doğrulamak için, aygıt kodu akışı, kullanıcının etkileşimli olarak oturum açmak için bilgisayar veya cep telefonu gibi başka bir aygıtı kullanmasına olanak tanır. Uygulama, aygıt kodu akışını kullanarak, bu aygıtlar veya oSes için tasarlanmış iki adımlı bir işlem yoluyla belirteçleri elde eder. Bu tür uygulamalara örnek olarak iOT veya komut satırı araçları (CLI) üzerinde çalışan uygulamalar verilebilir. Fikir şudur:

1. Kullanıcı kimlik doğrulaması gerektiğinde, uygulama kullanıcı için bir kod sağlar. Kullanıcıdan, örneğin BIR URL'ye gitmek için internete bağlı akıllı telefon gibi `https://microsoft.com/devicelogin`başka bir cihaz kullanması istenir. Daha sonra kullanıcıkodu girmek için istenir. Bu yapılırsa, web sayfası kullanıcıya, gerekirse onay istemleri ve çok faktörlü kimlik doğrulamayı içeren normal bir kimlik doğrulama deneyimi yle yönlendirir.

2. Başarılı bir kimlik doğrulaması üzerine, komut satırı uygulaması gerekli belirteçleri bir arka kanal üzerinden alır ve bunları ihtiyaç duyduğu web API çağrılarını gerçekleştirmek için kullanır.

### <a name="use-it"></a>Kullanın

# <a name="net"></a>[.NET](#tab/dotnet)

`IPublicClientApplication`adlı `AcquireTokenWithDeviceCode`bir yöntem içerir.

```csharp
 AcquireTokenWithDeviceCode(IEnumerable<string> scopes,
                            Func<DeviceCodeResult, Task> deviceCodeResultCallback)
```

Bu yöntem parametre olarak alır:

- Bir `scopes` erişim belirteci istemek için.
- Alan bir geri `DeviceCodeResult`arama.

  ![DeviceCodeResult özellikleri](https://user-images.githubusercontent.com/13203188/56024968-7af1b980-5d11-11e9-84c2-5be2ef306dc5.png)

Aşağıdaki örnek kod, alabileceğiniz özel durumlar ve bunların hafifletilmesi yle ilgili açıklamalarla birlikte en güncel örneği sunar.

```csharp
private const string ClientId = "<client_guid>";
private const string Authority = "https://login.microsoftonline.com/contoso.com";
private readonly string[] Scopes = new string[] { "user.read" };

static async Task<AuthenticationResult> GetATokenForGraph()
{
    IPublicClientApplication pca = PublicClientApplicationBuilder
            .Create(ClientId)
            .WithAuthority(Authority)
            .WithDefaultRedirectUri()
            .Build();

    var accounts = await pca.GetAccountsAsync();

    // All AcquireToken* methods store the tokens in the cache, so check the cache first
    try
    {
        return await pca.AcquireTokenSilent(Scopes, accounts.FirstOrDefault())
            .ExecuteAsync();
    }
    catch (MsalUiRequiredException ex)
    {
        // No token found in the cache or AAD insists that a form interactive auth is required (e.g. the tenant admin turned on MFA)
        // If you want to provide a more complex user experience, check out ex.Classification

        return await AcquireByDeviceCodeAsync(pca);
    }         
}

private async Task<AuthenticationResult> AcquireByDeviceCodeAsync(IPublicClientApplication pca)
{
    try
    {
        var result = await pca.AcquireTokenWithDeviceCode(scopes,
            deviceCodeResult =>
            {
                    // This will print the message on the console which tells the user where to go sign-in using
                    // a separate browser and the code to enter once they sign in.
                    // The AcquireTokenWithDeviceCode() method will poll the server after firing this
                    // device code callback to look for the successful login of the user via that browser.
                    // This background polling (whose interval and timeout data is also provided as fields in the
                    // deviceCodeCallback class) will occur until:
                    // * The user has successfully logged in via browser and entered the proper code
                    // * The timeout specified by the server for the lifetime of this code (typically ~15 minutes) has been reached
                    // * The developing application calls the Cancel() method on a CancellationToken sent into the method.
                    //   If this occurs, an OperationCanceledException will be thrown (see catch below for more details).
                    Console.WriteLine(deviceCodeResult.Message);
                return Task.FromResult(0);
            }).ExecuteAsync();

        Console.WriteLine(result.Account.Username);
        return result;
    }
    // TODO: handle or throw all these exceptions depending on your app
    catch (MsalServiceException ex)
    {
        // Kind of errors you could have (in ex.Message)

        // AADSTS50059: No tenant-identifying information found in either the request or implied by any provided credentials.
        // Mitigation: as explained in the message from Azure AD, the authoriy needs to be tenanted. you have probably created
        // your public client application with the following authorities:
        // https://login.microsoftonline.com/common or https://login.microsoftonline.com/organizations

        // AADSTS90133: Device Code flow is not supported under /common or /consumers endpoint.
        // Mitigation: as explained in the message from Azure AD, the authority needs to be tenanted

        // AADSTS90002: Tenant <tenantId or domain you used in the authority> not found. This may happen if there are
        // no active subscriptions for the tenant. Check with your subscription administrator.
        // Mitigation: if you have an active subscription for the tenant this might be that you have a typo in the
        // tenantId (GUID) or tenant domain name.
    }
    catch (OperationCanceledException ex)
    {
        // If you use a CancellationToken, and call the Cancel() method on it, then this *may* be triggered
        // to indicate that the operation was cancelled.
        // See https://docs.microsoft.com/dotnet/standard/threading/cancellation-in-managed-threads
        // for more detailed information on how C# supports cancellation in managed threads.
    }
    catch (MsalClientException ex)
    {
        // Possible cause - verification code expired before contacting the server
        // This exception will occur if the user does not manage to sign-in before a time out (15 mins) and the
        // call to `AcquireTokenWithDeviceCode` is not cancelled in between
    }
}
```
# <a name="java"></a>[Java](#tab/java)

Bu ekstre [MSAL Java dev örneklerinden.](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/public-client/)

```java
private static IAuthenticationResult acquireTokenDeviceCode() throws Exception {

    // Load token cache from file and initialize token cache aspect. The token cache will have
    // dummy data, so the acquireTokenSilently call will fail.
    TokenCacheAspect tokenCacheAspect = new TokenCacheAspect("sample_cache.json");

    PublicClientApplication pca = PublicClientApplication.builder(CLIENT_ID)
            .authority(AUTHORITY)
            .setTokenCacheAccessAspect(tokenCacheAspect)
            .build();

    Set<IAccount> accountsInCache = pca.getAccounts().join();
    // Take first account in the cache. In a production application, you would filter
    // accountsInCache to get the right account for the user authenticating.
    IAccount account = accountsInCache.iterator().next();

    IAuthenticationResult result;
    try {
        SilentParameters silentParameters =
                SilentParameters
                        .builder(SCOPE, account)
                        .build();

        // try to acquire token silently. This call will fail since the token cache
        // does not have any data for the user you are trying to acquire a token for
        result = pca.acquireTokenSilently(silentParameters).join();
    } catch (Exception ex) {
        if (ex.getCause() instanceof MsalException) {

            Consumer<DeviceCode> deviceCodeConsumer = (DeviceCode deviceCode) ->
                    System.out.println(deviceCode.message());

            DeviceCodeFlowParameters parameters =
                    DeviceCodeFlowParameters
                            .builder(SCOPE, deviceCodeConsumer)
                            .build();

            // Try to acquire a token via device code flow. If successful, you should see
            // the token and account information printed out to console, and the sample_cache.json
            // file should have been updated with the latest tokens.
            result = pca.acquireToken(parameters).join();
        } else {
            // Handle other exceptions accordingly
            throw ex;
        }
    }
    return result;
}
```

# <a name="python"></a>[Python](#tab/python)

Bu ekstre [MSAL Python dev örneklerinden.](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/dev/sample/)

```Python
# Create a preferably long-lived app instance which maintains a token cache.
app = msal.PublicClientApplication(
    config["client_id"], authority=config["authority"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )

# The pattern to acquire a token looks like this.
result = None

# Note: If your device-flow app does not have any interactive ability, you can
#   completely skip the following cache part. But here we demonstrate it anyway.
# We now check the cache to see if we have some end users signed in before.
accounts = app.get_accounts()
if accounts:
    logging.info("Account(s) exists in cache, probably with token too. Let's try.")
    print("Pick the account you want to use to proceed:")
    for a in accounts:
        print(a["username"])
    # Assuming the end user chose this one
    chosen = accounts[0]
    # Now let's try to find a token in cache for this account
    result = app.acquire_token_silent(config["scope"], account=chosen)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")

    flow = app.initiate_device_flow(scopes=config["scope"])
    if "user_code" not in flow:
        raise ValueError(
            "Fail to create device flow. Err: %s" % json.dumps(flow, indent=4))

    print(flow["message"])
    sys.stdout.flush()  # Some terminal needs this to ensure the message is shown

    # Ideally you should wait here, in order to save some unnecessary polling
    # input("Press Enter after signing in from another device to proceed, CTRL+C to abort.")

    result = app.acquire_token_by_device_flow(flow)  # By default it will block
        # You can follow this instruction to shorten the block time
        #    https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.acquire_token_by_device_flow
        # or you may even turn off the blocking behavior,
        # and then keep calling acquire_token_by_device_flow(flow) in your own customized loop
```

# <a name="macos"></a>[Macos](#tab/macOS)

Bu akış MacOS için geçerli değildir.

---

## <a name="file-based-token-cache"></a>Dosya tabanlı belirteç önbelleği

MSAL.NET, varsayılan olarak bellek içi belirteç önbelleği sağlanır.

### <a name="serialization-is-customizable-in-windows-desktop-apps-and-web-apps-or-web-apis"></a>Serileştirme, Windows masaüstü uygulamalarında ve web uygulamalarında veya web API'larında özelleştirilebilir

.NET Framework ve .NET Core durumunda, ekstra bir şey yapmazsanız, bellek içi belirteç önbelleği uygulama süresince devam eder. Serileştirmenin neden kutudan sağlanmadığını anlamak için, MSAL .NET masaüstü veya .NET Core uygulamalarının konsol veya Windows uygulamaları (dosya sistemine erişimi olan) *değil, aynı zamanda* web uygulamaları veya web API'leri olabileceğini unutmayın. Bu web uygulamaları ve web API'leri veritabanları, dağıtılmış önbellekler ve Redis önbellekleri gibi bazı özel önbellek mekanizmaları kullanabilir. .NET masaüstünde veya .NET Core'da kalıcı bir belirteç önbelleği uygulamasına sahip olmak için serileştirmeyi özelleştirmeniz gerekir.

Belirteç önbelleği serileştirmesinde yer alan sınıflar ve arabirimler aşağıdaki türlerdir:

- ``ITokenCache``, belirteç önbelleği serileştirme isteklerine abone olmak için olayları ve önbelleği çeşitli biçimlerde serihale veya deserialize etme yöntemlerini tanımlar (ADAL v3.0, MSAL 2.x ve MSAL 3.x = ADAL v5.0).
- ``TokenCacheCallback``serileştirmeyi işleyebilir diye olaylara geçirilen bir geri aramadır. Onlar tür ``TokenCacheNotificationArgs``argümanlar ile çağrılacak.
- ``TokenCacheNotificationArgs``yalnızca başvuruyu ``ClientId`` ve belirteçlerin kullanılabildiği kullanıcıya bir başvuru sağlar.

  ![Belirteç önbellek serileştirme diyagramı](https://user-images.githubusercontent.com/13203188/56027172-d58d1480-5d15-11e9-8ada-c0292f1800b3.png)

> [!IMPORTANT]
> MSAL.NET sizin için belirteç önbellekleri `IToken` oluşturur ve bir uygulamanın `UserTokenCache` ve `AppTokenCache` özellikleri çağırdığınızda önbelleği sağlar. Arayüzü kendiniz uygulamamanız gerekiyor. Özel bir belirteç önbelleği serileştirme uyguladığınızda sorumluluğunuz şudur:
>
> - Tepki `BeforeAccess` ve `AfterAccess` olaylar, ya da Onların *Async* meslektaşı. Temsilci`BeforeAccess` önbelleği deserializing sorumludur. Temsilci `AfterAccess` önbelleği seri hale getirmekten sorumludur.
> - Bu olayların bir bölümünün, olay argümanından istediğiniz depolama alanına geçirilen lekeleri depoladığını veya yüklediğini anlayın.

Stratejiler, masaüstü veya web api veya daemon uygulaması gibi bir genel istemci uygulaması için belirteç önbelleği serileştirmesi yazıp yazmadığınıza bağlı olarak farklıdır.

MSAL v2.x'ten bu yana birkaç seçeneğiniz var. Seçiminiz önbelleği yalnızca MSAL ile birlikte platformlarda yaygın olan birleşik bir biçim önbelleği olan MSAL.NET biçimine seri hale getirmek isteyip istemediğinize bağlıdır. Veya, ADAL [v3'ün eski](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization) belirteç önbelleği serileştirmesini de desteklemek isteyebilirsiniz.

ADAL.NET 3.x, ADAL.NET 5.x ve MSAL.NET arasında SSO durumunu paylaşmak için belirteç önbelleği serileştirmeözelleştirmesi, örnek [active-directory-dotnet-v1-to-v2'nin](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2)bir bölümünde açıklanmıştır.

### <a name="simple-token-cache-serialization-msal-only"></a>Basit belirteç önbelleği serileştirme (yalnızca MSAL)

Aşağıdaki örnek, masaüstü uygulamaları için belirteç önbelleğinin özel serileştirilmesinin safça uygulanmasıdır. Burada, kullanıcı belirteç önbelleği uygulamayla aynı klasördeki bir dosyada dır.

Uygulamayı yaptıktan sonra, uygulamayı ``TokenCacheHelper.EnableSerialization()`` `UserTokenCache`arayarak ve geçirerek serileştirmeyi etkinleştirin.

```csharp
app = PublicClientApplicationBuilder.Create(ClientId)
    .Build();
TokenCacheHelper.EnableSerialization(app.UserTokenCache);
```

Bu yardımcı sınıf aşağıdaki kod parçacığı gibi görünür:

```csharp
static class TokenCacheHelper
 {
  public static void EnableSerialization(ITokenCache tokenCache)
  {
   tokenCache.SetBeforeAccess(BeforeAccessNotification);
   tokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Path to the token cache
  /// </summary>
  public static readonly string CacheFilePath = System.Reflection.Assembly.GetExecutingAssembly().Location + ".msalcache.bin3";

  private static readonly object FileLock = new object();


  private static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeMsalV3(File.Exists(CacheFilePath)
            ? ProtectedData.Unprotect(File.ReadAllBytes(CacheFilePath),
                                      null,
                                      DataProtectionScope.CurrentUser)
            : null);
   }
  }

  private static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     // reflect changesgs in the persistent store
     File.WriteAllBytes(CacheFilePath,
                         ProtectedData.Protect(args.TokenCache.SerializeMsalV3(),
                                                 null,
                                                 DataProtectionScope.CurrentUser)
                         );
    }
   }
  }
 }
```

Windows, Mac ve Linux'ta çalışan masaüstü uygulamaları için ortak istemci uygulamaları için ürün kalitesi belirteç önbelleği dosya tabanlı serializer [Microsoft.Identity.Client.Extensions.Msal](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Msal) açık kaynak kitaplığından edinilebilir. Aşağıdaki NuGet paketinden uygulamalarınıza ekleyebilirsiniz: [Microsoft.Identity.Client.Extensions.Msal](https://www.nuget.org/packages/Microsoft.Identity.Client.Extensions.Msal/).

> [!NOTE]
> Yasal Uyarı: Microsoft.Identity.Client.Extensions.Msal kitaplığı MSAL.NET üzerinde bir uzantıdır. Bu kitaplıktaki sınıflar, olduğu gibi veya kırılma değişiklikleriyle gelecekte MSAL.NET yol alabilir.

### <a name="dual-token-cache-serialization-msal-unified-cache--adal-v3"></a>Çift belirteç önbelleği serileştirme (MSAL birleşik önbellek + ADAL v3)

Birleşik önbellek biçimiyle belirteç önbelleği serileştirmesini uygulamak isteyebilirsiniz. Bu biçim, aynı platformda 4,x ve MSAL.NET 2,x ADAL.NET ve aynı nesil veya daha eski diğer MSAL'larla ortaktır. Aşağıdaki koddan ilham alın:

```csharp
string appLocation = Path.GetDirectoryName(Assembly.GetEntryAssembly().Location;
string cacheFolder = Path.GetFullPath(appLocation) + @"..\..\..\..");
string adalV3cacheFileName = Path.Combine(cacheFolder, "cacheAdalV3.bin");
string unifiedCacheFileName = Path.Combine(cacheFolder, "unifiedCache.bin");

IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
                                    .Build();
FilesBasedTokenCacheHelper.EnableSerialization(app.UserTokenCache,
                                               unifiedCacheFileName,
                                               adalV3cacheFileName);

```

Bu kez yardımcı sınıf aşağıdaki kod gibi görünür:

```csharp
using System;
using System.IO;
using System.Security.Cryptography;
using Microsoft.Identity.Client;

namespace CommonCacheMsalV3
{
 /// <summary>
 /// Simple persistent cache implementation of the dual cache serialization (ADAL V3 legacy
 /// and unified cache format) for a desktop applications (from MSAL 2.x)
 /// </summary>
 static class FilesBasedTokenCacheHelper
 {
  /// <summary>
  /// Get the user token cache
  /// </summary>
  /// <param name="adalV3CacheFileName">File name where the cache is serialized with the
  /// ADAL V3 token cache format. Can
  /// be <c>null</c> if you don't want to implement the legacy ADAL V3 token cache
  /// serialization in your MSAL 2.x+ application</param>
  /// <param name="unifiedCacheFileName">File name where the cache is serialized
  /// with the Unified cache format, common to
  /// ADAL V4 and MSAL V2 and above, and also across ADAL/MSAL on the same platform.
  ///  Should not be <c>null</c></param>
  /// <returns></returns>
  public static void EnableSerialization(ITokenCache cache, string unifiedCacheFileName, string adalV3CacheFileName)
  {
   UnifiedCacheFileName = unifiedCacheFileName;
   AdalV3CacheFileName = adalV3CacheFileName;

   cache.SetBeforeAccess(BeforeAccessNotification);
   cache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// File path where the token cache is serialized with the unified cache format
  /// (ADAL.NET V4, MSAL.NET V3)
  /// </summary>
  public static string UnifiedCacheFileName { get; private set; }

  /// <summary>
  /// File path where the token cache is serialized with the legacy ADAL V3 format
  /// </summary>
  public static string AdalV3CacheFileName { get; private set; }

  private static readonly object FileLock = new object();

  public static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeAdalV3(ReadFromFileIfExists(AdalV3CacheFileName));
    try
    {
     args.TokenCache.DeserializeMsalV3(ReadFromFileIfExists(UnifiedCacheFileName));
    }
    catch(Exception ex)
    {
     // Compatibility with the MSAL v2 cache if you used one
     args.TokenCache.DeserializeMsalV2(ReadFromFileIfExists(UnifiedCacheFileName));
    }
   }
  }

  public static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     WriteToFileIfNotNull(UnifiedCacheFileName, args.TokenCache.SerializeMsalV3());
     if (!string.IsNullOrWhiteSpace(AdalV3CacheFileName))
     {
      WriteToFileIfNotNull(AdalV3CacheFileName, args.TokenCache.SerializeAdalV3());
     }
    }
   }
  }

  /// <summary>
  /// Read the content of a file if it exists
  /// </summary>
  /// <param name="path">File path</param>
  /// <returns>Content of the file (in bytes)</returns>
  private static byte[] ReadFromFileIfExists(string path)
  {
   byte[] protectedBytes = (!string.IsNullOrEmpty(path) && File.Exists(path))
       ? File.ReadAllBytes(path) : null;
   byte[] unprotectedBytes = encrypt ?
       ((protectedBytes != null) ? ProtectedData.Unprotect(protectedBytes, null, DataProtectionScope.CurrentUser) : null)
       : protectedBytes;
   return unprotectedBytes;
  }

  /// <summary>
  /// Writes a blob of bytes to a file. If the blob is <c>null</c>, deletes the file
  /// </summary>
  /// <param name="path">path to the file to write</param>
  /// <param name="blob">Blob of bytes to write</param>
  private static void WriteToFileIfNotNull(string path, byte[] blob)
  {
   if (blob != null)
   {
    byte[] protectedBytes = encrypt
      ? ProtectedData.Protect(blob, null, DataProtectionScope.CurrentUser)
      : blob;
    File.WriteAllBytes(path, protectedBytes);
   }
   else
   {
    File.Delete(path);
   }
  }

  // Change if you want to test with an un-encrypted blob (this is a json format)
  private static bool encrypt = true;
 }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulamasından web API'sini arama](scenario-desktop-call-api.md)
