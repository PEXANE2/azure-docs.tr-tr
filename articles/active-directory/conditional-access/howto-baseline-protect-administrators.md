---
title: Temel ilke, Yöneticiler için MFA gerektiriyor-Azure Active Directory
description: Yöneticiler için çok faktörlü kimlik doğrulaması gerektiren koşullu erişim ilkesi
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: b00b061b1763d4b4e7236d8dc9ac1eedf7f923bc
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533025"
---
# <a name="baseline-policy-require-mfa-for-admins-preview"></a>Temel ilke: Yöneticiler için MFA gerektir (Önizleme)

Ayrıcalıklı hesaplara erişimi olan kullanıcıların ortamınıza sınırsız erişimi vardır. Bu hesapların gücüyle ilgili olan güç nedeniyle, onlara özel bir dikkatli davranmanız gerekir. Ayrıcalıklı hesapların korunmasını artırmanın yaygın bir yöntemi, oturum açmak için kullanıldıkları zaman daha güçlü bir hesap doğrulama biçimi gerektirmaktır. Azure Active Directory, çok faktörlü kimlik doğrulaması (MFA) gerektirerek daha güçlü bir hesap doğrulaması alabilirsiniz.

**Yöneticiler IÇIN MFA gerektir (Önizleme)** , aşağıdaki ayrıcalıklı yönetici rollerden her biri oturum açtığında MFA gerektiren bir [temel ilkedir](concept-baseline-protection.md) :

* Genel yönetici
* SharePoint yöneticisi
* Exchange yöneticisi
* Koşullu Erişim yöneticisi
* Güvenlik yöneticisi
* Yardım Masası Yönetici/Parola Yöneticisi
* Faturalama yöneticisi
* Kullanıcı yöneticisi

Yöneticiler için MFA gerektir ilkesi etkinleştirildikten sonra, Authenticator uygulamasını kullanarak MFA 'ya kaydolmak için yukarıdaki dokuz yönetici rolü gerekecektir. MFA kaydı tamamlandıktan sonra, yöneticilerin her oturum açtıklarında MFA gerçekleştirmesi gerekir.

## <a name="deployment-considerations"></a>Dağıtma konuları

**Yöneticiler IÇIN MFA gerektir (Önizleme)** ilkesi tüm kritik Yöneticiler için geçerli olduğundan, kesintisiz bir dağıtım sağlamak için çeşitli hususlar yapılmalıdır. Bu noktalara, Azure AD 'deki kullanıcıları ve hizmet ilkelerini, MFA 'yı gerektirmeyen veya bu uygulamaların yanı sıra, kuruluşunuz tarafından modern kimlik doğrulamayı desteklemeyen uygulamalar ve istemciler için tanımlama sayılabilir.

### <a name="legacy-protocols"></a>Eski protokoller

Eski kimlik doğrulama protokolleri (IMAP, SMTP, POP3, vb.), posta istemcileri tarafından kimlik doğrulama isteklerini yapmak için kullanılır. Bu protokoller MFA 'yı desteklemez. Microsoft tarafından görülen hesabın büyük bir çoğunluğu, kötü aktörlerin, MFA 'yı atlamaya çalışan eski protokollere karşı saldırı gerçekleştirmesine neden olmuştur. Bir yönetim hesabında oturum açarken MFA 'nın gerekli olduğundan ve kötü aktörlerin MFA 'yı atlayabilmesi için, bu ilke eski protokollerden yönetici hesaplarına yapılan tüm kimlik doğrulama isteklerini engeller.

> [!WARNING]
> Bu ilkeyi etkinleştirmeden önce, yöneticilerin eski kimlik doğrulama protokollerini kullanmıyor olduğundan emin olun. Bkz. nasıl yapılır: [ Daha fazla bilgi için koşullu erişimle](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) Azure AD 'ye yönelik eski kimlik doğrulamasını engelleyin.

## <a name="enable-the-baseline-policy"></a>Temel ilkeyi etkinleştirme

İlke **temel ilkesi: Yöneticiler için MFA (Önizleme)** önceden yapılandırılmış olarak gelir ve Azure Portal ' deki koşullu erişim dikey penceresine gittiğinizde en üstte görünür.

Bu ilkeyi etkinleştirmek ve yöneticilerinizi korumak için:

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Koşullu erişimi** **Azure Active Directory** > için gidin.
1. İlke listesinde temel ilke ' yi seçin **: Yöneticiler için MFA gerektir (Önizleme)** .
1. İlkeyi **ilkeyi hemen kullanacak**şekilde ayarlayın.
1. **Kaydet**’e tıklayın.

> [!WARNING]
> Bu ilke Önizlemedeki bir ilke daha **sonra otomatik olarak etkinleştir** seçeneği vardı. Ani Kullanıcı etkisini en aza indirmek için bu seçeneği kaldırdık. Kullanılabilir olduğunda bu seçeneği belirlediyseniz, **ilkeyi kullanma** otomatik olarak seçilidir. Bu temel ilkeyi kullanmak istiyorlarsa, etkinleştirmek için yukarıdaki adımlara bakın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Koşullu erişim temel koruma ilkeleri](concept-baseline-protection.md)
* [Kimlik altyapınızı güvenli hale getirmenin beş adımı](../../security/fundamentals/steps-secure-identity.md)
* [Azure Active Directory Koşullu erişim nedir?](overview.md)
