---
title: Blok Blobu depolama hesabı oluşturma-Azure depolama | Microsoft Docs
description: Premium performans özellikleriyle Azure BlockBlobStorage hesabının nasıl oluşturulacağını gösterir.
author: tamram
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 1df1d5180d951e7a720ec82c548438892a47a426
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72881858"
---
# <a name="create-a-blockblobstorage-account"></a>BlockBlobStorage hesabı oluşturma

BlockBlobStorage hesap türü, Premium performans özellikleriyle blok Blobları oluşturmanızı sağlar. Bu depolama hesabı türü, yüksek işlem ücretleri olan veya çok hızlı erişim süreleri gerektiren iş yükleri için iyileştirilmiştir. Bu makalede Azure portal, Azure CLı veya Azure PowerShell kullanarak bir blok Blobstorage hesabının nasıl oluşturulacağı gösterilmektedir.

BlockBlobStorage hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

## <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Azure portal bir blok Blobstorage hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portal **depolama** kategorisi > depolama **hesapları**> **tüm hizmetler** ' i seçin.

1. **Depolama hesapları**altında **Ekle**' yi seçin.

1. **Abonelik** alanında, depolama hesabının oluşturulacağı aboneliği seçin.

1. **Kaynak grubu** alanında, var olan bir kaynak grubunu seçin veya **Yeni oluştur**' u seçin ve yeni kaynak grubu için bir ad girin.

1. **Depolama hesabı adı** alanına hesap için bir ad girin. Aşağıdaki yönergeleri dikkate alın:

   - AD Azure genelinde benzersiz olmalıdır.
   - Ad, üç ve 24 karakter uzunluğunda olmalıdır.
   - Ad yalnızca rakamlar ve küçük harfler içerebilir.

1. **Konum** alanında, depolama hesabı için bir konum seçin veya varsayılan konumu kullanın.

1. Ayarların geri kalanı için aşağıdakileri yapılandırın:

   |Alan     |Değer  |
   |---------|---------|
   |**Performans**    |  **Premium**' u seçin.   |
   |**Hesap türü**    | **Blockblobstorage**' ı seçin.      |
   |**Çoğaltma**    |  **Yerel olarak yedekli depolama (LRS)** varsayılan ayarını bırakın.      |

   ![Bir Blok Blobu depolama hesabı oluşturmak için Portal Kullanıcı arabirimini gösterir](media/storage-blob-create-account-block-blob/create-block-blob-storage-account.png)

1. Depolama hesabı ayarlarını gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

1. **Oluştur**'u seçin.

## <a name="azure-powershelltabazure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. Yükseltilmiş bir Windows PowerShell oturumu açın (yönetici olarak çalıştır).

1. `Az` PowerShell modülünün en son sürümünün yüklü olduğundan emin olmak için aşağıdaki komutu çalıştırın.

   ```powershell
   Install-Module -Name Az -AllowClobber
   ```

1. Yeni bir PowerShell konsolu açın ve Azure hesabınızla oturum açın.

   ```powershell
   Connect-AzAccount -SubscriptionId <SubscriptionID>
   ```

1. Gerekirse, yeni bir kaynak grubu oluşturun. Tekliflerdeki değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```powershell
   $resourcegroup = "new_resource_group_name"
   $location = "region_name"
   New-AzResourceGroup -Name $resourceGroup -Location $location
   ```

1. BlockBlobStorage hesabını oluşturun. Tekliflerdeki değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```powershell
   $resourcegroup = "resource_group_name"
   $storageaccount = "new_storage_account_name"
   $location = "region_name"

   New-AzStorageAccount -ResourceGroupName $resourcegroup -Name $storageaccount -Location $location -Kind "BlockBlobStorage" -SkuName "Premium_LRS"
   ```

## <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak bir Blok Blobu hesabı oluşturmak için önce Azure CLı v 'yi yüklemeniz gerekir. 2.0.46 veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli).

1. Azure aboneliğinizde oturum açın.

   ```azurecli
   az login
   ```

1. Gerekirse, yeni bir kaynak grubu oluşturun. Köşeli ayraçlar içindeki değerleri (köşeli ayraç dahil) değiştirin ve aşağıdaki komutu çalıştırın.

   ```azurecli
   az group create \
    --name "<new_resource_group_name>" \
    --location "<location>"
   ```

1. BlockBlobStorage hesabını oluşturun. Köşeli ayraçlar içindeki değerleri (köşeli ayraç dahil) değiştirin ve aşağıdaki komutu çalıştırın.

   ```azurecli
   az storage account create \
    --location "<location>" \
    --name "<new_storage_account_name>" \
    --resource-group "<resource_group_name>" \
    --kind "BlockBlobStorage" \
    --sku "Premium_LRS"
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

- Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
