---
title: Azure PowerShell Betiği Örneği - Windows Sanal Makinesi NLB’si Oluşturma
description: Azure PowerShell Betiği Örneği - Windows Sanal Makinesi NLB’si Oluşturma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b7dad64d7c3e3ab7caa56db26719996b3519b6d5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082418"
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a>Yüksek oranda kullanılabilir sanal makineler arasında yük dengeleme trafiği

Bu betik örneği, yüksek oranda kullanılabilir ve yük dengeli bir yapılandırmada yapılandırılmış birkaç Windows Server 2016 sanal makinesini çalıştırmak için gereken her şeyi oluşturur. Betiği çalıştırdıktan sonra bir Azure Kullanılabilirlik Kümesine eklenmiş ve bir Azure Load Balancer üzerinden erişilebilen üç sanal makineniz olur.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Create VM NLB")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma, sanal ağ oluşturma işlemiyle birlikte kullanılır. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur. |
| [New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur. |
| [New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig) | Yük dengeleyici için bir ön uç IP yapılandırması oluşturur. |
| [New-Azloadbalancerbackendadddresspoolconfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) | Yük dengeleyici için bir arka uç adres havuzu yapılandırması oluşturur. |
| [New-AzLoadBalancerProbeConfig](/powershell/module/az.network/new-azloadbalancerprobeconfig) | Yük dengeleyici için bir araştırma yapılandırması oluşturur. |
| [New-AzLoadBalancerRuleConfig](/powershell/module/az.network/new-azloadbalancerruleconfig) | Yük dengeleyici için bir kural yapılandırması oluşturur. |
| [New-Azloadbalancerınboundnatrutaconfig](/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) | Yük dengeleyici için gelen bir NAT kuralı yapılandırması oluşturur. |
| [New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer) | Yük dengeleyici oluşturur. |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) | Ağ güvenlik grubu kuralı yapılandırması oluşturur. Bu yapılandırma, NSG oluşturulduğunda bir NSG kuralı oluşturmak için kullanılır. |
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Bir ağ güvenlik grubu oluşturur. |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Alt ağ bilgilerini alır. Bu bilgiler, bir ağ arabirimi oluşturulurken kullanılır. |
| [New-Aznetworkınterface](/powershell/module/az.network/new-aznetworkinterface) | Ağ arabirimi oluşturur. |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | Sanal makine yapılandırması oluşturur. Bu yapılandırma; sanal makine adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma, sanal makine oluşturulurken kullanılır. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Sanal makine oluşturur. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

Ayrıca, kendi özel yönetilen görüntünüzü kullanarak da VM 'Ler oluşturabilirsiniz. VM yapılandırmasında,, `Set-AzVMSourceImage` `-Id` `-VM` ve yerine ve parametrelerini kullanın `-PublisherName` `-Offer` `-Skus` `-Version` .

Örneğin, VM yapılandırması oluşturma şöyle olacaktır:

```powershell
$vmConfig = New-AzVMConfig -VMName 'myVM3' -VMSize Standard_DS1_v2 -AvailabilitySetId $as.Id | `
  Set-AzVMOperatingSystem -Windows -ComputerName 'myVM3' -Credential $cred | `
  Set-AzVMSourceImage -Id <Image.ID of the custom managed image> | Add-AzVMNetworkInterface -Id $nicVM3.Id
 ```

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
