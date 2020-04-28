---
title: Azure'da windows vm kaynağını taşıma
description: Kaynak Yöneticisi dağıtım modelinde bir Windows VM'yi başka bir Azure aboneliğine veya kaynak grubuna taşıyın.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 07/03/2019
ms.author: cynthn
ms.openlocfilehash: 0c3f5541405d1fd983bbf988b99d2b4e10d8908c
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81865654"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Bir Windows sanal makinesini başka bir Azure aboneliğine veya kaynak grubuna taşıma
Bu makalede, kaynak grupları veya abonelikler arasında bir Windows sanal makine (VM) taşımak için nasıl size yol. Kişisel bir abonelikte bir VM oluşturduysanız ve şimdi işinize devam etmek için şirketinizin aboneliğine taşımak istiyorsanız, abonelikler arasında taşıma kullanışlı olabilir. Taşımak için VM'yi başlatmanız gerekmez ve taşıma sırasında çalışmaya devam etmelidir.

> [!IMPORTANT]
>Yeni kaynak teşekkler hareketin bir parçası olarak oluşturulur. VM taşındıktan sonra, yeni kaynak tanımlarını kullanmak için araçlarınızı ve komut dosyalarınızı güncelleştirmeniz gerekir.
>
>

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>VM'yi taşımak için Powershell'i kullanma

Sanal bir makineyi başka bir kaynak grubuna taşımak için, bağımlı kaynakların tümünün de taşınmasından emin olmanız gerekir. Bu kaynakların her birinin kaynak kimliğini içeren bir liste almak için [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) cmdlet'ini kullanın.

```azurepowershell-interactive
 Get-AzResource -ResourceGroupName myResourceGroup | Format-table -wrap -Property ResourceId
```

Her kaynağı hedefe taşımak için [Move-AzResource'a](https://docs.microsoft.com/powershell/module/az.resources/move-azresource) virgülle ayrılmış kaynak iI'lerinin listesini oluşturmak için önceki komutun çıktısını kullanabilirsiniz.

```azurepowershell-interactive
Move-AzResource -DestinationResourceGroupName "myDestinationResourceGroup" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```

Kaynakları farklı aboneliğe taşımak için **-DestinationSubscriptionId** parametresini ekleyin.

```azurepowershell-interactive
Move-AzResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


Belirtilen kaynakları taşımak istediğinizi onaylamanız istendiğinde, onaylamak için **Y** girin.

## <a name="next-steps"></a>Sonraki adımlar
Kaynak grupları ve abonelikler arasında birçok farklı kaynak türü taşıyabilirsiniz. Daha fazla bilgi için bkz. kaynakları [yeni bir kaynak grubuna veya aboneye taşı.](../../azure-resource-manager/management/move-resource-group-and-subscription.md)    
