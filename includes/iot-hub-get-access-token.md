---
author: robinsh
ms.author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 7f7dc1483002c2bdfe3227a8aade8dbf2a8da417
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70803015"
---
## <a name="obtain-an-azure-resource-manager-token"></a>Azure Kaynak Yöneticisi belirteci edinin
Azure Etkin Dizin, Azure Kaynak Yöneticisi'ni kullanarak kaynaklarda gerçekleştirdiğiniz tüm görevlerin doğruluğunu doğrulamalıdır. Burada gösterilen örnekte parola kimlik doğrulaması kullanır, diğer yaklaşımlar için [Azure Kaynak Yöneticisi isteklerini doğrulaması'na][lnk-authenticate-arm]bakın.

1. Uygulama kimliği ve parolasını kullanarak Azure AD'den bir belirteç almak için **Program.cs'daki Ana** yönteme aşağıdaki kodu ekleyin.
   
    ```csharp
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```
2. **Ana** yöntemin sonuna aşağıdaki kodu ekleyerek belirteci kullanan bir **Kaynak Yönetimiİste** nesnesi oluşturun:
   
    ```csharp
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Kullanmakta olduğunuz kaynak grubu oluşturmak veya bu kaynak grubuna başvuru almak:
   
    ```csharp
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx