---
title: 'Azure ExpressRoute: sanal ağa ağ geçidi ekleme: klasik'
description: Klasik dağıtım için bir VNet ağ geçidi bir ExpressRoute yapılandırma için PowerShell kullanarak VNet model.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 12/06/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: d7927af5b831dbe3294e1abc3a878e323bf1867e
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74928058"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>PowerShell (Klasik) kullanarak ExpressRoute için sanal ağ geçidi yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makalede, ekleme, yeniden boyutlandırma ve önceden var olan bir VNet için bir sanal ağ (VNet) ağ geçidini kaldırmak için adımlarında size yol gösterir. Bu yapılandırmanın adımları kullanılarak oluşturulan özel sanal ağlar için olan **Klasik dağıtım modeli** ve ExpressRoute yapılandırması kullanılır. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Başlamadan önce
Bu yapılandırma için gereken Azure PowerShell cmdlet 'lerini yüklediğinizi doğrulayın.

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidinin oluşturduktan sonra ağınız bir ExpressRoute bağlantı hattına bağlayabilirsiniz. Bkz: [bir sanal ağı ExpressRoute devresine bağlama](expressroute-howto-linkvnet-classic.md).

