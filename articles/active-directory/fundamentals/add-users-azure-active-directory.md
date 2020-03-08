---
title: Ekleme veya silme kullanıcılar - Azure Active Directory | Microsoft Docs
description: Yeni kullanıcıların eklenmesini veya Azure Active Directory kullanarak mevcut kullanıcı silme hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d72616422934501e042375edfb10a25aa27c527
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78377535"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>Ekleme veya Azure Active Directory kullanarak kullanıcı silme

Yeni kullanıcılar ekleyin veya Azure Active Directory (Azure AD) kuruluşunuzdaki mevcut kullanıcıları silin. Kullanıcıları eklemek veya silmek için Kullanıcı Yöneticisi veya genel yönetici olmanız gerekir.

## <a name="add-a-new-user"></a>Yeni kullanıcı ekleme

Azure Active Directory portalı kullanarak yeni bir kullanıcı oluşturabilir.

Yeni bir kullanıcı eklemek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/) , kuruluş için Kullanıcı Yöneticisi olarak oturum açın.

1. Herhangi bir sayfadan *Azure Active Directory* arayın ve seçin.

1. **Kullanıcılar**' ı seçin ve ardından **Yeni Kullanıcı**' yı seçin.

    ![Kullanıcılar aracılığıyla Kullanıcı ekleme-Azure AD 'deki tüm kullanıcılar](media/add-users-azure-active-directory/add-user-in-users-all-users.png)

1. **Kullanıcı** sayfasında, bu kullanıcı için bilgi girin:

   - **Ad**. Gereklidir. Yeni kullanıcı ilk ve son adı. Örneğin, *Mary Parker*.

   - **Kullanıcı adı**. Gereklidir. Yeni kullanıcının kullanıcı adı. Örneğin, `mary@contoso.com`.

     Kullanıcı adının etki alanı bölümü, ilk varsayılan etki alanı adını, *\<yourdomainname >. onmicrosoft. com*veya *contoso.com*gibi özel bir etki alanı adını kullanmalıdır. Özel etki alanı adı oluşturma hakkında daha fazla bilgi için, [Azure Active Directory portalını kullanarak özel etki alanı adınızı ekleme](add-custom-domain.md)bölümüne bakın.

   - **Gruplar**. İsteğe bağlı olarak, bir veya daha fazla mevcut gruplara kullanıcı ekleyebilirsiniz. Ayrıca, kullanıcı gruplarına daha sonra ekleyebilirsiniz. Gruplara kullanıcı ekleme hakkında daha fazla bilgi için bkz. [temel Grup oluşturma ve Azure Active Directory kullanarak üye ekleme](active-directory-groups-create-azure-portal.md).

   - **Dizin rolü**: Kullanıcı IÇIN Azure AD Yönetim izinlerine ihtiyacınız varsa, bunları BIR Azure AD rolüne ekleyebilirsiniz. Kullanıcıyı bir genel yönetici veya Azure AD 'de sınırlı bir veya daha fazla yönetici rolünden atayabilirsiniz. Rol atama hakkında daha fazla bilgi için bkz. [rolleri kullanıcılara atama](active-directory-users-assign-role-azure-portal.md).

   - **İş bilgisi**: Kullanıcı hakkında buraya daha fazla bilgi ekleyebilir ya da daha sonra bunu yapabilirsiniz. Kullanıcı bilgilerini ekleme hakkında daha fazla bilgi için bkz. [Kullanıcı profili bilgilerini ekleme veya değiştirme](active-directory-users-profile-azure-portal.md).

1. **Parola** kutusunda belirtilen otomatik oluşturulan parolayı kopyalayın. İlk kez oturum açmak için kullanıcıya bu parolayı vermeniz gerekir.

1. **Oluştur**’u seçin.

Kullanıcı oluşturulup Azure AD kuruluşunuza eklenir.

## <a name="add-a-new-guest-user"></a>Yeni Konuk Kullanıcı ekleme

Ayrıca yeni **Kullanıcı** sayfasından **Kullanıcı davet et** ' i seçerek yeni Konuk kullanıcıyı kuruluşunuzla işbirliği yapmaya davet edebilirsiniz. Kuruluşunuzun dış işbirliği ayarları, konukları davet etmenize izin vermek üzere yapılandırılmışsa, Kullanıcı işbirliği yapmaya başlamak için kabul etmeleri gereken bir davete e-postayla gönderilir. B2B işbirliği kullanıcılarını davet etme hakkında daha fazla bilgi için bkz. [Azure ACTIVE DIRECTORY B2B kullanıcılarını davet etme](../b2b/add-users-administrator.md)

## <a name="add-a-consumer-user"></a>Tüketici kullanıcısı ekleme

Azure Active Directory B2C (Azure AD B2C) dizininizde tüketici hesaplarını el ile oluşturmak istediğiniz senaryolar olabilir. Tüketici hesapları oluşturma hakkında daha fazla bilgi için, bkz. [Azure AD B2C tüketici kullanıcılarını oluşturma ve silme](../../active-directory-b2c/manage-users-portal.md).

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Bir karma ortamda yeni bir kullanıcı ekleyin

Azure Active Directory (bulut) hem de Windows Server Active Directory (şirket) ile bir ortamınız varsa, var olan kullanıcı hesabı verileri eşitleyerek yeni kullanıcıları ekleyebilirsiniz. Karma ortamlar ve kullanıcılar hakkında daha fazla bilgi için bkz. Şirket [içi dizinlerinizi Azure Active Directory tümleştirme](../hybrid/whatis-hybrid-identity.md).

## <a name="delete-a-user"></a>Kullanıcı silme

Mevcut bir kullanıcının Azure Active Directory portalında kullanarak silebilirsiniz.

Bir kullanıcıyı silmek için şu adımları izleyin:

1. Kuruluşun kullanıcı yönetici hesabını kullanarak [Azure Portal](https://portal.azure.com/) oturum açın.

1. Herhangi bir sayfadan *Azure Active Directory* arayın ve seçin.

1. Azure AD kiracınızdan silmek istediğiniz kullanıcıyı arayın ve seçin. Örneğin, _Mary Parker_.

1. **Kullanıcı Sil**' i seçin.

    ![Kullanıcılar - tüm kullanıcılar sayfası ile vurgulanmış kullanıcıyı silme](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

Kullanıcı silinir ve artık **Kullanıcılar-tüm kullanıcılar** sayfasında görünmez. Kullanıcı **silinen kullanıcılar** sayfasında sonraki 30 gün boyunca görünebilir ve bu süre içinde geri yüklenebilir. Bir kullanıcıyı geri yükleme hakkında daha fazla bilgi için, bkz. [Azure Active Directory kullanarak son silinen bir kullanıcıyı geri yükleme veya kaldırma](active-directory-users-restore.md).

Bir Kullanıcı silindiğinde, Kullanıcı tarafından kullanılan tüm lisanslar diğer kullanıcılar için kullanılabilir hale getirilir.

>[!Note]
>Yetkili kaynağı Windows Server Active Directory olan kullanıcılar için kimliği, iletişim bilgilerini veya iş bilgilerini güncelleştirmek üzere Windows Server Active Directory kullanmanız gerekir. Güncelleştirme tamamlandıktan sonra değişiklikleri görürsünüz önce tamamlanması için bir sonraki eşitleme döngüsü beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcılarınızı ekledikten sonra, aşağıdaki temel süreçler gerçekleştirebilirsiniz:

- [Profil bilgilerini ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Temel Grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Dinamik gruplarla ve kullanıcılarla çalışma](../users-groups-roles/groups-create-rule.md)

Ya da [başka bir dizinden Konuk kullanıcılar ekleme](../b2b/what-is-b2b.md) veya [silinen bir kullanıcıyı geri yükleme](active-directory-users-restore.md)gibi diğer kullanıcı yönetim görevlerini gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz. [Kullanıcı yönetimi belgelerini Azure Active Directory](../users-groups-roles/index.yml).
