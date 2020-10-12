---
title: 'Azure VPN Gateway: P2S VPN istemci profilleri hakkında'
description: Bir VPN istemci profili için ihtiyaç duyduğunuz bilgileri bulmak için bu makaleyi kullanın.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: 6a09767a7992a5f902adea6f99e937f3fc6fa7fa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90985929"
---
# <a name="about-p2s-vpn-client-profiles"></a>P2S VPN istemci profilleri hakkında

İndirilen profil dosyası, bir VPN bağlantısını yapılandırmak için gereken bilgileri içerir. Bu makale, bir VPN istemci profili için gereken bilgileri elde etmenize ve anlamanıza yardımcı olur.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* **OpenVPN klasörü** , anahtarı ve sertifikayı içerecek şekilde değiştirilmesi gereken *ovpn* profilini içerir. Daha fazla bilgi için bkz. [Azure Için OpenVPN Istemcilerini yapılandırma VPN Gateway](vpn-gateway-howto-openvpn-clients.md#windows). VPN ağ geçidinde Azure AD kimlik doğrulaması seçilirse, bu klasör ZIP dosyasında mevcut değildir. Bunun yerine AzureVPN klasörüne gidin ve azurevpnconfig.xml bulun.

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye hakkında daha fazla bilgi için bkz. [Noktadan siteye hakkında](point-to-site-about.md).
