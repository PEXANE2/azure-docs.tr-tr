---
title: PIM'deki Azure REKLAM rolleri için güvenlik uyarıları - Azure AD | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde Azure AD rolleri Ayrıcalıklı Kimlik Yönetimi için güvenlik uyarılarını yapılandırın.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 03/05/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 86dbcdc24c90ba8b161b041af96cbdd0665ad827
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253318"
---
# <a name="configure-security-alerts-for-azure-ad-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rolleri için güvenlik uyarılarını yapılandırma

Ayrıcalıklı Kimlik Yönetimi (PIM), Azure Etkin Dizin (Azure AD) kuruluşunuzda şüpheli veya güvenli olmayan etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde, Ayrıcalıklı Kimlik Yönetimi panosunda gösterir. Uyarıyı tetikleyen kullanıcıları veya rolleri listeleyen bir rapor görmek için uyarıyı seçin.

## <a name="determine-your-version-of-pim"></a>PIM sürümünüzü belirleyin

Kasım 2019'dan itibaren, Ayrıcalıklı Kimlik Yönetimi'nin Azure AD rolleri bölümü, Azure kaynak rollerinin deneyimleriyle eşleşen yeni bir sürüme güncelleniyor. Bu, [varolan API'de değişikliklerin](azure-ad-roles-features.md#api-changes)yanı sıra ek özellikler de oluşturur. Yeni sürüm kullanıma alınırken, bu makalede hangi yordamları izleyeceğiniz, şu anda sahip olduğunuz Ayrıcalıklı Kimlik Yönetimi sürümüne bağlıdır. Ayrıcalıklı Kimlik Yönetimi'nin hangi sürümüne sahip olduğunuzu belirlemek için bu bölümdeki adımları izleyin. Ayrıcalıklı Kimlik Yönetimi sürümünüzü aldıktan sonra, bu makalede bu sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünde olan bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın. Genel bakış sayfasının üst kısmında bir başlık varsa, bu makalenin **Yeni sürüm** sekmesindeki yönergeleri izleyin. Aksi takdirde, Önceki **sürüm** sekmesindeki yönergeleri izleyin.

  [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

Azure AD rolleri için güvenlik uyarılarını araştırmak için bu makaledeki adımları izleyin.

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

![Azure AD rolleri - Uyarı bölmesi listesi uyarıları ve önem derecesi](./media/pim-how-to-configure-security-alerts/view-alerts.png)

## <a name="security-alerts"></a>Güvenlik uyarıları

Bu bölümde, Azure AD rolleri için tüm güvenlik uyarılarının yanı sıra nasıl düzeltilen ve nasıl önleneceğimizle birlikte listelenir. Önem derecesi aşağıdaki anlama gelir:

- **Yüksek**: Bir ilke ihlali nedeniyle derhal harekete geçilmesini gerektirir.
- **Orta**: Hemen harekete geçilmesini gerektirmez, ancak olası bir politika ihlalinin sinyallerini vermektedir.
- **Düşük**: Hemen harekete geçmeyi gerektirmez, ancak tercih edilebilir bir politika değişikliği önerir.

### <a name="administrators-arent-using-their-privileged-roles"></a>Yöneticiler ayrıcalıklı rollerini kullanmıyor

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | İhtiyaçduymadıkları ayrıcalıklı roller atanan kullanıcılar saldırı olasılığını artırır. Ayrıca, etkin olarak kullanılmayan hesaplarda saldırganların fark edilmeden kalması da daha kolaydır. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve gereksinim duymadıkları ayrıcalıklı rollerden kaldırın. |
| **Önleme** | Yalnızca iş gerekçesi olan kullanıcılara ayrıcalıklı roller atayın. </br>Kullanıcıların erişimlerine hala ihtiyaç duyduğunu doğrulamak için düzenli [erişim incelemeleri](pim-how-to-start-security-review.md) zamanlayın. |
| **Portal içi azaltma eylemi** | Hesabı ayrıcalıklı rollerinden kaldırır. |
| **Tetikleyici** | Bir kullanıcı bir rolü etkinleştirmeden belirli sayıda günü geçerse tetiklenir. |
| **Gün sayısı** | Bu ayar, kullanıcının bir rolü etkinleştirmeden gidebileceği en fazla gün sayısını 0'dan 100'e kadar belirtir.|

### <a name="roles-dont-require-multi-factor-authentication-for-activation"></a>Roller etkinleştirme için çok faktörlü kimlik doğrulaması gerektirmez

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | Çok faktörlü kimlik doğrulama olmadan, gizliliği ihlal edilen kullanıcılar ayrıcalıklı rolleri etkinleştirebilir. |
| **Nasıl düzeltilir?** | Rollerin listesini gözden geçirin ve her rol için [çok faktörlü kimlik doğrulaması gerektirir.](pim-how-to-change-default-settings.md) |
| **Önleme** | Her rol için [MFA gerektirir.](pim-how-to-change-default-settings.md)  |
| **Portal içi azaltma eylemi** | Ayrıcalıklı rolün etkinleştirilmesi için çok faktörlü kimlik doğrulaması gerekli kılar. |

### <a name="the-organization-doesnt-have-azure-ad-premium-p2"></a>Kuruluşun Azure AD Premium P2'si yok

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | Geçerli Azure AD kuruluşunun Azure AD Premium P2'si yok. |
| **Nasıl düzeltilir?** | Azure AD sürümleri hakkındaki bilgileri gözden [geçirin.](../fundamentals/active-directory-whatis.md) Azure AD Premium P2'ye yükseltin. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Ayrıcalıklı bir roldeki olası eski hesaplar

| | |
| --- | --- |
| **Önem Derecesi** | Orta |
| **Neden bu uyarıyı alıyorum?** | Ayrıcalıklı bir roldeki hesaplar son 90 gün içinde parolalarını değiştirmemiştir. Bu hesaplar, bakımı olmayan ve saldırganlara karşı savunmasız olan hizmet veya paylaşılan hesaplar olabilir. |
| **Nasıl düzeltilir?** | Listedeki hesapları gözden geçirin. Artık erişime ihtiyaçları yoksa, bunları ayrıcalıklı rollerinden kaldırın. |
| **Önleme** | Parolayı bilen kullanıcılarda bir değişiklik olduğunda paylaşılan hesapların güçlü parolalar döndürdüğünü sağlayın. </br>[Erişim incelemelerini](pim-how-to-start-security-review.md) kullanarak ayrıcalıklı rolleri olan hesapları düzenli olarak gözden geçirin ve artık gerekmeden rol atamalarını kaldırın. |
| **Portal içi azaltma eylemi** | Hesabı ayrıcalıklı rollerinden kaldırır. |
| **En iyi uygulamalar** | Parola kullanarak kimlik doğrulaması yapan ve Global yönetici veya Güvenlik yöneticisi gibi yüksek ayrıcalıklı yönetim rollerine atanan paylaşılan, hizmet ve acil erişim hesaplarının parolalarının aşağıdaki durumlar için döndürülmesi gerekir:<ul><li>İdari erişim haklarının kötüye kullanılması veya tehlikeye atıldığı bir güvenlik olayından sonra</li><li>Herhangi bir kullanıcının ayrıcalıkları artık yönetici olmayacak şekilde değiştirildikten sonra (örneğin, yönetici olan bir çalışanın BT'den ayrılması veya kuruluştan ayrılmasından sonra)</li><li>Düzenli aralıklarla (örneğin, üç aylık veya yıllık), bilinen bir ihlal veya BT personelinde değişiklik olmasa bile</li></ul>Birden çok kişi bu hesapların kimlik bilgilerine erişebildiği için, rollerini bırakan kişilerin artık hesaplara erişememesini sağlamak için kimlik bilgileri döndürülmelidir. [Hesapların güvenliğini sağlama hakkında daha fazla bilgi edinin](../users-groups-roles/directory-admin-roles-secure.md) |

### <a name="roles-are-being-assigned-outside-of-privileged-identity-management"></a>İmtiyazlı Kimlik Yönetimi dışında roller atanmaktadır

| | |
| --- | --- |
| **Önem Derecesi** | Yüksek |
| **Neden bu uyarıyı alıyorum?** | Ayrıcalıklı Kimlik Yönetimi dışında yapılan ayrıcalıklı rol atamaları düzgün bir şekilde izlenmez ve etkin bir saldırıyı gösterebilir. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve ayrıcalıklı kimlik yönetimi dışında atanan ayrıcalıklı rollerden kaldırın. |
| **Önleme** | Kullanıcıların Ayrıcalıklı Kimlik Yönetimi dışında ayrıcalıklı görevlerin nerede atandığını araştırın ve gelecekteki atamaları buradan yasaklar. |
| **Portal içi azaltma eylemi** | Kullanıcıyı ayrıcalıklı rolünden kaldırır. |

### <a name="there-are-too-many-global-administrators"></a>Çok fazla genel yönetici var

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | Global yönetici en yüksek ayrıcalıklı roldür. Bir Global Administrator tehlikeye girerse, saldırgan tüm izinlerine erişir ve bu da tüm sisteminizi riske sokar. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve Global yönetici rolüne kesinlikle ihtiyaç duymayan kullanıcıları kaldırın. </br>Bunun yerine bu kullanıcılara daha düşük ayrıcalıklı roller atayın. |
| **Önleme** | Kullanıcılara gereksinim duydukları en az ayrıcalıklı rolü atayın. |
| **Portal içi azaltma eylemi** | Hesabı ayrıcalıklı rollerinden kaldırır. |
| **Tetikleyici** | İki farklı ölçüt karşılanırsa tetiklenir ve her ikisini de yapılandırabilirsiniz. İlk olarak, Global yöneticilerin belirli bir eşiğine ulaşmanız gerekir. İkinci olarak, toplam rol atamalarınızın belirli bir yüzdesi Global yöneticiler olmalıdır. Bu ölçümlerden yalnızca birini karşılıyorsanız, uyarı görüntülenmez. |
| **En az Global Yönetici sayısı** | Bu ayar, Azure REKLAM kuruluşunuz için çok az olduğunu düşündüğünüz 2'den 100'e kadar olan Genel yönetici sayısını belirtir. |
| **Genel Yöneticilerin Yüzdesi** | Bu ayar, Azure AD kuruluşunuzun dip yapmasını istemediğiniz minimum yüzdeyi %0'dan %100'e kadar Genel yönetici olarak belirtir. |

### <a name="roles-are-being-activated-too-frequently"></a>Roller çok sık etkinleştiriliyor

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | Aynı kullanıcı tarafından aynı ayrıcalıklı role birden çok etkinleştirme bir saldırının işaretidir. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve ayrıcalıklı rollerinin [etkinleştirme süresinin](pim-how-to-change-default-settings.md) görevlerini yerine getirmeleri için yeterince uzun olduğundan emin olun. |
| **Önleme** | Ayrıcalıklı rollerin [etkinleştirme süresinin](pim-how-to-change-default-settings.md) kullanıcıların görevlerini gerçekleştirebilmeleri için yeterince uzun olduğundan emin olun.</br>Birden çok yönetici tarafından paylaşılan hesapları olan ayrıcalıklı roller için [çok faktörlü kimlik doğrulaması gerektirir.](pim-how-to-change-default-settings.md) |
| **Portal içi azaltma eylemi** | Yok |
| **Tetikleyici** | Kullanıcı belirli bir süre içinde aynı ayrıcalıklı rolü birden çok kez etkinleştirirse tetiklenir. Hem zaman dönemini hem de etkinleştirme sayısını yapılandırabilirsiniz. |
| **Etkinleştirme yenileme zaman dilimi** | Bu ayar, şüpheli yenilemeleri izlemek için kullanmak istediğiniz gün, saat, dakika ve ikinci zaman dilimini belirtir. |
| **Etkinleştirme yenileme sayısı** | Bu ayar, seçtiğiniz zaman dilimi içinde, size bildirilmek istediğiniz 2'den 100'e kadar olan etkinleştirme sayısını belirtir. Kaydırıcıyı taşıyarak veya metin kutusuna bir sayı yazarak bu ayarı değiştirebilirsiniz. |

## <a name="configure-security-alert-settings"></a>Güvenlik uyarı ayarlarını yapılandırma

Uyarılar sayfasından **Ayarlar**sayfasına gidin.

![Ayarlar vurgulanmış uyarılar sayfası](media/pim-how-to-configure-security-alerts/alert-settings.png)

Ortamınız ve güvenlik hedefleriniz ile çalışmak için farklı uyarılardaki ayarları özelleştirin.

![Ayarları etkinleştirmek ve yapılandırmak için uyarı için sayfa ayarlama](media/pim-resource-roles-configure-alerts/rbac-alert-settings.png)

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

![Azure AD rolleri - Uyarı bölmesi listesi uyarıları ve önem derecesi](./media/pim-how-to-configure-security-alerts/pim-directory-alerts.png)

## <a name="security-alerts"></a>Güvenlik uyarıları

Bu bölümde, Azure AD rolleri için tüm güvenlik uyarılarının yanı sıra nasıl düzeltilen ve nasıl önleneceğimizle birlikte listelenir. Önem derecesi aşağıdaki anlama gelir:

- **Yüksek**: Bir ilke ihlali nedeniyle derhal harekete geçilmesini gerektirir.
- **Orta**: Hemen harekete geçilmesini gerektirmez, ancak olası bir politika ihlalinin sinyallerini vermektedir.
- **Düşük**: Hemen harekete geçmeyi gerektirmez, ancak tercih edilebilir bir politika değişikliği önerir.

### <a name="administrators-arent-using-their-privileged-roles"></a>Yöneticiler ayrıcalıklı rollerini kullanmıyor

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | İhtiyaçduymadıkları ayrıcalıklı roller atanan kullanıcılar saldırı olasılığını artırır. Ayrıca, etkin olarak kullanılmayan hesaplarda saldırganların fark edilmeden kalması da daha kolaydır. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve gereksinim duymadıkları ayrıcalıklı rollerden kaldırın. |
| **Önleme** | Yalnızca iş gerekçesi olan kullanıcılara ayrıcalıklı roller atayın. </br>Kullanıcıların erişimlerine hala ihtiyaç duyduğunu doğrulamak için düzenli [erişim incelemeleri](pim-how-to-start-security-review.md) zamanlayın. |
| **Portal içi azaltma eylemi** | Hesabı ayrıcalıklı rollerinden kaldırır. |
| **Tetikleyici** | Bir kullanıcı bir rolü etkinleştirmeden belirli sayıda günü geçerse tetiklenir. |
| **Gün sayısı** | Bu ayar, kullanıcının bir rolü etkinleştirmeden gidebileceği en fazla gün sayısını 0'dan 100'e kadar belirtir.|

### <a name="roles-dont-require-multi-factor-authentication-for-activation"></a>Roller etkinleştirme için çok faktörlü kimlik doğrulaması gerektirmez

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | Çok faktörlü kimlik doğrulama olmadan, gizliliği ihlal edilen kullanıcılar ayrıcalıklı rolleri etkinleştirebilir. |
| **Nasıl düzeltilir?** | Rollerin listesini gözden geçirin ve her rol için [çok faktörlü kimlik doğrulaması gerektirir.](pim-how-to-change-default-settings.md) |
| **Önleme** | Her rol için [MFA gerektirir.](pim-how-to-change-default-settings.md)  |
| **Portal içi azaltma eylemi** | Ayrıcalıklı rolün etkinleştirilmesi için çok faktörlü kimlik doğrulaması gerekli kılar. |

### <a name="the-organization-doesnt-have-azure-ad-premium-p2"></a>Kuruluşun Azure AD Premium P2'si yok

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | Geçerli Azure AD kuruluşunun Azure AD Premium P2'si yok. |
| **Nasıl düzeltilir?** | Azure AD sürümleri hakkındaki bilgileri gözden [geçirin.](../fundamentals/active-directory-whatis.md) Azure AD Premium P2'ye yükseltin. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Ayrıcalıklı bir roldeki olası eski hesaplar

| | |
| --- | --- |
| **Önem Derecesi** | Orta |
| **Neden bu uyarıyı alıyorum?** | Ayrıcalıklı bir roldeki hesaplar son 90 gün içinde parolalarını değiştirmemiştir. Bu hesaplar, bakımı olmayan ve saldırganlara karşı savunmasız olan hizmet veya paylaşılan hesaplar olabilir. |
| **Nasıl düzeltilir?** | Listedeki hesapları gözden geçirin. Artık erişime ihtiyaçları yoksa, bunları ayrıcalıklı rollerinden kaldırın. |
| **Önleme** | Parolayı bilen kullanıcılarda bir değişiklik olduğunda paylaşılan hesapların güçlü parolalar döndürdüğünü sağlayın. </br>[Erişim incelemelerini](pim-how-to-start-security-review.md) kullanarak ayrıcalıklı rolleri olan hesapları düzenli olarak gözden geçirin ve artık gerekmeden rol atamalarını kaldırın. |
| **Portal içi azaltma eylemi** | Hesabı ayrıcalıklı rollerinden kaldırır. |
| **En iyi uygulamalar** | Parola kullanarak kimlik doğrulaması yapan ve Global yönetici veya Güvenlik yöneticisi gibi yüksek ayrıcalıklı yönetim rollerine atanan paylaşılan, hizmet ve acil erişim hesaplarının parolalarının aşağıdaki durumlar için döndürülmesi gerekir:<ul><li>İdari erişim haklarının kötüye kullanılması veya tehlikeye atıldığı bir güvenlik olayından sonra</li><li>Herhangi bir kullanıcının ayrıcalıkları artık yönetici olmayacak şekilde değiştirildikten sonra (örneğin, yönetici olan bir çalışanın BT'den ayrılması veya kuruluştan ayrılmasından sonra)</li><li>Düzenli aralıklarla (örneğin, üç aylık veya yıllık), bilinen bir ihlal veya BT personelinde değişiklik olmasa bile</li></ul>Birden çok kişi bu hesapların kimlik bilgilerine erişebildiği için, rollerini bırakan kişilerin artık hesaplara erişememesini sağlamak için kimlik bilgileri döndürülmelidir. [Daha fazlasını öğrenin](https://aka.ms/breakglass) |

### <a name="roles-are-being-assigned-outside-of-privileged-identity-management"></a>İmtiyazlı Kimlik Yönetimi dışında roller atanmaktadır

| | |
| --- | --- |
| **Önem Derecesi** | Yüksek |
| **Neden bu uyarıyı alıyorum?** | Ayrıcalıklı Kimlik Yönetimi dışında yapılan ayrıcalıklı rol atamaları düzgün bir şekilde izlenmez ve etkin bir saldırıyı gösterebilir. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve ayrıcalıklı kimlik yönetimi dışında atanan ayrıcalıklı rollerden kaldırın. |
| **Önleme** | Kullanıcıların Ayrıcalıklı Kimlik Yönetimi dışında ayrıcalıklı görevlerin nerede atandığını araştırın ve gelecekteki atamaları buradan yasaklar. |
| **Portal içi azaltma eylemi** | Kullanıcıyı ayrıcalıklı rolünden kaldırır. |

### <a name="there-are-too-many-global-administrators"></a>Çok fazla genel yönetici var

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | Global yönetici en yüksek ayrıcalıklı roldür. Bir Global Administrator tehlikeye girerse, saldırgan tüm izinlerine erişir ve bu da tüm sisteminizi riske sokar. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve Global yönetici rolüne kesinlikle ihtiyaç duymayan kullanıcıları kaldırın. </br>Bunun yerine bu kullanıcılara daha düşük ayrıcalıklı roller atayın. |
| **Önleme** | Kullanıcılara gereksinim duydukları en az ayrıcalıklı rolü atayın. |
| **Portal içi azaltma eylemi** | Hesabı ayrıcalıklı rollerinden kaldırır. |
| **Tetikleyici** | İki farklı ölçüt karşılanırsa tetiklenir ve her ikisini de yapılandırabilirsiniz. İlk olarak, Global yöneticilerin belirli bir eşiğine ulaşmanız gerekir. İkinci olarak, toplam rol atamalarınızın belirli bir yüzdesi Global yöneticiler olmalıdır. Bu ölçümlerden yalnızca birini karşılıyorsanız, uyarı görüntülenmez. |
| **En az Global Yönetici sayısı** | Bu ayar, Azure REKLAM kuruluşunuz için çok az olduğunu düşündüğünüz 2'den 100'e kadar olan Genel yönetici sayısını belirtir. |
| **Genel Yöneticilerin Yüzdesi** | Bu ayar, Azure AD kuruluşunuzun dip yapmasını istemediğiniz minimum yüzdeyi %0'dan %100'e kadar Genel yönetici olarak belirtir. |

### <a name="roles-are-being-activated-too-frequently"></a>Roller çok sık etkinleştiriliyor

| | |
| --- | --- |
| **Önem Derecesi** | Düşük |
| **Neden bu uyarıyı alıyorum?** | Aynı kullanıcı tarafından aynı ayrıcalıklı role birden çok etkinleştirme bir saldırının işaretidir. |
| **Nasıl düzeltilir?** | Listedeki kullanıcıları gözden geçirin ve ayrıcalıklı rollerinin [etkinleştirme süresinin](pim-how-to-change-default-settings.md) görevlerini yerine getirmeleri için yeterince uzun olduğundan emin olun. |
| **Önleme** | Ayrıcalıklı rollerin [etkinleştirme süresinin](pim-how-to-change-default-settings.md) kullanıcıların görevlerini gerçekleştirebilmeleri için yeterince uzun olduğundan emin olun.</br>Birden çok yönetici tarafından paylaşılan hesapları olan ayrıcalıklı roller için [çok faktörlü kimlik doğrulaması gerektirir.](pim-how-to-change-default-settings.md) |
| **Portal içi azaltma eylemi** | Yok |
| **Tetikleyici** | Kullanıcı belirli bir süre içinde aynı ayrıcalıklı rolü birden çok kez etkinleştirirse tetiklenir. Hem zaman dönemini hem de etkinleştirme sayısını yapılandırabilirsiniz. |
| **Etkinleştirme yenileme zaman dilimi** | Bu ayar, şüpheli yenilemeleri izlemek için kullanmak istediğiniz gün, saat, dakika ve ikinci zaman dilimini belirtir. |
| **Etkinleştirme yenileme sayısı** | Bu ayar, seçtiğiniz zaman dilimi içinde, size bildirilmek istediğiniz 2'den 100'e kadar olan etkinleştirme sayısını belirtir. Kaydırıcıyı taşıyarak veya metin kutusuna bir sayı yazarak bu ayarı değiştirebilirsiniz. |

## <a name="configure-security-alert-settings"></a>Güvenlik uyarı ayarlarını yapılandırma

Kuruluşunuzun gereksinimleri ve güvenlik hedefleri ile çalışmak için Ayrıcalıklı Kimlik Yönetimi'ndeki bazı güvenlik uyarılarını özelleştirebilirsiniz. Güvenlik uyarısı ayarlarını açmak için aşağıdaki adımları izleyin:

1. Azure AD'de **Ayrıcalıklı Kimlik Yönetimi'ni** açın.

1. **Azure AD rollerini**seçin.

1. **Ayarlar'ı** ve ardından **Uyarıları**seçin.

    ![Azure AD rolleri - Uyarıların seçili ayarları](./media/pim-how-to-configure-security-alerts/settings-alerts.png)

1. Bu uyarının ayarını yapılandırmak için bir uyarı adı seçin.

    ![Seçili uyarı için, güvenlik uyarı ayarları bölmesi](./media/pim-how-to-configure-security-alerts/security-alert-settings.png)

---

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
