---
title: PIM'de Azure AD rollerini atama - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure AD rollerini nasıl atadiğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/07/2020
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5048cefaae10cd55091dd72f0b73a3cf9d731a35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253279"
---
# <a name="assign-azure-ad-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetiminde Azure AD rollerini atama

Azure Etkin Dizini (Azure AD) ile, global yönetici **kalıcı** Azure AD yöneticisi rol atamaları yapabilir. Bu rol atamaları [Azure portalı](../users-groups-roles/directory-assign-admin-roles.md) kullanılarak veya [PowerShell komutları](/powershell/module/azuread#directory_roles)kullanılarak oluşturulabilir.

Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM) hizmeti, Ayrıcalıklı rol yöneticilerinin kalıcı yönetici rol atamaları yapmasına da olanak tanır. Ayrıca, ayrıcalıklı rol yöneticileri kullanıcıları Azure AD yönetici rolleri için **uygun** hale getirebilir. Uygun bir yönetici, gerektiğinde rolü etkinleştirebilir ve bu görev tamamlandığında izinlerinin süresi dolabilir.

## <a name="determine-your-version-of-pim"></a>PIM sürümünüzü belirleyin

Kasım 2019'dan itibaren, Ayrıcalıklı Kimlik Yönetimi'nin Azure AD rolleri bölümü, Azure kaynak rollerinin deneyimleriyle eşleşen yeni bir sürüme güncelleniyor. Bu, [varolan API'de değişikliklerin](azure-ad-roles-features.md#api-changes)yanı sıra ek özellikler de oluşturur. Yeni sürüm kullanıma alınırken, bu makalede hangi yordamları izleyeceğiniz, şu anda sahip olduğunuz Ayrıcalıklı Kimlik Yönetimi sürümüne bağlıdır. Ayrıcalıklı Kimlik Yönetimi'nin hangi sürümüne sahip olduğunuzu belirlemek için bu bölümdeki adımları izleyin. Ayrıcalıklı Kimlik Yönetimi sürümünüzü aldıktan sonra, bu makalede bu sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünde olan bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın. Genel bakış sayfasının üst kısmında bir başlık varsa, bu makalenin **Yeni sürüm** sekmesindeki yönergeleri izleyin. Aksi takdirde, Önceki **sürüm** sekmesindeki yönergeleri izleyin.

  [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

## <a name="assign-a-role"></a>Rol atama

Bir kullanıcıyı Azure AD yöneticisi rolü için uygun hale getirmek için aşağıdaki adımları izleyin.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolünün bir üyesi olan bir kullanıcıyla Azure [portalında](https://portal.azure.com/) oturum açın.

    Ayrıcalıklı Kimlik Yönetimi'ni yönetmek için başka bir yöneticiye erişim izni verme hakkında bilgi için, [Ayrıcalıklı Kimlik Yönetimi'ni yönetmek için diğer yöneticilere erişim izni verilmesi bölümüne](pim-how-to-give-access-to-pim.md)bakın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. Azure AD izinlerinin rol listesini görmek için **Roller'i** seçin.

    ![Azure AD rolleri](./media/pim-how-to-add-role-to-user/roles-list.png)

1. **Yeni atama** sayfasını açmak için **Üye Ekle'yi** seçin.

1. Rol sayfası seç'i açmak için **rol seç'i** seçin.

    ![Yeni atama bölmesi](./media/pim-how-to-add-role-to-user/select-role.png)

1. Atamak istediğiniz bir rolü seçin ve sonra **Seç'i**tıklatın.

1. Role atamak istediğiniz bir üye seçin ve ardından **Seç'i**seçin.

1. **Üyelik ayarları** bölmesindeki **Atama türü** listesinde **Uygun** veya **Etkin'i**seçin.

    - **Uygun** atamalar, rolü kullanmak için bir eylem gerçekleştirmek için rolün üyesigerektirir. Eylemler, çok faktörlü bir kimlik doğrulama (MFA) denetimi gerçekleştirmeyi, bir iş gerekçesi sağlamayı veya atanan onaylayıcılardan onay istemeyi içerebilir.

    - **Etkin** atamalar, üyenin rolü kullanmak için herhangi bir eylem gerçekleştirmesini gerektirmez. Etkin olarak atanan üyeler, her zaman bu role atanan ayrıcalıklara sahiptir.

1. Atama kalıcı (kalıcı olarak uygun veya kalıcı olarak atanmış) olmalıdır, **Kalıcı** onay kutusunu seçin.

    Rol ayarlarına bağlı olarak, onay kutusu görünmeyebilir veya değiştirilemez olabilir.

1. Belirli bir atama süresini belirtmek için onay kutusunu temizleyin ve başlangıç ve/veya bitiş tarihi ve saat kutularını değiştirin. Tamamlandığında, **Bitti'yi**seçin.

    ![Üyelik ayarları - tarih ve saat](./media/pim-how-to-add-role-to-user/start-and-end-dates.png)

1. Yeni rol ataması oluşturmak için **Ekle'yi**seçin. Durum bildirimi görüntülenir.

    ![Yeni atama - Bildirim](./media/pim-how-to-add-role-to-user/assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Varolan bir rol atamayı güncelleştirme veya kaldırma

Varolan bir rol atamasını güncelleştirmek veya kaldırmak için aşağıdaki adımları izleyin.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. Azure AD için roller listesini görmek için **Roller'i** seçin.

1. Güncelleştirmek veya kaldırmak istediğiniz rolü seçin.

1. **Uygun rollerdeki** veya Etkin **roller** sekmelerindeki rol atamasını bulun.

    ![Rol atamayı güncelleştirme veya kaldırma](./media/pim-how-to-add-role-to-user/remove-update-assignments.png)

1. Rol atamasını güncelleştirmek veya kaldırmak için **Güncelleştir** veya **Kaldır'ı** seçin.

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

## <a name="make-a-user-eligible-for-a-role"></a>Bir kullanıcıyı rol için uygun hale getirme

Bir kullanıcıyı Azure AD yöneticisi rolü için uygun hale getirmek için aşağıdaki adımları izleyin.

1. **Roller** veya **Üyeler'i**seçin.

    ![Azure AD rolleri](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Yönetilen **üye ekle'yi** açmak için üye ekle'yi seçin. **Add managed members**

1. **Rolü seçin,** yönetmek istediğiniz rolü seçin ve sonra **Seçin'i**seçin.

    ![Rol seçin](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. **Üyeleri Seç'i**seçin, role atamak istediğiniz kullanıcıları seçin ve sonra **Seç'i**seçin.

    ![Rol seçin](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. **Yönetilen üye ekle'de,** kullanıcıyı role eklemek için **Tamam'ı** seçin.

1. Roller listesinde, üye listesini görmek için yalnızca atadığınız rolü seçin.

     Rol atandığında, seçtiğiniz kullanıcı rol için **Uygun** olarak üyeler listesinde görünür.

    ![Bir rol için uygun kullanıcı](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Artık kullanıcı rol için uygun olduğuna göre, ayrıcalıklı kimlik yönetiminde Azure [AD rollerimi etkinleştirme'deki](pim-how-to-activate-role.md)yönergelere göre etkinleştirebileceklerini bildirin.

    Uygun yöneticilerin etkinleştirme sırasında Azure Çok Faktörlü Kimlik Doğrulama (MFA) için kaydolması istenir. Bir kullanıcı MFA'ya kaydolemiyorsa veya bir @outlook.comMicrosoft hesabı kullanıyorsa (örneğin), bunları tüm rollerinde kalıcı hale getirmeniz gerekir.

## <a name="make-a-role-assignment-permanent"></a>Rol ataması kalıcı hale getirme

Varsayılan olarak, yeni kullanıcılar yalnızca Azure AD yöneticisi rolü için *uygundur.* Bir rol atamasını kalıcı hale getirmek istiyorsanız aşağıdaki adımları izleyin.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. **Üyeler**’i seçin.

    ![Üye listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Kalıcı hale getirmek istediğiniz **Uygun** bir rol seçin.

1. **Daha Fazla'yı** seçin ve ardından **perm yap'ı**seçin.

    ![Rol ataması kalıcı hale getirme](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    Rol artık **kalıcı**olarak listelenir.

    ![Kalıcı değişikliği olan üyelerin listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Kullanıcıyı rolden kaldırma

Kullanıcıları rol atamalarından kaldırabilirsiniz, ancak her zaman kalıcı bir Global yönetici olan en az bir kullanıcı olduğundan emin olun. Hangi kullanıcıların rol atamalarına hala ihtiyaç duyduğundan emin değilseniz, [rol için bir erişim incelemesi başlatabilirsiniz.](pim-how-to-start-security-review.md)

Belirli bir kullanıcıyı Azure AD yöneticisi rolünden kaldırmak için aşağıdaki adımları izleyin.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. **Üyeler**’i seçin.

    ![Üye listesi](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Kaldırmak istediğiniz bir rol ataması seçin.

1. **Daha Fazla'yı** seçin ve ardından **Kaldır'ı**seçin.

    ![Rolü kaldırma](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. Onaylamanızı isteyen iletide **Evet'i**seçin.

    ![Rolü kaldırma](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    Rol ataması kaldırılır.

## <a name="authorization-error-when-assigning-roles"></a>Roller atarken yetkilendirme hatası

Yakın zamanda bir abonelik için Ayrıcalıklı Kimlik Yönetimi'ni etkinleştirdiyseniz ve bir kullanıcıyı Azure AD yöneticisi rolü için uygun hale getirmeye çalıştığınızda bir yetkilendirme hatası alırsanız, bunun nedeni MS-PIM hizmet yöneticisinin henüz uygun izinlere sahip olmaması olabilir. MS-PIM hizmet sorumlusu, başkalarına rol atamak için [Kullanıcı Erişim Yöneticisi](../../role-based-access-control/built-in-roles.md#user-access-administrator) rolüne sahip olmalıdır. MS-PIM'ye Kullanıcı Erişim Yöneticisi rolü atanana kadar beklemek yerine, el ile atayabilirsiniz.

Kullanıcı Erişim Yöneticisi rolünü bir abonelik için MS-PIM hizmet ilkesine atamak için aşağıdaki adımları izleyin.

1. Azure portalında Global Administrator olarak oturum açın.

1. **Tüm hizmetleri** ve ardından **Abonelikleri**seçin.

1. Aboneliğinizi seçin.

1. **Erişim denetimi (IAM)** öğesini seçin.

1. Abonelik kapsamındaki geçerli rol atamaları listesini görmek için **Rol atamaları'nı** seçin.

   ![Abonelik için erişim kontrolü (IAM) bıçak](./media/pim-how-to-add-role-to-user/ms-pim-access-control.png)

1. **MS-PIM** hizmet sorumlusuna Kullanıcı **Erişim Yöneticisi** rolü atanıp atanmadığını denetleyin.

1. Değilse, **rol atama** ekle bölmesini açmak için **rol atamaekle'yi** seçin.

1. **Rol** açılır **listesinde, Kullanıcı Erişim Yöneticisi** rolünü seçin.

1. **Seç** **listesinde, MS-PIM** servis ilkesini bulun ve seçin.

   ![Rol atama bölmesi ekleme - MS-PIM hizmet sorumlusu için izin ekleme](./media/pim-how-to-add-role-to-user/ms-pim-add-permissions.png)

1. Rolü atamak için **Kaydet'i** seçin.

   Birkaç dakika sonra, MS-PIM hizmet sorumlusuna abonelik kapsamında Kullanıcı Erişim Yöneticisi rolü atanır.

   ![MS-PIM hizmet sorumlusu için Kullanıcı erişim yöneticisi rol atamasını gösteren erişim kontrol sayfası](./media/pim-how-to-add-role-to-user/ms-pim-user-access-administrator.png)

 ---

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD yöneticisi rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini atama](pim-resource-roles-assign-roles.md)