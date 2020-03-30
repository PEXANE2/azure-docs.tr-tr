---
title: PIM'de Azure AD rol ayarlarını yapılandırma - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure AD rol ayarlarını nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/28/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dd525973a8c8ba5bb3103126b4ddb5b6973590a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78205033"
---
# <a name="configure-azure-ad-role-settings-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rol ayarlarını yapılandırma

Ayrıcalıklı bir rol yöneticisi, uygun bir rol atamasını etkinleştiren bir kullanıcının deneyimini değiştirmek de dahil olmak üzere Azure Etkin Dizin (Azure AD) kuruluşlarında Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) özelleştirebilir.

## <a name="determine-your-version-of-pim"></a>PIM sürümünüzü belirleyin

Kasım 2019'dan itibaren, Ayrıcalıklı Kimlik Yönetimi'nin Azure AD rolleri bölümü, Azure kaynak rollerinin deneyimleriyle eşleşen yeni bir sürüme güncelleniyor. Bu, [varolan API'de değişikliklerin](azure-ad-roles-features.md#api-changes)yanı sıra ek özellikler de oluşturur. Yeni sürüm kullanıma alınırken, bu makalede hangi yordamları izleyeceğiniz, şu anda sahip olduğunuz Ayrıcalıklı Kimlik Yönetimi sürümüne bağlıdır. Ayrıcalıklı Kimlik Yönetimi'nin hangi sürümüne sahip olduğunuzu belirlemek için bu bölümdeki adımları izleyin. Ayrıcalıklı Kimlik Yönetimi sürümünüzü aldıktan sonra, bu makalede bu sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünde olan bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın. Genel bakış sayfasının üst kısmında bir başlık varsa, bu makalenin **Yeni sürüm** sekmesindeki yönergeleri izleyin. Aksi takdirde, Önceki **sürüm** sekmesindeki yönergeleri izleyin.

  [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

Azure AD rolleri isteklerini onaylamak veya reddetmek için bu makaledeki adımları izleyin.

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

## <a name="open-role-settings"></a>Rol ayarlarını açma

Azure AD rolünün ayarlarını açmak için aşağıdaki adımları izleyin.

1. [Ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolündeki bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.
gt
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi** &gt; **Azure AD rollerini** &gt; açın **Rol ayarları.**

    ![Azure AD rollerini listeleyen rol ayarları sayfası](./media/pim-how-to-change-default-settings/role-settings.png)

1. Ayarlarını yapılandırmak istediğiniz rolü seçin.

    ![Rol belirleme ayrıntıları sayfası birkaç atama ve etkinleştirme ayarını listele](./media/pim-how-to-change-default-settings/role-settings-page.png)

1. Rol ayarları sayfasını açmak için **Edit'i** seçin.

    ![Atama ve etkinleştirme ayarlarını güncelleştirme seçenekleriyle rol ayarları sayfasını değiştirme](./media/pim-how-to-change-default-settings/role-settings-edit.png)

    Her rol için Rol ayarı bölmesinde, yapılandırabileceğiniz birkaç ayar vardır.

## <a name="assignment-duration"></a>Atama süresi

Bir rol için ayarları yapılandırırken her atama türü (uygun ve etkin) için iki atama süresi seçeneği arasından seçim yapabilirsiniz. Bu seçenekler, bir kullanıcı Ayrıcalıklı Kimlik Yönetimi'ndeki role atandığında varsayılan maksimum süre olur.

Bu **uygun** atama süresi seçeneklerinden birini seçebilirsiniz:

| | |
| --- | --- |
| **Kalıcı uygun atamaya izin verme** | Genel yöneticiler ve Ayrıcalıklı rol yöneticileri kalıcı uygun atama atayabilir. |
| **Uygun atamadan sonra süresi doldu** | Genel yöneticiler ve Ayrıcalıklı rol yöneticileri, tüm uygun atamaların belirli bir başlangıç ve bitiş tarihine sahip olmayı gerektirebilir. |

Ayrıca, şu **etkin** atama süresi seçeneklerinden birini seçebilirsiniz:

| | |
| --- | --- |
| **Kalıcı etkin atamaya izin verme** | Genel yöneticiler ve Ayrıcalıklı rol yöneticileri kalıcı etkin atama atayabilir. |
| **Önce etkin atamanın süresi doldu** | Genel yöneticiler ve Ayrıcalıklı rol yöneticileri, tüm etkin atamaların belirli bir başlangıç ve bitiş tarihine sahip olmayı gerektirebilir. |

> [!NOTE]
> Belirli bir bitiş tarihiolan tüm atamalar Global yöneticiler ve Ayrıcalıklı rol yöneticileri tarafından yenilenebilir. Ayrıca, kullanıcılar [rol atamalarını genişletmek veya yenilemek](pim-resource-roles-renew-extend.md)için self servis isteklerini başlatabilir.

## <a name="require-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulama gerektirir

Ayrıcalıklı Kimlik Yönetimi, iki farklı senaryo için Azure Çok Faktörlü Kimlik Doğrulama'nın isteğe bağlı olarak uygulanmasını sağlar.

### <a name="require-multi-factor-authentication-on-active-assignment"></a>Etkin atamada Çok Faktörlü Kimlik Doğrulama Gerektirir

Bazı durumlarda, bir kullanıcıyı kısa bir süre için (örneğin bir gün) bir role atamak isteyebilirsiniz. Bu durumda, atanan kullanıcıların etkinleştirme istemesi gerekmez. Bu senaryoda, Ayrıcalıklı Kimlik Yönetimi, kullanıcı rol atamasını kullandığında çok faktörlü kimlik doğrulamasını zorlayamamaktadır, çünkü bu, atandığı andan itibaren rolde zaten etkindir.

Atamayı gerçekleştiren yöneticinin söyledikleri kişi olduğundan emin olmak için, etkin atama kutusunda Çok **Faktörlü Kimlik DoğrulamaYı İste'yi** işaretleyerek etkin atamada çok faktörlü kimlik doğrulaması uygulayabilirsiniz.

### <a name="require-multi-factor-authentication-on-activation"></a>Etkinleştirmede Çok Faktörlü Kimlik Doğrulama Gerektirir

Bir rol için uygun olan kullanıcıların etkinleştirilmeden önce Azure Çok Faktörlü Kimlik Doğrulamasını kimler kullandığını kanıtlamalarını isteyebilirsiniz. Çok faktörlü kimlik doğrulama, kullanıcının makul bir kesinlikle söyledikleri kişi olmasını sağlar. Bu seçeneğin uygulanması, kullanıcı hesabının tehlikeye girdiği durumlarda kritik kaynakları korur.

Etkinleştirmeden önce çok faktörlü kimlik doğrulaması gerektirmek için, Düzenleme rol **ayarı**Atama sekmesinde etkinleştirme kutusunda **Çok Faktörlü Kimlik Doğrulama** Yı gerektirin.

Daha fazla bilgi için [çok faktörlü kimlik doğrulama ve Ayrıcalıklı Kimlik Yönetimi'ne](pim-how-to-require-mfa.md)bakın.

## <a name="activation-maximum-duration"></a>Etkinleştirme maksimum süresi

Bir rolün süresi dolmadan önce etkin kalmasını sağlayan maksimum süreyi saat olarak ayarlamak için **Etkinleştirme maksimum süre** kaydırıcısını kullanın. Bu değer 1 ila 24 saat arasında olabilir.

## <a name="require-justification"></a>Yaslama gerektirme

Etkinleştirildiğinde kullanıcıların bir iş gerekçesi girmesini gerektirebilirsiniz. Yaslama gerektirmek için, etkin atama kutusundaki **Yaslama Yığmayı** veya etkinleştirme kutusundaki **YaslamaYı İste'yi** işaretleyin.

## <a name="require-approval-to-activate"></a>Etkinleştirmek için onay gerektirme

Birden çok onaylayıcı ayarlıyorsanız, onay, onaylanır onaylanmaz veya reddeder onaylamaz tamamlanır. En az iki kullanıcının onayını alamazsınız. Bir rolü etkinleştirmek için onay istemek için aşağıdaki adımları izleyin.

1. Onay kutusunu **etkinleştirmek için onay iste'yi** işaretleyin.

1. **Onaylayıcıları Seç'i seçin.**

    ![Onaylayıcıları seçmek için bir kullanıcı veya grup bölmesi seçin](./media/pim-resource-roles-configure-role-settings/resources-role-settings-select-approvers.png)

1. En az bir kullanıcı seçin ve sonra **Seç'i**tıklatın. En az bir onaylayıcı seçmeniz gerekir. Varsayılan onaylayıcı yok.

    Seçimleriniz seçili onaylayanlar listesinde görünür.

1. Tüm rol ayarlarınızı belirttikten sonra değişikliklerinizi kaydetmek için **Güncelleştir'i** seçin.

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

## <a name="open-role-settings"></a>Rol ayarlarını açma

Azure AD rolünün ayarlarını açmak için aşağıdaki adımları izleyin.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. **Ayarlar'ı**seçin.

    ![Azure AD rolleri - Ayarlar](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. **Roller'i**seçin.

1. Ayarlarını yapılandırmak istediğiniz rolü seçin.

    ![Azure AD rolleri - Ayarlar Rolleri](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Her rolün ayarlar sayfasında, yapılandırabileceğiniz birkaç ayar vardır. Bu ayarlar, **kalıcı** atamaları değil, **yalnızca uygun** atamaları olan kullanıcıları etkiler.

## <a name="activations"></a>Etkinleştirme

Bir rolün süresi dolmadan önce etkin kalmasını sağlayan maksimum zamanı saat olarak ayarlamak için **Etkinleştirme** kaydırıcısını kullanın. Bu değer 1 ile 72 saat arasında olabilir.

## <a name="notifications"></a>Bildirimler

Roller etkinleştirildiğinde yöneticilerin e-posta bildirimleri alıp almayacağını belirtmek için **Bildirimler** anahtarını kullanın. Bu bildirim, yetkisiz veya gayri meşru etkinleştirmeleri algılamak için yararlı olabilir.

**Etkinleştir'e**ayarlandığında, bildirimler şu şekilde gönderilir:

- Ayrıcalıklı rol yöneticisi
- Güvenlik yöneticisi
- Genel yönetici

Daha fazla bilgi için [Ayrıcalıklı Kimlik Yönetimi'ndeki E-posta bildirimlerine](pim-email-notifications.md)bakın.

## <a name="incidentrequest-ticket"></a>Olay/İstek bileti

Uygun yöneticilerin rollerini etkinleştirirken bir bilet numarası eklemelerini sağlamak için **Olay/İstek bilet** anahtarını kullanın. Bu uygulama, rol erişim denetimlerini daha etkili hale getirebilir.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Kullanıcıların rollerini etkinleştirmeden önce MFA ile kimliklerini doğrulamalarını isteyip istemediklerini belirtmek için **Çok Faktörlü Kimlik Doğrulama** anahtarını kullanın. Bir rolü her etkinleştirdiklerinde değil, oturum başına yalnızca bir kez kimliklerini doğrulamaları gerekir. MFA'yı etkinleştirdiğinizde aklınızda bulundurmanız gereken iki ipucu vardır:

- E-posta adresleri için Microsoft hesabı olan @outlook.comkullanıcılar (genellikle , ancak her zaman değil) Azure Çok Faktörlü Kimlik Doğrulaması için kaydolamaz. Microsoft hesapları olan kullanıcılara roller atamak istiyorsanız, bu rolleri kalıcı yöneticiler haline getirmeniz veya bu rol için çok faktörlü kimlik doğrulamayı devre dışı bırakmanız gerekir.
- Azure AD ve Office 365 için çok ayrıcalıklı roller için Azure Çok Faktörlü Kimlik Doğrulaması'nı devre dışı kalamazsınız. Bu güvenlik özelliği aşağıdaki rollerin korunmasına yardımcı olur:  
  
  - Azure Bilgi Koruma yöneticisi
  - Faturalama yöneticisi
  - Bulut uygulama yöneticisi
  - Uyumluluk yöneticisi
  - Koşullu erişim yöneticisi
  - Dynamics 365 yöneticisi
  - Müşteri LockBox erişim onaylayıcısı
  - Dizin yazarları
  - Exchange yöneticisi
  - Genel yönetici
  - Intune yöneticisi
  - Güç BI yöneticisi
  - Ayrıcalıklı rol yöneticisi
  - Güvenlik yöneticisi
  - SharePoint yöneticisi
  - Skype Kurumsal yöneticisi
  - Kullanıcı yöneticisi

Daha fazla bilgi için [çok faktörlü kimlik doğrulama ve Ayrıcalıklı Kimlik Yönetimi'ne](pim-how-to-require-mfa.md)bakın.

## <a name="require-approval"></a>Onay gerektirme

Bir rolü etkinleştirmek için gerekli onayı vermek istiyorsanız, aşağıdaki adımları izleyin.

1. Onay **anahtarını** **Etkin**olarak ayarlama. Bölme, onaylayıcıları seçmek için seçeneklerle genişler.

    ![Azure AD rolleri - Ayarlar - Onay gerektirme](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Herhangi bir onaylayıcı belirtmezseniz, Ayrıcalıklı rol yöneticisi varsayılan onaylayıcı olur ve daha sonra bu rol için tüm etkinleştirme isteklerini onaylaması gerekir.

1. Onaylayıcılar eklemek için **onaylayıcıları seç'i**tıklatın.

    ![Azure AD rolleri - Ayarlar - Onay gerektirme](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Ayrıcalıklı rol yöneticisine ek olarak bir veya daha fazla onaylayıcı seçin ve sonra **Seç'i**tıklatın. En az iki onaylayıcı eklemenizi öneririz. Kendinizi onaylayan olarak ekleseniz bile, bir rol etkinleştirme yi kendi kendiniz onaylayamazsınız. Seçimleriniz seçili onaylayanlar listesinde görünür.

1. Tüm rol ayarlarınızı belirttikten sonra değişikliklerinizi kaydetmek için **Kaydet'i** seçin.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetiminde Azure AD rollerini atama](pim-how-to-add-role-to-user.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rolleri için güvenlik uyarılarını yapılandırma](pim-how-to-configure-security-alerts.md)
