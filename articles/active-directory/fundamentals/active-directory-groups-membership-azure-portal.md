---
title: Başka bir gruptan grup ekleme veya kaldırma-Azure AD
description: Azure Active Directory kullanarak başka bir gruptan grup ekleme veya kaldırma yönergeleri.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 10/19/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2abddefebdf58b6c4f82bd6e6e882e3cb5d16023
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85604310"
---
# <a name="add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>Azure Active Directory kullanarak başka bir gruptan grup ekleme veya kaldırma
Bu makale, Azure Active Directory kullanarak bir grubu başka bir gruptan eklemenize ve kaldırmanıza yardımcı olur.

>[!Note]
>Üst grubu silmeye çalışıyorsanız, bkz. [Grup ve üyelerini güncelleştirme veya silme](active-directory-groups-delete-group.md).

## <a name="add-a-group-to-another-group"></a>Başka bir gruba grup ekleme
Varolan bir güvenlik grubunu var olan başka bir güvenlik grubuna (iç içe geçmiş gruplar olarak da bilinir) ekleyebilir, üye grubu (alt grup) ve bir üst grup oluşturabilirsiniz. Üye grubu, yapılandırma süresini kaydederek üst grubun özniteliklerini ve özelliklerini devralır.

>[!Important]
>Şu anda şunları desteklemiyoruz:<ul><li>Grupları Şirket içi Active Directory eşitlenmiş bir gruba ekleme.</li><li>Office 365 gruplarına güvenlik grupları ekleme.</li><li>Office 365 gruplarını güvenlik gruplarına veya diğer Office 365 gruplarına ekleme.</li><li>İç içe gruplar için uygulama atama.</li><li>Lisanslar, iç içe geçmiş gruplara uygulanıyor.</li><li>İç içe geçmiş senaryolarda dağıtım grupları ekleme.</li></ul>

### <a name="to-add-a-group-as-a-member-of-another-group"></a>Bir grubu başka bir grubun üyesi olarak eklemek için

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**' yi seçin ve ardından **gruplar**' ı seçin.

3. **Gruplar-tüm gruplar** sayfasında, başka bir grubun üyesi olacak grubu arayın ve seçin. Bu alıştırma için **MDM ilkesi-Batı** grubunu kullanıyoruz.

    >[!Note]
    >Grubunuzu tek seferde yalnızca bir gruba üye olarak ekleyebilirsiniz. Ayrıca, **Grup Seç** kutusu, bir kullanıcının veya cihaz adının herhangi bir bölümüne girdinizi eşleştirmeye göre görünümü filtreler. Ancak joker karakterler desteklenmez.

    ![Gruplar-MDM ilkesi içeren tüm gruplar sayfası-Batı grubu seçildi](media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. **MDM ilkesi-Batı-Grup üyelikleri** sayfasında **Grup üyelikleri**' ni seçin, **Ekle**' yi seçin, grubunuzun üyesi olmasını istediğiniz grubu bulun ve ardından **Seç**' i seçin. Bu alıştırma için **MDM ilkesi-tüm kuruluş** grubunu kullanıyoruz.

    MDM ilkesi **-Batı** grubu artık MDM ilkesi-tüm **kuruluş** grubunun bir üyesidir ve MDM ilkesinin tüm özelliklerini ve yapılandırmalarını devralarak tüm kuruluş grubu.

    ![Başka bir gruba grup ekleyerek grup üyeliği oluşturma](media/active-directory-groups-membership-azure-portal/group-add-group-membership.png)

5. Grup ve üye ilişkisini görmek için **MDM ilkesi-Batı-Grup üyelikleri** sayfasını gözden geçirin.

6. Grup ve üye ilişkisinin daha ayrıntılı bir görünümü için Grup adını (**MDM ilkesi-tümü kuruluş**) seçin ve **MDM ilkesi-Batı** sayfası ayrıntılarına göz atın.

## <a name="remove-a-group-from-another-group"></a>Bir grubu başka bir gruptan Kaldır
Varolan bir güvenlik grubunu başka bir güvenlik grubundan kaldırabilirsiniz. Ancak, grubun kaldırılması, üyelerinin devralınan özniteliklerini ve özelliklerini de kaldırır.

### <a name="to-remove-a-member-group-from-another-group"></a>Bir üye grubunu başka bir gruptan kaldırmak için
1. **Gruplar-tüm gruplar** sayfasında, başka bir grubun üyesi olarak kaldırılacak grubu arayın ve seçin. Bu alıştırma için **MDM ilkesi-Batı** grubunu kullanmaya yeniden başladık.

2. **MDM ilkesi-Batı genel bakış** sayfasında **Grup üyelikleri**' nı seçin.

3. MDM ilkesi- **Batı-Grup üyelikleri** sayfasından **MDM ilkesini** seçin ve MDM **ilkesi-Batı** sayfası ayrıntılarından **Kaldır** ' ı seçin.

    ![Üye ve Grup ayrıntılarını gösteren Grup üyeliği sayfası](media/active-directory-groups-membership-azure-portal/group-membership-remove.png)

## <a name="additional-information"></a>Ek bilgiler
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Gruba üye ekleme veya gruptan üye kaldırma](active-directory-groups-members-azure-portal.md)

- [Grup ayarlarınızı düzenleme](active-directory-groups-settings-azure-portal.md)

- [SaaS uygulamalarına erişimi yönetmek için grup kullanma](../users-groups-roles/groups-saasapps.md)

- [Azure Active Directory 'de lisanslamayı yönetmek için grupları kullanarak senaryolar, sınırlamalar ve bilinen sorunlar](../users-groups-roles/licensing-group-advanced.md#limitations-and-known-issues)
