---
title: Son silinen kullanıcıyı geri yükleme veya kalıcı olarak kaldırma - Azure AD
description: Geri yüklenebilir kullanıcıları görüntüleme, silinmiş bir kullanıcıyı geri yükleme veya Azure Etkin Dizini ile bir kullanıcıyı kalıcı olarak silme.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 573269da1ca6b9ee09b493f4e758e78121d6c2f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422854"
---
# <a name="restore-or-remove-a-recently-deleted-user-using-azure-active-directory"></a>Azure Etkin Dizinini kullanarak yakın zamanda silinen bir kullanıcıyı geri yükleme veya kaldırma
Bir kullanıcıyı sildikten sonra, hesap 30 gün boyunca askıya alınmış durumda kalır. Bu 30 günlük süre boyunca, kullanıcı hesabı tüm özellikleriyle birlikte geri yüklenebilir. Bu 30 günlük süre geçtikten sonra, kullanıcı otomatik olarak ve kalıcı olarak silinir.

Azure portalında geri yüklenen kullanıcılarınızı görüntüleyebilir, silinmiş bir kullanıcıyı geri yükleyebilir veya Azure Active Directory (Azure AD) kullanarak bir kullanıcıyı kalıcı olarak silebilirsiniz.

>[!Important]
>Ne siz ne de Microsoft müşteri desteği kalıcı olarak silinen bir kullanıcıyı geri yükleyebilir.

## <a name="required-permissions"></a>Gerekli izinler
Kullanıcıları geri yüklemek ve kalıcı olarak silmek için aşağıdaki rollerden birine sahip olmalısınız.

- Genel yönetici

- Ortak Tier1 Desteği

- Ortak Tier2 Desteği

- Kullanıcı yöneticisi

## <a name="view-your-restorable-users"></a>Geri leştirilebilir kullanıcılarınızı görüntüleyin
30 günden daha kısa bir süre önce silinen tüm kullanıcıları görebilirsiniz. Bu kullanıcılar geri yüklenebilir.

### <a name="to-view-your-restorable-users"></a>Geri toyabilir kullanıcılarınızı görüntülemek için
1. Kuruluşun Global yönetici hesabını kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.

2. **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Silinen kullanıcıları**seçin.

    Geri yüklemeye hazır kullanıcıların listesini gözden geçirin.

    ![Kullanıcılar - Silinmiş kullanıcılar sayfası, hala geri yüklenebilir kullanıcılar ile](media/active-directory-users-restore/users-deleted-users-view-restorable.png)

## <a name="restore-a-recently-deleted-user"></a>Yakın zamanda silinen bir kullanıcıyı geri yükleme

Bir kullanıcı hesabı kuruluştan silindiğinde, hesap askıya alınmış durumdadır ve ilgili tüm kuruluş bilgileri korunur. Bir kullanıcıyı geri yüklediğinizde, bu kuruluş bilgileri de geri yüklenir.

> [!Note]
> Bir kullanıcı geri yüklendikten sonra, silme sırasında kullanıcıya atanan lisanslar, bu lisanslar için boş koltuk olmasa bile geri yüklenir. Daha sonra satın aldığınızdan daha fazla lisans tüketiyorsanız, kuruluşunuz lisans kullanımı için geçici olarak uyumsuz olabilir.

### <a name="to-restore-a-user"></a>Bir kullanıcıyı geri yüklemek için
1. Kullanıcılar **- Silinmiş kullanıcılar** sayfasında, kullanılabilir kullanıcılardan birini arayın ve seçin. Örneğin, _Mary Parker._

2. **Kullanıcıyı Geri Yükle'yi**seçin.

    ![Kullanıcılar - Silinmiş kullanıcılar sayfası, Kullanıcıyı Geri Yükleme seçeneği vurgulanmış](media/active-directory-users-restore/users-deleted-users-restore-user.png)

## <a name="permanently-delete-a-user"></a>Bir kullanıcıyı kalıcı olarak silme
Otomatik silme için 30 gün beklemeden bir kullanıcıyı kuruluşunuzdan kalıcı olarak silebilirsiniz. Kalıcı olarak silinen bir kullanıcı, sizin, başka bir yöneticinin veya Microsoft müşteri desteğinin geri yüklenmez.

>[!Note]
>Yanlışlıkla bir kullanıcıyı kalıcı olarak silerseniz, yeni bir kullanıcı oluşturmanız ve önceki tüm bilgileri el ile girmeniz gerekir. Yeni bir kullanıcı oluşturma hakkında daha fazla bilgi için [bkz.](add-users-azure-active-directory.md)

### <a name="to-permanently-delete-a-user"></a>Bir kullanıcıyı kalıcı olarak silmek için

1. Kullanıcılar **- Silinmiş kullanıcılar** sayfasında, kullanılabilir kullanıcılardan birini arayın ve seçin. Örneğin, _Rae Huff._

2. **Kalıcı Olarak Sil'i**seçin.

    ![Kullanıcılar - Silinmiş kullanıcılar sayfası, Kullanıcıyı Geri Yükleme seçeneği vurgulanmış](media/active-directory-users-restore/users-deleted-users-permanent-delete-user.png)

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcılarınızı geri yükledikten veya sildikten sonra aşağıdaki temel işlemleri gerçekleştirebilirsiniz:

- [Kullanıcı ekleme veya silme](add-users-azure-active-directory.md)

- [Kullanıcılara rol atama](active-directory-users-assign-role-azure-portal.md)

- [Profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)

- [Başka bir kuruluştan konuk kullanıcı ekleme](../b2b/what-is-b2b.md)

Kullanılabilir diğer kullanıcı yönetimi görevleri hakkında daha fazla bilgi için [Azure AD kullanıcı yönetimi belgeleri.](../users-groups-roles/index.yml)
