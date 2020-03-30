---
title: Azure AD özel rolünü yapılandırma - Ayrıcalıklı Kimlik Yönetimi (PIM)
description: Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure AD özel rolleri nasıl yapılandırılabilen
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/06/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f087ffe824a198c578f076857cca7fdf0f0a60c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77498689"
---
# <a name="configure-azure-ad-custom-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure AD özel rollerini yapılandırma

Ayrıcalıklı bir rol yöneticisi, atanmasını özel bir role etkinleştirdiğinde ve özel roller atan diğer uygulama yöneticileri için kullanıcı için geçerli olan rol ayarlarını değiştirebilir.

> [!NOTE]
> Azure AD özel rolleri önizleme sırasında yerleşik dizin rolleri ile tümleşik değildir. Yetenek genel olarak kullanılabilir olduğunda, rol yönetimi yerleşik roller deneyiminde yer alacaktır. Aşağıdaki banner'ı görürseniz, bu roller [yerleşik roller deneyiminde](pim-how-to-activate-role.md) yönetilmelidir ve bu makale geçerli değildir:
>
> [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

## <a name="open-role-settings"></a>Rol ayarlarını açma

Azure AD rolünün ayarlarını açmak için aşağıdaki adımları izleyin.

1. Azure portalında Ayrıcalıklı Rol Yöneticisi rolüne atanmış bir kullanıcı hesabıyla [Ayrıcalıklı Kimlik Yönetimi'nde](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_Azure_PIMCommon/CommonMenuBlade/quickStart) oturum açın.
1. **Azure AD özel rollerini seçin (Önizleme)**.

    ![Uygun rol atamalarını görmek için Azure AD özel roller önizlemesini seçin](./media/azure-ad-custom-roles-configure/settings-list.png)

1. **Ayarlar** sayfasını açmak için **Ayar'ı** seçin. Yapılandırmak istediğiniz ayarlar için rolü seçin.
1. **Rol ayarları** sayfasını açmak için **Edit'i** seçin.

    ![Ayarları nızı oluşturmak için Azure AD özel rolünü açın](./media/azure-ad-custom-roles-configure/edit-settings.png)

## <a name="role-settings"></a>Rol ayarları

Yapılandırabileceğiniz birkaç ayar vardır.

### <a name="assignment-duration"></a>Atama süresi

Bir rol için ayarları yapılandırırken her atama türü (uygun veya etkin) için iki atama süresi seçeneği arasından seçim yapabilirsiniz. Bu seçenekler, bir üye ayrıcalıklı kimlik yönetiminde role atandığında varsayılan maksimum süre olur.

Bu *uygun* atama süresi seçeneklerinden birini seçebilirsiniz.

- **Kalıcı uygun atamaya izin verin**: Yöneticiler kalıcı uygun üyelik atayabilirler.
- **Geçerli atamadan sonra geçerli atamanın sona ermesi**: Yöneticiler, tüm uygun atamaların belirli bir başlangıç ve bitiş tarihine sahip olmasını gerektirebilir.

Ayrıca, şu *etkin* atama süresi seçeneklerinden birini seçebilirsiniz:

- **Kalıcı etkin atamaya izin ver**: Yöneticiler kalıcı etkin üyelik atayabilir.
- **Önce etkin atamanın süresi dolabilir**: Yöneticiler, tüm etkin atamaların belirli bir başlangıç ve bitiş tarihine sahip olmasını gerektirebilir.

### <a name="require-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulamayı İsteyin

Ayrıcalıklı Kimlik Yönetimi, iki farklı senaryo için Azure Çok Faktörlü Kimlik Doğrulama'nın isteğe bağlı olarak uygulanmasını sağlar.

- **Etkin atamada Çok Faktörlü Kimlik Doğrulama Gerektirir**

  Bir üyeyi yalnızca kısa bir süre için bir role atamak istiyorsanız (örneğin, bir gün), atanan üyelerin etkinleştirme istemesini gerektirmek çok yavaş olabilir. Bu senaryoda, Ayrıcalıklı Kimlik Yönetimi, kullanıcı rol atamasını etkinleştirdiğinde çok faktörlü kimlik doğrulamasını zorlayamamaktadır, çünkü atandıkları andan itibaren rolde zaten etkindirler. Atamayı gerçekleştiren yöneticinin söyledikleri kişi olduğundan emin olmak için, etkin atama kutusunda **Çok Faktörlü Kimlik Doğrulamayı İste'yi** seçin.

- **Etkinleştirmede Çok Faktörlü Kimlik Doğrulama Gerektirir**

  Bir role atanan uygun kullanıcıların etkinleştirilmeden önce Azure Çok Faktörlü Kimlik Doğrulamasına kaydolmasını gerektirebilirsiniz. Bu işlem, etkinleştirme isteyen kullanıcının makul bir kesinlik ile olduğunu söyledikleri kişi olmasını sağlar. Bu seçeneğin uygulanması, kullanıcı hesabının tehlikeye girdiği durumlarda kritik rolleri korur. Uygun bir üyenin etkinleştirmeden önce Azure Çok Faktörlü Kimlik Doğrulaması'nı çalıştırmasını istemek için **etkinleştirme kutusunda Çok Faktörlü Kimlik Doğrulaması İste'yi** seçin.

Daha fazla bilgi için [çok faktörlü kimlik doğrulama ve Ayrıcalıklı Kimlik Yönetimi'ne](pim-how-to-require-mfa.md)bakın.

### <a name="activation-maximum-duration"></a>Etkinleştirme maksimum süresi

Bir rolün süresi dolmadan önce etkin kalmasını sağlayan maksimum süreyi saat olarak ayarlamak için **Etkinleştirme maksimum süre** kaydırıcısını kullanın. Bu değer 1 ve 24 saat olabilir.

### <a name="require-justification"></a>Yaslama gerektirme

Üyelerin etkin atamada veya etkinleştirildiğinde bir yaslama girmelerini gerektirebilirsiniz. Yaslama gerektirmek için, etkin atama onay kutusundaya **yaslama yı** veya **etkinleştirme** kutusundaya gerek seve sormasını seçin.

### <a name="require-approval-to-activate"></a>Etkinleştirmek için onay gerektirme

Bir rolü etkinleştirmek için onay almak istiyorsanız, aşağıdaki adımları izleyin.

1. Onay kutusunu **etkinleştirmek için Onay İste'yi** seçin.
1. **Üye veya grup** listesini seçmek için **onaylayıcıları seçin.**

    ![Ayarları nızı oluşturmak için Azure AD özel rolünü açın](./media/azure-ad-custom-roles-configure/select-approvers.png)

1. En az bir üye veya grup seçin ve sonra **Seç'i**tıklatın. En az bir onaylayıcı seçmeniz gerekir. Varsayılan onaylayıcı yok. Seçimleriniz seçili onaylayanlar listesinde görünür.
1. Rol ayarlarını belirttikten sonra değişikliklerinizi kaydetmek için **Güncelleştir'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD özel rolünü etkinleştirme](azure-ad-custom-roles-activate.md)
- [Azure AD özel rolü atama](azure-ad-custom-roles-assign.md)
- [Azure AD özel rol ataması kaldırma veya güncelleştirme](azure-ad-custom-roles-update-remove.md)
- [Azure AD'de rol tanımları](../users-groups-roles/directory-assign-admin-roles.md)
