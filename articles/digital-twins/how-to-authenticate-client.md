---
title: Uygulama kimlik doğrulaması kodu yazma
titleSuffix: Azure Digital Twins
description: Bkz. bir istemci uygulamasında kimlik doğrulama kodu yazma
author: baanders
ms.author: baanders
ms.date: 4/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.custom: devx-track-javascript
ms.openlocfilehash: c211c0e5ef0b39f778db7c922fafc735e2411068
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88930052"
---
# <a name="write-client-app-authentication-code"></a>İstemci uygulaması kimlik doğrulama kodunu yaz

[Bir Azure dijital TWINS örneği ve kimlik doğrulaması](how-to-set-up-instance-scripted.md)ayarladıktan sonra, örnekle etkileşim kurmak için kullanacağınız bir istemci uygulaması oluşturabilirsiniz. Bir başlatıcı istemci projesi ayarladıktan sonra bu makalede, Azure dijital TWINS örneğinde **kimlik doğrulaması yapmak için bu istemci uygulamasında nasıl kod yazacağınız** gösterilmektedir.

Bu makaledeki örnek kodda iki yaklaşım vardır. Tercih ettiğiniz dile bağlı olarak sizin için doğru olanı kullanabilirsiniz:
* Örnek kodun ilk bölümü Azure Digital Twins .NET (C#) SDK 'SıNı kullanır. SDK, .NET için Azure SDK 'sının bir parçasıdır ve şurada bulunur: [*.net Için Azure IoT Digital ikizi istemci kitaplığı*](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core).
* Örnek kodun ikinci bölümü, .NET SDK kullanmayan kullanıcılara ve bunun yerine diğer dillerde oto Rest tarafından oluşturulan SDK 'ların kullanılmasına yöneliktir. Bu strateji hakkında daha fazla bilgi için bkz. [*nasıl yapılır: Azure dijital TWINS için özel SDK 'Lar oluşturma Ile oto Rest*](how-to-create-custom-sdks.md).

Azure dijital TWINS için API 'Ler ve SDK 'lar hakkında daha fazla bilgi için [*bkz. nasıl yapılır: Azure dijital TWINS API 'leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md).

## <a name="prerequisites"></a>Önkoşullar

İlk olarak, [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama*](how-to-set-up-instance-scripted.md)bölümünde kurulum adımlarını doldurun. Bu, bir Azure dijital TWINS örneğiniz olduğundan, kullanıcılarınızın erişim izinlerine sahip olduğundan ve istemci uygulamaları için izinler ayarlamış olduğunuzdan emin olur. Tüm bu kurulumdan sonra, istemci uygulama kodunu yazmaya hazırsınızdır.

Devam etmek için, kodunuzu yazdığınız bir istemci uygulaması projesine ihtiyacınız olacaktır. Önceden ayarlanmış bir istemci uygulama projeniz yoksa, bu öğreticide kullanmak üzere seçtiğiniz dilde temel bir proje oluşturun.

## <a name="authentication-and-client-creation-net-c-sdk"></a>Kimlik doğrulaması ve istemci oluşturma: .NET (C#) SDK

İlk olarak, .NET SDK ve kimlik doğrulama araçları 'nı bu nasıl yapılır için kullanmak üzere projenize aşağıdaki paketleri ekleyin:
* `Azure.DigitalTwins.Core` (sürüm `1.0.0-preview.2` )
* `Azure.Identity`

Seçtiğiniz araçlara bağlı olarak, Visual Studio Paket Yöneticisi 'ni veya komut satırı aracını kullanarak paketleri dahil edebilirsiniz `dotnet` . 

Ayrıca şu using deyimleri de gereklidir:

```csharp
using Azure.Identity;
using Azure.DigitalTwins.Core;
```
.NET SDK ile kimlik doğrulaması yapmak için, [Azure. Identity](https://docs.microsoft.com/dotnet/api/azure.identity?view=azure-dotnet) kitaplığında tanımlanmış kimlik bilgileri alma yöntemlerinden birini kullanın. Yaygın olarak kullanılan iki (aynı uygulamada bile):

* [Interactivebrowsercredential](https://docs.microsoft.com/dotnet/api/azure.identity.interactivebrowsercredential?view=azure-dotnet) etkileşimli uygulamalara yöneliktir ve kimliği DOĞRULANMıŞ bir SDK istemcisi oluşturmak için kullanılabilir
* [Managedıdentitycredential](https://docs.microsoft.com/dotnet/api/azure.identity.managedidentitycredential?view=azure-dotnet) , yönetilen KIMLIKLER (MSI) gerektiren durumlarda harika çalışır ve Azure işlevleri ile çalışmaya yönelik iyi bir adaydır.

### <a name="interactivebrowsercredential-method"></a>Interactivebrowsercredential yöntemi
[Interactivebrowsercredential](https://docs.microsoft.com/dotnet/api/azure.identity.interactivebrowsercredential?view=azure-dotnet) yöntemi etkileşimli uygulamalara yöneliktir ve kimlik doğrulaması için bir Web tarayıcısı getirir.

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

### <a name="managedidentitycredential-method"></a>Managedıdentitycredential yöntemi
 [Managedıdentitycredential](https://docs.microsoft.com/dotnet/api/azure.identity.managedidentitycredential?view=azure-dotnet) yöntemi, [YÖNETILEN kimliklere (MSI)](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)ihtiyacınız olan durumlarda (örneğin, Azure işlevleri ile çalışırken) harika bir şekilde çalışır.
Bir Azure işlevinde, aşağıdaki gibi yönetilen kimlik bilgilerini kullanabilirsiniz:

```csharp
ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
DigitalTwinsClientOptions opts = 
    new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, opts);
```

Bkz. nasıl yapılır: işlev bağlamında önemli yapılandırma Seçimlerinizden bazılarını açıklayan daha tam bir örnek için [*verileri işlemek üzere bir Azure Işlevi ayarlama*](how-to-create-azure-function.md) .

Ayrıca, bir işlevde kimlik doğrulamasını kullanmak için şunları unutmayın:
* [Yönetilen kimliği etkinleştirme](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet)
* [Ortam değişkenlerini](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables?tabs=csharp) uygun şekilde kullanın
* İşlevler uygulamasına, dijital TWINS API 'Lerine erişmesini sağlayan izinler atayın. Azure Işlevleri işlemleri hakkında daha fazla bilgi için bkz. [*nasıl yapılır: verileri işlemek için bir Azure Işlevi ayarlama*](how-to-create-azure-function.md).

## <a name="authentication-with-an-autorest-generated-sdk"></a>Bir oto Rest tarafından oluşturulan SDK ile kimlik doğrulama

.NET kullanmıyorsanız, [*nasıl yapılır: Azure dijital TWINS için özel SDK 'Lar oluşturma*](how-to-create-custom-sdks.md)bölümünde açıklandığı gibi, tercih ettiğiniz BIR dilde SDK kitaplığı oluşturmayı tercih edebilirsiniz.

Bu bölümde, bu durumda nasıl kimlik doğrulaması yapılacağı açıklanmaktadır.

### <a name="prerequisites"></a>Önkoşullar

İlk olarak, [*nasıl yapılır: Azure dijital TWINS için özel*](how-to-create-custom-sdks.md)SDK 'lar oluşturma adımlarını kullanarak, oto Rest ile özel SDK oluşturma adımlarını tamamlamalısınız.

Bu örnek, oto Rest ile oluşturulan bir TypeScript SDK kullanır. Sonuç olarak, ayrıca şunları gerektirir:
* [msal-js](https://github.com/AzureAD/microsoft-authentication-library-for-js)
* [MS-Rest-js](https://github.com/Azure/ms-rest-js)

### <a name="minimal-authentication-code-sample"></a>En küçük kimlik doğrulama kodu örneği

Azure hizmetleriyle bir uygulamanın kimliğini doğrulamak için, istemci uygulamanızda aşağıdaki en az kodu kullanabilirsiniz.

*Uygulamanızın (istemci) kimliği* ve *Dizin (kiracı) kimliğiniz* Ile Azure DIJITAL TWINS örneğinizin URL 'sini de kullanmanız gerekecektir.

> [!TIP]
> Azure Digital TWINS örneğinin URL 'SI, Azure Digital TWINS örneğinizin *ana bilgisayar adının*başlangıcına *https://* eklenerek yapılır. *Ana bilgisayar adını*görmek için, örneğinizin tüm özellikleriyle birlikte çalıştırabilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` . `az account show --query tenantId` *Dizin (KIRACı) kimliğinizi*görmek için komutunu kullanabilirsiniz. 

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

MSAL, önbelleğe alma ve diğer kimlik doğrulama akışları gibi işlemleri uygulamak için kullanabileceğiniz birçok daha fazla seçenek içerir. Bu konuda daha fazla bilgi için bkz. [*Microsoft kimlik doğrulama kitaplığı 'Na genel bakış (msal)*](../active-directory/develop/msal-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS 'de güvenliğin nasıl çalıştığı hakkında daha fazla bilgi edinin:
* [*Kavramlar: Azure dijital TWINS çözümleri için güvenlik*](concepts-security.md)

Ya da artık kimlik doğrulaması ayarlanmış ise, örneğiniz içinde modeller oluşturmak için üzerine geçin:
* [*Nasıl yapılır: özel modelleri yönetme*](how-to-manage-model.md)