---
title: 'Azure VPN Gateway: P2S VPN istemci profilleri hakkında'
description: Bu, istemci profili dosyasıyla çalışmanıza yardımcı olur
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 05/13/2020
ms.author: cherylmc
ms.openlocfilehash: 4526611fa8b80a7b97ee7317f5e285c50f05b0de
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83650672"
---
# <a name="about-p2s-vpn-client-profiles"></a>P2S VPN istemci profilleri hakkında

İndirilen profil dosyası, bir VPN bağlantısını yapılandırmak için gereken bilgileri içerir. Bu makale, bir VPN istemci profili için gereken bilgileri elde etmenize ve anlamanıza yardımcı olur.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* **OpenVPN klasörü** , anahtarı ve sertifikayı içerecek şekilde değiştirilmesi gereken *ovpn* profilini içerir. Daha fazla bilgi için bkz. [Azure Için OpenVPN Istemcilerini yapılandırma VPN Gateway](vpn-gateway-howto-openvpn-clients.md#windows). VPN ağ geçidinde Azure AD kimlik doğrulaması seçilirse, bu klasör ZIP dosyasında mevcut değildir. Bunun yerine AzureVPN klasörüne gidin ve azurevpnconfig.xml bulun.

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye hakkında daha fazla bilgi için bkz. [Noktadan siteye hakkında](point-to-site-about.md).
