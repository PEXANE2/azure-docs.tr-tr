---
title: Azure sanal WAN için OpenVPN istemcileri yapılandırma
description: Azure sanal WAN için OpenVPN istemcilerini yapılandırma adımları
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 05502b344b1224dff5d12f95b96b05baace98970
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94491010"
---
# <a name="configure-an-openvpn-client-for-azure-virtual-wan"></a>Azure sanal WAN için OpenVPN istemcisi yapılandırma

Bu makale, **OpenVPN &reg; protokol** istemcilerini yapılandırmanıza yardımcı olur. OpenVPN protokolünü kullanarak bağlanmak için Windows 10 için Azure VPN Istemcisini de kullanabilirsiniz. [Burada](openvpn-azure-ad-client.md) daha fazla yönerge bulabilirsiniz

## <a name="before-you-begin"></a>Başlamadan önce

Bir kullanıcı VPN (Noktadan siteye) yapılandırması oluşturun. Tünel türü için "OpenVPN" seçtiğinizden emin olun. Adımlar için bkz. [Azure sanal WAN IÇIN P2S yapılandırması oluşturma](virtual-wan-point-to-site-portal.md#p2sconfig).

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı VPN (Noktadan siteye) hakkında daha fazla bilgi için bkz. [Kullanıcı VPN bağlantıları oluşturma](virtual-wan-point-to-site-portal.md).

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
