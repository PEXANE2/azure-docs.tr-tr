---
title: PIM'de Azure kaynak rol ayarlarını yapılandırma - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rol ayarlarını nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 01/01/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4db330a875b8241b642bcbc71fb0866c9833ee7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75638689"
---
# <a name="configure-azure-resource-role-settings-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rol ayarlarını yapılandırma

Azure kaynak rol ayarlarını yapılandırdığınızda, Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rol atamalarına uygulanan varsayılan ayarları tanımlarsınız. Onay iş akışını yapılandırmak ve istekleri kimin onaylayıp reddedebileceğini belirtmek için aşağıdaki yordamları kullanın.

## <a name="open-role-settings"></a>Rol ayarlarını açma

Azure kaynak rolü ayarlarını açmak için aşağıdaki adımları izleyin.

1. [Ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolündeki bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure kaynaklarını**seçin.

1. Abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağı seçin.

    ![Yönetilebilen azure kaynakları sayfası liste kaynakları](./media/pim-resource-roles-configure-role-settings/resources-list.png)

1. **Rol ayarlarını**seçin.

    ![Rol ayarları sayfası Azure kaynak rollerini listele](./media/pim-resource-roles-configure-role-settings/resources-role-settings.png)

1. Ayarlarını yapılandırmak istediğiniz rolü seçin.

    ![Rol belirleme ayrıntıları sayfası birkaç atama ve etkinleştirme ayarını listele](./media/pim-resource-roles-configure-role-settings/resources-role-setting-details.png)

1. **Rol ayarları** bölmesini açmak için **Edit'i** seçin. İlk sekme, Ayrıcalıklı Kimlik Yönetimi'nde rol etkinleştirme yapılandırmasını güncelleştirmenize olanak tanır.

    ![Etkinleştirme sekmesi açıkken rol ayarlarını sayfayı değiştir](./media/pim-resource-roles-configure-role-settings/role-settings-activation-tab.png)

1. **Atama** sekmesini veya **Sonraki:** Atama ayar sekmesini açmak için sayfanın altındaki Atama düğmesini seçin. Bu ayarlar, Ayrıcalıklı Kimlik Yönetimi arabirimi içinde yapılan rol atamalarını denetler.

    ![Rol ayarları sayfasında Rol Atama sekmesi](./media/pim-resource-roles-configure-role-settings/role-settings-assignment-tab.png)

1. Bu rol için bildirim ayarı sekmesine ulaşmak için sayfanın altındaki **Bildirim** sekmesini veya **Sonraki: Etkinleştirme** düğmesini kullanın. Bu ayarlar, bu rolle ilgili tüm e-posta bildirimlerini denetler.

    ![Rol ayarları sayfasında rol bildirimleri sekmesi](./media/pim-resource-roles-configure-role-settings/role-settings-notification-tab.png)

    Rol ayarları sayfasındaki **Bildirimler** sekmesinde, Ayrıcalıklı Kimlik Yönetimi, bildirimleri kimin aldığı ve hangi bildirimleri aldıkları üzerinde ayrıntılı denetim sağlar.

    - **E-postayı kapatma**<br>Varsayılan alıcı onay kutusunu temizleyerek ve ek alıcıları silerek belirli e-postaları kapatabilirsiniz.  

    - **E-postaları belirtilen e-posta adresleriyle sınırlandırın**<br>Varsayılan alıcı onay kutusunu temizleyerek varsayılan alıcılara gönderilen e-postaları kapatabilirsiniz. Daha sonra ek alıcılar olarak ek e-posta adresleri ekleyebilirsiniz. Birden fazla e-posta adresi eklemek istiyorsanız, bunları bir yarı nokta nokta (;) kullanarak ayırın.

    - **Hem varsayılan alıcılara hem de ek alıcılara e-posta gönderme**<br>Varsayılan alıcı onay kutusunu seçerek ve ek alıcılar için e-posta adresleri ekleyerek hem varsayılan alıcıya hem de ek alıcıya e-posta gönderebilirsiniz.

    - **Yalnızca kritik e-postalar**<br>Her e-posta türü için yalnızca kritik e-postaları almak için onay kutusunu seçebilirsiniz. Bunun anlamı, Ayrıcalıklı Kimlik Yönetimi'nin yapılandırılan alıcılara yalnızca e-posta nın hemen bir eylem gerektirdiğinde e-posta göndermeye devam edeceğidir. Örneğin, kullanıcılardan rol atamalarını genişletmelerini isteyen e-postalar tetiklenmezken, yöneticilerin bir uzantı isteğini onaylamasını gerektiren bir e-postalar tetiklenir.

1. Rol ayarlarını güncellemek için istediğiniz zaman **Güncelleştir** düğmesini seçin.

## <a name="assignment-duration"></a>Atama süresi

Bir rol için ayarları yapılandırırken her atama türü (uygun ve etkin) için iki atama süresi seçeneği arasından seçim yapabilirsiniz. Bu seçenekler, bir kullanıcı Ayrıcalıklı Kimlik Yönetimi'ndeki role atandığında varsayılan maksimum süre olur.

Bu **uygun** atama süresi seçeneklerinden birini seçebilirsiniz:

| | |
| --- | --- |
| **Kalıcı uygun atamaya izin verme** | Kaynak yöneticileri kalıcı uygun atama atayabilir. |
| **Uygun atamadan sonra süresi doldu** | Kaynak yöneticileri, tüm uygun atamaların belirli bir başlangıç ve bitiş tarihine sahip olduğunu isteyebilir. |

Ayrıca, şu **etkin** atama süresi seçeneklerinden birini seçebilirsiniz:

| | |
| --- | --- |
| **Kalıcı etkin atamaya izin verme** | Kaynak yöneticileri kalıcı etkin atama atayabilir. |
| **Önce etkin atamanın süresi doldu** | Kaynak yöneticileri, tüm etkin atamaların belirli bir başlangıç ve bitiş tarihine sahip olduğunu gerektirebilir. |

> [!NOTE]
> Belirli bir bitiş tarihi olan tüm atamalar kaynak yöneticileri tarafından yenilenebilir. Ayrıca, kullanıcılar [rol atamalarını genişletmek veya yenilemek](pim-resource-roles-renew-extend.md)için self servis isteklerini başlatabilir.

## <a name="require-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulama gerektirir

Ayrıcalıklı Kimlik Yönetimi, iki farklı senaryo için Azure Çok Faktörlü Kimlik Doğrulama'nın isteğe bağlı olarak uygulanmasını sağlar.

### <a name="require-multi-factor-authentication-on-active-assignment"></a>Etkin atamada Çok Faktörlü Kimlik Doğrulama Gerektirir

Bazı durumlarda, bir kullanıcıyı veya grubu kısa bir süre için bir role atamak isteyebilirsiniz (örneğin, bir gün). Bu durumda, atanan kullanıcıların etkinleştirme istemesi gerekmez. Bu senaryoda, Ayrıcalıklı Kimlik Yönetimi, kullanıcı rol atamasını kullandığında çok faktörlü kimlik doğrulamasını zorlayamamaktadır, çünkü bu, atandığı andan itibaren rolde zaten etkindir.

Atamayı gerçekleştiren kaynak yöneticisinin söyledikleri kişi olduğundan emin olmak için, etkin atama kutusunda **Çok Faktörlü Kimlik Doğrulamayı** Gerek'ini denetleyerek etkin atamada çok faktörlü kimlik doğrulaması uygulayabilirsiniz.

### <a name="require-multi-factor-authentication-on-activation"></a>Etkinleştirmede Çok Faktörlü Kimlik Doğrulama Gerektirir

Bir rol için uygun olan kullanıcıların etkinleştirilmeden önce Azure Çok Faktörlü Kimlik Doğrulamasını kimler kullandığını kanıtlamalarını isteyebilirsiniz. Çok faktörlü kimlik doğrulama, kullanıcının makul bir kesinlikle söyledikleri kişi olmasını sağlar. Bu seçeneğin uygulanması, kullanıcı hesabının tehlikeye girdiği durumlarda kritik kaynakları korur.

Etkinleştirmeden önce çok faktörlü kimlik doğrulaması gerektirmek için etkinleştirme kutusunda **Çok Faktörlü Kimlik Doğrulamayı İste'yi** işaretleyin.

Daha fazla bilgi için [çok faktörlü kimlik doğrulama ve Ayrıcalıklı Kimlik Yönetimi'ne](pim-how-to-require-mfa.md)bakın.

## <a name="activation-maximum-duration"></a>Etkinleştirme maksimum süresi

Bir rolün süresi dolmadan önce etkin kalmasını sağlayan maksimum süreyi saat olarak ayarlamak için **Etkinleştirme maksimum süre** kaydırıcısını kullanın. Bu değer 1 ila 24 saat arasında olabilir.

## <a name="require-justification"></a>Yaslama gerektirme

Etkinleştirildiğinde kullanıcıların bir iş gerekçesi girmesini gerektirebilirsiniz. Yaslama gerektirmek için, etkin atama kutusundaki **Yaslama Yığmayı** veya etkinleştirme kutusundaki **YaslamaYı İste'yi** işaretleyin.

## <a name="require-approval-to-activate"></a>Etkinleştirmek için onay gerektirme

Bir rolü etkinleştirmek için onay almak istiyorsanız, aşağıdaki adımları izleyin.

1. Onay kutusunu **etkinleştirmek için onay iste'yi** işaretleyin.

1. **Üye veya grup** sayfasını açmak için **onaylayıcıları seçin'** i seçin.

    ![Onaylayıcıları seçmek için bir kullanıcı veya grup bölmesi seçin](./media/pim-resource-roles-configure-role-settings/resources-role-settings-select-approvers.png)

1. En az bir kullanıcı veya grup seçin ve sonra **Seç'i**tıklatın. Herhangi bir kullanıcı ve grup birleşimi ekleyebilirsiniz. En az bir onaylayıcı seçmeniz gerekir. Varsayılan onaylayıcı yok.

    Seçimleriniz seçili onaylayanlar listesinde görünür.

1. Tüm rol ayarlarınızı belirttikten sonra değişikliklerinizi kaydetmek için **Güncelleştir'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini atama](pim-resource-roles-assign-roles.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için güvenlik uyarılarını yapılandırma](pim-resource-roles-configure-alerts.md)
