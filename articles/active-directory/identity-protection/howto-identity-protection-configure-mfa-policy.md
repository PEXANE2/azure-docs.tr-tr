---
title: MFA kayıt ilkesini yapılandırın - Azure Active Directory Identity Protection
description: Azure AD Kimlik Koruması çok faktörlü kimlik doğrulama kayıt ilkesini nasıl yapılandıracaksınız öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2fa6a4cf184b426355f62117ea51642127eee529
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74382136"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Nasıl Yapilir: Azure Çok Faktörlü Kimlik Doğrulama kayıt ilkesini yapılandırma

Azure AD Kimlik Koruması, hangi modern kimlik doğrulama uygulamasında oturum açtırdığınız önemli olacak koşullu erişim ilkesini MFA kaydı gerektirecek şekilde yapılandırarak Azure Çok Faktörlü Kimlik Doğrulama (MFA) kaydının kullanıma sunulmasını yönetmenize yardımcı olur.

## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>Azure Çok Faktörlü Kimlik Doğrulama kayıt ilkesi nedir?

Azure Çok Faktörlü Kimlik Doğrulama, bir kullanıcı adı ve paroladan daha fazlasını kullandığınızı doğrulamak için bir araç sağlar. Kullanıcı oturum açmalarına ikinci bir güvenlik katmanı sağlar. Kullanıcıların MFA istemlerine yanıt verebilmeleri için öncelikle Azure Çok Faktörlü Kimlik Doğrulama'ya kaydolmaları gerekir.

Kullanıcı oturum açmaları için Azure Çok Faktörlü Kimlik Doğrulaması'na şunları yapmanızı öneririz:

- Çeşitli doğrulama seçenekleri aracılığıyla güçlü kimlik doğrulaması sağlar.
- Kuruluşunuzun Kimlik Koruması'nda risk algılamalarından kendini düzeltmeye hazırlanmasında önemli bir rol oynar.

Azure Çok Faktörlü Kimlik Doğrulama hakkında daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama nedir?](../authentication/howto-mfa-getstarted.md)

## <a name="policy-configuration"></a>İlke yapılandırması

1. [Azure portalına](https://portal.azure.com) gidin.
1. **Azure Etkin Dizin** > **Güvenlik** > **Kimlik Koruması** > **MFA kayıt ilkesine**göz atın.
   1. **Atamalar** Altında
      1. **Kullanıcılar** - Ürününüzü sınırlandırıyorsa **Tüm kullanıcıları** seçin veya kişileri ve **grupları seçin.**
         1. İsteğe bağlı olarak, kullanıcıları ilkeden hariç tutmayı seçebilirsiniz.
   1. **Denetimler** Altında
      1. Onay kutusunun **Azure MFA kaydının** işaretli olduğundan emin olun ve **Seç'i**seçin.
   1. **Politikayı** - **Uygula**
   1. **Kaydet**

## <a name="user-experience"></a>Kullanıcı deneyimleri

Azure Active Directory Identity Protection, kullanıcılarınızdan bir sonraki oturum açtıklarında etkileşimli olarak kaydolmalarını ister ve kaydı tamamlamak için 14 günleri olur. Bu 14 günlük süre boyunca, kaydı atlayabilirler, ancak bu sürenin sonunda oturum açma işlemini tamamlamadan önce kayıt yaptırmaları gerekecektir.

İlgili kullanıcı deneyimine genel bir bakış için bkz:

- [Azure AD Kimlik Koruması ile oturum açma deneyimleri.](concept-identity-protection-user-experience.md)  

## <a name="next-steps"></a>Sonraki adımlar

- [Oturum açma ve kullanıcı risk ilkelerini etkinleştirme](howto-identity-protection-configure-risk-policies.md)

- [Azure AD self servis parola sıfırlamayı etkinleştirme](../authentication/howto-sspr-deployment.md)

- [Azure Multi-Factor Authentication’ı etkinleştirme](../authentication/howto-mfa-getstarted.md)
