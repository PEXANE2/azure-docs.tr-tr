---
title: Azure Event Grid abonelikleri sorgula
description: Bu makalede, Azure aboneliğinizde Event Grid aboneliklerinin nasıl listeleneceğini açıklanmaktadır. Abonelik türüne göre farklı parametreler sağlarsınız.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 2da4ba5946f16092c1d0918aec8dc3109b5fd4a8
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721554"
---
# <a name="query-event-grid-subscriptions"></a>Event Grid abonelikleri sorgula 

Bu makalede, Azure aboneliğinizdeki Event Grid aboneliklerinin nasıl listeleneceğini açıklanmaktadır. Mevcut Event Grid aboneliklerinizi sorgularken, farklı abonelik türlerinin anlaşılması önemlidir. Almak istediğiniz abonelik türüne göre farklı parametreler sağlarsınız.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="resource-groups-and-azure-subscriptions"></a>Kaynak grupları ve Azure abonelikleri

Azure abonelikleri ve kaynak grupları Azure kaynakları değildir. Bu nedenle, kaynak gruplarına veya Azure aboneliklerine yönelik olay Kılavuzu abonelikleri, Azure kaynaklarına yönelik olay Kılavuzu abonelikleriyle aynı özelliklere sahip değildir. Kaynak gruplarına veya Azure aboneliklerine yönelik olay Kılavuzu abonelikleri genel olarak değerlendirilir.

Bir Azure aboneliği ve kaynak grupları için Event Grid abonelikleri almak için herhangi bir parametre sağlamanız gerekmez. Sorgulamak istediğiniz Azure aboneliğini seçtiğinizden emin olun. Aşağıdaki örnekler, özel konular veya Azure kaynakları için Event Grid abonelikleri almaz.

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

Bir Azure aboneliğine yönelik olay Kılavuzu abonelikleri almak için **Microsoft. resources. aboneliklerinin**konu türünü belirtin.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.Subscriptions" --location global
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.Resources.Subscriptions"
```

Bir Azure aboneliği içindeki tüm kaynak grupları için Event Grid abonelikleri almak için **Microsoft. resources. ResourceGroups**konu türünü belirtin.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.ResourceGroups" --location global
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.Resources.ResourceGroups"
```

Belirtilen bir kaynak grubuna yönelik olay Kılavuzu aboneliklerini almak için, kaynak grubunun adını parametre olarak belirtin.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --resource-group myResourceGroup --location global
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -ResourceGroupName myResourceGroup
```

## <a name="custom-topics-and-azure-resources"></a>Özel konular ve Azure kaynakları

Olay Kılavuzu özel konuları Azure kaynaklarıdır. Bu nedenle, özel konular ve aynı şekilde BLOB depolama hesabı gibi diğer kaynaklar için olay Kılavuzu aboneliklerini sorgulayın. Özel konularda Event Grid abonelikleri almak için kaynağı tanımlayan veya kaynağın konumunu tanımlayan parametreleri sağlamanız gerekir. Azure aboneliğinizdeki kaynaklar için olay Kılavuzu aboneliklerini büyük ölçüde sorgulamak mümkün değildir.

Özel konular ve bir konumdaki diğer kaynaklar için Event Grid abonelikleri almak üzere konumun adını belirtin.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --location westus2
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -Location westus2
```

Bir konum için özel konulara abonelikleri almak üzere **Microsoft. EventGrid.** topic konumunu ve konu türünü belirtin.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.EventGrid.Topics" --location "westus2"
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.EventGrid.Topics" -Location westus2
```

Bir konum için depolama hesaplarına abonelikler almak üzere **Microsoft. Storage. StorageAccounts**konumunu ve konu türünü belirtin.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-type "Microsoft.Storage.StorageAccounts" --location westus2
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicTypeName "Microsoft.Storage.StorageAccounts" -Location westus2
```

Özel bir konuya yönelik olay Kılavuzu abonelikleri almak için, özel konunun adını ve kaynak grubunun adını belirtin.

Azure CLI için şunu kullanın:

```azurecli-interactive
az eventgrid event-subscription list --topic-name myCustomTopic --resource-group myResourceGroup
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
Get-AzEventGridSubscription -TopicName myCustomTopic -ResourceGroupName myResourceGroup
```

Belirli bir kaynak için Event Grid abonelikleri almak için kaynak KIMLIĞINI sağlayın.

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

* Olay teslimi ve yeniden denemeler hakkında daha fazla bilgi için [Event Grid ileti teslimi ve yeniden deneyin](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid kullanmaya hızlıca başlamak için bkz. [özel olayları oluşturma ve Azure Event Grid ile yönlendirme](custom-event-quickstart.md).
