---
title: Tutorial - Grant a user access to Azure resources using RBAC and the Azure portal
description: In this tutorial, learn how to grant a user access to Azure resources using role-based access control (RBAC) in the Azure portal.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/22/2019
ms.author: rolyon
ms.openlocfilehash: f4dd3995df2a068824c4aa6bccca5606d250a165
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74419655"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>Tutorial: Grant a user access to Azure resources using RBAC and the Azure portal

[Role-based access control (RBAC)](overview.md) is the way that you manage access to Azure resources. In this tutorial, you grant a user access to create and manage virtual machines in a resource group.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Grant access for a user at a resource group scope
> * Erişimi kaldırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

1. In the navigation list, click **Resource groups**.

1. Click **Add** to open the **Resource group** blade.

   ![Yeni kaynak grubu ekleme](./media/quickstart-assign-role-user-portal/resource-group.png)

1. For **Resource group name**, enter **rbac-resource-group**.

1. Bir abonelik ve bir konum seçin.

1. Click **Create** to create the resource group.

1. Click **Refresh** to refresh the list of resource groups.

   Yeni kaynak grubu kaynak grupları listenizde görünür.

   ![Kaynak grubu listesi](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Erişim verme

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir.

1. In the list of **Resource groups**, click the new **rbac-resource-group** resource group.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Click the **Role assignments** tab to see the current list of role assignments.

   ![Kaynak grubu için Erişim denetimi (IAM) dikey penceresi](./media/quickstart-assign-role-user-portal/access-control.png)

1. Click **Add** > **Add role assignment** to open the Add role assignment pane.

   If you don't have permissions to assign roles, the Add role assignment option will be disabled.

   ![Add menu](./media/role-assignments-portal/add-menu.png)

   ![Add role assignment pane](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. Aşağı açılan **Rol** listesinden **Sanal Makine Katılımcısı**'nı seçin.

1. **Seç** listesinde kendinizi ve başka bir kullanıcıyı seçin.

1. Click **Save** to create the role assignment.

   After a few moments, the user is assigned the Virtual Machine Contributor role at the rbac-resource-group resource group scope.

   ![Sanal Makine Katılımcısı rolü ataması](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Erişimi kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir.

1. In the list of role assignments, add a checkmark next to the user with the Virtual Machine Contributor role.

1. Click **Remove**.

   ![Rol atamasını kaldırma iletisi](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. In the remove role assignment message that appears, click **Yes**.

## <a name="clean-up"></a>Temizleme

1. In the navigation list, click **Resource groups**.

1. Click **rbac-resource-group** to open the resource group.

1. Click **Delete resource group** to delete the resource group.

   ![Kaynak grubunu silme](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. On the **Are you sure you want to delete** blade, type the resource group name: **rbac-resource-group**.

1. Click **Delete** to delete the resource group.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tutorial: Grant a user access to Azure resources using RBAC and Azure PowerShell](tutorial-role-assignments-user-powershell.md)
