---
title: Azure AD yetkilendirme yönetiminde paket yöneticilerine erişim için yetki erişimi yönetimini temsilcilendirme - Azure Etkin Dizin
description: Bt yöneticilerinin erişim yönetimini paket yöneticilerine ve proje yöneticilerine erişmek için nasıl devratabileceğinizi öğrenin, böylece erişim kendileri yönetebilsinler.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/07/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: efdc61b82a19cc5d370d6069e8c9dcd3ce5e8ae5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73174362"
---
# <a name="delegate-access-governance-to-access-package-managers-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde paket yöneticilerine erişim için erişim yönetimini temsilcilendirme

Bir katalogdaki erişim paketlerinin oluşturulmasını ve yönetimini devretmek için, kullanıcıları erişim paketi yöneticisi rolüne eklersiniz. Erişim paketi yöneticileri, kullanıcıların bir katalogdaki kaynaklara erişim isteme leri gereğini biliyor olmalıdır. Örneğin, bir proje için katalog kullanılıyorsa, proje müşteri adayı bu katalog için bir erişim paketi yöneticisi olabilir.  Erişim paketi yöneticileri bir kataloğa kaynak ekleyemez, ancak bir katalogdaki erişim paketlerini ve ilkelerini yönetebilir.  Bir erişim paketi yöneticisine devredeyken, bu kişi aşağıdakitarihten sorumlu olabilir:

- Bir kullanıcının katalogdaki kaynaklara ne gibi rolleri olacaktır?
- Kimler erişebilir
- Erişim isteklerini kimin onaylaması gerekiyor
- Projenin ne kadar süreceğü

Bu video, erişim yönetimini katalog sahibinden erişim paketi yöneticisine nasıl devredeceğine genel bir bakış sağlar.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3Lq08]

## <a name="as-a-catalog-owner-delegate-to-an-access-package-manager"></a>Katalog sahibi olarak, bir erişim paketi yöneticisine temsilci

Bir kullanıcıyı erişim paketi yöneticisi rolüne atamak için aşağıdaki adımları izleyin:

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi veya Katalog sahibi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Kataloglar'ı** tıklatın ve ardından yöneticilereklemek istediğiniz kataloğu açın.

1. Sol menüde, **Roller ve yöneticiler'i**tıklatın.

    ![Katalogrolleri ve yöneticileri](./media/entitlement-management-shared/catalog-roles-administrators.png)

1. Bu rollerin üyelerini seçmek için **erişim paketi yöneticileri ekle'yi** tıklatın.

1. Bu üyeleri eklemek için **Seç'i** tıklatın.

## <a name="remove-an-access-package-manager"></a>Erişim paketi yöneticisini kaldırma

Bir kullanıcıyı erişim paketi yöneticisi rolünden kaldırmak için aşağıdaki adımları izleyin:

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi veya Katalog sahibi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Kataloglar'ı** tıklatın ve ardından yöneticilereklemek istediğiniz kataloğu açın.

1. Sol menüde, **Roller ve yöneticiler'i**tıklatın.

1. Kaldırmak istediğiniz bir erişim paketi yöneticisinin yanına bir onay işareti ekleyin.

1. **Kaldır**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Yeni erişim paketi oluşturma](entitlement-management-access-package-create.md)
