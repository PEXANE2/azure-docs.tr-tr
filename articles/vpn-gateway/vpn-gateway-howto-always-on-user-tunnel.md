---
title: Her Zaman Açık VPN kullanıcı tünelini yapılandırma
titleSuffix: Azure VPN Gateway
description: Bu makalede, VPN ağ geçidiniz için Her Zaman VPN kullanıcı tünelinin nasıl yapılandırılabildiğini açıklanmaktadır.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 56934dd13661d8f623e673e2817e87618675c7ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502263"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>Always On VPN kullanıcı tüneli yapılandırma

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma

 IKEv2 ve sertifika tabanlı kimlik doğrulaması kullanmak için VPN ağ geçidini yapılandırmak için [Bir Noktaya Nokta VPN bağlantı](vpn-gateway-howto-point-to-site-resource-manager-portal.md) makalesinde yönergeleri kullanın.

## <a name="configure-a-user-tunnel"></a>Kullanıcı tünelini yapılandırma

[!INCLUDE [user configuration](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>Profili kaldırmak için

Bir profili kaldırmak için aşağıdaki adımları kullanın:

1. Şu komutu çalıştırın:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Bağlantıyı kesin ve Connect onay kutusunu **temizleyin.**

   ![Temizleme](./media/vpn-gateway-howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Oluşabilecek bağlantı sorunlarını gidermek için [Azure noktadan siteye bağlantı sorunlarına](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)bakın.
