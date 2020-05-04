---
title: Hızlı başlangıç-bir kullanıcının Azure kaynaklarına erişimi görüntüleme-Azure RBAC
description: Bu hızlı başlangıçta, Azure portal ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak bir kullanıcının veya diğer güvenlik sorumlusunun Azure kaynaklarına erişimi nasıl görüntüleyebileceğinizi öğreneceksiniz.
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
ms.openlocfilehash: 9be53aa964e75bab0b90495640537fe927a5af0e
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82734170"
---
# <a name="quickstart-view-the-access-a-user-has-to-azure-resources"></a>Hızlı başlangıç: bir kullanıcının Azure kaynaklarına erişimi görüntüleme

Bir kullanıcıya veya başka bir güvenlik sorumlusunun Azure kaynaklarına erişimi görüntülemek için [Azure rol tabanlı erişim denetimi 'nde (Azure RBAC)](overview.md) **ERIŞIM denetimi (IAM)** dikey penceresini kullanabilirsiniz. Ancak bazen tek bir kullanıcı veya başka bir güvenlik sorumlusu için erişimi hızlıca görüntülemeniz yeterlidir. Bunu yapmanın en kolay yolu Azure portal **erişimi denetle** özelliğini kullanmaktır.

## <a name="view-role-assignments"></a>Rol atamalarını görüntüle

 Bir kullanıcı için erişimi görüntülemenin yolu, rollerinin atamalarını listeme yöntemidir. Abonelik kapsamında tek bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik için rol atamalarını görüntülemek için bu adımları izleyin.

1. Azure portal, **tüm hizmetler** ve ardından **abonelikler**' e tıklayın.

1. Aboneliğinize tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. **Erişimi denetle** sekmesine tıklayın.

    ![Erişim denetimi-erişim sekmesine göz atın](./media/check-access/access-control-check-access.png)

1. **Bul** listesinde, erişimini denetlemek istediğiniz güvenlik sorumlusu türünü seçin.

1. Arama kutusuna, görünen adlar, e-posta adresleri veya nesne tanımlayıcıları için dizinde arama yapmak üzere bir dize girin.

    ![Erişim denetimi seçim listesi](./media/check-access/check-access-select.png)

1. **Atamalar** bölmesini açmak için güvenlik sorumlusuna tıklayın.

    ![Atamalar bölmesi](./media/check-access/check-access-assignments.png)

    Bu bölmede, seçilen güvenlik sorumlusuna ve kapsamına atanan rolleri görebilirsiniz. Bu kapsamda herhangi bir reddetme ataması varsa veya bu kapsama devralınmışsa, bunlar listelenecektir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure portal kullanarak Azure kaynaklarına Kullanıcı erişimi verme](quickstart-assign-role-user-portal.md)
