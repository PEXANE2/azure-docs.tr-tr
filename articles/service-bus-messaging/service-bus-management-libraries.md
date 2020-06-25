---
title: Yönetim kitaplıklarını Azure Service Bus | Microsoft Docs
description: Bu makalede, Service Bus ad alanlarını ve varlıklarını dinamik olarak sağlamak için Azure Service Bus yönetim kitaplıklarının nasıl kullanılacağı açıklanmaktadır.
ms.devlang: dotnet
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 042edcd1851f86dd2a660673bc87884b68410bfb
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85341713"
---
# <a name="service-bus-management-libraries"></a>Service Bus yönetim kitaplıkları

Azure Service Bus yönetim kitaplıkları, Service Bus ad alanlarını ve varlıklarını dinamik olarak sağlayabilir. Bu, karmaşık dağıtımlar ve mesajlaşma senaryolarına olanak tanır ve hangi varlıkların sağlanacağı ile programlı bir şekilde belirlenmesini olanaklı kılar. Bu kitaplıklar Şu anda .NET için kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevsellik

* Ad alanı oluşturma, güncelleştirme, silme
* Kuyruk oluşturma, güncelleştirme, silme
* Konu oluşturma, güncelleştirme, silme
* Abonelik oluşturma, güncelleştirme, silme

## <a name="prerequisites"></a>Ön koşullar

Service Bus yönetim kitaplıklarını kullanmaya başlamak için Azure Active Directory (Azure AD) hizmeti ile kimlik doğrulaması yapmanız gerekir. Azure AD, Azure kaynaklarınıza erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulaması yapmanızı gerektirir. Hizmet sorumlusu oluşturma hakkında daha fazla bilgi için şu makalelerden birine bakın:  

* [Kaynaklara erişebilen Active Directory uygulama ve hizmet sorumlusu oluşturmak için Azure portal kullanın](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Bu öğreticiler size, `AppId` `TenantId` `ClientSecret` Yönetim kitaplıkları tarafından kimlik doğrulaması için kullanılan BIR (istemci kimliği), ve (kimlik doğrulama anahtarı) sağlar. Çalıştırmak istediğiniz kaynak grubu için en az [**Azure Service Bus veri sahibi**](/azure/role-based-access-control/built-in-roles#azure-service-bus-data-owner) veya [**katkıda bulunan**](/azure/role-based-access-control/built-in-roles#contributor) izinlerinizin olması gerekir.

## <a name="programming-pattern"></a>Programlama stili

Herhangi bir Service Bus kaynağını işlemek için kullanılan desenler ortak bir protokol izler:

1. **Microsoft. IdentityModel. clients. ActiveDirectory** kitaplığını kullanarak Azure AD 'den bir belirteç edinin:
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.azure.com/", new ClientCredential(clientId, clientSecret));
   ```
2. Nesneyi oluşturun `ServiceBusManagementClient` :

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. `CreateOrUpdate`Parametreleri belirtilen değerlerinizle ayarlayın:

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. Çağrıyı yürütün:

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="complete-code-to-create-a-queue"></a>Kuyruk oluşturmak için kodu doldurun
Service Bus kuyruğu oluşturmak için kodun tamamı aşağıda verilmiştir: 

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
> Tüm örnek için [GitHub 'daki .NET Yönetim örneğine](https://github.com/Azure-Samples/service-bus-dotnet-management/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar
[Microsoft. Azure. Management. ServiceBus API başvurusu](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
