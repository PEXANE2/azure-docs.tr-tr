---
title: Azure portal-Azure RBAC kullanarak Azure rol atamaları ekleme veya kaldırma
description: Azure portal ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak kullanıcılar, gruplar, hizmet sorumluları veya yönetilen kimlikler için Azure kaynaklarına nasıl erişim sağlayacağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/25/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 3e4d2dca6817951f2f06a86c4338106f194b7751
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84790968"
---
# <a name="add-or-remove-azure-role-assignments-using-the-azure-portal"></a>Azure portal kullanarak Azure rol atamaları ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Bu makalede, Azure portal kullanarak rollerin nasıl atanacağı açıklanır.

Azure Active Directory ' de yönetici rolleri atamanız gerekiyorsa, bkz. [Azure Active Directory yönetici rollerini görüntüleme ve atama](../active-directory/users-groups-roles/directory-manage-roles-portal.md).

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmanız gerekir:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi izinler

## <a name="access-control-iam"></a>Erişim denetimi (IAM)

**Erişim denetimi (IAM)** , Azure kaynaklarına erişim vermek üzere rol atamak için kullandığınız dikey pencere olur. Kimlik ve erişim yönetimi olarak da bilinir ve Azure portal çeşitli konumlarda görüntülenir. Aşağıda bir abonelik için erişim denetimi (ıAM) dikey penceresi örneği gösterilmektedir.

![Abonelik için erişim denetimi (ıAM) dikey penceresi](./media/role-assignments-portal/access-control-subscription.png)

Erişim denetimi (ıAM) dikey penceresinde en etkili olması için, bir rol atamaya çalışırken aşağıdaki üç soruyu yanıtlayıp yanıtlayabilmeniz yardımcı olur:

1. **Kimin erişmesi gerekiyor?**

    Bir kullanıcıya, gruba, hizmet sorumlusuna veya yönetilen kimliğe başvurur. Buna *güvenlik sorumlusu*da denir.

1. **Hangi role ihtiyaç duyuyor?**

    İzinler, rollerle birlikte gruplandırılır. Birkaç [yerleşik rol](built-in-roles.md) listesinden seçim yapabilir veya kendi özel rollerinizi kullanabilirsiniz.

1. **Bunlara erişmesi gereken durumlar var mı?**

    Burada erişimin uygulandığı kaynak kümesini ifade eder. Nerede bir yönetim grubu, abonelik, kaynak grubu veya depolama hesabı gibi tek bir kaynak olabilir. Buna *kapsam*denir.

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

Azure RBAC 'de, bir Azure kaynağına erişim izni vermek için bir rol ataması eklersiniz. Rol atamak için aşağıdaki adımları izleyin.

1. Azure portal, **tüm hizmetler** ' e tıklayın ve ardından erişim vermek istediğiniz kapsamı seçin. Örneğin, **Yönetim grupları**, **abonelikler**, **kaynak grupları**veya bir kaynak seçebilirsiniz.

1. Bu kapsam için özel kaynağa tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu kapsamdaki rol atamalarını görüntülemek için **rol atamaları** sekmesine tıklayın.

    ![Erişim denetimi (ıAM) ve rol atamaları sekmesi](./media/role-assignments-portal/role-assignments.png)

1. **Add**  >  **Rol Ekle ataması**Ekle ' ye tıklayın.

   Rol atama izniniz yoksa rol ataması Ekle seçeneği devre dışı bırakılır.

   ![Menü ekle](./media/role-assignments-portal/add-menu.png)

    Rol ataması ekle bölmesi açılır.

   ![Rol atama bölmesi Ekle](./media/role-assignments-portal/add-role-assignment.png)

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

1. **Seç** listesinde, bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra, güvenlik sorumlusu seçili kapsamda role atanır.

    ![Kaydedilmiş rol ataması ekleme](./media/role-assignments-portal/add-role-assignment-save.png)

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Bir kullanıcıyı, aboneliğin yöneticisi olarak atama

Bir kullanıcıyı bir Azure aboneliğinin Yöneticisi yapmak için, abonelik kapsamında bu rolü [sahip](built-in-roles.md#owner) rolüne atayın. Sahip rolü, kullanıcıya, başkalarına erişim verme izni dahil olmak üzere abonelikteki tüm kaynaklara tam erişim sağlar. Bu adımlar diğer rol atamasıyla aynıdır.

1. Azure portal, **tüm hizmetler** ve ardından **abonelikler**' e tıklayın.

1. Erişim izni vermek istediğiniz aboneliğe tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu aboneliğin rol atamalarını görüntülemek için **rol atamaları** sekmesine tıklayın.

    ![Erişim denetimi (ıAM) ve rol atamaları sekmesi](./media/role-assignments-portal/role-assignments.png)

1. **Add**  >  **Rol Ekle ataması**Ekle ' ye tıklayın.

   Rol atama izniniz yoksa rol ataması Ekle seçeneği devre dışı bırakılır.

   ![Menü ekle](./media/role-assignments-portal/add-menu.png)

    Rol ataması ekle bölmesi açılır.

   ![Rol atama bölmesi Ekle](./media/role-assignments-portal/add-role-assignment.png)

1. **Rol** açılan listesinde **Sahip** rolünü seçin.

1. **Seç** listesinde bir kullanıcı seçin. Listede kullanıcıyı görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad ve e-posta adresi arayabilirsiniz.

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra kullanıcıya abonelik kapsamında Sahip rolü atanmış olur.

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

Azure RBAC 'de, bir Azure kaynağından erişimi kaldırmak için bir rol atamasını kaldırırsınız. Rol atamasını kaldırmak için aşağıdaki adımları izleyin.

1. Erişim **denetimi 'ni (IAM)** , erişimi kaldırmak istediğiniz yönetim grubu, abonelik, kaynak grubu veya kaynak gibi bir kapsamda açın.

1. Bu aboneliğin tüm rol atamalarını görüntülemek için **Rol atamaları** sekmesine tıklayın.

1. Rol ataması listesinde kaldırmak istediğiniz rol atamasına sahip güvenlik sorumlusunun yanına onay işareti ekleyin.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment-select.png)

1. **Kaldır**’a tıklayın.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment.png)

1. Görüntülenen rol atamasını Kaldır iletisinde **Evet**' e tıklayın.

    Devralınan rol atamalarının kaldırılamadığını belirten bir ileti görürseniz, bir alt kapsamda rol atamasını kaldırmaya çalışıyorsunuz. Rolün atandığı kapsamdaki erişim denetimini (ıAM) açmanız ve yeniden denemeniz gerekir. Doğru kapsamda erişim denetimini (ıAM) açmak için hızlı bir yol, **kapsam** sütununa bakmanız ve **(devralınmış)** yanındaki bağlantıya tıklamanız.

   ![Rol atamasını kaldırma iletisi](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal kullanarak Azure rol atamalarını listeleyin](role-assignments-list-portal.md)
- [Öğretici: Azure portal kullanarak Azure kaynaklarına Kullanıcı erişimi verme](quickstart-assign-role-user-portal.md)
- [Azure RBAC sorunlarını giderme](troubleshooting.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../governance/management-groups/overview.md)
