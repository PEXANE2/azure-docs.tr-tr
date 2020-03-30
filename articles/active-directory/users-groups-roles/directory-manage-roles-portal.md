---
title: Yönetici rol izinlerini görüntüleme ve atama - Azure AD | Microsoft Dokümanlar
description: Artık portaldaki Azure AD yöneticisi rolünün üyelerini görebilir ve yönetebilirsiniz. Sık sık rol atamaları yönetmek isteyenler için.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2815b496545ca5f920c00df7b2b5b7efe15c12a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900892"
---
# <a name="view-and-assign-administrator-roles-in-azure-active-directory"></a>Azure Etkin Dizin'de yönetici rollerini görüntüleme ve atama

Artık Azure Etkin Dizin portalındaki yönetici rollerinin tüm üyelerini görebilir ve yönetebilirsiniz. Sık sık rol atamaları yönetiyorsanız, büyük olasılıkla bu deneyimi tercih edersiniz. "Bu roller gerçekten ne yapıyor?" diye merak ettiyseniz, Azure REKLAM yöneticisi rollerinin her biri için ayrıntılı bir izin listesi görebilirsiniz.

## <a name="view-all-roles"></a>Tüm rolleri görüntüleme

**Azure Active Directory**'yi bulun ve seçin. Kullanılabilir tüm rollerin listesini görmek için **Roller'i ve yöneticileri** seçin.

Rolün ayrıntılı açıklamasını açmak için her satırın sağındaki elipsleri tıklatın.

![Azure AD portalındaki roller listesi](./media/directory-manage-roles-portal/view-roles-in-azure-active-directory.png)

## <a name="view-my-roles"></a>Rollerimi görüntüleme

Kendi izinlerinizi de kolayca görüntüleyin. Şu anda size atanan rolleri görmek için **Roller ve yöneticiler** **sayfasındaki Rolünüzü** seçin.

## <a name="view-assignments-for-a-role"></a>Bir rol için atamaları görüntüleme

Role atanan kullanıcıları görüntülemek için bir rolü tıklatın. Ek yönetim özellikleri için **PIM'de Yönet'i** seçebilirsiniz. Ayrıcalıklı Rol Yöneticileri "Kalıcı" (her zaman rol etkin) atamaları "Uygun" (yalnızca yükseltildiğinde rol) olarak değiştirebilir. PIM'iniz yoksa, deneme sürümüne kaydolmak için **PIM'de Yönet'i** seçebilirsiniz. Ayrıcalıklı Kimlik Yönetimi için [Azure AD Premium P2 lisans planı](../privileged-identity-management/subscription-requirements.md)gerekir.

![yönetici rolüne üye listesi](./media/directory-manage-roles-portal/member-list.png)

Genel Yönetici veya Ayrıcalıklı Rol Yöneticisiyseniz, üyeleri kolayca ekleyebilir veya kaldırabilir, listeye filtre uygulayabilir veya etkin atanan rollerini görmek için bir üye seçebilirsiniz.

## <a name="view-a-users-role-permissions"></a>Kullanıcının rol izinlerini görüntüleme

Bir rolün üyelerini görüntülerken, rol ataması tarafından verilen izinlerin tam listesini görmek için **Açıklama'yı** seçin. Sayfa, genel müdürlük rolleri boyunca size rehberlik edecek ilgili belgelere bağlantılar içerir.

![yönetici rolü için izinlistesi](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Sonraki adımlar

* Azure AD yönetim rolleri [forumunda](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)bizimle paylaşmakta çekinmeyin.
* Roller ve Yönetici rol ataması hakkında daha fazla şey için [yönetici rollerini atay'a](directory-assign-admin-roles.md)bakın.
* Varsayılan kullanıcı izinleri için varsayılan [konuk ve üye kullanıcı izinlerinin karşılaştırılmasına](../fundamentals/users-default-permissions.md)bakın.
