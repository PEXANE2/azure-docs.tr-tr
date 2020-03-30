---
title: Kuruluşunuz için Azure Çok Faktörlü Kimlik Doğrulama - Azure Etkin Dizini
description: Lisans modelinize dayalı olarak kuruluşunuz için Azure Çok Faktörlü Kimlik Doğrulama'nın kullanılabilir özellikleri hakkında bilgi edinin
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
ms.openlocfilehash: febdb708c637ac322c0ca884eae627da9bd5904c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530402"
---
# <a name="overview-of-azure-multi-factor-authentication-for-your-organization"></a>Kuruluşunuz için Azure Çok Faktörlü Kimlik Doğrulama'ya Genel Bakış

Kuruluşunuzun sahip olduğu lisansları temel alarak Azure Etkin Dizin (AD) kullanıcılarınız için Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmenin birden çok yolu vardır. 

![Sinyalleri araştırın ve gerekirse MFA'yı uygulayın](./media/concept-fundamentals-mfa-get-started/verify-signals-and-perform-mfa-if-required.png)

Çalışmalarımıza dayanarak, çok faktörlü kimlik doğrulama (MFA) kullanırsanız hesabınızın %99,9'dan daha az tehlikeye girme olasılığı daha yüksektir.

Peki nasıl kuruluşunuz bir istatistik haline gelmeden önce, ücretsiz bile MFA açmak mı?

## <a name="free-option"></a>Ücretsiz seçenek

Azure AD'nin ücretsiz avantajlarından yararlanan müşteriler, ortamlarında çok faktörlü kimlik doğrulamayı etkinleştirmek için [güvenlik varsayılanlarını](../fundamentals/concept-fundamentals-security-defaults.md) kullanabilir.

## <a name="office-365-business-premium-e3-or-e5"></a>Office 365 Business Premium, E3 veya E5

Office 365'e sahip müşteriler için iki seçenek vardır:

* Azure Çok Faktörlü Kimlik Doğrulama, tüm oturum açma etkinlikleri için tüm kullanıcılar için etkin veya devre dışı bırakılır. Yalnızca bir kullanıcı alt kümesi için veya yalnızca belirli senaryolar altında çok faktörlü kimlik doğrulamayı etkinleştirme olanağı yoktur. Yönetim, Office 365 portalı üzerinden yapılır. 
* Geliştirilmiş bir kullanıcı deneyimi için Azure AD Premium P1 veya P2'ye yükseltin ve Koşullu Erişim'i kullanın. Daha fazla bilgi için, çok faktörlü kimlik doğrulamalı güvenli Office 365 kaynaklarına bakın.

## <a name="azure-ad-premium-p1"></a>Azure AD Premium P1

Azure AD Premium P1 veya Enterprise Mobility + Security E3, Microsoft 365 F1 veya Microsoft 365 E3 gibi bu işlevselliği içeren benzer lisanslara sahip müşteriler için: 

Belirli senaryolar veya etkinlikler sırasında iş gereksinimlerinize uyacak şekilde kullanıcılardan çok faktörlü kimlik doğrulaması için komut istemi yapmak için [Azure AD Koşullu Erişim'i](../conditional-access/overview.md) kullanın.

## <a name="azure-ad-premium-p2"></a>Azure AD Premium P2

Azure AD Premium P2 veya Enterprise Mobility + Security E5 veya Microsoft 365 E5 gibi bu işlevselliği içeren benzer lisanslara sahip müşteriler için: 

En güçlü güvenlik konumunu ve geliştirilmiş kullanıcı deneyimini sağlar. Azure AD Premium P1 özelliklerine, kullanıcının desenlerine uyum sağlayan ve çok faktörlü kimlik doğrulama istemlerini en aza indiren [risk tabanlı Koşullu Erişim](../conditional-access/howto-conditional-access-policy-risk.md) ekler.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

|   | Güvenlik varsayılanları | Diğer tüm yöntemler |
| --- | --- | --- |
| Mobil uygulama üzerinden bildirim | X | X |
| Mobil uygulama veya donanım belirteci doğrulama kodu |   | X |
| Telefona kısa mesaj |   | X |
| Telefona çağrı |   | X |

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için Azure Çok [Faktörlü Kimlik Doğrulama ile kullanıcı oturum açma etkinliklerini güvence altına](../authentication/tutorial-enable-azure-mfa.md)alan öğreticiye bakın.

Lisanslama hakkında daha fazla bilgi için [Azure Çok Faktörlü Kimlik Doğrulama](../authentication/concept-mfa-licensing.md)özellikleri ve lisanslarına bakın.
