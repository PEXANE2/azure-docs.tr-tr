---
title: Kullanıcılar için rol tabanlı erişim verme
titleSuffix: Azure Maps
description: Kullanıcılara Azure Maps yetkilendirmesi sağlamak için rol tabanlı erişim denetimi kullanma
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 816e4e439bb98ced8bbc5b5d8a7d38ef65aee33f
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88010962"
---
## <a name="grant-role-based-access-for-users-to-azure-maps"></a>Kullanıcılara Azure Maps 'e rol tabanlı erişim verme

Bir ya da daha fazla Azure Maps rol tanımına bir Azure AD grubu veya güvenlik sorumlusu atayarak *Azure rol tabanlı erişim denetimi (Azure RBAC)* verirsiniz. Azure haritalar için kullanılabilen Azure rol tanımlarını görüntülemek için **erişim denetimi 'ne (IAM)** gidin. **Roller**' i seçin ve ardından *Azure Maps*ile başlayan roller için arama yapın.

* Büyük miktarda kullanıcının Azure Maps 'e erişimini verimli bir şekilde yönetmek için bkz. [Azure AD grupları](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups).
* Kullanıcıların uygulamada kimlik doğrulamasına izin verilmesi için kullanıcıların Azure AD 'de oluşturulması gerekir. Bkz. [Azure ad kullanarak Kullanıcı ekleme veya silme](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory).

Kullanıcıları bir dizini etkin bir şekilde yönetmek için [Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/) 'de daha fazla bilgi edinin.

1. **Azure haritalar hesabınıza**gidin. **Erişim denetimi (IAM)**  >  **rol atamasını**seçin.

    ![Azure RBAC kullanarak erişim verme](../media/how-to-manage-authentication/how-to-grant-rbac.png)

2. **Rol atamaları** sekmesinde, **rol**altında **Azure Maps veri okuyucusu** veya **Azure haritalar veri katılımcısı**gibi yerleşik bir Azure Maps rol tanımı seçin. **Erişim ata**' nın altında **Azure AD Kullanıcı, Grup veya hizmet sorumlusu**' nı seçin. Asıl ada göre ' yi seçin. Ardından **Kaydet**’i seçin.

   * [Rol atamaları ekleme veya kaldırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)ile ilgili ayrıntılara bakın.

> [!WARNING]
> Azure Maps yerleşik rol tanımları birçok Azure haritalar REST API 'Lerine çok büyük bir yetkilendirme erişimi sağlar. API 'Leri kullanıcılara en düşük düzeyde kısıtlamak için bkz. [özel rol tanımı oluşturma ve kullanıcıları](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) özel rol tanımına atama. Bu, kullanıcıların uygulama için gereken en düşük ayrıcalığa sahip olmasını sağlar.