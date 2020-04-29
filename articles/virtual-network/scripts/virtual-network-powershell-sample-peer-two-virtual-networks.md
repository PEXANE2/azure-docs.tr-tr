---
title: Eş iki sanal ağ-Azure PowerShell betik örneği
description: Azure PowerShell betiği örneği - İki sanal ağı eşleme
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: kumud
ms.openlocfilehash: 4061997aa2efbae250b30fc58cef06b1249c2b8f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74091297"
---
# <a name="peer-two-virtual-networks-script-sample"></a>İki sanal ağı eşleme betiği örneği

Bu betik, Azure ağı aracılığıyla aynı bölgede iki sanal ağ oluşturur ve bu sanal ağları birbirine bağlar. Betiği çalıştırdıktan sonra iki sanal ağ arasında eşleme oluşturmuş olursunuz.

Azure [Cloud Shell](https://shell.azure.com/powershell)’den veya yerel bir PowerShell yüklemesinden betiği yürütebilirsiniz. PowerShell 'i yerel olarak kullanırsanız, bu betik az PowerShell Module sürüm 5.4.1 veya üstünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda yer alan her komut, komuta özgü belgelere yönlendirir:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)| Bir Azure sanal ağı ve alt ağ oluşturur. |
| [Add-Azvirtualnetworkeşleme](/powershell/module/az.network/add-azvirtualnetworkpeering) | İki sanal ağ arasında eşleme oluşturur.  |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal ağ PowerShell betiği örnekleri, [Sanal ağ PowerShell örnekleri](../powershell-samples.md) bölümünde bulunabilir.