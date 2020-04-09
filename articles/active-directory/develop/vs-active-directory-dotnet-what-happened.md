---
title: Azure AD'ye bağlandığınızda Bir MVC projesinde yapılan değişiklikler
description: Visual Studio'ya bağlı hizmetleri kullanarak Azure AD'ye bağlandığınızda MVC projenize neler olduğunu açıklar
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: d42d905bf35c015213e76bc50c4bc339a5c4a062
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886118"
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>MVC projeme (Visual Studio Azure Active Directory bağlı hizmet) ne oldu?

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-dotnet-getting-started.md)
> - [Ne oldu](vs-active-directory-dotnet-what-happened.md)

Bu makalede, [Visual Studio kullanarak Azure Active Directory'ye bağlı hizmet](vs-active-directory-add-connected-service.md)eklerken ASP.NET MVC projesinde yapılan değişikliklerin tam olarak tanımlanmaktadır.

Bağlı hizmetle çalışma hakkında daha fazla bilgi için [başlarken](vs-active-directory-dotnet-getting-started.md)bkz.

## <a name="added-references"></a>Eklenen başvurular

Proje dosyasını (*.NET başvuruları) ve `packages.config` (NuGet başvurularını) etkiler.

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.IdentityModel.Protocol.Extensions |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.Cookies |
| .NET; NuGet | Microsoft.Owin.Security.OpenIdConnect |
| .NET; NuGet | Owin |
| .NET        | System.IdentityModel |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |
| .NET        | System.Runtime.Serialization |

**Okuma dizini veri** seçeneğini seçtiyseniz ek başvurular:

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | EntityFramework |
| .NET        | EntityFramework.SqlServer (yalnızca Visual Studio 2015) |
| .NET; NuGet | Microsoft.Azure.ActiveDirectory.GraphClient |
| .NET; NuGet | Microsoft.Data.Edm |
| .NET; NuGet | Microsoft.Data.OData |
| .NET; NuGet | Microsoft.Data.Services.Client |
| .NET; NuGet | Microsoft.IdentityModel.Clients.ActiveDirectory |
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms (Yalnızca Visual Studio 2015) |
| .NET; NuGet | System.Spatial |

Aşağıdaki başvurular kaldırılır (Visual Studio 2015'te olduğu gibi yalnızca 4 proje ASP.NET):

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.AspNet.Identity.Core |
| .NET; NuGet | Microsoft.AspNet.Identity.EntityFramework |
| .NET; NuGet | Microsoft.AspNet.Identity.Owin |

## <a name="project-file-changes"></a>Proje dosyası değişiklikleri

- Özelliği `IISExpressSSLPort` farklı bir sayıya ayarlayın.
- **Okuma dizini veri** seçeneğini seçildiyseniz özelliği `WebProject_DirectoryAccessLevelKey` 0 veya 1 olarak ayarlayın.
- Özelliği `IISUrl` `https://localhost:<port>/` `IISExpressSSLPort` değerle `<port>` eşleşen yere ayarlayın.

## <a name="webconfig-or-appconfig-changes"></a>web.config veya app.config değişiklikleri

- Aşağıdaki yapılandırma girişleri eklendi:

    ```xml
    <appSettings>
        <add key="ida:ClientId" value="<ClientId from the new Azure AD app>" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="<your selected Azure domain>" />
        <add key="ida:TenantId" value="<the Id of your selected Azure AD tenant>" />
        <add key="ida:PostLogoutRedirectUri" value="<project start page, such as https://localhost:44335>" />
    </appSettings>
    ```

- Düğüm `<dependentAssembly>` ün `<runtime><assemblyBinding>` altına eklenen `System.IdentityModel.Tokens.Jwt` `Microsoft.IdentityModel.Protocol.Extensions`öğeler için ve .

**Okuma dizini veri** seçeneğini seçtiyseniz ek değişiklikler:

- Aşağıdaki yapılandırma girişi `<appSettings>`altında eklendi:

    ```xml
    <add key="ida:ClientSecret" value="<Azure AD app's new client secret>" />
    ```

- Aşağıdaki öğeleri altında `<configuration>`eklendi; proje-mdf-dosya ve proje-katalog-id değerleri değişir:

    ```xml
    <configSections>
      <!-- For more information on Entity Framework configuration, visit https://go.microsoft.com/fwlink/?LinkID=237468 -->
      <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>

    <connectionStrings>
      <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\<project-mdf-file>.mdf;Initial Catalog=<project-catalog-id>;Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>

    <entityFramework>
      <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
        <parameters>
          <parameter value="mssqllocaldb" />
        </parameters>
      </defaultConnectionFactory>
      <providers>
        <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
      </providers>
    </entityFramework>
    ```

- `<dependentAssembly>` `<runtime><assemblyBinding>` Düğüm altında `Microsoft.Data.Services.Client`, , `Microsoft.Data.Edm`ve `Microsoft.Data.OData`.

## <a name="code-changes-and-additions"></a>Kod değişiklikleri ve eklemeler

- `[Authorize]` Öznitelik `Controllers/HomeController.cs` ve diğer varolan denetleyicileri eklendi.

- Azure AD kimlik doğrulaması için başlangıç mantığını içeren bir kimlik doğrulama başlangıç sınıfı `App_Start/Startup.Auth.cs`eklendi. **Oku dizin veri** sebunu seçtiyseniz, bu dosya bir OAuth kodu almak ve bir erişim belirteci ile değiştirmek için kod da içerir.

- Bir denetleyici sınıfı `Controllers/AccountController.cs`eklendi, içeren `SignIn` ve `SignOut` yöntemleri.

- Kısmi görünüm eklendi, `Views/Shared/_LoginPartial.cshtml`için bir `SignIn` eylem `SignOut`bağlantısı içeren ve .

- Oturum dışı arabirimi için HTML içeren kısmi bir görünüm `Views/Account/SignoutCallback.cshtml`eklendi.

- Sınıfın `Startup.Configuration` zaten var `ConfigureAuth(app)` olup olmadığı yla ilgili bir çağrı içerecek şekilde yöntemi güncelleştirin; aksi takdirde `Startup` çağrı yöntemini içeren bir sınıf eklendi.

- Visual `Connected Services/AzureAD/ConnectedService.json` Studio'nun bağlı hizmetin `Service References/Azure AD/ConnectedService.json` eklenmesini izlemek için kullandığı bilgileri içeren (Visual Studio 2017) veya (Visual Studio 2015) eklendi.

- **Read dizin verisi** veri `Models/ADALTokenCache.cs` seçeneğini `Models/ApplicationDbContext.cs` seçtiyseniz, belirteç önbelleğe alma ve destek lemek için eklendi. Ayrıca Azure grafik API'lerini kullanarak kullanıcı profili bilgilerine `Controllers/UserProfileController.cs`erişen leri göstermek için ek bir denetleyici ve görünüm eklendi: , `Views/UserProfile/Index.cshtml`, ve`Views/UserProfile/Relogin.cshtml`

### <a name="file-backup-visual-studio-2015"></a>Dosya yedekleme (Visual Studio 2015)

Bağlı hizmeti eklerken, Visual Studio 2015 değiştirilen ve kaldırılan dosyaları yedekler. Etkilenen tüm dosyalar klasöre `Backup/AzureAD`kaydedilir. Visual Studio 2017 ve daha sonra yedekleme oluşturmaz.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ManageViewModels.cs`
- `Views\Shared\_LoginPartial.cshtml`

## <a name="changes-on-azure"></a>Azure'daki değişiklikler

- Bağlı hizmeti eklerken seçtiğiniz etki alanında bir Azure AD Uygulaması oluşturdu.
- Bu seçenek **seçilirse,** uygulamayı Oku dizini veri iznini içerecek şekilde güncelleştirildi.

[Azure Etkin Dizini hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/services/active-directory/)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizin için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [ASP.NET bir web uygulamasına Microsoft ile oturum açma ekleme](quickstart-v2-aspnet-webapp.md)
