---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 01/07/2020
ms.author: orspodek
ms.openlocfilehash: 5443ee6912c30b89cee6fdb43f84f3bc1fbcfe68
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76021164"
---
Azure Veri Gezgini, bekleyen bir depolama hesabındaki tüm verileri şifreler. Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, veri şifrelemesi için kullanmak üzere müşteri tarafından yönetilen anahtarlar sağlayabilirsiniz. Müşteri tarafından yönetilen anahtarların bir [Azure Key Vault](/azure/key-vault/key-vault-overview)depolanması gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtarlar oluşturmak için bir Azure Key Vault API kullanabilirsiniz. Azure Veri Gezgini kümesi ve Anahtar Kasası aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir. Müşteri tarafından yönetilen anahtarlar hakkında ayrıntılı bir açıklama için bkz. [Azure Key Vault ile müşteri tarafından yönetilen anahtarlar](/azure/storage/common/storage-service-encryption). Bu makalede, müşteri tarafından yönetilen anahtarların nasıl yapılandırılacağı gösterilir.

> [!Note]
> Müşteri tarafından yönetilen anahtarları Azure Veri Gezgini ile yapılandırmak için [anahtar kasasında iki özellik ayarlamanız](/azure/key-vault/key-vault-ovw-soft-delete)gerekir: **geçici silme** ve **Temizleme**. Bu özellikler varsayılan olarak etkin değildir. Bu özellikleri etkinleştirmek için [PowerShell](/azure/key-vault/key-vault-soft-delete-powershell) veya [Azure CLI](/azure/key-vault/key-vault-soft-delete-cli)kullanın. Yalnızca RSA anahtarları ve anahtar boyutu 2048 desteklenir.

## <a name="assign-an-identity-to-the-cluster"></a>Kümeye bir kimlik atama

Kümeniz için müşteri tarafından yönetilen anahtarları etkinleştirmek üzere öncelikle kümeye bir sistem tarafından atanan yönetilen kimlik atayın. Bu yönetilen kimliği, anahtar kasasına erişmek için küme izinlerini vermek üzere kullanacaksınız. Sistem tarafından atanan yönetilen kimlikleri yapılandırmak için bkz. [Yönetilen kimlikler](/azure/data-explorer/managed-identities).

## <a name="create-a-new-key-vault"></a>Yeni bir Anahtar Kasası oluşturun

PowerShell kullanarak yeni bir Anahtar Kasası oluşturmak için [New-Azkeykasasını](/powershell/module/az.keyvault/new-azkeyvault)çağırın. Azure Veri Gezgini şifrelemesi için müşteri tarafından yönetilen anahtarları depolamak üzere kullandığınız anahtar kasasında iki anahtar koruma ayarı etkinleştirilmiş, **geçici silme** ve **Temizleme işlemi**yapılmalıdır. Köşeli ayraçlar içindeki yer tutucu değerlerini aşağıdaki örnekteki değerlerinizle değiştirin.

```azurepowershell-interactive
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

## <a name="configure-the-key-vault-access-policy"></a>Anahtar Kasası erişim ilkesini yapılandırma

Daha sonra, kümenin ona erişim izinleri olması için Anahtar Kasası için erişim ilkesini yapılandırın. Bu adımda, daha önce kümeye atadığınız sistem tarafından atanan yönetilen kimlik ' i kullanacaksınız. Anahtar Kasası erişim ilkesini ayarlamak için [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy)komutunu çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirin ve önceki örneklerde tanımlanan değişkenleri kullanın.

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $cluster.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Yeni anahtar oluştur

Ardından, anahtar kasasında yeni bir anahtar oluşturun. Yeni bir anahtar oluşturmak için [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey)komutunu çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirin ve önceki örneklerde tanımlanan değişkenleri kullanın.

```azurepowershell-interactive
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```
