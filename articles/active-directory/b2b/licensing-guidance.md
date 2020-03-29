---
title: B2B işbirliği lisanslama kılavuzu - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory B2B işbirliği ücretli Azure AD lisansları gerektirmez, ancak B2B konuk kullanıcıları için ücretli özellikler de alabilirsiniz
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28d02160163da4081ad8adbe233b27fee970a0df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74868856"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Azure Active Directory B2B işbirliği lisanslama kılavuzu

Azure Etkin Dizin (Azure AD) işletmeden işletmeye (B2B) işbirliğiyle, Harici Kullanıcıları (veya "konuk kullanıcıları") ücretli Azure REKLAM hizmetlerinizi kullanmaya davet edebilirsiniz. Bazı özellikler ücretsizdir, ancak ücretli Azure AD özellikleri için, kiracınızdaki bir çalışan veya konuk olmayan bir kullanıcı için sahip olduğunuz her Azure REKLAM sürümü lisansı için en fazla beş konuk kullanıcı davet edebilirsiniz.

> [!NOTE]
> Azure AD fiyatlandırması ve B2B işbirliği özellikleri hakkında ayrıntılar için [Azure Etkin Dizin fiyatlandırmasına](https://azure.microsoft.com/pricing/details/active-directory/) bakın.

B2B konuk kullanıcı lisanslama otomatik olarak hesaplanır ve 1:5 oranına göre raporlanır. 

Ayrıca, konuk kullanıcılar ek lisans gereksinimleri olmadan ücretsiz Azure REKLAM özelliklerini kullanabilir. Ücretli Azure REKLAM lisanslarınız olmasa bile konuk kullanıcılar ücretsiz Azure REKLAM özelliklerine erişebilir. 

## <a name="examples-calculating-guest-user-licenses"></a>Örnekler: Konuk kullanıcı lisanslarının hesaplanması
Ücretli Azure REKLAM hizmetlerinize kaç konuk kullanıcının erişmesi gerektiğini belirledikten sonra, konuk kullanıcıları gerekli 1:5 oranında kapsayacak kadar Azure AD ücretli lisansınız olduğundan emin olun. İşte bazı örnekler:

- 100 konuk kullanıcıyı Azure REKLAM uygulamalarınız veya hizmetlerinize davet etmek ve erişim yönetimi ve sağlama sağlamak istiyorsunuz. Bu konuk kullanıcıların 50'si için MFA ve Koşullu Erişim'e de ihtiyacınız vardır, bu nedenle bu özellikler için 10 Azure AD Premium P1 lisansına ihtiyacınız vardır. Kimlik Koruması özelliklerini konuk kullanıcılarınızla birlikte kullanmayı planlıyorsanız, konuk kullanıcıları kapsayacak şekilde aynı 1:5 oranında Azure AD Premium P2 lisanslarına ihtiyacınız olacaktır.
- MFA gerektiren 60 konuk kullanıcıyı davet etmek istiyorsunuz, bu nedenle en az 12 Azure AD Premium P1 lisansına sahip olmalısınız. 1:5 lisans oranı altında en fazla 50 konuk kullanıcıya olanak tanıyan Azure AD Premium P1 lisanslarına sahip 10 çalışanınvardır. 10 ek konuk kullanıcıyı kapsayacak şekilde iki ek Premium P1 lisansı satın almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği yle ilgili aşağıdaki kaynaklara bakın:

* [Azure Active Directory fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Azure Active Directory B2B işbirliği hakkında sık sorulan sorular (SSS)](faq.md)
