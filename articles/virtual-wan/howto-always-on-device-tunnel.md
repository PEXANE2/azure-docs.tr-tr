---
title: Always-On VPN tüneli yapılandırma
titleSuffix: Azure Virtual WAN
description: Sanal WAN için Always on VPN cihaz tüneli yapılandırma adımları
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: e814487cb4dab9c8c19daab2ea3bb81391d4a98f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "90983685"
---
# <a name="configure-an-always-on-vpn-device-tunnel-for-virtual-wan"></a>Sanal WAN için Always on VPN cihaz tüneli yapılandırma

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Önkoşullar

Noktadan siteye yapılandırma oluşturmanız ve sanal hub atamasını düzenlemeniz gerekir. Yönergeler için aşağıdaki bölümlere bakın:

* [P2S yapılandırması oluşturma](virtual-wan-point-to-site-portal.md#p2sconfig)
* [P2S ağ geçidi ile hub oluşturma](virtual-wan-point-to-site-portal.md#hub)

## <a name="configure-the-device-tunnel"></a>Cihaz tüneli yapılandırma

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>Bir profili kaldırmak için

Profili kaldırmak için aşağıdaki komutu çalıştırın:

![Ekran görüntüsünde Remove-VpnConnection-Name Machineccerttest komutunu çalıştıran bir PowerShell penceresi gösterilmektedir.](./media/howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).