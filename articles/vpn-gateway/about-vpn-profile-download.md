---
title: 'Azure VPN Gateway: P2S VPN istemci profilleri hakkında'
description: Bu, istemci profili dosyasıyla çalışmanıza yardımcı olur
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 03/17/2020
ms.author: cherylmc
ms.openlocfilehash: b78d32dbb7b4e0d9a3b13ff741e6e38c12be0e62
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79528515"
---
# <a name="about-p2s-vpn-client-profiles"></a>P2S VPN istemci profilleri hakkında

İndirilen profil dosyası, bir VPN bağlantısını yapılandırmak için gereken bilgileri içerir. Bu makale, bir VPN istemci profili için gereken bilgileri elde etmenize ve anlamanıza yardımcı olur.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* **OpenVPN klasörü** , anahtarı ve sertifikayı içerecek şekilde değiştirilmesi gereken *ovpn* profilini içerir. Daha fazla bilgi için bkz. [Azure Için OpenVPN Istemcilerini yapılandırma VPN Gateway](vpn-gateway-howto-openvpn-clients.md#windows). VPN ağ geçidinde Azure AD kimlik doğrulaması seçilirse, bu klasör ZIP dosyasında mevcut olmayacaktır. Bunun yerine, azurevpnconfig. xml AzureVPN klasöründe olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye hakkında daha fazla bilgi için bkz. [Noktadan siteye hakkında](point-to-site-about.md).
