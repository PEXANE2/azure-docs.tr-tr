---
title: Akıllı kilitleme kullanarak saldırıları önleme - Azure Etkin Dizin
description: Azure Active Directory akıllı kilitleme, parolaları tahmin etmeye çalışan kaba kuvvet saldırılarına karşı kuruluşunuzun korunmasına yardımcı olur
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 61ae942ed189dc4245a9a0b282daf4cad5323536
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652576"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory akıllı kilitleme

Akıllı lokavt, kullanıcılarınızın parolalarını tahmin etmeye veya içeri girmek için kaba kuvvet yöntemlerini kullanmaya çalışan kötü aktörleri kilitlemeye yardımcı olur. Geçerli kullanıcılardan gelen oturum açma ları tanıyabilir ve saldırganların ve diğer bilinmeyen kaynaklardan farklı davranabilir. Akıllı kilitleme saldırganları kilitlerken, kullanıcılarınızın hesaplarına erişmeye ve üretken olmaya devam etmelerine izin verir.

Varsayılan olarak, akıllı kilitleme, 10 başarısız denemeden sonra hesabı oturum açma denemelerinden bir dakika kilitler. Hesap, sonraki her başarısız oturum açma girişiminden sonra, ilk başta bir dakika ve sonraki denemelerde daha uzun süre kilitlenir.

Akıllı kilitleme, aynı parola için kilitleme sayacının artışını önlemek için son üç kötü parola çözümünü izler. Birisi aynı kötü parolayı birden çok kez girerse, bu davranış hesabın kilitlenmesine neden olmaz.

 > [!NOTE]
 > Kimlik doğrulama bulutta değil şirket içinde gerçekleştiğinden, geçiş kimlik doğrulaması etkinleştirilmiş müşteriler için karma izleme işlevi kullanılamaz.

AD FS 2016 ve AF FS 2019 kullanılarak federe [dağıtımlar, AD FS Extranet Lokavt ve Extranet Akıllı Lokavt](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection)kullanarak benzer avantajlar sağlayabilir.

Doğru güvenlik ve kullanılabilirlik karışımını sunan bu varsayılan ayarlara sahip tüm Azure AD müşterileri için akıllı kilitleme her zaman açıktır. Kuruluşunuza özgü değerlerle akıllı kilitleme ayarlarının özelleştirilmesi, kullanıcılarınız için ücretli Azure REKLAM lisansları gerektirir.

Akıllı kilitleme kullanmak, gerçek bir kullanıcının hiçbir zaman kilitlenmeyeceğini garanti etmez. Akıllı kilitleme bir kullanıcı hesabını kilitlediğinde, gerçek kullanıcıyı kilitlememek için elimizden geleni yapıyoruz. Kilitleme hizmeti, kötü aktörlerin gerçek bir kullanıcı hesabına erişememesini sağlamaya çalışır.  

* Her Azure Etkin Dizin veri merkezi kilitlemeyi bağımsız olarak izler. Kullanıcı her veri merkezine çarparsa, kullanıcının (threshold_limit * datacenter_count) sayıda denemesi olacaktır.
* Akıllı Kilitleme, kötü bir aktör le gerçek kullanıcı arasında ayrım yapmak için tanıdık konum ve yabancı konum kullanır. Tanıdık ve tanıdık konumlarda her ikisi de ayrı lokavt sayaçları olacaktır.

Akıllı kilitleme, şirket içi Active Directory hesaplarını saldırganlar tarafından kilitlenmekten korumak için parola karma eşitleme veya geçiş kimlik doğrulaması kullanarak karma dağıtımlarla tümleştirilebilir. Azure AD'de akıllı kilitleme ilkelerini uygun şekilde ayarlayarak, saldırılar şirket içi Active Directory'ye ulaşmadan önce filtrelenebilir.

Geçiş [kimlik doğrulaması](../hybrid/how-to-connect-pta.md)kullanırken, şundan emin olmanız gerekir:

* Azure AD kilitleme eşiği, Etkin Dizin hesabı kilitleme eşiğinin **altındadır.** Değerleri, Etkin Dizin hesabı kilitleme eşiğinin Azure AD kilitleme eşiğinin en az iki veya üç katı daha uzun olacak şekilde ayarlayın. 
* Azure AD kilitleme süresi, süre sonunda Etkin Dizin sıfırlama hesap kilitleme sayacından daha uzun ayarlanmalıdır. Azure REKLAM süresinin saniyeler içinde, AD süresinin ise dakikalar cinsinden ayarlandığını unutmayın. 

Örneğin, Azure REKLAM sayacınızın AD'den yüksek olmasını istiyorsanız, Azure AD'ınız 120 saniye (2 dakika) olurken, şirket içi REKLAMınız 1 dakikaya (60 saniye) ayarlanır.

> [!IMPORTANT]
> Şu anda, bir yönetici Akıllı Kilitleme özelliği tarafından kilitlendiklerinde kullanıcıların bulut hesaplarının kilidini açamaz. Yöneticinin kilitleme süresinin dolmasını beklemesi gerekir. Ancak, kullanıcı güvenilir bir aygıttan veya konumdan self servis parola sıfırlama (SSPR) kullanarak kilidini açabilir.

## <a name="verify-on-premises-account-lockout-policy"></a>Şirket içi hesap kilitleme ilkesini doğrulama

Şirket içi Active Directory hesap kilitleme ilkenizi doğrulamak için aşağıdaki yönergeleri kullanın:

1. Grup İlkesi Yönetimi aracını açın.
2. Kuruluşunuzun hesap kilitleme ilkesini içeren grup ilkesini (örneğin Varsayılan **Etki Alanı İlkesi)** düzenleme.
3. **Bilgisayar Yapılandırma** > **İlkeleri** > **Windows Ayarlar** > **Güvenlik Ayarları** > **Hesap Politikaları** > **Hesap Kilitleme İlke**göz atın.
4. Hesap **kilitleme eşiğinizi** doğrulayın ve **hesap kilitleme sayacını değerlerden sonra sıfırlayın.**

![Şirket içi Active Directory hesap kilitleme ilkesini değiştirme](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Azure AD akıllı kilitleme değerlerini yönetme

Kuruluş gereksinimlerinize bağlı olarak, akıllı kilitleme değerlerinin özelleştirilmiş olması gerekebilir. Kuruluşunuza özgü değerlerle akıllı kilitleme ayarlarının özelleştirilmesi, kullanıcılarınız için ücretli Azure REKLAM lisansları gerektirir.

Kuruluşunuz için akıllı kilitleme değerlerini denetlemek veya değiştirmek için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. *Azure Active Directory*'yi bulun ve seçin. **Güvenlik** > **Kimlik Doğrulama yöntemlerini** > seçin**Parola koruması.**
1. İlk **kilitlemeden**önce bir hesapta kaç başarısız oturum açma izni verildiğine bağlı olarak Kilitleme eşiğini ayarlayın. Varsayılan değer 10'dur.
1. Kilitleme **süresini saniyecinsinden,** her kilitlemenin saniye cinsinden uzunluğuna ayarlayın. Varsayılan değer 60 saniyedir (bir dakika).

> [!NOTE]
> Kilitlemeden sonra ilk oturum açma da başarısız olursa, hesap yeniden kilitlenir. Bir hesap tekrar tekrar kilitlenirse, kilitleme süresi artar.

![Azure portalında Azure AD akıllı kilitleme ilkesini özelleştirme](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)

## <a name="how-to-determine-if-the-smart-lockout-feature-is-working-or-not"></a>Akıllı kilitleme özelliğinin çalışıp çalışmadığını belirleme

Akıllı kilitleme eşiği tetiklendiğinde, hesap kilitliyken aşağıdaki iletiyi alırsınız:

**Yetkisiz kullanımı önlemek için hesabınız geçici olarak kilitlenir. Daha sonra tekrar deneyin ve hala sorun yaşıyorsanız, yöneticinize başvurun.**

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD'yi kullanarak kuruluşunuzdaki kötü parolaları nasıl yasaklarabileceğinizi öğrenin.](howto-password-ban-bad.md)
* [Kullanıcıların kendi hesaplarının kilidini açmalarına izin verecek şekilde self servis parola sıfırlamayı yapılandırın.](quickstart-sspr.md)
