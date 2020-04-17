---
title: Web API(mobil uygulamalar) aramak için bir belirteç edinin | Azure
titleSuffix: Microsoft identity platform
description: Web API'lerini çağıran bir mobil uygulama nın nasıl oluşturulabildiğini öğrenin. (Uygulama için bir belirteç alın.)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 5750f4a5aa62b33c7d793b3e0c34f304ce1b187e
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535936"
---
# <a name="get-a-token-for-a-mobile-app-that-calls-web-apis"></a>Web API'lerini çağıran bir mobil uygulama için belirteç alma

Uygulamanız korumalı web API'lerini arayabilmesi için önce bir erişim belirteci gerekir. Bu makale, Microsoft Kimlik Doğrulama Kitaplığı (MSAL) kullanarak bir belirteç almak için işlem boyunca size yol.

## <a name="define-a-scope"></a>Kapsam tanımlama

Bir belirteç isteğinde bulunduğunuzda, bir kapsam tanımlamanız gerekir. Kapsam, uygulamanızın hangi verilere erişebileceğini belirler.

Bir kapsamı tanımlamanın en kolay yolu, istenen web `App ID URI` API'larını kapsamla `.default`birleştirmektir. Bu tanım, Microsoft kimlik platformuna uygulamanızın portalda ayarlanan tüm kapsamları gerektirdiğini söyler.

### <a name="android"></a>Android
```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
```

### <a name="ios"></a>iOS
```swift
let scopes = ["https://graph.microsoft.com/.default"]
```

### <a name="xamarin"></a>Xamarin
```csharp
var scopes = new [] {"https://graph.microsoft.com/.default"};
```

## <a name="get-tokens"></a>Jeton alın

### <a name="acquire-tokens-via-msal"></a>MSAL üzerinden jeton edinme

MSAL, uygulamaların jetonları sessizce ve etkileşimli olarak edinmesine olanak tanır. `AcquireTokenSilent()` Aradığınızda `AcquireTokenInteractive()`veya , MSAL istenen kapsamlar için bir erişim belirteci döndürür. Doğru desen sessiz bir istek te ve daha sonra etkileşimli bir istek geri düşmek etmektir.

#### <a name="android"></a>Android

```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);

// Check if there are any accounts we can sign in silently.
// Result is in the silent callback (success or error).
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {

        if (accounts.isEmpty() && accounts.size() == 1) {
            // TODO: Create a silent callback to catch successful or failed request.
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts or > 1 account. */
        }
    }
});

[...]

// No accounts found. Interactively request a token.
// TODO: Create an interactive callback to catch successful or failed requests.
sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
```

#### <a name="ios"></a>iOS

İlk sessizce bir belirteç edinme deneyin:

```objc

NSArray *scopes = @[@"https://graph.microsoft.com/.default"];
NSString *accountIdentifier = @"my.account.id";

MSALAccount *account = [application accountForIdentifier:accountIdentifier error:nil];

MSALSilentTokenParameters *silentParams = [[MSALSilentTokenParameters alloc] initWithScopes:scopes account:account];
[application acquireTokenSilentWithParameters:silentParams completionBlock:^(MSALResult *result, NSError *error) {

    if (!error)
    {
        // You'll want to get the account identifier to retrieve and reuse the account
        // for later acquireToken calls
        NSString *accountIdentifier = result.account.identifier;

        // Access token to call the web API
        NSString *accessToken = result.accessToken;
    }

    // Check the error
    if (error && [error.domain isEqual:MSALErrorDomain] && error.code == MSALErrorInteractionRequired)
    {
        // Interactive auth will be required, call acquireTokenWithParameters:error:
        return;
    }
}];
```

```swift

let scopes = ["https://graph.microsoft.com/.default"]
let accountIdentifier = "my.account.id"

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

    // You'll want to get the account identifier to retrieve and reuse the account
    // for later acquireToken calls
    let accountIdentifier = authResult.account.identifier

    // Access token to call the web API
    let accessToken = authResult.accessToken
}
```

MSAL dönerse, `MSALErrorInteractionRequired`belirteçleri etkileşimli olarak edinmeyi deneyin:

```objc
UIViewController *viewController = ...; // Pass a reference to the view controller that should be used when getting a token interactively
MSALWebviewParameters *webParameters = [[MSALWebviewParameters alloc] initWithAuthPresentationViewController:viewController];
MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopes webviewParameters:webParameters];
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

```swift
let viewController = ... // Pass a reference to the view controller that should be used when getting a token interactively
let webviewParameters = MSALWebviewParameters(authPresentationViewController: viewController)
let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopes, webviewParameters: webviewParameters)
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in

    guard let authResult = result, error == nil else {
        print(error!.localizedDescription)
        return
    }

    // Get access token from result
    let accessToken = authResult.accessToken
})
```

iOS ve macOS için MSAL, etkileşimli veya sessizce bir belirteç elde etmek için çeşitli değiştiriciler destekler:
* [Belirteç almak için ortak parametreler](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALTokenParameters.html#/Configuration%20parameters)
* [Etkileşimli belirteç almak için parametreler](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALInteractiveTokenParameters.html#/Configuring%20MSALInteractiveTokenParameters)
* [Sessiz bir belirteç almak için parametreler](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALSilentTokenParameters.html)

#### <a name="xamarin"></a>Xamarin

Aşağıdaki örnekte, etkileşimli olarak bir belirteç almak için en az kodu gösterilmektedir. Örnek, kullanıcının profilini okumak için Microsoft Graph'ı kullanır.

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

#### <a name="mandatory-parameters-in-msalnet"></a>MSAL.NET zorunlu parametreler

`AcquireTokenInteractive`yalnızca bir zorunlu parametresi vardır: `scopes`. Parametre, `scopes` belirteç gerektiren kapsamları tanımlayan dizeleri numaralandırır. Belirteç Microsoft Graph içinse, her Microsoft Graph API'sinin API başvurusunda gerekli kapsamları bulabilirsiniz. Başvuruda, "İzinler" bölümüne gidin.

Örneğin, [kullanıcının ilgili kişilerini listelemek](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_contacts)için "User.Read", "Contacts.Read" kapsamını kullanın. Daha fazla bilgi için [Microsoft Graph izinleri başvurusuna](https://developer.microsoft.com/graph/docs/concepts/permissions_reference)bakın.

Android'de, uygulamayı oluştururken üst etkinliği kullanarak `PublicClientApplicationBuilder`belirtebilirsiniz. Ana etkinliği o anda belirtmezseniz, daha sonra aşağıdaki `.WithParentActivityOrWindow` bölümde ki gibi kullanarak belirtebilirsiniz. Üst etkinliği belirtirseniz, belirteç etkileşimden sonra bu üst aktiviteye geri döner. Belirtmezseniz, `.ExecuteAsync()` arama bir özel durum oluşturur.

#### <a name="specific-optional-parameters-in-msalnet"></a>MSAL.NET'da belirli isteğe bağlı parametreler

Aşağıdaki bölümlerde isteğe bağlı parametreler MSAL.NET.

##### <a name="withprompt"></a>WithPrompt

Parametre, `WithPrompt()` bir istem belirterek kullanıcıyla etkileşimi denetler.

<img src="https://user-images.githubusercontent.com/13203188/53438042-3fb85700-39ff-11e9-9a9e-1ff9874197b3.png" width="25%" />

Sınıf aşağıdaki sabitleri tanımlar:

- `SelectAccount`güvenlik belirteci hizmetini (STS) hesap seçimi iletişim kutusunu sunmaya zorlar. İletişim kutusu, kullanıcının oturum yaptığı hesapları içerir. Kullanıcının farklı kimlikler arasından seçim seçmesine izin vermek istediğinizde bu seçeneği kullanabilirsiniz. Bu seçenek, MSAL'ı kimlik sağlayıcısına göndermeye `prompt=select_account` iter.

    Sabit `SelectAccount` varsayılandır ve kullanılabilir bilgilere dayalı olarak mümkün olan en iyi deneyimi etkin bir şekilde sağlar. Kullanılabilir bilgiler hesap, kullanıcı için bir oturumun varlığı ve benzeri içerebilir. Bunu yapmak için iyi bir nedeniniz yoksa bu varsayılanı değiştirmeyin.
- `Consent`daha önce izin verilmiş olsa bile kullanıcıdan onay istemenizi sağlar. Bu durumda, MSAL `prompt=consent` kimlik sağlayıcısına gönderir.

    Kuruluş yönetiminin kullanıcıların `Consent` uygulamayı her kullandıklarında onay iletişim kutusunu görmelerini gerektirdiği güvenlik odaklı uygulamalarda sabiti kullanmak isteyebilirsiniz.
- `ForceLogin`komut istemi gerekli olmasa bile, hizmetin kullanıcıdan kimlik bilgilerini istemesini sağlar.

    Belirteç edinimi başarısız olursa ve kullanıcının yeniden oturum açmasına izin vermek istiyorsanız, bu seçenek yararlı olabilir. Bu durumda, MSAL `prompt=login` kimlik sağlayıcısına gönderir. Bu seçeneği, kuruluş yönetiminin kullanıcının uygulamanın belirli bölümlerine her erişirinde oturum açmasını gerektirdiği güvenlik odaklı uygulamalarda kullanmak isteyebilirsiniz.
- `Never`sadece .NET 4.5 ve Windows Runtime (WinRT) içindir. Bu sabit kullanıcıyı isteyaramaz, ancak gizli katıştırılmış web görünümünde depolanan çerezi kullanmaya çalışır. Daha fazla bilgi için bkz: [MSAL.NET ile web tarayıcılarını kullanma.](https://docs.microsoft.com/azure/active-directory/develop/msal-net-web-browsers)

    Bu seçenek başarısız `AcquireTokenInteractive` olursa, bir ui etkileşimi gerekli olduğunu bildirmek için bir özel durum atar. Sonra başka bir `Prompt` parametre kullanmanız gerekir.
- `NoPrompt`kimlik sağlayıcısına bir istem göndermez.

    Bu seçenek yalnızca Azure Etkin Dizin B2C'deki profil oluşturma ilkeleri için yararlıdır. Daha fazla bilgi için [B2C ayrıntılarına](https://aka.ms/msal-net-b2c-specificities)bakın.

##### <a name="withextrascopetoconsent"></a>Withextrascopetoconsent

Kullanıcının `WithExtraScopeToConsent` çeşitli kaynaklara önceden onay vermesini istediğiniz gelişmiş bir senaryoda değiştirici'yi kullanın. Normalde MSAL.NET veya Microsoft kimlik platformu 2.0 ile kullanılan artımlı onayı kullanmak istemiyorsanız bu değiştirici kullanabilirsiniz. Daha fazla bilgi için [bkz.](scenario-desktop-production.md#have-the-user-consent-upfront-for-several-resources)

Aşağıda bir kod örneği verilmiştir:

```csharp
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

##### <a name="other-optional-parameters"></a>Diğer isteğe bağlı parametreler

Diğer isteğe bağlı parametreler `AcquireTokenInteractive`hakkında bilgi edinmek için, [AcquireTokenInteractiveParameterBuilder için referans belgelerine](/dotnet/api/microsoft.identity.client.acquiretokeninteractiveparameterbuilder?view=azure-dotnet-preview#methods)bakın.

### <a name="acquire-tokens-via-the-protocol"></a>Protokol aracılığıyla belirteçler edinme

Jeton almak için protokolü doğrudan kullanmanızı önermiyoruz. Bunu yaparsanız, uygulama tek oturum açma (SSO), aygıt yönetimi ve koşullu erişim içeren bazı senaryoları desteklemez.

Mobil uygulamalar için belirteçler almak için protokolü kullandığınızda, iki istekte bulunun:

* Bir yetkilendirme kodu alın.
* Kodu bir belirteçle değiştirin.

#### <a name="get-an-authorization-code"></a>Yetkilendirme kodu alma

```Text
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=<CLIENT_ID>
&response_type=code
&redirect_uri=<ENCODED_REDIRECT_URI>
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2F.default
&state=12345
```

#### <a name="get-access-and-refresh-the-token"></a>Erişim elde edin ve belirteçleri yenileyin

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=<CLIENT_ID>
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=<ENCODED_REDIRECT_URI>
&grant_type=authorization_code
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'sini çağırma](scenario-mobile-call-api.md)
