---
title: Veri erişimi için bir RBAC rolü atamak için PowerShell'i kullanma
titleSuffix: Azure Storage
description: Rol tabanlı erişim denetimine (RBAC) sahip bir Azure Active Directory güvenlik ilkesine izin atamak için PowerShell'i nasıl kullanacağınızı öğrenin. Azure Depolama, Azure AD aracılığıyla kimlik doğrulama için yerleşik ve özel RBAC rollerini destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 1413035c879198cf333aeeb5d8fe993162939172
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75460575"
---
# <a name="use-powershell-to-assign-an-rbac-role-for-access-to-blob-and-queue-data"></a>Blob ve sıra verilerine erişmek için bir RBAC rolü atamak için PowerShell'i kullanma

Azure Etkin Dizin (Azure AD), rol tabanlı [erişim denetimi (RBAC)](../../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarına izin vermektedir. Azure Depolama, kapsayıcılara veya kuyruklara erişmek için kullanılan ortak izin kümelerini kapsayan yerleşik RBAC rolleri kümesini tanımlar.

Bir Azure REKLAM güvenlik ilkesine bir RBAC rolü atandığında, Azure bu güvenlik ilkesi için bu kaynaklara erişim sağlar. Erişim, abonelik düzeyine, kaynak grubuna, depolama hesabına veya tek bir kapsayıcı veya sıraya kadar kapsama edilebilir. Azure AD güvenlik ilkesi, [Azure kaynakları için](../../active-directory/managed-identities-azure-resources/overview.md)bir kullanıcı, bir grup, bir uygulama hizmeti ilkesi veya yönetilen bir kimlik olabilir.

Bu makalede, yerleşik RBAC rollerini listelemek ve kullanıcılara atamak için Azure PowerShell'in nasıl kullanılacağı açıklanmaktadır. Azure PowerShell'i kullanma hakkında daha fazla bilgi için Azure [PowerShell'e Genel Bakış](https://docs.microsoft.com/powershell/azure/overview)bölümüne bakın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>Lekeler ve kuyruklar için RBAC rolleri

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Kaynak kapsamını belirleme

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-rbac-roles"></a>Kullanılabilir RBAC rollerini listele

Azure PowerShell ile kullanılabilir yerleşik RBAC rollerini listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) komutunu kullanın:

```powershell
Get-AzRoleDefinition | FT Name, Description
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

Bir güvenlik ilkesine RBAC rolü atamak için [Yeni Rol Atama](/powershell/module/az.resources/new-azroleassignment) komutunu kullanın. Komutun biçimi, atamanın kapsamına bağlı olarak farklı olabilir. Komutu çalıştırmak için, ilgili kapsamda Sahip veya Katılımcı rolünün atanması gerekir. Aşağıdaki örnekler, çeşitli kapsamlarda bir kullanıcıya nasıl bir rol atadığınızı gösterir, ancak herhangi bir güvenlik ilkesine bir rol atamak için aynı komutu kullanabilirsiniz.

### <a name="container-scope"></a>Konteyner kapsamı

Kapsayıcıya kapsamlı bir rol atamak için, parametre için kapsayıcının `--scope` kapsamını içeren bir dize belirtin. Bir kapsayıcının kapsamı şu şekildedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

Aşağıdaki örnek, *örnek kapsayıcı*adlı bir kapsayıcıya kapsamlı bir kullanıcıya **Depolama Blob Veri Katılımcısı** rolünü atar. Ayraçtaki örnek değerleri ve yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### <a name="queue-scope"></a>Sıra kapsamı

Kuyruğa kapsamlı bir rol atamak için, parametre için `--scope` sıranın kapsamını içeren bir dize belirtin. Sıranın kapsamı şu şekildedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

Aşağıdaki örnek, *örnek sıra*adlı bir kuyruğa kapsamına giren depolama sırası **Veri Katılımcısı** rolünü kullanıcıya atar. Ayraçtaki örnek değerleri ve yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### <a name="storage-account-scope"></a>Depolama hesabı kapsamı

Depolama hesabına kapsamlı bir rol atamak için, parametre için `--scope` depolama hesabı kaynağının kapsamını belirtin. Depolama hesabının kapsamı şu şekildedir:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

Aşağıdaki örnek, **Depolama Blob Veri Okuyucu** rolünün depolama hesabı düzeyindeki bir kullanıcıya nasıl kapsam kazandırıldığını gösterir. Örnek değerleri kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>Kaynak grubu kapsamı

Kaynak grubuna kapsamlı bir rol atamak için, parametre için `--resource-group` kaynak grubu adını veya kimliğini belirtin. Aşağıdaki örnek, **Depolama Sırası Veri Okuyucu** rolünü kaynak grubu düzeyindeki bir kullanıcıya atar. Parantez içinde örnek değerleri ve yer tutucu değerlerini kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Reader" `
    -ResourceGroupName "sample-resource-group"
```

### <a name="subscription-scope"></a>Abonelik kapsamı

Abonelmeye yönelik bir rol atamak için, parametreiçin `--scope` aboneliğin kapsamını belirtin. Aboneliğin kapsamı şu şekildedir:

```
/subscriptions/<subscription>
```

Aşağıdaki örnek, **Depolama Blob Veri Okuyucu** rolünün depolama hesabı düzeyindeki bir kullanıcıya nasıl atandığını gösterir. Örnek değerleri kendi değerlerinizle değiştirdiğinizden emin olun: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>Sonraki adımlar

- [RBAC ve Azure PowerShell'i kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-powershell.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-cli.md)
- [Azure portalında RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-portal.md)
