---
title: Azure RBAC ve Azure CLı kullanarak rol atamalarını listeleme
description: Kullanıcıların, grupların, hizmet sorumlularının veya yönetilen kimliklerin Azure rol tabanlı erişim denetimi (RBAC) ve Azure CLı kullanarak hangi kaynakların erişimi olduğunu nasıl belirleyebileceğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/10/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 5716e7bb89d017866bd1575256e2d119bb7acbe5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80385070"
---
# <a name="list-role-assignments-using-azure-rbac-and-azure-cli"></a>Azure RBAC ve Azure CLı kullanarak rol atamalarını listeleme

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)]Bu makalede, Azure CLı kullanarak rol atamalarının nasıl listeleneceğini açıklanmaktadır.

> [!NOTE]
> Kuruluşunuzun, [Azure tarafından yetkilendirilen kaynak yönetimi](../lighthouse/concepts/azure-delegated-resource-management.md)kullanan bir hizmet sağlayıcısına dış kaynaklı yönetim işlevleri varsa, bu hizmet sağlayıcısı tarafından yetkilendirilen rol atamaları burada gösterilmez.

## <a name="prerequisites"></a>Ön koşullar

- Azure Cloud Shell veya [Azure CLI](/cli/azure) ['da Bash](/azure/cloud-shell/overview)

## <a name="list-role-assignments-for-a-user"></a>Bir kullanıcının rol atamalarını listeleme

Belirli bir kullanıcı için rol atamalarını listelemek için [az role atama listesi](/cli/azure/role/assignment#az-role-assignment-list)kullanın:

```azurecli-interactive
az role assignment list --assignee <assignee>
```

Varsayılan olarak, yalnızca geçerli abonelik için rol atamaları görüntülenir. Geçerli aboneliğin ve aşağıdaki rol atamalarını görüntülemek için `--all` parametresini ekleyin. Devralınan rol atamalarını görüntülemek için `--include-inherited` parametresini ekleyin.

Aşağıdaki örnekte, doğrudan *patlong\@contoso.com* kullanıcısına atanan rol atamaları listelenmektedir:

```azurecli-interactive
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
```

## <a name="list-role-assignments-for-a-resource-group"></a>Bir kaynak grubunun rol atamalarını listeleme

Bir kaynak grubu kapsamında var olan rol atamalarını listelemek için [az role atama listesi](/cli/azure/role/assignment#az-role-assignment-list)kullanın:

```azurecli-interactive
az role assignment list --resource-group <resource_group>
```

Aşağıdaki örnekte, *ilaç-Sales* kaynak grubu için rol atamaları listelenmektedir:

```azurecli-interactive
az role assignment list --resource-group pharma-sales --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}

...
```

## <a name="list-role-assignments-for-a-subscription"></a>Bir aboneliğin rol atamalarını listeleme

Tüm rol atamalarını bir abonelik kapsamında listelemek için [az role atama listesi](/cli/azure/role/assignment#az-role-assignment-list)' ni kullanın. Abonelik KIMLIĞINI almak için Azure portal **abonelikler** dikey penceresinde bulabilir veya [az Account List](/cli/azure/account#az-account-list)' i kullanabilirsiniz.

```azurecli-interactive
az role assignment list --subscription <subscription_name_or_id>
```

Örnek:

```azurecli-interactive
az role assignment list --subscription 00000000-0000-0000-0000-000000000000 --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-management-group"></a>Bir yönetim grubu için rol atamalarını listeleyin

Bir yönetim grubu kapsamındaki tüm rol atamalarını listelemek için [az role atama listesi](/cli/azure/role/assignment#az-role-assignment-list)' ni kullanın. Yönetim grubu KIMLIĞINI almak için Azure portal **Yönetim grupları** dikey penceresinde bulabilir veya [az Account Management-Group List](/cli/azure/account/management-group#az-account-management-group-list)kullanabilirsiniz.

```azurecli-interactive
az role assignment list --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

Örnek:

```azurecli-interactive
az role assignment list --scope /providers/Microsoft.Management/managementGroups/marketing-group --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-managed-identity"></a>Yönetilen bir kimlik için rol atamalarını listeleyin

1. Sistem tarafından atanan veya Kullanıcı tarafından atanan yönetilen kimliğin nesne KIMLIĞINI alır.

    Kullanıcı tarafından atanan yönetilen kimliğin nesne KIMLIĞINI almak için [az ad SP listesi](/cli/azure/ad/sp#az-ad-sp-list) ' ni veya [az Identity List](/cli/azure/identity#az-identity-list)' i kullanabilirsiniz.

    ```azurecli-interactive
    az ad sp list --display-name "<name>" --query [].objectId --output tsv
    ```

    Sistem tarafından atanan yönetilen kimliğin nesne KIMLIĞINI almak için [az ad SP List](/cli/azure/ad/sp#az-ad-sp-list)' i kullanabilirsiniz.

    ```azurecli-interactive
    az ad sp list --display-name "<vmname>" --query [].objectId --output tsv
    ```

1. Rol atamalarını listelemek için [az role atama listesi](/cli/azure/role/assignment#az-role-assignment-list)' ni kullanın.

    Varsayılan olarak, yalnızca geçerli abonelik için rol atamaları görüntülenir. Geçerli aboneliğin ve aşağıdaki rol atamalarını görüntülemek için `--all` parametresini ekleyin. Devralınan rol atamalarını görüntülemek için `--include-inherited` parametresini ekleyin.

    ```azurecli-interactive
    az role assignment list --assignee <objectid>
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve Azure CLı kullanarak rol atamaları ekleme veya kaldırma](role-assignments-cli.md)
