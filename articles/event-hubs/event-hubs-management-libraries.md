---
title: Yönetim kitaplıkları - Azure Etkinlik Hub'ları| Microsoft Dokümanlar
description: Bu makalede, .NET adresinden Azure Olay Hub'ları ad alanlarını ve varlıklarını yönetmek için kullanabileceğiniz kitaplık hakkında bilgiler sağlanmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60746667"
---
# <a name="event-hubs-management-libraries"></a>Event Hubs yönetim kitaplıkları

Olay Hub'ları ad alanlarını ve varlıklarını dinamik olarak sağlamak için Azure Etkinlik Hub'ları yönetim kitaplıklarını kullanabilirsiniz. Bu dinamik doğa, karmaşık dağıtımlara ve ileti senaryolarına olanak sağlar, böylece hangi varlıkların sağlanmasını programlı olarak belirleyebilirsiniz. Bu kitaplıklar şu anda .NET için kullanılabilir.

## <a name="supported-functionality"></a>Desteklenen işlevsellik

* Namespace oluşturma, güncelleştirme, silme
* Olay Hub'ları oluşturma, güncelleştirme, silme
* Tüketici Grubu oluşturma, güncelleme, silme

## <a name="prerequisites"></a>Ön koşullar

Olay Hub'ları yönetim kitaplıklarını kullanmaya başlamak için Azure Etkin Dizini (AAD) ile kimlik doğrulamanız gerekir. AAD, Azure kaynaklarına erişim sağlayan bir hizmet sorumlusu olarak kimlik doğrulamanızı gerektirir. Hizmet sorumlusu oluşturma hakkında bilgi için şu makalelerden birine bakın:  

* [Kaynaklara erişebilen Etkin Dizin uygulaması ve hizmet ilkesi oluşturmak için Azure portalını kullanın](../active-directory/develop/howto-create-service-principal-portal.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Bu öğreticiler, yönetim `AppId` kitaplıkları `TenantId`tarafından `ClientSecret` kimlik doğrulama için kullanılan bir (İstemci Kimliği) ve (kimlik doğrulama anahtarı) sağlar. Çalıştırmak istediğiniz kaynak grubu için **Sahip** izinlerine sahip olmalısınız.

## <a name="programming-pattern"></a>Programlama deseni

Herhangi bir Olay Hub'ı kaynağını işlemek için desen ortak bir protokol izler:

1. `Microsoft.IdentityModel.Clients.ActiveDirectory` Kitaplığı kullanarak AAD'den bir belirteç edinin.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Nesneyi `EventHubManagementClient` oluşturun.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Parametreleri `CreateOrUpdate` belirtilen değerlere ayarlayın.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Aramayı yürütün.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [.NET Yönetim örneği](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub Başvurusu](/dotnet/api/Microsoft.Azure.Management.EventHub) 
