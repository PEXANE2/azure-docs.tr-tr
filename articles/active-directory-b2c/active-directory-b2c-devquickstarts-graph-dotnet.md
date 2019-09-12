---
title: Azure Active Directory B2C içinde Graph API kullanın | Microsoft Docs
description: İşlemi otomatikleştirmek için bir uygulama kimliği kullanarak B2C kiracısı için Graph API çağırma.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/07/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 11a9fc521a7b17ae0ff2f579f173f4d43383bdd5
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70880097"
---
# <a name="azure-ad-b2c-use-the-azure-ad-graph-api"></a>Azure AD B2C: Azure AD Graph API’sini kullanma

>[!NOTE]
> Bir Azure AD B2C dizinindeki kullanıcıları yönetmek için [Azure AD Graph API](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-operations-overview) kullanmanız gerekir. Bu, Microsoft Graph API 'sinden farklıdır. [Burada](https://blogs.msdn.microsoft.com/aadgraphteam/2016/07/08/microsoft-graph-or-azure-ad-graph/) daha fazla bilgi edinin.

Azure Active Directory (Azure AD) B2C kiracılarının çok büyük olması eğilimi gösterir. Bu, birçok ortak Kiracı Yönetimi görevinin programlama yoluyla gerçekleştirilmesi gerektiği anlamına gelir. Birincil örnek, Kullanıcı yönetimi olur. Mevcut bir Kullanıcı deposunu B2C kiracısına geçirmeniz gerekebilir. Kullanıcı kaydını kendi sayfanızda barındırmak ve arka planda Azure AD B2C dizininizde Kullanıcı hesapları oluşturmak isteyebilirsiniz. Bu tür görevler, Kullanıcı hesapları oluşturma, okuma, güncelleştirme ve silme yeteneği gerektirir. Azure AD Graph API kullanarak bu görevleri gerçekleştirebilirsiniz.

B2C kiracılarında, Graph API ile iletişim kuran iki birincil mod vardır.

* Etkileşimli, bir kez çalıştırma görevleri için, görevleri gerçekleştirirken B2C kiracısında bir yönetici hesabı olarak görev yapmanız gerekir. Bu mod, yöneticinin Graph API hiçbir çağrı gerçekleştirebilmesi için kimlik bilgileriyle oturum açmasını gerektirir.
* Otomatikleştirilmiş ve sürekli görevler için, yönetim görevlerini gerçekleştirmek için gerekli ayrıcalıklarla sağladığınız bir tür hizmet hesabını kullanmanız gerekir. Azure AD 'de, uygulamayı kaydederek ve Azure AD 'de kimlik doğrulaması yaparak bunu yapabilirsiniz. Bu, [OAuth 2,0 istemci kimlik bilgileri ver](../active-directory/develop/service-to-service.md)' i kullanan BIR **uygulama kimliği** kullanılarak yapılır. Bu durumda, uygulama, Graph API çağırmak için Kullanıcı olarak değil, kendisi gibi davranır.

Bu makalede otomatik kullanım durumunu nasıl gerçekleştireceğinizi öğreneceksiniz. Kullanıcı oluşturma, okuma, güncelleştirme `B2CGraphClient` ve silme (CRUD) işlemlerini gerçekleştiren bir .NET 4,5 oluşturacaksınız. İstemcinin çeşitli yöntemleri çağırabilmeniz için bir Windows komut satırı arabirimi (CLı) olacaktır. Ancak, kod, etkileşimsiz, otomatik olarak davranacak şekilde yazılmıştır.

## <a name="get-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracı al
Uygulama veya Kullanıcı oluşturabilmeniz için önce bir Azure AD B2C kiracının olması gerekir. Henüz bir kiracınız yoksa [Azure AD B2C kullanmaya başlayın](active-directory-b2c-get-started.md).

## <a name="register-your-application-in-your-tenant"></a>Uygulamanızı kiracınıza kaydedin
B2C kiracınız olduktan sonra, [Azure Portal](https://portal.azure.com)kullanarak uygulamanızı kaydetmeniz gerekir.

> [!IMPORTANT]
> B2C kiracınızla Graph API kullanmak için, Azure AD B2C's *uygulamaları* menüsünde **değil** Azure Portal uygulama *kayıtları* hizmetini kullanarak bir uygulamayı kaydetmeniz gerekir. Aşağıdaki yönergeler size uygun menüye yol açabilir. Azure AD B2C's *uygulamaları* menüsünde KAYDETTIĞINIZ mevcut B2C uygulamalarını yeniden kullanamazsınız.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sayfanın sağ üst köşesindeki hesabınızı seçerek Azure AD B2C kiracınızı seçin.
3. Sol taraftaki Gezinti bölmesinde **tüm hizmetler**' i seçin, **uygulama kayıtları**' nı ve ardından **Yeni kayıt**' ı tıklatın.
4. Komut istemlerini izleyin ve yeni bir uygulama oluşturun. 
    1. Uygun bir ad ekleyin
    2. **Yalnızca bu kuruluş dizinindeki hesapları** Seç
    3. Uygulama türü olarak **Web** ' i seçin ve bu örneğe uygun olmadığından **herhangi bir oturum açma URL 'si** (ör. `https://B2CGraphAPI`) sağlayın.  
    4. Kaydol ' a tıklayın.
5. Uygulama artık uygulama listesinde görünür, **uygulama kimliğini** (istemci kimliği olarak da bilinir) almak için üzerine tıklayın. Daha sonraki bir bölümde ihtiyacınız olacak şekilde kopyalayın.
6. Ayarlar menüsünde **sertifikalar & parolaları**' na tıklayın.
7. **İstemci** gizli dizileri bölümünde **yeni istemci parolası**' na tıklayın, gizli dizi için bir açıklama girin ve bir süre seçin ve ardından **Ekle**' ye tıklayın. Daha sonraki bir bölümde kullanmak üzere gizli anahtar değerini (Istemci gizli anahtarı olarak da bilinir) kopyalayın.

## <a name="configure-create-read-and-update-permissions-for-your-application"></a>Uygulamanız için oluşturma, okuma ve güncelleştirme izinlerini yapılandırma
Şimdi, Kullanıcı oluşturma, okuma, güncelleştirme ve silme için gereken tüm izinleri almak üzere uygulamanızı yapılandırmanız gerekir.

1. Azure portal uygulama kayıtları menüsünde devam etmek için uygulamanızı seçin.
2. Ayarlar menüsünde, **gerekli izinler**' e tıklayın.
3. Gerekli izinler menüsünde **Windows Azure Active Directory**' ye tıklayın.
4. Erişimi etkinleştir menüsünde, **uygulama izinlerinden** **Dizin verilerini oku ve yaz** iznini seçin ve **Kaydet**' e tıklayın.
5. Son olarak, gerekli izinler menüsüne geri dönün, **Izin ver** düğmesine tıklayın.

Artık B2C kiracınızdan Kullanıcı oluşturma, okuma ve güncelleştirme iznine sahip bir uygulamanız var.

> [!NOTE]
> İzinlerin verilmesi, tam olarak işlenmesi birkaç dakika sürebilir.
> 
> 

## <a name="configure-delete-or-update-password-permissions-for-your-application"></a>Uygulamanız için silme veya parola güncelleştirme izinlerini yapılandırma
Şu anda, *Dizin verilerini okuma ve yazma* izni, kullanıcıları silme veya Kullanıcı parolalarını güncelleştirme **özelliğini içermez.** Uygulamanıza Kullanıcı silme veya parolaları güncelleştirme yeteneği vermek istiyorsanız, PowerShell içeren bu ek adımları yapmanız gerekir, aksi takdirde sonraki bölüme atlayabilirsiniz.

İlk olarak, henüz yüklemediyseniz, [Azure AD PowerShell v1 modülünü (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0)yükleyebilirsiniz:

```powershell
Install-Module MSOnline
```

PowerShell modülünü yükledikten sonra Azure AD B2C kiracınıza bağlanın.

> [!IMPORTANT]
> B2C kiracısında **Yerel** olan bir B2C kiracı yönetici hesabı kullanmanız gerekir. Bu hesaplar şöyle görünür: myusername@myb2ctenant.onmicrosoft.com.

```powershell
Connect-MsolService
```

Şimdi, uygulamayı kullanıcı hesabı yönetici rolü atamak için aşağıdaki komut dosyasındaki **uygulama kimliğini** kullanacağız. Bu roller iyi bilinen tanımlayıcılara sahiptir, bu nedenle tüm yapmanız gereken **uygulama kimliğinizi** aşağıdaki betikte girmelidir.

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

Uygulamanız artık Kullanıcı silme veya B2C kiracınızdan parolaları güncelleştirme izinlerine de sahiptir.

## <a name="download-configure-and-build-the-sample-code"></a>Örnek kodu indirme, yapılandırma ve derleme
İlk olarak, örnek kodu indirin ve çalışmaya başlayın. Daha sonra bu duruma daha yakından bakacağız.  [Örnek kodu bir. zip dosyası olarak indirebilirsiniz](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Ayrıca, bunu istediğiniz dizine kopyalayabilirsiniz:

```cmd
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Visual Studio 'da Visual Studio çözümünü açın. `B2CGraphClient\B2CGraphClient.sln` `B2CGraphClient` Projede dosyasını`App.config`açın. Üç uygulama ayarını kendi değerlerinizle değiştirin:

```xml
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Ardından, `B2CGraphClient` çözüme sağ tıklayın ve örneği yeniden derleyin. Başarılı olursanız, artık içinde `B2C.exe` `B2CGraphClient\bin\Debug`bulunan bir yürütülebilir dosyanız olmalıdır.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Graph API kullanarak Kullanıcı CRUD işlemleri oluşturma
B2CGraphClient kullanmak için bir `cmd` Windows komut istemi açın ve dizininizi `Debug` dizinle değiştirin. Sonra `B2C Help` komutunu çalıştırın.

```cmd
cd B2CGraphClient\bin\Debug
B2C Help
```

Bu, her komutun kısa bir açıklamasını görüntüler. Bu komutlardan `B2CGraphClient` birini her çağırdığınızda Azure AD Graph API bir istek yapar.

### <a name="get-an-access-token"></a>Bir erişim belirteci alma
Graph API herhangi bir istek, kimlik doğrulaması için bir erişim belirteci gerektirir. `B2CGraphClient`, erişim belirteçleri edinmeye yardımcı olması için açık kaynaklı Active Directory Authentication Library (ADAL) kullanır. ADAL, basit bir API sağlayarak ve erişim belirteçlerini önbelleğe alma gibi bazı önemli ayrıntılarla ilgilenerek belirteç alma işlemini kolaylaştırır. Ancak belirteçleri almak için ADAL kullanmanız gerekmez. Ayrıca, HTTP isteklerini yenerek belirteç alabilirsiniz.

> [!NOTE]
> Bu kod örneği, Graph API ile iletişim kurmak için ADAL v2 kullanır.  Azure AD Graph API ile kullanılabilecek erişim belirteçleri almak için ADAL v2 veya v3 kullanmanız gerekir.
> 
> 

Çalıştırıldığında, `B2CGraphClient` sınıfının bir örneğini oluşturur. `B2CGraphClient` Bu sınıfın Oluşturucusu bir ADAL kimlik doğrulaması yapı iskelesi ayarlıyor:

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

Bir örnek olarak komutunu kullanacağız. `B2C Get-User` Ek bir giriş olmadan çağrıldığında, CLI `B2CGraphClient.GetAllUsers(...)` yöntemini çağırır. `B2C Get-User` Bu yöntem, `B2CGraphClient.SendGraphGetRequest(...)`Graph API bir http get isteği gönderen çağırır. Get `B2CGraphClient.SendGraphGetRequest(...)` isteğini göndermeden önce, önce adal kullanarak bir erişim belirteci alır:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

ADAL `AuthenticationContext.AcquireToken(...)` yöntemini çağırarak Graph API için bir erişim belirteci alabilirsiniz. ADAL daha sonra uygulamanın `access_token` kimliğini temsil eden bir döndürür.

### <a name="read-users"></a>Kullanıcıları okuyun
Kullanıcıların listesini almak veya Graph API belirli bir kullanıcıyı almak istediğinizde, `GET` `/users` uç noktaya bir http isteği gönderebilirsiniz. Bir Kiracıdaki tüm kullanıcılar için bir istek şöyle görünür:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Bu isteği görmek için şunu çalıştırın:

 ```cmd
 B2C Get-User
 ```

Dikkat etmeniz gereken iki önemli nokta vardır:

* ADAL aracılığıyla alınan erişim belirteci, `Authorization` `Bearer` şema kullanılarak üstbilgiye eklenir.
* B2C kiracılarında sorgu parametresini `api-version=1.6`kullanmanız gerekir.

Bu ayrıntıların her ikisi de `B2CGraphClient.SendGraphGetRequest(...)` yönteminde işlenir:

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
B2C kiracınızda Kullanıcı hesapları oluşturduğunuzda, `POST` `/users` uç noktaya bir http isteği gönderebilirsiniz:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying to the end user
    "mailNickname": "joec",                        // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Bu istekteki özelliklerin çoğu tüketici kullanıcıları oluşturmak için gereklidir. Daha fazla bilgi edinmek için [buraya](/previous-versions/azure/ad/graph/api/users-operations#CreateLocalAccountUser)tıklayın. `//` Açıklama için yorumların dahil edildiğini unutmayın. Bunları gerçek bir istek içine eklemeyin.

İsteği görmek için aşağıdaki komutlardan birini çalıştırın:

```cmd
B2C Create-User ..\..\..\usertemplate-email.json
B2C Create-User ..\..\..\usertemplate-username.json
```

`Create-User` Komut bir. json dosyasını giriş parametresi olarak alır. Bu, bir Kullanıcı nesnesinin JSON gösterimini içerir. Örnek kodda iki Sample. JSON dosyası vardır: `usertemplate-email.json` ve. `usertemplate-username.json` Bu dosyaları gereksinimlerinize uyacak şekilde değiştirebilirsiniz. Yukarıdaki gerekli alanlara ek olarak, kullanabileceğiniz bazı isteğe bağlı alanlar bu dosyalara dahil edilir. İsteğe bağlı alanlarla ilgili ayrıntılar [Azure AD Graph API varlık başvurusunda](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#user-entity)bulunabilir.

POST isteğinin nasıl oluşturulduğunu `B2CGraphClient.SendGraphPostRequest(...)`görebilirsiniz.

* İsteğin `Authorization` üstbilgisine bir erişim belirteci iliştirir.
* Ayarlar `api-version=1.6`.
* İstek gövdesinde JSON Kullanıcı nesnesini içerir.

> [!NOTE]
> Var olan bir kullanıcı deposundan geçiş yapmak istediğiniz hesaplar, [Azure AD B2C tarafından zorlanan güçlü parola kuvvetinden](/previous-versions/azure/jj943764(v=azure.100))daha düşük bir parola gücüne sahipse, ' deki `DisableStrongPassword` değeri kullanarak güçlü parola gereksinimini devre dışı bırakabilirsiniz. `passwordPolicies`özelliği. Örneğin, yukarıda belirtilen kullanıcı oluştur isteğini aşağıdaki şekilde değiştirebilirsiniz: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.
> 
> 

### <a name="update-consumer-user-accounts"></a>Tüketici Kullanıcı hesaplarını güncelleştirme
Kullanıcı nesnelerini güncelleştirdiğinizde, işlem Kullanıcı nesneleri oluşturmak için kullandığınız bir şekilde benzerdir. Ancak bu işlem http `PATCH` yöntemini kullanır:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer"                // this request updates only the user's displayName
}
```

JSON dosyalarınızı yeni verilerle güncelleştirerek bir kullanıcıyı güncelleştirmeyi deneyin. Ardından, aşağıdaki komutlardan `B2CGraphClient` birini çalıştırmak için kullanabilirsiniz:

```cmd
B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Bu isteğin nasıl gönderileceği hakkındaki ayrıntılar için yönteminiinceleyin.`B2CGraphClient.SendGraphPatchRequest(...)`

### <a name="search-users"></a>Kullanıcı ara
B2C kiracınızdaki kullanıcıları birkaç yolla arayabilirsiniz. Biri, kullanıcının nesne kimliğini veya ikisini kullanarak, kullanıcının oturum açma tanımlayıcısını (yani, `signInNames` özelliği) kullanarak.

Belirli bir kullanıcıyı aramak için aşağıdaki komutlardan birini çalıştırın:

```cmd
B2C Get-User <user-object-id>
B2C Get-User <filter-query-expression>
```

İşte birkaç örnek:

```cmd
B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Kullanıcıları Sil
Bir kullanıcıyı silme işlemi basittir. HTTP `DELETE` yöntemini kullanın ve URL 'yi doğru nesne kimliğiyle oluşturun:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Bir örnek görmek için, bu komutu girin ve konsola yazdırılmış silme isteğini görüntüleyin:

```cmd
B2C Delete-User <object-id-of-user>
```

Bu isteğin nasıl gönderileceği hakkındaki ayrıntılar için yönteminiinceleyin.`B2CGraphClient.SendGraphDeleteRequest(...)`

Kullanıcı yönetimine ek olarak Azure AD Graph API diğer birçok eylemi gerçekleştirebilirsiniz. [Azure AD Graph API başvurusu](/previous-versions/azure/ad/graph/api/api-catalog) , örnek isteklerle birlikte her eyleme ilişkin ayrıntılar sağlar.

## <a name="use-custom-attributes"></a>Özel öznitelikler kullanma
Çoğu tüketici uygulamasının bazı tür özel kullanıcı profili bilgilerini depolaması gerekir. Bunu yapmanın bir yolu, B2C kiracınızda özel bir öznitelik tanımlamaktır. Daha sonra, bu özniteliği bir kullanıcı nesnesi üzerinde diğer herhangi bir özelliği değerlendirmiş şekilde kullanabilirsiniz. Özniteliği güncelleştirebilir, özniteliğini silebilir, özniteliği ile sorgulayabilir, öznitelik, oturum açma belirteçlerinde talep olarak gönderebilir ve daha fazlasını yapabilirsiniz.

B2C kiracınızda özel bir öznitelik tanımlamak için [B2C özel öznitelik başvurusuna](active-directory-b2c-reference-custom-attr.md)bakın.

Aşağıdakileri kullanarak `B2CGraphClient`B2C kiracınızda tanımlanan özel öznitelikleri görüntüleyebilirsiniz:

```cmd
B2C Get-B2C-Application
B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Bu işlevlerin çıktısı, aşağıdaki gibi her bir özel özniteliğin ayrıntılarını ortaya koyar:

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

Kullanıcı nesnelerinizin bir özelliği olarak gibi tam adı `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`kullanabilirsiniz.  . JSON Dosyanızı yeni özelliği ve özelliği için bir değer ile güncelleştirin ve ardından şunu çalıştırın:

```cmd
B2C Update-User <object-id-of-user> <path-to-json-file>
```

Kullanarak `B2CGraphClient`, B2C kiracı kullanıcılarınızı programlama yoluyla yönetebileceğini bir hizmet uygulamanız vardır. `B2CGraphClient`, Azure AD Graph API kimlik doğrulaması yapmak için kendi uygulama kimliğini kullanır. Ayrıca, bir istemci gizli anahtarı kullanarak belirteçleri alır. Uygulamanıza bu işlevselliği eklediğinizde B2C uygulamaları için birkaç önemli noktayı unutmayın:

* Uygulamaya kiracıda uygun izinleri vermeniz gerekir.
* Şimdilik, erişim belirteçleri almak için ADAL (MSAL değil) kullanmanız gerekir. (Bir kitaplık kullanmadan doğrudan protokol iletileri de gönderebilirsiniz.)
* Graph API çağırdığınızda kullanın `api-version=1.6`.
* Tüketici kullanıcılarını oluştururken ve güncelleştirdiğinizde, yukarıda açıklandığı gibi birkaç özellik gereklidir.

B2C kiracınızda Graph API kullanarak gerçekleştirmek istediğiniz herhangi bir sorunuz veya isteğiniz varsa, bu makaleye bir yorum bırakın veya GitHub kodu örnek deposunda bir sorun kodlayın.

