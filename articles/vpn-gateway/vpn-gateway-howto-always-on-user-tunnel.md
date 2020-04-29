---
title: Her zaman açık VPN Kullanıcı tüneli yapılandırma
titleSuffix: Azure VPN Gateway
description: Bu makalede, VPN ağ geçidiniz için Always on VPN Kullanıcı tünelinin nasıl yapılandırılacağı açıklanmaktadır
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: cherylmc
ms.openlocfilehash: 56934dd13661d8f623e673e2817e87618675c7ed
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79502263"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>Always On VPN kullanıcı tüneli yapılandırma

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>Ağ geçidini yapılandırma

 VPN ağ geçidini Ikev2 ve sertifika tabanlı kimlik doğrulaması kullanacak şekilde yapılandırmak için [noktadan sıteye VPN bağlantısı yapılandırma](vpn-gateway-howto-point-to-site-resource-manager-portal.md) makalesindeki yönergeleri kullanın.

## <a name="configure-a-user-tunnel"></a>Kullanıcı tüneli yapılandırma

[!INCLUDE [user configuration](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>Bir profili kaldırmak için

Bir profili kaldırmak için aşağıdaki adımları kullanın:

1. Şu komutu çalıştırın:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Bağlantının bağlantısını kesin ve **otomatik olarak bağlan** onay kutusunu temizleyin.

   ![Temizleme](./media/vpn-gateway-howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Oluşabilecek bağlantı sorunlarını gidermek için bkz. [Azure Noktadan siteye bağlantı sorunları](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
