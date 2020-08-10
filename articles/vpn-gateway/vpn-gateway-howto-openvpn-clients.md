---
title: Azure VPN Gateway için OpenVPN Istemcilerini yapılandırma | Microsoft Docs
description: Windows, Linux ve Mac işletim sistemi istemcileri için Azure VPN Gateway için OpenVPN 'i yapılandırmayı öğrenin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: 92447a541726c1c11b7b10d6d52cf91cfc07f945
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88036870"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Azure VPN Gateway için OpenVPN istemcileri yapılandırma

Bu makale, **OpenVPN &reg; protokol** istemcilerini yapılandırmanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

VPN ağ geçidiniz için OpenVPN yapılandırma adımlarını tamamladığınızdan emin olun. Ayrıntılar için bkz. [Azure Için OpenVPN 'ı yapılandırma VPN Gateway](vpn-gateway-howto-openvpn.md).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Sonraki adımlar

VPN istemcilerinin başka bir sanal ağdaki kaynaklara erişebilmesini istiyorsanız, VNET 'ten VNET 'e bağlantı kurmak için [VNET-VNET](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesindeki yönergeleri izleyin. Ağ geçitleri ve bağlantılarda BGP 'yi etkinleştirdiğinizden emin olun, aksi takdirde trafik akmaz.

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
