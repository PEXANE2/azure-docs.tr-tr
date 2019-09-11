---
title: PıM-Azure Active Directory 'de Azure AD rolleri için güvenlik uyarılarını yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rolleri için güvenlik uyarılarını yapılandırma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3984d3276590357866c824d01ea8c51cf5b28fd7
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804396"
---
# <a name="configure-security-alerts-for-azure-ad-roles-in-pim"></a>PıM 'de Azure AD rolleri için güvenlik uyarılarını yapılandırma

Azure Active Directory (Azure AD) Privileged Identity Management (PıM), ortamınızda şüpheli veya güvenli olmayan bir etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde, PıM panosu üzerinde görüntülenir. Uyarıyı tetikleyen kullanıcıları veya rolleri listeleyen bir rapor görmek için uyarıyı seçin.

![Azure AD rolleri-uyarıları ve önem derecesini listeleme uyarı bölmesi](./media/pim-how-to-configure-security-alerts/pim-directory-alerts.png)

## <a name="security-alerts"></a>Güvenlik uyarıları

Bu bölümde, Azure AD rolleri için tüm güvenlik uyarıları ve nasıl düzeltileceğini ve nasıl önleneceği anlatılmaktadır. Önem derecesi aşağıdaki anlama sahiptir:

* **Yüksek**: İlke ihlali nedeniyle hemen eyleme gerek duyar.
* **Orta**: Acil eylem gerektirmez, ancak olası bir ilke ihlaline işaret eder.
* **Düşük**: Acil eylem gerektirmez, ancak tercih edilen bir ilke değişikliği önerir.

### <a name="administrators-arent-using-their-privileged-roles"></a>Yöneticiler ayrıcalıklı rollerini kullanmıyor

| | |
| --- | --- |
| **İnin** | Düşük |
| **Bu uyarıyı neden alıyorum?** | Ayrıcalıklı roller atanmış kullanıcılar, saldırının olasılığını artırır. Saldırganlar, etkin olarak kullanılmayan hesaplara ait olmaması durumunda daha da kolaydır. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve gerekmeyen ayrıcalıklı rollerden kaldırın. |
| **Önlem** | Yalnızca iş gerekçesinin olduğu kullanıcılara ayrıcalıklı roller atayın. </br>Kullanıcıların erişimine hala ihtiyacı olduğunu doğrulamak için düzenli [erişim İncelemeleri](pim-how-to-start-security-review.md) zamanlayın. |
| **Portalın risk azaltma eylemi** | Hesabı ayrıcalıklı rolünden kaldırır. |
| **Tetikleyici** | Bir Kullanıcı bir rolün etkinleştirilmeksizin belirli bir süre geçtiğinde tetiklenir. |
| **Gün sayısı** | Bu ayar, bir kullanıcının bir rolü etkinleştirmeden gidebileceği, 0 ile 100 arasında gün sayısını belirtir.|

### <a name="roles-dont-require-multi-factor-authentication-for-activation"></a>Roller etkinleştirme için çok faktörlü kimlik doğrulaması gerektirmez

| | |
| --- | --- |
| **İnin** | Düşük |
| **Bu uyarıyı neden alıyorum?** | MFA olmadan, güvenliği aşılmış kullanıcılar ayrıcalıklı rolleri etkinleştirebilir. |
| **Nasıl düzeltilir?** | Rol listesini gözden geçirin ve her rol için [MFA gerektir](pim-how-to-change-default-settings.md) . |
| **Önlem** | Her rol için [MFA gerektir](pim-how-to-change-default-settings.md) .  |
| **Portalın risk azaltma eylemi** | Ayrıcalıklı rolün etkinleştirilmesi için MFA 'yı gerekli hale getirir. |

### <a name="the-tenant-doesnt-have-azure-ad-premium-p2"></a>Kiracının Azure AD Premium P2 yok

| | |
| --- | --- |
| **İnin** | Düşük |
| **Bu uyarıyı neden alıyorum?** | Geçerli kiracının Azure AD Premium P2 yok. |
| **Nasıl düzeltilir?** | [Azure AD sürümleri](../fundamentals/active-directory-whatis.md)hakkındaki bilgileri gözden geçirin. Azure AD Premium P2 ' ye yükseltin. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Ayrıcalıklı bir roldeki olası eski hesaplar

| | |
| --- | --- |
| **İnin** | Orta |
| **Bu uyarıyı neden alıyorum?** | Ayrıcalıklı roldeki, son 90 gün içinde parolasını değiştirolmayan hesaplar. Bu hesaplar, tutulmayan ve saldırganların savunmasız olduğu hizmet veya paylaşılan hesaplar olabilir. |
| **Nasıl düzeltilir?** | Listedeki hesapları gözden geçirin. Artık erişim gerekmiyorsa, bunları ayrıcalıklı rollerden kaldırın. |
| **Önlem** | Parolayı bilen kullanıcılarda değişiklik olduğunda, paylaşılan hesapların güçlü parolalar döndürürken emin olun. </br>[Erişim gözden geçirmeleri](pim-how-to-start-security-review.md) kullanarak ayrıcalıklı rollere sahip hesapları düzenli olarak gözden geçirin ve artık gerekmeyen rol atamalarını kaldırın. |
| **Portalın risk azaltma eylemi** | Hesabı ayrıcalıklı rolünden kaldırır. |
| **En iyi uygulamalar** | Bir parola kullanarak kimlik doğrulayan ve genel yönetici veya güvenlik yöneticisi gibi son derece ayrıcalıklı yönetim rollerine atanan paylaşılan, hizmet ve acil durum erişim hesapları, parolalarının aşağıdaki durumlar için döndürülmelidir:<ul><li>Bir güvenlik olayından sonra yönetim erişimi haklarının kötüye kullanımını veya güvenliğinin aşılmasına neden olacak şekilde</li><li>Herhangi bir kullanıcının ayrıcalıkları, artık yönetici kalmayacak şekilde değiştirildikten sonra (örneğin, yönetici olan bir çalışan, veya kuruluştan ayrıldıktan sonra)</li><li>Düzenli aralıklarla (örneğin, üç aylık veya yıllık), bilinen bir ihlal olmaması veya BT ekip üzerinde değişiklik yapılmasa bile</li></ul>Birden çok kişinin bu hesapların kimlik bilgilerine erişimi olduğundan, rollerinin bırakılmış kişilerin hesaplara artık erişememesini sağlamak için kimlik bilgileri döndürülmelidir. [Daha fazla bilgi edinin](https://aka.ms/breakglass) |

### <a name="roles-are-being-assigned-outside-of-pim"></a>Roller PıM dışında atanıyor

| | |
| --- | --- |
| **İnin** | Yüksek |
| **Bu uyarıyı neden alıyorum?** | PıM dışında yapılan ayrıcalıklı rol atamaları düzgün izlenmiyor ve etkin bir saldırı gösterebilir. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve PıM dışında atanan ayrıcalıklı rollerden kaldırın. |
| **Önlem** | Kullanıcıların, PıM dışında ayrıcalıklı roller atandığını ve bundan sonraki atamaları yasakladığını araştırın. |
| **Portalın risk azaltma eylemi** | Hesabı ayrıcalıklı rolünden kaldırır. |

### <a name="there-are-too-many-global-administrators"></a>Çok fazla genel yönetici var

| | |
| --- | --- |
| **İnin** | Düşük |
| **Bu uyarıyı neden alıyorum?** | Genel yönetici, en yüksek ayrıcalıklı roldür. Küresel bir yöneticinin güvenliği tehlikeye girerse, saldırgan tüm ve tüm sisteminizi riske sokan tüm izinlerine erişim kazanır. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve kesinlikle genel yönetici rolüne gerek olmayan her türlü kaldırın. </br>Bu kullanıcılara daha düşük ayrıcalıklı roller atayın. |
| **Önlem** | Kullanıcılara gereken en az ayrıcalıklı rolü atayın. |
| **Portalın risk azaltma eylemi** | Hesabı ayrıcalıklı rolünden kaldırır. |
| **Tetikleyici** | İki farklı ölçüt karşılanırsa tetiklenir ve her ikisini de yapılandırabilirsiniz. İlk olarak, belirli bir genel yönetici eşiğine erişmeniz gerekir. İkinci olarak, toplam rol atamalarınızın belirli bir yüzdesi genel yönetici olmalıdır. Yalnızca bu ölçülerden birini karşılıyorsa, uyarı görünmez. |
| **En az genel yönetici sayısı** | Bu ayar, güvenli olmayan bir miktarı kabul ettiğiniz genel yönetici sayısını 2 ile 100 arasında belirtir. |
| **Genel yöneticilerin yüzdesi** | Bu ayar, ortamınızda güvenli olmayan% 0 ile% 100 arasında genel yönetici olan en düşük yönetici yüzdesini belirtir. |

### <a name="roles-are-being-activated-too-frequently"></a>Roller çok sık etkinleştiriliyor

| | |
| --- | --- |
| **İnin** | Düşük |
| **Bu uyarıyı neden alıyorum?** | Aynı kullanıcı tarafından aynı ayrıcalıklı rol için birden çok etkinleştirme saldırının bir imzadır. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve ayrıcalıklı rollerinin [Etkinleştirme süresinin](pim-how-to-change-default-settings.md) görevlerini gerçekleştirmesi için yeterince uzun ayarlandığından emin olun. |
| **Önlem** | Ayrıcalıklı roller için [Etkinleştirme süresinin](pim-how-to-change-default-settings.md) , kullanıcıların görevlerini gerçekleştirmesi için yeterince uzun ayarlandığından emin olun.</br>Birden çok yönetici tarafından paylaşılan hesaplara sahip ayrıcalıklı roller için [MFA gerektir](pim-how-to-change-default-settings.md) . |
| **Portalın risk azaltma eylemi** | Yok |
| **Tetikleyici** | Bir Kullanıcı belirli bir süre içinde aynı ayrıcalıklı rolü birden çok kez etkinleştirdiğinde tetiklenir. Hem zaman dönemini hem de etkinleştirme sayısını yapılandırabilirsiniz. |
| **Etkinleştirme yenileme zaman çerçevesi** | Bu ayar, şüpheli yenilemeler izlemek için kullanmak istediğiniz zaman dilimini gün, saat, dakika ve saniye cinsinden belirtir. |
| **Etkinleştirme yenilemesinin sayısı** | Bu ayar, seçtiğiniz zaman dilimi içinde, uyarının önemli olduğunu düşündüğünüz 2 ile 100 arasında etkinleştirme sayısını belirtir. Kaydırıcıyı taşıyarak veya metin kutusuna bir sayı yazarak bu ayarı değiştirebilirsiniz. |

## <a name="configure-security-alert-settings"></a>Güvenlik Uyarısı ayarlarını yapılandırma

PıM 'deki bazı güvenlik uyarılarını, ortamınız ve güvenlik hedeflerinizle çalışacak şekilde özelleştirebilirsiniz. Güvenlik Uyarısı ayarlarını açmak için aşağıdaki adımları izleyin:

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ne tıklayın.

1. **Ayarlar** ' a ve ardından **Uyarılar**' a tıklayın.

    ![Azure AD rolleri-uyarılar seçiliyken ayarlar](./media/pim-how-to-configure-security-alerts/settings-alerts.png)

1. Bu uyarının ayarını yapılandırmak için bir uyarı adına tıklayın.

    ![Seçili uyarı için güvenlik uyarısı ayarları bölmesi](./media/pim-how-to-configure-security-alerts/security-alert-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
