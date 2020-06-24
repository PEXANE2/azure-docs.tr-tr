---
title: Bir istemci uygulamasının kimliğini doğrulama
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS hizmetinde istemci uygulamasının kimliğini doğrulama.
author: baanders
ms.author: baanders
ms.date: 4/22/2020
ms.topic: how-to
ms.service: digital-twins
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: 89de2e86e425e2c585f9a1243f6666a0ae077c10
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84726165"
---
# <a name="authenticate-a-client-application-with-azure-digital-twins"></a>Azure dijital TWINS ile istemci uygulamanın kimliğini doğrulama

[!INCLUDE [Azure Digital Twins current preview status](../../includes/digital-twins-preview-status.md)]

[Bir Azure dijital TWINS örneği](how-to-set-up-instance.md)oluşturduktan sonra, örnekle etkileşim kurmak için kullanacağınız bir istemci uygulaması oluşturabilirsiniz. Bir başlatıcı istemci projesi ayarladıktan sonra bu makalede, istemci uygulamasının Azure dijital TWINS örneğiyle nasıl doğru şekilde doğrulanabilmesi gösterilmektedir.

Bu iki adımda yapılır:
1. Uygulama kaydı oluşturma
2. İstemci uygulamasında kimlik doğrulama kodu yazma

[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

## <a name="create-an-app-registration"></a>Uygulama kaydı oluşturma

Bir istemci uygulamasından Azure dijital TWINS 'te kimlik doğrulaması yapmak için [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)bir **uygulama kaydı** ayarlamanız gerekir.

Bu uygulama kaydı, [Azure dijital TWINS API 'leri](how-to-use-apis-sdks.md)için erişim izinlerini yapılandırdığınız yerdir. İstemci uygulamanız uygulama kaydına göre kimlik doğrulaması yapar ve sonuç olarak API 'lere yapılandırılmış erişim izinleri verilir.

Bir uygulama kaydı oluşturmak için Azure dijital TWINS API 'Leri için kaynak kimliklerini ve API 'nin temel izinlerini sağlamanız gerekir. Çalışma dizininizde, yeni bir dosya açın ve bu ayrıntıları yapılandırmak için aşağıdaki JSON kod parçacığını girin: 

```json
[{
    "resourceAppId": "0b07f429-9f4b-4714-9392-cc5e8e80c8b0",
    "resourceAccess": [
     {
       "id": "4589bd03-58cb-4e6c-b17f-b580e39652f8",
       "type": "Scope"
     }
    ]
}]
``` 

Bu dosyayı *manifest.js*olarak kaydedin.

> [!NOTE] 
> `https://digitaltwins.azure.net`GUID yerine, Azure Digital TWINS kaynak uygulama kimliği için "kolay ve" insan tarafından okunabilen bir dizenin kullanılabileceği bazı konumlar vardır `0b07f429-9f4b-4714-9392-cc5e8e80c8b0` . Örneğin, bu belge genelinde birçok örnek, MSAL kitaplığı ile kimlik doğrulaması kullanır ve kolay dize bu şekilde kullanılabilir. Ancak, uygulama kaydını oluşturma adımında, KIMLIğIN GUID biçimi yukarıda gösterildiği gibi gereklidir. 

Cloud Shell pencerenizde, "dosyaları karşıya yükle/Indir" simgesine tıklayın ve "karşıya yükle" yi seçin.

:::image type="content" source="media/how-to-authenticate-client/upload-extension.png" alt-text="Karşıya yükleme seçeneğinin seçimini gösteren Cloud Shell pencere":::
Yeni oluşturduğunuz *manifest.js* gidin ve "Aç" düğmesine basın.

Ardından, bir uygulama kaydı oluşturmak için aşağıdaki komutu çalıştırın (yer tutucuları gerektiği gibi değiştirin):

```azurecli
az ad app create --display-name <name-for-your-app> --native-app --required-resource-accesses manifest.json --reply-url http://localhost
```

Bu komutun çıktısı şuna benzer şekilde görünür.

:::image type="content" source="media/how-to-authenticate-client/new-app-registration.png" alt-text="Yeni AAD uygulama kaydı":::

Uygulama kaydını oluşturduktan sonra, Azure portal AAD uygulama kaydına genel bakış sayfasına gitmek için [Bu bağlantıyı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) izleyin.

Bu genel bakışta, listeden yeni oluşturduğunuz uygulama kaydını seçin. Bu, ayrıntılarını şunun gibi bir sayfada açar:

:::image type="content" source="media/how-to-authenticate-client/get-authentication-ids.png" alt-text="Azure portal: kimlik doğrulama kimlikleri":::

**Sayfanızda gösterilen** *uygulama (istemci) kimliğini* ve *Dizin (kiracı) kimliğini* bir yere göz atın. Bu değerleri daha sonra, Azure dijital TWINS API 'Lerinde istemci uygulamasının kimliğini doğrulamak için kullanacaksınız.

> [!NOTE]
> Senaryonuza bağlı olarak, uygulama kaydında ek değişiklikler yapmanız gerekebilir. Aşağıda, karşılamanız gerekebilecek bazı yaygın gereksinimler verilmiştir:
> * Ortak istemci erişimini etkinleştir
> * Web ve masaüstü erişimi için belirli yanıt URL 'Lerini ayarlayın
> * Örtük OAuth2 kimlik doğrulama akışları için izin ver
> * Azure aboneliğiniz, canlı, Xbox veya Hotmail gibi bir Microsoft hesabı kullanılarak oluşturulduysa, kişisel hesapları desteklemek için uygulama kaydında *Signınaudience* 'yı ayarlamanız gerekir.
> Bu ayarları ayarlamanın en kolay yolu [Azure Portal](https://portal.azure.com/)kullanmaktır. Bu işlem hakkında daha fazla bilgi için bkz. [Microsoft Identity platformu ile uygulama kaydetme](https://docs.microsoft.com/graph/auth-register-app-v2).

## <a name="write-client-app-authentication-code-net-c-sdk"></a>İstemci uygulaması kimlik doğrulama kodunu yaz: .NET (C#) SDK

Bu bölümde, .NET (C#) SDK 'SıNı kullanarak kimlik doğrulama işlemini tamamlayabilmeniz için istemci uygulamanıza eklemeniz gereken kod açıklanmaktadır.
Azure Digital TWINS C# SDK 'Sı, .NET için Azure SDK 'sının bir parçasıdır. Burada bulunur: [.net Için Azure IoT Digital ikizi istemci kitaplığı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core).

### <a name="prerequisites"></a>Ön koşullar

Zaten ayarlanmış bir başlatıcı istemci uygulama projeniz yoksa, bu öğreticide kullanmak üzere temel bir .NET projesi oluşturun.

.NET SDK 'yı kullanabilmeniz için projenize aşağıdaki paketleri eklemeniz gerekir:
* `Azure.DigitalTwins.Core`(sürüm `1.0.0-preview.2` )
* `Azure.Identity`

Tercih ettiğiniz araçlara bağlı olarak, Visual Studio Paket Yöneticisi veya `dotnet` komut satırı aracı ile bunu yapabilirsiniz. 

### <a name="authentication-and-client-creation-net"></a>Kimlik doğrulaması ve istemci oluşturma: .NET

.NET SDK ile kimlik doğrulaması yapmak için, [Azure. Identity](https://docs.microsoft.com/dotnet/api/azure.identity?view=azure-dotnet) kitaplığında tanımlanmış kimlik bilgileri alma yöntemlerinden birini kullanın.

Yaygın olarak kullanılan ikisi şunlardır: 
* [Interactivebrowsercredential](https://docs.microsoft.com/dotnet/api/azure.identity.interactivebrowsercredential?view=azure-dotnet). Bu yöntem etkileşimli uygulamalara yöneliktir ve kimlik doğrulaması için bir Web tarayıcısı getirir.
* [Managedıdentitycredential](https://docs.microsoft.com/dotnet/api/azure.identity.managedidentitycredential?view=azure-dotnet). Bu yöntem, [yönetilen kimliklere (MSI)](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)ihtiyacınız olan durumlarda (örneğin, Azure işlevleriyle çalışırken) harika bir şekilde çalışır. 

Ayrıca şu using deyimleri de gereklidir:

```csharp
using Azure.Identity;
using Azure.DigitalTwins.Core;
```

Kimlik doğrulamalı bir SDK istemcisi oluşturmak üzere etkileşimli tarayıcı kimlik bilgilerini kullanmak için şu kodu ekleyin:

```csharp
// Your client / app registration ID
private static string clientId = "<your-client-ID>"; 
// Your tenant / directory ID
private static string tenantId = "<your-tenant-ID>";
// The URL of your instance, starting with the protocol (https://)
private static string adtInstanceUrl = "<your-Azure-Digital-Twins-instance-URL>";

//...

DigitalTwinsClient client;
try
{
    var credential = new InteractiveBrowserCredential(tenantId, clientId);
    client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credential);
} catch(Exception e)
{
    Console.WriteLine($"Authentication or client creation error: {e.Message}");
    Environment.Exit(0);
}
```

>[!NOTE]
> İstemci KIMLIĞI, kiracı KIMLIĞI ve örnek URL 'sini yukarıda gösterildiği gibi doğrudan koda yerleştirebileceğiniz sürece, kodunuzun bu değerleri bir yapılandırma dosyası veya ortam değişkeniyle alması iyi bir fikirdir.

Azure işlevinde, yönetilen kimlik bilgilerini şöyle kullanabilirsiniz:

```csharp
ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
DigitalTwinsClientOptions opts = 
    new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, opts);
```

Bkz. nasıl yapılır: işlev bağlamında önemli yapılandırma Seçimlerinizden bazılarını açıklayan daha tam bir örnek için [verileri işlemek üzere bir Azure Işlevi ayarlama](how-to-create-azure-function.md) .

Ayrıca, bir işlevde kimlik doğrulamasını kullanmak için şunları unutmayın:
* [Yönetilen kimliği etkinleştirme](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet)
* [Ortam değişkenleri](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables?tabs=csharp)
* İşlevler uygulamasına, dijital TWINS API 'Lerine erişmesini sağlayan izinler atayın. Bkz. nasıl yapılır: daha fazla bilgi için [verileri işlemeye yönelik bir Azure Işlevi ayarlama](how-to-create-azure-function.md) .

## <a name="authentication-in-an-autorest-generated-sdk"></a>Bir oto Rest tarafından oluşturulan SDK 'da kimlik doğrulaması

.NET kullanmıyorsanız, [nasıl yapılır: Azure dijital TWINS için özel SDK 'Lar oluşturma](how-to-create-custom-sdks.md)bölümünde açıklandığı gibi, tercih ettiğiniz BIR dilde SDK kitaplığı oluşturmayı tercih edebilirsiniz.

Bu bölümde, bu durumda nasıl kimlik doğrulaması yapılacağı açıklanmaktadır.

### <a name="prerequisites"></a>Ön koşullar

Bu örnek, oto Rest ile oluşturulan bir TypeScript SDK kullanır. Sonuç olarak, ayrıca şunları gerektirir:
* [msal-js](https://github.com/AzureAD/microsoft-authentication-library-for-js)
* [MS-Rest-js](https://github.com/Azure/ms-rest-js)

### <a name="minimal-authentication-code-sample"></a>En küçük kimlik doğrulama kodu örneği

Azure hizmetleriyle bir .NET uygulamasının kimliğini doğrulamak için, istemci uygulamanızda aşağıdaki en az kodu kullanabilirsiniz.

*Uygulamanızın (istemci) kimliği* ve *Dizin (kiracı) kimliğiniz* Ile Azure DIJITAL TWINS örneğinizin URL 'sini de kullanmanız gerekecektir.

> [!TIP]
> Azure Digital TWINS örneğinin URL 'SI, Azure Digital TWINS örneğinizin *ana bilgisayar adının*başlangıcına *https://* eklenerek yapılır. Ana bilgisayar adını görmek için, örneğinizin tüm özellikleriyle birlikte çalıştırabilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

```javascript
import * as Msal from "msal";
import { TokenCredentials } from "@azure/ms-rest-js";
// Autorest-generated SDK
import { AzureDigitalTwinsAPI } from './azureDigitalTwinsAPI';

// Client / app registration ID
var ClientId = "<your-client-ID>";
// Azure tenant / directory ID
var TenantId = "<your-tenant-ID>";
// URL of the Azure Digital Twins instance
var AdtInstanceUrl = "<your-instance-URL>"; 

var AdtAppId = "https://digitaltwins.azure.net";

let client = null;

export async function login() {

    const msalConfig = {
        auth: {
            clientId: ClientId,
            redirectUri: "http://localhost:3000",
            authority: "https://login.microsoftonline.com/"+TenantId
        }
    };

    const msalInstance = new Msal.UserAgentApplication(msalConfig);

    msalInstance.handleRedirectCallback((error, response) => {
        // handle redirect response or error
    });

    var loginRequest = {
        scopes: [AdtAppId + "/.default"] 
    };

    try {
        await msalInstance.loginPopup(loginRequest)
        var accessToken;
        // if the user is already logged in you can acquire a token
        if (msalInstance.getAccount()) {
            var tokenRequest = {
                scopes: [AdtAppId + "/.default"]
            };
            try {
                const response = await msalInstance.acquireTokenSilent(tokenRequest);
                accessToken = response.accessToken;
            } catch (err) {
                if (err.name === "InteractionRequiredAuthError") {
                    const response = await msalInstance.acquireTokenPopup(tokenRequest)
                    accessToken = response.accessToken;
                }
            }
        }
        if (accessToken!=null)
        {
            var tokenCredentials = new TokenCredentials(accessToken);
                
            // Add token and server URL to service instance
            const clientConfig = {
                baseUri: AdtInstanceUrl
            };
            client = new AzureDigitalTwinsAPI(tokenCredentials, clientConfig);
            appDataStore.client = client;
        }
    } catch (err) {
        ...
    }
}
```

Yukarıdaki kod, istemci KIMLIĞI, kiracı KIMLIĞI ve örnek URL 'sini kolaylık sağlaması için doğrudan koda yerleştirdiğinden, kodunuzun bu değerleri bir yapılandırma dosyası veya ortam değişkeniyle alması iyi bir fikir olabilir.

MSAL, önbelleğe alma ve diğer kimlik doğrulama akışları gibi işlemleri uygulamak için kullanabileceğiniz birçok daha fazla seçenek içerir. Bu konuda daha fazla bilgi için bkz. [Microsoft kimlik doğrulama kitaplığı 'Na genel bakış (msal)](../active-directory/develop/msal-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS 'de güvenliğin nasıl çalıştığı hakkında daha fazla bilgi edinin:
* [Kavramlar: Azure dijital TWINS çözümleri için güvenlik](concepts-security.md)

Ya da artık kimlik doğrulaması ayarlanmış ise, örneğiniz içinde modeller oluşturmak için üzerine geçin:
* [Nasıl yapılır: ikizi modelini yönetme](how-to-manage-model.md)