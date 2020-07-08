---
title: 'Azure ExpressRoute: sanal ağa ağ geçidi ekleme: PowerShell'
description: Bu makale, ExpressRoute için zaten oluşturulmuş bir Kaynak Yöneticisi VNet 'e VNet ağ geçidi eklemenize yardımcı olur.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: how-to
ms.date: 02/21/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: 58d2949a18bfbf2800ae6ab4ac74b02b05b3eb07
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84736399"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>PowerShell kullanarak ExpressRoute için sanal ağ geçidi yapılandırma
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi Azure portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik-PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makale, önceden var olan VNet için bir sanal ağ (VNet) ağ geçidi eklemenize, yeniden boyutlandırmanıza ve kaldırmanıza yardımcı olur. Bu yapılandırmaya yönelik adımlar, bir ExpressRoute yapılandırması için Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan sanal ağlar için geçerlidir. Daha fazla bilgi için bkz. [ExpressRoute için sanal ağ geçitleri hakkında](expressroute-about-virtual-network-gateways.md).

## <a name="before-beginning"></a>Başlamadan önce

### <a name="working-with-powershell"></a>PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [working with cloud shell](../../includes/expressroute-cloudshell-powershell-about.md)]

### <a name="configuration-reference-list"></a>Yapılandırma başvuru listesi

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidini oluşturduktan sonra sanal ortamınızı bir ExpressRoute devresine bağlayabilirsiniz. Bkz. [sanal ağı bir ExpressRoute devresine bağlama](expressroute-howto-linkvnet-arm.md).
