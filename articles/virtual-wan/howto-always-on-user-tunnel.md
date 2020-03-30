---
title: Her Zaman Açık VPN kullanıcı tünelini yapılandırma
titleSuffix: Azure Virtual WAN
description: Bu makalede, Virtual WAN'ınız için Her Zaman VPN kullanıcı tünelinin nasıl yapılandırılabildiğini açıklanmaktadır.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: cherylmc
ms.openlocfilehash: dd5b215b143fbaf487325744a158bb8b05707951
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502873"
---
# <a name="configure-an-always-on-vpn-user-tunnel-for-virtual-wan"></a>Virtual WAN için Her Zaman VPN kullanıcı tünelini yapılandırma

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Ön koşullar

Bir noktadan siteye yapılandırma oluşturmanız ve sanal hub atamasını düzenlemeniz gerekir. Talimatlar için aşağıdaki bölümlere bakın:

* [P2S yapılandırması oluşturma](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Hub atamasını değiştirme](virtual-wan-point-to-site-portal.md#edit)

## <a name="configure-a-user-tunnel"></a>Kullanıcı tünelini yapılandırma

[!INCLUDE [user tunnel](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>Profili kaldırmak için

Bir profili kaldırmak için aşağıdaki adımları kullanın:

1. Şu komutu çalıştırın:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Bağlantıyı kesin ve Connect onay kutusunu **temizleyin.**

   ![Temizleme](./media/howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [SSS](virtual-wan-faq.md)bölümüne bakın.
