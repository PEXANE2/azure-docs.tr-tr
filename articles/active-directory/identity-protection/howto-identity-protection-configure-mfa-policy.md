---
title: MFA kayıt ilkesini Yapılandırma-Azure Active Directory Kimlik Koruması
description: Azure AD Kimlik Koruması Multi-Factor Authentication kayıt ilkesini nasıl yapılandıracağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 06/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4c6b57eaa2a68906053faade48dd0e63fbf0db7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84464343"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Nasıl yapılır: Azure Multi-Factor Authentication kayıt ilkesini yapılandırma

Azure AD Kimlik Koruması, bir koşullu erişim ilkesini, oturum açmak istediğiniz modern kimlik doğrulama uygulamasına bakılmaksızın MFA kaydı gerektirecek şekilde yapılandırarak Azure Multi-Factor Authentication (MFA) kaydını yönetmenize yardımcı olur.

## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>Azure Multi-Factor Authentication kayıt ilkesi nedir?

Azure Multi-Factor Authentication, yalnızca bir Kullanıcı adı ve paroladan fazlasını kullandığınızı doğrulamak için bir yol sağlar. Kullanıcı oturum açma işlemlerinin ikinci bir güvenlik katmanını sağlar. Kullanıcıların MFA istemlerine yanıt verebilmeleri için önce Azure Multi-Factor Authentication 'e kaydolmaları gerekir.

Kullanıcı oturum açma işlemleri için Azure Multi-Factor Authentication gerektirmenizi öneririz, çünkü:

- Bir dizi doğrulama seçeneği aracılığıyla güçlü kimlik doğrulaması sunar.
- Kuruluşunuzun kimlik korumasında risk algılamalarını kendi kendine düzeltmesini sağlamak için temel bir rol oynar.

Azure Multi-Factor Authentication hakkında daha fazla bilgi için bkz. [azure Multi-Factor Authentication nedir?](../authentication/howto-mfa-getstarted.md)

## <a name="policy-configuration"></a>İlke yapılandırması

1. [Azure portalına](https://portal.azure.com) gidin.
1. **Azure Active Directory**  >  **güvenlik**  >  **kimlik koruması**  >  **MFA kayıt ilkesi**' ne gidin.
   1. **Atamalar** altında
      1. **Kullanıcılar** - **tüm kullanıcılar** ' ı seçin veya dağıtımı sınırlandırdıysanız **bireyler ve gruplar ' ı seçin** .
         1. İsteğe bağlı olarak, kullanıcıların ilkeden hariç tutulmasını seçebilirsiniz.
   1. **Denetimler** altında
      1. CheckBox 'ın **Azure MFA kaydının** seçili olduğundan emin olun ve **Seç ' i**seçin.
   1. **Ilkeyi zorla**  -  **Üzerinde**
   1. **Kaydet**

## <a name="user-experience"></a>Kullanıcı deneyimi

Azure Active Directory Kimlik Koruması, kullanıcılarınıza etkileşimli olarak oturum açtıklarında kaydolmaları istenir ve kayıt işleminin tamamlanışında 14 gün daha olur. Bu 14 günlük süre boyunca kayıt atlayabilir, ancak sürenin sonunda oturum açma işlemini tamamlayabilmeleri için önce kaydolmaları gerekecektir.

İlgili Kullanıcı deneyimine genel bir bakış için bkz.:

- [Azure AD kimlik koruması Ile oturum açma deneyimleri](concept-identity-protection-user-experience.md).  

## <a name="next-steps"></a>Sonraki adımlar

- [Oturum açma ve Kullanıcı risk ilkelerini etkinleştir](howto-identity-protection-configure-risk-policies.md)

- [Azure AD self servis parola sıfırlamayı etkinleştirme](../authentication/howto-sspr-deployment.md)

- [Azure Multi-Factor Authentication’ı etkinleştirme](../authentication/howto-mfa-getstarted.md)
