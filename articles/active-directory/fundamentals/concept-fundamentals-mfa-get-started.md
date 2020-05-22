---
title: Kuruluşunuz için Azure Multi-Factor Authentication-Azure Active Directory
description: Lisans modelinize bağlı olarak, kuruluşunuz için Azure Multi-Factor Authentication 'nin kullanılabilir özellikleri hakkında bilgi edinin
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1db133f52a99bd716c2228add0573e8d44255c9e
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83736529"
---
# <a name="overview-of-azure-multi-factor-authentication-for-your-organization"></a>Kuruluşunuz için Azure Multi-Factor Authentication genel bakış

Kuruluşunuzun sahip olduğu lisanslara göre Azure Active Directory (AD) kullanıcılarınız için Azure Multi-Factor Authentication etkinleştirmenin birden çok yolu vardır. 

![Gerektiğinde sinyalleri araştırın ve MFA 'yı uygulayın](./media/concept-fundamentals-mfa-get-started/verify-signals-and-perform-mfa-if-required.png)

Çalışmalarımıza göre, Multi-Factor Authentication (MFA) kullanıyorsanız, hesabınız% 99,9 ' den daha az olabilir.

Bu nedenle kuruluşunuz, istatistik olmadan önce, ücretsiz olarak MFA 'yı nasıl açabilir?

## <a name="free-option"></a>Ücretsiz seçenek

Azure AD 'nin ücretsiz avantajlarından yararlanan müşteriler, ortamınızda Multi-Factor Authentication 'ı etkinleştirmek için [güvenlik varsayılanlarını](../fundamentals/concept-fundamentals-security-defaults.md) kullanabilir.

## <a name="office-365-business-premium-e3-or-e5"></a>Office 365 Business Premium, E3 veya E5

Office 365 ile müşteriler için iki seçenek vardır:

* Tüm oturum açma olayları için Azure Multi-Factor Authentication, tüm kullanıcılar için etkin veya devre dışı bırakılmış. Yalnızca bir kullanıcı alt kümesi için veya yalnızca belirli senaryolarda Multi-Factor Authentication 'ı etkinleştirebilme özelliği yoktur. Yönetim, Office 365 portalından yapılır. 
* Gelişmiş bir kullanıcı deneyimi için Azure AD Premium P1 veya P2 sürümüne yükseltin ve koşullu erişimi kullanın. Daha fazla bilgi için bkz. Multi-Factor Authentication ile güvenli Office 365 kaynakları.

## <a name="azure-ad-premium-p1"></a>Azure AD Premium P1

Enterprise Mobility + Security E3, Microsoft 365 F1 veya Microsoft 365 E3 gibi bu işlevselliği içeren Azure AD Premium P1 veya benzer lisanslara sahip müşteriler için: 

Belirli senaryolar veya olaylar sırasında iş gereksinimlerinize uyacak şekilde kullanıcılardan çok faktörlü kimlik doğrulaması için kullanıcı istemek üzere [Azure AD koşullu erişimi](../authentication/tutorial-enable-azure-mfa.md) kullanın.

## <a name="azure-ad-premium-p2"></a>Azure AD Premium P2

Enterprise Mobility + Security E5 veya Microsoft 365 E5 gibi bu işlevselliği içeren Azure AD Premium P2 veya benzer lisansları olan müşteriler için: 

En güçlü güvenlik konumunu ve geliştirilmiş Kullanıcı deneyimini sağlar. Kullanıcının desenlerine uyum sağlayan Azure AD Premium P1 özelliklerine [risk tabanlı koşullu erişim](../conditional-access/howto-conditional-access-policy-risk.md) ekler ve Multi-Factor Authentication istemlerini en aza indirir.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

|   | Güvenlik varsayılanları | Diğer tüm Yöntemler |
| --- | --- | --- |
| Mobil uygulama aracılığıyla bildirim | X | X |
| Mobil uygulama veya donanım belirtecinden doğrulama kodu |   | X |
| Telefona kısa mesaj |   | X |
| Telefon çağrısı |   | X |

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [Azure Multi-Factor Authentication ile Kullanıcı oturum açma olaylarını güvenli hale getirme](../authentication/tutorial-enable-azure-mfa.md)öğreticisine bakın.

Lisanslama hakkında daha fazla bilgi için bkz. [Azure Multi-Factor Authentication Için Özellikler ve lisanslar](../authentication/concept-mfa-licensing.md).
