---
title: Mevcut bir Azure Yük Bakiyesi ile sanal makine ölçeği ayarlarını yapılandırın - Azure PowerShell
description: Varolan bir Azure Yük Dengeleyicisi ile sanal makine ölçeği kümesini nasıl yapılandıracağımız öğrenin.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: article
ms.date: 03/26/2020
ms.openlocfilehash: 0db09083a2197ce72e6d6eed2381b0308239586e
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349986"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-azure-powershell"></a>Azure PowerShell'i kullanarak mevcut bir Azure Yük Bakiyeleyicisiyle sanal makine ölçeği kümesini yapılandırın

Bu makalede, varolan bir Azure Yük Dengeleyicisi ile ayarlanmış sanal makine ölçeğini nasıl yapılandıracağınız öğrenilir. 

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği.
- Sanal makine ölçeği kümesinin dağıtılanacağı abonelikteki mevcut standart sku yük dengeleyicisi.
- Sanal makine ölçeği kümesi için bir Azure Sanal Ağı.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma

Azure'da oturum açın.

```azurepowershell-interactive
Connect-AzAccount
```

## <a name="deploy-a-virtual-machine-scale-set-with-existing-load-balancer"></a>Mevcut yük dengeleyicisi ile ayarlanmış sanal makine ölçeğini dağıtma

Parantez içinde değerleri yapılandırmanızdaki kaynakların adlarıyla değiştirin.

```azurepowershell-interactive

$rsg = <resource-group>
$loc = <location>
$vms = <vm-scale-set-name>
$vnt = <virtual-network>
$sub = <subnet-name>
$lbn = <load-balancer-name>
$pol = <upgrade-policy-mode>

$lb = Get-AzLoadBalancer -ResourceGroupName $rsg -Name $lbn

New-AzVmss -ResourceGroupName $rsg -Location $loc -VMScaleSetName $vms -VirtualNetworkName $vnt -SubnetName $sub -LoadBalancerName $lb -UpgradePolicyMode $pol

```

Aşağıdaki örnekte, aşağıdakilerle ayarlanmış sanal bir makine ölçeği dağıtılır:

- **MyVMSS** adlı sanal makine ölçeği kümesi
- Azure Yük **DengeleyicimyLoadBalancer** adlı
- **MyBackendPool** adlı yük dengeleyici arka uç havuzu
- Azure Sanal Ağı **myVnet** adlı
- Subnet **mySubnet** adlı
- **myResourceGroup** adlı kaynak grubu

```azureppowershell-interactive

$rsg = "myResourceGroup"
$loc = "East US 2"
$vms = "myVMSS"
$vnt = "myVnet"
$sub = "mySubnet"
$pol = "Automatic"
$lbn = "myLoadBalancer"

$lb = Get-AzLoadBalancer -ResourceGroupName $rsg -Name $lbn

New-AzVmss -ResourceGroupName $rsg -Location $loc -VMScaleSetName $vms -VirtualNetworkName $vnt -SubnetName $sub -LoadBalancerName $lb -UpgradePolicyMode $pol
```
> [!NOTE]
> Ölçek kümesi oluşturulduktan sonra, arka uç bağlantı noktası yük dengeleyicisinin sistem durumu sondası tarafından kullanılan bir yük dengeleme kuralı için değiştirilemez. Bağlantı noktasını değiştirmek için, Azure sanal makine ölçeği kümesini güncelleştirerek sistem durumu sondasını kaldırabilir, bağlantı noktasını güncelleyebilir ve sistem durumu sondasını yeniden yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, varolan bir Azure Yük Dengeleyicisi ile ayarlanmış sanal bir makine ölçeği dağıttınız.  Sanal makine ölçek kümeleri ve yük dengeleyicisi hakkında daha fazla bilgi edinmek için bkz:

- [Azure Load Balancer nedir?](load-balancer-overview.md)
- [Sanal makine ölçek kümeleri nedir?](../virtual-machine-scale-sets/overview.md)
                                