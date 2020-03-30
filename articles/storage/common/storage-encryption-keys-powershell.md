---
title: Müşteri tarafından yönetilen anahtarları yapılandırmak için PowerShell'i kullanın
titleSuffix: Azure Storage
description: Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırmak için PowerShell'i nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/10/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 264dbbaedca5a28c8741d699a683b3e2b2385383
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061148"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-powershell"></a>PowerShell'i kullanarak Azure Key Vault ile müşteri tarafından yönetilen tuşları yapılandırın

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Bu makalede, PowerShell kullanarak müşteri tarafından yönetilen anahtarlarla azure anahtar kasası nasıl yapılandırılanın gösterilmektedir. Azure CLI kullanarak anahtar kasası oluşturmayı öğrenmek için [Quickstart: PowerShell'i kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın.](../../key-vault/quick-create-powershell.md)

## <a name="assign-an-identity-to-the-storage-account"></a>Depolama hesabına kimlik atama

Depolama hesabınız için müşteri tarafından yönetilen anahtarları etkinleştirmek için, önce depolama hesabına sistem tarafından atanmış yönetilen bir kimlik atayın. Bu yönetilen kimliği, depolama hesabının anahtar kasasına erişme izni vermek için kullanırsınız.

PowerShell kullanarak yönetilen bir kimlik atamak için [Set-AzStorageAccount'u](/powershell/module/az.storage/set-azstorageaccount)arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```powershell
$storageAccount = Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -Name <storage-account> `
    -AssignIdentity
```

PowerShell ile sistem tarafından atanan yönetilen kimlikleri yapılandırma hakkında daha fazla bilgi için, [PowerShell'i kullanarak Azure vm'de Azure kaynakları için yönetilen kimlikleri yapılandırma](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)'ya bakın.

## <a name="create-a-new-key-vault"></a>Yeni bir anahtar kasası oluşturma

PowerShell kullanarak yeni bir anahtar kasası oluşturmak için [New-AzKeyVault'u](/powershell/module/az.keyvault/new-azkeyvault)arayın. Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları depolamak için kullandığınız anahtar kasasının iki anahtar koruma ayarı etkin leştirilmiş olmalıdır: **Yumuşak Silme** ve **Temizleme).**

Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

**PowerShell** ile varolan bir anahtar kasasında Soft Delete ve **Do Sunurge'ı** etkinleştirmeyi öğrenmek için [PowerShell ile yumuşak](../../key-vault/key-vault-soft-delete-powershell.md) **silmeyi etkinleştirme** ve **Temizleme Korumasını Etkinleştirme** başlıklı bölümlere bakın.

## <a name="configure-the-key-vault-access-policy"></a>Anahtar kasa erişim ilkesini yapılandırma

Ardından, depolama hesabının erişim izinleri olacak şekilde anahtar kasasının erişim ilkesini yapılandırın. Bu adımda, depolama hesabına daha önce atadığınız yönetilen kimliği kullanırsınız.

Anahtar kasasının erişim ilkesini ayarlamak için [Set-AzKeyVaultAccessPolicy'yi](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy)arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>Yeni bir anahtar oluşturma

Ardından, anahtar kasasında yeni bir anahtar oluşturun. Yeni bir anahtar oluşturmak için [Add-AzKeyVaultKey'i](/powershell/module/az.keyvault/add-azkeyvaultkey)arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma

Varsayılan olarak, Azure Depolama şifrelemesi Microsoft tarafından yönetilen anahtarları kullanır. Bu adımda, Azure Depolama hesabınızı müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırın ve depolama hesabıyla ilişkilendirilen anahtarı belirtin.

Aşağıdaki örnekte gösterildiği gibi, depolama hesabının şifreleme ayarlarını güncelleştirmek için [Set-AzStorageAccount'u](/powershell/module/az.storage/set-azstorageaccount) arayın. Depolama hesabı için müşteri tarafından yönetilen anahtarları etkinleştirmek için **-KeyvaultEncryption** seçeneğini ekleyin. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için depolama hesabını güncelleştirmeniz gerekir. İlk olarak, anahtarın en son sürümünü almak için [Get-AzKeyVaultKey'i](/powershell/module/az.keyvault/get-azkeyvaultkey) arayın. Ardından, önceki bölümde gösterildiği gibi anahtarın yeni sürümünü kullanmak için depolama hesabının şifreleme ayarlarını güncelleştirmek için [Set-AzStorageAccount'u](/powershell/module/az.storage/set-azstorageaccount) arayın.

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Azure Depolama şifrelemesi için kullanılan anahtarı değiştirmek için, [şifrelemeyi müşteri tarafından yönetilen anahtarlarla yapılandır'da](#configure-encryption-with-customer-managed-keys) gösterildiği gibi [Set-AzStorageAccount'u](/powershell/module/az.storage/set-azstorageaccount) arayın ve yeni anahtar adını ve sürümünü sağlayın. Yeni anahtar farklı bir anahtar kasasındaysa, uri anahtar kasasını da güncelleyin.

## <a name="revoke-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları iptal edin

Bir anahtarın ele geçirildiğini düşünüyorsanız, anahtar kasa erişim ilkesini kaldırarak müşteri tarafından yönetilen anahtarları iptal edebilirsiniz. Müşteri tarafından yönetilen bir anahtarı iptal etmek için aşağıdaki örnekte gösterildiği gibi [Kaldır-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/remove-azkeyvaultaccesspolicy) komutunu arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Remove-AzKeyVaultAccessPolicy -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
```

## <a name="disable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları devre dışı

Müşteri tarafından yönetilen anahtarları devre dışı dışı bettiğinizde, depolama hesabınız bir kez daha Microsoft tarafından yönetilen anahtarlarla şifrelenir. Müşteri tarafından yönetilen anahtarları devre dışı bırakarak, aşağıdaki `-StorageEncryption` örnekte gösterildiği gibi [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) seçeneğini belirleyin. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -StorageEncryption  
```

## <a name="next-steps"></a>Sonraki adımlar

- [Veriler için Azure Depolama şifrelemesi](storage-service-encryption.md)
- [Azure Anahtar Kasası Nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
