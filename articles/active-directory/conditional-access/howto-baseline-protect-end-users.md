---
title: Temel ilke Son Kullanıcı koruması (Önizleme)-Azure Active Directory
description: Kullanıcılar için çok faktörlü kimlik doğrulaması gerektiren koşullu erişim ilkesi
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
ms.openlocfilehash: 0f1a2e0bad39b54edc153416e4120bbc6912578c
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125460"
---
# <a name="baseline-policy-end-user-protection-preview"></a>Temel ilke: Son Kullanıcı koruması (Önizleme)

Yönetici hesaplarının, çok faktörlü kimlik doğrulamasıyla (MFA) korunması gereken tek hesap olduğunu düşündük. Yöneticiler hassas bilgilere geniş erişime sahiptir ve abonelik genelindeki ayarlarda değişiklik yapabilir. Ancak, kötü aktör hedef son kullanıcılara eğilimlidir. Erişim kazandıktan sonra, bu kötü aktörler özgün hesap sahibi adına ayrıcalıklı bilgilere erişim isteğinde bulunabilir veya tüm kuruluşunuzda bir kimlik avı saldırısı gerçekleştirmek için tüm dizini indirebilir. Tüm kullanıcılar için korumayı geliştirmenin yaygın bir yöntemi, çok faktörlü kimlik doğrulaması (MFA) gibi daha güçlü bir hesap doğrulaması biçimi gerektirmaktır.

Güvenlik ve kullanılabilirlik açısından makul bir dengeyi elde etmek için kullanıcılardan her oturum açtıklarında uyarılmamalıdır. Aynı konumdan aynı cihazdan oturum açma gibi normal kullanıcı davranışlarını yansıtan kimlik doğrulama istekleri, düşük bir uzlaşmaya karşı risk altında olabilir. Yalnızca riskli olarak kabul edilen ve hatalı aktörün özelliklerini gösteren oturum açma işlemleri, MFA güçlükleri ile birlikte sorulmalıdır.

Son Kullanıcı koruması, tüm yönetici rolleri dahil olmak üzere bir dizindeki tüm kullanıcıları koruyan risk tabanlı MFA [temel ilkesidir](concept-baseline-protection.md) . Bu ilkenin etkinleştirilmesi, tüm kullanıcıların kimlik doğrulayıcı uygulamasını kullanarak MFA 'ya kaydolmanızı gerektirir. Kullanıcılar MFA kayıt isteminde 14 gün boyunca yok sayabilir, sonrasında MFA 'ya kaydoluncaya kadar oturum açmalarını engellenecektir. MFA için kaydolduktan sonra kullanıcılardan yalnızca riskli oturum açma girişimleri sırasında MFA sorulur. Güvenliği aşılmış Kullanıcı hesapları, parolaları sıfırlanana ve risk algılamaları kapatıldığından engellenir.

> [!NOTE]
> Bu ilke, Konuk hesapları dahil tüm kullanıcılar için geçerlidir ve tüm uygulamalarda oturum açarken değerlendirilir.

## <a name="recovering-compromised-accounts"></a>Güvenliği aşılmış hesapları kurtarma

Microsoft 'un sızdırılan kimlik bilgisi hizmeti, müşterilerimizin korunmasına yardımcı olmak için herkese açık Kullanıcı adı/parola çiftleri buluyor. Kullanıcılarımızdan biriyle eşleşiyorsa, bu hesabın güvenliğini hemen güvence altına almaya yardımcı olur. Sızdırılan kimlik bilgilerine sahip olarak tanımlanan kullanıcıların güvenliği aşılmış. Bu kullanıcıların, parolaları sıfırlanana kadar oturum açması engellenir.

Bir Azure AD Premium Lisansı atanan kullanıcılar, özelliği dizinde etkinleştirilmişse self servis parola sıfırlama (SSPR) üzerinden erişimi geri yükleyebilir. Premium lisansına sahip olmayan kullanıcılar el ile parola sıfırlama işlemi gerçekleştirmek ve bayraklı Kullanıcı riski algılamayı kapatmak için bir yöneticiye başvurmalıdır.

### <a name="steps-to-unblock-a-user"></a>Kullanıcının engellemesini kaldırma adımları

Kullanıcının oturum açma günlüklerini inceleyerek Kullanıcı ilke tarafından engellenmiş olduğunu doğrulayın.

1. Yöneticinin **Azure Portal** oturum açması ve Kullanıcı adına > ve oturum açma işlemleri için **Azure Active Directory** > **kullanıcılara** gitmeniz gerekir.
1. Engellenen bir kullanıcının parola sıfırlama işlemini başlatmak için, bir yöneticinin**risk olarak işaretlenen** **Azure Active Directory** > kullanıcılara gezinilmesi gerekir
1. Hesabı, kullanıcının son oturum açma etkinliği hakkındaki bilgileri görüntülemek üzere engellenen kullanıcıya tıklayın.
1. Bir sonraki oturum açma sonrasında değiştirilmesi gereken geçici bir parola atamak için Parolayı Sıfırla ' ya tıklayın.
1. Kullanıcının risk Puanını sıfırlamak için tüm olayları Kapat ' a tıklayın.

Kullanıcı artık oturum açabilir, parolasını sıfırlayabilir ve uygulamaya erişebilir.

## <a name="deployment-considerations"></a>Dağıtma konuları

**Son Kullanıcı koruma** ilkesi dizininizdeki tüm kullanıcılara uygulandığından, sorunsuz bir dağıtım sağlamak için birkaç dikkat edilmesi gerekir. Bu noktalara, Azure AD 'deki kullanıcıları ve hizmet ilkelerini, MFA 'yı gerektirmeyen veya bu uygulamaların yanı sıra, kuruluşunuz tarafından modern kimlik doğrulamayı desteklemeyen uygulamalar ve istemciler için tanımlama sayılabilir.

### <a name="legacy-protocols"></a>Eski protokoller

Eski kimlik doğrulama protokolleri (IMAP, SMTP, POP3, vb.), posta istemcileri tarafından kimlik doğrulama isteklerini yapmak için kullanılır. Bu protokoller MFA 'yı desteklemez.  Microsoft tarafından görülen hesabın büyük bir çoğunluğu, kötü aktörlerin, MFA 'yı atlamaya çalışan eski protokollere karşı saldırı gerçekleştirmesine neden olmuştur. Bir hesapta oturum açarken MFA 'nın gerekli olduğundan ve kötü aktörlerin MFA 'yı atlayabilmesi için, bu ilke eski protokollerden yönetici hesaplarına yapılan tüm kimlik doğrulama isteklerini engeller.

> [!WARNING]
> Bu ilkeyi etkinleştirmeden önce, kullanıcılarınızın eski kimlik doğrulama protokollerini kullanmadığından emin olun. Bkz. nasıl yapılır: [ Daha fazla bilgi için koşullu erişimle](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) Azure AD 'ye yönelik eski kimlik doğrulamasını engelleyin.

## <a name="enable-the-baseline-policy"></a>Temel ilkeyi etkinleştirme

İlke **temel ilkesi: Son Kullanıcı koruması (Önizleme)** önceden yapılandırılmış olarak gelir ve Azure Portal 'de koşullu erişim dikey penceresine gittiğinizde en üstte görünür.

Bu ilkeyi etkinleştirmek ve kullanıcılarınızı korumak için:

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Koşullu erişimi** **Azure Active Directory** > için gidin.
1. İlke listesinde temel ilke ' yi seçin **: Son Kullanıcı koruması (Önizleme)** .
1. İlkeyi **ilkeyi hemen kullanacak**şekilde ayarlayın.
1. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Koşullu erişim temel koruma ilkeleri](concept-baseline-protection.md)
* [Kimlik altyapınızı güvenli hale getirmenin beş adımı](../../security/fundamentals/steps-secure-identity.md)
* [Azure Active Directory Koşullu erişim nedir?](overview.md)
