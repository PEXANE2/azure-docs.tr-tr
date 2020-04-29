---
title: Azure VPN Gateway için OpenVPN Istemcilerini yapılandırma | Microsoft Docs
description: Azure VPN Gateway için OpenVPN Istemcilerini yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: 09ff3ccebad0baa4148e68995254c818a29d7bd4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80066171"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Azure VPN Gateway için OpenVPN istemcileri yapılandırma

Bu makale, **OpenVPN &reg; protokol** istemcilerini yapılandırmanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

VPN ağ geçidiniz için OpenVPN yapılandırma adımlarını tamamladığınızdan emin olun. Ayrıntılar için bkz. [Azure Için OpenVPN 'ı yapılandırma VPN Gateway](vpn-gateway-howto-openvpn.md).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Sonraki adımlar

VPN istemcilerinin başka bir sanal ağdaki kaynaklara erişebilmesini istiyorsanız, VNET 'ten VNET 'e bağlantı kurmak için [VNET-VNET](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesindeki yönergeleri izleyin. Ağ geçitleri ve bağlantılarda BGP 'yi etkinleştirdiğinizden emin olun, aksi takdirde trafik akmaz.

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
