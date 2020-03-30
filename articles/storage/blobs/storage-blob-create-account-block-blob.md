---
title: Blok blob depolama hesabı oluşturma - Azure Depolama | Microsoft Dokümanlar
description: Üstün performans özelliklerine sahip bir Azure BlockBlobStorage hesabının nasıl oluşturulurolduğunu gösterir.
author: tamram
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 6303644ada5c6f093611dba94daf8006f8cc5819
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536913"
---
# <a name="create-a-blockblobstorage-account"></a>BlockBlobStorage hesabı oluşturma

BlockBlobStorage hesap türü, üstün performans özelliklerine sahip blok lekeleri oluşturmanıza olanak tanır. Bu tür depolama hesabı, yüksek işlem oranlarına sahip veya çok hızlı erişim süreleri gerektiren iş yükleri için optimize edilebiyi içerir. Bu makalede, Azure portalı, Azure CLI veya Azure PowerShell kullanarak bir BlockBlobStorage hesabınasıl oluşturulacak gösterilmektedir.

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

BlockBlobStorage hesapları hakkında daha fazla bilgi için [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview)bilgisine bakın.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Yok.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Bu nasıl yapılsa makale, Azure PowerShell modülü Az sürüm 1.2.0 veya daha sonra gerektirir. Geçerli sürümünüzü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure'da oturum açabilir ve Azure CLI komutlarını iki şekilde çalıştırabilirsiniz:

- Azure portalı nın içinden CLI komutlarını Azure Bulut Bulutu'nda çalıştırabilirsiniz.
- CLI'yi yükleyebilir ve CLI komutlarını yerel olarak çalıştırabilirsiniz.

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI önceden yüklenmiş ve hesabınızla birlikte kullanmak üzere yapılandırılmıştır. Azure portalının sağ üst bölümündeki menüdeki **Bulut Kabuğu** düğmesini tıklatın:

[![Cloud Shell](../common/media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

Düğme, bu nasıl yapılabilir makalesinde özetlenen adımları çalıştırmak için kullanabileceğiniz etkileşimli bir kabuk başlatır:

[![Portaldaki Bulut Kabuğu penceresini gösteren ekran görüntüsü](../common/media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

Ayrıca, Azure CLI’yi yerel olarak yükleyip kullanabilirsiniz. Bu nasıl yapılır makalesi, Azure CLI sürümünü 2.0.46 veya daha sonra çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

---

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Azure portalında](https://portal.azure.com)oturum açın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

`Connect-AzAccount` Komutla Azure aboneliğinizde oturum açın ve kimlik doğrulaması için ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure Bulut Shell'i başlatmak için [Azure portalında](https://portal.azure.com)oturum açın.

CLI'nin yerel yüklemesine giriş yapmak için [az giriş](/cli/azure/reference-index#az-login) komutunu çalıştırın:

```azurecli
az login
```

---

## <a name="create-a-blockblobstorage-account"></a>BlockBlobStorage hesabı oluşturma

## <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalında bir BlockBlobStorage hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında, **Depolama** kategorisi > **Depolama hesapları**> Tüm **hizmetleri** seçin.

1. **Depolama hesapları**altında **Ekle'yi**seçin.

1. **Abonelik** alanında, depolama hesabı oluşturmak için aboneliği seçin.

1. Kaynak **grubu** alanında, varolan bir kaynak grubu seçin veya **yeni oluştur'u**seçin ve yeni kaynak grubu için bir ad girin.

1. Depolama **hesabı adı** alanına, hesabın adını girin. Aşağıdaki yönergeleri not edin:

   - Bu ad Azure'da benzersiz olmalıdır.
   - Ad üç ile 24 karakter uzunluğunda olmalıdır.
   - Ad yalnızca sayılar ve küçük harfler içerebilir.

1. **Konum** alanında, depolama hesabı için bir konum seçin veya varsayılan konumu kullanın.

1. Ayarların geri kalanı için aşağıdakileri yapılandırın:

   |Alan     |Değer  |
   |---------|---------|
   |**Performans**    |  **Premium'u**seçin.   |
   |**Hesap türü**    | **BlockBlobStorage'ı**seçin.      |
   |**Çoğaltma**    |  **Yerel olarak yedekli depolama alanının (LRS)** varsayılan ayarını bırakın.      |

   ![Bir blok blob depolama hesabı oluşturmak için portal Kullanıcı Arabirimi gösterir](media/storage-blob-create-account-block-blob/create-block-blob-storage-account.png)

1. Depolama hesabı ayarlarını incelemek için **Gözden Geçir + oluştur'u** seçin.

1. **Oluştur'u**seçin.

## <a name="azure-powershell"></a>[Azure Powershell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. Yükseltilmiş bir Windows PowerShell oturumu açın (Yönetici olarak çalıştırın).

1. `Az` PowerShell modülünün en son sürümünün yüklü olduğundan emin olmak için aşağıdaki komutu çalıştırın.

   ```powershell
   Install-Module -Name Az -AllowClobber
   ```

1. Yeni bir PowerShell konsolu açın ve Azure hesabınızla oturum açın.

   ```powershell
   Connect-AzAccount -SubscriptionId <SubscriptionID>
   ```

1. Gerekirse, yeni bir kaynak grubu oluşturun. Tırnak halindeki değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```powershell
   $resourcegroup = "new_resource_group_name"
   $location = "region_name"
   New-AzResourceGroup -Name $resourceGroup -Location $location
   ```

1. BlockBlobStorage hesabını oluşturun. Tırnak halindeki değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```powershell
   $resourcegroup = "resource_group_name"
   $storageaccount = "new_storage_account_name"
   $location = "region_name"

   New-AzStorageAccount -ResourceGroupName $resourcegroup -Name $storageaccount -Location $location -Kind "BlockBlobStorage" -SkuName "Premium_LRS"
   ```

## <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI'yi kullanarak bir blok blob hesabı oluşturmak için öncelikle Azure CLI v'yi yüklemeniz gerekir. 2.0.46 veya daha sonraki bir sürüm. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

1. Azure aboneliğinizde oturum açın.

   ```azurecli
   az login
   ```

1. Gerekirse, yeni bir kaynak grubu oluşturun. Parantez içinde (parantez ler dahil) değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```azurecli
   az group create \
    --name "<new_resource_group_name>" \
    --location "<location>"
   ```

1. BlockBlobStorage hesabını oluşturun. Parantez içinde (parantez ler dahil) değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```azurecli
   az storage account create \
    --location "<location>" \
    --name "<new_storage_account_name>" \
    --resource-group "<resource_group_name>" \
    --kind "BlockBlobStorage" \
    --sku "Premium_LRS"
   ```

---

## <a name="next-steps"></a>Sonraki adımlar

- Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

- Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
