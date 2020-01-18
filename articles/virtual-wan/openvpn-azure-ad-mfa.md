---
title: "VPN kullanıcıları için MFA 'yı etkinleştirme: Azure AD kimlik doğrulaması"
description: VPN kullanıcıları için Multi-Factor Authentication 'ı etkinleştirme
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: alzam
ms.openlocfilehash: fcb60e80189da89b3f634c14582be606307536e6
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76166659"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>VPN kullanıcıları için Azure Multi-Factor Authentication (MFA) etkinleştirme

Erişim vermeden önce kullanıcılardan ikinci bir kimlik doğrulama faktörü istenmesini istiyorsanız, Azure AD kiracınız için Azure Multi-Factor Authentication (MFA) yapılandırabilirsiniz. Bu makaledeki adımlar, iki adımlı doğrulama gereksinimini etkinleştirmenize yardımcı olur.

## <a name="prereq"></a>Koşul

Bu yapılandırma için önkoşul, [Kiracı yapılandırma](openvpn-azure-ad-tenant.md)içindeki adımları kullanarak yapılandırılmış BIR Azure AD kiracısıdır.

[!INCLUDE [MFA steps](../../includes/vpn-gateway-vwan-openvpn-azure-ad-mfa.md)]

## <a name="enablesign"></a>Oturum açma ayarlarını yapılandırma

**Azure VPN-Özellikler** sayfasında, oturum açma ayarlarını yapılandırın.

1. **Kullanıcıların oturum açması Için etkinleştirildi mi?** **Evet**olarak ayarlandı. Bu, AD kiracısındaki tüm kullanıcıların VPN 'e başarıyla bağlanmasını sağlar.
2. Oturum açma iznini yalnızca Azure VPN izinleri olan kullanıcılarla sınırlandırmak istiyorsanız, **Kullanıcı atamasını gerekli midir?** **Evet** olarak ayarlayın.
3. Yaptığınız değişiklikleri kaydedin.

   ![İzinler](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız gerekir. Bkz. Azure [Ile Noktadan siteye bağlantı Için Azure AD kimlik doğrulamasını yapılandırma](virtual-wan-point-to-site-azure-ad.md).
