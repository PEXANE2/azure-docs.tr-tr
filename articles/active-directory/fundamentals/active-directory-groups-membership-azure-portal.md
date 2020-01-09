---
title: Başka bir gruptan grup ekleme veya kaldırma-Azure AD
description: Ekleme veya bir grup Azure Active Directory'yi kullanarak başka bir gruptan kaldırma hakkında yönergeler.
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
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75423082"
---
# <a name="add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>Ekleme veya bir grup Azure Active Directory'yi kullanarak başka bir gruptan kaldırma
Bu makalede, bir grup Azure Active Directory'yi kullanarak başka bir gruptan ekleyip yardımcı olur.

>[!Note]
>Üst grubun silmeye çalışıyorsanız, bkz [güncelleştirmek veya bir grup ve üyelerini silmek nasıl](active-directory-groups-delete-group.md).

## <a name="add-a-group-to-another-group"></a>Bir grubu başka bir gruba ekleyin
Mevcut güvenlik grubuna üye grubu (alt) ve bir üst grubu oluşturma var olan başka bir güvenlik grubu (olarak da bilinen iç içe geçmiş gruplar) ekleyebilirsiniz. Grubu üyeleri, öznitelikleri ve yapılandırma zamandan tasarruf üst grubunun özelliklerini devralır.

>[!Important]
>Şu anda desteklemiyoruz:<ul><li>Grupları Şirket içi Active Directory eşitlenmiş bir gruba ekleme.</li><li>Office 365 gruplarına güvenlik grupları ekleme.</li><li>Office 365 gruplarını güvenlik gruplarına veya diğer Office 365 gruplarına ekleme.</li><li>İç içe gruplar için uygulama atama.</li><li>Lisanslar, iç içe geçmiş gruplara uygulanıyor.</li><li>İç içe geçmiş senaryolarda dağıtım grupları ekleme.</li></ul>

### <a name="to-add-a-group-as-a-member-of-another-group"></a>Bir grubu başka bir grubun üyesi olarak eklemek için

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

2. Seçin **Azure Active Directory**ve ardından **grupları**.

3. Üzerinde **gruplar - tüm gruplar** sayfasında arayın ve başka bir grubun üyesi olmak için grubu seçin. Bu alıştırma için kullandığımız **MDM İlkesi - Batı** grubu.

    >[!Note]
    >Grubunuzun bir üyesi olarak aynı anda yalnızca bir gruba ekleyebilirsiniz. Ayrıca, **grup** kutusu görünen giriş için bir kullanıcı veya cihaz adı herhangi bir bölümünü eşleşmesi temeline göre filtreler. Ancak, joker karakterler desteklenmez.

    ![Seçili grupları - MDM ilkesiyle tüm grupları sayfası - Batı grubu](media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. Üzerinde **MDM İlkesi - Batı - grup üyeliklerini** sayfasında **grup üyeliklerini**seçin **Ekle**, grubunuzun bir üyesi olmanız ve ardından istediğinizgrububulun **Seçin**. Bu alıştırma için kullandığımız **MDM İlkesi - tüm kuruluş** grubu.

    **MDM İlkesi - Batı** grup, artık üye **MDM İlkesi - tüm kuruluş** grubu, tüm özellikleri ve MDM İlkesi - tüm kuruluş grubu yapılandırmasını devralıyor.

    ![Başka bir gruba grubu ekleyerek bir grup üyeliği oluştur](media/active-directory-groups-membership-azure-portal/group-add-group-membership.png)

5. Gözden geçirme **MDM İlkesi - Batı - grup üyeliklerini** grup ve üye ilişkiyi görmek için sayfayı.

6. Grup ve üye ilişkinin daha ayrıntılı bir görünüm için Grup adını seçin (**MDM İlkesi - tüm kuruluş**) ve göz atın **MDM İlkesi - Batı** Ayrıntıları sayfası.

## <a name="remove-a-group-from-another-group"></a>Bir grubu başka bir gruptan Kaldır
Mevcut bir güvenlik grubuna başka bir güvenlik grubundan kaldırabilirsiniz. Ancak, grup kaldırma de tüm devralınan öznitelikleri ve özellikleri üyeleri için kaldırır.

### <a name="to-remove-a-member-group-from-another-group"></a>Üye grubu başka bir gruptan kaldırmak için
1. Üzerinde **gruplar - tüm gruplar** sayfasında arayın ve başka bir grubun üyesi olarak kaldırılacak grubunu seçin. Bu alıştırma için yeniden kullanıyoruz **MDM İlkesi - Batı** grubu.

2. Üzerinde **MDM İlkesi - Batı genel bakış** sayfasında **grup üyeliklerini**.

3. Seçin **MDM İlkesi - tüm kuruluş** gelen grup **MDM İlkesi - Batı - grup üyeliklerini** sayfasında ve ardından **Kaldır** gelen **MDM İlkesi - Batı** Ayrıntıları sayfası.

    ![Grup üyeliği sayfası hem üye ve Grup ayrıntılarını gösterme](media/active-directory-groups-membership-azure-portal/group-membership-remove.png)

## <a name="additional-information"></a>Ek Bilgi
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Üye ekleme veya gruptan kaldırma](active-directory-groups-members-azure-portal.md)

- [Grup ayarlarınızı düzenleme](active-directory-groups-settings-azure-portal.md)

- [SaaS uygulamalarına erişimi yönetmek için grup kullanma](../users-groups-roles/groups-saasapps.md)

- [Senaryoları, sınırlamalar ve bilinen sorunlar Azure Active Directory'de lisanslama yönetmek için grupları kullanma](../users-groups-roles/licensing-group-advanced.md#limitations-and-known-issues)
