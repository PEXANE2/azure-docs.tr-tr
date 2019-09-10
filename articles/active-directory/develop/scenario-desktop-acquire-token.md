---
title: Web API 'Lerini çağıran masaüstü uygulaması (uygulama için bir belirteç alınıyor)-Microsoft Identity platform
description: Web API 'Lerini çağıran bir masaüstü uygulaması oluşturmayı öğrenin (uygulama için bir belirteç alınıyor |)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5409b5619f8be16ef92f517b4b598e2a8e5e2b7
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872825"
---
# <a name="desktop-app-that-calls-web-apis---acquire-a-token"></a>Web API 'Lerini çağıran masaüstü uygulaması-belirteç alma

Bunu oluşturduktan sonra, bir Web API 'si çağırmak için kullanacağınız bir belirteç elde etmek üzere onu kullanacaksınız. `IPublicClientApplication`

## <a name="recommended-pattern"></a>Önerilen model

Web API 'SI tarafından `scopes`tanımlanır. Uygulamanızda sağladığınız deneyim ne olursa olsun, kullanmak istediğiniz model şu şekilde olur:

- Şunu çağırarak belirteç önbelleğinden belirteç almaya çalışıyor`AcquireTokenSilent`
- Bu çağrı başarısız olursa, kullanmak istediğiniz `AcquireToken` akışı kullanın (burada tarafından `AcquireTokenXX`temsil edilir)

```CSharp
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

Artık, bir masaüstü uygulamasında belirteçleri edinmenin çeşitli yollarının ayrıntıları aşağıda verilmiştir

## <a name="acquiring-a-token-interactively"></a>Bir belirteci etkileşimli olarak alma

Aşağıdaki örnek, Microsoft Graph ile kullanıcının profilini okumak üzere bir belirteci etkileşimli olarak almak için minimum kodu gösterir.

```CSharp
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

`AcquireTokenInteractive`yalnızca bir zorunlu parametreye ``scopes``sahiptir ve bu, belirtecin gerekli olduğu kapsamları tanımlayan dizelerin bir listesini içerir. Belirteç Microsoft Graph için ise, gereken kapsamlar, "Permissions" adlı bölümdeki her bir Microsoft Graph API 'sinin API başvurusunda bulunabilir. Örneğin, [kullanıcının kişilerini listelemek](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_contacts)için, "User. Read", "Contacts. Read" kapsamının kullanılması gerekecektir. Ayrıca bkz. [Microsoft Graph izinleri başvurusu](https://developer.microsoft.com/graph/docs/concepts/permissions_reference).

Android 'de, belirtecin etkileşime geçtikten sonra bu üst etkinliğe geri dönmesi için `.WithParentActivityOrWindow`üst etkinliği de belirtmeniz gerekir (kullanarak aşağıya bakın). Bunu belirtmezseniz, çağrılırken `.ExecuteAsync()`bir özel durum oluşturulur.

### <a name="specific-optional-parameters"></a>Belirli isteğe bağlı parametreler

#### <a name="withparentactivityorwindow"></a>WithParentActivityOrWindow

Etkileşimli çalışıyor, UI önemlidir. `AcquireTokenInteractive`, kendisini destekleyen platformlar, üst Kullanıcı arabirimini desteklemek için, isteğe bağlı bir isteğe bağlı parametre içerir. Bir masaüstü uygulamasında kullanıldığında, `.WithParentActivityOrWindow` platforma bağlı olarak farklı bir tür vardır:

```CSharp
// net45
WithParentActivityOrWindow(IntPtr windowPtr)
WithParentActivityOrWindow(IWin32Window window)

// Mac
WithParentActivityOrWindow(NSWindow window)

// .Net Standard (this will be on all platforms at runtime, but only on NetStandard at build time)
WithParentActivityOrWindow(object parent).
```

Açıklamalarının

- `object` .NET Standard, beklenen `Activity` Android, `UIViewController` ave`IWin32Window` iOS 'ta, Mac `IntPr` 'teveveyaWindowsüzerindebir.`NSWindow`
- Windows 'da, katıştırılmış tarayıcının uygun `AcquireTokenInteractive` UI Eşitleme bağlamını alması için UI iş parçacığından çağırmanız gerekir.  UI iş parçacığından çağrılmayan iletiler, kullanıcı arabiriminden doğru şekilde ve/veya kilitlenme senaryolarında karşılıklı olarak alınmamasına neden olabilir. Kullanıcı arabirimi iş parçacığında yoksa, `Dispatcher` WPF 'de kullanmak için Kullanıcı arabirimi iş parçacığından msal çağırmanın bir yolu.
- WPF kullanıyorsanız, WPF denetiminden bir pencere almak için sınıfını kullanabilirsiniz `WindowInteropHelper.Handle` . Daha sonra, bir WPF denetiminden (`this`) çağrı yapılır:
  
  ```CSharp
  result = await app.AcquireTokenInteractive(scopes)
                    .WithParentActivityOrWindow(new WindowInteropHelper(this).Handle)
                    .ExecuteAsync();
  ```

#### <a name="withprompt"></a>WithPrompt

`WithPrompt()`bir Istem belirterek kullanıcıyla etkileşimi denetlemek için kullanılır

<img src="https://user-images.githubusercontent.com/13203188/53438042-3fb85700-39ff-11e9-9a9e-1ff9874197b3.png" width="25%" />

Sınıfı aşağıdaki sabitleri tanımlar:

- ``SelectAccount``: STS 'nin, kullanıcının oturumu olan hesapları içeren hesap seçme iletişim kutusunu sunmasını zorlayacaktır. Bu seçenek, uygulamalar geliştiriciler kullanıcıların farklı kimlikler arasında seçim yapmasına izin vermek istediğinizde yararlıdır. Bu seçenek, msal 'in kimlik ``prompt=select_account`` sağlayıcısına gönderilmesini sağlar. Bu seçenek varsayılandır ve kullanılabilir bilgilere (hesap, Kullanıcı için bir oturumun varlığına vb.) göre olası en iyi deneyimi sağlamanın iyi bir işidir. ...). Bunu yapmak için iyi bir nedeniniz olmadığı müddetçe değiştirmeyin.
- ``Consent``: daha önce izin verilse bile, uygulama geliştiricisinin kullanıcıya onay istenmesini sağlar. Bu durumda, msal kimlik sağlayıcısına `prompt=consent` gönderilir. Bu seçenek, kuruluşun yönetilebileceği bazı güvenlik odaklı uygulamalarda, uygulamanın her kullanıldığı her seferinde kullanıcıdan onay iletişim kutusunu sunduğunu talep etmek için kullanılabilir.
- ``ForceLogin``: Bu Kullanıcı istemi gerekli olmasa bile, uygulama geliştiricisinin kullanıcıya hizmet tarafından kimlik bilgileri istenmesini sağlar. Bu seçenek, kullanıcının yeniden oturum açmasını sağlamak için bir belirteç alma başarısız olursa yararlı olabilir. Bu durumda, msal kimlik sağlayıcısına `prompt=login` gönderilir. Bu durumda, kuruluşun idare ettiği bazı güvenlik odaklı uygulamalarda, uygulamanın belirli bölümlerine her erişirken kullanıcı tarafından yeniden oturum açtığı görüldü.
- ``Never``(yalnızca .NET 4,5 ve WinRT için) kullanıcıya sormaz, bunun yerine gizli katıştırılmış Web görünümünde depolanan tanımlama bilgisini kullanmayı dener (aşağıya bakın: MSAL.NET 'de Web görünümleri). Bu seçeneğin kullanılması başarısız olabilir ve bu durumda `AcquireTokenInteractive` , Kullanıcı arabirimi etkileşiminin gerekli olduğunu bildirmek için bir özel durum oluşturulur ve başka `Prompt` bir parametre kullanmanız gerekir.
- ``NoPrompt``: Kimlik sağlayıcısına hiçbir istem göndermez. Bu seçenek yalnızca Azure AD B2C düzenleme profili ilkeleri için yararlıdır ( [B2C özelliklerine](https://aka.ms/msal-net-b2c-specificities)bakın).

#### <a name="withextrascopetoconsent"></a>Withextrascopetoonay

Bu değiştirici, kullanıcının birden fazla kaynağa ön onay vermesini istediğiniz (ve normalde MSAL.NET/Microsoft Identity platformu ile kullanılan artımlı onayı kullanmak istemediğiniz) gelişmiş bir senaryoda kullanılır. Ayrıntılar için bkz. [nasıl yapılır: Kullanıcı onayını birkaç kaynağın önüne](scenario-desktop-production.md#how-to-have--the-user-consent-upfront-for-several-resources)alma.

```CSharp
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

#### <a name="withcustomwebui"></a>WithCustomWebUi

Web Kullanıcı arabirimi, tarayıcıyı çağırmak için bir mekanizmadır. Bu mekanizma, adanmış bir UI WebBrowser denetimi veya tarayıcıyı açmayı devretmek için bir yol olabilir.
MSAL çoğu platform için Web ARABIRIMI uygulamaları sağlar, ancak tarayıcıyı kendiniz barındırmak isteyebileceğiniz durumlar da vardır: 

- MSAL tarafından açıkça kapsanmayan platformlar, örneğin, Blazor, Unity, mono on Desktop
- uygulamanızı test etmek ve Selenium ile kullanılabilecek otomatik bir tarayıcı kullanmak istiyor musunuz? 
- tarayıcı ve MSAL çalıştıran uygulama ayrı işlemlerdir

##### <a name="at-a-glance"></a>Bir bakışta

Bunu başarmak için, son kullanıcının Kullanıcı adını girebilmesi `start Url`için bir tarayıcıda gösterilmesi gereken msal a 'ya sahip olursunuz. Kimlik doğrulaması tamamlandıktan sonra, uygulamanızın Azure AD tarafından sunulan bir kodu içeren msal `end Url`öğesine geri geçirmesi gerekir.
Uygulamasının `end Url` Konağı`redirectUri`her zaman olur. `end Url` Bunu yapmak için şunları yapabilirsiniz: 

- `redirect Url` e-p veya
- tarayıcınızın bir URL 'ye yönlendirilmesini sağlamak için

##### <a name="withcustomwebui-is-an-extensibility-point"></a>WithCustomWebUi bir genişletilebilirlik noktasıdır

`WithCustomWebUi`, genel istemci uygulamalarında kendi Kullanıcı arabiriminizi sağlamanıza ve kullanıcının kimlik sağlayıcısının/Yetkilendir uç noktasında dolaşmasına izin vermek ve oturum açmasını ve izin vermesini sağlamak için bir genişletilebilirlik noktasıdır. MSAL.NET, ardından kimlik doğrulama kodunu kullanabilir ve bir belirteç alabilir. Bu, Visual Studio 'da, elektriler uygulamalarının (örneğin, geri bildirim için) Web etkileşimi sağlaması için kullanıldığı, ancak işin büyük bir kısmını MSAL.NET için bırakın. UI Otomasyonu sağlamak isterseniz de kullanabilirsiniz. Ortak istemci uygulamalarında MSAL.NET, güvenliğin sağlandığından emin olmak için PCE standardını ([OAuth ortak istemciler tarafından kod alışverişi Için RFC 7636-kanıt anahtarı](https://tools.ietf.org/html/rfc7636)) kullanır: Kodu yalnızca MSAL.NET kullanabilir.

  ```CSharp
  using Microsoft.Identity.Client.Extensions;
  ```

##### <a name="how-to-use-withcustomwebui"></a>WithCustomWebUi kullanma

Kullanmak `.WithCustomWebUI`için şunları yapmanız gerekir:
  
  1. Arabirimi uygulayın (buraya bakın. [](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/src/Microsoft.Identity.Client/Extensibility/ICustomWebUI.cs#L32-L70) `ICustomWebUi` Genellikle yetkilendirme kodu URL 'sini kabul eden bir `AcquireAuthorizationCodeAsync` yöntemi uygulamanız gerekir (msal.NET tarafından hesaplanır), kullanıcının kimlik sağlayıcısıyla etkileşime geçmesine izin vermek ve ardından kimlik sağlayıcısının Uygulamanızı geri çağırmalı (yetkilendirme kodu dahil). Sorunlarınız varsa, uygulamanız msal ile sorunsuz bir şekilde çalışmak `MsalExtensionException` için bir özel durum oluşturması gerekir.
  2. Çağrın, özel Web UI 'nizin örneğini geçirmeyi değiştirici kullanabilirsiniz `.WithCustomUI()` `AcquireTokenInteractive`

     ```CSharp
     result = await app.AcquireTokenInteractive(scopes)
                       .WithCustomWebUi(yourCustomWebUI)
                       .ExecuteAsync();
     ```

##### <a name="examples-of-implementation-of-icustomwebui-in-test-automation---seleniumwebui"></a>Test Otomasyonu 'nda ıcustomwebui 'ın uygulama örnekleri-SeleniumWebUI

MSAL.NET ekibi, bu genişletilebilirlik mekanizmasından yararlanmak için UI testlerimizi yeniden yazan. İlginizi çekiyor olmanız durumunda MSAL.NET kaynak kodundaki [Seleniumwebui](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/tests/Microsoft.Identity.Test.Integration/Infrastructure/SeleniumWebUI.cs#L15-L160) sınıfına bakabilirsiniz

##### <a name="providing-a-great-experience-with-systemwebviewoptions"></a>SystemWebViewOptions ile harika bir deneyim sağlama

MSAL.NET 4,1 [`SystemWebViewOptions`](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.systemwebviewoptions?view=azure-dotnet) ' den şunları belirtmenizi sağlar:

- Sistem Web tarayıcısında oturum açma/`BrowserRedirectError`onay hataları durumunda görüntülenecek olan URI () veya`HtmlMessageError`HTML parçası ()
- başarılı oturum açma/onay durumunda`BrowserRedirectSuccess`görüntülenecek olan URI () veya HTML parçası (`HtmlMessageSuccess`).
- Sistem tarayıcısını başlatmak için çalıştırılacak eylem. Bunun için `OpenBrowserAsync` temsilciyi ayarlayarak kendi uygulamanızı sağlayabilirsiniz. Sınıfı ayrıca iki tarayıcı için varsayılan bir uygulama sağlar: `OpenWithEdgeBrowserAsync` ve `OpenWithChromeEdgeBrowserAsync`sırasıyla, Microsoft Edge ve [Microsoft Edge için kmıum üzerinde](https://www.windowscentral.com/faq-edge-chromium).

Bu yapıyı kullanmak için aşağıdakine benzer bir ad yazabilirsiniz:

```CSharp
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

[Acquiretokenınteractiveparameterbuilder](/dotnet/api/microsoft.identity.client.acquiretokeninteractiveparameterbuilder?view=azure-dotnet-preview#methods) için başvuru `AcquireTokenInteractive` belgelerinden diğer tüm isteğe bağlı parametreler hakkında daha fazla bilgi edinin

## <a name="integrated-windows-authentication"></a>Tümleşik Windows kimlik doğrulaması

Bir etki alanı veya Azure AD 'ye katılmış makinede bir etki alanı kullanıcısı oturumu açmak istiyorsanız şunu kullanmanız gerekir:

```csharp
AcquireTokenByIntegratedWindowsAuth(IEnumerable<string> scopes)
```

### <a name="constraints"></a>Kısıtlamalar

- Acquiretokenbyıntegratedwindowsauth (ıWA) yalnızca yalnızca **Federal** kullanıcılar için kullanılabilir, diğer bir deyişle, bir Active Directory ve Azure Active Directory tarafından desteklenir. AD ile **yönetilen** kullanıcılar olmadan doğrudan AAD 'de oluşturulan kullanıcılar-bu kimlik doğrulama akışını kullanamaz. Bu sınırlama, Kullanıcı adı/parola akışını etkilemez.
- IWA .NET Framework, .NET Core ve UWP platformları için yazılan uygulamalar içindir
- IWA MFA 'yı atlamaz (Multi Factor Authentication). MFA yapılandırıldıysa, MFA Kullanıcı etkileşimi gerektirdiğinden, bir MFA sınaması gerekliyse ıWA başarısız olabilir.
  > [!NOTE]
  > Bu, karmaşık bir değer. IWA etkileşimli değil, ancak MFA Kullanıcı etkileşimi gerektirir. Kimlik sağlayıcısının MFA istediğinde, kiracı yöneticisinin ne zaman yapılacağını kontrol edersiniz. Gözlemlerimizde, farklı bir ülkeden oturum açtığınızda MFA gerekir, bu, VPN üzerinden bir kurumsal ağa bağlanmaz ve bazen VPN aracılığıyla bağlandığında bile olur. Belirleyici bir kural kümesi beklemediğinden Azure Active Directory, MFA 'yı sürekli olarak öğrenmek için AI 'yi kullanır. IWA başarısız olursa, bir Kullanıcı istemine (etkileşimli kimlik doğrulama veya cihaz kodu akışı) geri dönüş yapmanız gerekir.

- Şu şekilde geçirilen `PublicClientApplicationBuilder` yetkilinin olması gerekir:
  - Kiracı tarafından alınan ( `https://login.microsoftonline.com/{tenant}/` `tenant` , Kiracı kimliğini temsil eden GUID veya kiracı ile ilişkili bir etki alanı).
  - herhangi bir iş ve okul hesabı (`https://login.microsoftonline.com/organizations/`) için
  - Microsoft kişisel hesapları desteklenmez (sık karşılaşılan veya/tüketiciler kiracılarını kullanamazsınız)

- Tümleşik Windows kimlik doğrulaması, sessiz bir akışdır:
  - Uygulamanızın kullanıcısının, uygulamayı kullanmak için önceden verilmiş olması gerekir
  - ya da kiracı yöneticisinin uygulamayı kullanabilmesi için Kiracıdaki tüm kullanıcılara daha önce sahip olması gerekir.
  - Diğer bir deyişle:
    - geliştirici için Azure portal bir geliştirici, sizin için olan **ver** düğmesine bastınız.
    - ya da bir kiracı yöneticisi, uygulamanın kaydının **API izinleri** sekmesindeki **{kiracı etki alanı} Için yönetici onayı verme/iptal etme/iptal** düğmesine bastına (bkz. [Web API 'lerine erişim izinleri ekleme](https://docs.microsoft.com/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-permissions-to-access-web-apis))
    - ya da kullanıcıların uygulamayı onaylaması için bir yol sağladınız (bkz. [bireysel kullanıcı Izni isteme](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-individual-user-consent))
    - ya da kiracı yöneticisinin uygulamayı onaylaması için bir yol sağladınız (bkz. [yönetici onayı](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant))

- Bu akış .net Masaüstü, .NET Core ve Windows Universal (UWP) uygulamaları için etkinleştirilmiştir. .NET Core 'da, .NET Core platformu kullanıcı adını işletim sistemine sormamakta olduğundan, yalnızca Kullanıcı adını alan aşırı yükleme kullanılabilir.
  
Onay hakkında daha fazla bilgi için bkz. [Microsoft Identity platform izinleri ve onayı](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent)

### <a name="how-to-use-it"></a>Kullanımı

Normalde yalnızca bir parametreye (`scopes`) ihtiyacınız vardır. Ancak, Windows yöneticinizin ilkeleri kurma şekline bağlı olarak, Windows makinenizde uygulamaların oturum açan kullanıcıyı arama izni yoktur. Bu durumda, ikinci bir yöntemi `.WithUsername()` kullanın ve oturum açmış kullanıcının Kullanıcı adını UPN `joe@contoso.com`biçimi olarak geçirin.

Aşağıdaki örnek, alacağınız özel durumların türüyle ilgili açıklamalar ve bunların azaltmaları ile en güncel durumu gösterir

```CSharp
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

AcquireTokenByIntegratedWindowsAuthentication üzerindeki olası değiştiricilerin listesi için bkz. [Acquiretokenbyıntegratedwindowsauthparameterbuilder](/dotnet/api/microsoft.identity.client.acquiretokenbyintegratedwindowsauthparameterbuilder?view=azure-dotnet-preview#methods)

## <a name="username--password"></a>Kullanıcı adı/parola

Ayrıca, Kullanıcı adı ve parola sağlayarak bir belirteç elde edebilirsiniz. Bu akış sınırlı ve önerilmez, ancak gerekli olduğu durumlarda hala kullanım durumları vardır.

### <a name="this-flow-isnt-recommended"></a>Bu akış önerilmez

Bu akış **önerilmez** çünkü uygulamanız için bir kullanıcıdan parola isteyen güvenli değildir. Bu sorun hakkında daha fazla bilgi için [Bu makaleye](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/)bakın. Windows etki alanına katılmış makinelerde sessizce belirteç almak için tercih edilen akış, [Windows kimlik doğrulaması ' nı tümleştirilmiştir](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Integrated-Windows-Authentication). Aksi takdirde, [cihaz kod akışını](https://aka.ms/msal-net-device-code-flow) da kullanabilirsiniz

> [!NOTE] 
> Bu, bazı durumlarda (DevOps senaryolarında) yararlı olsa da, onw Kullanıcı arabirimini sağladığınız Etkileşimli senaryolarda Kullanıcı adı/parola kullanmak istiyorsanız, bundan uzaklaşmaya gerçekten göz önünde bulundurun. Kullanıcı adı/parola kullanarak, bir dizi şey verirsiniz:
>
> - Modern kimlik için temel kiracılar: parola, yeniden yürütülür ve yeniden yürütülür. Bu, ele geçirilebilecek bir paylaşılan gizlilik kavramımız için.
> Bu, passwordless ile uyumsuzdur.
> - MFA yapması gereken kullanıcılar oturum açamaz (hiçbir etkileşim yoktur)
> - Kullanıcılar çoklu oturum açma yapamaz

### <a name="constraints"></a>Kısıtlamalar

Aşağıdaki kısıtlamalar da geçerlidir:

- Kullanıcı adı/parola akışı, koşullu erişim ve Multi-Factor Authentication ile uyumlu değil: Sonuç olarak, uygulamanız kiracı yöneticisinin çok faktörlü kimlik doğrulaması gerektirdiği bir Azure AD kiracısında çalışıyorsa, bu akışı kullanamazsınız. Birçok kuruluş bu şekilde yapılır.
- Yalnızca Iş ve okul hesapları için çalışır (MSA değil)
- Flow, .net masaüstü ve .NET Core 'ta mevcuttur, ancak UWP 'de değildir

### <a name="b2c-specifics"></a>B2C özellikleri

[B2C Ile ROPC kullanma hakkında daha fazla bilgi](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics#resource-owner-password-credentials-ropc-with-b2c).

### <a name="how-to-use-it"></a>Nasıl kullanılır?

`IPublicClientApplication`yöntemi içerir`AcquireTokenByUsernamePassword`

Aşağıdaki örnek, Basitleştirilmiş bir durum gösterir

```CSharp
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

Aşağıdaki örnek, alacağınız özel durumların türüyle ilgili açıklamalar ve bunların azaltmaları ile en güncel durumu gösterir

```CSharp
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

Uygulanabilecek `AcquireTokenByUsernamePassword`tüm değiştiriciler hakkında daha fazla bilgi için bkz. [acquiretokenbyusernamepasswordparameterbuilder](/dotnet/api/microsoft.identity.client.acquiretokenbyusernamepasswordparameterbuilder?view=azure-dotnet-preview#methods)

## <a name="command-line-tool-without-web-browser"></a>Komut satırı aracı (Web tarayıcısı olmadan)

### <a name="device-code-flow-why-and-how"></a>Cihaz kodu Flow neden? ve nasıl?

Bir komut satırı aracı (Web denetimlerine sahip olmayan) yazıyorsanız ve önceki akışları kullanmak istemiyorsanız, kullanmanız `AcquireTokenWithDeviceCode`gerekir.

Azure AD ile etkileşimli kimlik doğrulaması için bir Web tarayıcısı gerekir (Ayrıntılar için bkz. [Web tarayıcıları kullanımı](https://aka.ms/msal-net-uses-web-browser)). Ancak, bir Web tarayıcısı sağlamayan cihazlarda veya işletim sistemlerinde kullanıcıların kimliğini doğrulamak için, cihaz kodu akışı kullanıcının etkileşimli olarak oturum açmasını sağlamak için başka bir cihaz (örneğin, başka bir bilgisayar veya cep telefonu) kullanmasına izin verir. Uygulama, cihaz kod akışını kullanarak belirteçleri, özellikle bu cihazlar/OSE için tasarlanan iki adımlı bir işlemle alır. Bu uygulamaların örnekleri, IoT üzerinde çalışan uygulamalar veya komut satırı araçları (CLı). Fikir şu şekilde olur:

1. Kullanıcı kimlik doğrulaması gerekli olduğunda, uygulama bir kod sağlar ve kullanıcıdan bir URL 'ye (örneğin, `https://microsoft.com/devicelogin`internet 'e bağlı bir akıllı telefon) gitmesini ister ve kullanıcıdan kodu girmesi istenir. Bu işlem yapıldığında, Web sayfası, gerektiğinde izin istemleri ve çok faktörlü kimlik doğrulaması dahil olmak üzere normal bir kimlik doğrulama deneyimi aracılığıyla kullanıcıya yol açacaktır.

2. Kimlik doğrulaması başarılı olduğunda, komut satırı uygulaması gerekli belirteçleri bir arka kanal üzerinden alır ve bunu, gereken Web API çağrılarını gerçekleştirmek için kullanır.

### <a name="code"></a>Kod

`IPublicClientApplication`adlı bir yöntemi içerir`AcquireTokenWithDeviceCode`

```CSharp
 AcquireTokenWithDeviceCode(IEnumerable<string> scopes,
                            Func<DeviceCodeResult, Task> deviceCodeResultCallback)
```

Bu yöntem parametre olarak alır:

- İçin erişim belirteci istemek üzere `scopes`
- Şunu alacak bir geri arama`DeviceCodeResult`

  ![image](https://user-images.githubusercontent.com/13203188/56024968-7af1b980-5d11-11e9-84c2-5be2ef306dc5.png)

Aşağıdaki örnek kod, alacağınız özel durumların türüyle ilgili açıklamaları ve bunların hafifliklerini içeren en güncel durumu gösterir.

```CSharp
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

## <a name="file-based-token-cache"></a>Dosya tabanlı belirteç önbelleği

MSAL.NET ' de, bir bellek içi belirteç önbelleği varsayılan olarak sağlanır.

### <a name="serialization-is-customizable-in-windows-desktop-apps-and-web-appsweb-apis"></a>Serileştirme, Windows Masaüstü uygulamaları ve Web Apps/Web API 'Lerinde özelleştirilebilir

.NET Framework ve .NET Core durumunda, ek bir şey yapmazsanız, bellek içi belirteç önbelleği uygulama süresince sürer. Serileştirme 'in kutudan neden olmadığını anlamak için, MSAL .NET Desktop/Core uygulamalarının konsol veya Windows uygulamaları (dosya sistemine erişimi olan) veya **Ayrıca** Web uygulamaları veya Web API 'si olabileceğini unutmayın. Bu Web uygulamaları ve Web API 'Leri veritabanları, dağıtılmış önbellekler, redıs önbellekler gibi bazı belirli önbellek mekanizmalarını kullanabilir. .NET masaüstü veya çekirdekli bir kalıcı belirteç önbelleği uygulamasına sahip olmak için serileştirme 'yı özelleştirmeniz gerekir.

Belirteç önbelleği serileştirmesi ile ilgili sınıflar ve arabirimler aşağıdaki türlerdir:

- ``ITokenCache``, belirteç önbelleği serileştirme isteklerine abone olmak için olayları ve ayrıca çeşitli biçimlerde (ADAL v 3.0, MSAL 2. x ve MSAL 3. x = ADAL v 5.0) önbelleğe alma veya seri hale getirme yöntemlerini tanımlar
- ``TokenCacheCallback``, serileştirme işlemini işleyebilmeniz için olaylara geçirilmiş bir geri çağırma işlemi. Bunlar, türündeki ``TokenCacheNotificationArgs``bağımsız değişkenlerle çağırılır.
- ``TokenCacheNotificationArgs``yalnızca uygulamanın ve ``ClientId`` belirtecin kullanılabildiği kullanıcıya bir başvuru sağlar

  ![image](https://user-images.githubusercontent.com/13203188/56027172-d58d1480-5d15-11e9-8ada-c0292f1800b3.png)

> [!IMPORTANT]
> MSAL.net sizin için belirteç önbellekleri oluşturur ve bir `IToken` `UserTokenCache` uygulamanın ve `AppTokenCache` özelliklerini çağırdığınızda önbellek sağlar. Arabirimi kendiniz uygulamanız gerekmez. Özel bir belirteç önbelleği serileştirmesi uyguladığınızda, sorumluluğu şu şekilde olur:
>
> - Ve "olaylara" tepki verir (ya da *zaman uyumsuz* karşılarlar). `AfterAccess` `BeforeAccess` Bu temsilci önbelleğin serisini kaldırmak, `AfterAccess` ancak önbelleğin serileştirilmesinden sorumludur.`BeforeAccess`
> - Bu olay deposunun veya yükleme bloblarının bir parçası olarak olay bağımsız değişkeni aracılığıyla istediğiniz depolama alanına geçirilir.

Bir genel istemci uygulaması (Masaüstü) veya gizli bir istemci uygulaması (Web uygulaması/Web API 'SI, Daemon uygulaması) için bir belirteç önbelleği serileştirmesi yazıyorsanız, stratejileri farklılık açıktır.

MSAL v2. x için, önbelleği yalnızca MSAL.NET biçimine (MSAL ile ortak olan Birleşik biçim önbelleği, ancak platformlar arasında) seri hale getirmek istiyorsanız veya [eski](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization) belirteç önbelleğini desteklemek istiyorsanız, bu kadar birkaç seçeneğiniz vardır. ADAL v3 serileştirilmesi.

ADAL.NET 3. x, ADAL.NET 5. x ve MSAL.NET arasındaki SSO durumunu paylaşmak için belirteç önbelleği serileştirme özelleştirmesi, şu örneğin bir parçası olarak açıklanmaktadır: [Active-Directory-DotNet-v1--v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2)

### <a name="simple-token-cache-serialization-msal-only"></a>Basit belirteç önbelleği serileştirme (yalnızca MSAL)

Masaüstü uygulamaları için bir belirteç önbelleğinin özel serileştirilmesi Naïve uygulamasının bir örneği aşağıda verilmiştir. Burada, uygulama ile aynı klasördeki bir dosyadaki Kullanıcı belirteci önbelleği.

Uygulamayı oluşturduktan sonra, uygulamayı geçirmeyi çağırarak ``TokenCacheHelper.EnableSerialization()`` Serileştirmeyi etkinleştirirsiniz`UserTokenCache`

```CSharp
app = PublicClientApplicationBuilder.Create(ClientId)
    .Build();
TokenCacheHelper.EnableSerialization(app.UserTokenCache);
```

Bu yardımcı sınıf aşağıdaki kod parçacığı gibi görünür:

```CSharp
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

Ortak istemci uygulamaları için bir ürün kalitesi belirteç önbelleği dosya tabanlı serileştirici Önizlemesi (Windows, Mac ve Linux üzerinde çalışan masaüstü uygulamaları için) [Microsoft. Identity. Client. Extensions. msal](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Msal) açık kaynak kitaplığından bulunabilir. Bunu uygulamalarınıza aşağıdaki NuGet paketinden dahil edebilirsiniz: [Microsoft. Identity. Client. Extensions. msal](https://www.nuget.org/packages/Microsoft.Identity.Client.Extensions.Msal/).

> [!NOTE]
> Sorumluluk reddi. Microsoft. Identity. Client. Extensions. msal kitaplığı, MSAL.NET üzerinde bir uzantıdır. Bu kitaplıklardaki sınıflar, MSAL.NET veya son değişiklikler ile, gelecekte bu şekilde bir şekilde.

### <a name="dual-token-cache-serialization-msal-unified-cache--adal-v3"></a>Çift belirteç önbelleği serileştirme (MSAL Birleşik önbellek + ADAL v3)

Her ikisi de birleştirilmiş önbellek biçimiyle (ADAL.NET 4. x ve MSAL.NET 2. x için ortak olan ve aynı kuşya da daha eski olan diğer Msallara, aynı platformda) belirteç önbelleği serileştirme uygulamak istiyorsanız, aşağıdaki kod ile ilham alabilirsiniz :

```CSharp
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

Bu kez yardımcı sınıfı aşağıdaki kod gibi görünür:

```CSharp
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
> [Masaüstü uygulamasından bir Web API 'SI çağırma](scenario-desktop-call-api.md)
