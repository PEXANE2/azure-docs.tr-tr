---
title: Azure AD 'ye bağlanılırken, WebAPI projelerinde yapılan değişiklikler
description: Visual Studio kullanarak Azure AD 'ye bağlandığınızda, WebAPI projenizde ne olacağını açıklar
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
ms.openlocfilehash: 9858fdb7da80fe03498392ea93bf3c83b8a7e4dd
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76699878"
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>WebAPI projem 'e ne oldu (Visual Studio Azure Active Directory bağlı hizmeti)

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-webapi-getting-started.md)
> - [Ne oldu](vs-active-directory-webapi-what-happened.md)

Bu makalede, [Visual Studio kullanarak Azure Active Directory bağlı hizmeti](vs-active-directory-add-connected-service.md)eklenirken ASP.net WebAPI, ASP.net tek sayfalı uygulama ve ASP.net Azure API projelerinde yapılan tam değişiklikler tanımlanmaktadır. Visual Studio 2015 ' de ASP.NET Azure mobil hizmet projeleri için de geçerlidir.

Bağlı hizmetle çalışma hakkında daha fazla bilgi için [bkz. Başlarken](vs-active-directory-webapi-getting-started.md).

## <a name="added-references"></a>Eklenen başvurular

Proje dosyasını *. NET başvuruları) ve `packages.config` (NuGet başvuruları) etkiler.

| Tür | Başvuru |
| --- | --- |
| NET NuGet | Microsoft.Owin |
| NET NuGet | Microsoft.Owin.Host.SystemWeb |
| NET NuGet | Microsoft.Owin.Security |
| NET NuGet | Microsoft. Owin. Security. ActiveDirectory |
| NET NuGet | Microsoft.Owin.Security.Jwt |
| NET NuGet | Microsoft. Owin. Security. OAuth |
| NET NuGet | Owın |
| NET NuGet | System.IdentityModel.Tokens.Jwt |

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
| .NET        | Microsoft. IdentityModel. clients. ActiveDirectory. WindowsForms<br>(Yalnızca Visual Studio 2015) |
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
        <add key="ida:Tenant" value="<your selected Azure domain>" />
        <add key="ida:Audience" value="<your selected domain + / + project name>" />
    </appSettings>
    ```

- Yalnızca Visual Studio 2017: `<appSettings>`"altına aşağıdaki giriş de eklenmiştir

    ```xml
    <add key="ida:MetadataAddress" value="<domain URL + /federationmetadata/2007-06/federationmetadata.xml>" />
    ```

- `System.IdentityModel.Tokens.Jwt`için `<runtime><assemblyBinding>` düğümü altına `<dependentAssembly>` öğeleri eklendi.

- **Dizin verilerini oku** seçeneğini belirlediyseniz, `<appSettings>`altına aşağıdaki yapılandırma girişi eklendi:

    ```xml
    <add key="ida:Password" value="<Your Azure AD app's new password>" />
    ```

## <a name="code-changes-and-additions"></a>Kod değişiklikleri ve eklemeler

- `Controllers/ValueController.cs` ve var olan diğer denetleyicilere `[Authorize]` özniteliği eklendi.

- Azure AD kimlik doğrulaması için başlangıç mantığını içeren `App_Start/Startup.Auth.cs`bir kimlik doğrulama başlangıç sınıfı eklediniz veya onu uygun şekilde değiştirmiş. **Dizin verilerini oku** seçeneğini belirlediyseniz, bu dosya Ayrıca bir OAuth kodu alacak kodu içerir ve bir erişim belirteci için onu değiş tokuş sağlar.

- (Yalnızca ASP.NET 4 uygulama içeren Visual Studio 2015) `App_Start/IdentityConfig.cs` kaldırıldı ve `Controllers/AccountController.cs`, `Models/IdentityModel.cs`ve `Providers/ApplicationAuthProvider.cs`eklendi.

- Bağlı hizmetin eklenmesini izlemek için Visual Studio 'nun kullandığı bilgileri içeren `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) veya `Service References/Azure AD/ConnectedService.json` (Visual Studio 2015) eklendi.

### <a name="file-backup-visual-studio-2015"></a>Dosya yedekleme (Visual Studio 2015)

Bağlı hizmet eklenirken, Visual Studio 2015, değiştirilen ve kaldırılan dosyaları yedekler. Etkilenen tüm dosyalar `Backup/AzureAD`klasöre kaydedilir. Visual Studio 2017, yedeklemeler oluşturmaz.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ApplicationOAuthProvider.cs`

## <a name="changes-on-azure"></a>Azure 'daki değişiklikler

- Bağlı hizmeti eklerken seçtiğiniz etki alanında bir Azure AD uygulaması oluşturuldu.
- Uygulama, bu seçenek seçildiyse, **Dizin verilerini oku** iznini içerecek şekilde güncelleştirildi.

[Azure Active Directory hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [Microsoft 'a bir ASP.NET Web uygulamasına oturum açma ekleme](quickstart-v1-aspnet-webapp.md)
