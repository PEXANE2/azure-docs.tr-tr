---
title: Azure PowerShell-Azure AD kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme & silme
description: Azure PowerShell kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme ve silme hakkında adım adım yönergeler.
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
ms.openlocfilehash: 9acd5140de6d9fe387958f0df02c55cce71bbc33
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84694221"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-azure-powershell"></a>Azure PowerShell kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme veya silme

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory yönetilen bir kimlik ile Azure hizmetleri sağlar. Kodunuzda kimlik bilgileri gerekmeden Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulaması yapmak için bu kimliği kullanabilirsiniz. 

Bu makalede, Azure PowerShell kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturmayı, listeleyeceğinizi ve silmeyi öğreneceksiniz.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. ** [Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#managed-identity-types)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Henüz yapmadıysanız [Azure PowerShell en son sürümünü](/powershell/azure/install-az-ps) yükleyebilirsiniz.
- PowerShell'i yerel ortamda çalıştırıyorsanız şunları da yapmanız gerekir: 
    - Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.
    - [PowerShellGet'in en son sürümünü](/powershell/scripting/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget) yükleyin.
    - `Install-Module -Name PowerShellGet -AllowPrerelease` komutunu çalıştırarak `PowerShellGet` modülünün yayın öncesi sürümünü alın (`Az.ManagedServiceIdentity` modülünü yüklemek için bu komutu çalıştırdıktan sonra geçerli PowerShell oturumundan `Exit` ile çıkmanız gerekebilir).
    - `Install-Module -Name Az.ManagedServiceIdentity -AllowPrerelease` `Az.ManagedServiceIdentity` Bu makalede Kullanıcı tarafından atanan yönetilen kimlik işlemlerini gerçekleştirmek üzere modülün ön sürümü sürümünü yüklemek için ' i çalıştırın.

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için hesabınıza [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması gerekir.

Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için `New-AzUserAssignedIdentity` komutunu kullanın. `ResourceGroupName`Parametresi, Kullanıcı tarafından atanan yönetilen kimliğin oluşturulacağı kaynak grubunu belirtir ve `-Name` parametresi adını belirtir. `<RESOURCE GROUP>`Ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirin:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listeleme

Kullanıcı tarafından atanan yönetilen kimliği listelemek/okumak için hesabınızın [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü ataması gerekir.

Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için, [Get-Azuseratandı] komutunu kullanın.  `-ResourceGroupName`Parametresi, Kullanıcı tarafından atanan yönetilen kimliğin oluşturulduğu kaynak grubunu belirtir. `<RESOURCE GROUP>`Değerini kendi değeri ile değiştirin:

```azurepowershell-interactive
Get-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
Yanıtta, Kullanıcı tarafından atanan yönetilen kimliklerin `"Microsoft.ManagedIdentity/userAssignedIdentities"` anahtar için döndürülen değeri vardır `Type` .

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliği silme

Kullanıcı tarafından atanan bir yönetilen kimliği silmek için hesabınıza [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması gerekir.

Kullanıcı tarafından atanan bir yönetilen kimliği silmek için `Remove-AzUserAssignedIdentity` komutunu kullanın.  `-ResourceGroupName`Parametresi, Kullanıcı tarafından atanan kimliğin oluşturulduğu kaynak grubunu belirtir ve `-Name` parametresi adını belirtir. `<RESOURCE GROUP>`Ve `<USER ASSIGNED IDENTITY NAME>` parametrelerinin değerlerini kendi değerlerinizle değiştirin:

 ```azurepowershell-interactive
Remove-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Kullanıcı tarafından atanan bir yönetilen kimliğin silinmesi başvuruyu, atandığı herhangi bir kaynaktan kaldırmaz. Kimlik atamalarının ayrı ayrı kaldırılması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure kaynakları komutları için Azure PowerShell yönetilen kimliklerin tam listesi ve daha ayrıntılı bilgi için bkz. [az. Managedserviceıdentity](/powershell/module/az.managedserviceidentity#managed_service_identity).
