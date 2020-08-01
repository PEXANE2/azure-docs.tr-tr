---
title: Yönetici rolü izinlerini görüntüleme ve atama-Azure AD | Microsoft Docs
description: Artık portalda bir Azure AD yönetici rolünün üyelerini görebilir ve yönetebilirsiniz. Rol atamalarını sıklıkla yönetenler için.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 06/15/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: fea05ada52ba4281acb3451769446d7f0bad790d
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87448704"
---
# <a name="view-and-assign-administrator-roles-in-azure-active-directory"></a>Azure Active Directory yönetici rollerini görüntüleme ve atama

Artık Azure Active Directory portalında yönetici rollerinin tüm üyelerini görebilir ve yönetebilirsiniz. Rol atamalarını sık sık yönetiyorsanız, büyük olasılıkla bu deneyimi tercih edersiniz. "Bu rollerin ne kadar iyi olduğunu merak edebilir?" seçeneğini aldıysanız, her bir Azure AD yönetici rolü için izinlerin ayrıntılı bir listesini görebilirsiniz.

## <a name="view-all-roles"></a>Tüm rolleri görüntüle

1. [Azure Portal](https://portal.azure.com) oturum açın ve **Azure Active Directory**' ı seçin.

1. Tüm kullanılabilir rollerin listesini görmek için **Roller ve yöneticiler '** i seçin.

1. Rolün izinlerini görmek için her satırın sağ tarafındaki üç noktayı seçin. Role atanan kullanıcıları görüntülemek için bir rol seçin. Aşağıdaki resimden farklı bir şey görürseniz, Privileged Identity Management (PıM) olup olmadığını doğrulamak için [ayrıcalıklı roller için atamaları görüntüle](#view-assignments-for-privileged-roles) bölümündeki notu okuyun.

    ![Azure AD portalındaki rollerin listesi](./media/directory-manage-roles-portal/view-roles-in-azure-active-directory.png)

## <a name="view-my-roles"></a>Rollerimi görüntüle

Kendi izinlerinizi de kolayca görüntüleyebilirsiniz. Size atanmış olan rolleri görmek için **Roller ve yöneticiler** sayfasında **rolünüzü** seçin.

## <a name="view-assignments-for-privileged-roles"></a>Ayrıcalıklı roller için atamaları görüntüleme

Ek yönetim özellikleri için **PIM 'de Yönet '** i seçebilirsiniz. Ayrıcalıklı rol yöneticileri "kalıcı" (rol içinde her zaman etkin) atamalarıyla "uygun" (yalnızca yükseltilmiş olduğunda rol) olarak değişiklik yapabilir. Privileged Identity Management yoksa, denemeye kaydolmak için **PIM 'de Yönet** ' i de seçebilirsiniz. Privileged Identity Management, bir [Azure AD Premium P2 lisans planı](../privileged-identity-management/subscription-requirements.md)gerektirir.

![Yönetici rolünün üyelerinin listesi](./media/directory-manage-roles-portal/member-list.png)

Genel yönetici veya ayrıcalıklı rol yöneticisiyseniz, üyeleri kolayca ekleyebilir veya kaldırabilir, listeyi filtreleyebilir veya etkin atanan rollerini görmek için bir üyeyi seçebilirsiniz.

> [!Note]
> Azure AD Premium P2 lisansınız varsa ve Privileged Identity Management zaten kullanıyorsanız, tüm rol yönetimi görevleri Azure AD 'de değil, ayrıcalık kimlik yönetimi 'nde gerçekleştirilir.
>
> ![Zaten PıM kullanan ve Premium P2 lisansına sahip olan kullanıcılar için PıM 'de yönetilen Azure AD rolleri](./media/directory-manage-roles-portal/pim-manages-roles-for-p2.png)

## <a name="view-a-users-role-permissions"></a>Kullanıcının rol izinlerini görüntüleme

Rolün üyelerini görüntülerken, rol ataması tarafından verilen izinlerin tüm listesini görmek için **Açıklama** ' yı seçin. Sayfa, Dizin rollerini yönetirken size yol göstermeye yardımcı olacak ilgili belgelerin bağlantılarını içerir.

![Yönetici rolü için izin listesi](./media/directory-manage-roles-portal/role-description.png)

## <a name="download-role-assignments"></a>Rol atamalarını indir

Belirli bir rolün tüm atamalarını indirmek için, **Roller ve yöneticiler** sayfasında bir rol seçin ve ardından **rol atamalarını indir**' i seçin. Bu rolün tüm kapsamlarındaki atamaları listeleyen bir CSV dosyası indirilir.

![bir rol için tüm atamaları indirin](./media/directory-manage-roles-portal/download-role-assignments.png)

## <a name="assign-a-role"></a>Rol atama

1. [Azure Portal](https://portal.azure.com) genel yönetici veya ayrıcalıklı rol yöneticisi izinleriyle oturum açın ve **Azure Active Directory**' ı seçin.

1. Tüm kullanılabilir rollerin listesini görmek için **Roller ve yöneticiler '** i seçin.

1. Atamalarını görmek için bir rol seçin.

    ![Yönetici rolü için izin listesi](./media/directory-manage-roles-portal/member-list.png)

1. Atama **Ekle** ' yi seçin ve atamak istediğiniz rolleri seçin. Ek yönetim özellikleri için **PIM 'de Yönet '** i seçebilirsiniz. Aşağıdaki resimden farklı bir şey görürseniz, PıM içinde olup olmadığını doğrulamak için [ayrıcalıklı roller için atamaları görüntüle](#view-assignments-for-privileged-roles) bölümündeki notu okuyun.

    ![Yönetici rolü için izin listesi](./media/directory-manage-roles-portal/directory-role-select-role.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD Yönetim rolleri forumundan](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşabilirsiniz.
* Roller ve yönetici rolü atama hakkında daha fazla bilgi için bkz. [yönetici rolleri atama](directory-assign-admin-roles.md).
* Varsayılan Kullanıcı izinleri için bkz. [varsayılan Konuk ve üye Kullanıcı izinlerinin karşılaştırması](../fundamentals/users-default-permissions.md).
