---
title: Azure PowerShell komut dosyası örneği - Çok katmanlı uygulamalar için ağ oluşturma
description: Azure PowerShell betiği örneği - Çok katmanlı uygulamalar için sanal ağ oluşturma.
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
ms.openlocfilehash: 2faa799276bafca1ac731dbbef1c08430c7cea73
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521490"
---
# <a name="create-a-network-for-multi-tier-applications"></a>Çok katmanlı uygulamalar için ağ oluşturma

Bu betik örneği, ön uç ve arka uç alt ağları ile sanal ağ oluşturur. 3306 numaralı bağlantı noktası için, ön uç alt ağına giden trafik HTTP ve SSH ile sınırlıyken, arka uç alt ağına giden trafik MySQL ile sınırlıdır. Betiği çalıştırdıktan sonra, her bir alt ağda, web sunucusunu ve MySQL yazılımını dağıtabileceğiniz iki sanal makineniz vardır.

Gerekirse, [Azure PowerShell kılavuzunda](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)bulunan yönergeyi kullanarak Azure PowerShell'i yükleyin ve ardından Azure ile bağlantı oluşturmak için çalıştırın. `Connect-AzAccount`

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal ağ ve ağ güvenliği grupları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [Yeni-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Bir arka uç alt ağı oluşturur. |
| [Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | VM'ye Internet'ten erişmek için herkese açık bir IP adresi oluşturur. |
| [Yeni-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Sanal ağ arabirimleri oluşturur ve bunları sanal ağın ön uç ve arka uç alt ağlarına ekler. |
| [Yeni-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Ön uç ve arka uç alt ağlarıyla ilişkilendirilmiş ağ güvenlik grupları (NSG) oluşturur. |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) |Belirli alt ağlara yönelik belirli bağlantı noktalarına izin veren veya engelleyen NSG kuralları oluşturur. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Sanal makineler oluşturur ve her sanal makineye bir NIC ekler. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview).

Ek ağ PowerShell betiği örnekleri, [Azure Ağına Genel Bakış belgeleri](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json) içinde bulunabilir.
