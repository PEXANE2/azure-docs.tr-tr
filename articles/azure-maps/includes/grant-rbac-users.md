---
title: Kullanıcılar için rol tabanlı erişim verme
titleSuffix: Azure Maps
description: Kullanıcılara Azure Maps yetkilendirmesi sağlamak için rol tabanlı erişim denetimi kullanma
author: philmea
ms.author: philmea
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 86f89397f3685443071788580253ee11ce4b70be
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84988685"
---
## <a name="grant-role-based-access-for-users-to-azure-maps"></a>Kullanıcılara Azure Maps 'e rol tabanlı erişim verme

Bir veya daha fazla Azure Maps erişim denetimi rol tanımına bir Azure AD grubu veya güvenlik sorumlusu atayarak *rol tabanlı erişim denetimi* (RBAC) verirsiniz. Azure haritalar için kullanılabilen RBAC rol tanımlarını görüntülemek için **erişim denetimi (IAM)** sayfasına gidin. **Roller**' i seçin ve ardından *Azure Maps*ile başlayan roller için arama yapın.

* Büyük miktarda kullanıcının Azure Maps 'e erişimini verimli bir şekilde yönetmek için bkz. [Azure AD grupları](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups).
* Kullanıcıların uygulamada kimlik doğrulamasına izin verilmesi için kullanıcıların Azure AD 'de oluşturulması gerekir. Bkz. [Azure ad kullanarak Kullanıcı ekleme veya silme](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory).

Kullanıcıları bir dizini etkin bir şekilde yönetmek için [Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/) 'de daha fazla bilgi edinin.

1. **Azure haritalar hesabınıza**gidin. **Erişim denetimi (IAM)**  >  **rol atamasını**seçin.

    ![RBAC verme](../media/how-to-manage-authentication/how-to-grant-rbac.png)

2. **Rol atamaları** sekmesinde, **rol**altında **Azure Maps veri okuyucusu** veya **Azure haritalar veri katılımcısı**gibi yerleşik bir Azure Maps rol tanımı seçin. **Erişim ata**' nın altında **Azure AD Kullanıcı, Grup veya hizmet sorumlusu**' nı seçin. Asıl ada göre ' yi seçin. Sonra **Kaydet**'i seçin.

   * [Rol atamaları ekleme veya kaldırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)ile ilgili ayrıntılara bakın.

> [!WARNING]
> Azure Maps yerleşik rol tanımları birçok Azure haritalar REST API 'Lerine çok büyük bir yetkilendirme erişimi sağlar. API 'Leri kullanıcılara en düşük düzeyde kısıtlamak için bkz. [özel rol tanımı oluşturma ve kullanıcıları](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) özel rol tanımına atama. Bu, kullanıcıların uygulama için gereken en düşük ayrıcalığa sahip olmasını sağlar.