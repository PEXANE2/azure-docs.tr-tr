---
title: 'Azure ExpressRoute: sanal ağa ağ geçidi ekleme: klasik'
description: ExpressRoute yapılandırması için PowerShell 'i kullanarak klasik bir dağıtım modeli VNet için VNet ağ geçidi yapılandırın.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: how-to
ms.date: 12/06/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: bcffd60596f8c2286b91eaf13be3e09bb8bec94f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84736416"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>PowerShell kullanarak ExpressRoute için sanal ağ geçidi yapılandırma (klasik)
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik-PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video-Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makale, önceden var olan VNet için bir sanal ağ (VNet) ağ geçidi ekleme, yeniden boyutlandırma ve kaldırma adımlarında size yol gösterecektir. Bu yapılandırmaya yönelik adımlar, **klasik dağıtım modeli** kullanılarak oluşturulan ve bir ExpressRoute yapılandırmasında kullanılacak olan sanal ağlar için de kullanılır. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Başlamadan önce
Bu yapılandırma için gereken Azure PowerShell cmdlet 'lerini yüklediğinizi doğrulayın.

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidini oluşturduktan sonra sanal ortamınızı bir ExpressRoute devresine bağlayabilirsiniz. Bkz. [sanal ağı bir ExpressRoute devresine bağlama](expressroute-howto-linkvnet-classic.md).

