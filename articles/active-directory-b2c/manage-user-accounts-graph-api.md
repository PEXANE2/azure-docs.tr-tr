---
title: Microsoft Graph API ile kullanıcıları yönetme
titleSuffix: Azure AD B2C
description: Microsoft Graph API 'sini çağırarak ve işlemi otomatikleştirmek için bir uygulama kimliği kullanarak Azure AD B2C kiracısındaki kullanıcıları yönetme.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: b53405d199072211304e21b681de646c4e41243c
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77585637"
---
# <a name="manage-azure-ad-b2c-user-accounts-with-microsoft-graph"></a>Microsoft Graph ile Azure AD B2C Kullanıcı hesaplarını yönetme

Microsoft Graph, Microsoft Graph API 'sinde oluşturma, okuma, güncelleştirme ve silme yöntemleri sağlayarak Azure AD B2C dizininizdeki Kullanıcı hesaplarını yönetmenizi sağlar. Mevcut bir Kullanıcı deposunu Azure AD B2C kiracıya geçirebilir ve Microsoft Graph API 'sini çağırarak diğer Kullanıcı hesabı yönetim işlemlerini gerçekleştirebilirsiniz.

Aşağıdaki bölümlerde, Microsoft Graph API 'siyle Azure AD B2C Kullanıcı yönetiminin temel yönleri sunulur. Burada sunulan Microsoft Graph API işlemleri, türleri ve özellikleri, Microsoft Graph API başvuru belgelerinde görünen bir alt kümesidir.

## <a name="register-a-management-application"></a>Bir yönetim uygulamasını kaydetme

Yazdığınız herhangi bir kullanıcı yönetimi uygulaması veya betiği Azure AD B2C kiracınızdaki kaynaklarla etkileşime girebilmek için izinleri veren bir uygulama kaydına ihtiyacınız vardır.

Yönetim uygulamanızın kullanabileceği bir uygulama kaydı oluşturmak için bu nasıl yapılır makalesindeki adımları izleyin:

[Microsoft Graph ile Azure AD B2C yönetme](microsoft-graph-get-started.md)

## <a name="user-management-microsoft-graph-operations"></a>Kullanıcı Yönetimi Microsoft Graph işlemleri

[MICROSOFT Graph API](https://docs.microsoft.com/graph/api/resources/user)'de aşağıdaki kullanıcı yönetim işlemleri mevcuttur:

- [Kullanıcıların listesini al](https://docs.microsoft.com/graph/api/user-list)
- [Kullanıcı oluşturma](https://docs.microsoft.com/graph/api/user-post-users)
- [Kullanıcı al](https://docs.microsoft.com/graph/api/user-get)
- [Kullanıcı güncelleştirme](https://docs.microsoft.com/graph/api/user-update)
- [Kullanıcı silme](https://docs.microsoft.com/graph/api/user-delete)

## <a name="user-properties"></a>Kullanıcı özellikleri

### <a name="display-name-property"></a>Görünen ad özelliği

`displayName`, Kullanıcı için Azure portal Kullanıcı yönetiminde görüntülenecek addır ve erişim belirtecinde Azure AD B2C uygulamaya geri döner. Bu özellik gereklidir.

### <a name="identities-property"></a>Kimlikler özelliği

Bir tüketici, iş ortağı veya vatandaşlık olabilecek bir müşteri hesabı şu kimlik türleriyle ilişkilendirilebilir:

- **Yerel** kimlik-Kullanıcı adı ve parola Azure AD B2C dizininde yerel olarak depolanır. Genellikle bu kimliklere "yerel hesaplar" olarak başvurduk.
- **Federasyon** kimliği- *sosyal* veya *Kurumsal* hesaplar olarak da bilinen kullanıcı KIMLIĞI, Facebook, Microsoft, ADFS veya Salesforce gibi bir federal kimlik sağlayıcısı tarafından yönetilir.

Müşteri hesabı olan bir Kullanıcı birden çok kimlik ile oturum açabilir. Örneğin, Kullanıcı adı, e-posta, çalışan KIMLIĞI, kamu KIMLIĞI ve diğerleri. Tek bir hesabın aynı parolayla hem yerel hem de sosyal birden çok kimliği olabilir.

Microsoft Graph API 'sinde, hem yerel hem de Federasyon kimlikleri, [Objectıdentity][graph-objectIdentity]türünde olan User `identities` özniteliğinde depolanır. `identities` koleksiyonu, bir kullanıcı hesabında oturum açmak için kullanılan bir kimlik kümesini temsil eder. Bu koleksiyon, kullanıcının Kullanıcı hesabında ilişkili kimliklerinden herhangi biriyle oturum açmasını sağlar.

| Özellik   | Tür |Açıklama|
|:---------------|:--------|:----------|
|Signıntype|string| Dizininizdeki Kullanıcı oturum açma türlerini belirtir. Yerel hesap için: `emailAddress`, `emailAddress1`, `emailAddress2`, `emailAddress3`, `userName`veya istediğiniz diğer herhangi bir tür. Sosyal hesabın `federated`olarak ayarlanması gerekir.|
|enden|string|Kimliğin verenini belirtir. Yerel hesaplar için ( **Signıntype** `federated`değil), bu ÖZELLIK yerel B2C kiracısı varsayılan etki alanı adıdır, örneğin `contoso.onmicrosoft.com`. Sosyal kimlik için ( **Signıntype** `federated`), değer verenin adıdır, örneğin `facebook.com`|
|ıssueratandıd|string|Kullanıcıya veren tarafından atanan benzersiz tanımlayıcıyı belirtir. **Issuer** ve **ıssueratanmadı** birleşimi kiracınız dahilinde benzersiz olmalıdır. Yerel hesap için, **Signıntype** `emailAddress` veya `userName`olarak ayarlandığında, Kullanıcı için oturum açma adını temsil eder.<br>**Signıntype** şu şekilde ayarlandığında: <ul><li>`emailAddress` (veya `emailAddress1`gibi `emailAddress` başlar) **ıssueratandıd** geçerli bir e-posta adresi olmalıdır</li><li>`userName` (veya başka bir değer), **ıssueratandıd** [bir e-posta adresinin geçerli bir yerel parçası](https://tools.ietf.org/html/rfc3696#section-3) olmalıdır</li><li>`federated`, **ıssueratanmadı** , Federasyon hesabının benzersiz tanımlayıcısını temsil ediyor</li></ul>|

Federal kimlikler için, kimlik sağlayıcısına bağlı olarak, **ıssueratandıd** , uygulama veya geliştirme hesabı başına belirli bir kullanıcı için benzersiz bir değerdir. Azure AD B2C ilkesini, daha önce sosyal sağlayıcı veya aynı geliştirme hesabı içindeki başka bir uygulama tarafından atanan aynı uygulama KIMLIĞIYLE yapılandırın.

### <a name="password-profile-property"></a>Parola profili özelliği

Yerel bir kimlik için **Passwordprofile** özelliği gereklidir ve kullanıcının parolasını içerir. `forceChangePasswordNextSignIn` özelliği `false`olarak ayarlanmalıdır.

Bir federal (sosyal) kimlik için **Passwordprofile** özelliği gerekli değildir.

```JSON
"passwordProfile" : {
    "password": "password-value",
    "forceChangePasswordNextSignIn": false
  }
```

### <a name="password-policy-property"></a>Parola İlkesi özelliği

Azure AD B2C parola ilkesi (yerel hesaplar için) Azure Active Directory [güçlü parola gücü](../active-directory/authentication/concept-sspr-policy.md) ilkesini temel alır. Azure AD B2C kaydolma veya oturum açma ve parola sıfırlama ilkeleri için bu güçlü parola gücü gerekir ve parolaların süreleri dolmaz.

Kullanıcı geçişi senaryolarında, geçirmek istediğiniz hesapların Azure AD B2C tarafından zorlanan [güçlü parola kuvvetinden](../active-directory/authentication/concept-sspr-policy.md) daha zayıf olması halinde güçlü parola gereksinimini devre dışı bırakabilirsiniz. Varsayılan parola ilkesini değiştirmek için `passwordPolicies` özelliğini `DisableStrongPassword`olarak ayarlayın. Örneğin, kullanıcı oluştur isteğini aşağıdaki şekilde değiştirebilirsiniz:

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

### <a name="extension-properties"></a>Uzantı özellikleri

Müşterilere yönelik her uygulamanın, toplanacak bilgiler için benzersiz gereksinimleri vardır. Azure AD B2C kiracınız, belirtilen ad, soyadı, şehir ve posta kodu gibi özelliklerde depolanan yerleşik bir bilgi kümesiyle gelir. Azure AD B2C, her müşteri hesabında depolanan özellikler kümesini genişletebilirsiniz. Özel öznitelikler tanımlama hakkında daha fazla bilgi için bkz. [özel öznitelikler (Kullanıcı akışları)](user-flow-custom-attributes.md) ve [özel öznitelikler (özel ilkeler)](custom-policy-custom-attributes.md).

Microsoft Graph API, uzantı öznitelikleri olan bir kullanıcının oluşturulmasını ve güncelleştirilmesini destekler. Graph API uzantı öznitelikleri, kural `extension_ApplicationObjectID_attributename`kullanılarak adlandırılır. Örnek:

```JSON
"extension_831374b3bd5041bfaa54263ec9e050fc_loyaltyNumber": "212342"
```

## <a name="code-sample"></a>Kod örneği

Bu kod örneği, Microsoft Graph API ile etkileşim kurmak için [Microsoft Graph SDK 'sını](https://docs.microsoft.com/graph/sdks/sdks-overview) kullanan bir .NET Core konsol uygulamasıdır. Kodu, bir Azure AD B2C kiracısındaki kullanıcıları programlı bir şekilde yönetmek için API 'nin nasıl çağrılacağını gösterir.
[Örnek Arşivi](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management/archive/master.zip) (*. zip) Indirebilir, GitHub 'daki [depoya gözatabilir](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management) veya depoyu klonlayabilirsiniz:

```cmd
git clone https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management.git
```

Kod örneğini aldıktan sonra, ortamınız için yapılandırın ve ardından projeyi derleyin:

1. Projeyi [Visual Studio](https://visualstudio.microsoft.com) 'da veya [Visual Studio Code](https://code.visualstudio.com)açın.
1. `src/appsettings.json` programını açın.
1. `appSettings` bölümünde, `your-b2c-tenant`, kiracınızın adıyla değiştirin ve `Application (client) ID` ve `Client secret` yönetim uygulaması kaydınızın değerleriyle (Bu makalenin [yönetim uygulaması kaydetme](#register-a-management-application) bölümüne bakın).
1. Deponun yerel kopyanızda bir konsol penceresi açın, `src` dizinine geçin ve ardından projeyi derleyin:
    ```console
    cd src
    dotnet build
    ```
1. `dotnet` komutuyla uygulamayı çalıştırın:

```console
dotnet bin/Debug/netcoreapp3.0/b2c-ms-graph.dll
```

Uygulama, yürütebilmeniz için kullanabileceğiniz komutların bir listesini görüntüler. Örneğin, tüm kullanıcıları alma, tek bir Kullanıcı alma, Kullanıcı silme, Kullanıcı parolasını güncelleştirme ve toplu içeri aktarma.

### <a name="code-discussion"></a>Kod tartışması

Örnek kod, Microsoft Graph erişen yüksek kaliteli, verimli ve esnek uygulamalar oluşturmayı basitleştirmek için tasarlanan [Microsoft Graph SDK 'sını](https://docs.microsoft.com/graph/sdks/sdks-overview)kullanır. Bu nedenle, tüm Microsoft Graph API 'sini doğrudan yapmanız gerekmez.

Microsoft Graph API 'sine yönelik herhangi bir istek, kimlik doğrulaması için bir erişim belirteci gerektirir. Çözüm, Microsoft Graph SDK ile kullanmak üzere Microsoft kimlik doğrulama kitaplığı 'nın (MSAL) kimlik doğrulama senaryosu tabanlı bir sarmalayıcı sağlayan [Microsoft. Graph. auth](https://www.nuget.org/packages/Microsoft.Graph.Auth/) NuGet paketini kullanır.

_Program.cs_ dosyasındaki `RunAsync` yöntemi:

1. _AppSettings. JSON_ dosyasından uygulama ayarlarını okur
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

[Microsoft Graph SDK 'ları kullanarak API çağrıları yapın](https://docs.microsoft.com/graph/sdks/create-requests) Microsoft Graph bilgileri okuma ve yazma hakkında bilgiler içerir, döndürülen özellikleri denetlemek, özel sorgu parametreleri sağlamak ve `$filter` ve `$orderBy` sorgu parametrelerini kullanmak için `$select` kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2C kaynakları için desteklenen Microsoft Graph API işlemlerinin tam dizini için bkz. [Azure AD B2C için kullanılabilen Microsoft Graph işlemleri](microsoft-graph-operations.md).

<!-- LINK -->

[graph-objectIdentity]: https://docs.microsoft.com/graph/api/resources/objectidentity
[graph-user]: (https://docs.microsoft.com/graph/api/resources/user)
