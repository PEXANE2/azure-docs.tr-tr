---
title: Azure portal kullanarak cihazları yönetme | Microsoft Docs
description: Azure portal kullanarak cihazları yönetme hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 241e5baf7f00bc9897e84fb0c10ac7db605fb712
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84628323"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Azure portal kullanarak cihaz kimliklerini yönetme

Azure Active Directory (Azure AD) ' de cihaz kimlik yönetimi ile kullanıcılarınızın, güvenlik ve uyumluluk standartlarınıza uygun cihazlardan kaynaklarınıza erişmesini sağlayabilirsiniz.

Bu makalede:

- [Azure Active Directory içinde cihaz kimliği yönetimine giriş](overview.md) hakkında bilgi sahibi olduğunuzu varsayar.
- Azure AD portalını kullanarak cihaz kimliklerinizi yönetme hakkında bilgi sağlar

![Azure portal tüm cihazlar görünümü](./media/device-management-azure-portal/all-devices-azure-portal.png)

## <a name="manage-device-identities"></a>Cihaz kimliklerini yönetme

Azure AD portalı, cihaz kimliklerinizi yönetmek için size merkezi bir yer sağlar. Bu yere [doğrudan bir bağlantı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) kullanarak ulaşabilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Azure Active Directory**  >  **cihazlara**gidin.

**Cihazlar** sayfası şunları yapmanızı sağlar:

- Cihaz ayarlarınızı yapılandırın
- Cihaz bulma
- Cihaz kimliği yönetim görevlerini gerçekleştirme
- Cihazla ilgili denetim günlüklerini gözden geçirme  
  
## <a name="configure-device-settings"></a>Cihaz ayarlarını yapılandırma

Azure AD portalını kullanarak cihaz kimliklerinizi yönetmek için, cihazlarınızın [kayıtlı veya](overview.md) Azure AD 'ye katılmış olması gerekir. Yönetici olarak, cihaz ayarlarını yapılandırarak cihazları kaydetme ve birleştirme sürecinde ince ayar yapabilirsiniz.

Cihaz ayarları sayfası, cihaz kimlikleriyle ilgili ayarları yapılandırmanıza olanak sağlar:

![Azure AD ile ilgili cihaz ayarları](./media/device-management-azure-portal/device-settings-azure-portal.png)

- **Kullanıcılar cihazları Azure AD 'ye katabilir** -Bu ayar, CIHAZLARıNı Azure AD 'ye katılmış cihazlar olarak kaydedebilen kullanıcıları seçmenize olanak sağlar. Varsayılan değer **Tümü**' dir.

> [!NOTE]
> **Kullanıcılar cihazları Azure AD 'ye katabilir** ve yalnızca Windows 10 ' da Azure AD katılımı için geçerlidir.

- **Azure AD 'ye katılmış cihazlarda ek yerel Yöneticiler** -bir cihazda yerel yönetici hakları verilen kullanıcıları seçebilirsiniz. Buraya eklenen kullanıcılar, Azure AD 'de *cihaz yöneticileri* rolüne eklenir. Azure AD ve cihaz sahiplerine Genel Yöneticiler varsayılan olarak yerel yönetici haklarına sahiptir. Bu seçenek, Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünlerle kullanılabilen bir Premium sürüm özelliğidir.
- **Kullanıcılar cihazlarını Azure AD 'ye kaydedebileceği** için bu ayarı Windows 10 Personal, IOS, Android ve MacOS CIHAZLARıNıN Azure AD 'ye kaydolmalarına izin verecek şekilde yapılandırmanız gerekir. **Hiçbiri**' ni seçerseniz, CIHAZLARıN Azure AD 'ye kaydolmaya izin verilmez. Office 365 için Microsoft Intune veya mobil cihaz yönetimi (MDM) kaydı için kayıt gereklidir. Bu hizmetlerden birini yapılandırdıysanız, **Tümü** seçilidir ve **hiçbiri** kullanılamaz.
- **Cihazlara katılması Için Multi-Factor auth gerektir** -kullanıcıların CIHAZLARıNı Azure AD 'ye katmak için ek bir kimlik doğrulama faktörü sağlaması gerekip gerekmediğini seçebilirsiniz. Varsayılan değer **Hayır**' dır. Bir cihazı kaydederken çok faktörlü kimlik doğrulamasının gerekli olmasını öneririz. Bu hizmet için Multi-Factor Authentication 'ı etkinleştirmeden önce, cihazlarını kaydeden kullanıcılar için Multi-Factor Authentication ' ın yapılandırıldığından emin olmanız gerekir. Farklı Azure Multi-Factor Authentication hizmetleri hakkında daha fazla bilgi için bkz. [Azure Multi-Factor Authentication](../authentication/concept-mfa-whichversion.md)kullanmaya başlama. 

> [!NOTE]
> **Cihazların katılmasını sağlamak Için Multi-Factor auth iste** ayarı, Azure AD 'ye katılmış veya Azure AD 'ye kayıtlı cihazlar için geçerlidir. Bu ayar, karma Azure AD 'ye katılmış cihazlar için geçerlidir.

- **En fazla cihaz sayısı** -Bu ayar, bir kullanıcının Azure AD 'de sahip olduğu en fazla Azure AD 'ye katılmış veya Azure AD kayıtlı cihaz sayısını seçmenize olanak sağlar. Bir Kullanıcı bu kotaya ulaşırsa, mevcut cihazların bir veya daha fazlası kaldırılana kadar ek cihaz ekleyemeyecektir. Varsayılan değer **20**' dir.

> [!NOTE]
> Azure AD 'ye katılmış ya da Azure AD 'ye kayıtlı cihazlar için **en fazla cihaz sayısı** ayarı geçerlidir. Bu ayar, karma Azure AD 'ye katılmış cihazlar için geçerlidir.

- **Kullanıcılar ayarları ve uygulama verilerini cihazlarda eşitleyebilir** -varsayılan olarak, bu ayar **none**olarak ayarlanır. Belirli kullanıcılar veya gruplar veya tümü seçildiğinde kullanıcının ayarlarının ve uygulama verilerinin Windows 10 cihazlarında eşitlenmesi izin verir. Windows 10 ' da eşitlemenin nasıl çalıştığı hakkında daha fazla bilgi edinin.
Bu seçenek, Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünlerle kullanılabilen bir Premium yetenektir.

## <a name="locate-devices"></a>Cihaz bulma

Kayıtlı ve birleştirilmiş cihazları bulmak için iki seçeneğiniz vardır:

- **Cihazlar** sayfasının **Yönet** bölümündeki **tüm cihazlar**  
- Bir **Kullanıcı** sayfasının **Yönet** bölümündeki **cihazlar**

Her iki seçenekten de şunları yapabilirsiniz:

- Ekran adını veya cihaz KIMLIĞINI filtre olarak kullanarak cihazları aramanızı sağlar.
- Kayıtlı ve birleştirilmiş cihazlara ayrıntılı bir genel bakış sağlar
- Ortak cihaz yönetim görevleri gerçekleştirmenizi sağlar

>[!TIP]
>
>* KAYıTLı sütun altında "bekliyor" durumuna sahip "hibrit Azure AD 'ye katılmış" bir cihaz görürseniz, cihazın Azure AD Connect 'ten eşitlendiğini ve istemciden kayıt tamamlamayı beklediğini gösterir. [Hibrit Azure AD JOIN uygulamanızın nasıl planlanacağı](hybrid-azuread-join-plan.md)hakkında daha fazla bilgi edinin. Daha fazla bilgi için, [cihazlar sık sorulan sorular](faq.md)makalesinde bulunabilir.
>
>* Bazı iOS cihazlarında, kesme işaretleri içeren cihaz adları, kesme işareti gibi farklı karakterler kullanabilir. Bu nedenle, bu tür cihazları aramak biraz karmaşık değildir. arama sonuçlarını doğru şekilde görmüyorsanız, arama dizesinin eşleşen kesme işareti karakteri içerdiğinden emin olun.

## <a name="device-identity-management-tasks"></a>Cihaz kimliği yönetim görevleri

Genel yönetici veya bulut cihaz yöneticisi olarak, kayıtlı veya katılmış cihazları yönetebilirsiniz. Intune hizmet yöneticileri şunları yapabilir:

- Cihazları güncelleştirme-örnekleri cihazları etkinleştirme/devre dışı bırakma gibi günlük işlemlerdir
- Cihazları Sil-bir cihaz kullanımdan kaldırıldığında ve Azure AD 'de silinmeli

Bu bölüm, ortak cihaz kimlik yönetimi görevleri hakkında bilgi sağlar.

### <a name="manage-an-intune-device"></a>Intune cihazını yönetme

Bir Intune yöneticisiyseniz, **Microsoft Intune**olarak işaretlenen cihazları yönetebilirsiniz. Cihaz Microsoft Intune kayıtlı değilse, "Yönet" seçeneği gri kalır.

![Intune cihazını yönetme](./media/device-management-azure-portal/31.png)

### <a name="enable--disable-an-azure-ad-device"></a>Azure AD cihazını etkinleştirme/devre dışı bırakma

Bir cihazı etkinleştirmek/devre dışı bırakmak için iki seçeneğiniz vardır:

- **Tüm cihazlar** sayfasında görevler menüsü ("...")

   ![Intune cihazını yönetme](./media/device-management-azure-portal/71.png)

- **Cihazlar** sayfasındaki araç çubuğu

   ![Intune cihazını yönetme](./media/device-management-azure-portal/32.png)

**Açıklamalarının**

- Bir cihazı etkinleştirmek/devre dışı bırakmak için Azure AD 'de Genel yönetici veya bulut cihaz yöneticisi olmanız gerekir. 
- Bir cihazın devre dışı bırakılması, cihazın Azure AD ile başarılı bir şekilde kimlik doğrulamasına engel olur, böylece cihazın cihaz CA 'sı tarafından korunan Azure AD kaynaklarınıza veya WH4B kimlik bilgilerinizi kullanarak erişmesini önler.
- Cihazın devre dışı bırakılması hem birincil yenileme belirtecini (PRT) hem de cihazdaki yenileme belirteçlerini (RT) iptal eder.

### <a name="delete-an-azure-ad-device"></a>Azure AD cihazını silme

Bir cihazı silmek için iki seçeneğiniz vardır:

- **Tüm cihazlar** sayfasında görevler menüsü ("...")

   ![Intune cihazını yönetme](./media/device-management-azure-portal/72.png)

- **Cihazlar** sayfasındaki araç çubuğu

   ![Cihazı silme](./media/device-management-azure-portal/34.png)

**Açıklamalarının**

- Bir cihazı silmek için Azure AD 'de Genel yönetici veya Intune yöneticisi olmanız gerekir.
- Bir cihazı silme:
   - Bir cihazın Azure AD kaynaklarınıza erişmesini önler.
   - Cihaza iliştirilmiş tüm ayrıntıları kaldırır, örneğin, Windows cihazları için BitLocker anahtarları.  
   - Kurtarılabilir olmayan bir etkinliği temsil eder ve gerekli olmadığı takdirde önerilmez.

Bir cihaz başka bir yönetim yetkilisi tarafından yönetiliyorsa (örneğin, Microsoft Intune), cihazın Azure AD 'de silinmeden önce temizlendiğinden/devre dışı olduğundan emin olun. Herhangi bir cihazı silmeden önce [eski cihazların nasıl yönetileceğini](manage-stale-devices.md) inceleyin.

### <a name="view-or-copy-device-id"></a>Cihaz KIMLIĞINI görüntüle veya Kopyala

Cihazdaki cihaz KIMLIĞI ayrıntılarını doğrulamak veya sorun giderme sırasında PowerShell 'i kullanmak için bir cihaz KIMLIĞI kullanabilirsiniz. Kopyala seçeneğine erişmek için cihaza tıklayın.

![Cihaz KIMLIĞINI görüntüleme](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>BitLocker anahtarlarını görüntüleme veya kopyalama

Kullanıcıların şifreli sürücüsünü kurtarmalarına yardımcı olmak için BitLocker anahtarlarını görüntüleyebilir ve kopyalayabilirsiniz. Bu anahtarlar yalnızca şifrelenmiş ve anahtarları Azure AD 'de depolanan Windows cihazlarında kullanılabilir. Cihazın ayrıntılarına erişirken bu anahtarları kopyalayabilirsiniz.

![BitLocker anahtarlarını görüntüle](./media/device-management-azure-portal/36.png)

BitLocker anahtarlarını görüntülemek veya kopyalamak için, cihazın sahibi ya da aşağıdaki rollerden en az birine sahip olan bir kullanıcı olmanız gerekir:

- Bulut Cihaz Yöneticisi
- Genel Yönetici
- Yardım Masası Yöneticisi
- Intune Hizmet Yöneticisi
- Güvenlik Yöneticisi
- Güvenlik okuyucusu

> [!NOTE]
> Karma Azure AD 'ye katılmış Windows 10 cihazlarının sahibi yok. Bu nedenle, bir cihazı sahibine göre arıyorsanız ve bulamıyorsanız, cihaz KIMLIĞINE göre arama yapın.

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
