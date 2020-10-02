---
title: 'Hızlı başlangıç: Kullanıcı oturumu açma ve Evrensel Windows Platformu uygulamada Microsoft Graph çağırma | Mavisi'
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, bir Evrensel Windows Platformu (UWP) uygulamasının bir erişim belirtecini nasıl alabileceğinizi ve Microsoft Identity platform tarafından korunan bir API 'YI nasıl çağırabileceğinizi öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 12/12/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:UWP
ms.openlocfilehash: 5b954c5eae9c203efa65dc9dc1883d8e00f3937a
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91630534"
---
# <a name="quickstart-call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Hızlı Başlangıç: Evrensel Windows Platformu (UWP) uygulamasından Microsoft Graph API'sini çağırma

Bu hızlı başlangıç, bir Evrensel Windows Platformu (UWP) uygulamasının kullanıcıları kişisel hesaplar veya iş ve okul hesaplarıyla nasıl oturum açıp Microsoft Graph API 'sini çağırabileceğinizi gösteren bir kod örneği içerir. (Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.)

> [!div renderon="docs"]
> ## <a name="prerequisites"></a>Önkoşullar
>
> * Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
> * [Visual Studio 2019](https://visualstudio.microsoft.com/vs/)
>
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> [!div renderon="docs" class="sxs-lookup"]
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Yeni [Azure Portal-uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/UwpQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'e tıklayın.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için şu adımları izleyin:
> 1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://aka.ms/MobileAppReg) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanızın kayıt bilgilerini girin:
>      - **Ad** bölümünde, örneğin, uygulamanın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin `UWP-App-calling-MsGraph` .
>      - **Desteklenen hesap türleri** bölümünde **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesapları (ör. Skype, Xbox, Outlook.com)** seçeneğini belirtin.
>      - Uygulamayı kaydetmek için **Kaydet**'i seçin.
> 1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
> 1. **Yeniden yönlendirme URI 'lerinde**  |  **ortak istemciler için önerilen yeniden yönlendirme URI 'leri (mobil, masaüstü)** bölümüne bakın **https://login.microsoftonline.com/common/oauth2/nativeclient** .
> 1. **Kaydet**'i seçin.

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>1. Adım: Uygulamanızı yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için yeniden yönlendirme URI 'SI eklemeniz gerekir **https://login.microsoftonline.com/common/oauth2/nativeclient** .
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-uwp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-visual-studio-project"></a>2. Adım: Visual Studio projenizi indirme

> [!div renderon="docs"]
> [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Visual Studio 2019 kullanarak projeyi çalıştırın.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
> Projenizi uygulamanızın özelliklerinin değerleriyle yapılandırdık ve çalıştırılmaya hazırlanıyor.

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-visual-studio-project"></a>3. Adım: Visual Studio projenizi yapılandırma
>
> 1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) ayıklayın.
> 1. Projeyi Visual Studio'da açın. UWP SDK 'Yı yüklemek isteyip istemediğiniz sorulabilir. Bu durumda kabul edin.
> 1. **MainPage.xaml.cs** öğesini düzenleyin ve alanın değerlerini değiştirin `ClientId` :
>
>    ```csharp
>    private const string ClientId = "Enter_the_Application_Id_here";
>    ```
> Burada:
> - `Enter_the_Application_Id_here` - Kaydettiğiniz uygulamanın Uygulama Kimliği değeridir.
>
> > [!TIP]
> > *Uygulama kimliği*değerini bulmak için, portaldaki **genel bakış** bölümüne gidin

#### <a name="step-4-run-your-application"></a>4. Adım: uygulamanızı çalıştırma

Windows makinenizde hızlı başlangıcı denemek istiyorsanız:

1. Visual Studio araç çubuğunda doğru platformu seçin (büyük olasılıkla **x64** veya **x86**, ARM değil). Hedef cihazın *cihazdan* *yerel makineye* değiştiğini gözlemleyeceksiniz
1. Hata ayıklamayı Seç | **Hata ayıklama olmadan Başlat**

## <a name="more-information"></a>Daha fazla bilgi

Bu bölümde hızlı başlangıç hakkında daha fazla bilgi verilmektedir.

### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı
![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-uwp/uwp-intro.svg)

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client)), kullanıcıları oturum açmak ve güvenlik belirteçleri istemek için kullanılan kitaplıktır. Güvenlik belirteçleri, geliştiriciler için Microsoft Identity platform tarafından korunan bir API 'ye erişmek için kullanılır. MSAL kitaplığını Visual Studio'nun *Paket Yöneticisi Konsolu*'nda aşağıdaki komutu çalıştırarak yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Identity.Client
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```csharp
using Microsoft.Identity.Client;
```

Daha sonra, MSAL aşağıdaki kod kullanılarak başlatılır:

```csharp
public static IPublicClientApplication PublicClientApp;
PublicClientApp = PublicClientApplicationBuilder.Create(ClientId)
                                                .WithRedirectUri("https://login.microsoftonline.com/common/oauth2/nativeclient")
                                                    .Build();
```

> |Burada: | Açıklama |
> |---------|---------|
> | `ClientId` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |

### <a name="requesting-tokens"></a>Belirteç isteme

MSAL, UWP uygulamasında belirteçleri almak için iki yönteme sahiptir: `AcquireTokenInteractive` ve `AcquireTokenSilent` .

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Bazı durumlar, kullanıcıların kimlik bilgilerini doğrulamak veya onay vermek üzere bir açılan pencere aracılığıyla Microsoft Identity platform uç noktasıyla etkileşime geçmesini zorunlu kılmalarına gerek duyar. Bazı örnekler:

- İlk kullanıcılar uygulamada oturum açtığında
- Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmesi gerektiğinde
- Uygulamanız bir kaynağa erişim isteğinde bulunduğunda, kullanıcının onay yapması gerekir
- İki öğeli kimlik doğrulaması gerektiğinde

```csharp
authResult = await App.PublicClientApp.AcquireTokenInteractive(scopes)
                      .ExecuteAsync();
```

> |Burada:| Açıklama |
> |---------|---------|
> | `scopes` | `{ "user.read" }`Microsoft Graph veya `{ "api://<Application ID>/access_as_user" }` özel Web API 'leri için istenen kapsamları içerir. |

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

`AcquireTokenSilent`Başlangıç yönteminden sonra korumalı kaynaklara erişmek için belirteçleri elde etmek için yöntemini kullanın `AcquireTokenInteractive` . Bir kaynağa erişmesi gereken her seferinde kullanıcının kimlik bilgilerini doğrulamasını gerektirmek istemezsiniz. Herhangi bir kullanıcı etkileşimi olmadan belirteç alma ve yenileme işlemleri istediğiniz zaman

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
var firstAccount = accounts.FirstOrDefault();
authResult = await App.PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                      .ExecuteAsync();
```

> |Burada: | Açıklama |
> |---------|---------|
> | `scopes` | `{ "user.read" }`Microsoft Graph veya `{ "api://<Application ID>/access_as_user" }` özel Web API 'leri için istenen kapsamları içerir |
> | `firstAccount` | Önbellekteki ilk kullanıcı hesabını belirtir (MSAL, tek bir uygulamadaki birden çok kullanıcıyı destekler) |

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıcın tam bir açıklamasının da içinde olduğu yeni özellikleri ve uygulamaları oluşturma hakkında eksiksiz adım adım kılavuz için Windows masaüstü öğreticisini deneyin.

> [!div class="nextstepaction"]
> [UWP - Graph API'si çağırma öğreticisi](tutorial-v2-windows-uwp.md)
