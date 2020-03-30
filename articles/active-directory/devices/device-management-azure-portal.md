---
title: Azure portalını kullanarak cihazları yönetme | Microsoft Dokümanlar
description: Cihazları yönetmek için Azure portalını nasıl kullanacağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: e09de5911ca0946bfcbcb77d1ad4131c8feac9f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262249"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Azure portalını kullanarak aygıt kimliklerini yönetme

Azure Active Directory 'deki (Azure AD) aygıt kimlik yönetimi yle, kullanıcılarınızın kaynaklarınıza güvenlik ve uyumluluk standartlarınızı karşılayan aygıtlardan eriştirdiğinden emin olabilirsiniz.

Bu makalede:

- [Azure Active Directory'de aygıt kimlik yönetimine girişe](overview.md) aşina olduğunuzu varsayar
- Azure AD portalını kullanarak cihazınızın kimliklerini yönetme hakkında bilgi sağlar

## <a name="manage-device-identities"></a>Cihaz kimliklerini yönetme

Azure AD portalı, aygıt kimliklerinizi yönetmek için merkezi bir yer sağlar. Doğrudan bir [bağlantı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) kullanarak buraya ulaşabilirsiniz veya:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Azure Etkin Dizin** > **Aygıtlarına**göz atın.

**Cihazlar** sayfası şunları yapmanızı sağlar:

- Cihaz ayarlarınızı yapılandırın
- Cihaz bulma
- Aygıt kimlik yönetimi görevlerini gerçekleştirme
- Aygıtla ilgili denetim günlüklerini gözden geçirme  
  
## <a name="configure-device-settings"></a>Cihaz ayarlarını yapılandırma

Azure AD portalını kullanarak aygıt kimliklerinizi yönetmek için cihazlarınızın [kaydolması veya](overview.md) Azure AD'ye katılması gerekir. Yönetici olarak, aygıt ayarlarını yapılandırarak aygıtları kaydetme ve birleştirme işleminde ince ayar yapabilirsiniz.

Aygıt ayarları sayfası, aygıt kimlikleri ile ilgili ayarları yapılandırmanızı sağlar:

![Bir Intune cihazını yönetme](./media/device-management-azure-portal/21.png)

- **Kullanıcılar aygıtları Azure AD'ye katılabilir** - Bu ayar, aygıtlarını Azure AD'ye katılan aygıtlar olarak kaydedebilirsiniz kullanıcıları seçmenize olanak tanır. Varsayılan **tüm.**

> [!NOTE]
> **Kullanıcılar aygıtları Azure REKLAM ayarına katılabilir,** yalnızca Windows 10'daki Azure AD join için geçerlidir.

- **Azure AD'ye katılan cihazlardaki ek yerel yöneticiler** - Bir aygıtta yerel yönetici hakları verilen kullanıcıları seçebilirsiniz. Buraya eklenen kullanıcılar Azure AD'deki *Aygıt Yöneticileri* rolüne eklenir. Azure AD'deki genel yöneticilere ve aygıt sahiplerine varsayılan olarak yerel yönetici hakları verilir. Bu seçenek, Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilen bir premium sürüm özelliğidir.
- **Kullanıcılar cihazlarını Azure AD'ye kaydedebilir** - Bu ayarı Windows 10 kişisel, iOS, Android ve macOs cihazlarının Azure AD'ye kaydolmasına izin verecek şekilde yapılandırmanız gerekir. **Yok'u**seçerseniz, aygıtların Azure AD'ye kaydolmasına izin verilmez. Office 365 için Microsoft Intune veya Mobil Aygıt Yönetimi (MDM) kaydı için kayıt gerektirir. Bu hizmetlerden birini yapılandırmışsanız, **TÜMÜ** seçilir ve **YOK** kullanılamaz.
- **Aygıtlara katılmak için Çok Faktörlü Auth'un olmasını zorunlu kullanabilirsiniz** - Kullanıcıların aygıtlarını Azure AD'ye katılmak için ek bir kimlik doğrulama faktörü sağlamaları gerekip gerekmediğini seçebilirsiniz. Varsayılan değer **No**Hayır'dır. Bir aygıtı kaydederken çok faktörlü kimlik doğrulaması gerektirmenizi öneririz. Bu hizmet için çok faktörlü kimlik doğrulamasını etkinleştirmeden önce, aygıtlarını kaydeden kullanıcılar için çok faktörlü kimlik doğrulamanın yapılandırıldığından emin olmanız gerekir. Farklı Azure çok faktörlü kimlik doğrulama hizmetleri hakkında daha fazla bilgi için Azure [çok faktörlü kimlik doğrulama ile başlamaya](../authentication/concept-mfa-whichversion.md)bakın. 

> [!NOTE]
> Azure AD'ye katılan veya Azure AD'ye kayıtlı aygıtlara **katılmak için Çok Faktörlü Auth'un** aygıtlara katılmasını zorunlu kılmasını zorunlu kınla. Bu ayar, karma Azure AD birleştirilmiş aygıtlar için geçerli değildir.

- **Maksimum aygıt sayısı** - Bu ayar, bir kullanıcının Azure AD'de sahip olabileceği maksimum Azure AD veya Azure AD kayıtlı aygıt sayısını seçmenize olanak tanır. Bir kullanıcı bu kotaya ulaşırsa, varolan aygıtlardan biri veya birkaçı kaldırılana kadar ek aygıt lar ekemez. Varsayılan değer **20'dir.**

> [!NOTE]
> Azure AD'ye katılan veya Azure AD'ye kayıtlı aygıtlar için **maksimum aygıt ayarı geçerlidir.** Bu ayar, karma Azure AD birleştirilmiş aygıtlar için geçerli değildir.

- **Kullanıcılar ayarları ve uygulama verilerini cihazlar arasında eşitleyebilir** - Varsayılan olarak, bu ayar **NONE**olarak ayarlanır. Belirli kullanıcıları veya grupları veya ALL'ı seçmek, kullanıcının ayarlarının ve uygulama verilerinin Windows 10 aygıtları arasında eşitlemesine olanak tanır. Windows 10'da eşitlemenin nasıl çalıştığı hakkında daha fazla bilgi edinin.
Bu seçenek, Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilen bir premium özelliktir.

## <a name="locate-devices"></a>Cihaz bulma

Kayıtlı ve birleştirilmiş aygıtları bulmak için iki seçeneğiniz vardır:

- **Cihazları** **Yönet** sayfasındaki **tüm cihazlar**  
- **Kullanıcı** sayfasının **Yönet** bölümündeki **aygıtlar**

Her iki seçenekle de, şu görünüme ulaşabilirsiniz:

- Görüntü adını veya aygıt kimliğini filtre olarak kullanarak aygıtları aramanızı sağlar.
- Kayıtlı ve birleştirilmiş aygıtların ayrıntılı genel görünümünü sağlar
- Ortak aygıt yönetimi görevlerini gerçekleştirmenizi sağlar

![Tüm cihazlar](./media/device-management-azure-portal/51.png)

>[!TIP]
>
>* KAYITLI sütunun altında "Bekleyen" durumu olan "Karma Azure AD'si" olan bir aygıt görürseniz, aygıtın Azure AD bağlantısından senkronize edildiğini ve istemciden kaydı tamamlamak için beklediğini gösterir. [Karma Azure AD'nizin uygulanmasını nasıl planlayabildiğini](hybrid-azuread-join-plan.md)daha fazla okuyun. Ek bilgiler makalede bulunabilir, [Cihazlar sık sorulan sorular](faq.md).
>
>   ![Bekleyen cihazlar](./media/device-management-azure-portal/75.png)
>
>* Bazı iOS aygıtlarında, keskifler içeren aygıt adları, apostrophes'e benzeyen farklı karakterler kullanabilir. Bu nedenle, bu tür aygıtları aramak biraz zor - arama sonuçlarını doğru görmüyorsanız, arama dizesinin eşleşen apostrophe karakteri içerdiğinden emin olun.

## <a name="device-identity-management-tasks"></a>Cihaz kimlik yönetimi görevleri

Genel yönetici veya bulut aygıt yöneticisi olarak, kayıtlı veya birleştirilmiş aygıtları yönetebilirsiniz. Intune Hizmeti yöneticileri şunları yapabilir:

- Cihazları güncelleme - Aygıtları etkinleştirme/devre dışı bırakma gibi günlük işlemlere örnekler verilebilir
- Aygıtları silme – Bir aygıt kullanımdan kaldırıldığında ve Azure AD'de silinmesi gerektiğinde

Bu bölümde, ortak aygıt kimlik yönetimi görevleri hakkında bilgi sağlar.

### <a name="manage-an-intune-device"></a>Bir Intune cihazını yönetme

Bir Intune yöneticisiyseniz, **Microsoft Intune**olarak işaretlenmiş aygıtları yönetebilirsiniz. Aygıt Microsoft Intune'a kayıtlı değilse "Yönet" seçeneği soluk hale gelir.

![Bir Intune cihazını yönetme](./media/device-management-azure-portal/31.png)

### <a name="enable--disable-an-azure-ad-device"></a>Azure AD aygıtını etkinleştirme / devre dışı

Bir aygıtı etkinleştirmek/ devre dışı kalmak için iki seçeneğiniz vardır:

- **Tüm cihazlar** sayfasındaki görevler menüsü ("...")

   ![Bir Intune cihazını yönetme](./media/device-management-azure-portal/71.png)

- **Cihazlar** sayfasındaki araç çubuğu

   ![Bir Intune cihazını yönetme](./media/device-management-azure-portal/32.png)

**Açıklamalar:**

- Bir aygıtı etkinleştirmek / devre dışı kalmak için Azure AD'de genel bir yönetici veya bulut aygıtı yöneticisi olmanız gerekir. 
- Aygıtı devre dışı bırakmak, aygıtın Azure AD ile başarılı bir şekilde kimlik doğrulamasını önler ve bu nedenle aygıtın Aygıt CA tarafından korunan Azure AD kaynaklarınıza erişmesini veya WH4B kimlik bilgilerinizi kullanmasını engeller.
- Aygıtı devre dışı bırakmak, hem Birincil Yenileme Belirteci'ni (PRT) hem de cihazdaki tüm Yenileme Belirteçleri'ni (RT) iptal eder.

### <a name="delete-an-azure-ad-device"></a>Azure AD aygıtını silme

Bir aygıtı silmek için iki seçeneğiniz vardır:

- **Tüm cihazlar** sayfasındaki görevler menüsü ("...")

   ![Bir Intune cihazını yönetme](./media/device-management-azure-portal/72.png)

- **Cihazlar** sayfasındaki araç çubuğu

   ![Cihazı silme](./media/device-management-azure-portal/34.png)

**Açıklamalar:**

- Bir aygıtı silmek için Azure AD'de genel yönetici veya Intune yöneticisi olmanız gerekir.
- Aygıtı silme:
   - Aygıtın Azure REKLAM kaynaklarınıza erişmesini engeller.
   - Aygıta iliştirilen tüm ayrıntıları (örneğin, Windows aygıtları için BitLocker tuşları) kaldırır.  
   - Kurtarılamayan bir etkinliği temsil eder ve gerekli olmadıkça önerilmez.

Bir aygıt başka bir yönetim yetkilisi (örneğin, Microsoft Intune) tarafından yönetiliyorsa, aygıtın Azure AD'de silmeden önce silindiğinden /kullanımdan kaldırıldıksa emin olun. Aygıtları silmeden önce [eski aygıtların](device-management-azure-portal.md) nasıl yönetilcesini gözden geçirin.

### <a name="view-or-copy-device-id"></a>Aygıt kimliğini görüntüleme veya kopyalama

Aygıttaki aygıt kimliği ayrıntılarını doğrulamak veya sorun giderme sırasında PowerShell'i kullanmak için aygıt kimliği kullanabilirsiniz. Kopyalama seçeneğine erişmek için aygıtı tıklatın.

![Aygıt kimliğini görüntüleme](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>BitLocker tuşlarını görüntüleme veya kopyalama

Kullanıcıların şifreli sürücülerini kurtarmalarına yardımcı olmak için BitLocker anahtarlarını görüntüleyebilir ve kopyalayabilirsiniz. Bu anahtarlar yalnızca şifrelenmiş ve anahtarlarını Azure AD'de depolayan Windows aygıtları için kullanılabilir. Aygıtın ayrıntılarına erişirken bu anahtarları kopyalayabilirsiniz.

![BitLocker tuşlarını görüntüleyin](./media/device-management-azure-portal/36.png)

BitLocker anahtarlarını görüntülemek veya kopyalamak için aygıtın sahibi veya aşağıdaki rollerden en az birine atanmış bir kullanıcı olmanız gerekir:

- Bulut Aygıt Yöneticisi
- Genel Yönetici
- Yardım Masası Yöneticisi
- Intune Hizmet Yöneticisi
- Güvenlik Yöneticisi
- Güvenlik Okuyucu

> [!NOTE]
> Karma Azure AD Joined Windows 10 cihazlarının bir sahibi yoktur. Bu nedenle, sahibitarafından bir aygıt arıyorsanız ve bulamamışsanız, aygıt kimliğine göre arama yapın.

## <a name="audit-logs"></a>Denetim günlükleri

Cihaz etkinlikleri etkinlik günlükleri aracılığıyla kullanılabilir. Bu günlükler, aygıt kayıt hizmeti ve kullanıcılar tarafından tetiklenen etkinlikleri içerir:

- Cihaz oluşturma ve aygıta sahipler / kullanıcılar ekleme
- Aygıt ayarlarında yapılan değişiklikler
- Aygıtı silme veya güncelleştirme gibi aygıt işlemleri

Denetim verilerine giriş **noktanız, Cihazlar** sayfasının **Etkinlik** bölümündeki **Denetim günlükleridir.**

Denetim günlüğü, aşağıdakileri gösteren bir varsayılan liste görünümüne sahiptir:

- Olayın tarih ve saati
- Hedefler
- Bir etkinliğin başlatıcısı / aktörü (kim)
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
- Başlatan (Aktör)

Filtrelere ek olarak, belirli girişleri de arayabilirsiniz.

![Denetim günlükleri](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD'de eski aygıtları yönetme](manage-stale-devices.md)
