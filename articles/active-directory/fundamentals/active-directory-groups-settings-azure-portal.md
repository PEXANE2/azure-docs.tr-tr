---
title: Grup bilgilerinizi düzenleme-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kullanarak grubunuzun bilgilerini düzenleme yönergeleri.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 08/27/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 410570798e468244763c05b3275801427dc4b2fd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85604242"
---
# <a name="edit-your-group-information-using-azure-active-directory"></a>Azure Active Directory kullanarak Grup bilgilerinizi düzenleme

Azure Active Directory (Azure AD) kullanarak, adını, açıklamasını veya üyelik türünü güncelleştirmek dahil olmak üzere bir grubun ayarlarını düzenleyebilirsiniz.

## <a name="to-edit-your-group-settings"></a>Grup ayarlarınızı düzenlemek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**' yi seçin ve ardından **gruplar**' ı seçin.

    Tüm etkin gruplarınızı gösteren **gruplar-tüm gruplar** sayfası açılır.

3. **Gruplar-tüm gruplar** sayfasında, **arama** kutusuna, Grup adının çoğunu yazın. Bu makalenin amaçları doğrultusunda **MDM ilkesi-Batı** grubunu arıyoruz.

    **Arama sonuçları** , daha fazla karakter yazarken, ' ın altında görüntülenir.

    ![Tüm gruplar sayfası, arama kutusunda arama metniyle](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. **MDM ilkesi-Batı**grubunu seçin ve ardından **Yönet** alanından **Özellikler** ' i seçin.

    ![Gruba genel bakış sayfası, üye seçeneği ve bilgiler vurgulandı](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Aşağıdakiler dahil olmak üzere **Genel ayarlar** bilgilerini gerektiği şekilde güncelleştirin:

    ![Bir grubun Özellikler ayarları](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Grup adı.** Var olan grup adını düzenleyin.
    
    - **Grup açıklaması.** Mevcut Grup açıklamasını düzenleyin.

    - **Grup türü.** Oluşturulduktan sonra grup türünü değiştiremezsiniz. **Grup türünü**değiştirmek için, grubu silip yeni bir tane oluşturmanız gerekir.
    
    - **Üyelik türü.** Üyelik türünü değiştirin. Kullanılabilir çeşitli üyelik türleri hakkında daha fazla bilgi için bkz. [nasıl yapılır: temel Grup oluşturma ve Azure Active Directory portalını kullanarak üye ekleme](active-directory-groups-create-azure-portal.md).
    
    - **Nesne KIMLIĞI.** Nesne KIMLIĞINI değiştiremezsiniz, ancak gruba ait PowerShell komutlarınız için kullanmak üzere kopyalayabilirsiniz. PowerShell cmdlet 'leri kullanma hakkında daha fazla bilgi için bkz. [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet 'leri](../users-groups-roles/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Gruba üye ekleme veya gruptan üye kaldırma](active-directory-groups-members-azure-portal.md)

- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-create-rule.md)

- [Bir grubun üyeliklerini yönetme](active-directory-groups-membership-azure-portal.md)

- [Grupları kullanarak kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Azure Active Directory bir Azure aboneliği ilişkilendirin veya ekleyin](active-directory-how-subscriptions-associated-directory.md)
