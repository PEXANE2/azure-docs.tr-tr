---
title: "VPN kullanıcıları için MFA 'yı etkinleştirme: Azure AD kimlik doğrulaması"
description: VPN kullanıcıları için Multi-Factor Authentication 'ı etkinleştirme
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: alzam
ms.openlocfilehash: 771dea2d9ae2979bc71880368ed3a9a538e8a803
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964759"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>VPN kullanıcıları için Azure Multi-Factor Authentication (MFA) etkinleştirme

Erişim vermeden önce kullanıcılardan ikinci bir kimlik doğrulama faktörü istenmesini istiyorsanız, Azure Multi-Factor Authentication (MFA) ' ı Kullanıcı temelinde yapılandırabilir veya daha ayrıntılı denetim için [koşullu erişim](../active-directory/conditional-access/overview.md) aracılığıyla MULTI-Factor AUTHENTICATION (MFA) özelliğinden yararlanabilirsiniz. Kullanıcı başına Multi-Factor Authentication yapılandırmak, ek bir ücret ödemeden etkinleştirilebilir, ancak kullanıcı başına MFA 'yı etkinleştirirken, kullanıcıdan Azure AD kiracısına bağlı tüm uygulamalarda ikinci faktör kimlik doğrulaması istenir. Koşullu erişim, ikinci bir faktörün yükseltilmesi gereken daha ince denetime izin verir ve MFA 'nın Azure AD kiracısına bağlı diğer uygulamalara değil yalnızca VPN 'e atanmasına izin verebilir.

## <a name="enableauth"></a>Kimlik doğrulamasını etkinleştir

1. **Azure Active Directory > kurumsal uygulamalar-tüm uygulamalar >** ' ne gidin.
2. **Kurumsal uygulamalar-tüm uygulamalar** SAYFASıNDA **Azure VPN**' yi seçin.

   ![Dizin KIMLIĞI](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/user1.jpg)

## <a name="enablesign"></a>Oturum açma ayarlarını yapılandırma

**Azure VPN-Özellikler** sayfasında, oturum açma ayarlarını yapılandırın.

1. **Kullanıcıların oturum açması Için etkinleştirildi mi?** **Evet**olarak ayarlandı. Bu, AD kiracısındaki tüm kullanıcıların VPN 'e başarıyla bağlanmasını sağlar.
2. Oturum açma iznini yalnızca Azure VPN izinleri olan kullanıcılarla sınırlandırmak istiyorsanız, **Kullanıcı atamasını gerekli midir?** **Evet** olarak ayarlayın.
3. Yaptığınız değişiklikleri kaydedin.

   ![İzinler](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="option-1---enable-multi-factor-authentication-mfa-via-conditional-access"></a>Seçenek 1-koşullu erişim aracılığıyla Multi-Factor Authentication (MFA) özelliğini etkinleştirme

Koşullu erişim, uygulama başına ayrıntılı erişim denetimi sağlar.  Koşullu erişimin yararlanması için, koşullu erişim kurallarına tabi olacak Azure AD Premium 1 veya daha fazla lisansın uygulanmış olması gerektiğini lütfen unutmayın.

1. **Kurumsal uygulamalar-tüm uygulamalar** SAYFASıNDA **Azure VPN**' yi seçin, **koşullu erişim**' i seçin ve **Yeni ilke**' ye tıklayın.
2. Kullanıcılar ve gruplar altında, *dahil et* sekmesinde **Kullanıcılar ve gruplar**' ı seçin, **Kullanıcılar ve gruplar**' ı işaretleyin ve MFA 'ya tabi olması gereken bir grup veya kullanıcı kümesi seçin.  **Bitti**’ye tıklayın.
![Atamalar](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-assignments.png)
3. **Grant**' ın altında, **erişim ver**' i işaretleyin, **çok faktörlü kimlik doğrulaması gerektir**' i işaretleyin, **Tüm Seçili denetimleri gerektir**' i işaretleyin ve **Seç** düğmesine tıklayın.
![erişim-MFA](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-grant-mfa.png)
4. **Ilkeyi etkinleştir** **' in altından** **Oluştur** düğmesine tıklayın.
Ilkeyi etkinleştirmek ![](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-enable-policy.png)

## <a name="option-2---enable-multi-factor-authentication-mfa-per-user"></a>Seçenek 2-Kullanıcı başına Multi-Factor Authentication (MFA) etkinleştir

[!INCLUDE [MFA steps](../../includes/vpn-gateway-vwan-openvpn-azure-ad-mfa.md)]

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız gerekir. Bkz. [P2S VPN bağlantıları IÇIN VPN Istemcisi yapılandırma](openvpn-azure-ad-client.md).
