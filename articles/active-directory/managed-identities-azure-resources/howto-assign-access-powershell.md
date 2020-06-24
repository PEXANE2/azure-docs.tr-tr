---
title: PowerShell kullanarak bir kaynağa yönetilen kimlik erişimi atama-Azure AD
description: Bir kaynakta yönetilen kimlik atamaya, başka bir kaynağa erişim için PowerShell kullanarak adım adım yönergeler.
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
ms.openlocfilehash: cba16f59944f94d505dd3da92e00830885ce86cf
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84693932"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>PowerShell kullanarak bir kaynağa yönetilen kimlik erişimi atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen kimliğe sahip bir Azure kaynağı yapılandırdıktan sonra, herhangi bir güvenlik sorumlusu gibi, yönetilen kimliğe başka bir kaynağa erişim izni verebilirsiniz. Bu örnekte, PowerShell kullanarak bir Azure depolama hesabına Azure sanal makinesinin yönetilen kimlik erişimi verme yöntemi gösterilmektedir.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. ** [Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#managed-identity-types)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Henüz yapmadıysanız [Azure PowerShell en son sürümünü](/powershell/azure/install-az-ps) yükleyebilirsiniz.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynağa yönetilen kimlik erişimi atamak için RBAC kullanma

Azure [VM](qs-configure-powershell-windows-vm.md)gibi bir Azure kaynağında yönetilen kimlik etkinleştirildikten sonra:

1. Cmdlet 'ini kullanarak Azure 'da oturum açın `Connect-AzAccount` . Yönetilen kimliği yapılandırdığınız Azure aboneliğiyle ilişkili bir hesabı kullanın:

   ```powershell
   Connect-AzAccount
   ```
2. Bu örnekte, bir depolama hesabına Azure VM erişimi veriyoruz. İlk olarak, yönetilen kimliği etkinleştirdiğimiz sırada oluşturulan adlı VM 'ye ait hizmet sorumlusunu almak için [Get-AzVM](/powershell/module/az.compute/get-azvm) `myVM` ' i kullanıyoruz. Ardından, [Yeni-Azroleatama](/powershell/module/Az.Resources/New-AzRoleAssignment) kullanarak VM **okuyucusu** için adlı bir depolama hesabına erişim sağlayın `myStorageAcct` :

    ```powershell
    $spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Azure VM 'de yönetilen kimliği etkinleştirmek için bkz. [PowerShell kullanarak Azure VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-powershell-windows-vm.md).
