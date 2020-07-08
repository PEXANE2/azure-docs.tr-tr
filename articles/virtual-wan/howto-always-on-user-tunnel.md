---
title: Her zaman açık VPN Kullanıcı tüneli yapılandırma
titleSuffix: Azure Virtual WAN
description: Bu makalede, sanal WAN 'niz için Always on VPN Kullanıcı tünelinin nasıl yapılandırılacağı açıklanmaktadır
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 06/22/2020
ms.author: cherylmc
ms.openlocfilehash: 03f67053a5a199c8c64efb05d2b6a65ad6707650
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564058"
---
# <a name="configure-an-always-on-vpn-user-tunnel-for-virtual-wan"></a>Sanal WAN için Always on VPN Kullanıcı tüneli yapılandırma

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Ön koşullar

Noktadan siteye yapılandırma oluşturmanız ve sanal hub atamasını düzenlemeniz gerekir. Yönergeler için aşağıdaki bölümlere bakın:

* [P2S yapılandırması oluşturma](virtual-wan-point-to-site-portal.md#p2sconfig)
* [P2S ağ geçidi ile hub oluşturma](virtual-wan-point-to-site-portal.md#hub)

## <a name="configure-a-user-tunnel"></a>Kullanıcı tüneli yapılandırma

[!INCLUDE [user tunnel](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>Bir profili kaldırmak için

Bir profili kaldırmak için aşağıdaki adımları kullanın:

1. Şu komutu çalıştırın:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Bağlantının bağlantısını kesin ve **otomatik olarak bağlan** onay kutusunu temizleyin.

   ![Temizleme](./media/howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
