---
title: Her zaman açık VPN Kullanıcı tüneli yapılandırma
titleSuffix: Azure VPN Gateway
description: Bu makalede, VPN ağ geçidiniz için Always on VPN Kullanıcı tünelinin nasıl yapılandırılacağı açıklanmaktadır
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: 99d21222a62ed6b27a6a1b2a73b704f4cb26457b
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89435805"
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
