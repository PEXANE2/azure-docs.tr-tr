---
title: Yönetim kitaplıkları-Azure Event Hubs | Microsoft Docs
description: Bu makalede, .NET Event Hubs ad alanlarını ve varlıklarını .NET 'ten yönetmek için kullanabileceğiniz kitaplık hakkında bilgi sağlanır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 79656d95cf925c21b0f0c5f2b3bafe6348e0b553
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87074827"
---
# <a name="event-hubs-management-libraries"></a>Event Hubs yönetim kitaplıkları

Event Hubs ad alanlarını ve varlıklarını dinamik olarak sağlamak için Azure Event Hubs yönetim kitaplıklarını kullanabilirsiniz. Bu dinamik yapı, karmaşık dağıtımlar ve mesajlaşma senaryolarına olanak sağladığından, sağlanacak varlıkları programlı bir şekilde tespit edebilirsiniz. Bu kitaplıklar Şu anda .NET için kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevsellik

* Ad alanı oluşturma, güncelleştirme, silme
* Event Hubs oluşturma, güncelleştirme, silme
* Tüketici grubu oluşturma, güncelleştirme, silme

## <a name="prerequisites"></a>Önkoşullar

Event Hubs yönetim kitaplıklarını kullanmaya başlamak için Azure Active Directory (AAD) ile kimlik doğrulaması yapmanız gerekir. AAD, Azure kaynaklarınıza erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulaması yapmanızı gerektirir. Hizmet sorumlusu oluşturma hakkında daha fazla bilgi için şu makalelerden birine bakın:  

* [Kaynaklara erişebilen Active Directory uygulama ve hizmet sorumlusu oluşturmak için Azure portal kullanın](../active-directory/develop/howto-create-service-principal-portal.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma](/cli/azure/create-an-azure-service-principal-azure-cli)

Bu öğreticiler size, `AppId` `TenantId` `ClientSecret` Yönetim kitaplıkları tarafından kimlik doğrulaması için kullanılan BIR (istemci kimliği), ve (kimlik doğrulama anahtarı) sağlar. Çalıştırmak istediğiniz kaynak grubu için **sahip** izinleriniz olmalıdır.

## <a name="programming-pattern"></a>Programlama stili

Herhangi bir Event Hubs kaynağını işlemek için kullanılan desenler ortak bir protokol izler:

1. Kitaplığı kullanarak AAD 'den bir belirteç alın `Microsoft.IdentityModel.Clients.ActiveDirectory` .
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Nesnesini oluşturun `EventHubManagementClient` .
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. `CreateOrUpdate`Parametreleri belirtilen değerlerinizle ayarlayın.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Çağrıyı yürütün.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [.NET Yönetim örneği](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft. Azure. Management. EventHub başvurusu](/dotnet/api/Microsoft.Azure.Management.EventHub) 
