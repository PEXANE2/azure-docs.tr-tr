---
title: RBAC ve Azure portalı ile rol atamaları ekleme veya kaldırma
description: Azure rol tabanlı erişim denetimi (RBAC) ve Azure portalı kullanarak kullanıcılar, gruplar, hizmet müdürleri veya yönetilen kimlikler için Azure kaynaklarına nasıl erişim tanıyın öğrenin.
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
ms.date: 01/25/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e2458b07602bca3b6f12b2f486b509c11d705461
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246363"
---
# <a name="add-or-remove-role-assignments-using-azure-rbac-and-the-azure-portal"></a>Azure RBAC ve Azure portalını kullanarak rol atamaları ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Bu makalede, Azure portalını kullanarak rollerin nasıl atanılabildiğini açıklanmaktadır.

Azure Etkin Dizin'de yönetici rolleri atamanız gerekiyorsa, [Azure Etkin Dizin'de Görüntüle ve yönetici rollerini atayın.'a](../active-directory/users-groups-roles/directory-manage-roles-portal.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmış olmalısınız:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` Kullanıcı Erişim [Yöneticisi](built-in-roles.md#user-access-administrator) veya [Sahibi](built-in-roles.md#owner) gibi izinler

## <a name="access-control-iam"></a>Erişim denetimi (IAM)

**Erişim denetimi (IAM),** Azure kaynaklarına erişim sağlamak için rol atamak için kullandığınız bıçaktır. Kimlik ve erişim yönetimi olarak da bilinir ve Azure portalında çeşitli konumlarda görünür. Aşağıda, abonelik için Access denetimi (IAM) bıçağının bir örneği gösterilmektedir.

![Abonelik için erişim kontrolü (IAM) bıçak](./media/role-assignments-portal/access-control-subscription.png)

Access denetimi (IAM) bıçağıyla en etkili olmak için, bir rol atamaya çalışırken aşağıdaki üç soruyu yanıtlayabilirseniz yardımcı olur:

1. **Kimin erişime ihtiyacı var ki?**

    Kullanıcı, grup, hizmet sorumlusu veya yönetilen kimler için başvurur. Buna *güvenlik ilkesi*de denir.

1. **Nasıl bir role ihtiyaçları var?**

    İzinler roller halinde gruplandırılır. Birkaç [yerleşik rolden](built-in-roles.md) birini seçebilir veya kendi özel rollerinizi kullanabilirsiniz.

1. **Nereden erişime ihtiyaçları var?**

    Erişimin uygulandığı kaynak kümesine başvuruyor. Yönetim grubu, abonelik, kaynak grubu veya depolama hesabı gibi tek bir kaynak nerede olabilir. Buna *kapsam*denir.

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

Azure RBAC'da, bir Azure kaynağına erişim izni vermek için bir rol ataması eklersiniz. Bir rol atamak için aşağıdaki adımları izleyin.

1. Azure portalında **Tüm hizmetler'i** tıklatın ve ardından erişim izni vermek istediğiniz kapsamı seçin. Örneğin, **Yönetim gruplarını,** **Abonelikleri,** **Kaynak gruplarını**veya bir kaynağı seçebilirsiniz.

1. Bu kapsam için belirli kaynağı tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu kapsamdaki rol **atamalarını** görüntülemek için Rol atamaları sekmesini tıklatın.

    ![Erişim denetimi (IAM) ve Rol atamaları sekmesi](./media/role-assignments-portal/role-assignments.png)

1.  > **Rol Atama** **ekle'yi**tıklatın.

   Rolleri atama izniniz yoksa, rol atama ekle seçeneği devre dışı bırakılır.

   ![Menü ekle](./media/role-assignments-portal/add-menu.png)

    Rol ataması ekle bölmesi açılır.

   ![Rol atama bölmesi ekleme](./media/role-assignments-portal/add-role-assignment.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Select** listesinde bir kullanıcı, grup, hizmet sorumlusu veya yönetilen kimlik seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra, güvenlik ilkesiseçili kapsamda rol atanır.

    ![Kaydedilen rol ataması ekleme](./media/role-assignments-portal/add-role-assignment-save.png)

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Bir kullanıcıyı, aboneliğin yöneticisi olarak atama

Bir kullanıcıyı Azure aboneliğinin yöneticisi yapmak için, bu kullanıcıya abonelik kapsamında [Ki'nin Sahibi](built-in-roles.md#owner) rolünü atayın. Sahip rolü, kullanıcıya başkalarına erişim izni de dahil olmak üzere abonelikteki tüm kaynaklara tam erişim sağlar. Bu adımlar diğer rol atamasıyla aynıdır.

1. Azure portalında **Tüm hizmetler** ve ardından **Abonelikler'i**tıklatın.

1. Erişim izni vermek istediğiniz aboneliğe tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu aboneliğin rol **atamalarını** görüntülemek için Rol atamaları sekmesini tıklatın.

    ![Erişim denetimi (IAM) ve Rol atamaları sekmesi](./media/role-assignments-portal/role-assignments.png)

1.  > **Rol Atama** **ekle'yi**tıklatın.

   Rolleri atama izniniz yoksa, rol atama ekle seçeneği devre dışı bırakılır.

   ![Menü ekle](./media/role-assignments-portal/add-menu.png)

    Rol ataması ekle bölmesi açılır.

   ![Rol atama bölmesi ekleme](./media/role-assignments-portal/add-role-assignment.png)

1. **Rol** açılan listesinde **Sahip** rolünü seçin.

1. **Seç** listesinde bir kullanıcı seçin. Listede kullanıcıyı görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad ve e-posta adresi arayabilirsiniz.

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra kullanıcıya abonelik kapsamında Sahip rolü atanmış olur.

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

Azure RBAC'da, bir Azure kaynağından erişimi kaldırmak için bir rol atamasını kaldırırsınız. Rol atamasını kaldırmak için aşağıdaki adımları izleyin.

1. Erişimi kaldırmak istediğiniz yönetim grubu, abonelik, kaynak grubu veya kaynak gibi bir kapsamda **Açık Erişim denetimi (IAM).**

1. Bu aboneliğin tüm rol atamalarını görüntülemek için **Rol atamaları** sekmesine tıklayın.

1. Rol ataması listesinde kaldırmak istediğiniz rol atamasına sahip güvenlik sorumlusunun yanına onay işareti ekleyin.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment-select.png)

1. **Kaldır**’a tıklayın.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment.png)

1. Görünen rol atamasını kaldır iletisinde **Evet'i**tıklatın.

    Devralınan rol atamalarının kaldırılamayacağını belirten bir ileti görürseniz, alt kapsamdaki bir rol atamasını kaldırmaya çalışıyorsunuz. Rolün atandığı kapsamda Access denetimini (IAM) açmalı ve yeniden denemelisiniz. Access denetimini (IAM) doğru kapsamda açmanın hızlı bir **yolu, Kapsam** sütununa bakmak ve **(Devralınan)** yanındaki bağlantıyı tıklatmaktır.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve Azure portalı kullanarak rol atamalarını listele](role-assignments-list-portal.md)
- [Öğretici: RBAC ve Azure portalını kullanarak kullanıcıya Azure kaynaklarına erişim hakkı tanıyın](quickstart-assign-role-user-portal.md)
- [Azure kaynakları için RBAC sorun giderme](troubleshooting.md)
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleme](../governance/management-groups/overview.md)
