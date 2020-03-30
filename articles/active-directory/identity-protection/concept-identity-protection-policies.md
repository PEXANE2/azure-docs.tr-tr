---
title: Azure AD Kimlik Koruması ilkeleri
description: Kimlik Koruması ile etkinleştirilen üç ilkeyi tanımlama
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
ms.openlocfilehash: 4a44e32efa3e38cf9c4df7dc00e3189c129db418
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72887422"
---
# <a name="identity-protection-policies"></a>Kimlik Koruması ilkeleri

Azure Etkin Dizin Kimlik Koruması, yöneticilerin etkinleştirmeyi seçebileceği üç varsayılan ilke içerir. Bu ilkeler sınırlı özelleştirme içerir, ancak çoğu kuruluş için geçerlidir. Tüm [ilkeler, acil durum erişiminiz veya kesme cam yöneticisi hesaplarınız](../users-groups-roles/directory-emergency-access.md)gibi kullanıcıları hariç taramaya olanak sağlar.

![Kimlik Koruması ilkeleri](./media/concept-identity-protection-policies/identity-protection-policies.png)

## <a name="azure-mfa-registration-policy"></a>Azure MFA kayıt ilkesi

Kimlik Koruması, kuruluşların oturum açma sırasında kayıt gerektiren Koşullu Erişim ilkesini kullanarak Azure Çok Faktörlü Kimlik Doğrulaması (MFA) kullanıma sunulmasına yardımcı olabilir. Bu iilenin etkinleştirilmesi, kuruluşunuzdaki yeni kullanıcıların ilk gün MFA'ya kaydolmasını sağlamanın harika bir yoludur. Çok faktörlü kimlik doğrulama, Kimlik Koruması içindeki risk olayları için kendi kendini düzeltme yöntemlerinden biridir. Kendi kendini düzeltme, kullanıcılarınızın yardım masası çağrı hacmini azaltmak için kendi başlarına harekete geçmelerine olanak tanır.

Azure Çok Faktörlü Kimlik Doğrulama hakkında daha fazla bilgiyi makalede bulabilirsiniz, [Nasıl çalışır: Azure Çok Faktörlü Kimlik Doğrulama](../authentication/concept-mfa-howitworks.md).

## <a name="sign-in-risk-policy"></a>Oturum açma risk ilkesi

Kimlik Koruması, hem gerçek zamanlı hem de çevrimdışı olarak her oturum açma sırasındaki sinyalleri analiz eder ve oturum açmanın kullanıcı tarafından gerçekleştirilme olasılığına bağlı olarak bir risk puanı hesaplar. Yöneticiler, kuruluş gereksinimlerini uygulamak için bu risk puanı sinyaline dayalı bir karar verebilir. Yöneticiler erişimi engellemeyi seçebilir, erişime izin verebilir veya erişime izin verebilir, ancak çok faktörlü kimlik doğrulaması gerektirebilir.

Risk algılanırsa, kullanıcılar yöneticiler için gereksiz gürültüyü önlemek için riskli oturum açma olayını kendi kendine düzeltmek ve kapatmak için çok faktörlü kimlik doğrulama gerçekleştirebilir.

> [!NOTE] 
> Oturum açma risk ilkesini tetiklemeden önce kullanıcıların Azure Çok Faktörlü Kimlik Doğrulaması'na daha önce kaydolmuş olması gerekir.

### <a name="custom-conditional-access-policy"></a>Özel Koşullu Erişim ilkesi

Yöneticiler ayrıca, atama koşulu olarak oturum açma riskini de içeren özel bir Koşullu Erişim ilkesi oluşturmayı da seçebilirler. Koşullu Erişim hakkında daha fazla bilgi makalede bulunabilir, [Koşullu Erişim nedir?](../conditional-access/overview.md)

![Özel Koşullu Erişim oturum açma risk ilkesi](./media/concept-identity-protection-policies/identity-protection-custom-sign-in-policy.png)

## <a name="user-risk-policy"></a>Kullanıcı risk ilkesi

Kimlik Koruması, bir kullanıcının davranışı için normal olduğuna inandığı şeyi hesaplayabilir ve bunu riskleri için kararları temel almak için kullanabilir. Kullanıcı riski, bir kimliğin tehlikeye girme olasılığının bir hesaplamasıdır. Yöneticiler, kuruluş gereksinimlerini uygulamak için bu risk puanı sinyaline dayalı bir karar verebilir. Yöneticiler erişimi engellemeyi, erişime izin vermeyi seçebilir, ancak [Azure AD self servis parola sıfırlamayı](../authentication/howto-sspr-deployment.md)kullanarak parola değişikliği gerektirebilir.

Risk algılanırsa, kullanıcılar yöneticiler için gereksiz gürültüyü önlemek için self servis parola sıfırlama gerçekleştirebilir ve kullanıcı riski olayını kapatabilir.

> [!NOTE] 
> Kullanıcıların, kullanıcı risk ilkesini tetiklemeden önce self servis parola sıfırlama için daha önce kaydolmuş olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD self servis parola sıfırlamayı etkinleştirme](../authentication/howto-sspr-deployment.md)

- [Azure Multi-Factor Authentication’ı etkinleştirme](../authentication/howto-mfa-getstarted.md)

- [Azure Çok Faktörlü Kimlik Doğrulama kayıt ilkesini etkinleştirme](howto-identity-protection-configure-mfa-policy.md)

- [Oturum açma ve kullanıcı risk ilkelerini etkinleştirme](howto-identity-protection-configure-risk-policies.md)
