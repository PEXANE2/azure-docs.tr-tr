---
title: Depolama hesabı için hangi şifreleme anahtar modelinin kullanıldığını belirleme
titleSuffix: Azure Storage
description: Depolama hesabı için şifreleme anahtarlarının nasıl yönetildiğini kontrol etmek için Azure portalı, PowerShell veya Azure CLI'yi kullanın. Anahtarlar Microsoft (varsayılan) veya müşteri tarafından yönetilebilir. Müşteri tarafından yönetilen anahtarlar Azure Key Vault'ta depolanmalıdır.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/13/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 0df0ba4ce76d249bcb4738b41c94677e061f14ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409873"
---
# <a name="determine-which-azure-storage-encryption-key-model-is-in-use-for-the-storage-account"></a>Depolama hesabı için hangi Azure Depolama şifreleme anahtar modelinin kullanıldığını belirleme

Depolama hesabınızdaki veriler Azure Depolama tarafından otomatik olarak şifrelenir. Azure Depolama şifrelemesi, şifreleme anahtarlarını depolama hesabı düzeyinde yönetmek için iki seçenek sunar:

- **Microsoft tarafından yönetilen anahtarlar.** Varsayılan olarak, Microsoft depolama hesabınızı şifrelemek için kullanılan anahtarları yönetir.
- **Müşteri tarafından yönetilen anahtarlar.** İsteğe bağlı olarak depolama hesabınız için şifreleme anahtarlarını yönetmeyi seçebilirsiniz. Müşteri tarafından yönetilen anahtarlar Azure Key Vault'ta depolanmalıdır.

Ayrıca, bazı Blob depolama işlemleri için tek tek bir istek düzeyinde bir şifreleme anahtarı sağlayabilirsiniz. İstekte bir şifreleme anahtarı belirtildiğinde, bu anahtar depolama hesabında etkin olan şifreleme anahtarını geçersiz kılar. Daha fazla bilgi için bkz. [Blob depolama alanına yapılan bir istekte müşteri tarafından sağlanan bir anahtar belirtin.](../blobs/storage-blob-customer-provided-key.md)

Şifreleme anahtarları hakkında daha fazla bilgi için, [veriler için Azure Depolama şifrelemesi'ne](storage-service-encryption.md)bakın.

## <a name="check-the-encryption-key-model-for-the-storage-account"></a>Depolama hesabı için şifreleme anahtar modelini denetleme

Depolama hesabının Şifreleme için Microsoft tarafından yönetilen anahtarları mı yoksa müşteri tarafından yönetilen anahtarları mı kullandığını belirlemek için aşağıdaki yaklaşımlardan birini kullanın.

# <a name="azure-portal"></a>[Azure portalında](#tab/portal)

Azure portalını kullanarak depolama hesabının şifreleme modelini denetlemek için aşağıdaki adımları izleyin:

1. Azure portalında depolama hesabınıza gidin.
1. **Şifreleme** ayarını seçin ve ayarı not edin.

Aşağıdaki resim, Microsoft tarafından yönetilen anahtarlarla şifrelenmiş bir depolama hesabı gösterir:

![Microsoft tarafından yönetilen anahtarlarla şifrelenmiş hesabı görüntüleme](media/storage-encryption-key-model-get/microsoft-managed-encryption-key-setting-portal.png)

Ve aşağıdaki resim, müşteri tarafından yönetilen anahtarlarla şifrelenmiş bir depolama hesabı gösterir:

![Azure portalında şifreleme anahtarı ayarını gösteren ekran görüntüsü](media/storage-encryption-key-model-get/customer-managed-encryption-key-setting-portal.png)

# <a name="powershell"></a>[Powershell](#tab/powershell)

PowerShell'i kullanarak depolama hesabının şifreleme modelini kontrol etmek için [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount) komutunu arayın ve ardından hesap için **KeySource** özelliğini kontrol edin.

```powershell
$account = Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -Name <storage-account>
$account.Encryption.KeySource
```

**KeySource** özelliğinin değeri ise, `Microsoft.Storage`hesap Microsoft tarafından yönetilen anahtarlarla şifrelenir. **KeySource** özelliğinin değeri ise, `Microsoft.Keyvault`hesap müşteri tarafından yönetilen anahtarlarla şifrelenir.

# <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Azure CLI'yi kullanarak depolama hesabının şifreleme modelini denetlemek için [az depolama hesabı göster](/cli/azure/storage/account#az-storage-account-show) komutunu arayın ve ardından hesabın **keySource** özelliğini kontrol edin.

```azurecli-interactive
key_source=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --query encryption.keySource \
    --output tsv)
```

**KeySource** özelliğinin değeri ise, `Microsoft.Storage`hesap Microsoft tarafından yönetilen anahtarlarla şifrelenir. **KeySource** özelliğinin değeri ise, `Microsoft.Keyvault`hesap müşteri tarafından yönetilen anahtarlarla şifrelenir.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Veriler için Azure Depolama şifrelemesi](storage-service-encryption.md)
- [Azure Depolama şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanın](encryption-customer-managed-keys.md)