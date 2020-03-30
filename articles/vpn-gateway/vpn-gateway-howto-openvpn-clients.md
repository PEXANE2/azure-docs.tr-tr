---
title: Azure VPN Ağ Geçidi için OpenVPN İstemleri nasıl yapılandırılabilen| Microsoft Dokümanlar
description: Azure VPN Ağ Geçidi için OpenVPN İstemlerini yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: 09ff3ccebad0baa4148e68995254c818a29d7bd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066171"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Azure VPN Ağ Geçidi için OpenVPN istemcilerini yapılandırma

Bu makale, **OpenVPN &reg; Protokolü** istemcilerini yapılandırmanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

VPN ağ geçidiniz için OpenVPN'i yapılandırma adımlarını tamamladığınızı doğrulayın. Ayrıntılar için Azure [VPN Ağ Geçidi için OpenVPN'i Yapılandır'a](vpn-gateway-howto-openvpn.md)bakın.

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Sonraki adımlar

VPN istemcilerinin başka bir VNet'teki kaynaklara erişebilmesini istiyorsanız, vnet-to-vnet bağlantısı kurmak [için VNet-to-VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesindeki yönergeleri izleyin. Ağ geçitlerinde ve bağlantılarda BGP'yi etkinleştirdiğinden emin olun, aksi takdirde trafik akmaz.

**"OpenVPN", OpenVPN Inc. şirketinin ticari markasıdır.**
