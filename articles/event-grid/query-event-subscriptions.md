---
title: Azure Olay Ağı aboneliklerini sorgula
description: Bu makalede, Azure aboneliğinizde Olay Ağı aboneliklerinin nasıl listelenebildiğini açıklanmaktadır. Abonelik türüne bağlı olarak farklı parametreler sağlarsınız.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 2da4ba5946f16092c1d0918aec8dc3109b5fd4a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721554"
---
# <a name="query-event-grid-subscriptions"></a>Olay Izgara aboneliklerini sorgula 

Bu makalede, Azure aboneliğinizdeki Olay Ağı abonelikleri nasıl listelenir. Varolan Olay Ağı aboneliklerinizi sorgularken, farklı abonelik türlerini anlamak önemlidir. Almak istediğiniz abonelik türüne bağlı olarak farklı parametreler sağlarsınız.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="resource-groups-and-azure-subscriptions"></a>Kaynak grupları ve Azure abonelikleri

Azure abonelikleri ve kaynak grupları Azure kaynakları değildir. Bu nedenle, kaynak gruplarına veya Azure aboneliklerine yapılan olay ızgara sıyrık abonelikleri, Azure kaynaklarına yapılan olay ızgara abonelikleri ile aynı özelliklere sahip değildir. Kaynak gruplarına veya Azure aboneliklerine ait olay ızgarası abonelikleri genel olarak kabul edilir.

Azure aboneliği ve kaynak grupları için olay ızgarası abonelikleri almak için herhangi bir parametre sağlamanız gerekmez. Sorgulamak istediğiniz Azure aboneliğini seçtiğinizden emin olun. Aşağıdaki örnekler, özel konular veya Azure kaynakları için olay ızgarası abonelikleri almamaktadır.

Azure CLI için şunu kullanın:

```azurecli-interactive
az account set -s "My Azure Subscription"
az eventgrid event-subscription list
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Set-AzContext -Subscription "My Azure Subscription"
Get-AzEventGridSubscription
```

Azure aboneliği için olay ızgarası abonelikleri almak için **Microsoft.Resources.Subscriptions**konu türünü sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.Subscriptions" --location global
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.Resources.Subscriptions"
```

Azure aboneliğindeki tüm kaynak grupları için olay ızgarası abonelikleri almak için **Microsoft.Resources.ResourceGroups**konu türünü sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.ResourceGroups" --location global
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.Resources.ResourceGroups"
```

Belirli bir kaynak grubu için olay ızgarası abonelikleri almak için, kaynak grubunun adını parametre olarak sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --resource-group myResourceGroup --location global
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -ResourceGroupName myResourceGroup
```

## <a name="custom-topics-and-azure-resources"></a>Özel konular ve Azure kaynakları

Olay ızgarası özel konuları Azure kaynaklarıdır. Bu nedenle, blob depolama hesabı gibi özel konular ve diğer kaynaklar için olay ızgara aboneliklerini aynı şekilde sorgularsınız. Özel konular için olay ızgarası abonelikleri almak için, kaynağı tanımlayan veya kaynağın konumunu tanımlayan parametreler sağlamanız gerekir. Azure aboneliğinizdeki kaynaklar için olay ızgara aboneliklerini geniş bir şekilde sorgulamak mümkün değildir.

Bir konumdaki özel konular ve diğer kaynaklar için olay ızgarası abonelikleri almak için, konumun adını sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --location westus2
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -Location westus2
```

Bir konum için özel konulara abonelik almak için, **Microsoft.EventGrid.Topics'in**konumunu ve konu türünü sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.EventGrid.Topics" --location "westus2"
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.EventGrid.Topics" -Location westus2
```

Bir konumiçin depolama hesaplarına abonelik almak için, **Microsoft.Storage.StorageAccounts'un**konumunu ve konu türünü sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type "Microsoft.Storage.StorageAccounts" --location westus2
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.Storage.StorageAccounts" -Location westus2
```

Özel bir konu için olay ızgarası abonelikleri almak için, özel konunun adını ve kaynak grubunun adını sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-name myCustomTopic --resource-group myResourceGroup
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicName myCustomTopic -ResourceGroupName myResourceGroup
```

Belirli bir kaynak için olay ızgarası abonelikleri almak için kaynak kimliğini sağlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
resourceid=$(az resource show -n mystorage -g myResourceGroup --resource-type "Microsoft.Storage/storageaccounts" --query id --output tsv)
az eventgrid event-subscription list --resource-id $resourceid
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
$resourceid = (Get-AzResource -Name mystorage -ResourceGroupName myResourceGroup).ResourceId
Get-AzEventGridSubscription -ResourceId $resourceid
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeleri hakkında bilgi için, [Olay Grid ileti teslimi ve yeniden deneme.](delivery-and-retry.md)
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Olay Ağıt'ı kullanmaya hızla başlamak için [Azure Olay Ağıtı ile özel etkinlikler oluştur ve yönlendir'e](custom-event-quickstart.md)bakın.
