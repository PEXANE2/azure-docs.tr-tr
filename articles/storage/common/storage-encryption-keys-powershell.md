---
title: Müşteri tarafından yönetilen anahtarları yapılandırmak için PowerShell 'i kullanma
titleSuffix: Azure Storage
description: Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırmak üzere PowerShell 'in nasıl kullanılacağını öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/13/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: a3fdde755a5e024efead5c8861a1d5cd769b6d23
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87036838"
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

PowerShell kullanarak yeni bir Anahtar Kasası oluşturmak için, [az. Keykasası](https://www.powershellgallery.com/packages/Az.KeyVault/2.0.0) PowerShell modülünün 2.0.0 veya sonraki bir sürümünü yüklemelisiniz. Sonra yeni bir Anahtar Kasası oluşturmak için [New-Azkeykasasını](/powershell/module/az.keyvault/new-azkeyvault) çağırın.

Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarları depolamak üzere kullandığınız anahtar kasasında iki anahtar koruma ayarı etkinleştirilmiş, **geçici silme** ve **Temizleme işlemi**yapılmalıdır. Yeni bir Anahtar Kasası oluşturduğunuzda, az. Keykasa modülünün sürüm 2.0.0 ve üzeri sürümlerde geçici silme özelliği varsayılan olarak etkindir.

Aşağıdaki örnek, **geçici silme** ve **Temizleme** özellikleri etkin olan yeni bir Anahtar Kasası oluşturur. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
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

Varsayılan olarak, Azure depolama şifrelemesi Microsoft tarafından yönetilen anahtarları kullanır. Bu adımda, Azure depolama hesabınızı Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırın ve ardından depolama hesabıyla ilişkilendirilecek anahtarı belirtin.

Şifrelemeyi müşteri tarafından yönetilen anahtarlarla yapılandırdığınızda, ilişkili anahtar kasasındaki sürüm değiştiğinde şifreleme için kullanılan anahtarı otomatik olarak döndürmeyi tercih edebilirsiniz. Alternatif olarak, anahtar sürümü el ile güncelleştirilene kadar şifreleme için kullanılacak bir anahtar sürümünü açıkça belirtebilirsiniz.

### <a name="configure-encryption-for-automatic-rotation-of-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarların otomatik olarak dönmesi için şifrelemeyi yapılandırma

Müşteri tarafından yönetilen anahtarların otomatik dönüşüyle ilgili şifrelemeyi yapılandırmak için, [az. Storage](https://www.powershellgallery.com/packages/Az.Storage) modülünü, sürüm 2.0.0 veya üstünü yüklemelisiniz.

Müşteri tarafından yönetilen anahtarları otomatik olarak döndürmek için, depolama hesabı için müşteri tarafından yönetilen anahtarları yapılandırırken anahtar sürümünü atlayın. Aşağıdaki örnekte gösterildiği gibi, depolama hesabının şifreleme ayarlarını güncelleştirmek için [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) öğesini çağırın ve depolama hesabı için müşteri tarafından yönetilen anahtarları etkinleştirmek üzere **-keyvaultencryption** seçeneğini ekleyin. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVaultUri $keyVault.VaultUri
```

### <a name="configure-encryption-for-manual-rotation-of-key-versions"></a>Anahtar sürümlerinin el ile dönmesi için şifrelemeyi yapılandırma

Şifreleme için kullanılacak bir anahtar sürümünü açıkça belirtmek için, depolama hesabı için müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırırken anahtar sürümünü sağlayın. Aşağıdaki örnekte gösterildiği gibi, depolama hesabının şifreleme ayarlarını güncelleştirmek için [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) öğesini çağırın ve depolama hesabı için müşteri tarafından yönetilen anahtarları etkinleştirmek üzere **-keyvaultencryption** seçeneğini ekleyin. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

Anahtar sürümünü el ile döndürdüğünüzde, yeni sürümü kullanmak için depolama hesabının şifreleme ayarlarını güncelleştirmeniz gerekir. İlk olarak, anahtarın en son sürümünü almak için [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey) öğesini çağırın. Ardından, önceki örnekte gösterildiği gibi, depolama hesabının şifreleme ayarlarını anahtarın yeni sürümünü kullanacak şekilde güncelleştirmek için [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) komutunu çağırın.

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Azure depolama şifrelemesi için kullanılan anahtarı değiştirmek için, [set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) komutunu, [müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma](#configure-encryption-with-customer-managed-keys) bölümünde gösterildiği gibi çağırın ve yeni anahtar adını ve sürümünü sağlayın. Yeni anahtar farklı bir anahtar kasasında ise, Anahtar Kasası URI 'sini de güncelleştirin.

## <a name="revoke-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları iptal et

Anahtar Kasası erişim ilkesini kaldırarak müşteri tarafından yönetilen anahtarları iptal edebilirsiniz. Müşteri tarafından yönetilen bir anahtarı iptal etmek için, aşağıdaki örnekte gösterildiği gibi [Remove-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/remove-azkeyvaultaccesspolicy) komutunu çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

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
