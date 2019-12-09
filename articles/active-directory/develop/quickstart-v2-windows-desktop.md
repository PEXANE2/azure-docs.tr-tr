---
title: Microsoft Identity Platform Windows Masaüstü hızlı başlangıç | Microsoft Docs
description: Bir Windows Masaüstü .NET (XAML) uygulamasının bir erişim belirtecini nasıl alabileceğinizi ve bir Microsoft Identity platform uç noktası tarafından korunan bir API 'YI nasıl çağırabileceğinizi öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38ebf817d80fb1afdd3642f648d8e881b2e9d7de
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74920455"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-windows-desktop-app"></a>Hızlı Başlangıç: Bir belirteç alma ve bir Windows masaüstü uygulamasından Microsoft Graph API'si çağırma

Bu hızlı başlangıçta bir kişi, iş ve okul hesaplarında oturum açma, bir erişim belirteci alma ve Microsoft Graph API'sini çağırma işlemlerini gerçekleştiren bir Windows masaüstü .NET (WPF) uygulaması yazmayı öğreneceksiniz.

![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-windows-desktop/windesktop-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1\. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Yeni [Azure portal uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/WinDesktopQuickstartPage/sourceType/docs)gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2\. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://aka.ms/MobileAppReg) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
>      - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `Win-App-calling-MsGraph`.
>      - **Desteklenen hesap türleri** bölümünde **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları (ör. Skype, Xbox, Outlook.com)** seçeneğini belirtin.
>      - Uygulamayı kaydetmek için **Kaydet**'i seçin.
> 1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
> 1. **Masaüstü + cihazlar** bölümünü genişletin.  ( **Masaüstü + cihazlar** görünür değilse, ilk önce üst başlığa tıklayarak önizleme kimlik doğrulama deneyimini görüntüleyin)
> 1. **Yeniden yönlendirme URI 'si** bölümünde **URI Ekle**' yi seçin.  **Urn: ietf: WG: OAuth: 2.0: OOB**yazın.
> 1. **Kaydet**’i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1\. Adım: Uygulamanızı Azure portalında yapılandırma
> Bu hızlı başlangıç kod örneğinin çalışması için **urn:ietf:wg:oauth:2.0:oob** gibi bir yanıt URL’si eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-windows-desktop/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-visual-studio-project"></a>2\. Adım: Visual Studio projenizi indirme

[Visual Studio projesini indir](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/msal3x.zip) ([projeyi GitHub 'da görüntüle](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/))

#### <a name="step-3-configure-your-visual-studio-project"></a>3\. Adım: Visual Studio projenizi yapılandırma

1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) ayıklayın.
1. Projeyi Visual Studio'da açın.
1. **App.Xaml.cs** dosyasını düzenleyin ve `ClientId` ile `Tenant` alanlarının değerlerini aşağıdaki kodla değiştirin:

    ```csharp
    private static string ClientId = "Enter_the_Application_Id_here";
    private static string Tenant = "Enter_the_Tenant_Info_Here";
    ```
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Bu hızlı başlangıç Enter_the_Supported_Account_Info_Here destekler.

> [!div renderon="docs"]
> Nerede:
> - `Enter_the_Application_Id_here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
> - `Enter_the_Tenant_Info_Here` - aşağıdaki seçeneklerden birine ayarlanır:
>   - Uygulamanız **Bu kuruluş dizinindeki hesapları** destekliyorsa, bu değeri **Kiracı Kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com) ile değiştirin
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesaplarını** destekliyorsa bu değeri `common` ile değiştirin
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client)), Microsoft Identity platform tarafından korunan bir API 'ye erişmek için kullanılan kullanıcılara ve istek belirteçlerine oturum açmak için kullanılan kitaplıktır. MSAL kitaplığını Visual Studio'nun **Paket Yöneticisi Konsolu**'nda aşağıdaki komutu çalıştırarak yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```csharp
using Microsoft.Identity.Client;
```

Sonra da şu kodu kullanarak MSAL başlatın:

```csharp
public static IPublicClientApplication PublicClientApp;
PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AzureCloudInstance.AzurePublic, Tenant)
                .Build();
```

> |Nerede: ||
> |---------|---------|
> | `ClientId` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |

### <a name="requesting-tokens"></a>Belirteç isteme

MSAL, belirteç almak için iki yönteme sahiptir: `AcquireTokenInteractive` ve `AcquireTokenSilent`.

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Bazı durumlar, kullanıcıların kimlik bilgilerini doğrulamak veya onay vermek üzere bir açılan pencere aracılığıyla Microsoft Identity platform uç noktasıyla etkileşime geçmesini zorunlu kılıyor. Bazı örnekler:

- Kullanıcılar uygulamada ilk kez oturum açtığında
- Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmesi gerektiğinde
- Uygulamanız kullanıcının onaylaması gereken bir kaynağa erişim istediğinde
- İki faktörlü kimlik doğrulama gerektiğinde

```csharp
authResult = await App.PublicClientApp.AcquireTokenInteractive(_scopes)
                                      .ExecuteAsync();
```

> |Nerede:||
> |---------|---------|
> | `_scopes` | İstenen kapsamları (Microsoft Graph için `{ "user.read" }` veya özel Web API'leri için `{ "api://<Application ID>/access_as_user" }` gibi) barındırır. |

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Kullanıcının bir kaynağa erişmesi gerektiği her seferde kimlik bilgilerini doğrulamasının gerekmesini istemezsiniz. Çoğu kez, belirteç alma ve yenileme işlemlerinin kullanıcı etkileşimi olmadan gerçekleşmesini istersiniz. Korunan kaynaklara erişmek üzere belirteç almak için, ilk `AcquireTokenInteractive` yönteminden sonra `AcquireTokenSilent` yöntemini kullanabilirsiniz:

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
var firstAccount = accounts.FirstOrDefault();
authResult = await App.PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                      .ExecuteAsync();
```

> |Nerede: ||
> |---------|---------|
> | `scopes` | İstenen kapsamları (Microsoft Graph için `{ "user.read" }` veya özel Web API'leri için `{ "api://<Application ID>/access_as_user" }` gibi) barındırır. |
> | `firstAccount` | Önbellekteki ilk kullanıcıyı belirtir (MSAL destekleyen birden çok kullanıcı tek bir uygulama olarak). |

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıcın tam bir açıklamasının da içinde olduğu yeni özellikleri ve uygulamaları oluşturma hakkında eksiksiz adım adım kılavuz için Windows masaüstü öğreticisini deneyin.

> [!div class="nextstepaction"]
> [Graph API'si çağırma öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-windesktop)

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
