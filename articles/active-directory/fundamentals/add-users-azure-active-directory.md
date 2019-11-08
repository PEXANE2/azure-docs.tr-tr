---
title: Kullanıcı ekleme veya silme-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kullanarak yeni kullanıcıların nasıl ekleneceği veya mevcut kullanıcıların nasıl silineceği hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: e98b09d8b07c625613e3be149e64ac8f06adc089
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73805555"
---
# <a name="add-or-delete-users-using-azure-active-directory"></a>Azure Active Directory kullanarak Kullanıcı ekleme veya silme
Yeni kullanıcılar ekleyin veya Azure Active Directory (Azure AD) kuruluşunuzdaki mevcut kullanıcıları silin. Kullanıcıları eklemek veya silmek için Kullanıcı Yöneticisi veya genel yönetici olmanız gerekir. 

## <a name="add-a-new-user"></a>Yeni Kullanıcı Ekle
Azure Active Directory portalını kullanarak yeni bir kullanıcı oluşturabilirsiniz.

### <a name="to-add-a-new-user"></a>Yeni bir kullanıcı eklemek için
1. [Azure Portal](https://portal.azure.com/) , kuruluş için Kullanıcı Yöneticisi olarak oturum açın.

2. **Azure Active Directory**seçin, **Kullanıcılar**' ı seçin ve ardından **Yeni Kullanıcı**' yı seçin.

    ![Kullanıcılar-yeni Kullanıcı vurgulanmış tüm kullanıcılar sayfası](media/add-users-azure-active-directory/new-user-all-users-blade.png)

3. **Kullanıcı** sayfasında gerekli bilgileri doldurun.

    ![Kullanıcı bilgileriyle Yeni Kullanıcı, Kullanıcı sayfası ekleme](media/add-users-azure-active-directory/new-user-user-blade.png)

   - **Ad (gerekli).** Yeni kullanıcının adı ve soyadı. Örneğin, Mary Parker.

   - **Kullanıcı adı (gerekli).** Yeni kullanıcının Kullanıcı adı. Örneğin, mary@contoso.com.
    
       Kullanıcı adının etki alanı bölümü, ilk varsayılan etki alanı adını, <_etkialanıadınız_>. onmicrosoft. com veya contoso.com gibi özel bir etki alanı adını kullanmalıdır. Özel etki alanı adı oluşturma hakkında daha fazla bilgi için bkz. [Azure Active Directory için özel etki alanı adı ekleme](add-custom-domain.md).

   - **Profilinizi.** İsteğe bağlı olarak, Kullanıcı hakkında daha fazla bilgi ekleyebilirsiniz. Ayrıca, daha sonra Kullanıcı bilgilerini ekleyebilirsiniz. Kullanıcı bilgilerini ekleme hakkında daha fazla bilgi için bkz. [Kullanıcı profili bilgilerini ekleme veya değiştirme](active-directory-users-profile-azure-portal.md).

   - **Gruplandıran.** İsteğe bağlı olarak, kullanıcıyı bir veya daha fazla var olan gruba ekleyebilirsiniz. Kullanıcıyı daha sonra gruplara da ekleyebilirsiniz. Gruplara kullanıcı ekleme hakkında daha fazla bilgi için bkz. [temel Grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md).

   - **Dizin rolü.** İsteğe bağlı olarak, kullanıcıyı bir Azure AD yönetici rolüne ekleyebilirsiniz. Kullanıcıyı bir genel yönetici veya Azure AD 'de sınırlı bir veya daha fazla yönetici rolünden atayabilirsiniz. Rol atama hakkında daha fazla bilgi için bkz. [rolleri kullanıcılara atama](active-directory-users-assign-role-azure-portal.md).

4. **Parola** kutusunda belirtilen otomatik oluşturulan parolayı kopyalayın. İlk oturum açma işlemi için kullanıcıya bu parolayı vermeniz gerekir.

5. **Oluştur**'u seçin.

    Kullanıcı oluşturulup Azure AD kiracınıza eklenir.

## <a name="add-a-new-user-within-a-hybrid-environment"></a>Karma ortamda yeni bir kullanıcı ekleme
Hem Azure Active Directory (bulut) hem de Windows Server Active Directory (Şirket içi) olan bir ortamınız varsa, mevcut kullanıcı hesabı verilerini eşitleyerek yeni kullanıcılar ekleyebilirsiniz. Karma ortamlar ve kullanıcılar hakkında daha fazla bilgi için bkz. Şirket [içi dizinlerinizi Azure Active Directory tümleştirme](../hybrid/whatis-hybrid-identity.md).

## <a name="delete-a-user"></a>Kullanıcı silme
Azure Active Directory portalını kullanarak var olan bir kullanıcıyı silebilirsiniz.

### <a name="to-delete-a-user"></a>Bir kullanıcıyı silmek için
1. Kuruluşun kullanıcı yönetici hesabını kullanarak [Azure Portal](https://portal.azure.com/) oturum açın.

2. **Azure Active Directory**' i seçin, **Kullanıcılar**' ı SEÇIN ve ardından Azure AD kiracınızdan silmek istediğiniz kullanıcıyı bulun ve seçin. Örneğin, _Mary Parker_.

3. **Kullanıcı Sil**' i seçin.

    ![Kullanıcılar-silme Kullanıcı vurgulanmış tüm kullanıcılar sayfası](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    Kullanıcı silinir ve artık **Kullanıcılar-tüm kullanıcılar** sayfasında görünmez. Kullanıcı **silinen kullanıcılar** sayfasında sonraki 30 gün boyunca görünebilir ve bu süre içinde geri yüklenebilir. Bir kullanıcıyı geri yükleme hakkında daha fazla bilgi için bkz. [son silinen kullanıcıyı geri yükleme veya kalıcı olarak kaldırma](active-directory-users-restore.md). Bir Kullanıcı silindiğinde, Kullanıcı tarafından kullanılan tüm lisanslar diğer kullanıcıların kullanımına sunulur.

    >[!Note]
    >Yetkili kaynağı Windows Server Active Directory olan kullanıcılar için kimliği, iletişim bilgilerini veya iş bilgilerini güncelleştirmek üzere Windows Server Active Directory kullanmanız gerekir. Güncelleştirmenizi tamamladıktan sonra, değişiklikleri görebilmeniz için bir sonraki eşitleme döngüsünün tamamlanmasını beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcılarınızı ekledikten sonra, aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Profil bilgilerini ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Temel Grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Dinamik gruplarla ve kullanıcılarla çalışma](../users-groups-roles/groups-create-rule.md)

Ya da [başka bir dizinden Konuk kullanıcılar ekleme](../b2b/what-is-b2b.md) veya [silinen bir kullanıcıyı geri yükleme](active-directory-users-restore.md)gibi diğer kullanıcı yönetim görevlerini de gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz. [Kullanıcı yönetimi belgelerini Azure Active Directory](../users-groups-roles/index.yml).
