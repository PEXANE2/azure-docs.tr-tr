---
title: 'Azure VPN Ağ Geçidi: P2S VPN istemci profilleri hakkında'
description: Bu, istemci profil dosyasıyla çalışmanıza yardımcı olur
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 03/17/2020
ms.author: cherylmc
ms.openlocfilehash: b78d32dbb7b4e0d9a3b13ff741e6e38c12be0e62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528515"
---
# <a name="about-p2s-vpn-client-profiles"></a>P2S VPN istemci profilleri hakkında

İndirilen profil dosyası, VPN bağlantısını yapılandırmak için gerekli bilgileri içerir. Bu makale, VPN istemci profili için gerekli bilgileri edinmenize ve anlamanıza yardımcı olacaktır.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* **OpenVPN klasörü,** anahtarı ve sertifikayı içerecek şekilde değiştirilmesi gereken *ovpn* profilini içerir. Daha fazla bilgi için Azure [VPN Ağ Geçidi için OpenVPN istemcilerini yapılandırın'](vpn-gateway-howto-openvpn-clients.md#windows)a bakın. VPN ağ geçidinde Azure AD kimlik doğrulaması seçilirse, bu klasör zip dosyasında bulunmaz. Bunun yerine, azurevpnconfig.xml AzureVPN klasöründe olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye hakkında daha fazla bilgi için [bkz.](point-to-site-about.md)
