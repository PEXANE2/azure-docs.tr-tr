---
title: Kaynak ekleme
description: Learn about performing resource onboarding by using Azure Custom Providers to apply management or configuration to other Azure resource types.
author: jjbfour
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: jobreen
ms.openlocfilehash: 1846b036f12fe7e691021ec0248782cad946d9b7
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75650415"
---
# <a name="azure-custom-providers-resource-onboarding-overview"></a>Azure Custom Providers resource onboarding overview

Azure Custom Providers resource onboarding is an extensibility model for Azure resource types. It allows you to apply operations or management across existing Azure resources at scale. For more information, see [How Azure Custom Providers can extend Azure](overview.md). Bu makalede açıklanır:

- What resource onboarding can do.
- Resource onboarding basics and how to use it.
- Where to find guides and code samples to get started.

> [!IMPORTANT]
> Custom Providers is currently in public preview.
> This preview version is provided without a service-level agreement, and we don't recommend it for production workloads. Certain features might be unsupported or might have constrained capabilities.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="what-can-resource-onboarding-do"></a>What can resource onboarding do?

Similar to [Azure Custom Providers custom resources](./custom-providers-resources-endpoint-how-to.md), resource onboarding defines a contract that will proxy "onboarding" requests to an endpoint. Unlike custom resources, resource onboarding doesn't create a new resource type. Instead, it allows the extension of existing resource types. And resource onboarding works with Azure Policy, so management and configuration of resources can be done at scale. Some examples of resource onboarding workflows:

- Install and manage onto virtual machine extensions.
- Upload and configure defaults on Azure storage accounts.
- Enable baseline diagnostic settings at scale.

## <a name="resource-onboarding-basics"></a>Resource onboarding basics

You configure resource onboarding through Azure Custom Providers by using Microsoft.CustomProviders/resourceProviders and Microsoft.CustomProviders/associations resource types. To enable resource onboarding for a custom provider, during the configuration process, create a **resourceType** called "associations" with a **routingType** that includes "Extension". The Microsoft.CustomProviders/associations and Microsoft.CustomProviders/resourceProviders don't need to belong to the same resource group.

Here's a sample Azure custom provider:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "associations",
        "routingType": "Proxy,Cache,Extension",
        "endpoint": "https://microsoft.com/"
      }
    ]
  },
  "location": "eastus"
}
```

Özellik | Gerekli mi? | Açıklama
---|---|---
ad | Evet | The name of the endpoint definition. For resource onboarding, the name must be "associations".
routingType | Evet | Determines the type of contract with the endpoint. For resource onboarding, the valid **routingTypes** are "Proxy,Cache,Extension" and "Webhook,Cache,Extension".
endpoint | Evet | The endpoint to route the requests to. This will handle the response and any side effects of the request.

After you create the custom provider with the associations resource type, you can target using Microsoft.CustomProviders/associations. Microsoft.CustomProviders/associations is an extension resource that can extend any other Azure resource. When an instance of Microsoft.CustomProviders/associations is created, it will take a property **targetResourceId**, which should be a valid Microsoft.CustomProviders/resourceProviders or Microsoft.Solutions/applications resource ID. In these cases, the request will be forwarded to the associations resource type on the Microsoft.CustomProviders/resourceProviders instance you created.

> [!NOTE]
> If a Microsoft.Solutions/applications resource ID is provided as the **targetResourceId**, there must be a Microsoft.CustomProviders/resourceProviders deployed in the managed resource group with the name "public".

Sample Azure Custom Providers association:

```JSON
{
  "properties": {
    "targetResourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
    ...
  }
}
```

Özellik | Gerekli mi? | Açıklama
---|---|---
targetResourceId | Evet | Microsoft. CustomProviders/resourceProviders veya Microsoft. Solutions/Applications kaynak KIMLIĞI.

## <a name="how-to-use-resource-onboarding"></a>Kaynak ekleme 'yi kullanma

Kaynak ekleme, Microsoft. CustomProviders/Association uzantı kaynağıyla diğer kaynakları genişleterek işe yarar. Aşağıdaki örnekte, istek bir sanal makine için yapılır, ancak herhangi bir kaynak genişletilebilir.

İlk olarak, bir ilişki kaynak türü ile özel bir sağlayıcı kaynağı oluşturmanız gerekir. Bu işlem, özel sağlayıcıyı hedefleyen karşılık gelen bir Microsoft. CustomProviders/Association kaynağı oluşturulduğunda kullanılacak geri çağırma URL 'sini bildirir.

Örnek Microsoft. CustomProviders/resourceProviders oluşturma isteği:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
  "properties": {
    "resourceTypes": [
      {
        "name": "associations",
        "routingType": "Proxy,Cache,Extension",
        "endpoint": "https://{myCustomEndpoint}/"
      }
    ]
  },
  "location": "{location}"
}
```

Özel sağlayıcıyı oluşturduktan sonra, diğer kaynakları hedefleyebilir ve özel sağlayıcının yan etkilerini bunlara uygulayabilirsiniz.

Örnek Microsoft. CustomProviders/Association oluşturma isteği:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}/providers/Microsoft.CustomProviders/associations/{associationName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
  "properties": {
    "targetResourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
  }
}
```

Daha sonra bu istek, bu formda **Targetresourceıd** tarafından başvurulan, oluşturduğunuz özel sağlayıcıda belirtilen uç noktaya iletilir:

``` HTTP
PUT https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/associations/{associationName}
X-MS-CustomProviders-ExtensionPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}/providers/Microsoft.CustomProviders/associations/{associationName}
X-MS-CustomProviders-ExtendedResource: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}

{
  "properties": {
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
  }
}
```

Uç noktanın bir uygulama/JSON `Content-Type` ve geçerli bir JSON yanıt gövdesi ile yanıt vermesi gerekir. JSON 'ın **Özellikler** nesnesi altında döndürülen alanlar ilişkilendirme dönüş yanıtına eklenecektir.

## <a name="getting-help"></a>Yardım alma

Azure özel kaynak sağlayıcıları geliştirme hakkında sorularınız varsa [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers)üzerinde soru sormayı deneyin. Benzer bir soru zaten yanıtlanmış olabilir, bu nedenle göndermeden önce kontrol edin. Hızlı bir yanıt almak için etiketi ```azure-custom-providers``` ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel sağlayıcılar hakkında bilgi edindiniz. Daha fazla bilgi için şu makalelere göz atın:

- [Öğretici: özel sağlayıcılarla kaynak ekleme](./tutorial-resource-onboarding.md)
- [Öğretici: Azure 'da özel eylemler ve kaynaklar oluşturma](./tutorial-get-started-with-custom-providers.md)
- [Hızlı başlangıç: özel kaynak sağlayıcısı oluşturma ve özel kaynaklar dağıtma](./create-custom-provider.md)
- [Nasıl yapılır: Azure REST API özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl yapılır: Azure REST API özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
