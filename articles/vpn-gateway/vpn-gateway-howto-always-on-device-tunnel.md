---
title: Her zaman açık VPN tüneli yapılandırma
titleSuffix: Azure VPN Gateway
description: VPN Gateway için Always on VPN tünelini yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 6db48928ebac115c42c643d669f6541a3654a53a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84983130"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Always On VPN cihazı tüneli yapılandırma

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma

VPN ağ geçidini, [noktadan sıteye VPN bağlantısı yapılandırma](vpn-gateway-howto-point-to-site-resource-manager-portal.md) makalesini kullanarak Ikev2 ve sertifika tabanlı kimlik doğrulaması kullanacak şekilde yapılandırın.

## <a name="configure-the-device-tunnel"></a>Cihaz tüneli yapılandırma

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>Bir profili kaldırmak için

Profili kaldırmak için aşağıdaki komutu çalıştırın:

![Temizleme](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme için bkz. [Azure Noktadan siteye bağlantı sorunları](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
