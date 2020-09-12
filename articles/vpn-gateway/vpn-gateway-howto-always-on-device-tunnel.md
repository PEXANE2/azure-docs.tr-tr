---
title: Her zaman açık VPN tüneli yapılandırma
titleSuffix: Azure VPN Gateway
description: Kalıcı cihaz tünellerini Azure 'a kurmak ve yapılandırmak için Windows 10 her zaman açık olan ağ geçitlerini nasıl kullanacağınızı öğrenin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: ab951375fdf9c5317e2c3a2f67271666d0987142
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89435856"
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
