---
title: "VPN kullanıcıları için MFA'yı etkinleştirme: Azure AD kimlik doğrulaması"
description: VPN kullanıcıları için çok faktörlü kimlik doğrulamayı etkinleştirme
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: alzam
ms.openlocfilehash: 13f012af95bb2b6098317e59e5293fb72804a6a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471593"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>VPN kullanıcıları için Azure Çok Faktörlü Kimlik Doğrulamayı (MFA) etkinleştirme

[!INCLUDE [overview](../../includes/vpn-gateway-vwan-openvpn-enable-mfa-overview.md)]

## <a name="enable-authentication"></a><a name="enableauth"></a>Kimlik doğrulamayı etkinleştirme

[!INCLUDE [enable authentication](../../includes/vpn-gateway-vwan-openvpn-enable-auth.md)]

## <a name="configure-sign-in-settings"></a><a name="enablesign"></a>Oturum açma ayarlarını yapılandırma

[!INCLUDE [sign in](../../includes/vpn-gateway-vwan-openvpn-sign-in.md)]

## <a name="option-1---per-user-access"></a><a name="peruser"></a>Seçenek 1 - Kullanıcı başına erişim

[!INCLUDE [per user](../../includes/vpn-gateway-vwan-openvpn-per-user.md)]

## <a name="option-2---conditional-access"></a><a name="conditional"></a>Seçenek 2 - Koşullu Erişim

[!INCLUDE [conditional access](../../includes/vpn-gateway-vwan-openvpn-conditional.md)]

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız gerekir. Bkz. [Azure'a Nokta-To-Site bağlantısı için Azure AD kimlik doğrulaması yapılandırın.](virtual-wan-point-to-site-azure-ad.md)
