---
title: Azure AD'ye bağlanırken WebAPI projelerinde yapılan değişiklikler
description: Visual Studio'u kullanarak Azure AD'ye bağlandığınızda WebAPI projenize ne olduğunu açıklar
author: ghogen
manager: jillfra
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.workload: azure-vs
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: 595a89ca58e970a9c886d0b6c2dd05aecd1411ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77159395"
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>WebAPI projeme ne oldu (Visual Studio Azure Active Directory bağlı hizmet)

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-webapi-getting-started.md)
> - [Ne oldu](vs-active-directory-webapi-what-happened.md)

Bu makalede, [Visual Studio kullanarak Azure Active Directory'ye bağlı hizmeti](vs-active-directory-add-connected-service.md)eklerken webapi, ASP.NET Tek Sayfa uygulaması ve azure API projelerine ASP.NET ASP.NET değişiklikler tanımlanmaktadır. Visual Studio 2015'teki ASP.NET Azure Mobil Hizmet projeleri için de geçerlidir.

Bağlı hizmetle çalışma hakkında daha fazla bilgi için [başlarken](vs-active-directory-webapi-getting-started.md)bkz.

## <a name="added-references"></a>Eklenen başvurular

Proje dosyasını *.NET başvuruları) ve `packages.config` (NuGet başvuruları) etkiler.

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.ActiveDirectory |
| .NET; NuGet | Microsoft.Owin.Security.Jwt |
| .NET; NuGet | Microsoft.Owin.Security.OAuth |
| .NET; NuGet | Owin |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |

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
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms<br>(Yalnızca Visual Studio 2015) |
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
        <add key="ida:Tenant" value="<your selected Azure domain>" />
        <add key="ida:Audience" value="<your selected domain + / + project name>" />
    </appSettings>
    ```

- Visual Studio 2017 sadece: Ayrıca `<appSettings>`altında aşağıdaki giriş eklendi "

    ```xml
    <add key="ida:MetadataAddress" value="<domain URL + /federationmetadata/2007-06/federationmetadata.xml>" />
    ```

- Düğümün `<dependentAssembly>` `<runtime><assemblyBinding>` altına eklenen `System.IdentityModel.Tokens.Jwt`öğeler için .

- **Oku dizin veri** sebunu seçtiyseniz, aşağıdaki `<appSettings>`yapılandırma girişini aşağıdaki ler ekledi:

    ```xml
    <add key="ida:Password" value="<Your Azure AD app's new password>" />
    ```

## <a name="code-changes-and-additions"></a>Kod değişiklikleri ve eklemeler

- `[Authorize]` Öznitelik `Controllers/ValueController.cs` ve diğer varolan denetleyicileri eklendi.

- Azure AD kimlik doğrulaması için başlangıç mantığı içeren bir kimlik doğrulama başlangıç sınıfı `App_Start/Startup.Auth.cs`eklendi veya buna göre değiştirildi. **Oku dizin veri** sebunu seçtiyseniz, bu dosya bir OAuth kodu almak ve bir erişim belirteci ile değiştirmek için kod da içerir.

- (Visual Studio 2015 sadece ASP.NET 4 uygulaması ile) Kaldırıldı `App_Start/IdentityConfig.cs` ve `Controllers/AccountController.cs` `Models/IdentityModel.cs`eklendi `Providers/ApplicationAuthProvider.cs`, ve .

- Visual `Connected Services/AzureAD/ConnectedService.json` Studio'nun bağlı hizmetin `Service References/Azure AD/ConnectedService.json` eklenmesini izlemek için kullandığı bilgileri içeren (Visual Studio 2017) veya (Visual Studio 2015) eklendi.

### <a name="file-backup-visual-studio-2015"></a>Dosya yedekleme (Visual Studio 2015)

Bağlı hizmeti eklerken, Visual Studio 2015 değiştirilen ve kaldırılan dosyaları yedekler. Etkilenen tüm dosyalar klasöre `Backup/AzureAD`kaydedilir. Visual Studio 2017 yedekleme oluşturmaz.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ApplicationOAuthProvider.cs`

## <a name="changes-on-azure"></a>Azure'daki değişiklikler

- Bağlı hizmeti eklerken seçtiğiniz etki alanında bir Azure AD Uygulaması oluşturdu.
- Bu seçenek **seçilirse,** uygulamayı Oku dizini veri iznini içerecek şekilde güncelleştirildi.

[Azure Etkin Dizini hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/services/active-directory/)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizin için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [ASP.NET bir web uygulamasına Microsoft ile oturum açma ekleme](quickstart-v2-aspnet-webapp.md)
