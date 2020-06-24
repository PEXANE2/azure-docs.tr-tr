---
title: Azure VPN Gateway için OpenVPN Istemcilerini yapılandırma | Microsoft Docs
description: Azure VPN Gateway için OpenVPN Istemcilerini yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: bdddf097265e7af688175688b6f3214413a90fdc
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84984090"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Azure VPN Gateway için OpenVPN istemcileri yapılandırma

Bu makale, **OpenVPN &reg; protokol** istemcilerini yapılandırmanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

VPN ağ geçidiniz için OpenVPN yapılandırma adımlarını tamamladığınızdan emin olun. Ayrıntılar için bkz. [Azure Için OpenVPN 'ı yapılandırma VPN Gateway](vpn-gateway-howto-openvpn.md).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Sonraki adımlar

VPN istemcilerinin başka bir sanal ağdaki kaynaklara erişebilmesini istiyorsanız, VNET 'ten VNET 'e bağlantı kurmak için [VNET-VNET](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesindeki yönergeleri izleyin. Ağ geçitleri ve bağlantılarda BGP 'yi etkinleştirdiğinizden emin olun, aksi takdirde trafik akmaz.

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
