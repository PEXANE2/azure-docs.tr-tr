---
title: Azure CLI'yi kullanırken VM taşıma
description: Azure CLI'yi kullanarak bir VM'yi başka bir Azure aboneliğine veya kaynak grubuna taşıyın.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: ebcd5f166fd1876f67121787c23d23860c9fa4b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944598"
---
# <a name="move-a-vm-to-another-subscription-or-resource-group"></a>VM'yi başka bir aboneye veya kaynak grubuna taşıma
Bu makalede, kaynak grupları veya abonelikler arasında sanal bir makine (VM) taşımak için nasıl size yol. Kişisel bir abonelikte bir VM oluşturduysanız ve şimdi bunu şirketinizin aboneliğine taşımak istiyorsanız, bir VM'yi abonelikler arasında taşımak kullanışlı olabilir.

> [!IMPORTANT]
>Yeni kaynak teşekkler hareketin bir parçası olarak oluşturulur. VM taşındıktan sonra, yeni kaynak tanımlarını kullanmak için araçlarınızı ve komut dosyalarınızı güncelleştirmeniz gerekir.
>


## <a name="use-the-azure-cli-to-move-a-vm"></a>VM'yi taşımak için Azure CLI'yi kullanma


Azure CLI'yi kullanarak VM'nizi taşıyabilmeniz için önce kaynak ve hedef aboneliklerinin aynı kiracıiçinde bulunduğundan emin olmanız gerekir. Her iki aboneliğin de aynı kiracı kimliğine sahip olup olmadığını kontrol etmek için [az hesap göster'i](/cli/azure/account)kullanın.

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Kaynak ve hedef abonelikler için kiracı lı iT'ler aynı değilse, kaynakları yeni bir kiracıya taşımak için [desteğe](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) başvurmanız gerekir.

Bir VM'yi başarıyla taşımak için VM'yi ve tüm destekleyici kaynaklarını taşımanız gerekir. Kaynak grubundaki tüm kaynakları ve kimliklerini listelemek için [az kaynak listesi](/cli/azure/resource) komutunu kullanın. Bu komutun çıktısını bir dosyaya boruyla aktararak, daha sonraki komutlara kopyalayıp yapıştırabilirsiniz.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```

VM ve kaynaklarını başka bir kaynak grubuna taşımak için [az kaynak taşımasını](/cli/azure/resource)kullanın. Aşağıdaki örnek, bir VM'nin nasıl taşındığını ve gerektirdiği en yaygın kaynakları gösterir. **-ids** parametresini kullanın ve kaynakların taşınması için kimliklerin virgülle ayrılmış bir listesinde (boşluk suz) geçirin.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

VM'yi ve kaynaklarını farklı bir aboneliğe taşımak istiyorsanız, hedef aboneliğini belirtmek için **hedef-abonelikId** parametresini ekleyin.

Belirtilen kaynakları taşımak istediğinizi onaylamanız istendiğinde, onaylamak için **Y** girin.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Sonraki adımlar
Kaynak grupları ve abonelikler arasında birçok farklı kaynak türü taşıyabilirsiniz. Daha fazla bilgi için bkz. kaynakları [yeni bir kaynak grubuna veya aboneye taşı.](../../azure-resource-manager/management/move-resource-group-and-subscription.md)    
