---
title: Müşteri tarafından yönetilen anahtarları yapılandırmak için PowerShell 'i kullanma
titleSuffix: Azure Storage
description: Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırmak üzere PowerShell 'in nasıl kullanılacağını öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 04/02/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 6b2983bbaf22ae1b9e09ff3362a4bc06e6658b33
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85506216"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-powershell"></a>PowerShell kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault yapılandırma

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Bu makalede, PowerShell kullanarak müşteri tarafından yönetilen anahtarlarla bir Azure Key Vault nasıl yapılandırılacağı gösterilmektedir. Azure CLı kullanarak bir Anahtar Kasası oluşturmayı öğrenmek için bkz. [hızlı başlangıç: PowerShell kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma](../../key-vault/secrets/quick-create-powershell.md).

## <a name="assign-an-identity-to-the-storage-account"></a>Depolama hesabına bir kimlik atayın

Depolama hesabınız için müşteri tarafından yönetilen anahtarları etkinleştirmek üzere öncelikle depolama hesabına sistem tarafından atanan bir yönetilen kimlik atayın. Bu yönetilen kimliği, depolama hesabı izinlerini anahtar kasasına erişim vermek için kullanacaksınız.

PowerShell kullanarak yönetilen bir kimlik atamak için [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount)' ı çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```powershell
$storageAccount = Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -Name <storage-account> `
    -AssignIdentity
```

Sistem tarafından atanan yönetilen kimlikleri PowerShell ile yapılandırma hakkında daha fazla bilgi için bkz. [PowerShell kullanarak Azure VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Yeni bir Anahtar Kasası oluşturun

PowerShell kullanarak yeni bir Anahtar Kasası oluşturmak için [New-Azkeykasasını](/powershell/module/az.keyvault/new-azkeyvault)çağırın. Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarları depolamak üzere kullandığınız anahtar kasasında iki anahtar koruma ayarı etkinleştirilmiş, **geçici silme** ve **Temizleme işlemi**yapılmalıdır.

Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

**Geçici silme** özelliğini etkinleştirme ve var olan bir anahtar kasasında PowerShell ile **Temizleme** hakkında bilgi edinmek için, [PowerShell ile geçici silme özelliğini kullanma](../../key-vault/general/soft-delete-powershell.md)bölümünde **geçici** silme ve **Temizleme korumasını** etkinleştirme başlıklı bölümlere bakın.

## <a name="configure-the-key-vault-access-policy"></a>Anahtar Kasası erişim ilkesini yapılandırma

Daha sonra, depolama hesabının bu hesaba erişim izinleri olması için Anahtar Kasası için erişim ilkesini yapılandırın. Bu adımda, daha önce depolama hesabına atadığınız yönetilen kimliği kullanacaksınız.

Anahtar Kasası erişim ilkesini ayarlamak için [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy)komutunu çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get
```

## <a name="create-a-new-key"></a>Yeni anahtar oluştur

Ardından, anahtar kasasında yeni bir anahtar oluşturun. Yeni bir anahtar oluşturmak için [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey)komutunu çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```

Azure depolama şifrelemesi, 2048, 3072 ve 4096 boyutlarının RSA ve RSA-HSM anahtarlarını destekler. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys) **Key Vault anahtarlar** .

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma

Varsayılan olarak, Azure depolama şifrelemesi Microsoft tarafından yönetilen anahtarları kullanır. Bu adımda, Azure depolama hesabınızı müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırın ve depolama hesabıyla ilişkilendirilecek anahtarı belirtin.

Aşağıdaki örnekte gösterildiği gibi, depolama hesabının şifreleme ayarlarını güncelleştirmek için [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) komutunu çağırın. Depolama hesabı için müşteri tarafından yönetilen anahtarları etkinleştirmek üzere **-keyvaultencryption** seçeneğini dahil edin. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için depolama hesabını güncelleştirmeniz gerekir. İlk olarak, anahtarın en son sürümünü almak için [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey) öğesini çağırın. Ardından, önceki bölümde gösterildiği gibi, depolama hesabının şifreleme ayarlarını anahtarın yeni sürümünü kullanacak şekilde güncelleştirmek için [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) komutunu çağırın.

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Azure depolama şifrelemesi için kullanılan anahtarı değiştirmek için, [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) komutunu, [müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma](#configure-encryption-with-customer-managed-keys) bölümünde gösterildiği gibi çağırın ve yeni anahtar adını ve sürümünü sağlayın. Yeni anahtar farklı bir anahtar kasasında ise, Anahtar Kasası URI 'sini de güncelleştirin.

## <a name="revoke-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları iptal et

Bir anahtarın tehlikede olduğunu düşünüyorsanız, Anahtar Kasası erişim ilkesini kaldırarak müşteri tarafından yönetilen anahtarları iptal edebilirsiniz. Müşteri tarafından yönetilen bir anahtarı iptal etmek için, aşağıdaki örnekte gösterildiği gibi [Remove-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/remove-azkeyvaultaccesspolicy) komutunu çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Remove-AzKeyVaultAccessPolicy -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
```

## <a name="disable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları devre dışı bırak

Müşteri tarafından yönetilen anahtarları devre dışı bıraktığınızda, depolama hesabınız Microsoft tarafından yönetilen anahtarlarla yeniden şifrelenir. Müşteri tarafından yönetilen anahtarları devre dışı bırakmak için, aşağıdaki örnekte gösterildiği gibi [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) komutunu `-StorageEncryption` seçeneğiyle çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -StorageEncryption  
```

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veri için Azure Depolama şifrelemesi](storage-service-encryption.md)
- [Azure Key Vault nedir](https://docs.microsoft.com/azure/key-vault/key-vault-overview)?
