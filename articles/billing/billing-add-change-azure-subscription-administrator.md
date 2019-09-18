---
title: Azure Aboneliği yöneticileri ekleme veya değiştirme | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) kullanarak Azure abonelik yöneticisi ekleme veya değiştirme işlemini açıklar.
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 000315a2d751a05d3e401ee1bb9f593c6e321194
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "64922911"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Azure Aboneliği yöneticileri ekleme veya değiştirme

Azure kaynaklarına erişimi yönetmek için uygun yönetici rolüne sahip olmanız gerekir. Azure, aralarından seçim yapabileceğiniz çeşitli yerleşik rollerle rol tabanlı erişim denetimi (RBAC) adlı bir yetkilendirme sistemine sahiptir. Bu rolleri, yönetim grubu, abonelik veya kaynak grubu gibi farklı kapsamlara atayabilirsiniz.

Microsoft, RBAC kullanarak kaynaklara erişimi yönetmenizi önerir. Ancak halen klasik dağıtım modelini kullanıyor ve [Azure Hizmet Yönetimi PowerShell Modülü](https://docs.microsoft.com/powershell/module/servicemanagement/azure)’nü kullanarak klasik kaynakları yönetiyorsanız, klasik bir yönetici kullanmanız gerekir. 

> [!TIP]
> Klasik kaynakları yönetmek için yalnızca Azure portalını kullanıyorsanız, klasik yöneticiyi kullanmanız gerekmez.

Daha fazla bilgi için bkz. [Azure Resource Manager ve klasik dağıtım karşılaştırması](../azure-resource-manager/resource-manager-deployment-model.md) ve [Azure klasik abonelik yöneticileri](../role-based-access-control/classic-administrators.md).

Bu makalede, abonelik kapsamında RBAC kullanılarak bir kullanıcı için yönetici rolünün nasıl ekleneceği veya değiştirileceği açıklanmaktadır.

<a name="add-an-admin-for-a-subscription"></a>

## <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Bir kullanıcıyı, aboneliğin yöneticisi olarak atama

Bir kullanıcıyı, Azure aboneliğinin yöneticisi yapmak için o kullanıcıya abonelik kapsamında [Sahip](../role-based-access-control/built-in-roles.md#owner) rolünü (RBAC rolü) atamanız gerekir. Sahip rolü, kullanıcıya, başkalarına erişim yetkisi devretme hakkı da dahil olmak üzere abonelikteki tüm kaynaklara tam erişim izni verir. Bu adımlar diğer rol atamasıyla aynıdır.

1. Azure portalında [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)’i açın.

1. Erişim izni vermek istediğiniz aboneliğe tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Bu aboneliğin tüm rol atamalarını görüntülemek için **Rol atamaları** sekmesine tıklayın.

    ![Rol atamalarını gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/role-assignments.png)

1. **Ekle** > **Rol ataması ekle** seçeneğine tıklayarak **Rol ataması ekle** bölmesini açın.

    Rol atama iznine sahip değilseniz bu seçenek devre dışı bırakılır.

1. **Rol** açılan listesinde **Sahip** rolünü seçin.

1. **Seç** listesinde bir kullanıcı seçin. Listede kullanıcıyı görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad ve e-posta adresi arayabilirsiniz.

    ![Sahip rolünün seçili olduğunu gösteren ekran görüntüsü](./media/billing-add-change-azure-subscription-administrator/add-role.png)

1. Rolü atamak için **Kaydet**’e tıklayın.

    Birkaç dakika sonra kullanıcıya abonelik kapsamında Sahip rolü atanmış olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)
* [Azure'daki farklı rolleri anlama](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Nasıl yapılır: Azure Active Directory’ye bir Azure aboneliğini ekleme veya ilişkilendirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Azure Active Directory'deki yönetici rolü izinleri](../active-directory/users-groups-roles/directory-assign-admin-roles.md)

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Yine de yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
