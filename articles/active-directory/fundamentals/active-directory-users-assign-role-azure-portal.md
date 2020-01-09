---
title: Kullanıcılar - Azure Active Directory dizin rolleri atama | Microsoft Docs
description: Azure Active Directory ile kullanıcılara yönetici ve yönetici olmayan rol atama hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2df52969ea79e5d1af132aa82c2ec1ceedb92b82
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75422934"
---
# <a name="assign-administrator-and-non-administrator-roles-to-users-with-azure-active-directory"></a>Azure Active Directory ile kullanıcılara yönetici ve yönetici olmayan rol atayın
Kuruluşunuzdaki bir kullanıcı Azure Active Directory (Azure AD) kaynakları yönetme izni gerekiyorsa, kullanıcının uygun bir rol Azure AD'de kullanıcının gerçekleştirme izni gereken eylemler temelinde atamanız gerekir.

Kullanılabilir roller hakkında daha fazla bilgi için bkz: [Azure Active Directory'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md). Kullanıcı ekleme hakkında daha fazla bilgi için bkz. [Azure Active Directory'ye yeni kullanıcı ekleme](add-users-azure-active-directory.md).

## <a name="assign-roles"></a>Rol atama
Bir kullanıcıya Azure AD rolleri atamak için yaygın bir yolu açıktır **dizin rolü** sayfasında bir kullanıcı için.

Ayrıca, Privileged Identity Management (PIM) kullanarak rolleri atayabilirsiniz. PIM kullanma hakkında daha ayrıntılı bilgi için bkz: [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management).

### <a name="to-assign-a-role-to-a-user"></a>Bir kullanıcıya rol atamak için
1. [Azure Portal](https://portal.azure.com/) gidin ve Dizin Için bir genel yönetici hesabı kullanarak oturum açın. 

2. Arama yapın ve **Azure Active Directory**seçin.

      ![Azure Active Directory arama Azure portal](media/active-directory-users-assign-role-azure-portal/search-azure-active-directory.png)


3. **Kullanıcılar**’ı seçin.

4. Rol atamasını almak için arama yapın ve kullanıcıyı seçin. Örneğin, _Alain Charon_.

      ![Tüm kullanıcılar sayfası-kullanıcı seçin](media/active-directory-users-assign-role-azure-portal/directory-role-select-user.png)

5. **Alain Charon profili** sayfasında **atanan roller**' i seçin.

    **Alain Charon - dizin rolü** sayfası görüntülenir.

6. **Atama Ekle**' yi seçin, Alain atanacak rolü seçin (örneğin, _Uygulama Yöneticisi_) ve ardından **Seç**' i seçin.

    ![Atanan roller sayfası-seçili rol gösteriliyor](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    Alain Charon için uygulama Yönetici rolüne atanır ve göründüğü **Alain Charon - dizin rolü** sayfası.

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma
Bir kullanıcıdan rol atamasını kaldırmanız gerekirse, ayrıca, bunu yapabilirsiniz **Alain Charon - dizin rolü** sayfası.

### <a name="to-remove-a-role-assignment-from-a-user"></a>Bir kullanıcıdan bir rol atamasını kaldırmak için

1. Seçin **Azure Active Directory**seçin **kullanıcılar**, arayın ve kaldırılan bir rol ataması alma kullanıcıyı seçin. Örneğin, _Alain Charon_.

2. **Atanan roller**' i seçin, **Uygulama Yöneticisi**' ni seçin ve **Atamayı Kaldır**' ı seçin.

    ![Atanan roller sayfası ve seçili rol ve Kaldır seçeneği gösteriliyor](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    Uygulama Yöneticisi rolü Alain Charon ' kaldırılır ve artık göründüğü **Alain Charon - dizin rolü** sayfası.

## <a name="next-steps"></a>Sonraki adımlar
- [Ekleme veya kullanıcıları Sil](add-users-azure-active-directory.md)

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Başka bir dizinden konuk kullanıcılar ekleme](../b2b/what-is-b2b.md)

Veya temsilci atayarak, ilkeleri kullanarak ve kullanıcı hesapları paylaşma gibi diğer kullanıcı yönetim görevlerini gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz: [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).


