---
title: Yönetilen kimlikler
description: Yönetilen kimliklerin Azure Uygulama Hizmeti ve Azure İşlevleri'nde nasıl çalıştığını, yönetilen bir kimliği nasıl yapılandıracağınızı ve arka uç kaynağı için bir belirteç oluşturacağınızı öğrenin.
author: mattchenderson
ms.topic: article
ms.date: 04/14/2020
ms.author: mahender
ms.reviewer: yevbronsh
ms.openlocfilehash: 875d2bbebdfa95c6d180979399d876eb2afc01b4
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81392535"
---
# <a name="how-to-use-managed-identities-for-app-service-and-azure-functions"></a>Uygulama Hizmeti ve Azure İşlevleri için yönetilen kimlikler nasıl kullanılır?

> [!Important] 
> Uygulamanız abonelikler/kiracılar arasında taşınırsa, Uygulama Hizmeti ve Azure İşlevleri için yönetilen kimlikler beklendiği gibi çalışmaz. Uygulamanın, özelliği devre dışı bırakarak ve yeniden etkinleştirerek yapılabilecek yeni bir kimlik edinmesi gerekir. Bkz. Aşağıdaki [kimlik kaldırma.](#remove) Alt akış kaynaklarının, yeni kimliği kullanmak için erişim ilkelerinin güncelleştirilmiş olması da gerekir.

Bu konu, Uygulama Hizmeti ve Azure İşlevleri uygulamaları için yönetilen bir kimliğin nasıl oluşturulabileceğinizi ve diğer kaynaklara erişmek için nasıl kullanacağınızı gösterir. Azure Etkin Dizini'nden (Azure AD) yönetilen bir kimlik, uygulamanızın Azure Ad korumalı diğer Azure Erişim Kasası gibi kaynaklara kolayca erişmesine olanak tanır. Kimlik Azure platformu tarafından yönetilir ve herhangi bir sırrı sağlamanızı veya döndürmenizi gerektirmez. Azure AD'deki yönetilen kimlikler hakkında daha fazla şey için Azure [kaynakları için Yönetilen kimlikler'e](../active-directory/managed-identities-azure-resources/overview.md)bakın.

Başvurunuza iki tür kimlik verilebilir:

- **Sistemle atanmış bir kimlik** uygulamanıza bağlıdır ve uygulamanız silinirse silinir. Bir uygulamayalnızca bir sistem tarafından atanmış kimliğe sahip olabilir.
- **Kullanıcı tarafından atanan kimlik,** uygulamanıza atanabilecek bağımsız bir Azure kaynağıdır. Bir uygulamanın birden çok kullanıcı tarafından atanmış kimliği olabilir.

## <a name="add-a-system-assigned-identity"></a>Sisteme atanmış kimlik ekleme

Sistem tarafından atanmış bir kimliğe sahip bir uygulama oluşturmak, uygulamada ayarlanacak ek bir özellik gerektirir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Portalda yönetilen bir kimlik ayarlamak için, önce normal bir uygulama oluşturacak ve ardından özelliği etkinleştireceksiniz.

1. Portalda normalde olduğu gibi bir uygulama oluşturun. Portalda ona gidin.

2. Bir işlev uygulaması kullanıyorsanız, **Platform özelliklerine**gidin. Diğer uygulama türleri için, sol navigasyondaki **Ayarlar** grubuna gidin.

3. **Kimlik**seçin.

4. Atanan **Sistem** sekmesiiçinde, **Durum** Açık'a **On**geçin. **Kaydet**’e tıklayın.

    ![Uygulama Hizmetinde yönetilen kimlik](media/app-service-managed-service-identity/system-assigned-managed-identity-in-azure-portal.png)

### <a name="using-the-azure-cli"></a>Azure CLI kullanma

Azure CLI'yi kullanarak yönetilen bir kimlik oluşturmak için `az webapp identity assign` komutu varolan bir uygulamaya karşı kullanmanız gerekir. Bu bölümdeki örnekleri çalıştırmak için üç seçeneğiniz var:

- Azure portalından [Azure Bulut Kabuğu'nu](../cloud-shell/overview.md) kullanın.
- Aşağıdaki her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesi aracılığıyla gömülü Azure Bulut Kabuğu'nu kullanın.
- Yerel bir CLI konsolu kullanmak isterseniz [Azure CLI'nin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.31 veya sonraki) en son sürümünü yükleyin. 

Aşağıdaki adımlar, bir web uygulaması oluşturma ve CLI kullanarak bir kimlik atama ile size yol açacaktır:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. Uygulamayı dağıtmak istediğiniz Azure aboneliğiyle ilişkili bir hesap kullanın:

    ```azurecli-interactive
    az login
    ```

2. CLI'yi kullanarak bir web uygulaması oluşturun. CLI'nin App Service ile nasıl kullanılacağına daha fazla örnek için [Bkz. App Service CLI örnekleri:](../app-service/samples-cli.md)

    ```azurecli-interactive
    az group create --name myResourceGroup --location westus
    az appservice plan create --name myPlan --resource-group myResourceGroup --sku S1
    az webapp create --name myApp --resource-group myResourceGroup --plan myPlan
    ```

3. Bu `identity assign` uygulama için kimlik oluşturmak için komutu çalıştırın:

    ```azurecli-interactive
    az webapp identity assign --name myApp --resource-group myResourceGroup
    ```

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki adımlar, Azure PowerShell kullanarak bir web uygulaması oluşturmanız ve ona bir kimlik atamanız için size yol açacaktır:

1. Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeleri kullanarak Azure PowerShell'i yükleyin ve ardından Azure ile bağlantı oluşturmak için çalıştırın. `Login-AzAccount`

2. Azure PowerShell'i kullanarak bir web uygulaması oluşturun. Uygulama Hizmeti ile Azure PowerShell'in nasıl kullanılacağına daha fazla örnek için [Bkz. Uygulama Hizmeti PowerShell örnekleri:](../app-service/samples-powershell.md)

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzResourceGroup -Name myResourceGroup -Location $location

    # Create an App Service plan in Free tier.
    New-AzAppServicePlan -Name $webappname -Location $location -ResourceGroupName myResourceGroup -Tier Free

    # Create a web app.
    New-AzWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName myResourceGroup
    ```

3. Bu `Set-AzWebApp -AssignIdentity` uygulama için kimlik oluşturmak için komutu çalıştırın:

    ```azurepowershell-interactive
    Set-AzWebApp -AssignIdentity $true -Name $webappname -ResourceGroupName myResourceGroup 
    ```

### <a name="using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanma

Azure Kaynak Yöneticisi şablonu, Azure kaynaklarınızın dağıtımını otomatikleştirmek için kullanılabilir. Uygulama Hizmeti ve İşlevlerini dağıtma hakkında daha fazla bilgi edinmek için, [Uygulama Hizmeti'nde kaynak dağıtımını otomatikleştirme](../app-service/deploy-complex-application-predictably.md) ve [Azure İşlevlerini otomatikleştirme](../azure-functions/functions-infrastructure-as-code.md)hakkında bilgi edinin.

Herhangi bir `Microsoft.Web/sites` tür kaynak kaynak tanımına aşağıdaki özelliği ekleyerek bir kimlik ile oluşturulabilir:

```json
"identity": {
    "type": "SystemAssigned"
}
```

> [!NOTE]
> Bir uygulama aynı anda hem sistem tarafından atanmış hem de kullanıcı tarafından atanan kimliklere sahip olabilir. Bu durumda, `type` özellik`SystemAssigned,UserAssigned`

Sistem tarafından atanan türü eklemek, Azure'a uygulamanızın kimliğini oluşturmasını ve yönetmesini söyler.

Örneğin, bir web uygulaması aşağıdaki gibi görünebilir:

```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
    ]
}
```

Site oluşturulduğunda, aşağıdaki ek özelliklere sahiptir:

```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

TenantId özelliği, kimliğin hangi Azure AD kiracısına ait olduğunu tanımlar. PrincipalId, uygulamanın yeni kimliği için benzersiz bir tanımlayıcıdır. Azure AD'de, hizmet sorumlusu, Uygulama Hizmetinize veya Azure İşlevlerinize vermiş olduğunuz ada sahiptir.

## <a name="add-a-user-assigned-identity"></a>Kullanıcı tarafından atanan bir kimlik ekleme

Kullanıcı tarafından atanan bir kimliğe sahip bir uygulama oluşturmak, kimliği oluşturmanızı ve ardından kaynak tanımlayıcısını uygulama config'inize eklemenizi gerektirir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

İlk olarak, kullanıcı tarafından atanmış bir kimlik kaynağı oluşturmanız gerekir.

1. [Bu yönergelere](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)göre kullanıcı tarafından atanan yönetilen bir kimlik kaynağı oluşturun.

2. Portalda normalde olduğu gibi bir uygulama oluşturun. Portalda ona gidin.

3. Bir işlev uygulaması kullanıyorsanız, **Platform özelliklerine**gidin. Diğer uygulama türleri için, sol navigasyondaki **Ayarlar** grubuna gidin.

4. **Kimlik**seçin.

5. Kullanıcı **atanan** sekmesinde **Ekle'yi**tıklatın.

6. Daha önce oluşturduğunuz kimliği arayın ve seçin. **Ekle**'ye tıklayın.

    ![Uygulama Hizmetinde yönetilen kimlik](media/app-service-managed-service-identity/user-assigned-managed-identity-in-azure-portal.png)

### <a name="using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanma

Azure Kaynak Yöneticisi şablonu, Azure kaynaklarınızın dağıtımını otomatikleştirmek için kullanılabilir. Uygulama Hizmeti ve İşlevlerini dağıtma hakkında daha fazla bilgi edinmek için, [Uygulama Hizmeti'nde kaynak dağıtımını otomatikleştirme](../app-service/deploy-complex-application-predictably.md) ve [Azure İşlevlerini otomatikleştirme](../azure-functions/functions-infrastructure-as-code.md)hakkında bilgi edinin.

Herhangi bir `Microsoft.Web/sites` tür kaynak, kaynak tanımına aşağıdaki bloğu ekleyerek, istenen `<RESOURCEID>` kimliğin kaynak kimliğiyle değiştirilerek bir kimlikle oluşturulabilir:

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {}
    }
}
```

> [!NOTE]
> Bir uygulama aynı anda hem sistem tarafından atanmış hem de kullanıcı tarafından atanan kimliklere sahip olabilir. Bu durumda, `type` özellik`SystemAssigned,UserAssigned`

Kullanıcı tarafından atanan türü eklemek, Azure'a uygulamanız için belirtilen kullanıcı tarafından atanan kimliği kullanmasını söyler.

Örneğin, bir web uygulaması aşağıdaki gibi görünebilir:

```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]": {}
        }
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
    ]
}
```

Site oluşturulduğunda, aşağıdaki ek özelliklere sahiptir:

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {
            "principalId": "<PRINCIPALID>",
            "clientId": "<CLIENTID>"
        }
    }
}
```

PrincipalId, Azure AD yönetimi için kullanılan kimlik için benzersiz bir tanımlayıcıdır. ClientId, uygulamanın çalışma zamanı aramaları sırasında hangi kimliği niçin kullanılacağını belirtmek için kullanılan yeni kimliği için benzersiz bir tanımlayıcıdır.

## <a name="obtain-tokens-for-azure-resources"></a>Azure kaynakları için belirteçler edinin

Bir uygulama, Azure Anahtar Kasası gibi Azure AD tarafından korunan diğer kaynaklara erişmek için belirteçleri elde etmek için yönetilen kimliğini kullanabilir. Bu belirteçler, kaynağa erişen uygulamayı temsil eder ve uygulamanın belirli bir kullanıcısını temsil etmez. 

Uygulamanızdan erişime izin vermek için hedef kaynağı yapılandırmanız gerekebilir. Örneğin, Key Vault'a erişmek için bir belirteç talep ederseniz, uygulamanızın kimliğini içeren bir erişim ilkesi eklediğinizden emin olmanız gerekir. Aksi takdirde, Key Vault'a yapılan aramalar, belirteci içerseler bile reddedilir. Hangi kaynakların Azure Etkin Dizin belirteçlerini desteklediği hakkında daha fazla bilgi edinmek için Azure [AD kimlik doğrulamasını destekleyen Azure hizmetlerine](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)bakın.

> [!IMPORTANT]
> Yönetilen kimlikler için arka uç hizmetleri, kaynak URI başına önbelleği yaklaşık 8 saat boyunca korur. Belirli bir hedef kaynağın erişim ilkesini günceller ve bu kaynak için hemen bir belirteç alırsanız, belirteç süresi dolana kadar eski izinleri olan önbelleğe alınmış bir belirteç almaya devam edebilirsiniz. Şu anda bir belirteç yenileme zorlamak için bir yolu yoktur.

Uygulama Hizmeti ve Azure İşlevleri'nde belirteç elde etmek için basit bir REST protokolü vardır. Bu, tüm uygulamalar ve diller için kullanılabilir. .NET ve Java için Azure SDK bu protokol üzerinde bir soyutlama sağlar ve yerel bir geliştirme deneyimini kolaylaştırır.

### <a name="using-the-rest-protocol"></a>REST protokolünü kullanma

Yönetilen bir kimliğe sahip bir uygulamanın tanımlanmış iki ortam değişkeni vardır:

- IDENTITY_ENDPOINT - yerel belirteç hizmetinin URL'si.
- IDENTITY_HEADER - sunucu tarafındaki istek sahteciliği (SSRF) saldırılarını azaltmaya yardımcı olmak için kullanılan bir üstbilgi. Değer platform tarafından döndürülür.

**IDENTITY_ENDPOINT,** uygulamanızın jeton isteyebileceği yerel bir URL'dir. Bir kaynak için belirteç almak için, aşağıdaki parametreler de dahil olmak üzere bu uç noktaya bir HTTP GET isteği yapın:

> | Parametre adı    | İçindeki     | Açıklama                                                                                                                                                                                                                                                                                                                                |
> |-------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | kaynak          | Sorgu  | Belirteç alınması gereken kaynağın Azure AD kaynağı URI. Bu, [Azure AD kimlik doğrulamasını](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) veya başka bir kaynak URI'yi destekleyen Azure hizmetlerinden biri olabilir.    |
> | api-sürümü       | Sorgu  | Kullanılacak belirteç API sürümü. Lütfen "2019-08-01" veya daha sonra kullanın.                                                                                                                                                                                                                                                                 |
> | X-KİmLİk-ÜSTER | Üst bilgi | IDENTITY_HEADER ortamı değişkeninin değeri. Bu üstbilgi, sunucu tarafındaki istek sahteciliği (SSRF) saldırılarını azaltmaya yardımcı olmak için kullanılır.                                                                                                                                                                                                    |
> | client_id         | Sorgu  | (İsteğe bağlı) Kullanılacak kullanıcı tarafından atanan kimliğin istemci kimliği. `principal_id`" `mi_res_id`veya `object_id`. Tüm kimlik parametreleri `principal_id` `object_id`(, `mi_res_id``client_id`, , ve ) atlanırsa, sistem tarafından atanan kimlik kullanılır.                                             |
> | Principal_id      | Sorgu  | (İsteğe bağlı) Kullanılacak kullanıcı tarafından atanan kimliğin temel kimliği. `object_id`bunun yerine kullanılabilecek bir takma addır. client_id, mi_res_id veya object_id içeren bir istekte kullanılamaz. Tüm kimlik parametreleri `principal_id` `object_id`(, `mi_res_id``client_id`, , ve ) atlanırsa, sistem tarafından atanan kimlik kullanılır. |
> | mi_res_id         | Sorgu  | (İsteğe bağlı) Kullanılacak kullanıcı tarafından atanan kimliğin Azure kaynak kimliği. `principal_id`" `client_id`veya `object_id`. Tüm kimlik parametreleri `principal_id` `object_id`(, `mi_res_id``client_id`, , ve ) atlanırsa, sistem tarafından atanan kimlik kullanılır.                                      |

> [!IMPORTANT]
> Kullanıcı tarafından atanan kimlikler için belirteçler elde etmeye çalışıyorsanız, isteğe bağlı özelliklerden birini eklemeniz gerekir. Aksi takdirde belirteç hizmeti, sistem tarafından atanmış bir kimlik için var olabilir veya olmayabilir bir belirteç elde etmeye çalışır.

Başarılı bir 200 OK yanıtı aşağıdaki özelliklere sahip bir JSON gövdesi içerir:

> | Özellik adı | Açıklama                                                                                                                                                                                                                                        |
> |---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | access_token  | İstenen erişim jetonu. Arama web hizmeti, alıcı web hizmetinin kimliğini doğrulamak için bu belirteci kullanabilir.                                                                                                                               |
> | client_id     | Kullanılan kimliğin istemci kimliği.                                                                                                                                                                                                       |
> | expires_on    | Erişim belirteci süresi dolduğunda zaman aşımı. Tarih, "1970-01-01T0:0:0Z UTC" saniye sayısı olarak temsil edilir (belirteç `exp` talebine karşılık gelir).                                                                                |
> | not_before    | Erişim belirteci etkin olduğunda zaman alanı ve kabul edilebilir. Tarih, "1970-01-01T0:0:0Z UTC" saniye sayısı olarak temsil edilir (belirteç `nbf` talebine karşılık gelir).                                                      |
> | kaynak      | Erişim belirteci için istenen ve isteğin `resource` sorgu dize parametresi ile eşleşen kaynak.                                                                                                                               |
> | token_type    | Belirteç türü değerini gösterir. Azure AD'nin desteklediği tek tür FBearer'dır. Taşıyıcı belirteçleri hakkında daha fazla bilgi için Bkz. [OAuth 2.0 Yetkilendirme Çerçevesi: Taşıyıcı Belirteç Kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |

Bu yanıt, Azure [AD hizmetinden hizmete erişim belirteç isteğinin yanıtıyla](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#service-to-service-access-token-response)aynıdır.

> [!NOTE]
> Bu protokolün "2017-09-01" API sürümünü kullanan eski `secret` bir sürümü, `X-IDENTITY-HEADER` üstbilgi yerine `clientid` kullanılmıştır ve yalnızca kullanıcı tarafından atanan özelliği kabul eder. Ayrıca bir `expires_on` zaman damgası biçiminde döndü. MSI_ENDPOINT IDENTITY_ENDPOINT için takma ad olarak kullanılabilir ve MSI_SECRET IDENTITY_HEADER için bir takma ad olarak kullanılabilir.

### <a name="rest-protocol-examples"></a>REST protokol örnekleri

Örnek bir istek aşağıdaki gibi görünebilir:

```http
GET /MSI/token?resource=https://vault.azure.net&api-version=2019-08-01 HTTP/1.1
Host: localhost:4141
X-IDENTITY-HEADER: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```

Ve örnek bir yanıt aşağıdaki gibi görünebilir:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "access_token": "eyJ0eXAi…",
    "expires_on": "1586984735",
    "resource": "https://vault.azure.net",
    "token_type": "Bearer",
    "client_id": "5E29463D-71DA-4FE0-8E69-999B57DB23B0"
}
```

### <a name="code-examples"></a>Kod örnekleri

# <a name="net"></a>[.NET](#tab/dotnet)

> [!TIP]
> .NET dilleri için, bu isteği kendiniz oluşturmak yerine [Microsoft.Azure.Services.AppAuthentication'ı](#asal) da kullanabilirsiniz.

```csharp
private readonly HttpClient _client;
// ...
public async Task<HttpResponseMessage> GetToken(string resource)  {
    var request = new HttpRequestMessage(HttpMethod.Get, 
        String.Format("{0}/?resource={1}&api-version=2019-08-01", Environment.GetEnvironmentVariable("IDENTITY_ENDPOINT"), resource));
    request.Headers.Add("X-IDENTITY-HEADER", Environment.GetEnvironmentVariable("IDENTITY_HEADER"));
    return await _client.SendAsync(request);
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const rp = require('request-promise');
const getToken = function(resource, cb) {
    let options = {
        uri: `${process.env["IDENTITY_ENDPOINT"]}/?resource=${resource}&api-version=2019-08-01`,
        headers: {
            'X-IDENTITY-HEADER': process.env["IDENTITY_HEADER"]
        }
    };
    rp(options)
        .then(cb);
}
```

# <a name="python"></a>[Python](#tab/python)

```python
import os
import requests

identity_endpoint = os.environ["IDENTITY_ENDPOINT"]
identity_header = os.environ["IDENTITY_HEADER"]

def get_bearer_token(resource_uri):
    token_auth_uri = f"{identity_endpoint}?resource={resource_uri}&api-version=2019-08-01"
    head_msi = {'X-IDENTITY-HEADER':identity_header}

    resp = requests.get(token_auth_uri, headers=head_msi)
    access_token = resp.json()['access_token']

    return access_token
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:IDENTITY_ENDPOINT + "?resource=$resourceURI&api-version=2019-08-01"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"X-IDENTITY-HEADER"="$env:IDENTITY_HEADER"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```

---

### <a name="using-the-microsoftazureservicesappauthentication-library-for-net"></a><a name="asal"></a>.NET için Microsoft.Azure.Services.AppAuthentication kitaplığını kullanma

.NET uygulamaları ve işlevleri için yönetilen bir kimlikle çalışmanın en basit yolu Microsoft.Azure.Services.AppAuthentication paketidir. Bu kitaplık ayrıca Visual Studio, [Azure CLI](/cli/azure)veya Active Directory Integrated Authentication'daki kullanıcı hesabınızı kullanarak kodunuzu geliştirme makinenizde yerel olarak test etmenize olanak tanır. Bu kitaplıkla yerel geliştirme seçenekleri hakkında daha fazla bilgi için [Microsoft.Azure.Services.AppAuthentication başvurusuna]bakın. Bu bölümde, kodunuzda kitaplık la nasıl başlanıncayasınız gösterilmektedir.

1. [Microsoft.Azure.Services.AppAuthentication'a](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) ve diğer gerekli NuGet paketlerine başvurularınızı uygulamanıza ekleyin. Aşağıdaki örnekte [Microsoft.Azure.KeyVault da kullanabilirsiniz.](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)

2. Doğru kaynağı hedeflemek için aşağıdaki kodu uygulamanıza ekleyin. Bu örnek, Azure Anahtar Kasası ile çalışmanın iki yolunu gösterir:

    ```csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;
    // ...
    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
    // OR
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
    ```

Microsoft.Azure.Services.AppAuthentication ve sunduğu işlemler hakkında daha fazla bilgi edinmek için [Microsoft.Azure.Services.AppAuthentication başvurusu] ve [MSI .NET örnekli Uygulama Hizmeti ve KeyVault'a](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)bakın.

### <a name="using-the-azure-sdk-for-java"></a>Java için Azure SDK'yı kullanma

Java uygulamaları ve işlevleri için yönetilen bir kimlikle çalışmanın en basit yolu [Java için Azure SDK'dır.](https://github.com/Azure/azure-sdk-for-java) Bu bölümde, kodunuzda kitaplık la nasıl başlanıncayasınız gösterilmektedir.

1. [Azure SDK kitaplığına](https://mvnrepository.com/artifact/com.microsoft.azure/azure)başvuru ekleyin. Maven projeleri için, bu parçacığı projenin POM dosyasının bölümüne `dependencies` ekleyebilirsiniz:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure</artifactId>
        <version>1.23.0</version>
    </dependency>
    ```

2. Kimlik `AppServiceMSICredentials` doğrulaması için nesneyi kullanın. Bu örnek, bu mekanizmanın Azure Anahtar Kasası ile çalışmak için nasıl kullanılabileceğini gösterir:

    ```java
    import com.microsoft.azure.AzureEnvironment;
    import com.microsoft.azure.management.Azure;
    import com.microsoft.azure.management.keyvault.Vault
    //...
    Azure azure = Azure.authenticate(new AppServiceMSICredentials(AzureEnvironment.AZURE))
            .withSubscription(subscriptionId);
    Vault myKeyVault = azure.vaults().getByResourceGroup(resourceGroup, keyvaultName);

    ```


## <a name="remove-an-identity"></a><a name="remove"></a>Kimliği kaldırma

Sistemtarafından atanan kimlik, özelliği portal, PowerShell veya CLI kullanılarak oluşturulduğu şekilde devre dışı bırakarak kaldırılabilir. Kullanıcı tarafından atanan kimlikler tek tek kaldırılabilir. Tüm kimlikleri kaldırmak için, [ARM şablonundaki](#using-an-azure-resource-manager-template)türü "Yok" olarak ayarlayın:

```json
"identity": {
    "type": "None"
}
```

Bu şekilde sisteme atanmış bir kimliğin kaldırılması, kimliği Azure AD'den de siler. Sistem tarafından atanan kimlikler, uygulama kaynağı silindiğinde otomatik olarak Azure AD'den kaldırılır.

> [!NOTE]
> Yerel belirteç hizmetini devre dışı bırakan WEBSITE_DISABLE_MSI ayarlanabilen bir uygulama ayarı da vardır. Ancak, kimliği yerinde bırakır ve takım yine de yönetilen kimliği "a"veya "etkin" olarak gösterir. Sonuç olarak, bu ayarın kullanılması önerilmez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yönetilen bir kimlik kullanarak SQL Veritabanına güvenli bir şekilde erişin](app-service-web-tutorial-connect-msi.md)

[Microsoft.Azure.Services.AppAuthentication başvurusu]: https://go.microsoft.com/fwlink/p/?linkid=862452
