---
title: Grup bilgilerinizi edin - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Active Directory'yi kullanarak grubunuzun bilgilerini nasıl edinacağınız hakkında talimatlar.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc06abca08b2522ac57552e85f7c1bac3ef854af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68561886"
---
# <a name="edit-your-group-information-using-azure-active-directory"></a>Azure Etkin Dizini'ni kullanarak grup bilgilerinizi edin

Azure Etkin Dizini 'ni (Azure AD) kullanarak, bir grubun adını, açıklamasını veya üyelik türünü güncelleştirmeyi de içeren ayarlarını değiştirebilirsiniz.

## <a name="to-edit-your-group-settings"></a>Grup ayarlarınızı yeniden yapmak için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Etkin Dizini'ni**seçin ve ardından **Gruplar'ı**seçin.

    Tüm gruplar - **Tüm gruplar** sayfası görüntülenir ve tüm etkin gruplarınızı gösterir.

3. Gruplar **- Tüm gruplar** sayfasından, grup adının olabildiğince çoğunu **Arama** kutusuna yazın. Bu makalenin amaçları için, **MDM politikasını** arıyoruz - Batı grubu.

    Arama **sonuçları,** siz daha fazla karakter yazdıkça güncelleyerek Arama kutusunun altında görünür.

    ![Arama kutusunda arama metni bulunan tüm gruplar sayfası](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Grup **MDM ilkesini**seçin - Batı ve ardından **Yönet** alanından **Özellikler'i** seçin.

    ![Üye seçeneği ve bilgilerin vurgulandığı Grup Genel Bakış sayfası](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Genel **ayarlar** bilgilerini gerektiği gibi güncelleştirin:

    ![Bir grubun özellikleri ayarları](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Grup adı.** Varolan grup adını edin.
    
    - **Grup açıklaması.** Varolan grup açıklamasını edin.

    - **Grup türü.** Oluşturulduktan sonra grubun türünü değiştiremezsiniz. **Grup türünü**değiştirmek için grubu silmeniz ve yeni bir grup oluşturmanız gerekir.
    
    - **Üyelik türü.** Üyelik türünü değiştirin. Kullanılabilir çeşitli üyelik türleri hakkında daha fazla bilgi için [bkz: Temel bir grup oluşturun ve Azure Active Directory portalını kullanarak üye ekleyin.](active-directory-groups-create-azure-portal.md)
    
    - **Nesne Kimliği.** Nesne Kimliğini değiştiremezsiniz, ancak grup için PowerShell komutlarınızda kullanmak üzere kopyalayabilirsiniz. PowerShell cmdlets kullanma hakkında daha fazla bilgi için [grup ayarlarını yapılandırmak için Azure Active Directory cmdlets'e](../users-groups-roles/groups-settings-v2-cmdlets.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Bir gruptan üye ekleme veya kaldırma](active-directory-groups-members-azure-portal.md)

- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-create-rule.md)

- [Bir grubun üyeliklerini yönetme](active-directory-groups-membership-azure-portal.md)

- [Grupları kullanarak kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Azure Etkin Dizini'ne Azure aboneliği ni ilişkilendirme veya ekleme](active-directory-how-subscriptions-associated-directory.md)
