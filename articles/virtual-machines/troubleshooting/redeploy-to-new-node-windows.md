---
title: Windows sanal makinelerini Azure'da yeniden dağıtma | Microsoft Dokümanlar
description: RDP bağlantı sorunlarını azaltmak için Azure'daki Windows sanal makinelerini yeniden dağıtma.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: dcscontentpm
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 36af0eeb43fb209ed65f950576f2dc9e97ec3633
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71058635"
---
# <a name="redeploy-windows-virtual-machine-to-new-azure-node"></a>Windows sanal makinesini yeni bir Azure düğümüne yeniden dağıtma
Uzak Masaüstü (RDP) bağlantısını veya Windows tabanlı Azure sanal makinesine (VM) uygulama erişiminde sorun gidermede güçlük yaşıyorsanız, VM'yi yeniden dağıtmak yardımcı olabilir. Bir VM'yi yeniden dağıttığınızda, Azure VM'yi kapatır, VM'yi Azure altyapısı içinde yeni bir düğüme taşır ve ardından tüm yapılandırma seçeneklerinizi ve ilişkili kaynaklarınızı koruyarak yeniden güç sağlar. Bu makalede, Azure PowerShell veya Azure portalını kullanarak bir VM'yi nasıl yeniden dağıtabileceğiniz gösterilmektedir.

> [!NOTE]
> Bir VM'yi yeniden dağıttıktan sonra geçici disk kaybolur ve sanal ağ arabirimiyle ilişkili dinamik IP adresleri güncelleştirilir. 


## <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma
Makinenizde en son Azure PowerShell 1.x yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

Aşağıdaki örnek, adlı kaynak `myVM` grubunda adı geçen `myResourceGroup`VM'yi dağıtır:

```powershell
Set-AzVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlanmasorunları yaşıyorsanız, [sorun giderme RDP bağlantıları](troubleshoot-rdp-connection.md) veya [ayrıntılı RDP sorun giderme adımlarında](detailed-troubleshoot-rdp.md)özel yardım bulabilirsiniz. VM'nizde çalışan bir uygulamaya erişemiyorsanız, [uygulama sorun giderme sorunlarını](../windows/troubleshoot-app-connection.md)da okuyabilirsiniz.

