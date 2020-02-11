---
title: Klasik Azure Resource Manager geçiş teknik derinlemesine bakış
description: Klasik dağıtım modelinden Azure Resource Manager için, platform tarafından desteklenen kaynak geçişi hakkında teknik kapsamlı bakış.
services: virtual-machines-windows
documentationcenter: ''
author: tanmaygore
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 1ee40d32-a5e8-42a2-97d0-3232fd3cbb98
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: 03da35eb0f0a03bd9a9e28b0bd92a94979c6fcf5
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77115778"
---
# <a name="technical-deep-dive-on-platform-supported-migration-from-classic-to-azure-resource-manager"></a>Klasik 'dan Azure Resource Manager 'a yönelik platform destekli geçiş hakkında teknik kapsamlı bakış
Azure klasik dağıtım modelinden Azure Resource Manager dağıtım modeline geçiş konusunda ayrıntılı bilgi alalım. Azure platformunun kaynakları iki dağıtım modeli arasında nasıl geçirdiğini anlamanıza yardımcı olması için kaynak ve özellik düzeyindeki kaynaklara göz atacağız. Daha fazla bilgi için lütfen bkz. hizmet duyurusu makalesi: [IaaS kaynaklarının klasik sürümünden Azure Resource Manager geçişi](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-migration-deep-dive](../../../includes/virtual-machines-common-classic-resource-manager-migration-deep-dive.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [IaaS kaynaklarının klasik ile Azure Resource Manager geçişine genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasik 'ten Azure Resource Manager geçirmek için PowerShell 'i kullanma](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasik 'ten Azure Resource Manager geçirmek için CLı kullanma](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [VPN Gateway klasik Kaynak Yöneticisi geçişe](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-classic-resource-manager-migration)
* [ExpressRoute devreleri ve ilişkili sanal ağları klasik 'dan Kaynak Yöneticisi dağıtım modeline geçirin](https://docs.microsoft.com/azure/expressroute/expressroute-migration-classic-resource-manager)
* [IaaS kaynaklarının klasik 'dan Azure Resource Manager geçişine yardımcı olacak topluluk araçları](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasik konumundan Azure Resource Manager geçirme hakkında en sık sorulan soruları gözden geçirin](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
