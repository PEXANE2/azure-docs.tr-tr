---
title: PowerShell - Azure AD'yi kullanarak kaynağa yönetilen bir kimlik erişimi atama
description: PowerShell'i kullanarak bir kaynağa yönetilen bir kimlik atamak, başka bir kaynağa erişim için adım adım yönergeler.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74547258"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>PowerShell'i kullanarak kaynağa yönetilen kimlik erişimi atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bir Azure kaynağını yönetilen bir kimliğe göre yapılandırdıktan sonra, yönetilen kimlik erişimini herhangi bir güvenlik ilkesi gibi başka bir kaynağa verebilirsiniz. Bu örnek, PowerShell kullanarak bir Azure sanal makinesinin yönetilen kimlik erişimini bir Azure depolama hesabına nasıl vereceğinizi gösterir.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Azure [PowerShell'in en son sürümünü](/powershell/azure/install-az-ps) yükleyin.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynağa yönetilen kimlik erişimi atamak için RBAC'ı kullanma

[Azure VM gibi](qs-configure-powershell-windows-vm.md)bir Azure kaynağında yönetilen kimliği etkinleştirdikten sonra:

1. Cmdlet'i `Connect-AzAccount` kullanarak Azure'da oturum açın. Yönetilen kimliği yapılandırdığınız Azure aboneliğiyle ilişkili bir hesap kullanın:

   ```powershell
   Connect-AzAccount
   ```
2. Bu örnekte, bir depolama hesabına Azure VM erişimi veriyoruz. Önce, yönetilen kimliği etkinleştirdiğimizde oluşturulan VM adlı `myVM`VM'nin servis ilkesini almak için [Get-AzVM'yi](/powershell/module/az.compute/get-azvm) kullanıyoruz. Daha sonra, VM **Reader'a** aşağıdaki adlı depolama hesabına `myStorageAcct`erişim sağlamak için [Yeni-AzRoleAssignment'ı](/powershell/module/Az.Resources/New-AzRoleAssignment) kullanın:

    ```powershell
    $spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Azure VM'de yönetilen kimliği etkinleştirmek [için PowerShell'i kullanarak Azure kaynakları için yönetilen kimlikleri Yapılandırma'ya](qs-configure-powershell-windows-vm.md)bakın.
