---
title: Kullanıcılara dizin rolleri atama - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Active Directory'si olan kullanıcılara yönetici ve yönetici olmayan rollerin nasıl atanacağına ilişkin yönergeler.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422934"
---
# <a name="assign-administrator-and-non-administrator-roles-to-users-with-azure-active-directory"></a>Azure Etkin Dizini olan kullanıcılara yönetici ve yönetici olmayan roller atama
Kuruluşunuzdaki bir kullanıcının Azure Etkin Dizin (Azure AD) kaynaklarını yönetmek için izin alması gerekiyorsa, kullanıcının gerçekleştirmek için izin alması gereken eylemlere bağlı olarak kullanıcıya Azure AD'de uygun bir rol atamanız gerekir.

Kullanılabilir roller hakkında daha fazla bilgi için Azure [Etkin Dizini'nde yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md)'ya bakın. Kullanıcı ekleme hakkında daha fazla bilgi için [bkz.](add-users-azure-active-directory.md)

## <a name="assign-roles"></a>Rolleri atama
Azure AD rollerini bir kullanıcıya atamanın yaygın bir yolu, bir kullanıcının **Dizin rol** sayfasındadır.

Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) kullanarak da roller atayabilirsiniz. PIM'nin nasıl kullanılacağı hakkında daha ayrıntılı bilgi için [Ayrıcalıklı Kimlik Yönetimi'ne](https://docs.microsoft.com/azure/active-directory/privileged-identity-management)bakın.

### <a name="to-assign-a-role-to-a-user"></a>Bir kullanıcıya rol atamak için
1. [Azure portalına](https://portal.azure.com/) gidin ve dizin için Global yönetici hesabını kullanarak oturum açın. 

2. **Azure Active Directory**'yi bulun ve seçin.

      ![Azure Active Directory için Azure portalı araması](media/active-directory-users-assign-role-azure-portal/search-azure-active-directory.png)


3. **Kullanıcılar**’ı seçin.

4. Rol atamasını alan kullanıcıyı arayın ve seçin. Örneğin, _Alain Charon._

      ![Tüm kullanıcılar sayfası - kullanıcı seçin](media/active-directory-users-assign-role-azure-portal/directory-role-select-user.png)

5. **Alain Charon - Profil** **sayfasında, Atanan rolleri**seçin.

    **Alain Charon - Dizin rol** sayfası görüntülenir.

6. **Atama Ekle'yi**seçin, Alain'e atamak için rolü seçin (örneğin, _Uygulama yöneticisi),_ ve sonra **Seç'i**seçin.

    ![Atanan roller sayfası - seçili rolü gösterme](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    Uygulama yöneticisi rolü Alain Charon'a atanır ve **Alain Charon - Dizin rol** sayfasında görünür.

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma
Rol atamasını bir kullanıcıdan kaldırmanız gerekiyorsa, bunu **Alain Charon - Dizin rol** sayfasından da yapabilirsiniz.

### <a name="to-remove-a-role-assignment-from-a-user"></a>Bir rol atamasını kullanıcıdan kaldırmak için

1. **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından rol atamasını kaldıran kullanıcıyı arayın ve seçin. Örneğin, _Alain Charon._

2. **Atanan rolleri**seçin, **Uygulama yöneticisini**seçin ve sonra **atamayı kaldır'ı**seçin.

    ![Seçili rolü ve kaldırma seçeneğini gösteren atanmış roller sayfası](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    Uygulama yöneticisi rolü Alain Charon kaldırılır ve artık **Alain Charon - Dizin rol** sayfasında görünür.

## <a name="next-steps"></a>Sonraki adımlar
- [Kullanıcı ekleme veya silme](add-users-azure-active-directory.md)

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Başka bir dizinden konuk kullanıcılar ekleme](../b2b/what-is-b2b.md)

Veya temsilci atama, ilkeleri kullanma ve kullanıcı hesaplarını paylaşma gibi diğer kullanıcı yönetimi görevlerini gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için [Azure Etkin Dizin kullanıcı yönetimi belgelerine](../users-groups-roles/index.yml)bakın.


