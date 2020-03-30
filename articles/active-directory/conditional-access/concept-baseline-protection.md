---
title: Koşullu Erişim temel ilkeleri - Azure Etkin Dizini
description: Kuruluşları sık karşılaşılan saldırılardan korumak için Temel Koşullu Erişim ilkeleri
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 55de5a5c604273225a85e49ca682980f83a951d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75767577"
---
# <a name="what-are-baseline-policies"></a>Temel ilkeler nelerdir?

Temel ilkeler, kuruluşların birçok yaygın saldırıya karşı korunmasına yardımcı olan önceden tanımlanmış ilkeler kümesidir. Bu yaygın saldırılar parola püskürtme, yeniden oynatma ve kimlik avı içerebilir. Temel ilkeler, Azure AD'nin tüm sürümlerinde kullanılabilir. Kimlik tabanlı saldırılar son birkaç yıldır artmakta olduğundan Microsoft bu temel koruma ilkelerini herkes tarafından kullanılabilir hale getirsin. Bu dört ilkenin amacı, tüm kuruluşların hiçbir ek ücret ödemeden etkin leştirilmiş bir temel güvenlik düzeyine sahip olmasını sağlamaktır.

Özelleştirilmiş Koşullu Erişim ilkelerini yönetmek için Azure AD Premium lisansı gerektirir.

> [!IMPORTANT]
> Temel ilkeler küçümsenmektedir. Daha fazla bilgi için [Azure Active Directory'deki yeniliklere bakın?](../fundamentals/whats-new.md#replacement-of-baseline-policies-with-security-defaults)

## <a name="baseline-policies"></a>Ana hat ilkeleri

![Azure portalında Koşullu Erişim temel ilkeleri](./media/concept-baseline-protection/conditional-access-baseline-policies.png)

Dört temel ilke vardır:

* Yöneticiler için MFA gerektirir (önizleme)
* Son kullanıcı koruması (önizleme)
* Eski kimlik doğrulamasını engelleme (önizleme)
* Hizmet yönetimi için MFA gerektirir (önizleme)

Bu ilkelerin dördü de POP, IMAP ve eski Office masaüstü istemcileri gibi eski kimlik doğrulama akışlarını etkiler.

### <a name="exclusions"></a>Dışlamalar

Temel ilkeler ilk genel önizlemelerine girdiğinde, kullanıcıları ilkelerden dışlama seçeneği vardı. Bu özellik önizleme yoluyla gelişti ve Temmuz 2019'da kaldırıldı. Daha önce dışlamalar oluşturmuş olan kuruluşlar, yeni kullanıcıları tutmaya devam edebildi ve ilkelere dışlama ekleyemedi.

### <a name="require-mfa-for-admins-preview"></a>Yöneticiler için MFA gerektirir (önizleme)

Yönetici hesaplarının sahip olduğu güç ve erişim nedeniyle, bunları özel bir özenle tedavi etmelisiniz. Ayrıcalıklı hesapların korunmasını iyileştirmek için yaygın yöntemlerden biri, oturum açmaiçin kullanıldıklarında daha güçlü bir hesap doğrulama biçimi gerektirmektir. Azure Etkin Dizin'de, yöneticilerin Azure Çok Faktörlü Kimlik Doğrulama'ya kaydolmasını ve bunları kullanmasını isteyerek daha güçlü bir hesap doğrulaması elde edebilirsiniz.

Yöneticiler için MFA'yı zorunlu kılması (önizleme), en ayrıcalıklı Azure REKLAM rolleri olarak kabul edilen aşağıdaki dizin rolleri için çok faktörlü kimlik doğrulaması (MFA) gerektiren bir temel ilkedir:

* Genel yönetici
* SharePoint yöneticisi
* Exchange yöneticisi
* Koşullu Erişim yöneticisi
* Güvenlik yöneticisi
* Yardım Masası yöneticisi / Parola yöneticisi
* Faturalama yöneticisi
* Kullanıcı yöneticisi

Kuruluşunuzun bu hesapları komut dosyası veya kodda kullanılıyorsa, [bunları yönetilen kimliklerle](../managed-identities-azure-resources/overview.md)değiştirmeyi düşünün.

### <a name="end-user-protection-preview"></a>Son kullanıcı koruması (önizleme)

Saldırılarda hedef lenenler sadece yüksek ayrıcalıklı yöneticiler değildir. Kötü aktörler normal kullanıcıları hedefleme eğilimindedir. Erişim sağladıktan sonra, bu kötü aktörler orijinal hesap sahibi adına ayrıcalıklı bilgilere erişim talebinde bulunabilir veya dizinin tamamını indirebilir ve tüm kuruluşunuza kimlik avı saldırısı gerçekleştirebilir. Tüm kullanıcılar için korumayı geliştirmek için yaygın yöntemlerden biri, riskli bir oturum açma algılandığında daha güçlü bir hesap doğrulama biçimine ihtiyaç duymasıdır.

**Son kullanıcı koruması (önizleme),** bir dizindeki tüm kullanıcıları koruyan temel bir ilkedir. Bu ilkeyi etkinleştirmek için tüm kullanıcıların 14 gün içinde Azure Çok Faktörlü Kimlik Doğrulama'ya kaydolması gerekmektedir. Kayıt olduktan sonra, kullanıcılar ait olan riskli oturum açma girişimleri sırasında MFA'ya istenir. Parola sıfırlanana ve işten çıkarılma riskine kadar tehlikeye atılmış kullanıcı hesapları engellenir. 

> [!NOTE]
> Daha önce risk için işaretlenmiş tüm kullanıcılar, parola sıfırlanana ve ilke etkinleştirme üzerine işten çıkarılma riskine kadar engellenir.

### <a name="block-legacy-authentication-preview"></a>Eski kimlik doğrulamasını engelleme (önizleme)

Eski kimlik doğrulama protokolleri (ör. IMAP, SMTP, POP3) normalde eski posta istemcileri tarafından kimlik doğrulaması için kullanılan protokollerdir. Eski protokoller çok faktörlü kimlik doğrulamasını desteklemez. Dizininiz için çok faktörlü kimlik doğrulaması gerektiren bir ilkeniz olsa bile, kötü bir aktör bu eski protokollerden birini kullanarak kimlik doğrulaması yapabilir ve çok faktörlü kimlik doğrulamayı atlayabilir.

Hesabınızı eski protokoller tarafından yapılan kötü amaçlı kimlik doğrulama isteklerine karşı korumanın en iyi yolu, bunları engellemektir.

**Eski kimlik doğrulamasını (önizlemeyi) engelle** temel ilkesi, eski protokoller kullanılarak yapılan kimlik doğrulama isteklerini engeller. Tüm kullanıcılar için başarılı bir şekilde oturum açabilmek için modern kimlik doğrulaması kullanılmalıdır. Diğer temel ilkelerle birlikte kullanıldığında, eski protokollerden gelen istekler engellenir. Buna ek olarak, tüm kullanıcılar gerektiğinde MFA için gerekli olacaktır. Bu ilke Exchange ActiveSync engellemez.

### <a name="require-mfa-for-service-management-preview"></a>Hizmet yönetimi için MFA gerektirir (önizleme)

Kuruluşlar çeşitli Azure hizmetleri kullanır ve bunları Azure Kaynak Yöneticisi tabanlı araçlardan yönetir:

* Azure portalında
* Azure PowerShell
* Azure CLI

Kaynak yönetimini gerçekleştirmek için bu araçlardan herhangi birini kullanmak son derece ayrıcalıklı bir eylemdir. Bu araçlar, hizmet ayarları ve abonelik faturalandırması gibi abonelik genelindeki yapılandırmaları değiştirebilir.

Ayrıcalıklı eylemleri korumak **için, bu Hizmet yönetimi (önizleme) için MFA Gerektirir** ilkesi, Azure portalına, Azure PowerShell'e veya Azure CLI'ye erişen tüm kullanıcı için çok faktörlü kimlik doğrulama gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Güvenlik varsayılanlarını etkinleştirme](../fundamentals/concept-fundamentals-security-defaults.md)
* [Sık kullanılan Koşullu Erişim İlkeleri](concept-conditional-access-policy-common.md)
* [Kimlik altyapınızı güvence altına almak için beş adım](../../security/fundamentals/steps-secure-identity.md)
