---
title: Hızlı başlatma - Bir kullanıcının Azure kaynaklarına sahip olduğu erişimi görüntüleme
description: Bu QuickStart'ta, rol tabanlı erişim denetimi (RBAC) ve Azure portalını kullanarak bir kullanıcının veya diğer güvenlik ilkesinin Azure kaynaklarına erişimini nasıl görüntüleyin öğrenin.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b23c10fc2a551b8044b208911dbc048968b06564
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74419611"
---
# <a name="quickstart-view-the-access-a-user-has-to-azure-resources"></a>Hızlı başlatma: Bir kullanıcının Azure kaynaklarına sahip olduğu erişimi görüntüleme

Bir kullanıcının veya başka bir güvenlik sorumlusunun Azure kaynaklarına erişimini görüntülemek için [rol tabanlı erişim denetiminde (RBAC)](overview.md) Access denetimini **(IAM)** kullanabilirsiniz. Ancak, bazen yalnızca tek bir kullanıcı veya başka bir güvenlik ilkesi için erişimi hızlı bir şekilde görüntülemeniz gerekir. Bunu yapmanın en kolay yolu Azure portalındaki **Denetimerişimi** özelliğini kullanmaktır.

## <a name="view-role-assignments"></a>Rol atamalarını görüntüle

 Bir kullanıcının erişimini görüntüleme şekli, rol atamalarını listelemektir. Abonelik kapsamında tek bir kullanıcı, grup, hizmet sorumlusu veya yönetilen kimlik için rol atamalarını görüntülemek için aşağıdaki adımları izleyin.

1. Azure portalında **Tüm hizmetler** ve ardından **Abonelikler'i**tıklatın.

1. Aboneliğinizi tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Erişimi **Denetle** sekmesini tıklatın.

    ![Erişim denetimi - Erişim sekmesini denetle](./media/check-access/access-control-check-access.png)

1. **Bul** listesinde, erişimi denetlemek istediğiniz güvenlik sorumlusu türünü seçin.

1. Arama kutusuna, görüntü adları, e-posta adresleri veya nesne tanımlayıcıları için dizini aramak için bir dize girin.

    ![Erişim seç listesini kontrol et](./media/check-access/check-access-select.png)

1. **Atamalar** bölmesini açmak için güvenlik ilkesini tıklatın.

    ![atamalar bölmesi](./media/check-access/check-access-assignments.png)

    Bu bölmede, seçili güvenlik ilkesine ve kapsamına atanan rolleri görebilirsiniz. Bu kapsamda herhangi bir reddedilme ataması varsa veya bu kapsamda devralınmışsa, bunlar listelenir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: RBAC ve Azure portalını kullanarak kullanıcıya Azure kaynaklarına erişim hakkı tanıyın](quickstart-assign-role-user-portal.md)
