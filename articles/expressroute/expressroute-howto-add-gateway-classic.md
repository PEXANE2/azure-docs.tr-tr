---
title: "Azure ExpressRoute: Bir VNet'e ağ geçidi ekleme: klasik"
description: ExpressRoute yapılandırması için PowerShell'i kullanan klasik bir dağıtım modeli VNet için bir VNet ağ geçidi yapılandırın.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 12/06/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: d7927af5b831dbe3294e1abc3a878e323bf1867e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74928058"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>PowerShell kullanarak ExpressRoute için sanal ağ ağ ağ geçidini yapılandırma (klasik)
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure Portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makale, önceden varolan bir VNet için sanal ağ (VNet) ağ geçidi ekleme, yeniden boyutlandırma ve kaldırma adımlarını size iletir. Bu yapılandırma için adımlar, **klasik dağıtım modeli** kullanılarak oluşturulan ve ExpressRoute yapılandırmasında kullanılacak VNets için özeldir. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Başlamadan önce
Bu yapılandırma için gereken Azure PowerShell cmdlets'i yüklediğinizi doğrulayın.

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidini oluşturduktan sonra, VNet'inizi bir ExpressRoute devresine bağlayabilirsiniz. Bkz. [Bir Sanal Ağı Bir ExpressRoute devresine bağla.](expressroute-howto-linkvnet-classic.md)

