---
title: Tablolar ve kuyruklar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturma
titleSuffix: Azure Storage
description: Tablolar ve kuyruklar için müşteri tarafından yönetilen anahtarları yapılandırmayı destekleyen bir depolama hesabı oluşturmayı öğrenin. Azure Depolama şifrelemesi için hesap şifreleme anahtarına dayanan bir depolama hesabı oluşturmak için Azure CLI veya Azure Kaynak Yöneticisi şablonuna kullanın. Daha sonra hesap için müşteri tarafından yönetilen anahtarları yapılandırabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/05/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 09558a8d1e4e2dc68cefd2c870f54e008d10b97b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77083559"
---
# <a name="create-an-account-that-supports-customer-managed-keys-for-tables-and-queues"></a>Tablolar ve kuyruklar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturma

Azure Depolama, bir depolama hesabındaki tüm verileri istirahat halinde şifreler. Varsayılan olarak, Sıra depolama ve Tablo depolama hizmeti kapsamına giren ve Microsoft tarafından yönetilen bir anahtar kullanır. Sıra veya tablo verilerini şifrelemek için müşteri tarafından yönetilen anahtarları kullanmayı da tercih edebilirsiniz. Sıraları ve tabloları olan müşteri tarafından yönetilen anahtarları kullanmak için, önce hizmet yerine hesaba kapsamlı bir şifreleme anahtarı kullanan bir depolama hesabı oluşturmanız gerekir. Sıra ve tablo verileri için hesap şifreleme anahtarını kullanan bir hesap oluşturduktan sonra, söz konusu depolama hesabı için Azure Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırabilirsiniz.

Bu makalede, hesaba yönelik bir anahtara dayanan bir depolama hesabının nasıl oluşturuluyorum. Hesap ilk oluşturulduğunda, Microsoft hesaptaki verileri şifrelemek için hesap anahtarını kullanır ve Microsoft anahtarı yönetir. Daha sonra, kendi anahtarlarınızı sağlama, anahtar sürümünü güncelleştirme, anahtarları döndürme ve erişim denetimlerini iptal etme olanağı da dahil olmak üzere bu avantajlardan yararlanmak için hesap için müşteri tarafından yönetilen anahtarları yapılandırabilirsiniz.

## <a name="about-the-feature"></a>Özellik hakkında

Sıra ve Tablo depolama için hesap şifreleme anahtarına dayanan bir depolama hesabı oluşturmak için, öncelikle bu özelliği Azure ile kullanmak için kaydolmanız gerekir. Sınırlı kapasite nedeniyle, erişim isteklerinin onaylanmasının birkaç ay sürebileceğini unutmayın.

Aşağıdaki bölgelerde Sıra ve Tablo depolama için hesap şifreleme anahtarına dayanan bir depolama hesabı oluşturabilirsiniz:

- Doğu ABD
- Orta Güney ABD
- Batı ABD 2  

### <a name="register-to-use-the-account-encryption-key"></a>Hesap şifreleme anahtarını kullanmak için kaydolun

Hesap şifreleme anahtarını Sıra veya Tablo depolama alanına kullanmak için kaydolmak için PowerShell veya Azure CLI'yi kullanın.

# <a name="powershell"></a>[Powershell](#tab/powershell)

PowerShell'e kaydolmak için [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature) komutunu arayın.

```powershell
Register-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowAccountEncryptionKeyForQueues
Register-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowAccountEncryptionKeyForTables
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI'ye kaydolmak için [az özelliği kayıt](/cli/azure/feature#az-feature-register) komutunu arayın.

```azurecli
az feature register --namespace Microsoft.Storage \
    --name AllowAccountEncryptionKeyForQueues
az feature register --namespace Microsoft.Storage \
    --name AllowAccountEncryptionKeyForTables
```

# <a name="template"></a>[Şablon](#tab/template)

Yok

---

### <a name="check-the-status-of-your-registration"></a>Kaydınızın durumunu kontrol edin

Sıra veya Tablo depolama için kaydınızın durumunu kontrol etmek için PowerShell veya Azure CLI'yi kullanın.

# <a name="powershell"></a>[Powershell](#tab/powershell)

PowerShell ile kaydınızın durumunu kontrol etmek için [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature) komutunu arayın.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowAccountEncryptionKeyForQueues
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowAccountEncryptionKeyForTables
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI ile kaydınızın durumunu kontrol etmek için [az özelliği](/cli/azure/feature#az-feature-show) komutunu arayın.

```azurecli
az feature show --namespace Microsoft.Storage \
    --name AllowAccountEncryptionKeyForQueues
az feature show --namespace Microsoft.Storage \
    --name AllowAccountEncryptionKeyForTables
```

# <a name="template"></a>[Şablon](#tab/template)

Yok

---

### <a name="re-register-the-azure-storage-resource-provider"></a>Azure Depolama kaynak sağlayıcısını yeniden kaydedin

Kaydınız onaylandıktan sonra Azure Depolama kaynak sağlayıcısını yeniden kaydetmeniz gerekir. Kaynak sağlayıcısını yeniden kaydetmek için PowerShell veya Azure CLI'yi kullanın.

# <a name="powershell"></a>[Powershell](#tab/powershell)

Kaynak sağlayıcısını PowerShell'e yeniden kaydetmek için [Register-AzResourceProvider](/powershell/module/az.resources/register-azresourceprovider) komutunu arayın.

```powershell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.Storage'
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Kaynak sağlayıcısını Azure CLI'ye yeniden kaydetmek için [az sağlayıcı kayıt](/cli/azure/provider#az-provider-register) komutunu arayın.

```azurecli
az provider register --namespace 'Microsoft.Storage'
```

# <a name="template"></a>[Şablon](#tab/template)

Yok

---

## <a name="create-an-account-that-uses-the-account-encryption-key"></a>Hesap şifreleme anahtarını kullanan bir hesap oluşturma

Depolama hesabını oluşturduğunuz sırada kuyruklar ve tablolar için hesap şifreleme anahtarını kullanacak şekilde yeni bir depolama hesabı yapılandırmanız gerekir. Hesap oluşturulduktan sonra şifreleme anahtarının kapsamı değiştirilemez.

Depolama hesabı genel amaçlı v2 türünde olmalıdır. Azure CLI veya Azure Kaynak Yöneticisi şablonu kullanarak depolama hesabını oluşturabilir ve hesap şifreleme anahtarına güvenecek şekilde yapılandırabilirsiniz.

> [!NOTE]
> Yalnızca Sıra ve Tablo depolama, depolama hesabı oluşturulduğunda hesap şifreleme anahtarıyla verileri şifrelemek için isteğe bağlı olarak yapılandırılabilir. Blob depolama ve Azure Dosyaları verileri şifrelemek için her zaman hesap şifreleme anahtarını kullanır.

# <a name="powershell"></a>[Powershell](#tab/powershell)

Hesap şifreleme anahtarına dayanan bir depolama hesabı oluşturmak için PowerShell'i kullanmak için Azure PowerShell modüllerini, sürüm 3.4.0 veya sonraki sürümünü yüklediğinizden emin olun. Daha fazla bilgi için Azure [PowerShell modüllerini yükleyin.](/powershell/azure/install-az-ps)

Ardından, uygun parametrelerle Birlikte Yeni Depolama [Hesabı](/powershell/module/az.storage/new-azstorageaccount) komutunu arayarak genel amaçlı bir v2 depolama hesabı oluşturun:

- `-EncryptionKeyTypeForQueue` Seçenek ekleyin ve Sıra `Account` depolama verileri şifrelemek için hesap şifreleme anahtarı nı kullanmak için değerini ayarlayın.
- `-EncryptionKeyTypeForTable` Seçeneği ekleyin ve Tablo `Account` depolamasındaverileri şifrelemek için hesap şifreleme anahtarını kullanmak üzere değerini ayarlayın.

Aşağıdaki örnek, okuma-erişim coğrafi depolama (RA-GRS) için yapılandırılan ve hem Sıra hem de Tablo depolama için verileri şifrelemek için hesap şifreleme anahtarını kullanan genel amaçlı bir v2 depolama hesabının nasıl oluşturulduğunu gösterir. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
New-AzStorageAccount -ResourceGroupName <resource_group> `
    -AccountName <storage-account> `
    -Location <location> `
    -SkuName "Standard_RAGRS" `
    -Kind StorageV2 `
    -EncryptionKeyTypeForTable Account `
    -EncryptionKeyTypeForQueue Account
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Hesap şifreleme anahtarına dayanan bir depolama hesabı oluşturmak için Azure CLI'yi kullanmak için Azure CLI sürümünü 2.0.80 veya daha sonra yüklediğinizden emin olun. Daha fazla bilgi için azure [cli'yi yükleyin.](/cli/azure/install-azure-cli)

Ardından, uygun parametrelerle az depolama hesabı [oluşturma](/cli/azure/storage/account#az-storage-account-create) komutunu arayarak genel amaçlı bir v2 depolama hesabı oluşturun:

- `--encryption-key-type-for-queue` Seçenek ekleyin ve Sıra `Account` depolama verileri şifrelemek için hesap şifreleme anahtarı nı kullanmak için değerini ayarlayın.
- `--encryption-key-type-for-table` Seçeneği ekleyin ve Tablo `Account` depolamasındaverileri şifrelemek için hesap şifreleme anahtarını kullanmak üzere değerini ayarlayın.

Aşağıdaki örnek, okuma-erişim coğrafi depolama (RA-GRS) için yapılandırılan ve hem Sıra hem de Tablo depolama için verileri şifrelemek için hesap şifreleme anahtarını kullanan genel amaçlı bir v2 depolama hesabının nasıl oluşturulduğunu gösterir. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage account create \
    --name <storage-account> \
    --resource-group <resource-group> \
    --location <location> \
    --sku Standard_RAGRS \
    --kind StorageV2 \
    --encryption-key-type-for-table Account \
    --encryption-key-type-for-queue Account
```

# <a name="template"></a>[Şablon](#tab/template)

Aşağıdaki JSON örneği, okuma-erişim coğrafi depolama (RA-GRS) için yapılandırılan ve hem Sıra hem de Tablo depolama için verileri şifrelemek için hesap şifreleme anahtarını kullanan genel amaçlı bir v2 depolama hesabı oluşturur. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```json
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2019-06-01",
        "name": "[parameters('<storage-account>')]",
        "location": "[parameters('<location>')]",
        "dependsOn": [],
        "tags": {},
        "sku": {
            "name": "[parameters('Standard_RAGRS')]"
        },
        "kind": "[parameters('StorageV2')]",
        "properties": {
            "accessTier": "[parameters('<accessTier>')]",
            "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]",
            "largeFileSharesState": "[parameters('<largeFileSharesState>')]",
            "encryption": {
                "services": {
                    "queue": {
                        "keyType": "Account"
                    },
                    "table": {
                        "keyType": "Account"
                    }
                },
                "keySource": "Microsoft.Storage"
            }
        }
    }
],
```

---

Hesap şifreleme anahtarına dayanan bir hesap oluşturduktan sonra, Azure Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırmak için aşağıdaki makalelerden birine bakın:

- [Azure portalını kullanarak Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırın](storage-encryption-keys-portal.md)
- [PowerShell'i kullanarak Azure Key Vault ile müşteri tarafından yönetilen tuşları yapılandırın](storage-encryption-keys-powershell.md)
- [Azure CLI'yi kullanarak müşteri tarafından yönetilen anahtarları Azure Anahtar Kasası ile yapılandırın](storage-encryption-keys-cli.md)

## <a name="verify-the-account-encryption-key"></a>Hesap şifreleme anahtarını doğrulama

Depolama hesabındaki bir hizmetin hesap şifreleme anahtarını kullandığını doğrulamak için Azure CLI [az depolama hesabı](/cli/azure/storage/account#az-storage-account-show) komutunu arayın. Bu komut, depolama hesabı özellikleri ve değerlerini bir dizi döndürür. Şifreleme özelliğiiçindeki `keyType` her hizmet için alanı arayın ve '' olarak `Account`ayarlı olduğunu doğrulayın.

# <a name="powershell"></a>[Powershell](#tab/powershell)

Depolama hesabındaki bir hizmetin hesap şifreleme anahtarını kullandığını doğrulamak için [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) komutunu arayın. Bu komut, depolama hesabı özellikleri ve değerlerini bir dizi döndürür. Özellik içindeki `KeyType` her hizmet için alanı arayın ve '' olarak `Account`ayarlı olduğunu doğrulayın. `Encryption`

```powershell
$account = Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -StorageAccountName <storage-account>
$account.Encryption.Services.Queue
$account.Encryption.Services.Table
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Depolama hesabındaki bir hizmetin hesap şifreleme anahtarını kullandığını doğrulamak için [az depolama hesabı](/cli/azure/storage/account#az-storage-account-show) komutunu arayın. Bu komut, depolama hesabı özellikleri ve değerlerini bir dizi döndürür. Şifreleme özelliğiiçindeki `keyType` her hizmet için alanı arayın ve '' olarak `Account`ayarlı olduğunu doğrulayın.

```azurecli
az storage account show /
    --name <storage-account> /
    --resource-group <resource-group>
```

# <a name="template"></a>[Şablon](#tab/template)

Yok

---

## <a name="next-steps"></a>Sonraki adımlar

- [Veriler için Azure Depolama şifrelemesi](storage-service-encryption.md) 
- [Azure Anahtar Kasası Nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
