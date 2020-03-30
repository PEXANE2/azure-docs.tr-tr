---
title: Azure PowerShell betiği örneği - Ağ sanal gereci aracılığıyla trafiği yönlendirme | Microsoft Docs
description: Azure PowerShell betiği örneği - Güvenlik duvarı ağ sanal gereci aracılığıyla trafiği yönlendirme.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: kumud
ms.openlocfilehash: b31714e62ba0e7a0e4882d9b9fa830b3a14fe855
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73888519"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Bir ağ sanal gereci yoluyla trafiği yönlendirme

Bu betik örneği, ön uç ve arka uç alt ağları ile sanal ağ oluşturur. Ayrıca iki alt ağ arasında trafiği yönlendirmek için IP iletme etkinleştirilmiş şekilde bir sanal makine de oluşturur. Betiği çalıştırdıktan sonra, sanal makineye güvenlik duvarı uygulaması gibi ağ yazılımı dağıtabilirsiniz.

Gerekirse, [Azure PowerShell kılavuzunda](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)bulunan yönergeyi kullanarak Azure PowerShell'i yükleyin ve ardından Azure ile bağlantı oluşturmak için çalıştırın. `Connect-AzAccount`

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal ağ ve ağ güvenliği grupları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)  | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [Yeni-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Arka uç ve DMZ alt ağları oluşturur. |
| [Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | VM'ye Internet'ten erişmek için herkese açık bir IP adresi oluşturur. |
| [Yeni-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Bir sanal ağ arabirimi oluşturur ve bunun için IP iletimini etkinleştirir. |
| [Yeni-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Ağ güvenlik grubu (NSG) oluşturur. |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) | Sanal makineye gelen HTTP ve HTTPS bağlantı noktalarına izin veren NSG kuralları oluşturur. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)| NSG’leri ve rota tablolarını alt ağlarla ilişkilendirir. |
| [Yeni-AzRouteTable](/powershell/module/az.network/new-azroutetable)| Tüm rotalar için bir rota tablosu oluşturur. |
| [Yeni-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig)| VM üzerinden alt ağlar ve Internet arasındaki trafiği yönlendirmek için yollar oluşturur. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Bir sanal makine oluşturur ve ona NIC’yi ekler. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)  | Bir kaynak grubunu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview).

Ek ağ PowerShell betiği örnekleri, [Azure Ağına Genel Bakış belgeleri](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json) içinde bulunabilir.
