---
title: Her zaman açık VPN Kullanıcı tüneli yapılandırma
titleSuffix: Azure Virtual WAN
description: Bu makalede, sanal WAN 'niz için Always on VPN Kullanıcı tünelinin nasıl yapılandırılacağı açıklanmaktadır
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 03/13/2020
ms.author: cherylmc
ms.openlocfilehash: 72287403e8406e0cfce83a69a5b9d3f58c693b8b
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84750503"
---
# <a name="configure-an-always-on-vpn-user-tunnel-for-virtual-wan"></a>Sanal WAN için Always on VPN Kullanıcı tüneli yapılandırma

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Ön koşullar

Noktadan siteye yapılandırma oluşturmanız ve sanal hub atamasını düzenlemeniz gerekir. Yönergeler için aşağıdaki bölümlere bakın:

* [P2S yapılandırması oluşturma](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Hub atamasını düzenleme](virtual-wan-point-to-site-portal.md#edit)

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
