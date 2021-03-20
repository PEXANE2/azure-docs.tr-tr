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
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "89435788"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Azure VPN Gateway için OpenVPN istemcilerini yapılandırma

Bu makale, **OpenVPN &reg; protokol** istemcilerini yapılandırmanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

VPN ağ geçidiniz için OpenVPN yapılandırma adımlarını tamamladığınızdan emin olun. Ayrıntılar için bkz. [Azure Için OpenVPN 'ı yapılandırma VPN Gateway](vpn-gateway-howto-openvpn.md).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Sonraki adımlar

VPN istemcilerinin başka bir sanal ağdaki kaynaklara erişebilmesini istiyorsanız, VNET 'ten VNET 'e bağlantı kurmak için [VNET-VNET](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesindeki yönergeleri izleyin. Ağ geçitleri ve bağlantılarda BGP 'yi etkinleştirdiğinizden emin olun, aksi takdirde trafik akmaz.

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
