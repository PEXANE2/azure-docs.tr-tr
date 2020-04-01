---
title: Microsoft kimlik platformu Windows masaüstü quickstart | Microsoft Dokümanlar
description: Windows masaüstü .NET (XAML) uygulamasının nasıl erişim jetonu alabildiğini ve Microsoft kimlik platformu bitiş noktası tarafından korunan bir API'yi nasıl arayabildiğini öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 12/12/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: f7b6a682498bfce55107ba139600b6c9ab688542
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80473318"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-windows-desktop-app"></a>Hızlı Başlangıç: Bir belirteç alma ve bir Windows masaüstü uygulamasından Microsoft Graph API'si çağırma

Bu hızlı başlangıçta bir kişi, iş ve okul hesaplarında oturum açma, bir erişim belirteci alma ve Microsoft Graph API'sini çağırma işlemlerini gerçekleştiren bir Windows masaüstü .NET (WPF) uygulaması yazmayı öğreneceksiniz. (Bkz. örnek bir resim için [nasıl çalışır?)](#how-the-sample-works)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Yeni Azure [portalına](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/WinDesktopQuickstartPage/sourceType/docs)gidin - Uygulama kayıtları.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://aka.ms/MobileAppReg) sayfası.
> 1. **Yeni kayıt**seçin.
>      - **Ad** bölümüne, örneğin `Win-App-calling-MsGraph`uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
>      - **Desteklenen hesap türleri** bölümünde **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları (ör. Skype, Xbox, Outlook.com)** seçeneğini belirtin.
>      - Uygulamayı kaydetmek için **Kaydet**'i seçin.
> 1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
> 1. Yönlendirme **URI'lerinde** | **kamu istemcileri (mobil, masaüstü) bölümü için Önerilen Yönlendirme URI'leri,** kullanın. **https://login.microsoftonline.com/common/oauth2/nativeclient**
> 1. **Kaydet'i**seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
> Bu hızlı başlatmanın çalışması için kod örneği için yanıt **https://login.microsoftonline.com/common/oauth2/nativeclient**URL'si eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-windows-desktop/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-visual-studio-project"></a>2. Adım: Visual Studio projenizi indirme

> [!div renderon="docs"]
> [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/msal3x.zip)  

> [!div class="sxs-lookup" renderon="portal"]
> Visual Studio 2019'u kullanarak projeyi çalıştırın.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/msal3x.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Adım 3: Uygulamanız yapılandırıldı ve çalışmaya hazır
> Projenizi uygulamanızın özellikleriyle yapılandırdık ve çalışmaya hazır. 

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-visual-studio-project"></a>3. Adım: Visual Studio projenizi yapılandırma
> 1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) ayıklayın.
> 1. Projeyi Visual Studio'da açın.
> 1. **App.Xaml.cs** dosyasını düzenleyin ve `ClientId` ile `Tenant` alanlarının değerlerini aşağıdaki kodla değiştirin:
>
>    ```csharp
>    private static string ClientId = "Enter_the_Application_Id_here";
>    private static string Tenant = "Enter_the_Tenant_Info_Here";
>    ```
> 
> Konumlar:
> - `Enter_the_Application_Id_here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
> - `Enter_the_Tenant_Info_Here` - aşağıdaki seçeneklerden birine ayarlanır:
>   - Uygulamanız **Bu kuruluş dizinindeki hesapları** destekliyorsa, bu değeri **Kiracı Kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com) ile değiştirin
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesaplarını** destekliyorsa bu değeri `common` ile değiştirin
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örnek nasıl çalışır?
![Bu hızlı başlatma tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-windows-desktop/windesktop-intro.svg)

### <a name="msalnet"></a>MSAL.NET
MSAL ([Microsoft.Identity.Client),](https://www.nuget.org/packages/Microsoft.Identity.Client)Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan kullanıcı ve istek belirteçleri oturum açmaiçin kullanılan kitaplıktır. MSAL kitaplığını Visual Studio'nun **Paket Yöneticisi Konsolu**'nda aşağıdaki komutu çalıştırarak yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```csharp
using Microsoft.Identity.Client;
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```csharp
public static IPublicClientApplication PublicClientApp;
PublicClientApplicationBuilder.Create(ClientId)
                .WithRedirectUri("https://login.microsoftonline.com/common/oauth2/nativeclient")
                .WithAuthority(AzureCloudInstance.AzurePublic, Tenant)
                .Build();
```

> |Konumlar: ||
> |---------|---------|
> | `ClientId` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |

### <a name="requesting-tokens"></a>Belirteç isteme

MSAL, belirteç almak için iki yönteme sahiptir: `AcquireTokenInteractive` ve `AcquireTokenSilent`.

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Bazı durumlarda, kullanıcıların kimlik bilgilerini doğrulamak veya izin vermek için bir açılır pencere aracılığıyla Microsoft kimlik platformu bitiş noktasıyla etkileşime geçmeye zorlamaları gerekir. Bazı örnekler:

- Kullanıcılar uygulamada ilk kez oturum açtığında
- Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmesi gerektiğinde
- Uygulamanız kullanıcının onaylaması gereken bir kaynağa erişim istediğinde
- İki öğeli kimlik doğrulaması gerektiğinde

```csharp
authResult = await App.PublicClientApp.AcquireTokenInteractive(_scopes)
                                      .ExecuteAsync();
```

> |Konumlar:||
> |---------|---------|
> | `_scopes` | İstenen kapsamları (Microsoft Graph için `{ "user.read" }` veya özel Web API'leri için `{ "api://<Application ID>/access_as_user" }` gibi) barındırır. |

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Kullanıcının bir kaynağa her erişmesi gerektiğinde kimlik bilgilerini tekrar doğrulamak zorunda kalmasını istemezsiniz. Çoğu kez, belirteç alma ve yenileme işlemlerinin kullanıcı etkileşimi olmadan gerçekleşmesini istersiniz. Korunan kaynaklara erişmek üzere belirteç almak için, ilk `AcquireTokenInteractive` yönteminden sonra `AcquireTokenSilent` yöntemini kullanabilirsiniz:

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
var firstAccount = accounts.FirstOrDefault();
authResult = await App.PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                      .ExecuteAsync();
```

> |Konumlar: ||
> |---------|---------|
> | `scopes` | İstenen kapsamları (Microsoft Graph için `{ "user.read" }` veya özel Web API'leri için `{ "api://<Application ID>/access_as_user" }` gibi) barındırır. |
> | `firstAccount` | Önbellekteki ilk kullanıcıyı belirtir (MSAL destekleyen birden çok kullanıcı tek bir uygulama olarak). |

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıcın tam bir açıklamasının da içinde olduğu yeni özellikleri ve uygulamaları oluşturma hakkında eksiksiz adım adım kılavuz için Windows masaüstü öğreticisini deneyin.

> [!div class="nextstepaction"]
> [Graph API'si çağırma öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-windesktop)

Microsoft kimlik platformlarını geliştirmemize yardımcı olun. Kısa bir iki soruluk anketi tamamlayarak bize ne düşündüğünüzü söyleyin.

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
