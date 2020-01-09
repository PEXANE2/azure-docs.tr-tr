---
title: Kullanıcı profili bilgilerini ekleme veya güncelleştirme-Azure AD
description: Azure Active Directory'de bir resim ve iş ayrıntılar dahil olmak üzere, bir kullanıcının profilini bilgi ekleme hakkında yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ba51b0ad7f314058f34092dff4e26411020086a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75422873"
---
# <a name="add-or-update-a-users-profile-information-using-azure-active-directory"></a>Ekleme veya Azure Active Directory'yi kullanarak kullanıcının profil bilgilerini güncelleştirme
Kullanıcı profili bilgilerini, profil resminizi dahil olmak üzere, işe özgü bilgileri ve Azure Active Directory (Azure AD) kullanarak bazı ayarları ekleyin. Yeni kullanıcı ekleme hakkında daha fazla bilgi için bkz. [ekleyin veya Azure Active Directory'de kullanıcı silme](add-users-azure-active-directory.md).

## <a name="add-or-change-profile-information"></a>Profil bilgileri ekleme veya değiştirme
Gördüğünüz gibi yok daha fazla bilgi bir kullanıcının profilinde daha kullanıcının oluşturma sırasında ekleyebilirsiniz. Bu ek bilgiler isteğe bağlıdır ve kuruluşunuz tarafından gereken şekilde eklenebilir.

## <a name="to-add-or-change-profile-information"></a>Ekleme veya profil bilgilerini değiştirme
1. [Azure Portal](https://portal.azure.com/) , kuruluş için Kullanıcı Yöneticisi olarak oturum açın.

2. Seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından bir kullanıcı seçin. Örneğin, _Alain Charon_.

    **Alain Charon - profili** sayfası görüntülenir.

    ![Kullanıcının profil sayfasını düzenlenebilir bilgiler dahil olmak üzere,](media/active-directory-users-profile-azure-portal/user-profile-all-blade.png)

3. Seçin **Düzenle** isteğe bağlı olarak ekleyin ya da her bir kullanılabilir bölümler dahil bilgileri güncelleştirin.

    ![Kullanıcının profil sayfasını, düzenlenebilir alanları gösterme](media/active-directory-users-profile-azure-portal/user-profile-edit.png)

    - **Profil resmi.** Kullanıcı hesabı için bir küçük resim görüntüsünü seçin. Bu resim, Azure Active Directory ve myapps.microsoft.com sayfası gibi kullanıcının kişisel sayfalarında görünür.

    - **Kimlik.** Kullanıcı için evli soyadı gibi ek bir kimlik değeri ekleyin veya güncelleştirin. Bu adı, adı ve soyadı değerlerini bağımsız olarak ayarlayabilirsiniz. Örneğin, bunu, harfleri, bir şirket adını veya gösterilen adların sırasını değiştirmek için kullanabilirsiniz. Başka bir örnekte, adları ' Chris Green ' olan iki kullanıcı için kimlik dizesini kullanarak adlarını ' Chris B. yeşil ' ' Chris R. Green (contoso) olarak ayarlayabilirsiniz.

    - **İş bilgisi.** Kullanıcının iş unvanı, bölüme veya Yöneticisi gibi tüm iş ile ilgili bilgileri ekleyin.

    - **Ayarlar.** Kullanıcının Azure Active Directory kiracısı ile oturum olup olmadığını belirleyin. Ayrıca, kullanıcının genel konum belirtebilirsiniz.

    - **İletişim bilgileri.** Bazı kullanıcıların telefonu veya mobil iletişim bilgileri dışında, kullanıcının ilgili iletişim bilgilerini ekleyin (yalnızca bir genel yönetici Yönetici rollerindeki kullanıcılar için güncelleştirebilir).

    - **Kimlik doğrulaması iletişim bilgileri.** Bir kullanıcı için etkin bir telefon numarası ve e-posta adresi olduğundan emin olmak için bu bilgileri doğrulayın. Bu bilgiler, kullanıcının gerçekten oturum açma sırasında kullanıcı olduğundan emin olmak için Azure Active Directory tarafından kullanılır. Kimlik doğrulaması iletişim bilgileri, yalnızca bir genel yönetici tarafından güncelleştirilebilir.

4. **Kaydet**’i seçin.

    Kullanıcı için yaptığınız tüm değişiklikler kaydedilir.

    >[!Note]
    >Windows Server Active Directory, kimlik, iletişim bilgileri veya Windows Server Active Directory, yetki kaynağı olan kullanıcılar için iş bilgileri güncelleştirmek için kullanmanız gerekir. Güncelleştirme tamamlandıktan sonra değişiklikleri görürsünüz önce tamamlanması için bir sonraki eşitleme döngüsü beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcılarınızın profilleri güncelleştirdikten sonra aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Ekleme veya kullanıcıları Sil](add-users-azure-active-directory.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

Veya temsilci atayarak, ilkeleri kullanarak ve kullanıcı hesapları paylaşma gibi diğer kullanıcı yönetim görevlerini gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için bkz: [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).
