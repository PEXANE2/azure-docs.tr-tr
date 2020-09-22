---
title: "Hızlı başlangıç: ASP.NET Core Web API 'sini Microsoft Identity platformu ile koruma | Mavisi"
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, yetkilendirme için Microsoft Identity platformunu kullanarak bir ASP.NET Core Web API 'sini nasıl koruyabileceğinizi gösteren bir kod örneği indirip değiştirirsiniz.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/22/2020
ms.author: jmprieur
ms.custom: devx-track-csharp, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: dc0cdca2355403bc8f5409d9a6ca7f4ae89caf25
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948063"
---
# <a name="quickstart-protect-an-aspnet-core-web-api-with-microsoft-identity-platform"></a>Hızlı başlangıç: ASP.NET Core Web API 'sini Microsoft Identity platformu ile koruma

Bu hızlı başlangıçta, bir ASP.NET Core Web API 'sini yalnızca yetkili hesaplar tarafından erişilebilmesi için nasıl koruyacağınızı öğrenmek üzere bir kod örneği kullanırsınız. Hesaplar kişisel hesap (hotmail.com, outlook.com ve diğerleri) ve herhangi bir Azure Active Directory (Azure AD) örneğinde iş ve okul hesapları olabilir.

> [!div renderon="docs"]
> ## <a name="prerequisites"></a>Önkoşullar
>
> - Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
> - [Azure Active Directory kiracı](quickstart-create-new-tenant.md)
> - [.NET Core SDK 3.1 +](https://dotnet.microsoft.com/)
> - [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) veya [Visual Studio Code](https://code.visualstudio.com/)
>
> ## <a name="step-1-register-the-application"></a>1. Adım: uygulamayı kaydetme
>
> İlk olarak, Web API 'sini Azure AD kiracınıza kaydedin ve aşağıdaki adımları izleyerek bir kapsam ekleyin:
>
> 1. [Azure Portal](https://portal.azure.com) oturum açın.
> 1. Birden fazla kiracıya erişiminiz varsa, uygulamayı kaydetmek istediğiniz kiracıyı seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: .
> 1. **Azure Active Directory**'yi bulun ve seçin.
> 1. **Yönet**altında **uygulama kayıtları**ve ardından **Yeni kayıt**' ı seçin.
> 1. Uygulamanız için bir **ad** girin (örneğin,) `AspNetCoreWebApi-Quickstart` . Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz.
> 1. **Kaydet**’i seçin.
> 1. **Yönet**altında **bir API 'yi kullanıma** sunma ' yı seçin.
> 1. **Kapsam Ekle** ' yi seçin ve **Kaydet '** i SEÇIN ve varsayılan **uygulama kimliği URI**'sini kabul edin.
> 1. **Kapsam Ekle** bölmesinde, aşağıdaki değerleri girin:
>    - **Kapsam adı**: `access_as_user`
>    - **Kimler izin verebilir?**: **Yöneticiler ve kullanıcılar**
>    - **Yönetici onayı görünen adı**: `Access AspNetCoreWebApi-Quickstart`
>    - **Yönetici onay açıklaması**: `Allows the app to access AspNetCoreWebApi-Quickstart as the signed-in user.`
>    - **Kullanıcı izni görünen adı**: `Access AspNetCoreWebApi-Quickstart`
>    - **Kullanıcı onay açıklaması**: `Allow the application to access AspNetCoreWebApi-Quickstart on your behalf.`
>    - **Durum**: **etkin**
> 1. Kapsam **eklemeyi tamamlayacak kapsam Ekle** ' yi seçin.

## <a name="step-2-download-the-aspnet-core-project"></a>2. Adım: ASP.NET Core projesini Indirin

> [!div renderon="docs"]
> [ASP.NET Core çözümünü](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/archive/aspnetcore3-1.zip) GitHub 'dan indirin.

> [!div renderon="docs"]
> ## <a name="step-3-configure-the-aspnet-core-project"></a>3. Adım: ASP.NET Core projesini yapılandırma
>
> Bu adımda, örnek kodu daha önce oluşturduğunuz uygulama kaydıyla çalışacak şekilde yapılandırın.
>
> 1. . Zip arşivini sürücünüzün köküne yakın bir klasöre ayıklayın. Örneğin, *C:\Azure-Samples*.
> 1. Çözümü kod düzenleyicinizdeki *WebApi* klasöründe açın.
> 1. appsettings.jsdosya * üzerinde* açın ve aşağıdakileri değiştirin:
>
>    ```json
>    "ClientId": "Enter_the_Application_Id_here",
>    "TenantId": "Enter_the_Tenant_Info_Here"
>    ```
>
>    - `Enter_the_Application_Id_here`Azure Portal kaydettiğiniz uygulamanın **uygulama (ISTEMCI) kimliğiyle** değiştirin. Uygulamanın **genel bakış** sayfasında **uygulama (istemci) kimliğini** bulabilirsiniz.
>    - `Enter_the_Tenant_Info_Here`Aşağıdakilerden biriyle değiştirin:
>       - Uygulamanız **yalnızca bu kuruluş dizinindeki hesapları**destekliyorsa, bu değeri **Dizin (kiracı) kimliği** (GUID) veya **kiracı adı** (örneğin,) ile değiştirin `contoso.onmicrosoft.com` . Uygulamanın **genel bakış** sayfasında **Dizin (kiracı) kimliğini** bulabilirsiniz.
>       - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>       - Uygulamanız **tüm Microsoft hesabı kullanıcıları**destekliyorsa, bu değeri şu şekilde bırakın `common`
>
> Bu hızlı başlangıçta, dosyadaki *appsettings.js* başka bir değeri değiştirmeyin.

## <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı

Web API 'SI bir istemci uygulamasından bir belirteç alır ve Web API 'sindeki kod belirteci doğrular. Bu senaryo [Senaryo: korumalı Web API 'si](scenario-protected-web-api-overview.md)ile daha ayrıntılı olarak açıklanmıştır.

### <a name="startup-class"></a>Başlangıç sınıfı

*Microsoft. AspNetCore. Authentication* ara yazılımı, `Startup` barındırma işlemi başlatıldığında yürütülen bir sınıfı kullanır. `ConfigureServices`Yönteminde, `AddMicrosoftIdentityWebApi` *Microsoft. Identity. Web* tarafından verilen uzantı yöntemi çağırılır.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
                .AddMicrosoftIdentityWebApi(Configuration, "AzureAd");
    }
```

`AddAuthentication()`Yöntemi, hizmeti Jwttaşıyıcı tabanlı kimlik doğrulaması ekleyecek şekilde yapılandırır.

İçeren çizgi, `.AddMicrosoftIdentityWebApi` Web API 'Nize Microsoft Identity platform yetkilendirmesi ekler. Daha sonra, `AzureAD` yapılandırma dosyası *appsettings.js* bölümündeki bilgileri temel alarak Microsoft Identity platform uç noktası tarafından verilen erişim belirteçlerini doğrulamak üzere yapılandırılmıştır:

| *appsettings.js* anahtarda | Açıklama                                                                                                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ClientId`             | Azure portal kayıtlı uygulamanın **uygulama (istemci) kimliği** .                                                                                       |
| `Instance`             | Kullanıcının kimlik doğrulaması için güvenlik belirteci hizmeti (STS) uç noktası. Bu değer genellikle `https://login.microsoftonline.com/` Azure genel bulutu 'nı gösterir. |
| `TenantId`             | İş veya okul hesaplarıyla veya Microsoft kişisel hesaplarıyla kullanıcıların oturum açması için kiracınızın adı veya kiracı KIMLIĞI (GUID) veya *ortak* .                             |

`Configure()`Yöntemi, `app.UseAuthentication()` adlandırılmış işlevlerini etkinleştiren iki önemli yöntem içerir `app.UseAuthorization()` :

```csharp
// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // more code
    app.UseAuthentication();
    app.UseAuthorization();
    // more code
}
```

### <a name="protect-a-controller-a-controllers-method-or-a-razor-page"></a>Denetleyiciyi, denetleyicinin yöntemini veya Razor sayfasını koruma

Özniteliğini kullanarak bir denetleyiciyi veya denetleyici yöntemlerini koruyabilirsiniz `[Authorize]` . Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılara izin vererek denetleyiciye veya yöntemlere erişimi kısıtlar. Bu, kullanıcının kimlik doğrulaması yapılmadığı takdirde denetleyiciye erişmek için kimlik doğrulama sınaması başlatılabileceği anlamına gelir.

```csharp
namespace webapi.Controllers
{
    [Authorize]
    [ApiController]
    [Route("[controller]")]
    public class WeatherForecastController : ControllerBase
```

### <a name="validate-the-scope-in-the-controller"></a>Denetleyicideki kapsamı doğrulama

Sonra, API 'deki kod, gerekli kapsamların, kullanarak belirteçte olduğunu doğrular `HttpContext.VerifyUserHasAnyAcceptedScope(scopeRequiredByApi);`

```csharp
namespace webapi.Controllers
{
    [Authorize]
    [ApiController]
    [Route("[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        // The Web API will only accept tokens 1) for users, and 2) having the "access_as_user" scope for this API
        static readonly string[] scopeRequiredByApi = new string[] { "access_as_user" };

        [HttpGet]
        public IEnumerable<WeatherForecast> Get()
        {
            HttpContext.VerifyUserHasAnyAcceptedScope(scopeRequiredByApi);

            // some code here
        }
    }
}
```

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu ASP.NET Core Web API kodu örneğini içeren GitHub deposu, şu şekilde nasıl yapılacağını gösteren yönergeler ve daha fazla kod örneği içerir:

- Yeni bir ASP.NET Core Web API 'sine kimlik doğrulaması ekleme
- Bir masaüstü uygulamasından Web API 'sini çağırma
- Microsoft Graph ve diğer Microsoft API 'Leri gibi aşağı akış API 'Lerini çağırma

> [!div class="nextstepaction"]
> [GitHub 'da Web API öğreticilerini ASP.NET Core](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2)
