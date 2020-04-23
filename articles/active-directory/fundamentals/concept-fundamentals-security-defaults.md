---
title: Azure Etkin Dizin güvenlik varsayılanları
description: Kuruluşların sık karşılaşılan saldırılara karşı korunmasına yardımcı olan güvenlik varsayılan ilkeleri
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
ms.openlocfilehash: f307553a97973d03b0699248373e53e4845aa39a
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869910"
---
# <a name="what-are-security-defaults"></a>Güvenlik varsayılanları nelerdir?

Ortak kimlikle ilgili saldırılar giderek daha popüler hale geldiğinde güvenliği yönetmek zor olabilir. Bu saldırılar parola püskürtme, yeniden oynatma ve kimlik avı içerir.

Azure Etkin Dizini'ndeki (Azure AD) güvenlik varsayılanları, güvenli olmayı kolaylaştırır ve kuruluşunuzun korunmasına yardımcı olur. Güvenlik varsayılanları, sık yapılan saldırılar için önceden yapılandırılmış güvenlik ayarları içerir. 

Microsoft, güvenlik varsayılanlarını herkes tarafından kullanılabilir hale getiriyor. Amaç, tüm kuruluşların hiçbir ek ücret ödemeden etkin leştirilmiş temel bir güvenlik düzeyine sahip olmasını sağlamaktır. Azure portalında güvenlik varsayılanlarını açarsınız.

![Güvenlik varsayılanlarını etkinleştirmek için geçişle Azure portalının ekran görüntüsü](./media/concept-fundamentals-security-defaults/security-defaults-azure-ad-portal.png)
 
> [!TIP]
> Kiracınız 22 Ekim 2019 tarihinde veya sonrasında oluşturulduysa, yeni güvenli ve varsayılan davranışı yaşıyor olabilirsiniz ve kiracınızda zaten güvenlik varsayılanları etkinleştirildi. Tüm kullanıcılarımızı korumak amacıyla, güvenlik varsayılanları oluşturulan tüm yeni kiracılara dağıtılıyor.

Güvenlik varsayılanlarının neden kullanıma sunulduğuna ilişkin daha fazla ayrıntı Alex Weinert'in blog gönderisinde bulunabilir, [güvenlik varsayılanlarını tanıtın.](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/introducing-security-defaults/ba-p/1061414)

## <a name="unified-multi-factor-authentication-registration"></a>Birleşik Çok Faktörlü Kimlik Doğrulama kaydı

Kiracınızdaki tüm kullanıcıların Azure Çok Faktörlü Kimlik Doğrulama hizmeti biçiminde çok faktörlü kimlik doğrulama (MFA) için kaydolması gerekir. Kullanıcıların Microsoft Authenticator uygulamasını kullanarak Çoklu Faktör Kimlik Doğrulama'ya kaydolmak için 14 günleri vardır. 14 gün geçtikten sonra, kullanıcı Çok Faktörlü Kimlik Doğrulama kaydı tamamlanana kadar oturum açamaz.

Bazı kullanıcıların güvenlik varsayılanlarını etkinleştirdikten hemen sonra 14 gün boyunca ofis dışında olabileceğini veya oturum açamayacağını anlıyoruz. Her kullanıcının Çok Faktörlü Kimlik Doğrulama'ya kaydolmak için yeterli zamanı olduğundan emin olmak için, 14 günlük süre her kullanıcı için benzersizdir. Bir kullanıcının 14 günlük dönemi, güvenlik varsayılanlarını etkinleştirdikten sonra ilk başarılı etkileşimli oturum açmalarından sonra başlar.

## <a name="multi-factor-authentication-enforcement"></a>Çok Faktörlü Kimlik Doğrulama uygulaması

### <a name="protecting-administrators"></a>Yöneticilerin korunması

Ayrıcalıklı hesaplara erişimi olan kullanıcılar ortamınıza erişimi artırmıştır. Bu hesapların sahip olduğu güç nedeniyle, onlara özel bir özenle davranmalısınız. Ayrıcalıklı hesapların korunmasını iyileştirmek için yaygın yöntemlerden biri, oturum açma için daha güçlü bir hesap doğrulama biçimi gerektirmektir. Azure AD'de, Çok Faktörlü Kimlik Doğrulama'yı gerektirerek daha güçlü bir hesap doğrulaması elde edebilirsiniz.

Çok Faktörlü Kimlik Doğrulama kaydı tamamlandıktan sonra, her oturum açtıklarında ek kimlik doğrulaması gerçekleştirmek için aşağıdaki dokuz Azure AD yöneticisi rolü gerekir:

- Genel yönetici
- SharePoint yöneticisi
- Exchange yöneticisi
- Koşullu Erişim yöneticisi
- Güvenlik yöneticisi
- Yardım Masası yöneticisi veya parola yöneticisi
- Faturalama yöneticisi
- Kullanıcı yöneticisi
- Kimlik doğrulama yöneticisi

### <a name="protecting-all-users"></a>Tüm kullanıcıları koruma

Yönetici hesaplarının, fazladan kimlik doğrulama katmanlarıgerektiren tek hesap olduğunu düşünme eğilimindeyiz. Yöneticiler hassas bilgilere geniş erişime sahiptir ve abonelik genelindeki ayarlarda değişiklik yapabilir. Ancak saldırganlar son kullanıcıları hedefleme eğilimindedir. 

Bu saldırganlar erişim sağladıktan sonra, orijinal hesap sahibi adına ayrıcalıklı bilgilere erişim talebinde bulunabilirler. Hatta tüm kuruluşunuz üzerinde kimlik avı saldırısı gerçekleştirmek için tüm dizini indirebilirler. 

Tüm kullanıcılar için korumayı geliştirmek için yaygın yöntemlerden biri, herkes için Çok Faktörlü Kimlik Doğrulama gibi daha güçlü bir hesap doğrulama biçimine ihtiyaç duymanızdır. Kullanıcılar Çok Faktörlü Kimlik Doğrulama kaydını tamamladıktan sonra, gerektiğinde ek kimlik doğrulamaları istenir.

### <a name="blocking-legacy-authentication"></a>Eski kimlik doğrulamayı engelleme

Azure AD, kullanıcılarınıza bulut uygulamalarınıza kolay erişim sağlamak için eski kimlik doğrulama dahil olmak üzere çeşitli kimlik doğrulama protokollerini destekler. *Eski kimlik doğrulaması,* şu lar tarafından yapılan bir kimlik doğrulama isteğini ifade eden bir terimdir:

- Modern kimlik doğrulaması kullanmayan istemciler (örneğin, bir Office 2010 istemcisi).
- IMAP, SMTP veya POP3 gibi eski posta protokollerini kullanan tüm istemciler.

Bugün, ödün veren oturum açma girişimlerinin çoğu eski kimlik doğrulamasından gelir. Eski kimlik doğrulaması Çok Faktörlü Kimlik Doğrulaması'nı desteklemez. Dizininizde etkin leştirilmiş bir Çok Faktörlü Kimlik Doğrulama ilkesi olsa bile, saldırgan eski bir protokol kullanarak kimlik doğrulaması yapabilir ve Çok Faktörlü Kimlik Doğrulama'yı atlayabilir. 

Kiracınızda güvenlik varsayılanları etkinleştirildikten sonra, eski bir protokol tarafından yapılan tüm kimlik doğrulama istekleri engellenir. Güvenlik varsayılanları Exchange Active Sync temel kimlik doğrulamasını engeller.

> [!WARNING]
> Güvenlik varsayılanlarını etkinleştirmeden önce, yöneticilerinizin eski kimlik doğrulama protokollerini kullanmadığından emin olun. Daha fazla bilgi için eski [kimlik doğrulamasından nasıl uzaklaşılabildiğini](concept-fundamentals-block-legacy-authentication.md)öğrenin.

### <a name="protecting-privileged-actions"></a>Ayrıcalıklı eylemleri koruma

Kuruluşlar, Aşağıdakiler de dahil olmak üzere Azure Kaynak Yöneticisi API'si aracılığıyla yönetilen çeşitli Azure hizmetlerini kullanır:

- Azure portal 
- Azure PowerShell 
- Azure CLI

Hizmetlerinizi yönetmek için Azure Kaynak Yöneticisi'ni kullanmak son derece ayrıcalıklı bir eylemdir. Azure Kaynak Yöneticisi, hizmet ayarları ve abonelik faturalandırması gibi kiracı çapındaki yapılandırmaları değiştirebilir. Tek faktörlü kimlik doğrulama, kimlik avı ve parola spreyi gibi çeşitli saldırılara karşı savunmasızdır. 

Azure Kaynak Yöneticisi'ne erişmek ve yapılandırmaları güncelleştirmek isteyen kullanıcıların kimliğini doğrulamak önemlidir. Erişime izin vermeden önce ek kimlik doğrulaması gerektirerek kimliklerini doğrularsınız.

Kiracınızdaki güvenlik varsayılanlarını etkinleştirdikten sonra, Azure portalına, Azure PowerShell'e veya Azure CLI'sine erişen tüm kullanıcının ek kimlik doğrulamasını tamamlaması gerekir. Bu ilke, ister yönetici ister kullanıcı olsun, Azure Kaynak Yöneticisi'ne erişen tüm kullanıcılar için geçerlidir. 

> [!NOTE]
> 2017 Öncesi Exchange Online kiracılar, varsayılan olarak modern kimlik doğrulamayı devre dışı bıraktı. Bu kiracılar aracılığıyla kimlik doğrulaması yaparken bir giriş döngüsü olasılığını önlemek için, [modern kimlik doğrulama etkinleştirmelisiniz.](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online)

> [!NOTE]
> Azure AD Connect eşitleme hesabı güvenlik varsayılanlarının dışında dır ve çok faktörlü kimlik doğrulamasına kaydolması veya gerçekleştirmesi istenmez. Kuruluşlar bu hesabı başka amaçlarla kullanmamalıdır.

## <a name="deployment-considerations"></a>Dağıtma konuları

Aşağıdaki ek hususlar kiracınız için güvenlik varsayılanlarının dağıtımıyla ilgilidir.

### <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

Güvenlik varsayılanları, **yalnızca bildirimleri kullanarak yalnızca Microsoft Authenticator uygulamasını kullanarak**Azure Çok Faktörlü Kimlik Doğrulama'nın kaydedilmesine ve kullanılmasına izin verir. Koşullu Erişim, yöneticinin etkinleştirmeyi seçtiği herhangi bir kimlik doğrulama yönteminin kullanılmasına izin verir.

|   | Güvenlik varsayılanları | Koşullu Erişim |
| --- | --- | --- |
| Mobil uygulama üzerinden bildirim | X | X |
| Mobil uygulama veya donanım belirteci doğrulama kodu |   | X |
| Telefona kısa mesaj |   | X |
| Telefona çağrı |   | X |
| Uygulama parolaları |   | X** |

** Uygulama parolaları yalnızca eski kimlik doğrulama senaryoları yalnızca yöneticiler tarafından etkinleştirildiğinde kullanıcı başına MFA'da kullanılabilir.

### <a name="conditional-access"></a>Koşullu Erişim

Koşullu Erişim'i, güvenlik varsayılanlarına benzer, ancak güvenlik varsayılanlarında bulunmayan kullanıcı dışlamaları da dahil olmak üzere daha ayrıntılı lıkla yapılandırılabilir. Koşullu Erişim kullanıyorsanız ve ortamınızda Koşullu Erişim ilkeleri etkinleştirildiyse, güvenlik varsayılanları sizin kullanımınıza sunulmaz. Koşullu Erişim sağlayan ancak ortamınızda koşullu erişim ilkeleri etkin olmayan bir lisansınız varsa, Koşullu Erişim ilkelerini etkinleştirene kadar güvenlik varsayılanlarını kullanabilirsiniz. Azure AD lisanslama hakkında daha fazla bilgiyi [Azure AD fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/active-directory/)bulabilirsiniz.

![Güvenlik varsayılanlarına sahip olabileceğiniz uyarı iletisi veya Koşullu Erişim her ikisine de sahip olamaz](./media/concept-fundamentals-security-defaults/security-defaults-conditional-access.png)

Eşdeğer ilkeleri yapılandırmak için Koşullu Erişim'i nasıl kullanabileceğinize ilişkin adım adım kılavuzlar aşağıda verebilirsiniz:

- [Yöneticiler için MFA gerektirme](../conditional-access/howto-conditional-access-policy-admin-mfa.md)
- [Azure yönetimi için MFA gerektirme](../conditional-access/howto-conditional-access-policy-azure-management.md)
- [Eski kimlik doğrulamasını engelleme](../conditional-access/howto-conditional-access-policy-block-legacy.md)
- [Tüm kullanıcılar için MFA gerektirme](../conditional-access/howto-conditional-access-policy-all-users-mfa.md)
- [Azure MFA kaydı gerektirir](../identity-protection/howto-identity-protection-configure-mfa-policy.md) - Azure AD Kimlik Koruması Gerektirir

## <a name="enabling-security-defaults"></a>Güvenlik varsayılanlarını etkinleştirme

Dizininizde güvenlik varsayılanlarını etkinleştirmek için:

1.  [Azure portalında](https://portal.azure.com) güvenlik yöneticisi, Koşullu Erişim yöneticisi veya genel yönetici olarak oturum açın.
1.  **Azure Etkin Dizin** > **Özelliklerine**göz atın.
1. **Güvenlik varsayılanlarını yönet'i**seçin.
1. Güvenlik **varsayılanlarını Etkinleştir'i** **Evet'e**ayarlama.
1. **Kaydet**’i seçin.

## <a name="disabling-security-defaults"></a>Güvenlik varsayılanlarını devre dışı bırakma

Güvenlik varsayılanlarını değiştiren Koşullu Erişim ilkelerini uygulamayı seçen kuruluşlar, güvenlik varsayılanlarını devre dışı bırakmayı zorunlu durmalıdır. 

![Koşullu Erişimi etkinleştirmek için uyarı iletisi güvenlik varsayılanlarını devre dışı](./media/concept-fundamentals-security-defaults/security-defaults-disable-before-conditional-access.png)

Dizininizdeki güvenlik varsayılanlarını devre dışı kınık için:

1.  [Azure portalında](https://portal.azure.com) güvenlik yöneticisi, Koşullu Erişim yöneticisi veya genel yönetici olarak oturum açın.
1.  **Azure Etkin Dizin** > **Özelliklerine**göz atın.
1. **Güvenlik varsayılanlarını yönet'i**seçin.
1. Güvenlik **varsayılanlarını** etkinleştirme'yi **No'ya**ayarlama
1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Sık kullanılan Koşullu Erişim İlkeleri](../conditional-access/concept-conditional-access-policy-common.md)
