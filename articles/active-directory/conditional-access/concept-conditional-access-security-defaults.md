---
title: Azure Active Directory güvenlik Varsayılanları
description: Kuruluşların ortak saldırılara karşı korunması için tasarlanan güvenlik varsayılan ilkeleri
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: b4921f0605f16a601f6e2ee9a15b71b50d253201
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72453049"
---
# <a name="what-are-security-defaults"></a>Güvenlik Varsayılanları nelerdir?

Parola spreyi, yeniden yürütme ve kimlik avı gibi yaygın kimlikle ilgili saldırılar daha fazla ve popüler hale geldiğinde güvenliğin yönetilmesi zor olabilir. Kuruluşunuzun bu yaygın saldırılardan daha güvenli hale getirilmesi için daha basit bir yol oluşturmak, Azure Active Directory (AD) içindeki güvenlik varsayılanlarını hedefliyoruz. Güvenlik Varsayılanları güvenli olmasını kolaylaştırır ve kuruluşunuzun ortak saldırılara karşı korunmasına yardımcı olur. Güvenlik Varsayılanları, bu yaygın saldırılar için önceden yapılandırılmış güvenlik ayarlarını içerir. Microsoft, güvenlik varsayılanlarını herkes için kullanılabilir hale yapıyor. Amaç, tüm kuruluşların hiçbir ek maliyet olmadan etkin bir temel güvenlik düzeyi olmasını sağlamaktır.

![Yeni güvenlik Varsayılanları UX ekranının ekran görüntüsü](./media/concept-conditional-access-security-defaults/security-defaults-azure-ad-portal.png)
 
Kiracınızda aşağıdaki güvenlik yapılandırması açılır. 

## <a name="unified-mfa-registration"></a>Birleşik MFA kaydı

Kiracınızdaki tüm kullanıcılar Azure Multi-Factor Authentication (MFA) için gerekli kayıt olacaktır. Kullanıcıların Microsoft Authenticator uygulamasını kullanarak Azure MFA için kaydolmak üzere 14 gün daha sahip olacaktır. 14 gün geçtikten sonra, MFA kaydı tamamlanana kadar Kullanıcı oturum açamaz.

Bazı kullanıcıların Office dışı olabileceğini ve/veya güvenlik varsayılanlarını etkinleştirmeyi izleyen 14 gün boyunca oturum etmeyebilirsiniz. Her kullanıcıya MFA için kaydolmak üzere bir zaman verildiğinden emin olmak için, her kullanıcı için 14 günlük süre benzersizdir. Kullanıcının 14 günlük süresi, güvenlik Varsayılanları etkinleştirildikten sonra, ilk başarılı etkileşimli oturum açma işleminden sonra başlar.

## <a name="mfa-enforcement"></a>MFA zorlaması

### <a name="protecting-administrators"></a>Yöneticileri koruma

Ayrıcalıklı hesaplara erişimi olan kullanıcıların ortamınıza erişimi artar. Bu hesapların gücüyle ilgili olan güç nedeniyle, onlara özel bir dikkatli davranmanız gerekir. Ayrıcalıklı hesapların korunmasını artırmanın yaygın bir yöntemi, oturum açmak için kullanıldıkları zaman daha güçlü bir hesap doğrulama biçimi gerektirmaktır. Azure Active Directory, çok faktörlü kimlik doğrulaması gerektirerek daha güçlü bir hesap doğrulaması alabilirsiniz.

MFA kaydı tamamlandıktan sonra, her oturum açtıklarında MFA gerçekleştirmek için aşağıdaki dokuz Azure AD yönetici rolü gerekecektir.

- Genel yönetici
- SharePoint yöneticisi
- Exchange Yöneticisi
- Koşullu Erişim Yöneticisi
- Güvenlik yöneticisi
- Yardım Masası Yönetici/Parola Yöneticisi
- Faturalama yöneticisi
- Kullanıcı Yöneticisi
- Kimlik doğrulama Yöneticisi

### <a name="protecting-all-users"></a>Tüm kullanıcıları koruma

Yönetici hesaplarının, çok faktörlü kimlik doğrulamasıyla (MFA) korunması gereken tek hesap olduğunu düşündük. Yöneticiler hassas bilgilere geniş erişime sahiptir ve abonelik genelindeki ayarlarda değişiklik yapabilir. Ancak, kötü aktör hedef son kullanıcılara eğilimlidir. Erişim kazandıktan sonra, bu kötü aktörler özgün hesap sahibi adına ayrıcalıklı bilgilere erişim isteğinde bulunabilir veya tüm kuruluşunuzda bir kimlik avı saldırısı gerçekleştirmek için tüm dizini indirebilir. Tüm kullanıcılar için korumayı geliştirmenin yaygın bir yöntemi, herkes için çok faktörlü kimlik doğrulaması (MFA) gibi daha güçlü bir hesap doğrulama biçimi gerektirmaktır. MFA kaydı tamamlandıktan sonra, kullanıcılardan gerektiğinde MFA sorulur.

### <a name="blocking-legacy-authentication"></a>Eski kimlik doğrulaması engelleniyor

Kullanıcılarınıza bulut uygulamalarınıza kolay erişim sağlamak için Azure Active Directory (Azure AD) eski kimlik doğrulaması dahil olmak üzere çok çeşitli kimlik doğrulama protokollerini destekler. Eski kimlik doğrulaması, tarafından yapılan bir kimlik doğrulama isteğine başvuran bir terimdir:

- Modern kimlik doğrulaması kullanmayan eski Office istemcileri (örneğin, Office 2010 istemcisi)
- IMAP/SMTP/POP3 gibi eski posta protokollerini kullanan istemciler

Günümüzde, tüm güvenliği tehlikeye alınan oturum açma girişimlerinin çoğu eski kimlik doğrulamasından geliyor. Eski kimlik doğrulaması Multi-Factor Authentication 'ı (MFA) desteklemiyor. Dizininizde bir MFA ilkesi etkinleştirilmiş olsa bile, kötü bir aktör eski bir protokolü kullanarak kimlik doğrulaması yapabilir ve MFA 'yı atlayabilir. Kiracınızda güvenlik Varsayılanları etkinleştirildikten sonra, eski bir protokol tarafından herhangi bir şekilde yapılan tüm kimlik doğrulama istekleri engellenir. Güvenlik Varsayılanları Exchange ActiveSync 'ı engellemez.

### <a name="protecting-privileged-actions"></a>Ayrıcalıklı eylemleri koruma

Kuruluşlar, aşağıdakiler dahil olmak üzere Azure Resource Manager API 'SI aracılığıyla yönetilen çeşitli Azure hizmetleri kullanır:

- Azure portalı 
- Azure PowerShell 
- Azure CLI

Hizmetlerinizi yönetmek için Azure Resource Manager kullanmak, yüksek ayrıcalıklı bir işlemdir. Azure Resource Manager, hizmet ayarları ve abonelik faturalandırması gibi kiracı genelinde yapılandırmaların üzerinde değişiklik yapabilir. Tek faktörlü kimlik doğrulaması, kimlik avı ve parola spreyi gibi çeşitli saldırılara açıktır. Bu nedenle, erişime izin vermeden önce çok faktörlü kimlik doğrulaması gerektirerek Azure Resource Manager ve güncelleştirme yapılandırmalarına erişmek isteyen kullanıcıların kimliğini doğrulamak önemlidir.

Kiracınızda güvenlik Varsayılanları etkinleştirildikten sonra, Azure portal, Azure PowerShell veya Azure CLı 'ye erişen tüm kullanıcıların Multi-Factor Authentication 'ı tamamlaması gerekecektir. Bu ilke, yönetici veya Kullanıcı olduklarından bağımsız olarak Azure Resource Manager erişen tüm kullanıcılara uygulanır. Kullanıcı MFA için kayıtlı değilse, kullanıcının devam edebilmesi için Microsoft Authenticator uygulamasını kullanarak kaydetmesi gerekecektir. 14 günlük MFA kayıt süresi sağlanmaz.

## <a name="deployment-considerations"></a>Dağıtma konuları

Kiracınızın güvenlik varsayılanlarını dağıtma ile ilgili bazı ek konular aşağıda verilmiştir.

### <a name="legacy-protocols"></a>Eski protokoller

Eski kimlik doğrulama protokolleri (IMAP, SMTP, POP3, vb.), posta istemcileri tarafından kimlik doğrulama isteklerini yapmak için kullanılır. Bu protokoller MFA 'yı desteklemez. Microsoft tarafından görülen hesabın büyük bir çoğunluğu, kötü aktörlerin, MFA 'yı atlamaya çalışan eski protokollere karşı saldırı gerçekleştirmesine neden olmuştur. Yönetim hesabında günlüğe kaydetme ve kötü aktörlerin MFA 'yı atlayabileceği MFA 'nın gerekli olduğundan emin olmak için, güvenlik Varsayılanları eski protokollerden yönetici hesaplarına yapılan tüm kimlik doğrulama isteklerini engeller.

> [!WARNING]
> Bu ayarı etkinleştirmeden önce, yöneticilerin eski kimlik doğrulama protokollerini kullanmıyor olduğundan emin olun. Daha fazla bilgi için, [eski kimlik doğrulamasından geçme hakkında](concept-conditional-access-block-legacy-authentication.md)makalesine bakın.

### <a name="conditional-access"></a>Koşullu Erişim

Koşullu erişim, güvenlik Varsayılanları tarafından etkinleştirilen davranışı sağlayan ilkeleri yapılandırmak için kullanılabilir. Koşullu erişim kullanıyorsanız ve ortamınızda koşullu erişim ilkeleri etkinleştirilmişse, güvenlik Varsayılanları sizin için kullanılabilir olmayacaktır. Koşullu erişim sağlayan ancak ortamınızda hiç koşullu erişim ilkesi etkinleştirilmemiş bir lisansınız varsa, CA ilkelerini etkinleştirene kadar güvenlik varsayılanlarını kullanmaya hoş geldiniz.

![Güvenlik Varsayılanları veya koşullu erişim her ikisi değil](./media/concept-conditional-access-security-defaults/security-defaults-conditional-access.png)

Eşdeğer ilkeleri yapılandırmak için Koşullu erişimin nasıl kullanılabileceği konusunda adım adım kılavuzlar aşağıda verilmiştir:

- [Yöneticiler için MFA gerektir](howto-conditional-access-policy-admin-mfa.md)
- [Azure yönetimi için MFA gerektir](howto-conditional-access-policy-azure-management.md)
- [Eski kimlik doğrulamasını engelle](howto-conditional-access-policy-block-legacy.md)

## <a name="enabling-security-defaults"></a>Güvenlik varsayılanlarını etkinleştirme

Dizininizde güvenlik varsayılanlarını etkinleştirmek için:

1.  [Azure portal](https://portal.azure.com) Bir Güvenlik Yöneticisi, koşullu erişim Yöneticisi veya genel yönetici olarak oturum açın.
1.  **Azure Active Directory**  @ no__t-2 **özelliklerine** gidin
1. **Güvenlik varsayılanlarını Yönet** ' i seçin
1. **Güvenlik varsayılanlarını etkinleştir** ayarını **Evet**olarak ayarlayın.
1. Kaydet ' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Ortak koşullu erişim ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim nedir?](overview.md)
