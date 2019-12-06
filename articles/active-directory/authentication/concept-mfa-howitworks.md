---
title: Azure MFA 'yı nasıl Azure Active Directory?
description: Azure Multi-Factor Authentication, kullanıcıların basit bir oturum açma işlemi taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur.
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
ms.openlocfilehash: 0982f6fb70cd6866af48feab640d5dc36bcb6b28
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74848689"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Nasıl kullanılır: Azure Multi-Factor Authentication

İki adımlı doğrulamanın güvenliği, katmanlı yaklaşımda yer alır. Birden çok kimlik doğrulama faktörüne karşı, saldırganlar için önemli bir zorluk gösterir. Bir saldırgan Kullanıcı parolasını öğrenmeden yönetse de, ek kimlik doğrulama yöntemine sahip olmadan de yararsız olur. Aşağıdaki kimlik doğrulama yöntemlerinin iki veya daha fazlasını gerektirerek işe yarar:

* Bildiğiniz bir şey (genellikle bir parola)
* Sahip olduğunuz bir şey (telefon gibi kolayca çoğaltılmayan güvenilir bir cihaz)
* Bir şeyler (Biyometri)

<center>

Kavramsal kimlik doğrulama yöntemleri görüntüsünü ![](./media/concept-mfa-howitworks/methods.png)</center>

Azure Multi-Factor Authentication (MFA), kullanıcılar için basitlik sağlarken veri ve uygulamalara erişimin korunmasına yardımcı olur. İkinci bir kimlik doğrulama biçimi gerektirerek ek güvenlik sağlar ve bir dizi kullanımı kolay [kimlik doğrulama yöntemi](concept-authentication-methods.md)aracılığıyla güçlü kimlik doğrulaması sunar. Kullanıcılar, bir yöneticinin yaptığı yapılandırma kararlarından dolayı MFA 'ya yönelik olarak istenebilir veya olmayabilir.

## <a name="how-to-get-multi-factor-authentication"></a>Multi-Factor Authentication’ı nasıl edinebilirim?

Multi-Factor Authentication aşağıdaki tekliflerin bir parçası olarak gelir:

* **Azure Active Directory Premium** veya **Microsoft 365 iş** -çok faktörlü kimlik doğrulaması gerektiren koşullu erişim ilkelerini kullanarak Azure Multi-Factor Authentication 'ın tam özellikli kullanımı.

* **Azure AD ücretsiz** veya tek başına **Office 365** lisansları-kullanıcılarınız ve yöneticileriniz için çok faktörlü kimlik doğrulaması gerektirmek üzere önceden oluşturulmuş [koşullu erişim temel koruma ilkelerini](../conditional-access/concept-baseline-protection.md) kullanın.

* **Azure Active Directory Genel Yöneticileri** - Genel yönetici hesaplarını korumanın bir yolu olarak Azure Multi-Factor Authentication özelliklerinin bir alt kümesi kullanılabilir.

> [!NOTE]
> Yeni müşteriler artık 1 Eylül 2018 ' de geçerli olan tek başına bir teklif olarak Azure Multi-Factor Authentication satın alabilirler. Multi-Factor Authentication, Azure AD Premium lisanlarında kullanılabilir bir özellik olmaya devam edecektir.

## <a name="supportability"></a>Desteklenebilirlik

Çoğu Kullanıcı, kimlik doğrulamak için yalnızca parolaları kullanmaya alışkın olduğundan, kuruluşunuzun bu işlemle ilgili tüm kullanıcılara iletişim kurduğu önemlidir. Tanıma, kullanıcıların MFA ile ilgili küçük sorunlar için yardım masasına çağrı olasılığını azaltabilir. Ancak, MFA 'nın geçici olarak devre dışı bırakılması gerektiği bazı senaryolar vardır. Bu senaryoların nasıl işleneceğini anlamak için aşağıdaki yönergeleri kullanın:

* Destek personelinizi, kullanıcının kimlik doğrulama yöntemlerine erişimi olmadığından veya düzgün çalışmadıklarından oturum açmak zorunda olmadığı senaryoları işleyecek şekilde eğitme.
   * Azure MFA hizmeti için koşullu erişim ilkelerini kullanarak, destek personeliniz, MFA gerektiren bir ilkeden dışlanan bir gruba kullanıcı ekleyebilir.
* İki adımlı doğrulama istemlerini en aza indirmek için bir yol olarak koşullu erişim adlı konumlar kullanmayı düşünün. Bu işlevle Yöneticiler, Yeni Kullanıcı ekleme için kullanılan ağ kesimi gibi güvenli bir güvenilen ağ konumundan oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayabilir.
* [Azure AD kimlik koruması](../active-directory-identityprotection.md) dağıtın ve risk algılamaları temelinde iki aşamalı doğrulamayı tetikleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Adım adım Azure Multi-Factor Authentication dağıtımı](howto-mfa-getstarted.md)
