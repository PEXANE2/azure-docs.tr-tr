---
title: Tablolar ve kuyruklar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturun
titleSuffix: Azure Storage
description: Tablolar ve kuyruklar için müşteri tarafından yönetilen anahtarları yapılandırmayı destekleyen bir depolama hesabı oluşturmayı öğrenin. Azure depolama şifrelemesi için hesap şifreleme anahtarını temel alan bir depolama hesabı oluşturmak için Azure CLı veya Azure Resource Manager şablonunu kullanın. Daha sonra hesap için müşteri tarafından yönetilen anahtarları yapılandırabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 01/10/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 39568d65978592b360512db2e556cfa6b59760b6
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921213"
---
# <a name="create-an-account-that-supports-customer-managed-keys-for-tables-and-queues"></a>Tablolar ve kuyruklar için müşteri tarafından yönetilen anahtarları destekleyen bir hesap oluşturun

Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Varsayılan olarak, kuyruk depolama ve tablo depolama, hizmet kapsamındaki ve Microsoft tarafından yönetilen bir anahtar kullanır. Ayrıca, kuyruk veya tablo verilerini şifrelemek için müşteri tarafından yönetilen anahtarlar kullanmayı da tercih edebilirsiniz. Müşteri tarafından yönetilen anahtarları kuyruklar ve tablolarla birlikte kullanmak için, öncelikle hesap kapsamında olan ve hizmet yerine, bir şifreleme anahtarı kullanan bir depolama hesabı oluşturmanız gerekir. Kuyruk ve tablo verileri için hesap şifreleme anahtarını kullanan bir hesap oluşturduktan sonra, müşteri tarafından yönetilen anahtarları bu depolama hesabı için Azure Key Vault ile yapılandırabilirsiniz.

Bu makalede, hesap kapsamındaki bir anahtara dayanan bir depolama hesabının nasıl oluşturulacağı açıklanır. Hesap ilk oluşturulduğunda, Microsoft hesap anahtarını hesaptaki verileri şifrelemek için kullanır ve Microsoft bu anahtarı yönetir. Daha sonra, kendi anahtarlarınızı sağlamak, anahtar sürümünü güncelleştirmek, anahtarları döndürmek ve erişim denetimlerini iptal etmek dahil olmak üzere bu avantajlardan yararlanmak için hesap için müşteri tarafından yönetilen anahtarları yapılandırabilirsiniz.

## <a name="about-the-feature"></a>Özelliği hakkında

Kuyruk ve tablo depolaması için hesap şifreleme anahtarına dayanan bir depolama hesabı oluşturmak için, önce bu özelliği Azure ile kullanmak üzere kaydolmanız gerekir. Sınırlı kapasite nedeniyle, erişim isteklerinin onaylanabilmesi için birkaç ay sürebileceğinden emin olun.

Aşağıdaki bölgelerde kuyruk ve tablo depolaması için hesap şifreleme anahtarına dayanan bir depolama hesabı oluşturabilirsiniz:

- Doğu ABD
- Güney Orta ABD
- Batı ABD 2  

### <a name="register-to-use-the-account-encryption-key"></a>Hesap şifreleme anahtarını kullanmak için kaydolun

Azure CLı ile kaydolmak için [az Feature Register](/cli/azure/feature#az-feature-register) komutunu çağırın.

Hesap şifreleme anahtarını kuyruk depolama ile kullanmak üzere kaydolmak için:

```azurecli
az feature register --namespace Microsoft.Storage --name AllowAccountEncryptionKeyForQueues
```

Hesap şifreleme anahtarını tablo depolama ile kullanmak üzere kaydolmak için:

```azurecli
az feature register --namespace Microsoft.Storage --name AllowAccountEncryptionKeyForTables
```

### <a name="check-the-status-of-your-registration"></a>Kaydlarınızın durumunu denetleyin

Kuyruk depolaması için kaydlarınızın durumunu denetlemek için:

```azurecli
az feature show --namespace Microsoft.Storage --name AllowAccountEncryptionKeyForQueues
```

Tablo depolaması için kaydlarınızın durumunu denetlemek için:

```azurecli
az feature show --namespace Microsoft.Storage --name AllowAccountEncryptionKeyForTables
```

### <a name="re-register-the-azure-storage-resource-provider"></a>Azure depolama kaynak sağlayıcısı 'nı yeniden kaydetme

Kaydınız onaylandıktan sonra Azure depolama kaynak sağlayıcısı 'nı yeniden kaydetmeniz gerekir. [Az Provider Register](/cli/azure/provider#az-provider-register) komutunu çağırın:

```azurecli
az provider register --namespace 'Microsoft.Storage'
```

## <a name="create-an-account-that-uses-the-account-encryption-key"></a>Hesap şifreleme anahtarını kullanan bir hesap oluşturun

Depolama hesabını oluşturduğunuz sırada kuyruklar ve tablolar için hesap şifreleme anahtarını kullanmak üzere yeni bir depolama hesabı yapılandırmanız gerekir. Şifreleme anahtarının kapsamı, hesap oluşturulduktan sonra değiştirilemez.

Depolama hesabının genel amaçlı v2 türünde olması gerekir. Depolama hesabı oluşturabilir ve bunu, Azure CLı veya bir Azure Resource Manager şablonu kullanarak hesap şifreleme anahtarına göre yapılandırabilirsiniz.

> [!NOTE]
> Depolama hesabı oluşturulduğunda, yalnızca kuyruk ve tablo depolaması, hesap şifreleme anahtarıyla verileri şifrelemek için isteğe bağlı olarak yapılandırılabilir. BLOB depolama ve Azure dosyaları her zaman, verileri şifrelemek için hesap şifreleme anahtarını kullanır.

### <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Hesap şifreleme anahtarına dayanan bir depolama hesabı oluşturmak için Azure CLı 'yi kullanmak için Azure CLı sürüm 2.0.80 veya üstünü yüklediğinizden emin olun. Daha fazla bilgi için [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

Ardından, [az Storage Account Create](/cli/azure/storage/account#az-storage-account-create) komutunu çağırarak, uygun parametrelerle genel amaçlı v2 depolama hesabı oluşturun:

- Sıra depolamada verileri şifrelemek için hesap şifreleme anahtarını kullanmak üzere `--encryption-key-type-for-queue` seçeneğini ekleyin ve değerini `Account` olarak ayarlayın.
- `--encryption-key-type-for-table` seçeneğini ekleyin ve değerini tablo depolamadaki verileri şifrelemek üzere hesap şifreleme anahtarını kullanmak için `Account` olarak ayarlayın.

Aşağıdaki örnek, LRS için yapılandırılmış ve sıra ve tablo depolama için verileri şifrelemek üzere hesap şifreleme anahtarını kullanan genel amaçlı v2 depolama hesabının nasıl oluşturulacağını gösterir. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli
az storage account create \
    --name <storage-account> \
    --resource-group <resource-group> \
    --location <location> \
    --sku Standard_LRS \
    --kind StorageV2 \
    --encryption-key-type-for-table Account \
    --encryption-key-type-for-queue Account
```

### <a name="templatetabtemplate"></a>[Şablon](#tab/template)

Aşağıdaki JSON örneği, LRS için yapılandırılmış ve hem sıra hem de tablo depolama için verileri şifrelemek üzere hesap şifreleme anahtarını kullanan, genel amaçlı bir v2 depolama hesabı oluşturur. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

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
            "name": "[parameters('Standard_LRS')]"
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

Hesap şifreleme anahtarına dayanan bir hesap oluşturduktan sonra, müşteri tarafından yönetilen anahtarları Azure Key Vault yapılandırmak için aşağıdaki makalelerden birine bakın:

- [Azure portal kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault yapılandırma](storage-encryption-keys-portal.md)
- [PowerShell kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault yapılandırma](storage-encryption-keys-powershell.md)
- [Azure CLı kullanarak, müşteri tarafından yönetilen anahtarları Azure Key Vault ile yapılandırma](storage-encryption-keys-cli.md)

## <a name="verify-the-account-encryption-key"></a>Hesap şifreleme anahtarını doğrulama

Yeni depolama hesabının hesap şifreleme anahtarını kullandığını doğrulamak için Azure CLı [az Storage Account](/cli/azure/storage/account#az-storage-account-show) komutunu çağırın. Bu komut, depolama hesabı özelliklerinin ve bunların değerlerinin listesini döndürür. `keyType` özelliğini bulun ve `Account`olarak ayarlandığını doğrulayın.

```azurecli
az storage account show /
    --name <storage-account> /
    --resource-group <resource-group>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veriler için Azure depolama şifrelemesi](storage-service-encryption.md) 
- [Azure Key Vault nedir](https://docs.microsoft.com/azure/key-vault/key-vault-overview)?
