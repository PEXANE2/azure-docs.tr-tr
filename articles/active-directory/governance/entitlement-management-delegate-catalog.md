---
title: Azure AD yetkilendirme yönetiminde katalog oluşturucularına erişim yönetimini temsilcilendirme - Azure Active Directory
description: Bt yöneticilerinden erişim yönetimini katalog oluşturucularına ve proje yöneticilerine nasıl devredebileceğinizi öğrenin, böylece erişim kendileri yönetebilecekler.
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
ms.date: 10/26/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51e37ad006da5042291614c773f242b5a1f3be97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77120186"
---
# <a name="delegate-access-governance-to-catalog-creators-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde katalog oluşturucularına erişim yönetimini temsilci olarak

Katalog, kaynak ve erişim paketlerinin bir kapsayıcısIdur. İlgili kaynakları gruplandırmak ve paketlere erişmek istediğinizde bir katalog oluşturursunuz. Varsayılan olarak, Bir Global yönetici veya Kullanıcı yöneticisi [bir katalog oluşturabilir](entitlement-management-catalog-create.md)ve katalog sahibi olarak ek kullanıcılar ekleyebilir.

Yönetici olmayan kullanıcıları kendi kataloglarını oluşturabilmeleri için temsilci vermek için, bu kullanıcıları Azure AD yetkilendirme yönetimi tanımlı katalog oluşturucu rolüne ekleyebilirsiniz. Tek tek kullanıcıları ekleyebilir veya üyeleri katalog oluşturabilen bir grup ekleyebilirsiniz.  Bir katalog oluşturduktan sonra, kataloglarına sahip oldukları kaynakları ekleyebilirler.

## <a name="as-an-it-administrator-delegate-to-a-catalog-creator"></a>BT yöneticisi olarak, katalog oluşturucuya temsilci

Bir kullanıcıyı katalog oluşturucu rolüne atamak için aşağıdaki adımları izleyin.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Yetkilendirme yönetimi** bölümünde **Ayarlar'ı**tıklatın.

1. **Düzenle**’ye tıklayın.

    ![Katalog oluşturucuları eklemek için ayarlar](./media/entitlement-management-delegate-catalog/settings-delegate.png)

1. Temsilci **yetkilendirme yönetimi** bölümünde, bu yetkilendirme yönetimi rolünü devretmek istediğiniz kullanıcıları veya grupları seçmek için **katalog oluşturucuları ekle'yi** tıklatın.

1. **Seç'i**tıklatın.

1. **Kaydet**'e tıklayın.

## <a name="allow-delegated-roles-to-access-the-azure-portal"></a>Yetkin rollerin Azure portalına erişmesine izin verme

Katalog oluşturucuları ve erişim paketi yöneticileri gibi temsilci rollerin, erişim paketlerini yönetmek için Azure portalına erişmesine izin vermek için yönetim portalı ayarını denetlemeniz gerekir.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı yöneticisi

1. Azure portalında **Azure Etkin Dizini'ni** tıklatın ve ardından **Kullanıcılar'ı**tıklatın.

1. Sol menüde **Kullanıcı ayarlarını**tıklatın.

1. Azure **AD yönetim portalına erişimi kısıtla'nın** **Hayır**olarak ayarlandıklarına emin olun.

    ![Azure AD kullanıcı ayarları - Yönetim portalı](./media/entitlement-management-delegate-catalog/user-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Kaynak kataloğu oluşturma ve yönetme](entitlement-management-catalog-create.md)
- [Erişim paketi yöneticilerine erişim yönetimini temsilci olarak](entitlement-management-delegate-managers.md)

