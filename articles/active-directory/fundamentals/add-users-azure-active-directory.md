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
ms.openlocfilehash: b01eb7730290fbf7340fc0a6d8cac8157498f64a
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74013628"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>Ekleme veya Azure Active Directory kullanarak kullanıcı silme

Yeni kullanıcılar ekleyin veya Azure Active Directory (Azure AD) kuruluşunuzdaki mevcut kullanıcıları silin. Kullanıcıları eklemek veya silmek için Kullanıcı Yöneticisi veya genel yönetici olmanız gerekir.

## <a name="add-a-new-user"></a>Yeni kullanıcı ekleme

Azure Active Directory portalı kullanarak yeni bir kullanıcı oluşturabilir.

### <a name="to-add-a-new-user"></a>Yeni bir kullanıcı eklemek için

1. [Azure Portal](https://portal.azure.com/) , kuruluş için Kullanıcı Yöneticisi olarak oturum açın.

2. Seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **yeni kullanıcı**.

    ![Kullanıcılar - yeni kullanıcı ile vurgulanmış tüm kullanıcılar sayfası](media/add-users-azure-active-directory/new-user-all-users-blade.png)

3. **Yeni Kullanıcı** sayfasında **Kullanıcı oluştur** ' u seçin ve ardından kullanıcının bilgilerini ekleyin.

    ![Yeni kullanıcı, kullanıcı bilgileri kullanıcının Sayfası Ekle](media/add-users-azure-active-directory/new-user-user-blade.png)

   - **Ad (gerekli)** : yeni kullanıcının adı ve soyadı. Örneğin, Chris yeşil.

   - **Kullanıcı adı (gerekli)** : yeni kullanıcının Kullanıcı adı. Örneğin, chris@contoso.com.

     Kullanıcı adının etki alanı bölümü, contoso.com gibi Azure AD kuruluşunuzda ilk varsayılan etki alanı adını, <_etkialanıadınız_>. onmicrosoft. com ' u veya özel bir etki alanı adını kullanmalıdır. Kullanılabilir etki alanları listesinden seçim yapabilirsiniz. Ayrıca, etki alanı adının bir bölümünü yazarak listeye filtre uygulayabilirsiniz. Özel etki alanı adı oluşturma hakkında daha fazla bilgi için bkz. [Azure Active Directory'ye özel etki alanı adı ekleme](add-custom-domain.md).

   - **Gruplar**: kullanıcıyı bir veya daha fazla var olan gruba ekleyebilir veya daha sonra yapabilirsiniz. Kullanıcı grupları ekleme hakkında daha fazla bilgi için bkz. [temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md).

   - **Dizin rolü**: Kullanıcı IÇIN Azure AD Yönetim izinlerine ihtiyacınız varsa, bunları BIR Azure AD rolüne ekleyebilirsiniz. Kullanıcıyı bir genel yönetici veya Azure AD 'de sınırlı bir veya daha fazla yönetici rolünden atayabilirsiniz. Rol atama hakkında daha fazla bilgi için bkz. [kullanıcılara roller atama](active-directory-users-assign-role-azure-portal.md).

   - **İş bilgisi**: Kullanıcı hakkında buraya daha fazla bilgi ekleyebilir ya da daha sonra bunu yapabilirsiniz. Kullanıcı bilgileri ekleme hakkında daha fazla bilgi için bkz. [eklemek veya kullanıcı profili bilgilerini değiştirmek nasıl](active-directory-users-profile-azure-portal.md).

4. Sağlanan otomatik olarak oluşturulan parola kopyalama **parola** kutusu. Parola kutusunda belirtilen otomatik oluşturulan parolayı kullanmayı veya özel bir parola oluşturmayı seçebilirsiniz. İlk oturum açma işlemi için bu parolayı kullanıcıya vermeniz gerekir.

5. **Oluştur**'u seçin.

Kullanıcı oluşturulup Azure AD kuruluşunuza eklenir.

## <a name="add-a-new-guest-user"></a>Yeni Konuk Kullanıcı ekleme

Ayrıca yeni **Kullanıcı** sayfasından **Kullanıcı davet et** ' i seçerek yeni Konuk kullanıcıyı kuruluşunuzla işbirliği yapmaya davet edebilirsiniz. Kuruluşunuzun dış işbirliği ayarları, konukları davet etmenize izin vermek üzere yapılandırılmışsa, Kullanıcı işbirliği yapmaya başlamak için kabul etmeleri gereken bir davete e-postayla gönderilir. B2B işbirliği kullanıcılarını davet etme hakkında daha fazla bilgi için bkz. [Azure ACTIVE DIRECTORY B2B kullanıcılarını davet etme](../b2b/add-users-administrator.md)

## <a name="add-a-consumer-user"></a>Tüketici kullanıcısı ekleme

Azure Active Directory B2C (Azure AD B2C) dizininizde tüketici hesaplarını el ile oluşturmak istediğiniz senaryolar olabilir. Tüketici hesapları oluşturma hakkında daha fazla bilgi için, bkz. [Azure AD B2C tüketici kullanıcılarını oluşturma ve silme](../../active-directory-b2c/manage-users-portal.md).

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Bir karma ortamda yeni bir kullanıcı ekleyin

Azure Active Directory (bulut) hem de Windows Server Active Directory (şirket) ile bir ortamınız varsa, var olan kullanıcı hesabı verileri eşitleyerek yeni kullanıcıları ekleyebilirsiniz. Karma ortamlar ve kullanıcılar hakkında daha fazla bilgi için bkz. [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md).

## <a name="delete-a-user"></a>Kullanıcı silme

Mevcut bir kullanıcının Azure Active Directory portalında kullanarak silebilirsiniz.

### <a name="to-delete-a-user"></a>Kullanıcı silme

1. Kuruluşun kullanıcı yönetici hesabını kullanarak [Azure Portal](https://portal.azure.com/) oturum açın.

1. Seçin **Azure Active Directory**seçin **kullanıcılar**, arayın ve Azure AD kiracınızdan silmek istediğiniz kullanıcıyı seçin. Örneğin, _Mary Parker_.

1. Seçin **kullanıcıyı silme**.

    ![Kullanıcılar - tüm kullanıcılar sayfası ile vurgulanmış kullanıcıyı silme](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    Kullanıcı silindi ve artık görünür **kullanıcılar - tüm kullanıcılar** sayfası. Kullanıcı görülebilir **silinmiş kullanıcılarını** sayfa sonraki 30 gün boyunca ve bu süre içerisinde geri yüklenebilir. Bir kullanıcıyı geri yükleme hakkında daha fazla bilgi için bkz. [geri yüklemek veya yakın zamanda silinen bir kullanıcıyı kalıcı olarak kaldırmak nasıl](active-directory-users-restore.md). Bir Kullanıcı silindiğinde, Kullanıcı tarafından kullanılan tüm lisanslar diğer kullanıcıların kullanımına sunulur.

    >[!Note]
    >Windows Server Active Directory, kimlik, iletişim bilgileri veya Windows Server Active Directory, yetki kaynağı olan kullanıcılar için iş bilgileri güncelleştirmek için kullanmanız gerekir. Güncelleştirme tamamlandıktan sonra değişiklikleri görürsünüz önce tamamlanması için bir sonraki eşitleme döngüsü beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcılarınızın ekledikten sonra aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Dinamik gruplar ve kullanıcılar ile çalışma](../users-groups-roles/groups-create-rule.md)

Ya da [başka bir Azure AD kuruluştan Konuk kullanıcılar ekleme](../b2b/what-is-b2b.md) veya [silinen bir kullanıcıyı geri yükleme](active-directory-users-restore.md)gibi diğer kullanıcı yönetim görevlerini gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz: [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).
