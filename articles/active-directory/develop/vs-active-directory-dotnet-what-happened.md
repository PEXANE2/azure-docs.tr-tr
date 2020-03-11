---
title: Azure AD 'ye bağlandığınızda MVC projesinde yapılan değişiklikler
description: Visual Studio bağlı hizmetler 'i kullanarak Azure AD 'ye bağlandığınızda MVC projenizde ne olacağını açıklar.
author: ghogen
manager: jillfra
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: ac38adba4ca0d978dd48a546bed1b1faf4fe40fe
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79036968"
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>MVC projem 'e ne oldu (Visual Studio Azure Active Directory bağlı hizmeti)?

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-dotnet-getting-started.md)
> - [Ne oldu](vs-active-directory-dotnet-what-happened.md)

Bu makalede, [Azure Active Directory bağlı hizmeti Visual Studio kullanılarak](vs-active-directory-add-connected-service.md)eklenirken BIR ASP.NET MVC projesinde yapılan tam değişiklikler tanımlanmaktadır.

Bağlı hizmetle çalışma hakkında daha fazla bilgi için [bkz. Başlarken](vs-active-directory-dotnet-getting-started.md).

## <a name="added-references"></a>Eklenen başvurular

Proje dosyasını (*. NET başvuruları) ve `packages.config` (NuGet başvuruları) etkiler.

| Tür | Başvuru |
| --- | --- |
| NET NuGet | Microsoft.IdentityModel.Protocol.Extensions |
| NET NuGet | Microsoft.Owin |
| NET NuGet | Microsoft.Owin.Host.SystemWeb |
| NET NuGet | Microsoft.Owin.Security |
| NET NuGet | Microsoft. Owin. Security. Cookies |
| NET NuGet | Microsoft.Owin.Security.OpenIdConnect |
| NET NuGet | Owın |
| .NET        | System.IdentityModel |
| NET NuGet | System.IdentityModel.Tokens.Jwt |
| .NET        | System.Runtime.Serialization |

**Dizin verilerini oku** seçeneğini belirlediyseniz ek başvurular:

| Tür | Başvuru |
| --- | --- |
| NET NuGet | EntityFramework |
| .NET        | EntityFramework. SqlServer (yalnızca Visual Studio 2015) |
| NET NuGet | Microsoft. Azure. ActiveDirectory. GraphClient |
| NET NuGet | Microsoft.Data.Edm |
| NET NuGet | Microsoft.Data.OData |
| NET NuGet | Microsoft.Data.Services.Client |
| NET NuGet | Microsoft. IdentityModel. clients. ActiveDirectory |
| .NET        | Microsoft. IdentityModel. clients. ActiveDirectory. WindowsForms (yalnızca Visual Studio 2015) |
| NET NuGet | System. uzamsal |

Aşağıdaki başvurular kaldırılır (Visual Studio 2015 ' de olduğu gibi, yalnızca ASP.NET 4 projeleri):

| Tür | Başvuru |
| --- | --- |
| NET NuGet | Microsoft.AspNet.Identity.Core |
| NET NuGet | Microsoft.AspNet.Identity.EntityFramework |
| NET NuGet | Microsoft.AspNet.Identity.Owin |

## <a name="project-file-changes"></a>Proje dosyası değişiklikleri

- Özellik `IISExpressSSLPort` ayrı bir sayı olarak ayarlayın.
- `WebProject_DirectoryAccessLevelKey`, **Dizin verilerini oku** seçeneğini belirlediyseniz özelliği 0 veya 1 olarak ayarlayın.
- Özellik `IISUrl`, `<port>` `IISExpressSSLPort` değerle eşleşen `https://localhost:<port>/` olarak ayarlayın.

## <a name="webconfig-or-appconfig-changes"></a>Web. config veya App. config değişiklikleri

- Aşağıdaki yapılandırma girdileri eklendi:

    ```xml
    <appSettings>
        <add key="ida:ClientId" value="<ClientId from the new Azure AD app>" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="<your selected Azure domain>" />
        <add key="ida:TenantId" value="<the Id of your selected Azure AD tenant>" />
        <add key="ida:PostLogoutRedirectUri" value="<project start page, such as https://localhost:44335>" />
    </appSettings>
    ```

- `System.IdentityModel.Tokens.Jwt` ve `Microsoft.IdentityModel.Protocol.Extensions`için `<runtime><assemblyBinding>` düğümü altına `<dependentAssembly>` öğeleri eklendi.

**Dizin verilerini oku** seçeneğini belirlediyseniz ek değişiklikler:

- `<appSettings>`altına aşağıdaki yapılandırma girişi eklendi:

    ```xml
    <add key="ida:ClientSecret" value="<Azure AD app's new client secret>" />
    ```

- Aşağıdaki öğeler `<configuration>`altına eklendi; Project-MDF-File ve Project-Catalog-id değerleri farklılık gösterir:

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

- `Microsoft.Data.Services.Client`, `Microsoft.Data.Edm`ve `Microsoft.Data.OData`için `<runtime><assemblyBinding>` düğümü altına `<dependentAssembly>` öğeleri eklendi.

## <a name="code-changes-and-additions"></a>Kod değişiklikleri ve eklemeler

- `Controllers/HomeController.cs` ve var olan diğer denetleyicilere `[Authorize]` özniteliği eklendi.

- Azure AD kimlik doğrulaması için başlangıç mantığını içeren `App_Start/Startup.Auth.cs`bir kimlik doğrulama başlangıç sınıfı eklendi. **Dizin verilerini oku** seçeneğini belirlediyseniz, bu dosya Ayrıca bir OAuth kodu alacak kodu içerir ve bir erişim belirteci için onu değiş tokuş sağlar.

- `SignIn` ve `SignOut` yöntemleri içeren `Controllers/AccountController.cs`bir denetleyici sınıfı eklendi.

- `SignIn` ve `SignOut`için bir eylem bağlantısı içeren kısmi bir görünüm eklendi `Views/Shared/_LoginPartial.cshtml`.

- Oturum kapatma Kullanıcı arabirimi için HTML içeren kısmi bir görünüm (`Views/Account/SignoutCallback.cshtml`) eklendi.

- `Startup.Configuration` yöntemi, sınıf zaten varsa `ConfigureAuth(app)` çağrısı içerecek şekilde güncelleştirildi; Aksi takdirde, yöntemini çağıran bir `Startup` sınıfı eklenmiştir.

- Bağlı hizmetin eklenmesini izlemek için Visual Studio 'nun kullandığı bilgileri içeren `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) veya `Service References/Azure AD/ConnectedService.json` (Visual Studio 2015) eklendi.

- **Dizin verilerini oku** seçeneğini belirlediyseniz, belirteç önbelleğe alma desteği için `Models/ADALTokenCache.cs` ve `Models/ApplicationDbContext.cs` eklendi. Ayrıca, Azure Graph API 'Leri kullanarak Kullanıcı profili bilgilerine erişimi göstermek için ek bir denetleyici ve görünüm de eklendi: `Controllers/UserProfileController.cs`, `Views/UserProfile/Index.cshtml`ve `Views/UserProfile/Relogin.cshtml`

### <a name="file-backup-visual-studio-2015"></a>Dosya yedekleme (Visual Studio 2015)

Bağlı hizmet eklenirken, Visual Studio 2015, değiştirilen ve kaldırılan dosyaları yedekler. Etkilenen tüm dosyalar `Backup/AzureAD`klasöre kaydedilir. Visual Studio 2017 ve üzeri yedeklemeler oluşturmaz.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ManageViewModels.cs`
- `Views\Shared\_LoginPartial.cshtml`

## <a name="changes-on-azure"></a>Azure 'daki değişiklikler

- Bağlı hizmeti eklerken seçtiğiniz etki alanında bir Azure AD uygulaması oluşturuldu.
- Uygulama, bu seçenek seçildiyse, **Dizin verilerini oku** iznini içerecek şekilde güncelleştirildi.

[Azure Active Directory hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [Microsoft 'a bir ASP.NET Web uygulamasına oturum açma ekleme](quickstart-v2-aspnet-webapp.md)
