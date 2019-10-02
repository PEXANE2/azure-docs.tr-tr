---
title: Azure Active Directory B2C Graph API kullanın
description: Azure AD Graph API çağırarak ve işlemi otomatikleştirmek için bir uygulama kimliği kullanarak Azure AD B2C kiracısındaki kullanıcıları yönetme.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2585b47d049047cc191bfc284c4486361917f1ed
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802077"
---
# <a name="azure-ad-b2c-use-the-azure-ad-graph-api"></a>Azure AD B2C: Azure AD Graph API kullanma

Azure Active Directory B2C (Azure AD B2C) kiracıların binlerce veya milyonlarca kullanıcısı olabilir. Bu, birçok ortak Kiracı Yönetimi görevinin programlama yoluyla gerçekleştirilmesi gerektiği anlamına gelir. Birincil örnek, Kullanıcı yönetimi olur.

Mevcut bir Kullanıcı deposunu B2C kiracısına geçirmeniz gerekebilir. Kullanıcı kaydını kendi sayfanızda barındırmak ve arka planda Azure AD B2C dizininizde Kullanıcı hesapları oluşturmak isteyebilirsiniz. Bu tür görevler, Kullanıcı hesapları oluşturma, okuma, güncelleştirme ve silme yeteneği gerektirir. Azure AD Graph API kullanarak bu gibi görevleri gerçekleştirebilirsiniz.

B2C kiracılarında, Graph API ile iletişim kuran iki birincil mod vardır:

* **Etkileşimli**, bir kez çalıştırma görevleri için, GÖREVLERI gerçekleştirirken B2C kiracısında bir yönetici hesabı olarak görev yapmanız gerekir. Bu mod, yöneticinin Graph API hiçbir çağrı gerçekleştirebilmesi için kimlik bilgileriyle oturum açmasını gerektirir.
* **Otomatikleştirilmiş**ve sürekli görevler için, yönetim görevlerini gerçekleştirmek için gerekli ayrıcalıklarla sağladığınız bir tür hizmet hesabını kullanmanız gerekir. Azure AD 'de, uygulamayı kaydederek ve Azure AD 'de kimlik doğrulaması yaparak bunu yapabilirsiniz. Bu, [OAuth 2,0 istemci kimlik bilgileri ver](../active-directory/develop/service-to-service.md)' i kullanan BIR *uygulama kimliği* kullanılarak yapılır. Bu durumda, uygulama, Graph API çağırmak için Kullanıcı olarak değil, kendisi gibi davranır.

Bu makalede, otomatik kullanım durumunun nasıl gerçekleştirileceğini öğreneceksiniz. Kullanıcı oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini gerçekleştiren bir .NET 4,5 `B2CGraphClient` oluşturacaksınız. İstemcinin çeşitli yöntemleri çağırabilmeniz için bir Windows komut satırı arabirimi (CLı) olacaktır. Ancak, kod etkileşimli olmayan, otomatik olarak davranacak şekilde yazılmıştır.

>[!IMPORTANT]
> Bir Azure AD B2C dizinindeki kullanıcıları yönetmek için [Azure AD Graph API](../active-directory/develop/active-directory-graph-api-quickstart.md) kullanmanız **gerekir** . Azure AD Graph API Microsoft Graph API 'sinden farklıdır. Bu MSDN blog gönderisindeki daha fazla bilgi edinin: [Microsoft Graph veya Azure AD grafiği](https://blogs.msdn.microsoft.com/aadgraphteam/2016/07/08/microsoft-graph-or-azure-ad-graph/).

## <a name="prerequisites"></a>Önkoşullar

Uygulama veya Kullanıcı oluşturabilmeniz için önce bir Azure AD B2C kiracının olması gerekir. Henüz bir tane yoksa [Azure Active Directory B2C kiracı oluşturun](tutorial-create-tenant.md).

## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Azure AD B2C kiracınız olduktan sonra, [Azure Portal](https://portal.azure.com)kullanarak yönetim uygulamanızı kaydetmeniz gerekir. Ayrıca, otomatik betik veya yönetim uygulamanız adına yönetim görevlerini gerçekleştirmek için gereken izinleri vermeniz gerekir.

### <a name="register-application-in-azure-active-directory"></a>Uygulamayı Azure Active Directory Kaydet

Azure AD Graph API, B2C kiracınızla birlikte kullanmak için, uygulamayı Azure Active Directory uygulama kaydı iş akışını kullanarak kaydetmeniz gerekir.

[!INCLUDE [active-directory-b2c-appreg-mgmt](../../includes/active-directory-b2c-appreg-mgmt.md)]

### <a name="assign-api-access-permissions"></a>API erişim izinleri atama

[!INCLUDE [active-directory-b2c-permissions-directory](../../includes/active-directory-b2c-permissions-directory.md)]

### <a name="create-client-secret"></a>İstemci parolası oluştur

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

Artık Azure AD B2C kiracınızda Kullanıcı *oluşturma*, *okuma*ve *güncelleştirme* iznine sahip bir uygulamanız var. Kullanıcı *silme* ve *parola güncelleştirme* izinleri eklemek için sonraki bölüme geçin.

## <a name="add-user-delete-and-password-update-permissions"></a>Kullanıcı silme ve parola güncelleştirme izinleri ekleme

Daha önce verdiğiniz *okuma ve yazma dizini verileri* **izni, kullanıcıları** silme veya parolalarını güncelleştirme özelliğini içermez.

Uygulamanıza kullanıcıları silme veya parolaları güncelleştirme yeteneği vermek istiyorsanız, *kullanıcıya Kullanıcı Yöneticisi* rolü vermeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure AD B2C kiracınızı içeren dizine geçin.
1. Sol menüden **Azure AD B2C** ' yi seçin. Ya da **tüm hizmetler** ' i seçip **Azure AD B2C**arayıp seçin.
1. **Yönet**altında **Roller ve yöneticiler**' i seçin.
1. **Kullanıcı Yöneticisi** rolünü seçin.
1. **Atama Ekle**' yi seçin.
1. Metin **Seç** kutusuna daha önce kaydettiğiniz uygulamanın adını girin, örneğin, *managementapp1*. Arama sonuçlarında göründüğünde uygulamanızı seçin.
1. **Add (Ekle)** seçeneğini belirleyin. İzinlerin tam olarak yayılması birkaç dakika sürebilir.

Azure AD B2C uygulamanız artık kullanıcıları silmek veya B2C kiracınızda parolalarını güncelleştirmek için gereken ek izinlere sahiptir.

## <a name="get-the-sample-code"></a>Örnek kodunu alma

Kod örneği, Azure AD Graph API etkileşimde bulunmak için [Active Directory Authentication Library (ADAL)](../active-directory/develop/active-directory-authentication-libraries.md) kullanan bir .NET konsol uygulamasıdır. Kodu, bir Azure AD B2C kiracısındaki kullanıcıları programlı bir şekilde yönetmek için API 'nin nasıl çağrılacağını gösterir.

[Örnek Arşivi](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip) (@no__t -1. zip) Indirebilir veya GitHub deposunu klonlayabilirsiniz:

```cmd
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Kod örneğini aldıktan sonra, ortamınız için yapılandırın ve ardından projeyi derleyin:

1. `B2CGraphClient\B2CGraphClient.sln` çözümünü Visual Studio’da açın.
1. **B2CGraphClient** projesinde *app. config* dosyasını açın.
1. @No__t-0 bölümünü aşağıdaki XML ile değiştirin. Daha sonra `{your-b2c-tenant}` ' ı kiracınızın adı ile ve daha önce kaydettiğiniz değerlerle birlikte `{Application ID}` ve `{Client secret}` olarak değiştirin.

    ```xml
    <appSettings>
        <add key="b2c:Tenant" value="{your-b2c-tenant}.onmicrosoft.com" />
        <add key="b2c:ClientId" value="{Application ID}" />
        <add key="b2c:ClientSecret" value="{Client secret}" />
    </appSettings>
    ```

1. Çözümü derleyin. Çözüm Gezgini **B2CGraphClient** çözümüne sağ tıklayın ve ardından **çözümü yeniden derle**' yi seçin.

Yapı başarılı olursa `B2C.exe` konsol uygulaması `B2CGraphClient\bin\Debug` ' de bulunabilir.

## <a name="review-the-sample-code"></a>Örnek kodu gözden geçirme

B2CGraphClient kullanmak için bir komut Istemi açın (`cmd.exe`) ve projenin `Debug` dizinine değiştirin. Sonra `B2C Help` komutunu çalıştırın.

```cmd
cd B2CGraphClient\bin\Debug
B2C Help
```

@No__t-0 komutu, kullanılabilir alt komutların kısa bir açıklamasını görüntüler. Alt komutlarının birini her çağırdığınızda `B2CGraphClient` Azure AD Graph API bir istek gönderir.

Aşağıdaki bölümlerde, uygulama kodunun Azure AD Graph API çağrıları nasıl yaptığı ele alınmaktadır.

### <a name="get-an-access-token"></a>Bir erişim belirteci alma

Azure AD Graph API yapılan herhangi bir istek, kimlik doğrulaması için bir erişim belirteci gerektirir. `B2CGraphClient`, erişim belirteçleri edinmenize yardımcı olması için açık kaynaklı Active Directory Authentication Library (ADAL) kullanır. ADAL, bir yardımcı API sağlayarak ve erişim belirteçleri önbelleğe alma gibi birkaç önemli ayrıntıyı gerçekleştirerek belirteç alma işlemini kolaylaştırır. Ancak belirteçleri almak için ADAL kullanmanız gerekmez. Bunun yerine, HTTP isteklerini el ile oluşturarak belirteçleri alabilirsiniz.

> [!NOTE]
> Azure AD Graph API ile kullanılabilecek erişim belirteçlerini almak için ADAL v2 veya üstünü kullanmalısınız. ADAL v1 'yi kullanamazsınız.

@No__t-0 yürütüldüğünde, `B2CGraphClient` sınıfının bir örneğini oluşturur. Bu sınıf için Oluşturucu ADAL kimlik doğrulaması yapı iskelesi ayarlıyor:

```csharp
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Örnek olarak `B2C Get-User` komutunu kullanalım.

@No__t-0 ek bağımsız değişkenler olmadan çağrıldığında, uygulama `B2CGraphClient.GetAllUsers()` yöntemini çağırır. `GetAllUsers()`, Azure AD Graph API bir HTTP GET isteği gönderen `B2CGraphClient.SendGraphGetRequest()` ' i çağırır. @No__t-0 ' ı, GET isteğini göndermeden önce, önce ADAL kullanarak bir erişim belirteci edinir:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);
    ...
```

ADAL `AuthenticationContext.AcquireToken()` yöntemini çağırarak Graph API için bir erişim belirteci alabilirsiniz. ADAL daha sonra uygulamanın kimliğini temsil eden `access_token` döndürür.

### <a name="read-users"></a>Kullanıcıları okuyun

Kullanıcıların listesini almak veya Azure AD Graph API belirli bir kullanıcıyı almak istediğinizde, `/users` uç noktasına bir HTTP `GET` isteği gönderebilirsiniz. Bir Kiracıdaki tüm kullanıcılar için bir istek şöyle görünür:

```HTTP
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Bu isteği görmek için şunu çalıştırın:

 ```cmd
 B2C Get-User
 ```

Dikkat etmeniz gereken iki önemli nokta vardır:

* ADAL kullanılarak alınan erişim belirteci `Authorization` üst bilgisine `Bearer` düzeni kullanılarak eklenir.
* B2C kiracılarında,-0 @no__t sorgu parametresini kullanmanız gerekir.

Bu ayrıntıların her ikisi de `B2CGraphClient.SendGraphGetRequest()` yönteminde işlenir:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Tüketici Kullanıcı hesapları oluşturma

B2C kiracınızda Kullanıcı hesapları oluşturduğunuzda, `/users` uç noktasına bir HTTP `POST` isteği gönderebilirsiniz. Aşağıdaki HTTP `POST` isteği, kiracıda oluşturulacak örnek bir Kullanıcı gösterir.

Aşağıdaki istekteki özelliklerin çoğu tüketici kullanıcıları oluşturmak için gereklidir. @No__t-0 açıklamaları çizim için eklenmiştir; bunları gerçek bir istek içine eklemeyin.

```HTTP
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                           // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",            // can be 'emailAddress' or 'userName'
            "value": "consumer@fabrikam.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Consumer User",            // a value that can be used for displaying to the end user
    "mailNickname": "cuser",                   // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false  // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

İsteği görmek için aşağıdaki komutlardan birini çalıştırın:

```cmd
B2C Create-User ..\..\..\usertemplate-email.json
B2C Create-User ..\..\..\usertemplate-username.json
```

@No__t-0 komutu, bir Kullanıcı nesnesinin JSON gösterimini içeren bir JSON dosyası olarak giriş parametresi olarak alır. Kod örneğinde iki örnek JSON dosyası vardır: `usertemplate-email.json` ve `usertemplate-username.json`. Bu dosyaları gereksinimlerinize uyacak şekilde değiştirebilirsiniz. Yukarıdaki gerekli alanlara ek olarak, bazı isteğe bağlı alanlar dosyalara dahil edilir.

Gerekli ve isteğe bağlı alanlar hakkında daha fazla bilgi için bkz. [varlık ve karmaşık tür başvurusu | Graph API başvurusu](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference).

POST isteğinin nasıl oluşturulduğunu `B2CGraphClient.SendGraphPostRequest()` ' da görebilirsiniz:

* İsteğin `Authorization` üstbilgisine bir erişim belirteci iliştirir.
* @No__t-0 olarak ayarlanır.
* İstek gövdesinde JSON Kullanıcı nesnesini içerir.

> [!NOTE]
> Mevcut bir kullanıcı deposundan geçiş yapmak istediğiniz hesapların [Azure AD B2C tarafından zorlanan güçlü parola kuvvetinden](active-directory-b2c-reference-password-complexity.md)daha düşük bir parola gücüne sahip olması durumunda, `passwordPolicies` ' deki `DisableStrongPassword` değerini kullanarak güçlü parola gereksinimini devre dışı bırakabilirsiniz özelliði. Örneğin, önceki Kullanıcı oluşturma isteğini şu şekilde değiştirebilirsiniz: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Tüketici Kullanıcı hesaplarını güncelleştirme

Kullanıcı nesnelerini güncelleştirdiğinizde, işlem Kullanıcı nesneleri oluşturmak için kullandığınız bir şekilde benzerdir, ancak HTTP `PATCH` yöntemini kullanır:

```HTTP
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer"    // this request updates only the user's displayName
}
```

JSON dosyalarınızda bazı değerleri değiştirerek bir kullanıcıyı güncelleştirmeyi deneyin ve ardından şu komutlardan birini çalıştırmak için `B2CGraphClient` kullanın:

```cmd
B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Bu isteğin nasıl gönderileceği hakkındaki ayrıntılar için `B2CGraphClient.SendGraphPatchRequest()` yöntemini inceleyin.

### <a name="search-users"></a>Kullanıcılarda arama

B2C kiracınızdaki kullanıcıları iki şekilde arayabilirsiniz:

* Kullanıcının **nesne kimliğine**başvurun.
* @No__t-0 özelliği olan oturum açma tanımlayıcı tanımlayıcısını başvuru.

Bir kullanıcı aramak için aşağıdaki komutlardan birini çalıştırın:

```cmd
B2C Get-User <user-object-id>
B2C Get-User <filter-query-expression>
```

Örnek:

```cmd
B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27consumer@fabrikam.com%27)
```

### <a name="delete-users"></a>Kullanıcıları Sil

Kullanıcıları silmek için HTTP `DELETE` yöntemini kullanın ve kullanıcının nesne KIMLIĞIYLE birlikte URL 'YI oluşturun:

```HTTP
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Bir örnek görmek için, bu komutu girin ve konsola yazdırılmış silme isteğini görüntüleyin:

```cmd
B2C Delete-User <object-id-of-user>
```

Bu isteğin nasıl gönderileceği hakkındaki ayrıntılar için `B2CGraphClient.SendGraphDeleteRequest()` yöntemini inceleyin.

Kullanıcı yönetimine ek olarak Azure AD Graph API diğer birçok eylemi gerçekleştirebilirsiniz. [Azure AD Graph API başvurusu](/previous-versions/azure/ad/graph/api/api-catalog) , örnek isteklerle birlikte her eyleme ilişkin ayrıntılar sağlar.

## <a name="use-custom-attributes"></a>Özel öznitelikler kullanma

Çoğu tüketici uygulamasının bazı tür özel kullanıcı profili bilgilerini depolaması gerekir. Bunu yapmanın bir yolu, B2C kiracınızda özel bir öznitelik tanımlamaktır. Daha sonra bu özniteliği bir kullanıcı nesnesi üzerinde diğer herhangi bir özelliği işlemiş olduğunuz şekilde değerlendirin. Özniteliği güncelleştirebilir, özniteliğini silebilir, özniteliği ile sorgulayabilir, öznitelik, oturum açma belirteçlerinde talep olarak gönderebilir ve daha fazlasını yapabilirsiniz.

B2C kiracınızda özel bir öznitelik tanımlamak için [B2C özel öznitelik başvurusuna](active-directory-b2c-reference-custom-attr.md)bakın.

Aşağıdaki `B2CGraphClient` komutlarını kullanarak B2C kiracınızda tanımlanan özel öznitelikleri görüntüleyebilirsiniz:

```cmd
B2C Get-B2C-Application
B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Çıktı her özel özniteliğin ayrıntılarını ortaya koyar. Örnek:

```json
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

@No__t-0 gibi tam adı Kullanıcı nesnelerinizin bir özelliği olarak kullanabilirsiniz. JSON Dosyanızı yeni özellik ve özellik için bir değer ile güncelleştirin ve ardından şunu çalıştırın:

```cmd
B2C Update-User <object-id-of-user> <path-to-json-file>
```

## <a name="next-steps"></a>Sonraki adımlar

@No__t-0 ' ı kullanarak, B2C kiracı kullanıcılarınızı programlama yoluyla yönetebileceğini bir hizmet uygulamanız vardır. `B2CGraphClient`, Azure AD Graph API kimlik doğrulamak için kendi uygulama kimliğini kullanır. Ayrıca, bir istemci gizli anahtarı kullanarak belirteçleri alır.

Bu işlevselliği kendi uygulamanıza eklediğinizde B2C uygulamaları için birkaç önemli noktayı unutmayın:

* Uygulamaya, kiracıya gerekli izinleri verin.
* Şimdilik, erişim belirteçleri almak için ADAL (MSAL değil) kullanmanız gerekir. (Bir kitaplık kullanmadan doğrudan protokol iletileri de gönderebilirsiniz.)
* Graph API çağırdığınızda, `api-version=1.6` ' ı kullanın.
* Tüketici kullanıcılarını oluştururken ve güncelleştirdiğinizde, yukarıda açıklandığı gibi birkaç özellik gereklidir.
