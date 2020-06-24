---
title: Azure portal-Azure RBAC kullanarak dış kullanıcılar için Azure rol atamaları ekleme veya kaldırma
description: Azure portal ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak bir kuruluşa harici kullanıcılar için Azure kaynaklarına nasıl erişim sağlayacağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: how-to
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: dc2763fd3c203a5b0443299d9824b6ebf61bdc78
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84791087"
---
# <a name="add-or-remove-azure-role-assignments-for-external-guest-users-using-the-azure-portal"></a>Azure portal kullanarak dış Konuk kullanıcılar için Azure rol atamaları ekleme veya kaldırma

[Azure rol tabanlı erişim denetimi (Azure RBAC)](overview.md) , büyük kuruluşlar için daha iyi güvenlik yönetimine olanak sağlar ve ortamınızda belirli kaynaklara erişmesi gereken, ancak tüm altyapıya ya da faturalandırma ile ilgili kapsamların olması gerekmeyen dış ortak, satıcı veya freelancers ile çalışan küçük ve orta ölçekli işletmeler için daha iyi güvenlik yönetimi sağlar. Dış Konuk kullanıcılarıyla işbirliği yapmak için [Azure ACTIVE DIRECTORY B2B](../active-directory/b2b/what-is-b2b.md) ' deki özellikleri kullanabilir ve yalnızca konuk kullanıcıların ortamınızda ihtiyaç duyduğu izinleri vermek IÇIN Azure RBAC 'yi kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmanız gerekir:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi izinler

## <a name="when-would-you-invite-guest-users"></a>Konuk kullanıcıları ne zaman davet edebilir?

Aşağıda, Konuk kullanıcıları kuruluşunuza davet edebilir ve izin vermeniz halinde birkaç örnek senaryo verilmiştir:

- Yalnızca bir proje için Azure kaynaklarınıza erişmek üzere bir e-posta hesabına sahip olan bir dış şirket içi satıcıya izin verin.
- Bir dış iş ortağının belirli kaynakları veya tüm abonelikleri yönetmesine izin verin.
- Sorunları gidermek için kuruluşunuzda Azure kaynağına geçici olarak erişmek için (Microsoft desteği gibi) destek mühendislerine izin verin.

## <a name="permission-differences-between-member-users-and-guest-users"></a>Üye kullanıcılar ve Konuk kullanıcılar arasındaki izin farklılıkları

Bir dizinin (üye kullanıcılar) yerel üyeleri, başka bir dizinden B2B işbirliği konuğunu (Konuk kullanıcılar) olarak davet edilen kullanıcılardan farklı izinlere sahiptir. Örneğin, Üyeler kullanıcısı, Konuk kullanıcılar kısıtlamalı dizin izinlerine sahip olan neredeyse tüm dizin bilgilerini okuyabilir. Üye kullanıcılar ve Konuk kullanıcılar hakkında daha fazla bilgi için bkz. [Azure Active Directory varsayılan kullanıcı Izinleri nelerdir?](../active-directory/fundamentals/users-default-permissions.md).

## <a name="add-a-guest-user-to-your-directory"></a>Dizininize konuk kullanıcı ekleme

Azure Active Directory sayfasını kullanarak dizininize Konuk Kullanıcı eklemek için bu adımları izleyin.

1. Kuruluşunuzun dış işbirliği ayarlarının, konukları davet etmenize izin verilecek şekilde yapılandırıldığından emin olun. Daha fazla bilgi için bkz. [B2B dış Işbirliğini etkinleştirme ve konukları kimlerin davet edebileceğini yönetme](../active-directory/b2b/delegate-invitations.md).

1. Azure Portal, **Azure Active Directory**  >  **Kullanıcılar**  >  **Yeni Konuk Kullanıcı**Azure Active Directory ' a tıklayın.

    ![Azure portal yeni Konuk Kullanıcı özelliği](./media/role-assignments-external-users/invite-guest-user.png)

1. Yeni bir Konuk Kullanıcı eklemek için adımları izleyin. Daha fazla bilgi için [Azure portal Azure ACTIVE DIRECTORY B2B işbirliği kullanıcıları ekleme](../active-directory/b2b/add-users-administrator.md#add-guest-users-to-the-directory)bölümüne bakın.

Bir konuk kullanıcıyı dizine ekledikten sonra, Konuk kullanıcıyı paylaşılan bir uygulamaya doğrudan bir bağlantı gönderebilirsiniz veya Konuk Kullanıcı davet e-postasında kullanım URL 'sini tıklatabilir.

![Konuk Kullanıcı davet e-postası](./media/role-assignments-external-users/invite-email.png)

Konuk kullanıcının dizininize erişebilmesi için davet işlemini tamamlaması gerekir.

![Konuk Kullanıcı daveti İnceleme izinleri](./media/role-assignments-external-users/invite-review-permissions.png)

Davet işlemi hakkında daha fazla bilgi için bkz. [Azure ACTIVE DIRECTORY B2B işbirliği daveti](../active-directory/b2b/redemption-experience.md)kullanım.

## <a name="add-a-role-assignment-for-a-guest-user"></a>Konuk Kullanıcı için rol ataması ekleme

Azure RBAC 'de, erişim izni vermek için bir rol atarsınız. Konuk kullanıcıya bir rol ataması eklemek için, bir üye Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik ile [aynı adımları](role-assignments-portal.md#add-a-role-assignment) izlüyordu. Bu adımları izleyerek, farklı kapsamlardaki Konuk Kullanıcı için rol ataması ekleyin.

1. Azure portalında **Tüm hizmetler**’e tıklayın.

1.  Erişimin uygulandığı kaynak kümesini (kapsam olarak da bilinir) seçin. Örneğin, **Yönetim grupları**, **abonelikler**, **kaynak grupları**veya bir kaynak seçebilirsiniz.

1. Belirli kaynağa tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

    Aşağıdaki ekran görüntüsünde, bir kaynak grubu için erişim denetimi (ıAM) dikey penceresi örneği gösterilmektedir. Burada herhangi bir erişim denetimi değişikliği yaparsanız, bu değişiklikler yalnızca kaynak grubuna uygulanır.

    ![Kaynak grubu için erişim denetimi (ıAM) dikey penceresi](./media/role-assignments-external-users/access-control-resource-group.png)

1. Bu kapsamdaki tüm rol atamalarını görüntülemek için **rol atamaları** sekmesine tıklayın.

1. **Ekle** > **Rol ataması ekle** seçeneğine tıklayarak Rol ataması ekle bölmesini açın.

    Rol atama izniniz yoksa rol ataması Ekle seçeneği devre dışı bırakılır.

    ![Menü ekle](./media/role-assignments-external-users/add-menu.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde Konuk kullanıcıyı seçin. Kullanıcıyı listede görmüyorsanız, **Select** kutusunu yazarak görünen adlar, e-posta adresleri ve nesne tanımlayıcıları için dizinde arama yapabilirsiniz.

   ![Rol atama bölmesi Ekle](./media/role-assignments-external-users/add-role-assignment.png)

1. Rolü seçili kapsamda atamak için **Kaydet** ' e tıklayın.

    ![Sanal makine katılımcısı için rol ataması](./media/role-assignments-external-users/access-control-role-assignments.png)

## <a name="add-a-role-assignment-for-a-guest-user-not-yet-in-your-directory"></a>Henüz dizininizde olmayan bir Konuk Kullanıcı için rol ataması ekleme

Konuk kullanıcıya bir rol ataması eklemek için, bir üye Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik ile [aynı adımları](role-assignments-portal.md#add-a-role-assignment) izlüyordu.

Konuk Kullanıcı henüz dizininizde değilse, kullanıcıyı doğrudan rol ataması Ekle bölmesinden davet edebilirsiniz.

1. Azure portalında **Tüm hizmetler**’e tıklayın.

1.  Erişimin uygulandığı kaynak kümesini (kapsam olarak da bilinir) seçin. Örneğin, **Yönetim grupları**, **abonelikler**, **kaynak grupları**veya bir kaynak seçebilirsiniz.

1. Belirli kaynağa tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu kapsamdaki tüm rol atamalarını görüntülemek için **rol atamaları** sekmesine tıklayın.

1. **Ekle** > **Rol ataması ekle** seçeneğine tıklayarak Rol ataması ekle bölmesini açın.

    ![Menü ekle](./media/role-assignments-external-users/add-menu.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde, davet etmek istediğiniz kişinin e-posta adresini yazın ve bu kişiyi seçin.

   ![Rol Ekleme atama bölmesinde Konuk kullanıcıyı davet et](./media/role-assignments-external-users/add-role-assignment-new-guest.png)

1. Konuk kullanıcıyı dizininize eklemek, rolü atamak ve davet göndermek için **Kaydet** ' e tıklayın.

    Birkaç dakika sonra rol atamasının ve davet hakkındaki bilgilerin bir bildirimini görürsünüz.

    ![Rol ataması ve davet edilen Kullanıcı bildirimi](./media/role-assignments-external-users/invited-user-notification.png)

1. Konuk kullanıcıyı el ile davet etmek için, bildirimde sağ tıklayın ve davet bağlantısını kopyalayın. Davet işlemini başlattığı için davet bağlantısına tıklamayın.

    Davet bağlantısı aşağıdaki biçimde olacaktır:

    `https://invitations.microsoft.com/redeem/...`

1. Davet işlemini tamamlamaya yönelik davet bağlantısını Konuk kullanıcıya gönderin.

    Davet işlemi hakkında daha fazla bilgi için bkz. [Azure ACTIVE DIRECTORY B2B işbirliği daveti](../active-directory/b2b/redemption-experience.md)kullanım.

## <a name="remove-a-guest-user-from-your-directory"></a>Bir konuk kullanıcıyı dizininizden kaldırma

Konuk kullanıcıyı bir dizinden kaldırmadan önce, önce bu Konuk Kullanıcı için herhangi bir rol atamasını kaldırmanız gerekir. Konuk kullanıcıyı bir dizinden kaldırmak için aşağıdaki adımları izleyin.

1. **Erişim denetimi (IAM)** , Konuk kullanıcının bir rol ataması olduğu yönetim grubu, abonelik, kaynak grubu veya kaynak gibi bir kapsamda açın.

1. Tüm rol atamalarını görüntülemek için **rol atamaları** sekmesine tıklayın.

1. Rol atamaları listesinde, Konuk kullanıcının yanına, kaldırmak istediğiniz rol atamasını içeren bir onay işareti ekleyin.

   ![Rol atamasını Kaldır](./media/role-assignments-external-users/remove-role-assignment-select.png)

1. **Kaldır**’a tıklayın.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-external-users/remove-role-assignment.png)

1. Görüntülenen rol atamasını Kaldır iletisinde **Evet**' e tıklayın.

1. Sol gezinti çubuğunda **Azure Active Directory**  >  **Kullanıcılar**' a tıklayın.

1. Kaldırmak istediğiniz Konuk Kullanıcı ' ya tıklayın.

1. **Sil**'e tıklayın.

   ![Konuk kullanıcıyı sil](./media/role-assignments-external-users/delete-guest-user.png)

1. Görüntülenen silme iletisinde **Evet**' e tıklayın.

## <a name="troubleshoot"></a>Sorun giderme

### <a name="guest-user-cannot-browse-the-directory"></a>Konuk Kullanıcı dizine gözatamıyorum

Konuk kullanıcıların kısıtlı dizin izinleri vardır. Örneğin, Konuk kullanıcılar dizine gözatıp gruplar veya uygulamalar arayamaz. Daha fazla bilgi için bkz. [Azure Active Directory varsayılan kullanıcı izinleri nelerdir?](../active-directory/fundamentals/users-default-permissions.md).

![Konuk Kullanıcı, bir dizindeki kullanıcılara gözatamıyorum](./media/role-assignments-external-users/directory-no-users.png)

Konuk kullanıcının dizinde ek ayrıcalıkları olması gerekiyorsa, Konuk kullanıcıya bir dizin rolü atayabilirsiniz. Aslında bir Konuk kullanıcının dizininiz için tam okuma erişimine sahip olmasını istiyorsanız, Konuk kullanıcıyı Azure AD 'de [Dizin okuyucuları](../active-directory/users-groups-roles/directory-assign-admin-roles.md) rolüne ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory kiracınızdaki iş ortağı kuruluşlarından kullanıcılara Izin verme](../active-directory/b2b/add-guest-to-role.md).

![Dizin okuyucuları rolü atama](./media/role-assignments-external-users/directory-roles.png)

### <a name="guest-user-cannot-browse-users-groups-or-service-principals-to-assign-roles"></a>Konuk Kullanıcı, rol atamak için kullanıcılara, gruplara veya hizmet sorumlularına gözatamez

Konuk kullanıcıların kısıtlı dizin izinleri vardır. Konuk Kullanıcı bir kapsamda [sahip](built-in-roles.md#owner) olsa bile, başka birinin erişimini sağlamak üzere bir rol ataması eklemeye çalışırlarsa, kullanıcılar, gruplar veya hizmet sorumluları listesine gözatamazsınız.

![Konuk Kullanıcı, rol atamak için güvenlik sorumlularına gözatamez](./media/role-assignments-external-users/directory-no-browse.png)

Konuk Kullanıcı, bir kişinin dizinde tam oturum açma adını biliyorsa, erişim izni verebilir. Aslında bir Konuk kullanıcının dizininiz için tam okuma erişimine sahip olmasını istiyorsanız, Konuk kullanıcıyı Azure AD 'de [Dizin okuyucuları](../active-directory/users-groups-roles/directory-assign-admin-roles.md) rolüne ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory kiracınızdaki iş ortağı kuruluşlarından kullanıcılara Izin verme](../active-directory/b2b/add-guest-to-role.md).

### <a name="guest-user-cannot-register-applications-or-create-service-principals"></a>Konuk Kullanıcı, uygulamaları kaydedemiyor veya hizmet sorumluları oluşturamıyor

Konuk kullanıcıların kısıtlı dizin izinleri vardır. Konuk kullanıcının uygulama kaydedebilmesi veya hizmet sorumluları oluşturması gerekiyorsa, Konuk kullanıcıyı Azure AD 'de [uygulama geliştirici](../active-directory/users-groups-roles/directory-assign-admin-roles.md) rolüne ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory kiracınızdaki iş ortağı kuruluşlarından kullanıcılara Izin verme](../active-directory/b2b/add-guest-to-role.md).

![Konuk Kullanıcı, uygulamaları kaydedemiyor](./media/role-assignments-external-users/directory-access-denied.png)

### <a name="guest-user-does-not-see-the-new-directory"></a>Konuk Kullanıcı yeni dizini görmez

Bir konuk kullanıcıya bir dizin erişimi verildiyse, ancak **Dizin + abonelik** bölmesine geçiş yapmaya çalıştıklarında Azure Portal listelenen yeni dizini görmüyorsanız, Konuk kullanıcının davet sürecini tamamladıklarından emin olun. Davet işlemi hakkında daha fazla bilgi için bkz. [Azure ACTIVE DIRECTORY B2B işbirliği daveti](../active-directory/b2b/redemption-experience.md)kullanım.

### <a name="guest-user-does-not-see-resources"></a>Konuk Kullanıcı kaynakları görmez

Bir konuk kullanıcıya bir dizin erişimi verildiyse, ancak Azure portal erişim izni verilen kaynakları görmüyorsanız, Konuk kullanıcının doğru dizini seçtiğinizden emin olun. Konuk kullanıcının birden çok dizine erişimi olabilir. Dizinler arasında geçiş yapmak için sol üst köşedeki **Dizin + abonelik**öğesine ve ardından uygun dizine tıklayın.

![Azure portal dizinler + abonelikler bölmesi](./media/role-assignments-external-users/directory-subscription.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal Azure Active Directory B2B işbirliği kullanıcıları ekleyin](../active-directory/b2b/add-users-administrator.md)
- [Azure Active Directory B2B işbirliği kullanıcısının özellikleri](../active-directory/b2b/user-properties.md)
- [B2B işbirliği davetiyesi e-postası öğeleri-Azure Active Directory](../active-directory/b2b/invitation-email-elements.md)
- [Konuk kullanıcıyı ortak yönetici olarak ekleme](classic-administrators.md#add-a-guest-user-as-a-co-administrator)