---
title: Bir grubu başka bir gruptan ekleme veya kaldırma - Azure AD
description: Azure Etkin Dizini'ni kullanarak bir grubun başka bir gruptan nasıl ekleyeceğiniz veya kaldırılacağına ilişkin yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: 830bf7134b3a8b0425c53673a1347dd77897a5bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75423082"
---
# <a name="add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>Azure Etkin Dizini'ni kullanarak bir grubu başka bir gruptan ekleme veya kaldırma
Bu makale, Azure Etkin Dizin'i kullanarak bir grubu başka bir gruptan eklemenize ve kaldırmanıza yardımcı olur.

>[!Note]
>Üst grubu silmeye çalışıyorsanız, [bir grubu ve üyelerini nasıl güncelleştirdiğinize veya silebilirsiniz.](active-directory-groups-delete-group.md)

## <a name="add-a-group-to-another-group"></a>Başka bir gruba grup ekleme
Varolan bir Güvenlik grubu (iç içe geçmiş gruplar olarak da bilinir), bir üye grubu (alt grup) ve bir üst grup oluşturabilirsiniz. Üye grup, üst grubun özniteliklerini ve özelliklerini devralır ve yapılandırma süresini kurtarır.

>[!Important]
>Şu anda destekliyoruz:<ul><li>Şirket içi Active Directory ile senkronize edilmiş bir gruba grup ekleme.</li><li>Office 365 gruplarına Güvenlik grupları ekleme.</li><li>Güvenlik gruplarına veya diğer Office 365 gruplarına Office 365 grupları ekleme.</li><li>İç içe olan gruplara uygulama atama.</li><li>İç içe gruplara lisans uygulama.</li><li>İç içe geçme senaryolarında dağıtım grupları ekleme.</li></ul>

### <a name="to-add-a-group-as-a-member-of-another-group"></a>Grubu başka bir grubun üyesi olarak eklemek için

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Etkin Dizini'ni**seçin ve ardından **Gruplar'ı**seçin.

3. Gruplar **- Tüm gruplar** sayfasında, başka bir gruba üye olacak grubu arayın ve seçin. Bu egzersiz **için, MDM politikasını kullanıyoruz- Batı** grubu.

    >[!Note]
    >Grupunuzu üye olarak aynı anda yalnızca bir gruba ekleyebilirsiniz. Ayrıca, **Grup Seç** kutusu, girişinizi kullanıcı veya aygıt adının herhangi bir bölümüyle eşleştirmeye göre ekranı filtreler. Ancak joker karakter desteklenmez.

    ![Gruplar - MDM politikası ile tüm gruplar sayfası - Batı grubu seçildi](media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. **MDM politikasında - Batı - Grup üyelikleri** sayfasında, **Grup üyelikleri**seçin , **Ekle**'yi seçin , grubunuzun üyesi olmasını istediğiniz grubu bulun ve sonra **Seç'i**seçin. Bu egzersiz **için, MDM politikasını** kullanıyoruz - Tüm org grubu.

    **MDM politikası - West** group artık **MDM politikasının** bir üyesidir - Tüm org grubu, MDM politikasının tüm özelliklerini ve yapılandırmasını devralan - Tüm org grubu.

    ![Başka bir gruba grup ekleyerek grup üyeliği oluşturma](media/active-directory-groups-membership-azure-portal/group-add-group-membership.png)

5. Grup ve üye ilişkisini görmek için MDM politikasını gözden geçirin **- Batı - Grup üyelikleri** sayfasını.

6. Grup ve üye ilişkisinin daha ayrıntılı bir görünümü için grup adını **(MDM ilkesi - Tümü org)** seçin ve **MDM ilkesine** bir göz atın - Batı sayfası ayrıntıları.

## <a name="remove-a-group-from-another-group"></a>Grubu başka bir gruptan kaldırma
Varolan bir Güvenlik grubunu başka bir Güvenlik grubundan kaldırabilirsiniz. Ancak, grubu kaldırmak, üyeleri için devralınan tüm öznitelikleri ve özellikleri de kaldırır.

### <a name="to-remove-a-member-group-from-another-group"></a>Bir üye grubu başka bir gruptan kaldırmak için
1. Gruplar **- Tüm gruplar** sayfasında, başka bir grubun üyesi olarak kaldırılacak grubu arayın ve seçin. Bu egzersiz için, yine **MDM politikasını** kullanıyoruz - Batı grubu.

2. **MDM politikasında - Batı'ya genel bakış** sayfasında, **Grup üyeliklerini**seçin.

3. **MDM ilkesini** seçin - **MDM ilkesinden** tüm org grubu - Batı - Grup üyelikleri sayfası ve ardından **MDM ilkesinden** **kaldır** ' ı seçin - Batı sayfası ayrıntıları.

    ![Hem üyeyi hem de grup ayrıntılarını gösteren grup üyelik sayfası](media/active-directory-groups-membership-azure-portal/group-membership-remove.png)

## <a name="additional-information"></a>Ek bilgiler
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Bir gruptan üye ekleme veya kaldırma](active-directory-groups-members-azure-portal.md)

- [Grup ayarlarınızı düzenleme](active-directory-groups-settings-azure-portal.md)

- [SaaS uygulamalarına erişimi yönetmek için grup kullanma](../users-groups-roles/groups-saasapps.md)

- [Azure Etkin Dizini'nde lisanslamayı yönetmek için grupları kullanarak senaryolar, sınırlamalar ve bilinen sorunlar](../users-groups-roles/licensing-group-advanced.md#limitations-and-known-issues)
