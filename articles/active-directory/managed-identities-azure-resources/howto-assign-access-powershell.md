---
title: PowerShell kullanarak bir kaynağa yönetilen kimlik erişimi atama-Azure AD
description: Adım adım yönergeler bir kaynakta bir yönetilen kimlik atamak için PowerShell kullanarak başka bir kaynağa erişin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: a2283ac076ef761fd098d75e7120e6557a959574
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74547258"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>PowerShell kullanarak bir kaynak için bir yönetilen kimlik erişim atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen bir kimlik ile bir Azure kaynağı yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak yönetilen kimlik erişim izni verebilirsiniz. Bu örnek PowerShell kullanarak Azure depolama hesabınız için bir Azure sanal makinenin yönetilen kimlik erişim vermek nasıl gösterir.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. **[Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Henüz yapmadıysanız [Azure PowerShell en son sürümünü](/powershell/azure/install-az-ps) yükleyebilirsiniz.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynak için bir yönetilen kimlik erişimi atamak için RBAC kullanma

Azure [VM](qs-configure-powershell-windows-vm.md)gibi bir Azure kaynağında yönetilen kimlik etkinleştirildikten sonra:

1. `Connect-AzAccount` cmdlet 'ini kullanarak Azure 'da oturum açın. Yönetilen kimlik altında yapılandırdığınız Azure aboneliği ile ilişkili olan bir hesabı kullanın:

   ```powershell
   Connect-AzAccount
   ```
2. Bu örnekte biz bir depolama hesabı için bir Azure VM erişimini vermiş olursunuz. İlk olarak, yönetilen kimliği etkinleştirdiğimiz sırada oluşturulan `myVM`adlı sanal makineye yönelik hizmet sorumlusunu almak için [Get-AzVM](/powershell/module/az.compute/get-azvm) ' i kullanırız. Ardından, `myStorageAcct`adlı bir depolama hesabına VM **okuyucusu** erişimi sağlamak için [New-azroleatama](/powershell/module/Az.Resources/New-AzRoleAssignment) kullanın:

    ```powershell
    $spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Azure VM 'de yönetilen kimliği etkinleştirmek için bkz. [PowerShell kullanarak Azure VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-powershell-windows-vm.md).
