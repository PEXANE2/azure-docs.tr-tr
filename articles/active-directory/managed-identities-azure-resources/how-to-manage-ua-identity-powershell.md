---
title: Azure PowerShell kullanarak kullanıcı tarafından atanan yönetilen kimliği oluşturma, listele & silmek - Azure AD
description: Azure PowerShell'i kullanarak kullanıcı tarafından atanan yönetilen kimliğin nasıl oluşturulacağına, listelenebildiğini ve silinacağına ilişkin adım adım talimatlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: c512a867685b4480c7b31ac582e2cee069ee2447
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74547412"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-azure-powershell"></a>Azure PowerShell'i kullanarak kullanıcı tarafından atanan yönetilen bir kimlik oluşturma, listele veya silme

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için yönetilen kimlikler Azure hizmetlerine Azure Etkin Dizini'nde yönetilen bir kimlik sağlar. Bu kimliği, kodunuzda kimlik bilgilerine gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure PowerShell kullanarak kullanıcı tarafından atanan yönetilen bir kimliği nasıl oluşturabileceğinizi, listelemeyi ve silmeyi öğrenirsiniz.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Azure [PowerShell'in en son sürümünü](/powershell/azure/install-az-ps) yükleyin.
- PowerShell'i yerel ortamda çalıştırıyorsanız şunları da yapmanız gerekir: 
    - Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.
    - [PowerShellGet'in en son sürümünü](/powershell/scripting/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget) yükleyin.
    - `Install-Module -Name PowerShellGet -AllowPrerelease` komutunu çalıştırarak `PowerShellGet` modülünün yayın öncesi sürümünü alın (`Az.ManagedServiceIdentity` modülünü yüklemek için bu komutu çalıştırdıktan sonra geçerli PowerShell oturumundan `Exit` ile çıkmanız gerekebilir).
    - Bu `Install-Module -Name Az.ManagedServiceIdentity -AllowPrerelease` makalede, kullanıcı tarafından `Az.ManagedServiceIdentity` atanan yönetilen kimlik işlemlerini gerçekleştirmek için modülün ön sürüm sürümünü yüklemek için çalıştırın.

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için, hesabınızın [Yönetilen Kimlik Oluşturici](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

Kullanıcı tarafından atanan yönetilen bir kimlik `New-AzUserAssignedIdentity` oluşturmak için komutu kullanın. Parametre, `ResourceGroupName` kullanıcı tarafından atanan yönetilen kimliğin oluşturulacağı kaynak grubunu `-Name` belirtir ve parametre adını belirtir. `<RESOURCE GROUP>` Ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirin:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listele

Kullanıcı tarafından atanan yönetilen bir kimliği listelemek/okumak için hesabınızın [Yönetilen Kimlik Operatörü'ne](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya Yönetilen [Kimlik Katkıda Bulunan](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için [Get-AzUserAssigned] komutunu kullanın.  Parametre, `-ResourceGroupName` kullanıcı tarafından atanan yönetilen kimliğin oluşturulduğu kaynak grubunu belirtir. Kendi `<RESOURCE GROUP>` değerinizle değiştirin:

```azurepowershell-interactive
Get-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
Yanıtta, kullanıcı tarafından atanan yönetilen `"Microsoft.ManagedIdentity/userAssignedIdentities"` kimliklerin değeri `Type`anahtar için döndürülür, .

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliği silme

Kullanıcı tarafından atanan yönetilen bir kimliği silmek için, hesabınızın [Yönetilen Kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

Kullanıcı tarafından atanan yönetilen kimliği silmek için komutu `Remove-AzUserAssignedIdentity` kullanın.  Parametre, `-ResourceGroupName` kullanıcı tarafından atanan kimliğin oluşturulduğu kaynak grubunu `-Name` ve parametrenin adını belirtir. `<RESOURCE GROUP>` Parametreler ve `<USER ASSIGNED IDENTITY NAME>` parametreler değerlerini kendi değerlerinizle değiştirin:

 ```azurepowershell-interactive
Remove-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Kullanıcı tarafından atanan yönetilen bir kimliğisiletmek, atandığı herhangi bir kaynaktan başvuruyu kaldırmaz. Kimlik atamalarının ayrı olarak kaldırılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure kaynakları komutları için Azure PowerShell yönetilen kimliklerin tam listesi ve daha fazla ayrıntısı için [Az.ManagedServiceIdentity](/powershell/module/az.managedserviceidentity#managed_service_identity)bölümüne bakın.
