---
title: Azure RBAC ve Azure portalı kullanarak rol atamalarını listele
description: Kullanıcıların, grupların, hizmet ilkelerinin veya yönetilen kimliklerin Azure rol tabanlı erişim denetimi (RBAC) ve Azure portalını kullanarak hangi kaynaklara erişebildiğini nasıl belirleyebilirsiniz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/18/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 03a3d3c7d572d7ec5b8d3ac3d527d0d59e649bc2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062235"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-azure-portal"></a>Azure RBAC ve Azure portalı kullanarak rol atamalarını listele

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)]Bu makalede, Azure portalını kullanarak rol atamalarının nasıl listeleneniz açıklanmaktadır.

> [!NOTE]
> Kuruluşunuz, [Azure temsilcili kaynak yönetimini](../lighthouse/concepts/azure-delegated-resource-management.md)kullanan bir hizmet sağlayıcısına dış kaynak yönetim işlevleri veriyorsa, bu hizmet sağlayıcısı tarafından yetkilendirilen rol atamaları burada gösterilmez.

## <a name="list-role-assignments-for-a-user-or-group"></a>Bir kullanıcı veya grup için rol atamalarını listele

Bir abonelikte bir kullanıcıya veya gruba atanan rolleri görmenin en kolay yolu **Azure kaynakları** bölmesini kullanmaktır.

1. Azure portalında **Tüm hizmetler'i** tıklatın ve ardından **Kullanıcılar** veya **Gruplar'ı**seçin.

1. Rol atamalarının listesini istediğiniz kullanıcıyı veya grubu tıklatın.

1. **Azure kaynaklarını**tıklatın.

    Yönetim grubu, abonelik, kaynak grubu veya kaynak gibi çeşitli kapsamlarda seçili kullanıcıya veya gruba atanan rollerin listesini görürsünüz. Bu liste, okuma izniniz olan tüm rol atamalarını içerir.

    ![Bir kullanıcıya rol atama](./media/role-assignments-list-portal/azure-resources-user.png)    

1. Aboneliği değiştirmek için **Abonelikler** listesini tıklatın.

## <a name="list-owners-of-a-subscription"></a>Abonelik sahiplerini listele

Abonelik için [Sahip](built-in-roles.md#owner) rolüne atanan kullanıcılar abonelikteki her şeyi yönetebilir. Abonelik sahiplerini listelemek için aşağıdaki adımları izleyin.

1. Azure portalında **Tüm hizmetler** ve ardından **Abonelikler'i**tıklatın.

1. Sahiplerini listelemek istediğiniz aboneliği tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu aboneliğin tüm rol atamalarını görüntülemek için **Rol atamaları** sekmesine tıklayın.

1. Bu abonelik için Sahip rolü atanan tüm kullanıcıları görmek için **Sahipler** bölümüne gidin.

   ![Abonelik Erişimi denetimi - Rol atamaları sekmesi](./media/role-assignments-list-portal/access-control-role-assignments-subscription.png)

## <a name="list-role-assignments-at-a-scope"></a>Rol atamalarını bir kapsamda listelama

1. Azure portalında **Tüm hizmetler'i** tıklatın ve ardından kapsamı seçin. Örneğin, **Yönetim gruplarını,** **Abonelikleri,** **Kaynak gruplarını**veya bir kaynağı seçebilirsiniz.

1. Belirli kaynağı tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu kapsamdaki tüm rol atamalarını görüntülemek için **Rol atamaları** sekmesini tıklatın.

   ![Erişim denetimi - Rol atamaları sekmesi](./media/role-assignments-list-portal/access-control-role-assignments.png)

   Rol atamaları sekmesinde, bu kapsamda kimlerin erişimi olduğunu görebilirsiniz. Bazı rollerin kapsamı **Bu kaynak** olarak belirlenmişken diğerlerinin başka bir kapsamdan **(Devralınmış)** olduğuna dikkat edin. Erişim, bu kaynağa özel olarak atanır veya bir atamadan üst kapsama devredilir.

## <a name="list-role-assignments-for-a-user-at-a-scope"></a>Kapsamdaki bir kullanıcı için rol atamalarını listele

Bir kullanıcı, grup, hizmet sorumlusu veya yönetilen kimliğe erişimi listelemek için, rol atamalarını listelersiniz. Belirli bir kapsamda tek bir kullanıcı, grup, hizmet sorumlusu veya yönetilen kimlik için rol atamaları listelemek için aşağıdaki adımları izleyin.

1. Azure portalında **Tüm hizmetler'i** tıklatın ve ardından kapsamı seçin. Örneğin, **Yönetim gruplarını,** **Abonelikleri,** **Kaynak gruplarını**veya bir kaynağı seçebilirsiniz.

1. Belirli kaynağı tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Erişimi **Denetle** sekmesini tıklatın.

    ![Erişim denetimi - Erişim sekmesini denetle](./media/role-assignments-list-portal/access-control-check-access.png)

1. **Bul** listesinde, erişimi denetlemek istediğiniz güvenlik sorumlusu türünü seçin.

1. Arama kutusuna, görüntü adları, e-posta adresleri veya nesne tanımlayıcıları için dizini aramak için bir dize girin.

    ![Erişim seç listesini kontrol et](./media/role-assignments-list-portal/check-access-select.png)

1. **Atamalar** bölmesini açmak için güvenlik ilkesini tıklatın.

    ![atamalar bölmesi](./media/role-assignments-list-portal/check-access-assignments.png)

    Bu bölmede, seçili güvenlik ilkesine ve kapsamına atanan rolleri görebilirsiniz. Bu kapsamda herhangi bir reddedilme ataması varsa veya bu kapsamda devralınmışsa, bunlar listelenir.

## <a name="list-role-assignments-for-a-system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik için rol atamalarını listele

1. Azure portalında, sistem tarafından atanmış yönetilen bir kimlik açın.

1. Sol menüde **Kimlik'i**tıklatın.

    ![Sistem le atanmış yönetilen kimlik](./media/role-assignments-list-portal/identity-system-assigned.png)

1. **Rol atamaları**altında, bu **yönetilen kimliğe atanan Azure RBAC rollerini göster'i**tıklatın.

    Yönetim grubu, abonelik, kaynak grubu veya kaynak gibi çeşitli kapsamlarda seçili sistem tarafından atanan yönetilen kimliğe atanan rollerin listesini görürsünüz. Bu liste, okuma izniniz olan tüm rol atamalarını içerir.

    ![Sistem tarafından atanan yönetilen kimlik için rol atamaları](./media/role-assignments-list-portal/azure-resources-system-assigned.png)

## <a name="list-role-assignments-for-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik için rol atamalarını listele

1. Azure portalında, kullanıcı tarafından atanmış yönetilen bir kimlik açın.

1. **Azure kaynaklarını**tıklatın.

    Yönetim grubu, abonelik, kaynak grubu veya kaynak gibi çeşitli kapsamlarda, seçili kullanıcı tarafından atanan yönetilen kimliğe atanan rollerin listesini görürsünüz. Bu liste, okuma izniniz olan tüm rol atamalarını içerir.

    ![Sistem tarafından atanan yönetilen kimlik için rol atamaları](./media/role-assignments-list-portal/azure-resources-user-assigned.png)

1. Aboneliği değiştirmek için **Abonelikler** listesini tıklatın.

## <a name="list-number-of-role-assignments"></a>Rol atamalarının listesi

Her abonelikte en fazla **2000** rol atamanız olabilir. Bu sınırı izlemenize yardımcı olmak **için, Rol atamaları** sekmesi geçerli abonelik için rol atamaları sayısını listeleyen bir grafik içerir.

![Erişim denetimi - Rol atamaları grafiğisayısı](./media/role-assignments-list-portal/access-control-role-assignments-chart.png)

En yüksek sayıya yaklaşıyorsanız ve daha fazla rol ataması eklemeye çalışıyorsanız, **rol atamaekle** bölmesinde bir uyarı görürsünüz. Rol atamalarının sayısını azaltmanın yolları için Azure [RBAC Sorun Giderme](troubleshooting.md#azure-role-assignments-limit)bölümüne bakın.

![Erişim denetimi - Rol atama uyarısı ekleme](./media/role-assignments-list-portal/add-role-assignment-warning.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve Azure portalını kullanarak rol atamaları ekleme veya kaldırma](role-assignments-portal.md)
- [Azure kaynakları için RBAC sorun giderme](troubleshooting.md)
