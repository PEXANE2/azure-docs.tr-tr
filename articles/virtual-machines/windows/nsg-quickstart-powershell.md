---
title: Azure PowerShell kullanarak bağlantı noktalarını VM'ye açma
description: Azure kaynak yöneticisi dağıtım modunu ve Azure PowerShell'i kullanarak windows vm'inizin bağlantı noktasını nasıl açacağınızı / bir bitiş noktası oluşturup kurabilirsiniz öğrenin
author: cynthn
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: a0dcc53d84edb4dd697213106c02626df24acfd8
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869407"
---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>PowerShell'i kullanarak Azure'da bir VM bağlantı noktası ve uç noktaları nasıl açılır?
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Hızlı komutlar
Ağ Güvenlik Grubu ve ACL kuralları oluşturmak için [Azure PowerShell'in en son sürümünün yüklü olması](/powershell/azureps-cmdlets-docs)gerekir. [Bu adımları Azure portalını kullanarak](nsg-quickstart-portal.md)da gerçekleştirebilirsiniz.

Azure hesabınızda oturum açın:

```powershell
Connect-AzAccount
```

Aşağıdaki örneklerde, parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları *myResourceGroup,* *myNetworkSecurityGroup*ve *myVnet'i*içeriyordu.

[New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig)ile bir kural oluşturun. Aşağıdaki örnek, *80*no'l'da *tcp* trafiğine izin vermek için *myNetworkSecurityGroupRule* adında bir kural oluşturur:

```powershell
$httprule = New-AzNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

Ardından, [Yeni-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup) ile Ağ Güvenliği grubunuzu oluşturun ve oluşturduğunuz HTTP kuralını aşağıdaki gibi atayın. Aşağıdaki örnek, *myNetworkSecurityGroup*adında bir Ağ Güvenlik Grubu oluşturur:

```powershell
$nsg = New-AzNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

Şimdi Ağ Güvenlik Grubunuzu bir alt ağa atayalım. Aşağıdaki örnek, [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork) *ile* $vnet değişkenine *myVnet* adlı mevcut bir sanal ağ atar:

```powershell
$vnet = Get-AzVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Ağ Güvenlik Grubunuzu alt ağınızla [Set-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetworksubnetconfig)ile ilişkilendirin. Aşağıdaki örnek, *mySubnet* adlı alt ağı Ağ Güvenlik Grubunuzla ilişkilendirer:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Son olarak, değişikliklerinizin etkili olması için sanal ağınızı [Set-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetwork) ile güncelleyin:

```powershell
Set-AzVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Ağ Güvenlik Grupları hakkında daha fazla bilgi
Buradaki hızlı komutlar, VM'nize akan trafikle çalışmaya devam etmenizi sağlar. Ağ Güvenlik Grupları, kaynaklarınıza erişimi denetlemek için birçok harika özellik ve ayrıntılılık sağlar. [Ağ Güvenlik Grubu ve ACL kuralları oluşturma](tutorial-virtual-network.md#secure-network-traffic)hakkında daha fazla bilgi buradan edinebilirsiniz.

Yüksek kullanılabilir web uygulamaları için, VM'lerinizi bir Azure Yük Dengeleyicisi'nin arkasına yerleştirmelisiniz. Yük dengeleyicisi, trafik filtreleme sağlayan bir Ağ Güvenlik Grubu ile Trafiği VM'lere dağıtır. Daha fazla bilgi için, [yüksek kullanılabilir bir uygulama oluşturmak için Azure'da Linux sanal makinelerinin bakiyesini nasıl yükleyin.](tutorial-load-balancer.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, HTTP trafiğine izin vermek için basit bir kural oluşturdunuz. Aşağıdaki makalelerde daha ayrıntılı ortamlar oluşturma hakkında bilgi bulabilirsiniz:

* [Azure Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md)
* [Ağ güvenlik grubu nedir?](../../virtual-network/security-overview.md)
* [Azure Yük Dengeleyicisi Genel Bakış](../../load-balancer/load-balancer-overview.md)

