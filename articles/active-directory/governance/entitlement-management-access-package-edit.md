---
title: Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) bir erişim paketini gizleme veya silme-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) bir erişim paketini gizlemeyi veya silmeyi öğrenin.
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
ms.openlocfilehash: ab6f3a86d8e88ee2d7465fd32bde7b7555f1f326
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389180"
---
# <a name="hide-or-delete-an-access-package-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) bir erişim paketini gizleme veya silme

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Erişim paketleri varsayılan olarak bulunabilir. Bu, bir ilke bir kullanıcının erişim paketini istemesine izin veriyorsa, erişim paketini otomatik olarak erişim portalı 'nda listelenmiş olarak görebilecekleri anlamına gelir. Ancak, **gizli** ayarı, erişim paketinin Kullanıcı erişim portalı 'nda listelenmemesi için değiştirebilirsiniz.

Bu makalede bir erişim paketinin nasıl gizlenmesi veya silineceği açıklanır.

## <a name="change-the-hidden-setting"></a>Gizli ayarı Değiştir

Bir erişim paketinin **gizli** ayarını değiştirmek için bu adımları izleyin.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Genel Bakış sayfasında, **Düzenle**' ye tıklayın.

1. **Gizli** ayarını ayarlayın.

    **Hayır**olarak ayarlanırsa, erişim paketi kullanıcının erişim portalında listelenir.

    **Evet**olarak ayarlanırsa, erişim paketi kullanıcının erişim portalı 'nda listelenmez. Bir kullanıcının erişim paketine doğrudan **erişim portalı bağlantısı** varsa, erişim paketini görüntüleyebilecekleri tek yöntem. Daha fazla bilgi için bkz. [erişim paketi istemek için bağlantıyı paylaşma](entitlement-management-access-package-settings.md).

## <a name="delete-an-access-package"></a>Erişim paketini silme

Erişim paketi yalnızca etkin kullanıcı atamaları yoksa silinebilir. Bir erişim paketini silmek için bu adımları izleyin.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Sol taraftaki menüde **atamalar** ' a tıklayın ve tüm kullanıcılar için erişimi kaldırın.

1. Sol menüde **genel bakış** ' a ve ardından **Sil**' e tıklayın.

1. Görüntülenen silme iletisinde **Evet**' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketi için atamaları görüntüleme, ekleme ve kaldırma](entitlement-management-access-package-assignments.md)
- [Raporları ve günlükleri görüntüleme](entitlement-management-reports.md)
