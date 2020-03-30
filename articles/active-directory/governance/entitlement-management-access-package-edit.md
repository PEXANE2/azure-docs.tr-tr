---
title: Yetkilendirme yönetiminde erişim paketini gizleme veya silme - Azure AD
description: Azure Active Directory yetkilendirme yönetiminde bir erişim paketini nasıl gizleyeceğinizi veya silmeyi öğrenin.
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
ms.openlocfilehash: cf410db5a483fc4f5d9efc1a6bd5c6202694ef4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262002"
---
# <a name="hide-or-delete-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketini gizleme veya silme

Erişim paketleri varsayılan olarak bulunabilir. Bu, bir ilke kullanıcının erişim paketini istemesine izin verirse, erişim paketini Erişim portalımda listelenen erişimi otomatik olarak göreceği anlamına gelir. Ancak, erişim paketinin kullanıcının Erişim portalımda listelenmemesi için **Gizli** ayarını değiştirebilirsiniz.

Bu makalede, bir erişim paketinin nasıl gizlenip silinilizleyeceği açıklanmaktadır.

## <a name="change-the-hidden-setting"></a>Gizli ayarı değiştirme

Bir erişim paketi için **Gizli** ayarını değiştirmek için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. Genel Bakış **sayfasında, Edit'i**tıklatın.

1. **Gizli** ayarı ayarlayın.

    **No**olarak ayarlanırsa, erişim paketi kullanıcının Erişim portalımda listelenir.

    **Evet**olarak ayarlanırsa, erişim paketi kullanıcının Erişim portalımda listelenmez. Bir kullanıcının erişim paketini görüntüleyebildiği tek yol, erişim paketine doğrudan **Erişim portalım bağlantısına** sahip olmasıdır. Daha fazla bilgi [için, erişim paketi istemek için Paylaş bağlantısına](entitlement-management-access-package-settings.md)bakın.

## <a name="delete-an-access-package"></a>Erişim paketini silme

Erişim paketi yalnızca etkin kullanıcı atamaları yoksa silinebilir. Bir erişim paketini silmek için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. Sol menüde **Atamalar'ı** tıklatın ve tüm kullanıcılar için erişimi kaldırın.

1. Sol menüde **Genel Bakış'ı** tıklatın ve sonra **Sil'i**tıklatın.

1. Görünen silme iletisinde **Evet'i**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketi için atamaları görüntüleme, ekleme ve kaldırma](entitlement-management-access-package-assignments.md)
- [Raporları ve günlükleri görüntüleme](entitlement-management-reports.md)
