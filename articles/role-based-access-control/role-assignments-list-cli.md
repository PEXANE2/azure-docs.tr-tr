---
title: Azure RBAC ve Azure CLI'yi kullanarak rol atamalarını listele
description: Kullanıcıların, grupların, hizmet ilkelerinin veya yönetilen kimliklerin Azure rol tabanlı erişim denetimi (RBAC) ve Azure CLI'yi kullanarak hangi kaynaklara erişebildiğini nasıl belirleyebilirsiniz.
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
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385070"
---
# <a name="list-role-assignments-using-azure-rbac-and-azure-cli"></a>Azure RBAC ve Azure CLI'yi kullanarak rol atamalarını listele

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)]Bu makalede, Azure CLI kullanarak rol atamalarınasıl listeleyiniz açıklanmaktadır.

> [!NOTE]
> Kuruluşunuz, [Azure temsilcili kaynak yönetimini](../lighthouse/concepts/azure-delegated-resource-management.md)kullanan bir hizmet sağlayıcısına dış kaynak yönetim işlevleri veriyorsa, bu hizmet sağlayıcısı tarafından yetkilendirilen rol atamaları burada gösterilmez.

## <a name="prerequisites"></a>Ön koşullar

- [Azure Bulut Uyp'ta Bash](/azure/cloud-shell/overview) veya [Azure CLI](/cli/azure)

## <a name="list-role-assignments-for-a-user"></a>Bir kullanıcının rol atamalarını listeleme

Belirli bir kullanıcının rol atamalarını listelemek için [az rol atama listesini](/cli/azure/role/assignment#az-role-assignment-list)kullanın:

```azurecli-interactive
az role assignment list --assignee <assignee>
```

Varsayılan olarak, yalnızca geçerli abonelik için rol atamaları görüntülenir. Geçerli abonelik ve aşağıdaki rol atamaları görüntülemek `--all` için parametre ekleyin. Devralınan rol atamalarını görüntülemek `--include-inherited` için parametreyi ekleyin.

Aşağıdaki örnekte, doğrudan *patlong\@contoso.com* kullanıcıya atanan rol atamaları listelenir:

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

Kaynak grubu kapsamında bulunan rol atamalarını listelemek için [az rol atama listesini](/cli/azure/role/assignment#az-role-assignment-list)kullanın:

```azurecli-interactive
az role assignment list --resource-group <resource_group>
```

Aşağıdaki *örnekte, farma-satış* kaynak grubunun rol atamaları listeleneb::

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

Abonelik kapsamındaki tüm rol atamalarını listelemek için [az rol atama listesini](/cli/azure/role/assignment#az-role-assignment-list)kullanın. Abonelik kimliğini almak için Azure portalındaki **Abonelikler** bıyığa veya [az hesap listesini](/cli/azure/account#az-account-list)kullanabilirsiniz.

```azurecli-interactive
az role assignment list --subscription <subscription_name_or_id>
```

Örnek:

```azurecli-interactive
az role assignment list --subscription 00000000-0000-0000-0000-000000000000 --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-management-group"></a>Yönetim grubu için rol atamalarını listele

Yönetim grubu kapsamındaki tüm rol atamalarını listelemek için [az rol atama listesini](/cli/azure/role/assignment#az-role-assignment-list)kullanın. Yönetim grubu kimliğini almak için, Azure portalındaki **Yönetim grupları** listesinde bulabilir veya az hesap yönetimi [grubu listesini](/cli/azure/account/management-group#az-account-management-group-list)kullanabilirsiniz.

```azurecli-interactive
az role assignment list --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

Örnek:

```azurecli-interactive
az role assignment list --scope /providers/Microsoft.Management/managementGroups/marketing-group --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-managed-identity"></a>Yönetilen bir kimlik için rol atamalarını listele

1. Sistem tarafından atanan veya kullanıcı tarafından atanan yönetilen kimliğin nesne kimliğini alın.

    Kullanıcı tarafından atanan yönetilen bir kimliğin nesne kimliğini almak için [az reklam sp listesini](/cli/azure/ad/sp#az-ad-sp-list) veya az kimlik [listesini](/cli/azure/identity#az-identity-list)kullanabilirsiniz.

    ```azurecli-interactive
    az ad sp list --display-name "<name>" --query [].objectId --output tsv
    ```

    Sistem tarafından atanan yönetilen bir kimliğin nesne kimliğini almak için [az reklam sp listesini](/cli/azure/ad/sp#az-ad-sp-list)kullanabilirsiniz.

    ```azurecli-interactive
    az ad sp list --display-name "<vmname>" --query [].objectId --output tsv
    ```

1. Rol atamalarını listelemek için [az rol atama listesini](/cli/azure/role/assignment#az-role-assignment-list)kullanın.

    Varsayılan olarak, yalnızca geçerli abonelik için rol atamaları görüntülenir. Geçerli abonelik ve aşağıdaki rol atamaları görüntülemek `--all` için parametre ekleyin. Devralınan rol atamalarını görüntülemek `--include-inherited` için parametreyi ekleyin.

    ```azurecli-interactive
    az role assignment list --assignee <objectid>
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve Azure CLI kullanarak rol atamaları ekleme veya kaldırma](role-assignments-cli.md)
