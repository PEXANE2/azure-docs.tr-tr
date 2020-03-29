---
title: Azure'da windows vm kaynağını taşıma
description: Kaynak Yöneticisi dağıtım modelinde bir Windows VM'yi başka bir Azure aboneliğine veya kaynak grubuna taşıyın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 07/03/2019
ms.author: cynthn
ms.openlocfilehash: ed29c92d20a6b0d749ec44a22f42ec446ec58650
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919575"
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
