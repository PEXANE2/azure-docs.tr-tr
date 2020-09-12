---
title: Azure VPN Gateway için OpenVPN Istemcilerini yapılandırma | Microsoft Docs
description: Windows, Linux ve Mac işletim sistemi istemcileri için Azure VPN Gateway için OpenVPN 'i yapılandırmayı öğrenin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: 4be903fa17ce95e96c82241249b421e1d794c80f
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89435788"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Azure VPN Gateway için OpenVPN istemcileri yapılandırma

Bu makale, **OpenVPN &reg; protokol** istemcilerini yapılandırmanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

VPN ağ geçidiniz için OpenVPN yapılandırma adımlarını tamamladığınızdan emin olun. Ayrıntılar için bkz. [Azure Için OpenVPN 'ı yapılandırma VPN Gateway](vpn-gateway-howto-openvpn.md).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Sonraki adımlar

VPN istemcilerinin başka bir sanal ağdaki kaynaklara erişebilmesini istiyorsanız, VNET 'ten VNET 'e bağlantı kurmak için [VNET-VNET](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesindeki yönergeleri izleyin. Ağ geçitleri ve bağlantılarda BGP 'yi etkinleştirdiğinizden emin olun, aksi takdirde trafik akmaz.

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
