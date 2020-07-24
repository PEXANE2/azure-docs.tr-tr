---
title: Veri erişimi için RBAC rolü atamak üzere PowerShell 'i kullanma
titleSuffix: Azure Storage
description: PowerShell kullanarak rol tabanlı erişim denetimi (RBAC) ile Azure Active Directory güvenlik sorumlusuna izin atama hakkında bilgi edinin. Azure depolama, Azure AD aracılığıyla kimlik doğrulaması için yerleşik ve Azure özel rollerini destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/16/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: dab10c8d7c93e130c707f6020309e5dd2440a028
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87124944"
---
# <a name="use-powershell-to-assign-an-rbac-role-for-access-to-blob-and-queue-data"></a>Blob ve kuyruk verilerine erişim için bir RBAC rolü atamak üzere PowerShell 'i kullanma

Azure Active Directory (Azure AD), [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarını yetkilendirir. Azure depolama, kapsayıcılara veya kuyruklara erişmek için kullanılan ortak izin kümelerini çevreleyen yerleşik RBAC rollerinin bir kümesini tanımlar.

Azure AD güvenlik sorumlusuna bir RBAC rolü atandığında Azure, bu güvenlik sorumlusu için bu kaynaklara erişim izni verir. Erişim, aboneliğin düzeyi, kaynak grubu, depolama hesabı veya tek bir kapsayıcı veya kuyruk kapsamına eklenebilir. Azure AD güvenlik sorumlusu, bir Kullanıcı, Grup, uygulama hizmeti sorumlusu veya [Azure kaynakları için yönetilen bir kimlik](../../active-directory/managed-identities-azure-resources/overview.md)olabilir.

Bu makalede, yerleşik RBAC rollerini listelemek ve kullanıcılara atamak için Azure PowerShell nasıl kullanılacağı açıklanır. Azure PowerShell kullanma hakkında daha fazla bilgi için bkz. [Azure PowerShell genel bakış](https://docs.microsoft.com/powershell/azure/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>Blob 'lar ve kuyruklar için RBAC rolleri

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Kaynak kapsamını belirleme

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-rbac-roles"></a>Kullanılabilir RBAC rollerini listeleme

Mevcut yerleşik RBAC rollerini Azure PowerShell listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın:

```powershell
Get-AzRoleDefinition | FT Name, Description
```

Azure için diğer yerleşik rollerle birlikte listelenen yerleşik Azure depolama veri rollerini görürsünüz:

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-rbac-role-to-a-security-principal"></a>Güvenlik sorumlusuna RBAC rolü atama

Bir güvenlik sorumlusuna RBAC rolü atamak için, [New-Azroleatama](/powershell/module/az.resources/new-azroleassignment) komutunu kullanın. Komutun biçimi atamanın kapsamına göre farklılık gösterebilir. Komutu çalıştırmak için, ilgili kapsamda sahip veya katkıda bulunan rolü atanmış olması gerekir. Aşağıdaki örneklerde, bir kullanıcıya çeşitli kapsamlardaki bir rol atama gösterilmektedir, ancak herhangi bir güvenlik sorumlusuna rol atamak için aynı komutu kullanabilirsiniz.

### <a name="container-scope"></a>Kapsayıcı kapsamı

Bir kapsayıcıya kapsamlı bir rol atamak için, parametre için kapsayıcının kapsamını içeren bir dize belirtin `--scope` . Kapsayıcının kapsamı şu biçimdedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

Aşağıdaki örnek, **Depolama Blobu veri katılımcısı** rolünü, *örnek kapsayıcı*adlı bir kapsayıcıya kapsamındaki bir kullanıcıya atar. Parantez içinde örnek değerleri ve yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### <a name="queue-scope"></a>Sıra kapsamı

Bir sıraya kapsamlı bir rol atamak için, parametre için kuyruğun kapsamını içeren bir dize belirtin `--scope` . Bir kuyruğun kapsamı şu biçimdedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

Aşağıdaki örnek, **depolama kuyruğu veri katılımcısı** rolünü, *örnek kuyruğu*adlı bir kuyruğa kapsamındaki bir kullanıcıya atar. Parantez içinde örnek değerleri ve yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### <a name="storage-account-scope"></a>Depolama hesabı kapsamı

Depolama hesabına kapsamlı bir rol atamak için, parametresi için depolama hesabı kaynağının kapsamını belirtin `--scope` . Depolama hesabının kapsamı şu biçimdedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

Aşağıdaki örnek, depolama hesabı düzeyindeki bir kullanıcıya **Depolama Blobu veri okuyucusu** rolünün nasıl kapsam oluşturulacağını gösterir. Örnek değerleri kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>Kaynak grubu kapsamı

Kaynak grubuna kapsamlı bir rol atamak için, parametresinin kaynak grubu adını veya KIMLIĞINI belirtin `--resource-group` . Aşağıdaki örnek, **depolama kuyruğu veri okuyucusu** rolünü, kaynak grubunun düzeyindeki bir kullanıcıya atar. Parantez içinde örnek değerleri ve yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Reader" `
    -ResourceGroupName "sample-resource-group"
```

### <a name="subscription-scope"></a>Abonelik kapsamı

Aboneliğe kapsamlı bir rol atamak için, parametresi için abonelik kapsamını belirtin `--scope` . Bir aboneliğin kapsamı şu biçimdedir:

```
/subscriptions/<subscription>
```

Aşağıdaki örnek, depolama hesabı düzeyinde bir kullanıcıya **Depolama Blobu veri okuyucusu** rolünün nasıl atanacağını gösterir. Örnek değerleri kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>Sonraki adımlar

- [RBAC ve Azure PowerShell kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-powershell.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-cli.md)
- [Azure portalında RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-portal.md)
