---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 03/25/2020
ms.author: orspodek
ms.openlocfilehash: 081ba777f6ab19be774f127383e359ff761e7f0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297917"
---
## <a name="create-a-new-key-vault"></a>Yeni bir anahtar kasası oluşturma

PowerShell kullanarak yeni bir anahtar kasası oluşturmak için [New-AzKeyVault'u](/powershell/module/az.keyvault/new-azkeyvault)arayın. Azure Veri Gezgini şifrelemesi için müşteri tarafından yönetilen anahtarları depolamak için kullandığınız anahtar kasasının iki anahtar koruma ayarı etkin leştirilmiş olmalıdır: **Yumuşak Silme** ve **Temizleme).** Parantez içinde yer tutucu değerlerini aşağıdaki örnekte kendi değerlerinizle değiştirin.

```azurepowershell-interactive
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

## <a name="configure-the-key-vault-access-policy"></a>Anahtar kasa erişim ilkesini yapılandırma

Ardından, kümenin erişmek için izinleri olacak şekilde anahtar kasasının erişim ilkesini yapılandırın. Bu adımda, kümeye daha önce atadığınız sistem atanmış yönetilen kimliği kullanırsınız. Anahtar kasasının erişim ilkesini ayarlamak için [Set-AzKeyVaultAccessPolicy'yi](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy)arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirin ve önceki örneklerde tanımlanan değişkenleri kullanın.

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $cluster.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Yeni bir anahtar oluşturma

Ardından, anahtar kasasında yeni bir anahtar oluşturun. Yeni bir anahtar oluşturmak için [Add-AzKeyVaultKey'i](/powershell/module/az.keyvault/add-azkeyvaultkey)arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirin ve önceki örneklerde tanımlanan değişkenleri kullanın.

```azurepowershell-interactive
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```
