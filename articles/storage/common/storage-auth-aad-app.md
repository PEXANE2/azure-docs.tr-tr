---
title: İstemci uygulamasından gelen istekleri yetkilendirmek için Azure AD'den bir belirteç edinin
titleSuffix: Azure Storage
description: Bir istemci uygulamasının içinden kimlik doğrulaması yapmak, bir OAuth 2.0 jetonu edinmek ve talepleri Azure Blob depolama ve Kuyruk depolamasına yetkilendirmek için Azure Etkin Dizini'ni kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: d3ee211298598d78f423d88fd4df1c58ed4bfa29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268489"
---
# <a name="acquire-a-token-from-azure-ad-for-authorizing-requests-from-a-client-application"></a>İstemci uygulamasından gelen istekleri yetkilendirmek için Azure AD'den bir belirteç edinin

Azure Blob depolama alanı veya Sıra depolama alanıyla Azure Active Directory 'i (Azure AD) kullanmanın önemli bir avantajı, kimlik bilgilerinizin artık kodunuzda depolanmasına gerek olmamasıdır. Bunun yerine, Microsoft kimlik platformundan (eski adıyla Azure AD) Bir OAuth 2.0 erişim jetonu isteyebilirsiniz. Azure AD, uygulamayı çalıştıran güvenlik ilkesinin (kullanıcı, grup veya hizmet sorumlusu) kimliğini doğrular. Kimlik doğrulama başarılı olursa, Azure AD erişim belirtecisini uygulamaya döndürür ve uygulama daha sonra istekleri Azure Blob depolama alanına veya Sıra depolamaalanına yetkilendirmek için erişim belirteci'ni kullanabilir.

Bu makalede, Microsoft kimlik platformu 2.0 ile kimlik doğrulama için yerel uygulama nızı veya web uygulamanızı nasıl yapılandırılacanız gösterilmektedir. Kod örneği .NET özelliğine sahiptir, ancak diğer diller de benzer bir yaklaşım kullanır. Microsoft kimlik platformu 2.0 hakkında daha fazla bilgi için [Microsoft kimlik platformuna (v2.0) genel bakış](../../active-directory/develop/v2-overview.md)bakın.

OAuth 2.0 kod hibe akışına genel bir bakış için, [OAuth 2.0 kod hibe akışını kullanarak Azure Active Directory web uygulamalarına erişimi yetkilendirme](../../active-directory/develop/v2-oauth2-auth-code-flow.md)bölümüne bakın.

## <a name="assign-a-role-to-an-azure-ad-security-principal"></a>Azure REKLAM güvenlik ilkesine rol atama

Azure Depolama uygulamanızdan bir güvenlik ilkesinin kimliğini doğrulamak için, önce bu güvenlik ilkesi için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure Depolama, kapsayıcılar ve kuyruklar için izinleri kapsayan yerleşik RBAC rollerini tanımlar. RBAC rolü bir güvenlik ilkesine atandığında, bu güvenlik ilkesine bu kaynağa erişim izni verilir. Daha fazla bilgi için [bkz.](storage-auth-aad-rbac.md)

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Uygulamanızı azure AD kiracısıyla kaydedin

Depolama kaynaklarına erişimi yetkilendirmek için Azure AD'yi kullanmanın ilk adımı, istemci uygulamanızı [Azure portalından](https://portal.azure.com)bir Azure AD kiracısına kaydetmektir. İstemci uygulamanızı kaydettirdiğinizde, uygulama yla ilgili bilgileri Azure AD'ye savurabilirsiniz. Azure AD, uygulamanızı çalışma zamanında Azure AD ile ilişkilendirmek için kullandığınız bir istemci kimliği *(uygulama kimliği*olarak da adlandırılır) sağlar. İstemci kimliği hakkında daha fazla bilgi edinmek için [Azure Etkin Dizini'ndeki Uygulama ve hizmet temel nesnelerine](../../active-directory/develop/app-objects-and-service-principals.md)bakın.

Azure Depolama uygulamanızı kaydetmek için [Quickstart: Microsoft kimlik platformuna bir uygulamayı](../../active-directory/develop/quickstart-configure-app-access-web-apis.md)kaydedin' de gösterilen adımları izleyin. Aşağıdaki resim, bir web uygulamasını kaydetmek için ortak ayarları gösterir:

![Depolama uygulamanızın Azure AD ile nasıl kaydedilenini gösteren ekran görüntüsü](./media/storage-auth-aad-app/app-registration.png)

> [!NOTE]
> Başvurunuzu yerel bir uygulama olarak kaydettirürseniz, **Yönlendirme URI**için geçerli bir URI belirtebilirsiniz. Yerel uygulamalar için bu değerin gerçek bir URL olması gerekmez. Web uygulamaları için, yeniden yönlendirme URI geçerli bir URI olmalıdır, çünkü belirteçleri sağlanan URL belirtir.

Başvurunuzu kaydettikten sonra **Ayarlar**altında uygulama kimliğini (veya istemci kimliğini) görürsünüz:

![İstemci kimliğini gösteren ekran görüntüsü](./media/storage-auth-aad-app/app-registration-client-id.png)

Bir uygulamayı Azure AD'ye kaydetme hakkında daha fazla bilgi [için](../../active-directory/develop/quickstart-v2-register-an-app.md)bkz.

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Kayıtlı uygulama izinlerinizi Azure Depolama'ya verme

Ardından, Uygulamanıza Azure Depolama API'lerini arama izni ver. Bu adım, uygulamanızın Azure AD ile Azure Depolama'ya istekleri yetkilendirmesini sağlar.

1. Kayıtlı uygulamanız için **Genel Bakış** **sayfasında, API İzinlerini Görüntüle'yi**seçin.
1. **API izinleri** bölümünde, **İzin ekle'yi** seçin ve **Microsoft API'lerini**seçin.
1. **İstek API izinleri** bölmesini görüntülemek için sonuçlar listesinden **Azure Depolama'yı** seçin.
1. **Uygulamanız ne tür izinler gerektirir?** **Delegated permissions** Bu seçenek varsayılan olarak sizin için seçilir.
1. **İstek API izinleri** bölmesinin **İzinleri Seç** bölümünde, **user_impersonation**yanındaki onay kutusunu seçin, ardından **İzin Ekle'yi**tıklatın.

    ![Depolama için izinleri gösteren ekran görüntüsü](media/storage-auth-aad-app/registered-app-permissions-1.png)

**API izinleri** bölmesi artık kayıtlı Azure AD uygulamanızın hem Microsoft Graph'a hem de Azure Depolama alanına erişimi olduğunu gösterir. Uygulamanızı Azure AD'ye ilk kaydettiğinizde izinler Otomatik olarak Microsoft Graph'a verilir.

![Kayıt uygulaması izinlerini gösteren ekran görüntüsü](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="create-a-client-secret"></a>İstemci sırrı oluşturma

Uygulama, bir belirteç talep ederken kimliğini kanıtlamak için bir istemci sırrı gerekir. İstemci sırrını eklemek için aşağıdaki adımları izleyin:

1. Azure portalındaki uygulama kaydınıza gidin.
1. **Sertifikalar & sırları** ayarını seçin.
1. **İstemci sırları**altında, yeni bir sır oluşturmak için **Yeni istemci gizli** tıklatın.
1. Gizli için bir açıklama sağlayın ve istenen son kullanma aralığını seçin.
1. Yeni sırrın değerini hemen güvenli bir konuma kopyalayın. Tam değer size yalnızca bir kez görüntülenir.

    ![İstemci sırrını gösteren ekran görüntüsü](media/storage-auth-aad-app/client-secret.png)

## <a name="client-libraries-for-token-acquisition"></a>Belirteç edinimi için istemci kitaplıkları

Uygulamanızı kaydettikten ve Azure Blob depolama veya Sıra depolama alanında verilere erişme izni verdikten sonra, bir güvenlik ilkesinin kimliğini doğrulamak ve bir OAuth 2.0 belirteci edinmek için uygulamanıza kod ekleyebilirsiniz. Belirteci doğrulamak ve elde etmek için, Microsoft [kimlik doğrulama kitaplıklarından](../../active-directory/develop/reference-v2-libraries.md) birini veya OpenID Connect 1.0'ı destekleyen başka bir açık kaynak kitaplığını kullanabilirsiniz. Uygulamanız daha sonra, Azure Blob depolama alanına veya Sıra depolamaalanına karşı bir isteği yetkilendirmek için erişim jetonunu kullanabilir.

Belirteçleri edinmenin desteklendiği senaryoların listesi için, Microsoft Kimlik Doğrulama [Kitaplığı içeriğinin](/azure/active-directory/develop/msal-overview) [kimlik doğrulama akışları](/en-us/azure/active-directory/develop/msal-authentication-flows) bölümüne bakın.

## <a name="well-known-values-for-authentication-with-azure-ad"></a>Azure AD ile kimlik doğrulama için iyi bilinen değerler

Azure AD ile bir güvenlik ilkesinin kimliğini doğrulamak için kodunuza bazı tanınmış değerler eklemeniz gerekir.

### <a name="azure-ad-authority"></a>Azure AD yetkilisi

Microsoft genel bulutu için, *kiracı kimliğinin* Etkin Dizin kiracı kimliğiniz (veya dizin kimliğiniz) olduğu temel Azure REKLAM yetkilisi aşağıdaki gibidir:

`https://login.microsoftonline.com/<tenant-id>/`

Kiracı kimliği, kimlik doğrulaması için kullanılacak Azure AD kiracısını tanımlar. Dizin kimliği olarak da adlandırılır. Kiracı kimliğini almak için Azure portalındaki uygulama kaydınız için **Genel Bakış** sayfasına gidin ve değeri buradan kopyalayın.

### <a name="azure-storage-resource-id"></a>Azure Depolama kaynak kimliği

[!INCLUDE [storage-resource-id-include](../../../includes/storage-resource-id-include.md)]

## <a name="net-code-example-create-a-block-blob"></a>.NET kodu örneği: Bir blok blob oluşturma

Kod örneği, Azure AD'den nasıl erişim jetonu alınabildiğini gösterir. Erişim belirteci, belirtilen kullanıcının kimliğini doğrulamak ve ardından bir blok blob oluşturmak için bir istek yetkilendirmek için kullanılır. Bu örneğin çalışmasını sağlamak için, önce önceki bölümlerde belirtilen adımları izleyin.

Belirteci talep etmek için uygulamanızın kaydından aşağıdaki değerlere ihtiyacınız olacaktır:

- Azure AD etki alanınızın adı. Azure Etkin Dizininizdeki **Genel Bakış** sayfasından bu değeri alın.
- Kiracı (veya dizin) kimliği. Bu değeri uygulama kaydınızın **Genel Bakış** sayfasından alın.
- İstemci (veya uygulama) kimliği. Bu değeri uygulama kaydınızın **Genel Bakış** sayfasından alın.
- İstemci URI'yi yeniden yönlendirir. Bu değeri uygulama kaydınız için **Kimlik Doğrulama** ayarlarından alın.
- Müşteri sırrının değeri. Bu değeri daha önce kopyaladığınız konumdan alın.

### <a name="create-a-storage-account-and-container"></a>Depolama hesabı ve kapsayıcı oluşturma

Kod örneğini çalıştırmak için, Azure Etkin Dizini'nizle aynı abonelik içinde bir depolama hesabı oluşturun. Ardından, bu depolama hesabı içinde bir kapsayıcı oluşturun. Örnek kodu bu kapsayıcıda bir blok blob oluşturur.

Ardından, örnek kodu çalıştıracağınız kullanıcı hesabına **Depolama Blob Veri Katılımcısı** rolünü açıkça atayın. Bu rolü Azure portalına nasıl atayacağına ilişkin talimatlar için, [Azure portalında RBAC ile Azure blob ve kuyruk verilerine Erişim Izni'ne](storage-auth-aad-rbac-portal.md)bakın.

> [!NOTE]
> Bir Azure Depolama hesabı oluşturduğunuzda, Azure AD üzerinden verilere erişmek için otomatik olarak izin atanırsınız. Azure Depolama için kendinize açıkça bir RBAC rolü atamalısınız. Aboneliğiniz, kaynak grubunuz, depolama hesabınız veya kapsayıcınız veya kuyruğuz düzeyinde atayabilirsiniz.

### <a name="create-a-web-application-that-authorizes-access-to-blob-storage-with-azure-ad"></a>Azure AD ile Blob depolama alanına erişimyetkisi veren bir web uygulaması oluşturun

Uygulamanız Azure Depolama'ya eriştığında, bunu kullanıcı adına yapar, bu da oturum açan kullanıcının izinlerini kullanarak blob veya kuyruk kaynaklarına erişilen anlamına gelir. Bu kod örneğini denemek için, kullanıcının Azure AD kimliğini kullanarak oturum açmasını gerektiren bir web uygulamasına ihtiyacınız vardır. Kendi uygulamanızı oluşturabilir veya Microsoft tarafından sağlanan örnek uygulamayı kullanabilirsiniz.

Bir belirteç edinen ve Azure Depolama'da bir leke oluşturmak için kullanan tamamlanmış bir örnek web uygulaması [GitHub'da](https://aka.ms/aadstorage)kullanılabilir. Tamamlanan örneği gözden geçirmek ve çalıştırmak kod örneklerini anlamak için yararlı olabilir. Tamamlanan örneğin nasıl çalıştırılaacağına ilişkin talimatlar için Görünüm başlıklı bölüme bakın [ve tamamlanan örneği çalıştırın.](#view-and-run-the-completed-sample)

#### <a name="add-references-and-using-statements"></a>Referans ekleme ve deyim kullanma  

Visual Studio'dan Azure Depolama istemci kitaplığını yükleyin. **Araçlar** menüsünden **NuGet Paket Yöneticisi**’ni ve ardından **Paket Yöneticisi Konsolu**’nu seçin. .NET için Azure Depolama istemci kitaplığından gerekli paketleri yüklemek için konsol penceresine aşağıdaki komutları yazın:

```console
Install-Package Microsoft.Azure.Storage.Blob
Install-Package Microsoft.Azure.Storage.Common
```

Ardından, HomeController.cs dosyasına aşağıdaki ifadeleri kullanarak ekleyin:

```csharp
using Microsoft.Identity.Client; //MSAL library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

#### <a name="create-a-block-blob"></a>Blok blob oluşturma

Bir blok blob oluşturmak için aşağıdaki kod snippet ekleyin:

```csharp
private static async Task<string> CreateBlob(string accessToken)
{
    // Create a blob on behalf of the user
    TokenCredential tokenCredential = new TokenCredential(accessToken);
    StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

    // Replace the URL below with your storage account URL
    CloudBlockBlob blob =
        new CloudBlockBlob(
            new Uri("https://<storage-account>.blob.core.windows.net/<container>/Blob1.txt"),
            storageCredentials);
    await blob.UploadTextAsync("Blob created by Azure AD authenticated user.");
    return "Blob successfully created";
}
```

> [!NOTE]
> OAuth 2.0 belirteci ile blob ve kuyruk işlemlerini yetkilendirmek için HTTPS kullanmanız gerekir.

Yukarıdaki örnekte, .NET istemci kitaplığı blok blob oluşturmak için istek yetkilendirme işler. Diğer diller için Azure Depolama istemci kitaplıkları da sizin için isteğin yetkilendirmesini işler. Ancak, REST API'sini kullanarak OAuth belirteci içeren bir Azure Depolama işlemini çağırıyorsanız, o oAuth belirteci kullanarak isteği yetkilendirmeniz gerekir.

OAuth erişim belirteçlerini kullanarak Blob ve Queue hizmet işlemlerini aramak **için, Taşıyıcı** düzenini kullanarak **Yetkilendirme** üstbilgisinde erişim belirteci geçmek ve aşağıdaki örnekte gösterildiği gibi 2017-11-09 veya daha yüksek bir hizmet sürümünü belirtin:

```https
GET /container/file.txt HTTP/1.1
Host: mystorageaccount.blob.core.windows.net
x-ms-version: 2017-11-09
Authorization: Bearer eyJ0eXAiOnJKV1...Xd6j
```

#### <a name="get-an-oauth-token-from-azure-ad"></a>Azure AD'den Bir OAuth jetonu alın

Ardından, kullanıcı adına Azure AD'den bir belirteç isteyen bir yöntem ekleyin. Bu yöntem, izinlerin verilebilmek için hangi kapsamı tanımladığını tanımlar. İzinler ve kapsamlar hakkında daha fazla bilgi için Microsoft [kimlik platformu bitiş noktasında İzinler ve onay bilgisine](../../active-directory/develop/v2-permissions-and-consent.md)bakın.

Belirteci elde etmek için kapsamı oluşturmak için kaynak kimliğini kullanın. Örnek, kaynak kimliğini yerleşik `user_impersonation` kapsamla birlikte kullanarak kapsamı oluşturuyor ve bu da belirteçkullanıcı adına istendiğini gösterir.

Kullanıcıya, kullanıcının kendi adına belirteç lerini istemeye onay göstermesini sağlayan bir arabirim sunmanız gerekebileceğini unutmayın. Onay gerektiğinde, örnek **MsalUiRequiredException** yakalar ve onay isteğini kolaylaştırmak için başka bir yöntem çağırır:

```csharp
public async Task<IActionResult> Blob()
{
    var scopes = new string[] { "https://storage.azure.com/user_impersonation" };
    try
    {
        var accessToken =
            await _tokenAcquisition.GetAccessTokenOnBehalfOfUser(HttpContext, scopes);
        ViewData["Message"] = await CreateBlob(accessToken);
        return View();
    }
    catch (MsalUiRequiredException ex)
    {
        AuthenticationProperties properties =
            BuildAuthenticationPropertiesForIncrementalConsent(scopes, ex);
        return Challenge(properties);
    }
}
```

İzin, kullanıcının bir uygulamaya kendi adlarına korunan kaynaklara erişmek için yetki verme işlemidir. Microsoft kimlik platformu 2.0, bir güvenlik sorumlusunun başlangıçta en az izin kümesini isteyebileceği ve gerektiğinde zaman içinde izinler ekleyebileceği anlamına gelen artımlı onayı destekler. Kodunuz bir erişim jetonu istediğinde, parametreye göre uygulamanızın herhangi bir `scope` zamanda ihtiyaç duyduğu izinlerin kapsamını belirtin. Artımlı onay hakkında daha fazla bilgi için, [Neden Microsoft kimlik platformuna (v2.0) güncelleştirin](../../active-directory/azuread-dev/azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent)bölümünde **Artımlı ve dinamik onay** başlıklı bölüme bakın?

Aşağıdaki yöntem, artımlı onay istemek için kimlik doğrulama özelliklerini içerir:

```csharp
private AuthenticationProperties BuildAuthenticationPropertiesForIncrementalConsent(string[] scopes,
                                                                                    MsalUiRequiredException ex)
{
    AuthenticationProperties properties = new AuthenticationProperties();

    // Set the scopes, including the scopes that ADAL.NET or MSAL.NET need for the Token cache.
    string[] additionalBuildInScopes = new string[] { "openid", "offline_access", "profile" };
    properties.SetParameter<ICollection<string>>(OpenIdConnectParameterNames.Scope,
                                                 scopes.Union(additionalBuildInScopes).ToList());

    // Attempt to set the login_hint so that the logged-in user is not presented
    // with an account selection dialog.
    string loginHint = HttpContext.User.GetLoginHint();
    if (!string.IsNullOrWhiteSpace(loginHint))
    {
        properties.SetParameter<string>(OpenIdConnectParameterNames.LoginHint, loginHint);

        string domainHint = HttpContext.User.GetDomainHint();
        properties.SetParameter<string>(OpenIdConnectParameterNames.DomainHint, domainHint);
    }

    // Specify any additional claims that are required (for instance, MFA).
    if (!string.IsNullOrEmpty(ex.Claims))
    {
        properties.Items.Add("claims", ex.Claims);
    }

    return properties;
}
```

## <a name="view-and-run-the-completed-sample"></a>Tamamlanan örneği görüntüleme ve çalıştırma

Örnek uygulamayı çalıştırmak için, ilk klon veya [GitHub](https://github.com/Azure-Samples/storage-dotnet-azure-ad-msal)indirin. Ardından aşağıdaki bölümlerde açıklandığı şekilde uygulamayı güncelleştirin.

### <a name="provide-values-in-the-settings-file"></a>Ayarlar dosyasında değerler sağlama

Ardından, *appsettings.json* dosyasını kendi değerlerinizle güncelleyin:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "<azure-ad-domain-name>.onmicrosoft.com",
    "TenantId": "<tenant-id>",
    "ClientId": "<client-id>",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc",

    // To call an API
    "ClientSecret": "<client-secret>"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="update-the-storage-account-and-container-name"></a>Depolama hesabı ve kapsayıcı adını güncelleştirme

*HomeController.cs* dosyasında, depolama hesabınızın ve kapsayıcınızın adını kullanmak için blok blob'una başvuran URI'yi güncelleştirin:

```csharp
CloudBlockBlob blob = new CloudBlockBlob(
                      new Uri("https://<storage-account>.blob.core.windows.net/<container>/Blob1.txt"),
                      storageCredentials);
```

### <a name="enable-implicit-grant-flow"></a>Örtük hibe akışını etkinleştirme

Örneği çalıştırmak için, uygulama kaydınız için örtülü hibe akışını yapılandırmanız gerekebilir. Şu adımları uygulayın:

1. Azure portalındaki uygulama kaydınıza gidin.
1. Yönet bölümünde **Kimlik Doğrulama** ayarını seçin.
1. **Gelişmiş ayarlar**altında, **Örtülü hibe** bölümünde, aşağıdaki resimde gösterildiği gibi erişim belirteçleri ve kimlik belirteçleri etkinleştirmek için onay kutularını seçin:

    ![Örtülü hibe akışı ayarlarını nasıl etkinleştireceklerini gösteren ekran görüntüsü](media/storage-auth-aad-app/enable-implicit-grant-flow.png)

### <a name="update-the-port-used-by-localhost"></a>Localhost tarafından kullanılan bağlantı noktasını güncelleştirme

Örneği çalıştırdığınızda, çalışma zamanında atanan *yerel ana bilgisayar* bağlantı noktasını kullanmak için uygulama kaydınızda belirtilen yeniden yönlendirme URI'yi güncelleştirmeniz gerektiğini görebilirsiniz. Atanan bağlantı noktasını kullanmak için URI yönlendirmesini güncelleştirmek için aşağıdaki adımları izleyin:

1. Azure portalındaki uygulama kaydınıza gidin.
1. Yönet bölümünde **Kimlik Doğrulama** ayarını seçin.
1. **Yönlendirme URI'leri**altında, aşağıdaki resimde gösterildiği gibi, örnek uygulama tarafından kullanılan bağlantı noktasını eşleşecek şekilde düzenleme:

    ![Uygulama kaydı için yeniden yönlendirme ÜR'leri'ni gösteren ekran görüntüsü](media/storage-auth-aad-app/redirect-uri.png)

## <a name="next-steps"></a>Sonraki adımlar

- Microsoft kimlik platformu hakkında daha fazla bilgi edinmek için [Microsoft kimlik platformuna](https://docs.microsoft.com/azure/active-directory/develop/)bakın.
- Azure depolama için RBAC rolleri hakkında daha fazla bilgi edinmek için [bkz.](storage-auth-aad-rbac.md)
- Azure Depolama ile Azure kaynakları için yönetilen kimlikleri kullanma hakkında bilgi edinmek için Azure [Etkin Dizini ve Azure Kaynakları için yönetilen kimliklerle blob'lara ve kuyruklara kimlik doğrulama](storage-auth-aad-msi.md)erişimi ne bakın.
