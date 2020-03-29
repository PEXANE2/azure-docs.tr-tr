---
title: Kullanıcı profili bilgileri ekleme veya güncelleme - Azure AD
description: Resim ve iş ayrıntıları da dahil olmak üzere Azure Active Directory'de bir kullanıcının profiline nasıl bilgi eklenacağına ilişkin talimatlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422873"
---
# <a name="add-or-update-a-users-profile-information-using-azure-active-directory"></a>Azure Active Directory kullanarak kullanıcının profil bilgilerini ekleme veya güncelleştirme
Azure Etkin Dizini (Azure AD) kullanarak profil resmi, işe özel bilgiler ve bazı ayarlar dahil olmak üzere kullanıcı profili bilgileri ekleyin. Yeni kullanıcı ekleme hakkında daha fazla bilgi için [Azure Etkin Dizini'nde kullanıcıların nasıl ekleyeceğiniz veya silindiğini](add-users-azure-active-directory.md)öğrenin.

## <a name="add-or-change-profile-information"></a>Profil bilgileri ekleme veya değiştirme
Göreceğiniz gibi, bir kullanıcının profilinde, kullanıcının oluşturulması sırasında ekleyebileceğinden daha fazla bilgi vardır. Tüm bu ek bilgiler isteğe bağlıdır ve kuruluşunuz tarafından gerektiği gibi eklenebilir.

## <a name="to-add-or-change-profile-information"></a>Profil bilgilerini eklemek veya değiştirmek için
1. Kuruluş için Kullanıcı yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açın.

2. **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından bir kullanıcı seçin. Örneğin, _Alain Charon._

    **Alain Charon - Profil** sayfası görüntülenir.

    ![Değiştirilebilir bilgiler de dahil olmak üzere kullanıcının profil sayfası](media/active-directory-users-profile-azure-portal/user-profile-all-blade.png)

3. Kullanılabilir bölümlerin her birinde yer alan bilgileri isteğe bağlı olarak eklemek veya güncellemek için **Edit'i** seçin.

    ![Kullanıcının profil sayfası, değiştirilebilir alanları gösteren](media/active-directory-users-profile-azure-portal/user-profile-edit.png)

    - **Profil resmi.** Kullanıcının hesabı için küçük resim resmi seçin. Bu resim Azure Etkin Dizin'de ve kullanıcının myapps.microsoft.com sayfası gibi kişisel sayfalarında görünür.

    - **Kimlik.** Kullanıcı için evli bir soyadı gibi ek bir kimlik değeri ekleyin veya güncelleştirin. Bu adı Ad ve Soyad değerlerinden bağımsız olarak ayarlayabilirsiniz. Örneğin, baş harfleri, şirket adını eklemek veya gösterilen ad sırasını değiştirmek için kullanabilirsiniz. Başka bir örnekte, adları 'Chris Green' olan iki kullanıcı için adlarını 'Chris B. Green' 'Chris R. Green (Contoso) olarak ayarlamak için Kimlik dizesini kullanabilirsiniz.

    - **İş bilgisi.** Kullanıcının iş unvanı, departman veya yöneticisi gibi işle ilgili bilgileri ekleyin.

    - **Ayarlar.** Kullanıcının Azure Etkin Dizin kiracısında oturum açıp açmayacağına karar verin. Ayrıca, kullanıcının genel konumunu da belirtebilirsiniz.

    - **İletişim bilgileri.** Bazı kullanıcının telefon veya mobil kişi bilgileri dışında kullanıcı için ilgili iletişim bilgilerini ekleyin (yalnızca genel bir yönetici yönetici rollerindeki kullanıcılar için güncellenebilir).

    - **Kimlik doğrulama iletişim bilgileri.** Kullanıcı için etkin bir telefon numarası ve e-posta adresi olduğundan emin olmak için bu bilgileri doğrulayın. Bu bilgiler, oturum açma sırasında kullanıcının gerçekten kullanıcı olduğundan emin olmak için Azure Active Directory tarafından kullanılır. Kimlik doğrulama kişi bilgileri yalnızca genel bir yönetici tarafından güncellenebilir.

4. **Kaydet'i**seçin.

    Tüm değişiklikleriniz kullanıcı için kaydedilir.

    >[!Note]
    >Yetki kaynağı Windows Server Active Directory olan kullanıcıların kimliğini, iletişim bilgilerini veya iş bilgilerini güncelleştirmek için Windows Server Active Directory'yi kullanmanız gerekir. Güncelleştirmenizi tamamladıktan sonra, değişiklikleri görmeden önce bir sonraki eşitleme döngüsünün tamamlanmasını beklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcılarınızın profillerini güncelledikten sonra aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Kullanıcı ekleme veya silme](add-users-azure-active-directory.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

Veya temsilci atama, ilkeleri kullanma ve kullanıcı hesaplarını paylaşma gibi diğer kullanıcı yönetimi görevlerini gerçekleştirebilirsiniz. Diğer kullanılabilir eylemler hakkında daha fazla bilgi için [Azure Etkin Dizin kullanıcı yönetimi belgelerine](../users-groups-roles/index.yml)bakın.
