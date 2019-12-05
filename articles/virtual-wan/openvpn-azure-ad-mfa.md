---
title: "VPN kullanıcıları için MFA 'yı etkinleştirme: Azure AD kimlik doğrulaması"
description: VPN kullanıcıları için Multi-Factor Authentication 'ı etkinleştirme
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: alzam
ms.openlocfilehash: 3700014752233e29d157f1695906babe4fa35c74
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74822779"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>VPN kullanıcıları için Azure Multi-Factor Authentication (MFA) etkinleştirme

Erişim vermeden önce kullanıcılardan ikinci bir kimlik doğrulama faktörü istenmesini istiyorsanız, Azure AD kiracınız için Azure Multi-Factor Authentication (MFA) yapılandırabilirsiniz. Bu makaledeki adımlar, iki adımlı doğrulama gereksinimini etkinleştirmenize yardımcı olur.

## <a name="prereq"></a>Koşul

Bu yapılandırma için önkoşul, [Kiracı yapılandırma](openvpn-azure-ad-tenant.md)içindeki adımları kullanarak yapılandırılmış BIR Azure AD kiracısıdır.

## <a name="mfa"></a>MFA sayfasını açın

1. Azure Portal’da oturum açın.
2. **Azure Active Directory > tüm kullanıcılara**gidin.
3. Multi-Factor Authentication sayfasını açmak için **Multi-Factor Authentication** ' ı seçin.

   ![Oturum aç](./media/openvpn-azure-ad-mfa/mfa1.jpg)

## <a name="users"></a>Kullanıcıları seçin

1. **Multi-Factor Authentication** SAYFASıNDA, MFA 'yı etkinleştirmek istediğiniz kullanıcıları seçin.
2. **Etkinleştir**’i seçin.

   ![Seçin](./media/openvpn-azure-ad-mfa/mfa2.jpg)

## <a name="enableauth"></a>Kimlik doğrulamasını etkinleştir

1. **Azure Active Directory > kurumsal uygulamalar-tüm uygulamalar >** ' ne gidin.
2. **Kurumsal uygulamalar-tüm uygulamalar** SAYFASıNDA **Azure VPN**' yi seçin.

   ![Dizin KIMLIĞI](./media/openvpn-azure-ad-mfa/user1.jpg)

## <a name="enablesign"></a>Oturum açma ayarlarını yapılandırma

**Azure VPN-Özellikler** sayfasında, oturum açma ayarlarını yapılandırın.

1. **Kullanıcıların oturum açması Için etkinleştirildi mi?** **Evet**olarak ayarlandı. Bu, AD kiracısındaki tüm kullanıcıların VPN 'e başarıyla bağlanmasını sağlar.
2. Oturum açma iznini yalnızca Azure VPN izinleri olan kullanıcılarla sınırlandırmak istiyorsanız, **Kullanıcı atamasını gerekli midir?** **Evet** olarak ayarlayın.
3. Yaptığınız değişiklikleri kaydedin.

   ![İzinler](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız gerekir. Bkz. Azure [Ile Noktadan siteye bağlantı Için Azure AD kimlik doğrulamasını yapılandırma](virtual-wan-point-to-site-azure-ad.md).
