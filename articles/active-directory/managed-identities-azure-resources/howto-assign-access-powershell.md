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
ms.openlocfilehash: 82fa5019e740d16d0b97111fcf8dbc4f6c91d57b
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74183994"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>PowerShell kullanarak bir kaynak için bir yönetilen kimlik erişim atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen bir kimlik ile bir Azure kaynağı yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak yönetilen kimlik erişim izni verebilirsiniz. Bu örnek PowerShell kullanarak Azure depolama hesabınız için bir Azure sanal makinenin yönetilen kimlik erişim vermek nasıl gösterir.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)** .
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Yükleme [Azure PowerShell'in en son sürümünü](/powershell/azure/install-az-ps) henüz yapmadıysanız.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynak için bir yönetilen kimlik erişimi atamak için RBAC kullanma

Yönetilen bir Azure kaynak kimliğini etkinleştirdikten sonra [Azure VM'deki gibi](qs-configure-powershell-windows-vm.md):

1. Oturum açmak için Azure kullanarak `Connect-AzAccount` cmdlet'i. Yönetilen kimlik altında yapılandırdığınız Azure aboneliği ile ilişkili olan bir hesabı kullanın:

   ```powershell
   Connect-AzAccount
   ```
2. Bu örnekte biz bir depolama hesabı için bir Azure VM erişimini vermiş olursunuz. İlk olarak, yönetilen kimliği etkinleştirdiğimiz sırada oluşturulan `myVM`adlı sanal makineye yönelik hizmet sorumlusunu almak için [Get-AzVM](/powershell/module/az.compute/get-azvm) ' i kullanırız. Ardından, `myStorageAcct`adlı bir depolama hesabına VM **okuyucusu** erişimi sağlamak için [New-azroleatama](/powershell/module/Az.Resources/New-AzRoleAssignment) kullanın:

    ```powershell
    $spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen kimlik Azure kaynaklarına genel bakış](overview.md)
- Azure VM'de yönetilen kimlik etkinleştirmek için bkz: [yapılandırma kimliklerini Azure VM'de PowerShell kullanarak Azure kaynakları için yönetilen](qs-configure-powershell-windows-vm.md).
