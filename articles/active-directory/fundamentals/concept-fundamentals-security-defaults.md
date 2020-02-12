---
title: Azure Active Directory güvenlik Varsayılanları
description: Kuruluşların yaygın saldırılara karşı korunmasına yardımcı olan güvenlik varsayılan ilkeleri
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 023a52f373844e026de0e588e9cd46323abdcf34
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77149839"
---
# <a name="what-are-security-defaults"></a>Güvenlik Varsayılanları nelerdir?

Kimlik ile ilgili yaygın saldırılar daha fazla ve popüler olduğunda güvenliği yönetmek zor olabilir. Bu saldırılar parola spreyi, yeniden oynama ve kimlik avı içerir.

Azure Active Directory (Azure AD) içindeki güvenlik Varsayılanları, güvenli olmasını ve kuruluşunuzu korumanıza yardımcı olmasını kolaylaştırır. Güvenlik Varsayılanları, yaygın saldırılar için önceden yapılandırılmış güvenlik ayarlarını içerir. 

Microsoft, güvenlik varsayılanlarını herkes için kullanılabilir hale yapıyor. Amaç, tüm kuruluşların hiçbir ek maliyet olmadan etkin bir temel güvenlik düzeyi olmasını sağlamaktır. Azure portal güvenlik varsayılanlarını açın.

![Güvenlik varsayılanlarını etkinleştirmek için geçiş ile birlikte Azure portal ekran görüntüsü](./media/concept-fundamentals-security-defaults/security-defaults-azure-ad-portal.png)
 
> [!TIP]
> Kiracınız 22nd, 2019 Ekim 'de veya sonrasında oluşturulduysa, yeni, varsayılan olarak güvenli olan davranışı ve kiracınızda zaten güvenlik Varsayılanları etkinleştirilmiş olması mümkündür. Tüm kullanıcılarınızı korumak için, güvenlik Varsayılanları oluşturulan tüm yeni kiracılar için kullanıma alınıyor.

Güvenlik varsayılanlarını neden kullanılabilir hale getirdiğinizle ilgili daha fazla ayrıntı, [güvenlik varsayılanlarını sunarak](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/introducing-security-defaults/ba-p/1061414)Alex Weinert 'ın blog gönderisine bulunabilir.

## <a name="unified-multi-factor-authentication-registration"></a>Birleşik Multi-Factor Authentication kaydı

Kiracınızdaki tüm kullanıcıların, Azure Multi-Factor Authentication hizmeti biçiminde Multi-Factor Authentication (MFA) için kaydolmalıdır. Kullanıcıların, Microsoft Authenticator uygulamasını kullanarak Multi-Factor Authentication için kaydolmak üzere 14 gün vardır. 14 gün geçtikten sonra, Multi-Factor Authentication kaydı tamamlanana kadar Kullanıcı oturum açamaz.

Bazı kullanıcıların Office dışı olabileceğini veya güvenlik varsayılanlarını etkinleştirdikten hemen sonra 14 gün boyunca oturum açmasını anladık. Her kullanıcının Multi-Factor Authentication için yeterli zamana sahip olduğundan emin olmak için, her kullanıcı için 14 günlük süre benzersizdir. Kullanıcının 14 günlük süresi, güvenlik varsayılanlarını etkinleştirdikten sonra, ilk başarılı etkileşimli oturum açma işleminden sonra başlar.

## <a name="multi-factor-authentication-enforcement"></a>Multi-Factor Authentication zorlama

### <a name="protecting-administrators"></a>Yöneticileri koruma

Ayrıcalıklı hesaplara erişimi olan kullanıcıların ortamınıza erişimi artar. Bu hesapların gücüyle ilgili olan güç nedeniyle, onlara özel bir dikkatli davranmanız gerekir. Ayrıcalıklı hesapların korunmasını artırmanın yaygın bir yöntemi, oturum açma için daha güçlü bir hesap doğrulama biçimi gerektirmaktır. Azure AD 'de Multi-Factor Authentication isteyerek daha güçlü bir hesap doğrulaması edinebilirsiniz.

Multi-Factor Authentication kaydı tamamlandıktan sonra, her oturum açtıklarında ek kimlik doğrulaması gerçekleştirmek için aşağıdaki dokuz Azure AD yönetici rolü gerekecektir:

- Genel yönetici
- SharePoint yöneticisi
- Exchange Yöneticisi
- Koşullu Erişim Yöneticisi
- Güvenlik yöneticisi
- Yardım Masası yönetici veya parola Yöneticisi
- Faturalama yöneticisi
- Kullanıcı Yöneticisi
- Kimlik doğrulama Yöneticisi

### <a name="protecting-all-users"></a>Tüm kullanıcıları koruma

Yönetici hesaplarının, çok fazla kimlik doğrulama katmanı gerektiren tek hesaplar olduğunu düşündük. Yöneticiler hassas bilgilere geniş erişime sahiptir ve abonelik genelindeki ayarlarda değişiklik yapabilir. Ancak saldırganlar, son kullanıcıları hedefleyecek şekilde eğilimlidir. 

Bu saldırganlar erişim kazandıktan sonra, özgün hesap sahibi adına ayrıcalıklı bilgilere erişim talep edebilir. Hatta tüm kuruluşunuzda kimlik avı saldırısı gerçekleştirmek için tüm dizini indirebilir. 

Tüm kullanıcılar için korumayı geliştirmenin yaygın bir yöntemi, herkes için Multi-Factor Authentication gibi daha güçlü bir hesap doğrulama biçimi gerektirmaktır. Kullanıcılar Multi-Factor Authentication kaydı tamamladıktan sonra gerektiğinde ek kimlik doğrulaması istenir.

### <a name="blocking-legacy-authentication"></a>Eski kimlik doğrulaması engelleniyor

Kullanıcılarınıza bulut uygulamalarınıza kolay erişim sağlamak için Azure AD, eski kimlik doğrulaması dahil olmak üzere çeşitli kimlik doğrulama protokollerini destekler. *Eski kimlik doğrulaması* , tarafından yapılan bir kimlik doğrulama isteğine başvuran bir terimdir:

- Modern kimlik doğrulaması kullanmayan eski Office istemcileri (örneğin, bir Office 2010 istemcisi).
- IMAP, SMTP veya POP3 gibi eski posta protokollerini kullanan tüm istemciler.

Günümüzde, oturum açma girişimlerinin çoğu eski kimlik doğrulamasından geliyor. Eski kimlik doğrulama Multi-Factor Authentication desteklemez. Dizininizde etkinleştirilmiş bir Multi-Factor Authentication ilkeniz olsa da, bir saldırgan eski bir protokolü kullanarak kimlik doğrulaması yapabilir ve Multi-Factor Authentication atlayabilirsiniz. 

Kiracınızda güvenlik Varsayılanları etkinleştirildikten sonra, eski bir protokol tarafından yapılan tüm kimlik doğrulama istekleri engellenir. Güvenlik Varsayılanları Exchange Active Sync temel kimlik doğrulamasını engeller.

> [!WARNING]
> Güvenlik varsayılanlarını etkinleştirmeden önce, yöneticileriniz tarafından daha eski kimlik doğrulama protokolleri kullanmıyor olduğundan emin olun. Daha fazla bilgi için bkz. [eski kimlik doğrulamasından nasıl uzaklaşın](concept-fundamentals-block-legacy-authentication.md).

### <a name="protecting-privileged-actions"></a>Ayrıcalıklı eylemleri koruma

Kuruluşlar, aşağıdakiler dahil olmak üzere Azure Resource Manager API 'SI aracılığıyla yönetilen çeşitli Azure hizmetleri kullanır:

- Azure portalı 
- Azure PowerShell 
- Azure CLI

Hizmetlerinizi yönetmek için Azure Resource Manager kullanmak, yüksek ayrıcalıklı bir işlemdir. Azure Resource Manager, hizmet ayarları ve abonelik faturalandırması gibi kiracı genelinde yapılandırmaların üzerinde değişiklik yapabilir. Tek faktörlü kimlik doğrulaması, kimlik avı ve parola spreyi gibi çeşitli saldırılara açıktır. 

Azure Resource Manager erişmek ve yapılandırmaya güncelleştirmek isteyen kullanıcıların kimliğini doğrulamak önemlidir. Erişim izni vermeden önce ek kimlik doğrulaması gerektirerek kimliklerini doğrularsınız.

Kiracınızda güvenlik varsayılanlarını etkinleştirdikten sonra, Azure portal, Azure PowerShell veya Azure CLı 'ye erişen tüm kullanıcıların ek kimlik doğrulamasını tamamlaması gerekir. Bu ilke, yönetici veya Kullanıcı olup olmadıkları gibi Azure Resource Manager erişen tüm kullanıcılar için geçerlidir. 

Kullanıcı Multi-Factor Authentication için kayıtlı değilse, kullanıcının devam edebilmesi için Microsoft Authenticator uygulamasını kullanarak kaydetmesi gerekecektir. 14 günlük Multi-Factor Authentication kayıt süresi sağlanmaz.

2017 öncesi Exchange Online kiracılarında modern kimlik doğrulaması varsayılan olarak devre dışıdır. Bu kiracılar aracılığıyla kimlik doğrulaması yaparken bir oturum açma döngüsü olasılığını önlemek için [modern kimlik doğrulamayı etkinleştirmeniz](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online)gerekir.

> [!NOTE]
> Azure AD Connect eşitleme hesabı güvenlik varsayılanlarını dışarıda bırakılır ve Multi-Factor Authentication 'ı kaydetmesi veya gerçekleştirmesi istenmez. Kuruluşlar bu hesabı başka amaçlar için kullanmamalıdır.

## <a name="deployment-considerations"></a>Dağıtma konuları

Aşağıdaki ek konular kiracınızın güvenlik varsayılanlarını dağıtma ile ilgilidir.

### <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

Güvenlik Varsayılanları **yalnızca bildirimleri kullanarak Microsoft Authenticator uygulamasını kullanarak**Azure Multi-Factor Authentication 'nin kaydedilmesine ve kullanımına izin verir. Koşullu erişim, yöneticinin etkinleştirmek üzere seçtiği herhangi bir kimlik doğrulama yönteminin kullanılmasına izin verir.

|   | Güvenlik varsayılanları | Koşullu Erişim |
| --- | --- | --- |
| Mobil uygulama aracılığıyla bildirim | X | X |
| Mobil uygulama veya donanım belirtecinden doğrulama kodu |   | X |
| Telefona kısa mesaj |   | X |
| Telefon çağrısı |   | X |
| Uygulama parolaları |   | X * * |

\* * Uygulama parolaları yalnızca yöneticiler tarafından etkinleştirildiyse eski kimlik doğrulama senaryolarıyla Kullanıcı başına MFA 'da kullanılabilir.

### <a name="conditional-access"></a>Koşullu Erişim

Güvenlik varsayılanlarına benzer ilkeleri yapılandırmak için koşullu erişimi kullanabilirsiniz, ancak güvenlik Varsayılanları ' nda kullanılamayan Kullanıcı dışlamaları dahil daha fazla ayrıntı düzeyi vardır. Koşullu erişim kullanıyorsanız ve ortamınızda koşullu erişim ilkeleri etkinleştirilmişse, güvenlik Varsayılanları sizin için kullanılamaz. Koşullu erişim sağlayan ancak ortamınızda hiç koşullu erişim ilkesi etkinleştirilmemiş bir lisansınız varsa, koşullu erişim ilkelerini etkinleştirene kadar güvenlik varsayılanlarını kullanmaya hoş geldiniz. Azure AD Lisansı hakkında daha fazla bilgi için [Azure AD fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/active-directory/)bulabilirsiniz.

![Uyarı iletisi yalnızca güvenlik varsayılanlarına veya koşullu erişime sahip olabilirsiniz](./media/concept-fundamentals-security-defaults/security-defaults-conditional-access.png)

Eşdeğer ilkeleri yapılandırmak için koşullu erişimi nasıl kullanabileceğinizi gösteren adım adım kılavuzlar aşağıda verilmiştir:

- [Yöneticiler için MFA gerektir](../conditional-access/howto-conditional-access-policy-admin-mfa.md)
- [Azure yönetimi için MFA gerektir](../conditional-access/howto-conditional-access-policy-azure-management.md)
- [Eski kimlik doğrulamasını engelle](../conditional-access/howto-conditional-access-policy-block-legacy.md)
- [Tüm kullanıcılar için MFA gerektir](../conditional-access/howto-conditional-access-policy-all-users-mfa.md)
- [Azure MFA kaydı gerektir](../identity-protection/howto-identity-protection-configure-mfa-policy.md) -Azure AD kimlik koruması gerekir

## <a name="enabling-security-defaults"></a>Güvenlik varsayılanlarını etkinleştirme

Dizininizde güvenlik varsayılanlarını etkinleştirmek için:

1.  [Azure portal](https://portal.azure.com) bir güvenlik yöneticisi, koşullu erişim Yöneticisi veya genel yönetici olarak oturum açın.
1.  **Azure Active Directory** > **özelliklerine**gidin.
1. **Güvenlik varsayılanlarını Yönet**' i seçin.
1. **Güvenlik varsayılanlarını etkinleştir** ayarını **Evet**olarak ayarlayın.
1. **Kaydet**’i seçin.

## <a name="disabling-security-defaults"></a>Güvenlik varsayılanlarını devre dışı bırakma

Güvenlik varsayılanlarını değiştirecek koşullu erişim ilkelerini uygulamayı tercih eden kuruluşlar, güvenlik varsayılanlarını devre dışı bırakmalıdır. 

![Uyarı iletisi koşullu erişimi etkinleştirmek için güvenlik varsayılanlarını devre dışı bırak](./media/concept-fundamentals-security-defaults/security-defaults-disable-before-conditional-access.png)

Dizininizde güvenlik varsayılanlarını devre dışı bırakmak için:

1.  [Azure portal](https://portal.azure.com) bir güvenlik yöneticisi, koşullu erişim Yöneticisi veya genel yönetici olarak oturum açın.
1.  **Azure Active Directory** > **özelliklerine**gidin.
1. **Güvenlik varsayılanlarını Yönet**' i seçin.
1. **Güvenlik varsayılanlarını etkinleştir** ' i ayarla **Hayır**olarak ayarlayın.
1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Ortak koşullu erişim ilkeleri](../conditional-access/concept-conditional-access-policy-common.md)
