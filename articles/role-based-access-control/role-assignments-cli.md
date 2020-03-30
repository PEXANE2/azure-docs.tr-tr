---
title: Azure RBAC ve Azure CLI kullanarak rol atamaları ekleme veya kaldırma
description: Azure rol tabanlı erişim denetimi (RBAC) ve Azure CLI'yi kullanarak kullanıcılar, gruplar, hizmet müdürleri veya yönetilen kimlikler için Azure kaynaklarına nasıl erişim tanıyın öğrenin.
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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b32df50715d5e7276861e0696df1bd6ceb3f684e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245674"
---
# <a name="add-or-remove-role-assignments-using-azure-rbac-and-azure-cli"></a>Azure RBAC ve Azure CLI kullanarak rol atamaları ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Bu makalede, Azure CLI kullanarak rollerin nasıl atanılabildiğini açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmış olmalısınız:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` Kullanıcı Erişim [Yöneticisi](built-in-roles.md#user-access-administrator) veya [Sahibi](built-in-roles.md#owner) gibi izinler
- [Azure Bulut Uyp'ta Bash](/azure/cloud-shell/overview) veya [Azure CLI](/cli/azure)

## <a name="get-object-ids"></a>Nesne tünelerini alma

Rol atamaları eklemek veya kaldırmak için nesnenin benzersiz kimliğini belirtmeniz gerekebilir. Kimlik biçimi vardır: `11111111-1111-1111-1111-111111111111`. Kimliği Azure portalını veya Azure CLI'yi kullanarak alabilirsiniz.

### <a name="user"></a>Kullanıcı

Bir Azure REKLAM kullanıcısının nesne kimliğini almak için [az reklam kullanıcı gösterisini](/cli/azure/ad/user#az-ad-user-show)kullanabilirsiniz.

```azurecli
az ad user show --id "{email}" --query objectId --output tsv
```

### <a name="group"></a>Grup

Bir Azure REKLAM grubunun nesne kimliğini almak için [az reklam grubu gösterisini](/cli/azure/ad/group#az-ad-group-show) veya [az reklam grubu listesini](/cli/azure/ad/group#az-ad-group-list)kullanabilirsiniz.

```azurecli
az ad group show --group "{name}" --query objectId --output tsv
```

### <a name="application"></a>Uygulama

Azure AD hizmet sorumlusunun (bir uygulama tarafından kullanılan kimlik) nesne kimliğini almak için [az reklam sp listesini](/cli/azure/ad/sp#az-ad-sp-list)kullanabilirsiniz. Bir hizmet sorumlusu için, uygulama kimliğini **değil,** nesne kimliğini kullanın.

```azurecli
az ad sp list --display-name "{name}" --query [].objectId --output tsv
```

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

RBAC'da, erişim sağlamak için bir rol ataması eklersiniz.

### <a name="user-at-a-resource-group-scope"></a>Kaynak grubu kapsamındaki kullanıcı

Kaynak grubu kapsamındaki bir kullanıcı için rol ataması eklemek için [az rol ataması oluşturma'yı](/cli/azure/role/assignment#az-role-assignment-create)kullanın.

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
```

Aşağıdaki örnek, *sanal makine katılımcısı* *rolünü, ilaç satış* kaynak grubu *kapsamındaki\@contoso.com* kullanıcıya atar:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="using-the-unique-role-id"></a>Benzersiz rol kimliğini kullanma

Bir rol adının değişebileceği birkaç kez vardır, örneğin:

- Kendi özel rolünüzü kullanıyorsunuz ve adı değiştirmeye karar vermissiniz.
- Adda **(Önizleme)** bulunan bir önizleme rolü kullanıyorsunuz. Rol yayımlandığında, rol yeniden adlandırılır.

> [!IMPORTANT]
> Önizleme sürümü hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Bir rol yeniden adlandırılsa bile, rol kimliği değişmez. Rol atamalarınızı oluşturmak için komut dosyalarını veya otomasyonu kullanıyorsanız, rol adı yerine benzersiz rol kimliğini kullanmak en iyi yöntemdir. Bu nedenle, bir rol yeniden adlandırıldıysa, komut dosyalarınızın çalışma olasılığı daha yüksektir.

Rol adı yerine benzersiz rol kimliği kullanarak bir rol ataması eklemek [için az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create)yı kullanın.

```azurecli
az role assignment create --role <role_id> --assignee <assignee> --resource-group <resource_group>
```

Aşağıdaki örnek, [Sanal Makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) *rolünü, ilaç satış* kaynak grubu kapsamındaki *patlong\@contoso.com* kullanıcısına atar. Benzersiz rol kimliği almak için [az rol tanımı listesini](/cli/azure/role/definition#az-role-definition-list) kullanabilir veya [Azure kaynakları için Yerleşik rolleri](built-in-roles.md)görebilirsiniz.

```azurecli
az role assignment create --role 9980e02c-c2be-4d73-94e8-173b1dc7cf3c --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="group-at-a-subscription-scope"></a>Abonelik kapsamında gruplandırma

Bir grup için rol ataması eklemek için [az rol ataması oluşturma'yı](/cli/azure/role/assignment#az-role-assignment-create)kullanın. Grubun nesne kimliğini nasıl alacağıhakkında bilgi için [bkz.](#get-object-ids)

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek, Bir abonelik kapsamında ID 222222222-2222-2222-2222222222222222222222222222222222222ile ile *Ann Mack Takım* grubuna *Reader* rolünü atar.

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000
```

### <a name="group-at-a-resource-scope"></a>Kaynak kapsamında gruplandırma

Bir grup için rol ataması eklemek için [az rol ataması oluşturma'yı](/cli/azure/role/assignment#az-role-assignment-create)kullanın. Grubun nesne kimliğini nasıl alacağıhakkında bilgi için [bkz.](#get-object-ids)

Aşağıdaki örnek, *Sanal Makine Katılımcısı* rolünü ANN Mack Team grubuna 22222222-2222-2222-222222222 222222 22222 222 2222 2222 222 222 222 222 222 222 222 222 222 222 222 222 222 kimliğiyle *Ann Mack Team* grubuna, *pharma-sales-project-network*adlı sanal bir ağ için kaynak kapsamında atar.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/pharma-sales/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="application-at-a-resource-group-scope"></a>Kaynak grubu kapsamında uygulama

Bir uygulama için rol ataması eklemek için [az rol ataması oluşturma'yı](/cli/azure/role/assignment#az-role-assignment-create)kullanın. Uygulamanın nesne kimliğini nasıl alacağı hakkında bilgi için [bkz.](#get-object-ids)

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group>
```

Aşağıdaki örnek, sanal *makine katılımcısı* *rolünü, pharma-satış* kaynak grubu kapsamında object ID 44444444-4444-4444-4444444444444444444444 4444 4 444 4 444 4 44 ile bir uygulamaya atar.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales
```

### <a name="user-at-a-subscription-scope"></a>Abonelik kapsamındaki kullanıcı

Abonelik kapsamındaki bir kullanıcı için rol ataması eklemek için [az rol ataması oluşturma'yı](/cli/azure/role/assignment#az-role-assignment-create)kullanın. Abonelik kimliğini almak için Azure portalındaki **Abonelikler** bıyığa veya [az hesap listesini](/cli/azure/account#az-account-list)kullanabilirsiniz.

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --subscription <subscription_name_or_id>
```

Aşağıdaki örnek, bir abonelik kapsamında *annm\@example.com* kullanıcıya *Reader* rolünü atar.

```azurecli
az role assignment create --role "Reader" --assignee annm@example.com --subscription 00000000-0000-0000-0000-000000000000
```

### <a name="user-at-a-management-group-scope"></a>Yönetim grubu kapsamında ki kullanıcı

Yönetim grubu kapsamındaki bir kullanıcı için rol ataması eklemek için [az rol ataması oluşturma'yı](/cli/azure/role/assignment#az-role-assignment-create)kullanın. Yönetim grubu kimliğini almak için, Azure portalındaki **Yönetim grupları** listesinde bulabilir veya az hesap yönetimi [grubu listesini](/cli/azure/account/management-group#az-account-management-group-list)kullanabilirsiniz.

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

Aşağıdaki örnek, bir yönetim grubu kapsamında *alain\@example.com* kullanıcıya Fatura *Okuyucu* rolünü atar.

```azurecli
az role assignment create --role "Billing Reader" --assignee alain@example.com --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

### <a name="new-service-principal"></a>Yeni hizmet müdürü

Yeni bir hizmet ilkesi oluşturur ve hemen bu hizmet ilkesine bir rol atamaya çalışırsanız, bu rol ataması bazı durumlarda başarısız olabilir. Örneğin, yeni yönetilen bir kimlik oluşturmak için bir komut dosyası kullanır ve sonra bu hizmet ilkesine bir rol atamaya çalışırsanız, rol ataması başarısız olabilir. Bu hatanın nedeni büyük olasılıkla bir çoğaltma gecikmesidir. Hizmet ilkesi tek bir bölgede oluşturulur; ancak, rol ataması hizmet ilkesini henüz çoğaltmamış farklı bir bölgede oluşabilir. Bu senaryoyu ele almak için, rol atamasını oluştururken temel türünü belirtmeniz gerekir.

Rol ataması eklemek için `--assignee-object-id`az [rol ataması oluşturmayı,](/cli/azure/role/assignment#az-role-assignment-create)bir `ServicePrincipal`değer belirtin ve sonra ' a ayarlayın. `--assignee-principal-type`

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --assignee-principal-type <assignee_principal_type> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek, *Sanal Makine Katılımcısı* *rolünü, ilaç satış* kaynak grubu *kapsamındaki msi-test* yönetilen kimliğine atar:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 33333333-3333-3333-3333-333333333333 --assignee-principal-type ServicePrincipal --resource-group pharma-sales
```

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

RBAC'da, erişimi kaldırmak [için, az rol atamasını silerek](/cli/azure/role/assignment#az-role-assignment-delete)bir rol atamasını kaldırırsınız:

```azurecli
az role assignment delete --assignee <assignee> --role <role_name_or_id> --resource-group <resource_group>
```

Aşağıdaki örnek, *Sanal Makine Katılımcısı* rol atamasını, *ilaç satış* kaynak grubundaki *patlong\@contoso.com* kullanıcısından kaldırır:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales
```

Aşağıdaki örnek, bir abonelik kapsamında ID 222222222-2222-2222-22222222222222222222222222222222222222 ile *Ann Mack Takım* grubundan *Reader* rolünü kaldırır. Grubun nesne kimliğini nasıl alacağıhakkında bilgi için [bkz.](#get-object-ids)

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --subscription 00000000-0000-0000-0000-000000000000
```

Aşağıdaki örnek, yönetim grubu kapsamındaki *alain\@example.com* kullanıcısından *Fatura Okuyucu* rolünü kaldırır. Yönetim grubunun kimliğini almak için az [hesap yönetimi grup listesini](/cli/azure/account/management-group#az-account-management-group-list)kullanabilirsiniz.

```azurecli
az role assignment delete --assignee alain@example.com --role "Billing Reader" --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve Azure CLI'yi kullanarak rol atamalarını listele](role-assignments-list-cli.md)
- [Azure kaynaklarını ve kaynak gruplarını yönetmek için Azure CLI'yi kullanın](../azure-resource-manager/cli-azure-resource-manager.md)
