---
title: Sanal makine ölçek kümesini mevcut bir Azure Load Balancer Yapılandırma Azure PowerShell
description: Bir sanal makine ölçek kümesini mevcut bir Azure Load Balancer nasıl yapılandıracağınızı öğrenin.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: how-to
ms.date: 03/26/2020
ms.openlocfilehash: 919883025075ca38bcef6c23ac744528e7bd5502
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88510071"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-azure-powershell"></a>Azure PowerShell kullanarak var olan bir Azure Load Balancer sanal makine ölçek kümesi yapılandırma

Bu makalede, bir sanal makine ölçek kümesini mevcut bir Azure Load Balancer nasıl yapılandıracağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği.
- Abonelikte sanal makine ölçek kümesinin dağıtılacağı mevcut bir standart SKU yük dengeleyici.
- Sanal makine ölçek kümesi için bir Azure sanal ağı.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma

Azure 'da oturum açın.

```azurepowershell-interactive
Connect-AzAccount
```

## <a name="deploy-a-virtual-machine-scale-set-with-existing-load-balancer"></a>Mevcut yük dengeleyiciye sahip bir sanal makine ölçek kümesi dağıtma

Köşeli ayraçlar içindeki değerleri, yapılandırmanızda bulunan kaynakların adlarıyla değiştirin.

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

Aşağıdaki örnek, ile bir sanal makine ölçek kümesi dağıtır:

- **Myvmss** adlı sanal makine ölçek kümesi
- **Myloadbalancer** adlı Azure Load Balancer
- **Mybackendpool** adlı yük dengeleyici arka uç havuzu
- **Myvnet** adlı Azure sanal ağı
- **Mysubnet** adlı alt ağ
- **Myresourcegroup** adlı kaynak grubu

```azurepowershell-interactive

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
> Ölçek kümesi oluşturulduktan sonra, yük dengeleyicinin bir sistem durumu araştırması tarafından kullanılan bir yük dengeleme kuralı için arka uç bağlantı noktası değiştirilemez. Bağlantı noktasını değiştirmek için Azure sanal makine ölçek kümesini güncelleştirerek sistem durumu araştırmasını kaldırabilir, bağlantı noktasını güncelleştirebilir ve ardından sistem durumu araştırmasını yeniden yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, mevcut bir Azure Load Balancer bir sanal makine ölçek kümesi dağıttınız.  Sanal Makine Ölçek Kümeleri ve yük dengeleyici hakkında daha fazla bilgi edinmek için bkz.:

- [Azure Load Balancer nedir?](load-balancer-overview.md)
- [Sanal makine ölçek kümeleri nedir?](../virtual-machine-scale-sets/overview.md)
