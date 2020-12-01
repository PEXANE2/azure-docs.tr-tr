---
title: Öğretici-kullanıcı olarak Microsoft Graph Web uygulaması erişim | Mavisi
description: Bu öğreticide, oturum açmış bir kullanıcı için Microsoft Graph verilere erişmeyi öğreneceksiniz.
services: microsoft-graph, app-service-web
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/30/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.custom: azureday1
ms.openlocfilehash: e07ec17a4e14f0099d82bd444f2ee8d37abe9908
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96435017"
---
# <a name="tutorial-access-microsoft-graph-from-a-secured-app-as-the-user"></a>Öğretici: Kullanıcı olarak güvenli bir uygulamadan Microsoft Graph erişim

Azure App Service çalıştıran bir Web uygulamasından Microsoft Graph erişme hakkında bilgi edinin.

:::image type="content" alt-text="Microsoft Graph erişimi gösteren diyagram." source="./media/scenario-secure-app-access-microsoft-graph/web-app-access-graph.svg" border="false":::

Web uygulamanızdan Microsoft Graph erişim eklemek ve oturum açmış kullanıcı olarak bazı eylemler gerçekleştirmek istiyorsunuz. Bu bölümde, Web uygulamasına temsilci izinleri verme ve oturum açan kullanıcının Azure Active Directory (Azure AD) üzerinden profil bilgileri alma açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Bir Web uygulamasına temsilci izinleri verin.
> * Bir Web uygulamasından oturum açmış kullanıcı için Microsoft Graph çağırın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [App Service kimlik doğrulama/yetkilendirme modülü etkinleştirilmiş](scenario-secure-app-authentication-app-service.md)Azure App Service üzerinde çalışan bir Web uygulaması.

## <a name="grant-front-end-access-to-call-microsoft-graph"></a>Çağrıya Microsoft Graph ön uç erişimi verme

Web uygulamanızda kimlik doğrulama ve yetkilendirme 'yi etkinleştirmiş olduğunuza göre, Web uygulaması Microsoft Identity platformu ile kaydedilir ve bir Azure AD uygulaması tarafından desteklenir. Bu adımda, Web uygulamasına kullanıcı için Microsoft Graph erişim izni verirsiniz. (Teknik olarak, Web uygulamasının Azure AD uygulamasına kullanıcı için Microsoft Graph Azure AD uygulamasına erişme izinleri verirsiniz.)

[Azure Portal](https://portal.azure.com) menüsünde **Azure Active Directory** ' i seçin veya herhangi bir sayfadan **Azure Active Directory** ' i arayıp seçin.

**Uygulama kayıtları**  >  **sahip olan uygulamalar**  >  **Bu dizindeki tüm uygulamaları görüntüle '** yi seçin. Web uygulaması adınızı seçin ve ardından **API izinleri**' ni seçin.

**Izin Ekle**' yi seçin ve ardından Microsoft API 'leri ve Microsoft Graph seçin.

**Temsilci izinleri**' ni seçin ve ardından listeden **Kullanıcı. oku** ' yi seçin. **Izin Ekle**' yi seçin.

## <a name="configure-app-service-to-return-a-usable-access-token"></a>App Service’i kullanılabilir bir erişim belirteci döndürecek şekilde yapılandırma

Web uygulaması artık oturum açmış kullanıcı olarak Microsoft Graph erişmek için gerekli izinlere sahiptir. Bu adımda, Microsoft Graph erişim için size kullanılabilir bir erişim belirteci sağlamak üzere App Service kimlik doğrulaması ve yetkilendirmeyi yapılandırırsınız. Bu adım için, aşağı akış hizmetinin (Microsoft Graph) istemci/uygulama KIMLIĞI gereklidir. Microsoft Graph için uygulama KIMLIĞI *00000003-0000-0000-C000-000000000000*.

> [!IMPORTANT]
> App Service kullanılabilir erişim belirteci döndürecek şekilde yapılandırmazsanız, ```CompactToken parsing failed with error code: 80049217``` kodunuzda Microsoft Graph API 'leri çağırdığınızda bir hata alırsınız.

[Azure Kaynak Gezgini](https://resources.azure.com/) gidin ve kaynak ağacını kullanarak Web uygulamanızı bulun. Kaynak URL 'SI ile aynı olmalıdır `https://resources.azure.com/subscriptions/subscription-id/resourceGroups/SecureWebApp/providers/Microsoft.Web/sites/SecureWebApp20200915115914` .

Azure Kaynak Gezgini artık kaynak ağacında seçili Web uygulamanız ile açılır. Sayfanın üst kısmında, Azure kaynaklarınızın düzenlenmesine olanak tanımak için **oku/yaz** ' ı seçin.

Sol tarayıcıda, **config**  >  **authsettings öğesine tıklayın** öğesine gidin.

**Authsettings öğesine tıklayın** görünümünde **Düzenle**' yi seçin. ```additionalLoginParams```Kopyaladığınız ISTEMCI kimliğini kullanarak AŞAĞıDAKI JSON dizesine ayarlayın.

```json
"additionalLoginParams": ["response_type=code id_token","resource=00000003-0000-0000-c000-000000000000"],
```

**Yerleştir**' i seçerek ayarlarınızı kaydedin. Bu ayarın etkili olması birkaç dakika sürebilir. Web uygulamanız artık uygun bir erişim belirteciyle Microsoft Graph erişecek şekilde yapılandırılmıştır. Bunu yapmazsanız, Microsoft Graph sıkıştırılmış belirtecin biçiminin yanlış olduğunu belirten bir hata döndürür.

## <a name="call-microsoft-graph-net"></a>Microsoft Graph çağrısı (.NET)

Web uygulamanız artık gerekli izinlere sahiptir ve ayrıca Microsoft Graph istemci KIMLIĞINI oturum açma parametrelerine ekler. Web uygulaması, [Microsoft. Identity. Web kitaplığı](https://github.com/AzureAD/microsoft-identity-web/)kullanarak Microsoft Graph kimlik doğrulaması için bir erişim belirteci alır. Sürüm 1.2.0 ve sonraki sürümlerde, Microsoft. Identity. Web kitaplığı ile tümleşir ve App Service kimlik doğrulaması/yetkilendirme modülü ile birlikte çalışabilir. Microsoft. Identity. Web, Web uygulamasının App Service ' de barındırıldığını algılar ve App Service kimlik doğrulama/yetkilendirme modülünden erişim belirtecini alır. Erişim belirteci daha sonra Microsoft Graph API 'SI ile kimliği doğrulanmış isteklere iletilir.

Bu kodu örnek bir uygulamanın parçası olarak görmek için [GitHub 'daki örneğe](https://github.com/Azure-Samples/ms-identity-easyauth-dotnet-storage-graphapi/tree/main/2-WebApp-graphapi-on-behalf)bakın.

> [!NOTE]
> Microsoft. Identity. Web kitaplığı, temel kimlik doğrulaması/yetkilendirme için Web uygulamanızda gerekli değildir veya Microsoft Graph ile isteklerin kimliğini doğrular. Yalnızca App Service kimlik doğrulaması/yetkilendirme modülü etkin olan [aşağı akış API 'lerini güvenli](tutorial-auth-aad.md#call-api-securely-from-server-code) bir şekilde çağırmak mümkündür.
> 
> Ancak App Service kimlik doğrulaması/yetkilendirme, daha temel kimlik doğrulama senaryoları için tasarlanmıştır. Daha karmaşık senaryolar (örneğin, özel talepler işleme) için Microsoft. Identity. Web kitaplığı veya [Microsoft kimlik doğrulama kitaplığı](../active-directory/develop/msal-overview.md)gerekir. Başlangıçta daha fazla kurulum ve yapılandırma işi vardır ancak Microsoft. Identity. Web kitaplığı App Service kimlik doğrulaması/yetkilendirme modülü ile birlikte çalışabilir. Daha sonra, Web uygulamanızın daha karmaşık senaryolar işlemesi gerektiğinde App Service kimlik doğrulaması/yetkilendirme modülünü devre dışı bırakabilir ve Microsoft. Identity. Web uygulamanızın bir parçası olacak.

### <a name="install-client-library-packages"></a>İstemci kitaplığı paketlerini yükler

.NET Core komut satırı arabirimini veya Visual Studio 'da Paket Yöneticisi konsolunu kullanarak [Microsoft. Identity. Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) ve [Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph) NuGet paketlerini projenize yükler.

# <a name="command-line"></a>[Komut satırı](#tab/command-line)

Bir komut satırı açın ve proje dosyanızı içeren dizine geçiş yapın.

Install komutlarını çalıştırın.

```dotnetcli
dotnet add package Microsoft.Graph

dotnet add package Microsoft.Identity.Web
```

# <a name="package-manager"></a>[Paket Yöneticisi](#tab/package-manager)

Visual Studio 'da projeyi/çözümü açın ve **Araçlar**  >  **NuGet Paket Yöneticisi**  >  **Paket Yöneticisi konsolu** komutunu kullanarak konsolunu açın.

Install komutlarını çalıştırın.
```powershell
Install-Package Microsoft.Graph

Install-Package Microsoft.Identity.Web
```

---

### <a name="startupcs"></a>Startup.cs

*Startup.cs* dosyasında, ```AddMicrosoftIdentityWebApp``` yöntemi Web uygulamanıza Microsoft. Identity. Web ekliyor. ```AddMicrosoftGraph```Yöntemi Microsoft Graph desteği ekler.

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Identity.Web;
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

// Some code omitted for brevity.
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
                .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"))
                    .EnableTokenAcquisitionToCallDownstreamApi()
                        .AddMicrosoftGraph(Configuration.GetSection("Graph"))
                        .AddInMemoryTokenCaches();

        services.AddRazorPages();
    }
}

```

### <a name="appsettingsjson"></a>appsettings.json

*Azuread* , Microsoft. Identity. Web kitaplığının yapılandırmasını belirtir. [Azure Portal](https://portal.azure.com)portal menüsünden **Azure Active Directory** ' i seçin ve ardından **uygulama kayıtları**' yı seçin. App Service kimlik doğrulaması/yetkilendirme modülünü etkinleştirdiğinizde oluşturulan uygulama kaydını seçin. (Uygulama kaydı, Web uygulamanızla aynı ada sahip olmalıdır.) Kiracı KIMLIĞINI ve istemci KIMLIĞINI uygulama kaydına Genel Bakış sayfasında bulabilirsiniz. Etki alanı adı, kiracınız için Azure AD Genel Bakış sayfasında bulunabilir.

*Graph* Microsoft Graph uç noktasını ve uygulama için gereken ilk kapsamları belirtir.

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "fourthcoffeetest.onmicrosoft.com",
    "TenantId": "[tenant-id]",
    "ClientId": "[client-id]",
    // To call an API
    "ClientSecret": "[secret-from-portal]", // Not required by this scenario
    "CallbackPath": "/signin-oidc"
  },

  "Graph": {
    "BaseUrl": "https://graph.microsoft.com/v1.0",
    "Scopes": "user.read"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="indexcshtmlcs"></a>Index.cshtml.cs

Aşağıdaki örnek, Microsoft Graph oturum açan kullanıcı olarak nasıl çağrılacağını ve bazı Kullanıcı bilgilerinin nasıl alınacağını gösterir. ```GraphServiceClient```Nesne denetleyiciye eklenir ve Microsoft. Identity. Web kitaplığı tarafından sizin için kimlik doğrulaması yapılandırıldı.

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Microsoft.Graph;
using System.IO;
using Microsoft.Identity.Web;
using Microsoft.Extensions.Logging;

// Some code omitted for brevity.

[AuthorizeForScopes(Scopes = new[] { "user.read" })]
public class IndexModel : PageModel
{
    private readonly ILogger<IndexModel> _logger;
    private readonly GraphServiceClient _graphServiceClient;

    public IndexModel(ILogger<IndexModel> logger, GraphServiceClient graphServiceClient)
    {
        _logger = logger;
        _graphServiceClient = graphServiceClient;
    }

    public async Task OnGetAsync()
    {
        try
        {
            var user = await _graphServiceClient.Me.Request().GetAsync();
            ViewData["Me"] = user;
            ViewData["name"] = user.DisplayName;

            using (var photoStream = await _graphServiceClient.Me.Photo.Content.Request().GetAsync())
            {
                byte[] photoByte = ((MemoryStream)photoStream).ToArray();
                ViewData["photo"] = Convert.ToBase64String(photoByte);
            }
        }
        catch (Exception ex)
        {
            ViewData["photo"] = null;
        }
    }
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyi bitirdiyseniz ve artık Web uygulamasına veya ilişkili kaynaklara ihtiyacınız yoksa, [oluşturduğunuz kaynakları temizleyin](scenario-secure-app-clean-up-resources.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
>
> * Bir Web uygulamasına temsilci izinleri verin.
> * Bir Web uygulamasından oturum açmış kullanıcı için Microsoft Graph çağırın.

> [!div class="nextstepaction"]
> [App Service Microsoft Graph uygulama olarak erişiyor](scenario-secure-app-access-microsoft-graph-as-app.md)