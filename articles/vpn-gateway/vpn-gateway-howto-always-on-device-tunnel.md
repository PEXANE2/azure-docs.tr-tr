---
title: Her Zaman Açık VPN tünelini yapılandırma
titleSuffix: Azure VPN Gateway
description: VPN Ağ Geçidi için Her Zaman VPN tünelinde yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 9036992a354b41cd75735e5ccd895d7287a00e91
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371791"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Always On VPN cihazı tüneli yapılandırma

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma

Bir [Point-to-Site VPN bağlantı makalesini Yapılandırarak](vpn-gateway-howto-point-to-site-resource-manager-portal.md) IKEv2 ve sertifika tabanlı kimlik doğrulaması kullanmak için VPN ağ geçidini yapılandırın.

## <a name="configure-the-device-tunnel"></a>Aygıt tünelini yapılandırma

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>Profili kaldırmak için

Profili kaldırmak için aşağıdaki komutu çalıştırın:

![Temizleme](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme için Azure [noktadan siteye bağlantı sorunlarına](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md) bakın
