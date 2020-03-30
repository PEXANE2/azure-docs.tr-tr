---
title: Microsoft Graph API ile kullanıcıları yönetme
titleSuffix: Azure AD B2C
description: Microsoft Graph API'yi arayarak ve işlemi otomatikleştirmek için bir uygulama kimliği kullanarak Azure AD B2C kiracısındaki kullanıcıları yönetme.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 3bd166572cea23fbb710cd053c28f51e76ba534a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476680"
---
# <a name="manage-azure-ad-b2c-user-accounts-with-microsoft-graph"></a>Microsoft Graph ile Azure AD B2C kullanıcı hesaplarını yönetme

Microsoft Graph, Microsoft Graph API'sinde oluşturma, okuma, güncelleştirme ve silme yöntemleri sağlayarak Azure AD B2C dizininizdeki kullanıcı hesaplarını yönetmenize olanak tanır. Varolan bir kullanıcı mağazasını Azure AD B2C kiracısına geçirebilir ve Microsoft Graph API'yi arayarak diğer kullanıcı hesabı yönetimi işlemlerini gerçekleştirebilirsiniz.

Takip eden bölümlerde, Azure AD B2C kullanıcı yönetiminin Microsoft Graph API'si ile önemli yönleri sunulur. Burada sunulan Microsoft Graph API işlemleri, türleri ve özellikleri, Microsoft Graph API başvuru belgelerinde görünen bir alt kümesidir.

## <a name="register-a-management-application"></a>Yönetim başvurusu kaydetme

Yazdığınız herhangi bir kullanıcı yönetimi uygulaması veya komut dosyasının Azure AD B2C kiracınızdaki kaynaklarla etkileşim kurabilmesi için, izinleri veren bir uygulama kaydına ihtiyacınız vardır.

Yönetim uygulamanızın kullanabileceği bir uygulama kaydı oluşturmak için bu nasıl yapılAbilir makalesindeki adımları izleyin:

[Microsoft Graph ile Azure AD B2C'yi yönetme](microsoft-graph-get-started.md)

## <a name="user-management-microsoft-graph-operations"></a>Kullanıcı yönetimi Microsoft Graph işlemleri

Aşağıdaki kullanıcı yönetimi işlemleri [Microsoft Graph API'de](https://docs.microsoft.com/graph/api/resources/user)kullanılabilir:

- [Kullanıcıların listesini alma](https://docs.microsoft.com/graph/api/user-list)
- [Kullanıcı oluşturma](https://docs.microsoft.com/graph/api/user-post-users)
- [Bir kullanıcı alma](https://docs.microsoft.com/graph/api/user-get)
- [Kullanıcıyı güncelleştirme](https://docs.microsoft.com/graph/api/user-update)
- [Kullanıcı sil](https://docs.microsoft.com/graph/api/user-delete)

## <a name="user-properties"></a>Kullanıcı özellikleri

### <a name="display-name-property"></a>Görüntü adı özelliği

Kullanıcı `displayName` için Azure portalı kullanıcı yönetiminde görüntülenecek ad dır ve access token Azure AD B2C uygulamaya döner. Bu özellik gereklidir.

### <a name="identities-property"></a>Kimlik özelliği

Tüketici, ortak veya vatandaş olabilecek bir müşteri hesabı, bu kimlik türleri ile ilişkilendirilebilir:

- **Yerel** kimlik - Kullanıcı adı ve parola Azure AD B2C dizininde yerel olarak depolanır. Bu kimliklere sık sık "yerel hesaplar" diyoruz.
- **Federe** kimlik - *Sosyal* veya *kurumsal* hesaplar olarak da bilinen kullanıcının kimliği, Facebook, Microsoft, ADFS veya Salesforce gibi federe bir kimlik sağlayıcısı tarafından yönetilir.

Müşteri hesabı olan bir kullanıcı birden çok kimlikle oturum açabilir. Örneğin, kullanıcı adı, e-posta, çalışan kimliği, devlet kimliği ve diğerleri. Tek bir hesabın hem yerel hem de sosyal olarak aynı parolaya sahip birden çok kimliği olabilir.

Microsoft Graph API'sinde, hem yerel hem de federe `identities` kimlikler, tür [objectIdentity][graph-objectIdentity]olan kullanıcı özniteliğinde depolanır. Koleksiyon, `identities` kullanıcı hesabında oturum açmada kullanılan bir kimlik kümesini temsil eder. Bu koleksiyon, kullanıcının ilişkili kimliklerinden herhangi biriyle kullanıcı hesabında oturum açmasını sağlar.

| Özellik   | Tür |Açıklama|
|:---------------|:--------|:----------|
|signInType|string| Dizininizde kullanıcı oturum açma türlerini belirtir. Yerel hesap `emailAddress`için: `emailAddress2` `emailAddress3`, `userName`, `emailAddress1`, , , veya istediğiniz başka bir tür. Sosyal `federated`hesap.|
|yayınlayan|string|Kimliği vereni belirtir. Yerel hesaplar için **(signInType** olmadığı `federated`durumlarda), bu özellik yerel B2C `contoso.onmicrosoft.com`kiracı varsayılan etki alanı adı, örneğin. Sosyal kimlik için **(signInType** olduğu `federated`yerde) değer, örneğin, verenin adıdır`facebook.com`|
|verenAssignedId|string|Veren tarafından kullanıcıya atanan benzersiz tanımlayıcıyı belirtir. **İhraççı** ve **verenAssignedId** kombinasyonu kiracıiçinde benzersiz olmalıdır. Yerel hesap için **signInType** ayarlandığında `emailAddress` veya `userName`kullanıcının oturum açma adını temsil eder.<br>**signInType** ayarlandığında: <ul><li>`emailAddress`(veya benzeri `emailAddress` `emailAddress1`ile başlar) **verenAssignedId** geçerli bir e-posta adresi olmalıdır</li><li>`userName`(veya başka bir değer), **verenAssignedId** [bir e-posta adresinin](https://tools.ietf.org/html/rfc3696#section-3) geçerli bir yerel parçası olmalıdır</li><li>`federated`, **verenAssignedId** federe hesap benzersiz tanımlayıcı temsil eder</li></ul>|

Oturum açma adı, oturum açma olarak e-posta adresi ve sosyal kimlik içeren yerel hesap kimliğine sahip aşağıdaki **Kimlikler** özelliği. 

 ```JSON
 "identities": [
     {
       "signInType": "userName",
       "issuer": "contoso.onmicrosoft.com",
       "issuerAssignedId": "johnsmith"
     },
     {
       "signInType": "emailAddress",
       "issuer": "contoso.onmicrosoft.com",
       "issuerAssignedId": "jsmith@yahoo.com"
     },
     {
       "signInType": "federated",
       "issuer": "facebook.com",
       "issuerAssignedId": "5eecb0cd"
     }
   ]
 ```

Federe kimlikler için, kimlik sağlayıcısına bağlı olarak, **verenAssignedId** uygulama veya geliştirme hesabı başına belirli bir kullanıcı için benzersiz bir değerdir. Azure AD B2C ilkesini, sosyal sağlayıcı veya aynı geliştirme hesabı içindeki başka bir uygulama tarafından daha önce atanan uygulama kimliğiyle yapılandırın.

### <a name="password-profile-property"></a>Parola profili özelliği

Yerel bir kimlik için **passwordProfile** özelliği gereklidir ve kullanıcının parolasını içerir. `false`Özellik' `forceChangePasswordNextSignIn` in ' e ayarlanınması gerekir.

Federe (sosyal) bir kimlik için **passwordProfile** özelliği gerekli değildir.

```JSON
"passwordProfile" : {
    "password": "password-value",
    "forceChangePasswordNextSignIn": false
  }
```

### <a name="password-policy-property"></a>Parola ilkesi özelliği

Azure AD B2C parola ilkesi (yerel hesaplar için) Azure Etkin Dizin [güçlü parola gücü](../active-directory/authentication/concept-sspr-policy.md) ilkesini temel adatır. Azure AD B2C kaydolma veya oturum açma ve parola sıfırlama ilkeleri bu güçlü parola kuvvetini gerektirir ve parolaların süresi dolmaz.

Kullanıcı geçişi senaryolarında, geçirmek istediğiniz hesapların parola gücü Azure AD B2C tarafından uygulanan [güçlü parola gücünden](../active-directory/authentication/concept-sspr-policy.md) daha zayıfsa, güçlü parola gereksinimini devre dışı kullanabilirsiniz. Varsayılan parola ilkesini değiştirmek `passwordPolicies` için `DisableStrongPassword`özelliği ' niçin . Örneğin, kullanıcı isteği oluşturmasını aşağıdaki gibi değiştirebilirsiniz:

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

### <a name="extension-properties"></a>Uzantı özellikleri

Müşteriye yönelik her uygulamanın toplanacak bilgiler için benzersiz gereksinimleri vardır. Azure AD B2C kiracınız, Verilen Ad, Soyad, Şehir ve Posta Kodu gibi özelliklerde depolanan yerleşik bir bilgi kümesiyle birlikte gelir. Azure AD B2C ile, her müşteri hesabında depolanan özellik kümesini genişletebilirsiniz. Özel öznitelikleri tanımlama hakkında daha fazla bilgi için bkz: [özel öznitelikler (kullanıcı akışları)](user-flow-custom-attributes.md) ve [özel öznitelikler (özel ilkeler)](custom-policy-custom-attributes.md).

Microsoft Graph API, uzantı özniteliklerine sahip bir kullanıcı oluşturmayı ve güncelleştirmeyi destekler. Grafik API'deki uzantı öznitelikleri, `extension_ApplicationObjectID_attributename`kuralı kullanılarak adlandırılır. Örnek:

```JSON
"extension_831374b3bd5041bfaa54263ec9e050fc_loyaltyNumber": "212342"
```

## <a name="code-sample"></a>Kod örneği

Bu kod örneği, Microsoft Graph API ile etkileşimde kalmak için [Microsoft Graph SDK](https://docs.microsoft.com/graph/sdks/sdks-overview) kullanan bir .NET Core konsol uygulamasıdır. Kodu, Bir Azure AD B2C kiracısındaki kullanıcıları programlı bir şekilde yönetmek için API'nin nasıl arandığını gösterir.
Örnek [arşivi indirebilir](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management/archive/master.zip) (*.zip), GitHub'daki [depoya göz atabilir](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management) veya depoyu klonlayabilirsiniz:

```cmd
git clone https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management.git
```

Kod örneğini aldıktan sonra, ortamınız için yapılandırın ve projeyi oluşturun:

1. [Projeyi Visual Studio](https://visualstudio.microsoft.com) veya [Visual Studio Code'da](https://code.visualstudio.com)açın.
1. `src/appsettings.json` dosyasını açın.
1. `appSettings` Bölümde, `your-b2c-tenant` kiracınızın adı ve `Application (client) ID` `Client secret` yönetim başvurusu kaydı için değerler (Bu makalenin bir yönetim uygulama [bölümüne](#register-a-management-application) bakın) değiştirin.
1. Repo'nun yerel klonunuzda bir konsol penceresi açın, dizine `src` geçin ve projeyi oluşturun:
    ```console
    cd src
    dotnet build
    ```
1. Uygulamayı `dotnet` komutla çalıştırın:

    ```console
    dotnet bin/Debug/netcoreapp3.0/b2c-ms-graph.dll
    ```

Uygulama, yürütebileceğiniz komutların listesini görüntüler. Örneğin, tüm kullanıcıları alın, tek bir kullanıcı alın, bir kullanıcıyı silin, kullanıcının parolalarını güncelleştirin ve toplu içe aktarma.

### <a name="code-discussion"></a>Kod tartışması

Örnek kod, Microsoft Graph'a erişen yüksek kaliteli, verimli ve esnek uygulamalar oluşturmayı kolaylaştırmak için tasarlanan [Microsoft Graph SDK'yı](https://docs.microsoft.com/graph/sdks/sdks-overview)kullanır. Bu nedenle, tüm Microsoft Graph API'sini doğrudan yapmanız gerekmez.

Microsoft Graph API'ye yapılan herhangi bir istek, kimlik doğrulaması için bir erişim belirteci gerektirir. Çözüm, Microsoft Graph SDK ile kullanılmak üzere Microsoft Kimlik Doğrulama Kitaplığı'nın (MSAL) kimlik doğrulama senaryosu tabanlı bir paket sağlayan [Microsoft.Graph.Auth](https://www.nuget.org/packages/Microsoft.Graph.Auth/) NuGet paketini kullanır.

Program.cs `RunAsync` dosyasındaki _Program.cs_ yöntem:

1. uygulama ayarlarını _appsettings.json_ dosyasından okur
1. [Auth 2.0 istemci kimlik bilgilerini hibe](../active-directory/develop/v2-oauth2-client-creds-grant-flow.md) akışını kullanarak auth sağlayıcısını başolarak karşılar. İstemci kimlik bilgileri hibe akışı yla, uygulama Microsoft Graph API'yi aramak için bir erişim jetonu alabilir.
1. Auth sağlayıcısıyla Microsoft Graph hizmet istemcisini ayarlar:

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

Daha sonra *GraphServiceClient* UserService.cs kullanıcı yönetimi işlemlerini gerçekleştirmek _için_ başlatılması nı sağlar. Örneğin, kiracıdaki kullanıcı hesaplarının listesini almak:

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

[Microsoft Graph SDK'larını kullanarak API aramaları yapma,](https://docs.microsoft.com/graph/sdks/create-requests) Microsoft Graph'tan `$select` bilgi okuma ve yazma, döndürülen özellikleri `$filter` denetlemek, özel sorgu parametreleri sağlamak ve `$orderBy` sorgu parametrelerini kullanmak hakkında bilgiler içerir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2C kaynakları için desteklenen Microsoft Graph API işlemlerinin tam dizini için Azure [AD B2C için kullanılabilen Microsoft Graph işlemlerine](microsoft-graph-operations.md)bakın.

<!-- LINK -->

[graph-objectIdentity]: https://docs.microsoft.com/graph/api/resources/objectidentity
[graph-user]: (https://docs.microsoft.com/graph/api/resources/user)
