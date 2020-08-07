---
title: Her zaman açık VPN tüneli yapılandırma
titleSuffix: Azure VPN Gateway
description: Kullanıcı oturum açma, ağ durumu değişikliği veya cihaz ekranı etkin gibi tetikleyicilere dayalı bir VPN bağlantısı tutan her zaman açık ' ı nasıl yapılandıracağınızı öğrenin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 24043576fca4910631ccddb3924303dd642c6842
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87927071"
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
