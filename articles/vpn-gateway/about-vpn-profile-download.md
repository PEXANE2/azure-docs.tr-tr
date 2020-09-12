---
title: 'Azure VPN Gateway: P2S VPN istemci profilleri hakkında'
description: Bu, istemci profili dosyasıyla çalışmanıza yardımcı olur
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: 326abaa123ede1fbe371c69fcbed47a310b54511
ms.sourcegitcommit: ac5cbef0706d9910a76e4c0841fdac3ef8ed2e82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89424846"
---
# <a name="about-p2s-vpn-client-profiles"></a>P2S VPN istemci profilleri hakkında

İndirilen profil dosyası, bir VPN bağlantısını yapılandırmak için gereken bilgileri içerir. Bu makale, bir VPN istemci profili için gereken bilgileri elde etmenize ve anlamanıza yardımcı olur.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* **OpenVPN klasörü** , anahtarı ve sertifikayı içerecek şekilde değiştirilmesi gereken *ovpn* profilini içerir. Daha fazla bilgi için bkz. [Azure Için OpenVPN Istemcilerini yapılandırma VPN Gateway](vpn-gateway-howto-openvpn-clients.md#windows). VPN ağ geçidinde Azure AD kimlik doğrulaması seçilirse, bu klasör ZIP dosyasında mevcut değildir. Bunun yerine AzureVPN klasörüne gidin ve azurevpnconfig.xml bulun.

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye hakkında daha fazla bilgi için bkz. [Noktadan siteye hakkında](point-to-site-about.md).
