---
title: Yönetim kitaplıkları-Azure Event Hubs | Microsoft Docs
description: Bu makalede, .NET Event Hubs ad alanlarını ve varlıklarını .NET 'ten yönetmek için kullanabileceğiniz kitaplık hakkında bilgi sağlanır.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 431fe04461f422274697d1e91c4b56e914ce2d4e
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "60746667"
---
# <a name="event-hubs-management-libraries"></a>Event Hubs yönetim kitaplıkları

Event Hubs ad alanlarını ve varlıklarını dinamik olarak sağlamak için Azure Event Hubs yönetim kitaplıklarını kullanabilirsiniz. Bu dinamik yapı, karmaşık dağıtımlar ve mesajlaşma senaryolarına olanak sağladığından, sağlanacak varlıkları programlı bir şekilde tespit edebilirsiniz. Bu kitaplıklar Şu anda .NET için kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevsellik

* Ad alanı oluşturma, güncelleştirme, silme
* Event Hubs oluşturma, güncelleştirme, silme
* Tüketici grubu oluşturma, güncelleştirme, silme

## <a name="prerequisites"></a>Ön koşullar

Event Hubs yönetim kitaplıklarını kullanmaya başlamak için Azure Active Directory (AAD) ile kimlik doğrulaması yapmanız gerekir. AAD, Azure kaynaklarınıza erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulaması yapmanızı gerektirir. Hizmet sorumlusu oluşturma hakkında daha fazla bilgi için şu makalelerden birine bakın:  

* [Kaynaklara erişebilen Active Directory uygulama ve hizmet sorumlusu oluşturmak için Azure portal kullanın](../active-directory/develop/howto-create-service-principal-portal.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Bu öğreticiler size, yönetim kitaplıkları `AppId` tarafından kimlik doğrulaması için kullanılan `TenantId`bir ( `ClientSecret` istemci kimliği), ve (kimlik doğrulama anahtarı) sağlar. Çalıştırmak istediğiniz kaynak grubu için **sahip** izinleriniz olmalıdır.

## <a name="programming-pattern"></a>Programlama stili

Herhangi bir Event Hubs kaynağını işlemek için kullanılan desenler ortak bir protokol izler:

1. `Microsoft.IdentityModel.Clients.ActiveDirectory` KITAPLıĞı kullanarak AAD 'den bir belirteç alın.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. `EventHubManagementClient` Nesnesini oluşturun.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. `CreateOrUpdate` Parametreleri belirtilen değerlerinizle ayarlayın.
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
