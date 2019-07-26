---
title: Azure Active Directory Kimlik Koruması 'de Multi-Factor Authentication kayıt ilkesini yapılandırma | Microsoft Docs
description: Azure AD Kimlik Koruması Multi-Factor Authentication kayıt ilkesini nasıl yapılandıracağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 05/01/2019
author: MicrosoftGuyJFlo
manager: daveba
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 27ff7512bb3f9422ed4c8edd7ab50fce23f0ed07
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68499553"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Nasıl Yapılır: Azure Multi-Factor Authentication kayıt ilkesini yapılandırma

Azure AD Kimlik Koruması, bir koşullu erişim ilkesini, hangi modern kimlik doğrulama uygulamasına oturum açmanızın ne olduğuna bakılmaksızın MFA kaydı gerektirecek şekilde yapılandırarak Multi-Factor Authentication (MFA) kaydını yönetme konusunda yardımcı olur. Bu makalede, ilkenin kullanılabileceği ve nasıl yapılandırılacağı açıklanmaktadır.



## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>Azure Multi-Factor Authentication kayıt ilkesi nedir?

Azure Multi-Factor Authentication, yalnızca bir Kullanıcı adı ve paroladan fazlasını kullandığınızı doğrulamak için bir yol sağlar. Kullanıcı oturum açma işlemlerinin ikinci bir güvenlik katmanını sağlar. Kullanıcıların MFA istemlerine yanıt verebilmeleri için önce Azure Multi-Factor Authentication 'a kaydolmaları gerekir.

Kullanıcı oturum açma işlemleri için Azure Multi-Factor Authentication gerektirmenizi öneririz, çünkü:

- Bir dizi kolay doğrulama seçeneği ile güçlü kimlik doğrulaması sunar
- Kimlik koruması 'ndaki risk olaylarını korumak ve kurtarmak için kuruluşunuzu hazırlarken bir anahtar rol oynar

MFA hakkında daha fazla bilgi için bkz. [Azure Multi-Factor Authentication nedir?](../authentication/howto-mfa-getstarted.md)

## <a name="how-do-i-access-the-registration-policy"></a>Kayıt ilkesine erişmek Nasıl yaparım? mı?

MFA kayıt ilkesi, [Azure AD kimlik koruması sayfasındaki](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) **Yapılandır** bölümünde bulunur.

![MFA ilkesi](./media/howto-mfa-policy/1014.png)

## <a name="policy-settings"></a>İlke ayarları

MFA kayıt ilkesini yapılandırırken aşağıdaki yapılandırma değişikliklerini yapmanız gerekir:

- İlke için geçerli olan kullanıcılar ve gruplar. Kuruluşunuzun [acil erişim hesaplarının](../users-groups-roles/directory-emergency-access.md)dışlanmasını unutmayın.

    ![Kullanıcılar ve gruplar](./media/howto-mfa-policy/11.png)

- Zorlamak istediğiniz denetim- **Azure MFA kaydı gerektir**

    ![Access](./media/howto-mfa-policy/12.png)

- Ilkeyi zorla ayarı **Açık**olmalıdır.

    ![İlkeyi zorla](./media/howto-mfa-policy/14.png)

- İlkenizi **kaydetme**

## <a name="user-experience"></a>Kullanıcı deneyimi

Azure Active Directory Kimlik Koruması, kullanıcılarınıza etkileşimli olarak bir sonraki oturum açtıklarında kaydolmaları istenir.

İlgili Kullanıcı deneyimine genel bir bakış için bkz.:

- [Multi-Factor Authentication kayıt akışı](flows.md#multi-factor-authentication-registration).  
- [Azure AD kimlik koruması Ile oturum açma deneyimleri](flows.md).  

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Kimlik Koruması genel bakış almak için bkz. [Azure AD kimlik koruması genel bakış](overview.md).
