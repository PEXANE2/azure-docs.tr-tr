---
title: Azure RBAC ve Azure CLı kullanarak rol atamaları ekleme veya kaldırma
description: Azure rol tabanlı erişim denetimi (RBAC) ve Azure CLı kullanarak kullanıcılar, gruplar, hizmet sorumluları veya yönetilen kimlikler için Azure kaynaklarına erişim izni verme hakkında bilgi edinin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79245674"
---
# <a name="add-or-remove-role-assignments-using-azure-rbac-and-azure-cli"></a>Azure RBAC ve Azure CLı kullanarak rol atamaları ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Bu makalede, Azure CLı kullanarak rollerin nasıl atanacağı açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmanız gerekir:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi izinler
- Azure Cloud Shell veya [Azure CLI](/cli/azure) ['da Bash](/azure/cloud-shell/overview)

## <a name="get-object-ids"></a>Nesne kimliklerini al

Rol atamaları eklemek veya kaldırmak için, bir nesnenin benzersiz KIMLIĞINI belirtmeniz gerekebilir. KIMLIK şu biçimdedir: `11111111-1111-1111-1111-111111111111`. KIMLIĞI Azure portal veya Azure CLı 'yi kullanarak edinebilirsiniz.

### <a name="user"></a>Kullanıcı

Bir Azure AD kullanıcısının nesne KIMLIĞINI almak için [az ad User Show](/cli/azure/ad/user#az-ad-user-show)seçeneğini kullanabilirsiniz.

```azurecli
az ad user show --id "{email}" --query objectId --output tsv
```

### <a name="group"></a>Grup

Bir Azure AD grubunun nesne KIMLIĞINI almak için [az Ad Group Show](/cli/azure/ad/group#az-ad-group-show) veya [az Ad Group List](/cli/azure/ad/group#az-ad-group-list)kullanabilirsiniz.

```azurecli
az ad group show --group "{name}" --query objectId --output tsv
```

### <a name="application"></a>Uygulama

Bir Azure AD hizmet sorumlusunun (bir uygulama tarafından kullanılan kimlik) nesne KIMLIĞINI almak için [az ad SP listesini](/cli/azure/ad/sp#az-ad-sp-list)kullanabilirsiniz. Hizmet sorumlusu için uygulama KIMLIĞINI **değil** , nesne kimliğini kullanın.

```azurecli
az ad sp list --display-name "{name}" --query [].objectId --output tsv
```

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

RBAC 'de, erişim izni vermek için bir rol ataması eklersiniz.

### <a name="user-at-a-resource-group-scope"></a>Kaynak grubu kapsamındaki Kullanıcı

Bir kaynak grubu kapsamındaki bir kullanıcı için rol ataması eklemek için [az role atama Create](/cli/azure/role/assignment#az-role-assignment-create)' i kullanın.

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
```

Aşağıdaki örnek, *sanal makine katılımcısı* rolünü *ilaç-Sales* kaynak grubu kapsamındaki *patlong\@contoso.com* kullanıcısına atar:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="using-the-unique-role-id"></a>Benzersiz rol KIMLIĞINI kullanma

Rol adının değişebilir birkaç zaman vardır, örneğin:

- Kendi özel rolünüzü kullanıyorsunuz ve adı değiştirmeye karar verdiniz.
- Adında **(Önizleme)** olan bir önizleme rolü kullanıyorsunuz. Rol serbest bırakıldığında, rol yeniden adlandırılır.

> [!IMPORTANT]
> Bir önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Rol yeniden adlandırılsa bile, rol KIMLIĞI değişmez. Rol atamalarınızı oluşturmak için betikler veya Otomasyon kullanıyorsanız, rol adı yerine benzersiz rol KIMLIĞINI kullanmak en iyi uygulamadır. Bu nedenle, bir rol yeniden adlandırılırsa, betiklerinizin çalışması daha olasıdır.

Rol adı yerine benzersiz rol KIMLIĞINI kullanarak rol ataması eklemek için [az role atama Create](/cli/azure/role/assignment#az-role-assignment-create)' i kullanın.

```azurecli
az role assignment create --role <role_id> --assignee <assignee> --resource-group <resource_group>
```

Aşağıdaki örnek, [sanal makine katılımcısı](built-in-roles.md#virtual-machine-contributor) rolünü *ilaç-Sales* kaynak grubu kapsamındaki *patlong\@contoso.com* kullanıcısına atar. Benzersiz rol KIMLIĞINI almak için [az role Definition List](/cli/azure/role/definition#az-role-definition-list) ' i kullanabilir veya [Azure kaynakları için yerleşik roller](built-in-roles.md)' e bakabilirsiniz.

```azurecli
az role assignment create --role 9980e02c-c2be-4d73-94e8-173b1dc7cf3c --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="group-at-a-subscription-scope"></a>Bir abonelik kapsamındaki Grup

Bir gruba rol ataması eklemek için [az role atama Create](/cli/azure/role/assignment#az-role-assignment-create)' i kullanın. Grubun nesne KIMLIĞINI alma hakkında daha fazla bilgi için bkz. [nesne kimliklerini alma](#get-object-ids).

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek, *okuyucu* rolünü bir ABONELIK kapsamındaki kimliği 22222222-2222-2222-2222-222222222222 olan *Ann Mack ekip* grubuna atar.

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000
```

### <a name="group-at-a-resource-scope"></a>Kaynak kapsamındaki Grup

Bir gruba rol ataması eklemek için [az role atama Create](/cli/azure/role/assignment#az-role-assignment-create)' i kullanın. Grubun nesne KIMLIĞINI alma hakkında daha fazla bilgi için bkz. [nesne kimliklerini alma](#get-object-ids).

Aşağıdaki örnek, *sanal makine katılımcısı* rolünü, kimlik 22222222-2222-2222-2222-222222222222 olan *Ann Mack ekip* grubuna, *ilaç-Sales-Project-Network*adlı bir sanal ağ için kaynak kapsamında atar.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/pharma-sales/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="application-at-a-resource-group-scope"></a>Kaynak grubu kapsamındaki uygulama

Bir uygulamaya rol ataması eklemek için [az role atama Create](/cli/azure/role/assignment#az-role-assignment-create)' i kullanın. Uygulamanın nesne KIMLIĞINI alma hakkında daha fazla bilgi için bkz. [nesne kimliklerini alma](#get-object-ids).

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group>
```

Aşağıdaki örnek, *sanal makine katılımcısı* rolünü, *ilaç-Sales* kaynak grubu kapsamındaki 44444444-4444-4444-4444-444444444444 nesne kimliği ile bir uygulamaya atar.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales
```

### <a name="user-at-a-subscription-scope"></a>Abonelik kapsamındaki Kullanıcı

Bir kullanıcı için abonelik kapsamındaki bir rol ataması eklemek için [az role atama oluştur](/cli/azure/role/assignment#az-role-assignment-create)' u kullanın. Abonelik KIMLIĞINI almak için Azure portal **abonelikler** dikey penceresinde bulabilir veya [az Account List](/cli/azure/account#az-account-list)' i kullanabilirsiniz.

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --subscription <subscription_name_or_id>
```

Aşağıdaki örnek, *okuyucu* rolünü bir abonelik kapsamındaki *annm\@example.com* kullanıcısına atar.

```azurecli
az role assignment create --role "Reader" --assignee annm@example.com --subscription 00000000-0000-0000-0000-000000000000
```

### <a name="user-at-a-management-group-scope"></a>Bir yönetim grubu kapsamındaki Kullanıcı

Bir kullanıcı için yönetim grubu kapsamındaki bir rol ataması eklemek için [az role atama oluştur](/cli/azure/role/assignment#az-role-assignment-create)' u kullanın. Yönetim grubu KIMLIĞINI almak için Azure portal **Yönetim grupları** dikey penceresinde bulabilir veya [az Account Management-Group List](/cli/azure/account/management-group#az-account-management-group-list)kullanabilirsiniz.

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

Aşağıdaki örnek, bir yönetim grubu kapsamındaki *Alain\@example.com* kullanıcısına *faturalandırma okuyucusu* rolünü atar.

```azurecli
az role assignment create --role "Billing Reader" --assignee alain@example.com --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

### <a name="new-service-principal"></a>Yeni hizmet sorumlusu

Yeni bir hizmet sorumlusu oluşturur ve bu hizmet sorumlusuna hemen bir rol atamayı denerseniz, bu rol ataması bazı durumlarda başarısız olabilir. Örneğin, yeni bir yönetilen kimlik oluşturmak ve ardından bu hizmet sorumlusuna bir rol atamayı denemek için bir komut dosyası kullanırsanız, rol ataması başarısız olabilir. Bu hatanın nedeni büyük olasılıkla çoğaltma gecikmesi. Hizmet sorumlusu tek bir bölgede oluşturulur; Ancak, rol ataması henüz hizmet sorumlusunu çoğaltılmamış farklı bir bölgede gerçekleşebilir. Bu senaryoya yönelik olarak, rol atamasını oluştururken asıl türü belirtmeniz gerekir.

Rol ataması eklemek için [az role atama oluştur](/cli/azure/role/assignment#az-role-assignment-create)kullanın, için `--assignee-object-id`bir değer belirtin ve sonra olarak `--assignee-principal-type` `ServicePrincipal`ayarlayın.

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --assignee-principal-type <assignee_principal_type> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek, *sanal makine katılımcısı* rolünü, *sahte ma-Sales* kaynak grubu kapsamındaki *MSI-test* tarafından yönetilen kimliğe atar:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 33333333-3333-3333-3333-333333333333 --assignee-principal-type ServicePrincipal --resource-group pharma-sales
```

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

RBAC 'de, erişimi kaldırmak için [az role atama Sil](/cli/azure/role/assignment#az-role-assignment-delete)' i kullanarak bir rol atamasını kaldırırsınız:

```azurecli
az role assignment delete --assignee <assignee> --role <role_name_or_id> --resource-group <resource_group>
```

Aşağıdaki örnek, *ilaç-Sales* kaynak grubundaki *\@Patlong contoso.com* kullanıcısının *sanal makine katılımcısı* rolü atamasını kaldırır:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales
```

Aşağıdaki örnek, bir abonelik kapsamındaki KIMLIĞI 22222222-2222-2222-2222-222222222222 olan *Ann Mack ekip* grubundan *okuyucu* rolünü kaldırır. Grubun nesne KIMLIĞINI alma hakkında daha fazla bilgi için bkz. [nesne kimliklerini alma](#get-object-ids).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --subscription 00000000-0000-0000-0000-000000000000
```

Aşağıdaki örnek, yönetim grubu kapsamındaki *Alain\@example.com* kullanıcısının *faturalandırma okuyucusu* rolünü kaldırır. Yönetim grubunun KIMLIĞINI almak için [az Account Management-Group List](/cli/azure/account/management-group#az-account-management-group-list)kullanabilirsiniz.

```azurecli
az role assignment delete --assignee alain@example.com --role "Billing Reader" --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve Azure CLı kullanarak rol atamalarını listeleme](role-assignments-list-cli.md)
- [Azure CLı 'yı kullanarak Azure kaynaklarını ve kaynak gruplarını yönetme](../azure-resource-manager/cli-azure-resource-manager.md)
