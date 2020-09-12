---
title: Azure Key Vault erişim ilkesi atama
description: Hizmet sorumlusuna veya uygulama kimliğine Key Vault erişim ilkesi atamak için Azure portal, Azure CLı veya Azure PowerShell kullanma.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/27/2020
ms.author: mbaldwin
ms.openlocfilehash: 817b9bfc0af054b344ca9f770085ac022a8e6eac
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89381274"
---
# <a name="assign-a-key-vault-access-policy-using-azure-powershell"></a>Azure PowerShell kullanarak Key Vault erişim ilkesi atama

Key Vault erişim ilkesi, belirli bir hizmet sorumlusunun bir uygulama veya Kullanıcı grubu tarafından Key Vault [gizli](../secrets/index.yml)diziler, [anahtarlar](../keys/index.yml)ve [Sertifikalar](../certificates/index.yml)üzerinde farklı işlemler yapıp gerçekleştiremeyeceğini belirler. [Azure Portal](assign-access-policy-portal.md), [Azure CLI](assign-access-policy-cli.md)veya Azure PowerShell (Bu makale) kullanarak erişim ilkeleri atayabilirsiniz.

[!INCLUDE [key-vault-access-policy-limits.md](../../../includes/key-vault-access-policy-limits.md)]

Azure PowerShell kullanarak Azure Active Directory Grup oluşturma hakkında daha fazla bilgi için, bkz. [New-AzureADGroup](/powershell/module/azuread/new-azureadgroup) ve [Add-AzADGroupMember](/powershell/module/az.resources/add-azadgroupmember).

## <a name="configure-powershell-and-sign-in"></a>PowerShell ve oturum açmayı yapılandırma

1. Komutları yerel olarak çalıştırmak için, henüz yapmadıysanız [Azure PowerShell](/powershell/azure/) yükleyebilirsiniz.

    Komutları bulutta doğrudan çalıştırmak için [Azure Cloud Shell](/azure/cloud-shell/overview)kullanın.

1. Yalnızca yerel PowerShell:

    1. [Azure Active Directory PowerShell modülünü](https://www.powershellgallery.com/packages/AzureAD)yükler.

    1. Azure 'da oturum açın:

        ```azurepowershell-interactive
        Login-AzAccount
        ```
    
## <a name="acquire-the-object-id"></a>Nesne KIMLIĞINI edinin

Erişim ilkesini atamak istediğiniz uygulamanın, grubun veya kullanıcının nesne KIMLIĞINI belirleme:

- Uygulamalar ve diğer hizmet sorumluları: sonuçları istenen hizmet sorumlusu adına göre filtrelemek için [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal) cmdlet 'ini parametresiyle birlikte kullanın `-SearchString` :

    ```azurepowershell-interactive
    Get-AzADServicePrincipal -SearchString <search-string>
    ```

- Gruplar: sonuçları istenen grubun adıyla filtrelemek için [Get-AzADGroup](/powershell/module/az.resources/get-azadgroup?view=azps-2.7.0) cmdlet 'ini parametresiyle birlikte kullanın `-SearchString` :

    ```azurepowershell-interactive
    Get-AzADGroup -SearchString <search-string>
    ```
    
    Çıktıda, nesne KIMLIĞI olarak listelenir `Id` .

- Kullanıcılar: Kullanıcı e-posta adresini parametreye geçirerek [Get-AzADUser](/powershell/module/az.resources/get-azaduser) cmdlet 'ini kullanın `-UserPrincipalName` .

    ```azurepowershell-interactive
     Get-AzAdUser -UserPrincipalName <email-address-of-user>
    ```

    Çıktıda, nesne KIMLIĞI olarak listelenir `Id` .

## <a name="assign-the-access-policy"></a>Erişim ilkesini atama

Erişim ilkesini atamak için [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet 'ini kullanın:

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy -VaultName <key-vault-name> -ObjectId <Id> -PermissionsToSecrets <secrets-permissions> -PermissionsToKeys <keys-permissions> -PermissionsToCertificates <certificate-permissions    
```

Yalnızca `-PermissionsToSecrets` `-PermissionsToKeys` `-PermissionsToCertificates` Bu belirli türlere izinler atarken, ve dahil etmeniz gerekir. , Ve için izin verilen değerler `<secret-permissions>` `<key-permissions>` `<certificate-permissions>` [set-AzKeyVaultAccessPolicy-Parameters](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy#parameters) belgelerinde verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault güvenliği: kimlik ve erişim yönetimi](overview-security.md#identity-and-access-management)
- [Anahtar kasanızın güvenliğini sağlayın](secure-your-key-vault.md).
- [Geliştirici Kılavuzu Azure Key Vault](developers-guide.md)
- [En iyi Azure Key Vault uygulamalar](best-practices.md)
