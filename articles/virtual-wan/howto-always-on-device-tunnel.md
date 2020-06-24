---
title: Her zaman açık VPN tüneli yapılandırma
titleSuffix: Azure Virtual WAN
description: Sanal WAN için Always on VPN cihaz tüneli yapılandırma adımları
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/13/2020
ms.author: cherylmc
ms.openlocfilehash: e911bf6e3736c931ca5c1563ab42f52ecb5cf3c1
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84750575"
---
# <a name="configure-an-always-on-vpn-device-tunnel-for-virtual-wan"></a>Sanal WAN için Always on VPN cihaz tüneli yapılandırma

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Ön koşullar

Noktadan siteye yapılandırma oluşturmanız ve sanal hub atamasını düzenlemeniz gerekir. Yönergeler için aşağıdaki bölümlere bakın:

* [P2S yapılandırması oluşturma](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Hub atamasını düzenleme](virtual-wan-point-to-site-portal.md#edit)

## <a name="configure-the-device-tunnel"></a>Cihaz tüneli yapılandırma

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>Bir profili kaldırmak için

Profili kaldırmak için aşağıdaki komutu çalıştırın:

![Temizleme](./media/howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).