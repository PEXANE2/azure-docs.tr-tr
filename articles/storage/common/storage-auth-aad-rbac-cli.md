---
title: Veri erişimi için bir RBAC rolü atamak için Azure CLI'yi kullanın
titleSuffix: Azure Storage
description: Rol tabanlı erişim denetimine (RBAC) sahip bir Azure Etkin Dizin güvenlik ilkesine izin atamak için Azure CLI'yi nasıl kullanacağınızı öğrenin. Azure Depolama, Azure AD aracılığıyla kimlik doğrulama için yerleşik ve özel RBAC rollerini destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: abe35f3193e2d7ff9a949ca7cd330cb58da2b78c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74891977"
---
# <a name="use-azure-cli-to-assign-an-rbac-role-for-access-to-blob-and-queue-data"></a>Blob ve sıra verilerine erişmek için bir RBAC rolü atamak için Azure CLI'yi kullanın

Azure Etkin Dizin (Azure AD), rol tabanlı [erişim denetimi (RBAC)](../../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarına izin vermektedir. Azure Depolama, blob veya sıra verilerine erişmek için kullanılan ortak izin kümelerini kapsayan yerleşik RBAC rolleri kümesini tanımlar.

Bir Azure REKLAM güvenlik ilkesine bir RBAC rolü atandığında, Azure bu güvenlik ilkesi için bu kaynaklara erişim sağlar. Erişim, abonelik düzeyine, kaynak grubuna, depolama hesabına veya tek bir kapsayıcı veya sıraya kadar kapsama edilebilir. Azure AD güvenlik ilkesi, [Azure kaynakları için](../../active-directory/managed-identities-azure-resources/overview.md)bir kullanıcı, bir grup, bir uygulama hizmeti ilkesi veya yönetilen bir kimlik olabilir.

Bu makalede, yerleşik RBAC rollerini listelemek ve kullanıcılara atamak için Azure CLI'nin nasıl kullanılacağı açıklanmaktadır. Azure CLI kullanma hakkında daha fazla bilgi için Azure [Komut Satırı Arabirimi (CLI)](https://docs.microsoft.com/cli/azure)bakın.

## <a name="rbac-roles-for-blobs-and-queues"></a>Lekeler ve kuyruklar için RBAC rolleri

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Kaynak kapsamını belirleme

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-rbac-roles"></a>Kullanılabilir RBAC rollerini listele

Kullanılabilir yerleşik RBAC rollerini Azure CLI ile listelemek için [az rol tanımı listesi](/cli/azure/role/definition#az-role-definition-list) komutunu kullanın:

```azurecli-interactive
az role definition list --out table
```

Azure'un diğer yerleşik rolleri ile birlikte listelenen yerleşik Azure Depolama veri rollerini görürsünüz:

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-rbac-role-to-a-security-principal"></a>Bir güvenlik ilkesine RBAC rolü atama

Bir güvenlik ilkesine RBAC rolü atamak için [az rol atama komutunu](/cli/azure/role/assignment#az-role-assignment-create) kullanın. Komutun biçimi, atamanın kapsamına bağlı olarak farklı olabilir. Aşağıdaki örnekler, çeşitli kapsamlarda bir kullanıcıya nasıl bir rol atadığınızı gösterir, ancak herhangi bir güvenlik ilkesine bir rol atamak için aynı komutu kullanabilirsiniz.

### <a name="container-scope"></a>Konteyner kapsamı

Kapsayıcıya kapsamlı bir rol atamak için, parametre için kapsayıcının `--scope` kapsamını içeren bir dize belirtin. Bir kapsayıcının kapsamı şu şekildedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container>
```

Aşağıdaki örnek, kapsayıcının düzeyine göre kapsamlı bir kullanıcıya **Depolama Blob Veri Katılımcısı** rolünü atar. Ayraçtaki örnek değerleri ve yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun:

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container>"
```

### <a name="queue-scope"></a>Sıra kapsamı

Kuyruğa kapsamlı bir rol atamak için, parametre için `--scope` sıranın kapsamını içeren bir dize belirtin. Sıranın kapsamı şu şekildedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue>
```

Aşağıdaki örnek, sıra düzeyine kadar kapsamlı bir kullanıcıya **Depolama Sırası Veri Katılımcısı** rolünü atar. Ayraçtaki örnek değerleri ve yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun:

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue>"
```

### <a name="storage-account-scope"></a>Depolama hesabı kapsamı

Depolama hesabına kapsamlı bir rol atamak için, parametre için `--scope` depolama hesabı kaynağının kapsamını belirtin. Depolama hesabının kapsamı şu şekildedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

Aşağıdaki örnek, **Depolama Blob Veri Okuyucu** rolünün depolama hesabı düzeyindeki bir kullanıcıya nasıl atandığını gösterir. Örnek değerleri kendi değerlerinizle değiştirdiğinizden emin olun: \

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Reader" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>Kaynak grubu kapsamı

Kaynak grubuna kapsamlı bir rol atamak için, parametre için `--resource-group` kaynak grubu adını veya kimliğini belirtin. Aşağıdaki örnek, **Depolama Sırası Veri Okuyucu** rolünü kaynak grubu düzeyindeki bir kullanıcıya atar. Parantez içinde örnek değerleri ve yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun:

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Reader" \
    --assignee <email> \
    --resource-group <resource-group>
```

### <a name="subscription-scope"></a>Abonelik kapsamı

Abonelmeye yönelik bir rol atamak için, parametreiçin `--scope` aboneliğin kapsamını belirtin. Aboneliğin kapsamı şu şekildedir:

```
/subscriptions/<subscription>
```

Aşağıdaki örnek, **Depolama Blob Veri Okuyucu** rolünün depolama hesabı düzeyindeki bir kullanıcıya nasıl atandığını gösterir. Örnek değerleri kendi değerlerinizle değiştirdiğinizden emin olun: 

```azurecli-interactive
az role assignment create \
    --role "Storage Blob Data Reader" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>Sonraki adımlar

- [RBAC ve Azure PowerShell'i kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-powershell.md)
- [Azure PowerShell'i kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-powershell.md)
- [Azure portalında RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-portal.md)
