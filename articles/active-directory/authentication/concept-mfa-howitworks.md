---
title: Azure MFA nasıl çalışır - Azure Active Directory
description: Azure Çok Faktörlü Kimlik Doğrulama, basit bir oturum açma işlemi için kullanıcı talebini karşılarken verilere ve uygulamalara erişimi korumaya yardımcı olur.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39948214f5bd080be417ed515bea6bff87d3b303
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77484069"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Nasıl çalışır: Azure Multi-Factor Authentication

İki aşamalı doğrulamanın güvenliği katmanlı yaklaşımında yatsın. Birden çok kimlik doğrulama etkeninden ödün verme saldırganlar için önemli bir sorun teşkil eder. Bir saldırgan kullanıcının parolasını öğrenmeyi başarsa bile, ek kimlik doğrulama yöntemine sahip olmadan da yararsızdır. Aşağıdaki kimlik doğrulama yöntemlerinden iki veya daha fazlasını gerektirerek çalışır:

* Bildiğiniz bir şey (genellikle bir parola)
* Sahip olduğunuz bir şey (telefon gibi kolayca çoğaltılamayan güvenilir bir aygıt)
* Bir şey (biyometrik)

<center>

![Kavramsal kimlik doğrulama yöntemleri görüntü](./media/concept-mfa-howitworks/methods.png)</center>

Azure Çok Faktörlü Kimlik Doğrulama (MFA), kullanıcılar için basitliği korurken verilere ve uygulamalara erişimi korumaya yardımcı olur. İkinci bir kimlik doğrulama biçimi gerektirerek ek güvenlik sağlar ve kullanımı kolay [kimlik doğrulama yöntemleri](concept-authentication-methods.md)ile güçlü kimlik doğrulaması sağlar. Kullanıcılar, yöneticinin verdiği yapılandırma kararlarına dayanarak MFA için meydan okuyabilir veya sorgulanmayabilir.

## <a name="how-to-get-multi-factor-authentication"></a>Multi-Factor Authentication’ı nasıl edinebilirim?

Multi-Factor Authentication aşağıdaki tekliflerin bir parçası olarak gelir:

* **Azure Active Directory Premium** veya **Microsoft 365 Business** - Çok faktörlü kimlik doğrulama yı gerektirecek Koşullu Erişim ilkelerini kullanarak Azure Çok Faktörlü Kimlik Doğrulaması'nın tam özellikli kullanımı.

* **Azure AD Ücretsiz** veya bağımsız **Office 365** lisansları - Kullanıcılarınız ve yöneticileriniz için çok faktörlü kimlik doğrulaması gerektirmek için [Güvenlik Varsayılanlarını](../fundamentals/concept-fundamentals-security-defaults.md) kullanın.

* **Azure Active Directory Genel Yöneticileri** - Genel yönetici hesaplarını korumanın bir yolu olarak Azure Multi-Factor Authentication özelliklerinin bir alt kümesi kullanılabilir.

> [!NOTE]
> Yeni müşteriler, 1 Eylül 2018 tarihinden itibaren geçerli olmak üzere bağımsız bir teklif olarak Azure Çok Faktörlü Kimlik Doğrulaması'nı artık satın alamaz. Çok faktörlü kimlik doğrulama, Azure AD Premium lisanslarında kullanılabilir bir özellik olmaya devam eder.

## <a name="supportability"></a>Desteklenebilirlik

Çoğu kullanıcı kimlik doğrulaması yapmak için yalnızca parola kullanmaya alışkın olduğundan, kuruluşunuzun bu işlemle ilgili olarak tüm kullanıcılarla iletişim kurması önemlidir. Farkındalık, kullanıcıların MFA ile ilgili küçük sorunlar için yardım masanızı arama olasılığını azaltabilir. Ancak, Geçici Olarak MFA devre dışı bırakılması gerekli olduğu bazı senaryolar vardır. Bu senaryoların nasıl işleyeceğini anlamak için aşağıdaki yönergeleri kullanın:

* Destek personelinizi, kimlik doğrulama yöntemlerine erişemedikleri veya doğru çalışmadıkları için kullanıcının oturum açamadığı senaryoları işlemesi için eğitin.
   * Azure MFA Hizmeti için Koşullu Erişim ilkelerini kullanarak, destek personeliniz MFA gerektiren bir ilke dışında olan bir gruba bir kullanıcı ekleyebilir.
* Koşullu Erişim adlı konumları iki adımlı doğrulama istemlerini en aza indirmek için kullanmayı düşünün. Bu işlevsellikle, yöneticiler, yeni kullanıcı onboarding için kullanılan ağ kesimi gibi güvenli bir güvenilen ağ konumundan oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayabilir.
* [Azure AD Kimlik Koruması'nı](../active-directory-identityprotection.md) dağıtın ve risk algılamalarını temel alarak iki aşamalı doğrulamayı tetikle.

## <a name="next-steps"></a>Sonraki adımlar

- [Adım adım Azure Çok Faktörlü Kimlik Doğrulama dağıtımı](howto-mfa-getstarted.md)
