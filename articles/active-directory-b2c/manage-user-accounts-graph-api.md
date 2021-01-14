---
title: Microsoft Graph API ile kullanıcıları yönetme
titleSuffix: Azure AD B2C
description: Microsoft Graph API 'sini çağırarak ve işlemi otomatikleştirmek için bir uygulama kimliği kullanarak Azure AD B2C kiracısındaki kullanıcıları yönetme.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/13/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ff3cd858de86d21637f4a7a9ab9d9a83c7022f5a
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98178883"
---
# <a name="manage-azure-ad-b2c-user-accounts-with-microsoft-graph"></a>Microsoft Graph ile Azure AD B2C Kullanıcı hesaplarını yönetme

Microsoft Graph, Microsoft Graph API 'sinde oluşturma, okuma, güncelleştirme ve silme yöntemleri sağlayarak Azure AD B2C dizininizdeki Kullanıcı hesaplarını yönetmenizi sağlar. Mevcut bir Kullanıcı deposunu Azure AD B2C kiracıya geçirebilir ve Microsoft Graph API 'sini çağırarak diğer Kullanıcı hesabı yönetim işlemlerini gerçekleştirebilirsiniz.

Aşağıdaki bölümlerde, Microsoft Graph API 'siyle Azure AD B2C Kullanıcı yönetiminin temel yönleri sunulur. Burada sunulan Microsoft Graph API işlemleri, türleri ve özellikleri, Microsoft Graph API başvuru belgelerinde görünen bir alt kümesidir.

## <a name="register-a-management-application"></a>Bir yönetim uygulamasını kaydetme

Yazdığınız herhangi bir kullanıcı yönetimi uygulaması veya betiği Azure AD B2C kiracınızdaki kaynaklarla etkileşime girebilmek için izinleri veren bir uygulama kaydına ihtiyacınız vardır.

Yönetim uygulamanızın kullanabileceği bir uygulama kaydı oluşturmak için bu nasıl yapılır makalesindeki adımları izleyin:

[Microsoft Graph ile Azure AD B2C yönetme](microsoft-graph-get-started.md)

## <a name="user-management-microsoft-graph-operations"></a>Kullanıcı Yönetimi Microsoft Graph işlemleri

[MICROSOFT Graph API](/graph/api/resources/user)'de aşağıdaki kullanıcı yönetim işlemleri mevcuttur:

- [Kullanıcıların listesini al](/graph/api/user-list)
- [Kullanıcı oluşturma](/graph/api/user-post-users)
- [Kullanıcı al](/graph/api/user-get)
- [Kullanıcı güncelleştirme](/graph/api/user-update)
- [Kullanıcı silme](/graph/api/user-delete)


## <a name="code-sample-how-to-programmatically-manage-user-accounts"></a>Kod örneği: program aracılığıyla Kullanıcı hesaplarını yönetme

Bu kod örneği, Microsoft Graph API ile etkileşim kurmak için [Microsoft Graph SDK 'sını](/graph/sdks/sdks-overview) kullanan bir .NET Core konsol uygulamasıdır. Kodu, bir Azure AD B2C kiracısındaki kullanıcıları programlı bir şekilde yönetmek için API 'nin nasıl çağrılacağını gösterir.
[Örnek Arşivi](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management/archive/master.zip) (*. zip) Indirebilir, GitHub 'daki [depoya gözatabilir](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management) veya depoyu klonlayabilirsiniz:

```cmd
git clone https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management.git
```

Kod örneğini aldıktan sonra, ortamınız için yapılandırın ve ardından projeyi derleyin:

1. Projeyi [Visual Studio](https://visualstudio.microsoft.com) 'da veya [Visual Studio Code](https://code.visualstudio.com)açın.
1. `src/appsettings.json` dosyasını açın.
1. Bölümünde, `appSettings` `your-b2c-tenant` kiracınızın adıyla ve `Application (client) ID` ve `Client secret` yönetim uygulaması kaydınızın değerleriyle değiştirin (Bu makalenin [yönetim uygulaması kaydetme](#register-a-management-application) bölümüne bakın).
1. Deponun yerel kopyanızda bir konsol penceresi açın, dizine geçin ve `src` ardından projeyi derleyin:
    ```console
    cd src
    dotnet build
    ```
1. Uygulamayı `dotnet` komutla çalıştırın:

    ```console
    dotnet bin/Debug/netcoreapp3.1/b2c-ms-graph.dll
    ```

Uygulama, yürütebilmeniz için kullanabileceğiniz komutların bir listesini görüntüler. Örneğin, tüm kullanıcıları alma, tek bir Kullanıcı alma, Kullanıcı silme, Kullanıcı parolasını güncelleştirme ve toplu içeri aktarma.

### <a name="code-discussion"></a>Kod tartışması

Örnek kod, Microsoft Graph erişen yüksek kaliteli, verimli ve esnek uygulamalar oluşturmayı basitleştirmek için tasarlanan [Microsoft Graph SDK 'sını](/graph/sdks/sdks-overview)kullanır.

Microsoft Graph API 'sine yönelik herhangi bir istek, kimlik doğrulaması için bir erişim belirteci gerektirir. Çözüm, Microsoft Graph SDK ile kullanmak üzere Microsoft kimlik doğrulama kitaplığı 'nın (MSAL) kimlik doğrulama senaryosu tabanlı bir sarmalayıcı sağlayan [Microsoft. Graph. auth](https://www.nuget.org/packages/Microsoft.Graph.Auth/) NuGet paketini kullanır.

`RunAsync` _Program.cs_ dosyasındaki yöntemi:

1. _appsettings.js_ dosyadaki uygulama ayarlarını okur
1. Kimlik doğrulama sağlayıcısını [OAuth 2,0 istemci kimlik bilgileri verme](../active-directory/develop/v2-oauth2-client-creds-grant-flow.md) akışı kullanarak başlatır. İstemci kimlik bilgileri verme akışı ile, uygulama Microsoft Graph API 'sini çağırmak için bir erişim belirteci alabilir.
1. Kimlik doğrulama sağlayıcısına Microsoft Graph hizmeti istemcisini ayarlar:

    ```csharp
    // Read application settings from appsettings.json (tenant ID, app ID, client secret, etc.)
    AppSettings config = AppSettingsFile.ReadFromJsonFile();

    // Initialize the client credential auth provider
    IConfidentialClientApplication confidentialClientApplication = ConfidentialClientApplicationBuilder
        .Create(config.AppId)
        .WithTenantId(config.TenantId)
        .WithClientSecret(config.ClientSecret)
        .Build();
    ClientCredentialProvider authProvider = new ClientCredentialProvider(confidentialClientApplication);

    // Set up the Microsoft Graph service client with client credentials
    GraphServiceClient graphClient = new GraphServiceClient(authProvider);
    ```

Başlatılmış *GraphServiceClient* daha sonra Kullanıcı yönetimi işlemlerini gerçekleştirmek için _UserService.cs_ ' de kullanılır. Örneğin, Kiracıdaki Kullanıcı hesaplarının bir listesini alma:

```csharp
public static async Task ListUsers(GraphServiceClient graphClient)
{
    Console.WriteLine("Getting list of users...");

    // Get all users (one page)
    var result = await graphClient.Users
        .Request()
        .Select(e => new
        {
            e.DisplayName,
            e.Id,
            e.Identities
        })
        .GetAsync();

    foreach (var user in result.CurrentPage)
    {
        Console.WriteLine(JsonConvert.SerializeObject(user));
    }
}
```

[Microsoft Graph SDK 'ları kullanarak API çağrıları yapın](/graph/sdks/create-requests) Microsoft Graph bilgileri okuma ve yazma hakkında bilgiler içerir, `$select` döndürülen özellikleri denetlemek, özel sorgu parametreleri sağlamak ve `$filter` ve sorgu parametrelerini kullanmak için kullanın `$orderBy` .

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2C kaynakları için desteklenen Microsoft Graph API işlemlerinin tam dizini için bkz. [Azure AD B2C için kullanılabilen Microsoft Graph işlemleri](microsoft-graph-operations.md).

<!-- LINK -->

[graph-objectIdentity]: /graph/api/resources/objectidentity
[graph-user]: (https://docs.microsoft.com/graph/api/resources/user)
