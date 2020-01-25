---
title: Azure RBAC ve Azure portal kullanarak rol atamalarını listeleyin
description: Kullanıcıların, grupların, hizmet sorumlularının veya yönetilen kimliklerin Azure rol tabanlı erişim denetimi (RBAC) ve Azure portal kullanarak hangi kaynakların erişimi olduğunu nasıl belirleyebileceğinizi öğrenin.
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
ms.date: 01/23/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 099cf74dd27f39a4289397d5178511125d9ebf6f
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720750"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-azure-portal"></a>Azure RBAC ve Azure portal kullanarak rol atamalarını listeleyin

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] Bu makalede, Azure portal kullanılarak rol atamalarının nasıl listeleneceğini açıklanmaktadır.

> [!NOTE]
> Kuruluşunuzun, [Azure tarafından yetkilendirilen kaynak yönetimi](../lighthouse/concepts/azure-delegated-resource-management.md)kullanan bir hizmet sağlayıcısına dış kaynaklı yönetim işlevleri varsa, bu hizmet sağlayıcısı tarafından yetkilendirilen rol atamaları burada gösterilmez.

## <a name="list-role-assignments-for-a-user-or-group"></a>Bir kullanıcı veya grup için rol atamalarını listeleme

Bir abonelikte bir kullanıcıya veya gruba atanan rolleri görmenin en kolay yolu **Azure kaynakları** bölmesini kullanmaktır.

1. Azure portal, **tüm hizmetler** ' e tıklayın ve ardından **Kullanıcılar** veya **gruplar**' ı seçin.

1. İçin rol atamalarını listelemek istediğiniz kullanıcıya veya gruba tıklayın.

1. **Azure kaynakları**' na tıklayın.

    Yönetim grubu, abonelik, kaynak grubu veya kaynak gibi çeşitli kapsamlardaki seçili kullanıcı veya gruba atanan rollerin listesini görürsünüz. Bu liste, okuma izninizin olduğu tüm rol atamalarını içerir.

    ![Bir kullanıcıya rol atama](./media/role-assignments-list-portal/azure-resources-user.png)    

1. Aboneliği değiştirmek için **abonelikler** listesine tıklayın.

## <a name="list-owners-of-a-subscription"></a>Abonelik sahiplerini listeleme

Bir abonelik için [sahip](built-in-roles.md#owner) rolüne atanan kullanıcılar, abonelikteki her şeyi yönetebilir. Bir aboneliğin sahiplerini listelemek için bu adımları izleyin.

1. Azure portal, **tüm hizmetler** ve ardından **abonelikler**' e tıklayın.

1. Sahiplerini listelemek istediğiniz aboneliğe tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu aboneliğin tüm rol atamalarını görüntülemek için **Rol atamaları** sekmesine tıklayın.

1. Bu abonelik için sahip rolüne atanmış tüm kullanıcıları görmek için **sahipler** bölümüne ilerleyin.

   ![Abonelik erişim denetimi-rol atamaları sekmesi](./media/role-assignments-list-portal/access-control-role-assignments-subscription.png)

## <a name="list-role-assignments-at-a-scope"></a>Rol atamalarını bir kapsamda listeleme

1. Azure portal, **tüm hizmetler** ' e tıklayın ve kapsamı seçin. Örneğin, **Yönetim grupları**, **abonelikler**, **kaynak grupları**veya bir kaynak seçebilirsiniz.

1. Belirli kaynağa tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu kapsamdaki tüm rol atamalarını görüntülemek için **rol atamaları** sekmesine tıklayın.

   ![Erişim denetimi-rol atamaları sekmesi](./media/role-assignments-list-portal/access-control-role-assignments.png)

   Rol atamaları sekmesinde kimlerin bu kapsamda erişimi olduğunu görebilirsiniz. Bazı rollerin kapsamı **Bu kaynak** olarak belirlenmişken diğerlerinin başka bir kapsamdan **(Devralınmış)** olduğuna dikkat edin. Erişim özellikle bu kaynağa atanır veya bir atamadan üst kapsama devralınır.

## <a name="list-role-assignments-for-a-user-at-a-scope"></a>Kapsamdaki Kullanıcı için rol atamalarını listeleme

Bir kullanıcının, grubun, hizmet sorumlusunun veya yönetilen kimliğin erişimini listelemek için, rol atamalarını listeleyin. Belirli bir kapsamdaki tek bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik için rol atamalarını listelemek üzere bu adımları izleyin.

1. Azure portal, **tüm hizmetler** ' e tıklayın ve kapsamı seçin. Örneğin, **Yönetim grupları**, **abonelikler**, **kaynak grupları**veya bir kaynak seçebilirsiniz.

1. Belirli kaynağa tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. **Erişimi denetle** sekmesine tıklayın.

    ![Erişim denetimi-erişim sekmesine göz atın](./media/role-assignments-list-portal/access-control-check-access.png)

1. **Bul** listesinde, erişimini denetlemek istediğiniz güvenlik sorumlusu türünü seçin.

1. Arama kutusuna, görünen adlar, e-posta adresleri veya nesne tanımlayıcıları için dizinde arama yapmak üzere bir dize girin.

    ![Erişim denetimi seçim listesi](./media/role-assignments-list-portal/check-access-select.png)

1. **Atamalar** bölmesini açmak için güvenlik sorumlusuna tıklayın.

    ![Atamalar bölmesi](./media/role-assignments-list-portal/check-access-assignments.png)

    Bu bölmede, seçilen güvenlik sorumlusuna ve kapsamına atanan rolleri görebilirsiniz. Bu kapsamda herhangi bir reddetme ataması varsa veya bu kapsama devralınmışsa, bunlar listelenecektir.

## <a name="list-role-assignments-for-a-system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik için rol atamalarını listeleyin

1. Azure portal, sistem tarafından atanan bir yönetilen kimlik açın.

1. Sol taraftaki menüden **kimlik**' e tıklayın.

    ![Sistem tarafından atanan yönetilen kimlik](./media/role-assignments-list-portal/identity-system-assigned.png)

1. **Rol atamaları**altında **Bu yönetilen KIMLIĞE atanan Azure RBAC rollerini göster ' e**tıklayın.

    Yönetim grubu, abonelik, kaynak grubu veya kaynak gibi çeşitli kapsamlardaki seçili sistem tarafından atanan yönetilen kimliğe atanan rollerin listesini görürsünüz. Bu liste, okuma izninizin olduğu tüm rol atamalarını içerir.

    ![Sistem tarafından atanan yönetilen kimlik için rol atamaları](./media/role-assignments-list-portal/azure-resources-system-assigned.png)

## <a name="list-role-assignments-for-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik için rol atamalarını listeleyin

1. Azure portal, sistem tarafından atanan bir yönetilen kimlik açın.

1. **Azure kaynakları**' na tıklayın.

    Yönetim grubu, abonelik, kaynak grubu veya kaynak gibi çeşitli kapsamlardaki seçili kullanıcı tarafından atanan yönetilen kimliğe atanan rollerin listesini görürsünüz. Bu liste, okuma izninizin olduğu tüm rol atamalarını içerir.

    ![Sistem tarafından atanan yönetilen kimlik için rol atamaları](./media/role-assignments-list-portal/azure-resources-user-assigned.png)

1. Aboneliği değiştirmek için **abonelikler** listesine tıklayın.

## <a name="list-number-of-role-assignments"></a>Rol atamalarının sayısını listeleyin

Her abonelikte en fazla **2000** rol ataması yapabilirsiniz. Bu sınırları izlemenize yardımcı olması için, **rol atamaları** sekmesi geçerli rol atamalarının sayısını listeleyen bir grafik içerir.

![Erişim denetimi-rol atamaları grafiğinin sayısı](./media/role-assignments-list-portal/access-control-role-assignments-chart.png)

En büyük sayıya yakınlarınız varsa ve daha fazla rol ataması eklemeye çalışırsanız, **rol ataması Ekle** bölmesinde bir uyarı görürsünüz. Artık gerekli olmayan rol atamalarını silerek veya bireysel kullanıcılar yerine rolleri gruplara atayarak rol atamalarının sayısını azaltabilirsiniz.

![Erişim denetimi-rol atama uyarısı ekleme](./media/role-assignments-list-portal/add-role-assignment-warning.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve Azure portal kullanarak rol atamaları ekleme veya kaldırma](role-assignments-portal.md)
- [Azure kaynakları için RBAC sorunlarını giderme](troubleshooting.md)
