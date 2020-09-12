---
title: Azure portal kullanarak cihazları yönetme | Microsoft Docs
description: Azure portal kullanarak cihazları yönetme hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: how-to
ms.date: 08/03/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: hafowler
ms.collection: M365-identity-device-management
ms.openlocfilehash: 569bc54d262e8611679c8aa86ed4dbfbed61aa79
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89442688"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Azure portalını kullanarak cihaz kimliklerini yönetme

Azure AD, cihaz kimliklerini yönetmek için size merkezi bir yer sağlar.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Azure Active Directory**  >  **cihazlara**gidin.

[![Azure portal tüm cihazlar görünümü](./media/device-management-azure-portal/all-devices-azure-portal.png)](./media/device-management-azure-portal/all-devices-azure-portal.png#lightbox)

**Tüm cihazlar** sayfası şunları yapmanızı sağlar:

- Aşağıdakiler dahil olmak üzere cihazları tanımla:
   - Azure AD 'ye katılmış veya kayıtlı olan cihazlar.
   - [Windows Autopilot](/windows/deployment/windows-autopilot/windows-autopilot)kullanılarak dağıtılan cihazlar.
   - [Evrensel Yazdırma](/universal-print/fundamentals/universal-print-getting-started) kullanan yazıcılar
- Etkinleştirme, devre dışı bırakma, silme veya yönetme gibi cihaz kimlik yönetimi görevlerini gerçekleştirin.
   - [Yazıcıların](/universal-print/fundamentals/) ve [Windows AUTOPILOT](/windows/deployment/windows-autopilot/windows-autopilot) cihazlarının Azure AD 'de sınırlı sayıda yönetim seçeneği vardır. Bunların ilgili yönetici arabirimlerinden yönetilmesi gerekir.
- Cihaz kimliği ayarlarınızı yapılandırın.
- Enterprise State Roaming etkinleştirin veya devre dışı bırakın.
- Cihazla ilgili denetim günlüklerini gözden geçirme

## <a name="manage-devices"></a>Cihazları yönetme

Azure AD 'de cihazları yönetmek için iki konum vardır:

- **Azure Portal**  >  **Azure Active Directory**  >  **Cihazlar**
- **Azure Portal**  >  **Azure Active Directory**  >  **Kullanıcılar** > bir Kullanıcı > **cihaz** seçin

Her iki seçenek de yöneticilere şunları yapabilme olanağı sağlar:

- Cihazlar için arama yapın.
- Aşağıdakiler dahil cihaz ayrıntılarına bakın:
    - Cihaz adı
    - Cihaz Kimliği
    - İşletim sistemi ve sürüm
    - Katılım türü
    - Sahip
    - Mobil cihaz yönetimi ve uyumluluğu
    - BitLocker kurtarma anahtarı
- Etkinleştirme, devre dışı bırakma, silme veya yönetme gibi cihaz kimlik yönetimi görevlerini gerçekleştirin.
   - [Yazıcıların](/universal-print/fundamentals/) ve [Windows AUTOPILOT](/windows/deployment/windows-autopilot/windows-autopilot) cihazlarının Azure AD 'de sınırlı sayıda yönetim seçeneği vardır. Bunların ilgili yönetici arabirimlerinden yönetilmesi gerekir.

> [!TIP]
> - Karma Azure AD 'ye katılmış Windows 10 cihazlarının sahibi yok. Bir cihazı sahibine göre arıyorsanız ve bulamıyorsanız, cihaz KIMLIĞINE göre arama yapın.
>
> - KAYıTLı sütun altında "bekliyor" durumuna sahip "hibrit Azure AD 'ye katılmış" bir cihaz görürseniz, cihazın Azure AD Connect 'ten eşitlendiğini ve istemciden kayıt tamamlamayı beklediğini gösterir. [Hibrit Azure AD JOIN uygulamanızın nasıl planlanacağı](hybrid-azuread-join-plan.md)hakkında daha fazla bilgi edinin. Daha fazla bilgi için, [cihazlar sık sorulan sorular](faq.md)makalesinde bulunabilir.
>
> - Bazı iOS cihazlarında, kesme işaretleri içeren cihaz adları, kesme işareti gibi farklı karakterler kullanabilir. Bu nedenle, bu tür cihazları aramak biraz karmaşık değildir. arama sonuçlarını doğru şekilde görmüyorsanız, arama dizesinin eşleşen kesme işareti karakteri içerdiğinden emin olun.

### <a name="manage-an-intune-device"></a>Intune cihazını yönetme

Bir Intune yöneticisiyseniz, MDM 'nin **Microsoft Intune**olarak işaretlendiği cihazları yönetebilirsiniz. Cihaz Microsoft Intune kayıtlı değilse, "Yönet" seçeneği gri kalır.

### <a name="enable-or-disable-an-azure-ad-device"></a>Azure AD cihazını etkinleştirme veya devre dışı bırakma

Cihazları etkinleştirmek veya devre dışı bırakmak için iki seçeneğiniz vardır:

- Bir veya daha fazla cihaz seçildikten sonra **tüm cihazlar** sayfasındaki araç çubuğu.
- Belirli bir cihazda detaya gidildikten sonra araç çubuğu.

> [!IMPORTANT]
> - Bir cihazı etkinleştirmek veya devre dışı bırakmak için Azure AD 'de bir genel yönetici veya bulut cihaz yöneticisi olmanız gerekir. 
> - Bir cihazın devre dışı bırakılması, cihazın Azure AD ile başarılı bir şekilde kimlik doğrulamasına engel olur, böylece cihazın cihaz tabanlı koşullu erişim ile korunan Azure AD kaynaklarınıza erişmesini veya Iş kimlik bilgileri için Windows Hello 'Yu kullanmasını önler.
> - Cihazı devre dışı bırakmak, hem birincil yenileme belirtecini (PRT) hem de cihazdaki yenileme belirteçlerini (RT) iptal eder.
> - Azure AD 'de yazıcılar etkinleştirilemez veya devre dışı bırakılamaz.

### <a name="delete-an-azure-ad-device"></a>Azure AD cihazını silme

Bir cihazı silmek için iki seçeneğiniz vardır:

- Bir veya daha fazla cihaz seçildikten sonra **tüm cihazlar** sayfasındaki araç çubuğu.
- Belirli bir cihazda detaya gidildikten sonra araç çubuğu.

> [!IMPORTANT]
> - Bir cihazı silmek için Azure AD 'de bulut Cihaz Yöneticisi, Intune Yöneticisi veya genel yönetici rolü atanmış olmalıdır.
> - Yazıcılar ve Windows Autopilot cihazları Azure AD 'de silinemez
> - Bir cihazı silme:
>    - Bir cihazın Azure AD kaynaklarınıza erişmesini önler.
>    - Cihaza iliştirilmiş tüm ayrıntıları kaldırır, örneğin, Windows cihazları için BitLocker anahtarları.  
>    - Kurtarılabilir olmayan bir etkinliği temsil eder ve gerekli olmadığı takdirde önerilmez.

Bir cihaz başka bir yönetim yetkilisi tarafından yönetiliyorsa (örneğin, Microsoft Intune), cihazın Azure AD 'de silinmeden önce temizlendiğinden/devre dışı olduğundan emin olun. Herhangi bir cihazı silmeden önce [eski cihazların nasıl yönetileceğini](manage-stale-devices.md) inceleyin.

### <a name="view-or-copy-device-id"></a>Cihaz KIMLIĞINI görüntüle veya Kopyala

Cihazdaki cihaz KIMLIĞI ayrıntılarını doğrulamak veya sorun giderme sırasında PowerShell 'i kullanmak için bir cihaz KIMLIĞI kullanabilirsiniz. Kopyala seçeneğine erişmek için cihaza tıklayın.

![Cihaz KIMLIĞINI görüntüleme](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>BitLocker anahtarlarını görüntüleme veya kopyalama

Kullanıcıların şifrelenmiş sürücüleri kurtarmasına izin vermek için BitLocker anahtarlarını görüntüleyebilir ve kopyalayabilirsiniz. Bu anahtarlar yalnızca şifrelenmiş ve anahtarları Azure AD 'de depolanan Windows cihazlarında kullanılabilir. **Kurtarma anahtarını göster**' i seçerek, bir cihazın ayrıntılarına erişirken bu anahtarları bulabilirsiniz. **Kurtarma anahtarını göster** ' i seçtiğinizde, kategoride bulabileceğiniz bir denetim günlüğü oluşturulur `KeyManagement` .

![BitLocker anahtarlarını görüntüle](./media/device-management-azure-portal/device-details-show-bitlocker-key.png)

BitLocker anahtarlarını görüntülemek veya kopyalamak için, cihazın sahibi ya da aşağıdaki rollerden en az birine sahip olan bir kullanıcı olmanız gerekir:

- Bulut Cihaz Yöneticisi
- Genel Yönetici
- Yardım Masası Yöneticisi
- Intune Hizmet Yöneticisi
- Güvenlik Yöneticisi
- Güvenlik Okuyucusu

### <a name="device-list-filtering-preview"></a>Cihaz listesi filtrelemesi (Önizleme)

Daha önce, cihaz listesini etkinliğe ve etkin duruma göre filtreleyebilirsiniz. Bu önizleme artık cihazlar listesini bir cihazda aşağıdaki özniteliklere göre filtrelemenize olanak tanır:

- Etkin durum
- Uyumlu durum
- Katılım türü (Azure AD 'ye katılmış, karma Azure AD 'ye katılmış, Azure AD kayıtlı)
- Etkinlik zaman damgası
- İşletim Sistemi
- Cihaz türü (yazıcılar, güvenli VM 'Ler, paylaşılan cihazlar, kayıtlı cihazlar)

**Tüm cihazlar** görünümünde önizleme filtreleme işlevini etkinleştirmek için:

![Önizleme işlevselliğinin filtrelenmesini etkinleştir](./media/device-management-azure-portal/device-filter-preview-enable.png)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Azure Active Directory**  >  **cihazlara**gidin.
1. Görüntülenen başlığı seçin, **yeni cihaz filtreleme geliştirmelerini deneyin. Önizlemeyi etkinleştirmek için tıklayın.**

Artık, **tüm cihazlar** görünümlarınıza **filtre ekleme** imkanına sahip olursunuz.

## <a name="configure-device-settings"></a>Cihaz ayarlarını yapılandırma

Azure AD portalını kullanarak cihaz kimliklerini yönetmek için, bu cihazların [kayıtlı veya](overview.md) Azure AD 'ye katılmış olmaları gerekir. Yönetici olarak, aşağıdaki cihaz ayarlarını yapılandırarak cihazları kaydetme ve birleştirme sürecini denetleyebilirsiniz.

![Azure AD ile ilgili cihaz ayarları](./media/device-management-azure-portal/device-settings-azure-portal.png)

- **Kullanıcılar cihazları Azure AD 'ye katabilir** -Bu ayar, CIHAZLARıNı Azure AD 'ye katılmış cihazlar olarak kaydedebilen kullanıcıları seçmenize olanak sağlar. Varsayılan değer **Tümü**' dir.

> [!NOTE]
> **Kullanıcılar cihazları Azure AD 'ye katabilir** ve yalnızca Windows 10 ' da Azure AD katılımı için geçerlidir.

- **Azure AD 'ye katılmış cihazlarda ek yerel Yöneticiler** -bir cihazda yerel yönetici hakları verilen kullanıcıları seçebilirsiniz. Bu kullanıcılar, Azure AD 'de *cihaz yöneticileri* rolüne eklenir. Azure AD ve cihaz sahiplerine Genel Yöneticiler varsayılan olarak yerel yönetici haklarına sahiptir. Bu seçenek, Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünlerle kullanılabilen bir Premium sürüm özelliğidir.
- **Kullanıcılar cihazlarını Azure AD 'ye kaydedebileceği** için bu ayarı Windows 10 Personal, IOS, Android ve MacOS CIHAZLARıNıN Azure AD 'ye kaydolmalarına izin verecek şekilde yapılandırmanız gerekir. **Hiçbiri**' ni seçerseniz, CIHAZLARıN Azure AD 'ye kaydolmaya izin verilmez. Office 365 için Microsoft Intune veya mobil cihaz yönetimi (MDM) kaydı için kayıt gereklidir. Bu hizmetlerden birini yapılandırdıysanız, **Tümü** seçilidir ve **hiçbiri** kullanılamaz.
- **Cihazlara katılması Için Multi-Factor auth gerektir** -kullanıcıların CIHAZLARıNı Azure AD 'ye katmak için ek bir kimlik doğrulama faktörü sağlaması gerekip gerekmediğini seçebilirsiniz. Varsayılan değer **Hayır**' dır. Bir cihazı kaydederken çok faktörlü kimlik doğrulamasının gerekli olmasını öneririz. Bu hizmet için Multi-Factor Authentication 'ı etkinleştirmeden önce, cihazlarını kaydeden kullanıcılar için Multi-Factor Authentication ' ın yapılandırıldığından emin olmanız gerekir. Farklı Azure Multi-Factor Authentication hizmetleri hakkında daha fazla bilgi için bkz. [Azure Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md)kullanmaya başlama. 

> [!NOTE]
> **Cihazların katılmasını sağlamak Için Multi-Factor auth iste** ayarı, Azure AD 'ye katılmış veya Azure AD 'ye kayıtlı cihazlar için geçerlidir. Bu ayar, karma Azure AD 'ye katılmış cihazlar için geçerlidir.

- **En fazla cihaz sayısı** -Bu ayar, bir kullanıcının Azure AD 'de sahip olduğu en fazla Azure AD 'ye katılmış veya Azure AD kayıtlı cihaz sayısını seçmenize olanak sağlar. Bir Kullanıcı bu kotaya ulaşırsa, mevcut cihazların bir veya daha fazlası kaldırılana kadar ek cihaz ekleyemeyecektir. Varsayılan değer **50**' dir.

> [!NOTE]
> Azure AD 'ye katılmış ya da Azure AD 'ye kayıtlı cihazlar için **en fazla cihaz sayısı** ayarı geçerlidir. Bu ayar, karma Azure AD 'ye katılmış cihazlar için geçerlidir.

- [Enterprise State Roaming](enterprise-state-roaming-overview.md)

## <a name="audit-logs"></a>Denetim günlükleri

Cihaz etkinlikleri, etkinlik günlükleri aracılığıyla kullanılabilir. Bu Günlükler cihaz kayıt hizmeti tarafından tetiklenen etkinlikleri ve kullanıcılar tarafından şunları içerir:

- Cihaz oluşturma ve cihazda sahipler/kullanıcılar ekleme
- Cihaz ayarlarında yapılan değişiklikler
- Bir cihazı silme veya güncelleştirme gibi cihaz işlemleri

Denetim verilerine giriş noktanız, **cihazlar** sayfasının **etkinlik** bölümündeki **Denetim günlükleri** ' dir.

Denetim günlüğünde, şunu gösteren bir varsayılan liste görünümü vardır:

- Olayın tarih ve saati
- Hedefler
- Etkinliğin başlatıcısı/aktör (kim)
- Etkinlik (ne)

![Denetim günlükleri](./media/device-management-azure-portal/63.png)

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Denetim günlükleri](./media/device-management-azure-portal/64.png)

Raporlanan verileri istediğiniz düzeye gelecek şekilde daraltmak için, aşağıdaki alanları kullanarak denetim verilerini filtreleyebilirsiniz:

- Kategori
- Etkinlik kaynak türü
- Etkinlik
- Tarih aralığı
- Hedef
- Başlatan (aktör)

Filtrelerin yanı sıra belirli girdileri de arayabilirsiniz.

![Denetim günlükleri](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD 'de eski cihazları yönetme](manage-stale-devices.md)

[Enterprise State Roaming](enterprise-state-roaming-overview.md)