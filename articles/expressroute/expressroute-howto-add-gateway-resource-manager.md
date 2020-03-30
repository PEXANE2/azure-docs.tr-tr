---
title: "Azure ExpressRoute: VNet:PowerShell'e ağ geçidi ekleme"
description: Bu makale, ExpressRoute için zaten oluşturulmuş bir Kaynak Yöneticisi VNet'e VNet ağ geçidi eklemenize yardımcı olur.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 02/21/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: 6e6d79afa1126acb26cb8856c39ba486cce31a5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74037421"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>PowerShell kullanarak ExpressRoute için sanal ağ geçidi yapılandırma
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi - Azure portalı](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makale, önceden varolan bir VNet için sanal ağ (VNet) ağ geçidi eklemenize, yeniden boyutlandırmanıza ve kaldırmanıza yardımcı olur. Bu yapılandırma için adımlar, ExpressRoute yapılandırması için Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan VNets için geçerlidir. Daha fazla bilgi için [ExpressRoute için sanal ağ ağ geçitleri hakkında](expressroute-about-virtual-network-gateways.md)bilgi alabiliyorum.

## <a name="before-beginning"></a>Başlamadan önce

### <a name="working-with-powershell"></a>PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [working with cloud shell](../../includes/expressroute-cloudshell-powershell-about.md)]

### <a name="configuration-reference-list"></a>Yapılandırma başvuru listesi

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidini oluşturduktan sonra, VNet'inizi bir ExpressRoute devresine bağlayabilirsiniz. Bkz. [Bir Sanal Ağı Bir ExpressRoute devresine bağla.](expressroute-howto-linkvnet-arm.md)
