---
title: Azure AD 'ye bağlandığınızda MVC projesinde yapılan değişiklikler
description: Visual Studio bağlı hizmetler 'i kullanarak Azure AD 'ye bağlandığınızda MVC projenizde ne olacağını açıklar.
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: how-to
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: devx-track-csharp, aaddev, vs-azure
ms.openlocfilehash: ab72512b2e5d915dbd4b8a3d7d8446a5932eef9e
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88165456"
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
| NET NuGet | Microsoft. IdentityModel. Protocol. Extensions |
| NET NuGet | Microsoft. Owin |
| NET NuGet | Microsoft.Owin.Host.SystemWeb |
| NET NuGet | Microsoft. Owin. Security |
| NET NuGet | Microsoft.Owin.Security.Cookies |
| NET NuGet | Microsoft.Owin.Security.OpenIdConnect |
| NET NuGet | Owın |
| .NET        | System. IdentityModel |
| NET NuGet | System. IdentityModel. Tokens. JWT |
| .NET        | System.Runtime.Serialization |

**Dizin verilerini oku** seçeneğini belirlediyseniz ek başvurular:

| Tür | Başvuru |
| --- | --- |
| NET NuGet | EntityFramework |
| .NET        | EntityFramework. SqlServer (yalnızca Visual Studio 2015) |
| NET NuGet | Microsoft. Azure. ActiveDirectory. GraphClient |
| NET NuGet | Microsoft. Data. Edm |
| NET NuGet | Microsoft. Data. OData |
| NET NuGet | Microsoft. Data. Services. Client |
| NET NuGet | Microsoft.IdentityModel.Clients.ActiveDirectory |
| .NET        | Microsoft. IdentityModel. clients. ActiveDirectory. WindowsForms (yalnızca Visual Studio 2015) |
| NET NuGet | System. uzamsal |

Aşağıdaki başvurular kaldırılır (Visual Studio 2015 ' de olduğu gibi, yalnızca ASP.NET 4 projeleri):

| Tür | Başvuru |
| --- | --- |
| NET NuGet | Microsoft. AspNet. Identity. Core |
| NET NuGet | Microsoft. AspNet. Identity. EntityFramework |
| NET NuGet | Microsoft. AspNet. Identity. Owin |

## <a name="project-file-changes"></a>Proje dosyası değişiklikleri

- Özelliği `IISExpressSSLPort` ayrı bir sayı olarak ayarlayın.
- Özelliği 0 olarak ayarlayın `WebProject_DirectoryAccessLevelKey` veya **Dizin verilerini oku** seçeneğini belirlediyseniz 1 yapın.
- Özelliğini `IISUrl` `https://localhost:<port>/` değeri eşleşen olarak ayarlayın `<port>` `IISExpressSSLPort` .

## <a name="webconfig-or-appconfig-changes"></a>web.config veya app.config değişiklikleri

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

- `<dependentAssembly>` `<runtime><assemblyBinding>` Ve için düğümünün altına öğe eklendi `System.IdentityModel.Tokens.Jwt` `Microsoft.IdentityModel.Protocol.Extensions` .

**Dizin verilerini oku** seçeneğini belirlediyseniz ek değişiklikler:

- Aşağıdaki yapılandırma girişi altına eklendi `<appSettings>` :

    ```xml
    <add key="ida:ClientSecret" value="<Azure AD app's new client secret>" />
    ```

- Aşağıdaki öğeler altına eklendi `<configuration>` ; Proje-MDF-File ve proje-Catalog-id değerleri değişir:

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

- `<dependentAssembly>` `<runtime><assemblyBinding>` , Ve için düğümü altına eklenen `Microsoft.Data.Services.Client` öğeler `Microsoft.Data.Edm` `Microsoft.Data.OData` .

## <a name="code-changes-and-additions"></a>Kod değişiklikleri ve eklemeler

- `[Authorize]`Özniteliği `Controllers/HomeController.cs` ve diğer mevcut denetleyicilere eklendi.

- `App_Start/Startup.Auth.cs`Azure AD kimlik doğrulaması için başlangıç mantığını içeren bir kimlik doğrulama başlangıç sınıfı eklendi. **Dizin verilerini oku** seçeneğini belirlediyseniz, bu dosya Ayrıca bir OAuth kodu alacak kodu içerir ve bir erişim belirteci için onu değiş tokuş sağlar.

- `Controllers/AccountController.cs`, Ve yöntemleri içeren bir denetleyici sınıfı `SignIn` eklendi `SignOut` .

- `Views/Shared/_LoginPartial.cshtml`Ve için bir eylem bağlantısı içeren kısmi bir görünüm eklendi `SignIn` `SignOut` .

- `Views/Account/SignoutCallback.cshtml`Oturum kapatma Kullanıcı arabirimi IÇIN HTML içeren kısmi bir görünüm eklendi.

- `Startup.Configuration`Sınıfı zaten mevcutsa öğesine bir çağrı eklemek için yöntemi güncelleştirildi `ConfigureAuth(app)` ; Aksi halde `Startup` yöntemi çağıran bir sınıf eklediniz.

- `Connected Services/AzureAD/ConnectedService.json` `Service References/Azure AD/ConnectedService.json` Bağlı hizmetin eklenmesini Izlemek Için Visual Studio tarafından kullanılan bilgileri içeren (visual Studio 2017) veya (visual Studio 2015) eklendi.

- **Dizin verilerini oku** seçeneğini seçtiyseniz, `Models/ADALTokenCache.cs` `Models/ApplicationDbContext.cs` belirteç önbelleğe almayı destekler. Ayrıca, Azure Graph API 'Leri kullanarak Kullanıcı profili bilgilerine erişimi göstermek için ek bir denetleyici ve görünüm de eklenmiştir: `Controllers/UserProfileController.cs` , `Views/UserProfile/Index.cshtml` ve `Views/UserProfile/Relogin.cshtml`

### <a name="file-backup-visual-studio-2015"></a>Dosya yedekleme (Visual Studio 2015)

Bağlı hizmet eklenirken, Visual Studio 2015, değiştirilen ve kaldırılan dosyaları yedekler. Etkilenen tüm dosyalar klasörüne kaydedilir `Backup/AzureAD` . Visual Studio 2017 ve üzeri yedeklemeler oluşturmaz.

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

- [Azure Active Directory için kimlik doğrulama senaryoları](./authentication-vs-authorization.md)
- [Microsoft 'a bir ASP.NET Web uygulamasına oturum açma ekleme](quickstart-v2-aspnet-webapp.md)