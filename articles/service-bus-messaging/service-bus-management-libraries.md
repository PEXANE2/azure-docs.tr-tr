---
title: Azure Hizmet Veri Mes'i yönetim kitaplıkları| Microsoft Dokümanlar
description: Bu makalede, Hizmet Veri Kurumu ad alanlarını ve varlıklarını dinamik olarak sağlamak için Azure Hizmet Veri Meskeni yönetim kitaplıklarının nasıl kullanılacağı açıklanmaktadır.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: d0e90d9278ede97de04ad8efeaa59d94a4567f66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76756275"
---
# <a name="service-bus-management-libraries"></a>Service Bus yönetim kitaplıkları

Azure Hizmet Veri Veri Mes'ü yönetim kitaplıkları, Hizmet Veri Servisi ad alanlarını ve varlıklarını dinamik olarak sağlayabilir. Bu, karmaşık dağıtımları ve ileti senaryolarını sağlar ve hangi varlıkların sağlanabilir olduğunu programlı olarak belirlemeyi mümkün kılar. Bu kitaplıklar şu anda .NET için kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevsellik

* Namespace oluşturma, güncelleştirme, silme
* Sıra oluşturma, güncelleştirme, silme
* Konu oluşturma, güncelleştirme, silme
* Abonelik oluşturma, güncelleştirme, silme

## <a name="prerequisites"></a>Ön koşullar

Hizmet Veri Mes'ü yönetim kitaplıklarını kullanmaya başlamak için Azure Etkin Dizin (Azure AD) hizmetiyle kimlik doğrulamanız gerekir. Azure AD, Azure kaynaklarına erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulamanızı gerektirir. Hizmet sorumlusu oluşturma hakkında bilgi için şu makalelerden birine bakın:  

* [Kaynaklara erişebilen Etkin Dizin uygulaması ve hizmet ilkesi oluşturmak için Azure portalını kullanın](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Bu öğreticiler, yönetim `AppId` kitaplıkları `TenantId`tarafından `ClientSecret` kimlik doğrulama için kullanılan bir (İstemci Kimliği) ve (kimlik doğrulama anahtarı) sağlar. Çalıştırmak istediğiniz kaynak grubu için **Sahip** izinlerine sahip olmalısınız.

## <a name="programming-pattern"></a>Programlama deseni

Herhangi bir Hizmet Veri Servisi kaynağını işlemek için desen ortak bir protokol izler:

1. **Microsoft.IdentityModel.Clients.ActiveDirectory** kitaplığını kullanarak Azure AD'den bir belirteç edinin:
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.azure.com/", new ClientCredential(clientId, clientSecret));
   ```
2. Nesneyi `ServiceBusManagementClient` oluşturun:

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. Parametreleri `CreateOrUpdate` belirtilen değerlere ayarlayın:

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. Aramayı yürüt:

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="complete-code-to-create-a-queue"></a>Sıra oluşturmak için kodu tamamla
Hizmet Veri Servisi kuyruğu oluşturmak için tam kod aşağıda veda edebilirsiniz: 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.Management.ServiceBus;
using Microsoft.Azure.Management.ServiceBus.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;

namespace SBusADApp
{
    class Program
    {
        static void Main(string[] args)
        {
            CreateQueue().GetAwaiter().GetResult();
        }

        private static async Task CreateQueue()
        {
            try
            {
                var subscriptionID = "<SUBSCRIPTION ID>";
                var resourceGroupName = "<RESOURCE GROUP NAME>";
                var namespaceName = "<SERVICE BUS NAMESPACE NAME>";
                var queueName = "<NAME OF QUEUE YOU WANT TO CREATE>";

                var token = await GetToken();

                var creds = new TokenCredentials(token);
                var sbClient = new ServiceBusManagementClient(creds)
                {
                    SubscriptionId = subscriptionID,
                };

                var queueParams = new SBQueue();

                Console.WriteLine("Creating queue...");
                await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, queueName, queueParams);
                Console.WriteLine("Created queue successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not create a queue...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

        private static async Task<string> GetToken()
        {
            try
            {
                var tenantId = "<AZURE AD TENANT ID>";
                var clientId = "<APPLICATION/CLIENT ID>";
                var clientSecret = "<CLIENT SECRET>";

                var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

                var result = await context.AcquireTokenAsync(
                    "https://management.azure.com/",
                    new ClientCredential(clientId, clientSecret)
                );

                // If the token isn't a valid string, throw an error.
                if (string.IsNullOrEmpty(result.AccessToken))
                {
                    throw new Exception("Token result is empty!");
                }

                return result.AccessToken;
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not get a token...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

    }
}
```

> [!IMPORTANT]
> Tam bir örnek için [GitHub'daki .NET yönetim örneğine](https://github.com/Azure-Samples/service-bus-dotnet-management/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft.Azure.Management.ServiceBus API başvurusu](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
