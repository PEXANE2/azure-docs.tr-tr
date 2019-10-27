---
title: Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) bir erişim paketi için atamaları görüntüleme, ekleme ve kaldırma-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) bir erişim paketi için atamaları görüntüleme, ekleme ve kaldırma hakkında bilgi edinin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: a20ff526ba00f5b6521198122972071360bb58d6
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72934482"
---
# <a name="view-add-and-remove-assignments-for-an-access-package-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) bir erişim paketi için atamaları görüntüleme, ekleme ve kaldırma

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure AD Yetkilendirme Yönetimi 'nde, paketlere kimlerin atandığını, ilkelerini ve durumunu görebilirsiniz. Bir erişim paketinin uygun bir ilkesi varsa, kullanıcıyı da bir erişim paketine doğrudan atayabilirsiniz. Bu makalede, bir erişim paketleri için atamaları görüntüleme, ekleme ve kaldırma işlemlerinin nasıl yapılacağı açıklanır.

## <a name="view-who-has-an-assignment"></a>Kimin atamaya sahip olduğunu görüntüleme

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Etkin atamaların listesini görmek için **atamalar** ' a tıklayın.

    ![Erişim paketine yönelik atamaların listesi](./media/entitlement-management-access-package-assignments/assignments-list.png)

1. Ek ayrıntıları görmek için belirli bir atamaya tıklayın.

1. Tüm kaynak rollerinin düzgün şekilde sağlandığı atamaların listesini görmek için, filtre durumuna tıklayın ve **teslim etme**' yi seçin.

    **İstek** sayfasında, kullanıcının karşılık gelen isteğini bularak teslim hatalarıyla ilgili ek ayrıntıları görebilirsiniz.

1. Süre biten atamaları görmek için, filtre durumuna tıklayın ve süre **bitti**' yi seçin.

1. Filtrelenmiş listenin CSV dosyasını indirmek için **İndir**' e tıklayın.

## <a name="directly-assign-a-user"></a>Doğrudan kullanıcı atama

Bazı durumlarda, kullanıcıların erişim paketini isteme işlemini istememeleri için bir erişim paketine doğrudan belirli kullanıcıları atamak isteyebilirsiniz. Kullanıcıları doğrudan atamak için, erişim paketinin yönetici doğrudan atamalarına izin veren bir ilkesi olması gerekir.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Sol taraftaki menüde **atamalar**' a tıklayın.

1. Erişim paketine Kullanıcı Ekle ' yi açmak için **yeni atama** ' ya tıklayın.

    ![Atamalar-erişim paketine Kullanıcı ekleme](./media/entitlement-management-access-package-assignments/assignments-add-user.png)

1. Bu erişim paketini atamak istediğiniz kullanıcıları seçmek için **Kullanıcı Ekle** ' ye tıklayın.

1. **Ilke Seç** listesinde, kullanıcıların gelecekteki istekleri ve yaşam döngüsünün tarafından yönetilecektir ve izlendiğini belirten bir ilke seçin. Seçili kullanıcıların farklı ilke ayarlarına sahip olmasını istiyorsanız yeni ilke **Oluştur** ' a tıklayarak yeni ilke ekleyebilirsiniz.

1. Seçili kullanıcıların atamasının başlamasını ve bitmesini istediğiniz tarih ve saati ayarlayın. Bir bitiş tarihi sağlanmazsa, ilkenin yaşam döngüsü ayarları kullanılacaktır.

1. İsteğe bağlı olarak kayıt tutmaya yönelik doğrudan atamanız için bir gerekçe sağlar.

1. Seçili kullanıcıları doğrudan erişim paketine atamak için **Ekle** ' ye tıklayın.

    Birkaç dakika sonra, atamalar listesinde kullanıcıları görmek için **Yenile** ' ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketi için isteği ve ayarları değiştirme](entitlement-management-access-package-request-policy.md)
- [Raporları ve günlükleri görüntüleme](entitlement-management-reports.md)
