---
title: RBAC ve Azure portalına sahip harici kullanıcılar için rol atamaları ekleme veya kaldırma
description: Azure rol tabanlı erişim denetimi (RBAC) kullanarak bir kuruluşun dışındaki kullanıcılar için Azure kaynaklarına nasıl erişim tanıyacağım öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: 1c440b85f792ac5bb1336f4d20f930aafc38ad7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245648"
---
# <a name="add-or-remove-role-assignments-for-external-guest-users-using-azure-rbac-and-the-azure-portal"></a>Azure RBAC ve Azure portalını kullanan harici konuk kullanıcılar için rol atamaları ekleme veya kaldırma

[Azure rol tabanlı erişim denetimi (RBAC),](overview.md) büyük kuruluşlar ve ortamınızdaki belirli kaynaklara erişmesi gereken, ancak tüm altyapıveya faturalandırmayla ilgili kapsamlara erişmesi gereken harici işbirlikçiler, satıcılar veya serbest çalışanlarla çalışan küçük ve orta ölçekli işletmeler için daha iyi güvenlik yönetimi sağlar. [Azure Active Directory B2B'deki](../active-directory/b2b/what-is-b2b.md) yetenekleri harici konuk kullanıcılarla işbirliği yapmak için kullanabilir ve rbac'ı kullanarak sadece konuk kullanıcıların ortamınızda ihtiyaç duyduğu izinleri verebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmış olmalısınız:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` Kullanıcı Erişim [Yöneticisi](built-in-roles.md#user-access-administrator) veya [Sahibi](built-in-roles.md#owner) gibi izinler

## <a name="when-would-you-invite-guest-users"></a>Konuk kullanıcıları ne zaman davet eyleyebilirsiniz?

Konuk kullanıcıları kuruluşunuza davet edebileceğiniz ve izin verebilirsiniz birkaç örnek senaryo aşağıda verilmiştir:

- Yalnızca bir e-posta hesabı olan harici bir serbest çalışan satıcının proje için Azure kaynaklarınıza erişmesine izin verin.
- Harici bir iş ortağının belirli kaynakları veya aboneliğin tamamını yönetmesine izin verin.
- Sorunları gidermek için kuruluşunuzdaki (Microsoft desteği gibi) destek mühendislerinin Azure kaynağınıza geçici olarak erişmesine izin verin.

## <a name="permission-differences-between-member-users-and-guest-users"></a>Üye kullanıcılar ve konuk kullanıcılar arasındaki izin farkları

Bir dizinin yerel üyeleri (üye kullanıcılar), b2B işbirliği konuğu olarak başka bir dizinden davet edilen kullanıcılardan (konuk kullanıcılar) farklı izinlere sahiptir. Örneğin, konuk kullanıcılar sınırlı dizin izinlerine sahipken, üye kullanıcı hemen hemen tüm dizin bilgilerini okuyabilir. Üye kullanıcılar ve konuk kullanıcılar hakkında daha fazla bilgi için Azure [Etkin Dizini'nde varsayılan kullanıcı izinleri nelerdir?](../active-directory/fundamentals/users-default-permissions.md)

## <a name="add-a-guest-user-to-your-directory"></a>Dizininize konuk kullanıcı ekleme

Azure Etkin Dizin sayfasını kullanarak dizininize konuk kullanıcı eklemek için aşağıdaki adımları izleyin.

1. Kuruluşunuzun dış işbirliği ayarlarının, konukları davet etme izniniz olacak şekilde yapılandırıldığından emin olun. Daha fazla bilgi için [B2B dış işbirliğini etkinleştir mecnuna bakın ve konukları kimlerin davet edebileceğini yönetin.](../active-directory/b2b/delegate-invitations.md)

1. Azure portalında Azure **Etkin Dizin** > **Kullanıcıları** > **Yeni konuk kullanıcıyı**tıklatın.

    ![Azure portalında yeni konuk kullanıcı özelliği](./media/role-assignments-external-users/invite-guest-user.png)

1. Yeni bir konuk kullanıcı eklemek için adımları izleyin. Daha fazla bilgi için azure [portalındaki Azure Etkin Dizin B2B işbirliği kullanıcıları ekle'ye](../active-directory/b2b/add-users-administrator.md#add-guest-users-to-the-directory)bakın.

Dizine bir konuk kullanıcı ekledikten sonra, konuk kullanıcıya paylaşılan bir uygulamaya doğrudan bağlantı gönderebilir veya konuk kullanıcı davet e-postasında kullanım URL'sini tıklatabilirsiniz.

![Konuk kullanıcı e-posta daveti](./media/role-assignments-external-users/invite-email.png)

Konuk kullanıcının dizininize erişebilmesi için davet işlemini tamamlamaları gerekir.

![Konuk kullanıcı davet inceleme izinleri](./media/role-assignments-external-users/invite-review-permissions.png)

Davet süreci hakkında daha fazla bilgi için [Azure Active Directory B2B işbirliği daveti ödeme](../active-directory/b2b/redemption-experience.md)sini görün.

## <a name="add-a-role-assignment-for-a-guest-user"></a>Konuk kullanıcı için rol ataması ekleme

RBAC'da, erişim izni vermek için bir rol atarsınız. Konuk kullanıcı için rol ataması eklemek için, üye kullanıcı, grup, hizmet sorumlusu veya yönetilen kimlik için yaptığınız [adımları](role-assignments-portal.md#add-a-role-assignment) izlersiniz. Aşağıdaki adımları izleyin, farklı kapsamlarda konuk kullanıcı için bir rol ataması ekleyin.

1. Azure portalında **Tüm hizmetler**’e tıklayın.

1.  Kapsamın da bildiği erişimin uygulandığı kaynak kümesini seçin. Örneğin, **Yönetim gruplarını,** **Abonelikleri,** **Kaynak gruplarını**veya bir kaynağı seçebilirsiniz.

1. Belirli kaynağı tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

    Aşağıdaki ekran görüntüsü, bir kaynak grubu için Access denetimi (IAM) bıçak örneği gösterir. Burada herhangi bir erişim denetimi değişikliği yaparsanız, bunlar yalnızca kaynak grubuna uygulanır.

    ![Kaynak grubu için erişim denetimi (IAM) bıçak](./media/role-assignments-external-users/access-control-resource-group.png)

1. Bu kapsamdaki tüm rol atamalarını görüntülemek için **Rol atamaları** sekmesini tıklatın.

1. Rol Atama ekle bölmesini açmak için**ekle rol atamasını** **tıklatın.** > 

    Rolleri atama izniniz yoksa, rol atama ekle seçeneği devre dışı bırakılır.

    ![Menü ekle](./media/role-assignments-external-users/add-menu.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde konuk kullanıcıyı seçin. Listede kullanıcıyı görmüyorsanız, görüntü adları, e-posta adresleri ve nesne tanımlayıcıları için dizinde arama yapmak için **Seç** kutusuna yazabilirsiniz.

   ![Rol atama bölmesi ekleme](./media/role-assignments-external-users/add-role-assignment.png)

1. Seçili kapsamda rolü atamak için **Kaydet'i** tıklatın.

    ![Sanal Makine Katılımcısı için rol ataması](./media/role-assignments-external-users/access-control-role-assignments.png)

## <a name="add-a-role-assignment-for-a-guest-user-not-yet-in-your-directory"></a>Dizininizde henüz olmayan konuk kullanıcı için rol ataması ekleme

Konuk kullanıcı için rol ataması eklemek için, üye kullanıcı, grup, hizmet sorumlusu veya yönetilen kimlik için yaptığınız [adımları](role-assignments-portal.md#add-a-role-assignment) izlersiniz.

Konuk kullanıcı henüz dizininizde değilse, kullanıcıyı doğrudan Rol Atama ekle bölmesinden davet edebilirsiniz.

1. Azure portalında **Tüm hizmetler**’e tıklayın.

1.  Kapsamın da bildiği erişimin uygulandığı kaynak kümesini seçin. Örneğin, **Yönetim gruplarını,** **Abonelikleri,** **Kaynak gruplarını**veya bir kaynağı seçebilirsiniz.

1. Belirli kaynağı tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu kapsamdaki tüm rol atamalarını görüntülemek için **Rol atamaları** sekmesini tıklatın.

1. Rol Atama ekle bölmesini açmak için**ekle rol atamasını** **tıklatın.** > 

    ![Menü ekle](./media/role-assignments-external-users/add-menu.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde, davet etmek istediğiniz kişinin e-posta adresini yazın ve bu kişiyi seçin.

   ![Rol atama ekle bölmesine konuk kullanıcıyı davet et](./media/role-assignments-external-users/add-role-assignment-new-guest.png)

1. Konuk kullanıcıyı dizine eklemek, rolü atamak ve davet göndermek için **Kaydet'i** tıklatın.

    Birkaç dakika sonra, rol ataması bildirimini ve davetle ilgili bilgileri görürsünüz.

    ![Rol atamave davet edilen kullanıcı bildirimi](./media/role-assignments-external-users/invited-user-notification.png)

1. Konuk kullanıcıyı el ile davet etmek için bildirimdeki davet bağlantısını sağ tıklatın ve kopyalayın. Davet işlemini başlattığı için davet bağlantısını tıklatmayın.

    Davet bağlantısı aşağıdaki biçime sahip olacaktır:

    `https://invitations.microsoft.com/redeem/...`

1. Davet işlemini tamamlamak için davet bağlantısını konuk kullanıcıya gönderin.

    Davet süreci hakkında daha fazla bilgi için [Azure Active Directory B2B işbirliği daveti ödeme](../active-directory/b2b/redemption-experience.md)sini görün.

## <a name="remove-a-guest-user-from-your-directory"></a>Konuk kullanıcıyı dizininizden kaldırma

Konuk kullanıcıyı dizinden kaldırmadan önce, öncelikle bu konuk kullanıcıiçin tüm rol atamalarını kaldırmanız gerekir. Konuk kullanıcıyı dizinden kaldırmak için aşağıdaki adımları izleyin.

1. Konuk kullanıcının rol ataması olan yönetim grubu, abonelik, kaynak grubu veya kaynak gibi bir kapsamda **Açık Erişim denetimi (IAM).**

1. Tüm rol **atamalarını** görüntülemek için Rol atamaları sekmesini tıklatın.

1. Rol atamaları listesinde, kaldırmak istediğiniz rol atamasıyla konuk kullanıcının yanına bir onay işareti ekleyin.

   ![Rol atamayı kaldırma](./media/role-assignments-external-users/remove-role-assignment-select.png)

1. **Kaldır**’a tıklayın.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-external-users/remove-role-assignment.png)

1. Görünen rol atamasını kaldır iletisinde **Evet'i**tıklatın.

1. Sol gezinti çubuğunda **Azure Etkin Dizin** > **Kullanıcıları'nı**tıklatın.

1. Kaldırmak istediğiniz konuk kullanıcıyı tıklatın.

1. **Sil'i**tıklatın.

   ![Konuk kullanıcıyı silme](./media/role-assignments-external-users/delete-guest-user.png)

1. Görünen silme iletisinde **Evet'i**tıklatın.

## <a name="troubleshoot"></a>Sorun giderme

### <a name="guest-user-cannot-browse-the-directory"></a>Konuk kullanıcı dizine göz atamıyor

Konuk kullanıcıların dizin izinleri kısıtlandı. Örneğin, konuk kullanıcılar dizine göz atamıyor ve grupları veya uygulamaları arayamaz. Daha fazla bilgi için Azure [Etkin Dizin'inde varsayılan kullanıcı izinleri nelerdir?](../active-directory/fundamentals/users-default-permissions.md)

![Konuk kullanıcı bir dizinde kullanıcılara göz atamıyor](./media/role-assignments-external-users/directory-no-users.png)

Konuk kullanıcının dizinde ek ayrıcalıklara ihtiyacı varsa, konuk kullanıcıya bir dizin rolü atayabilirsiniz. Konuk bir kullanıcının dizininize tam okuma erişimi olmasını gerçekten istiyorsanız, konuk kullanıcıyı Azure AD'deki [Dizin Okuyucuları](../active-directory/users-groups-roles/directory-assign-admin-roles.md) rolüne ekleyebilirsiniz. Daha fazla bilgi için azure [Etkin Dizin kiracınızdaki iş ortağı kuruluşlarından kullanıcılara izin verme](../active-directory/b2b/add-guest-to-role.md)izni ne resitresine bakın.

![Dizin Okuyucuları rolünü atama](./media/role-assignments-external-users/directory-roles.png)

### <a name="guest-user-cannot-browse-users-groups-or-service-principals-to-assign-roles"></a>Konuk kullanıcı rolleri atamak için kullanıcılara, gruplara veya hizmet ilkelerine göz atamaz

Konuk kullanıcıların dizin izinleri kısıtlandı. Bir konuk kullanıcı kapsamda [Sahip](built-in-roles.md#owner) olsa bile, bir başkasına erişim sağlamak için bir rol ataması eklemeye çalışırsa, kullanıcı, grup veya hizmet ilkeleri listesine göz atamaz.

![Konuk kullanıcı rolleri atamak için güvenlik ilkelerine göz atamaz](./media/role-assignments-external-users/directory-no-browse.png)

Konuk kullanıcı, bir kişinin dizindeki tam oturum açma adını biliyorsa, erişim izni verebilir. Konuk bir kullanıcının dizininize tam okuma erişimi olmasını gerçekten istiyorsanız, konuk kullanıcıyı Azure AD'deki [Dizin Okuyucuları](../active-directory/users-groups-roles/directory-assign-admin-roles.md) rolüne ekleyebilirsiniz. Daha fazla bilgi için azure [Etkin Dizin kiracınızdaki iş ortağı kuruluşlarından kullanıcılara izin verme](../active-directory/b2b/add-guest-to-role.md)izni ne resitresine bakın.

### <a name="guest-user-cannot-register-applications-or-create-service-principals"></a>Konuk kullanıcı uygulamaları kaydedemez veya hizmet ilkeleri oluşturamaz

Konuk kullanıcıların dizin izinleri kısıtlandı. Konuk bir kullanıcının uygulamaları kaydedebilmesi veya hizmet ilkeleri oluşturabilmesi gerekiyorsa, konuk kullanıcıyı Azure AD'deki [Uygulama Geliştiricisi](../active-directory/users-groups-roles/directory-assign-admin-roles.md) rolüne ekleyebilirsiniz. Daha fazla bilgi için azure [Etkin Dizin kiracınızdaki iş ortağı kuruluşlarından kullanıcılara izin verme](../active-directory/b2b/add-guest-to-role.md)izni ne resitresine bakın.

![Konuk kullanıcı uygulamaları kaydedemez](./media/role-assignments-external-users/directory-access-denied.png)

### <a name="guest-user-does-not-see-the-new-directory"></a>Konuk kullanıcı yeni dizini görmüyor

Bir konuk kullanıcıya bir dizine erişim izni verilmişse, ancak **Dizin + abonelik** bölmelerinde geçiş yapmaya çalıştıklarında Azure portalında listelenen yeni dizini görmüyorsa, konuk kullanıcının davet işlemini tamamladığınızdan emin olun. Davet süreci hakkında daha fazla bilgi için [Azure Active Directory B2B işbirliği daveti ödeme](../active-directory/b2b/redemption-experience.md)sini görün.

### <a name="guest-user-does-not-see-resources"></a>Konuk kullanıcı kaynakları görmüyor

Konuk kullanıcıya bir dizine erişim izni verilmişse, ancak Azure portalında erişim izni verilen kaynakları görmüyorsa, konuk kullanıcının doğru dizini seçtiğinden emin olun. Konuk kullanıcının birden çok dizine erişimi olabilir. Dizinleri değiştirmek için sol üstte **Dizin + aboneliğini**tıklatın ve ardından uygun dizini tıklatın.

![Azure portalında Dizinler + Abonelikler bölmesi](./media/role-assignments-external-users/directory-subscription.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalında Azure Active Directory B2B işbirliği kullanıcıları ekleme](../active-directory/b2b/add-users-administrator.md)
- [Azure Active Directory B2B işbirliği kullanıcısının özellikleri](../active-directory/b2b/user-properties.md)
- [B2B işbirliği daveti e-postasının öğeleri - Azure Active Directory](../active-directory/b2b/invitation-email-elements.md)
- [Yardımcı Yönetici olarak konuk kullanıcı ekleme](classic-administrators.md#add-a-guest-user-as-a-co-administrator)