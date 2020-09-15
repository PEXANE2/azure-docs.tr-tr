---
title: Premium Azure dosya paylaşma oluşturma
description: Azure portal, Azure PowerShell modülünü veya Azure CLı kullanarak Azure Premium dosya paylaşımının nasıl oluşturulacağını öğrenin.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 08/26/2020
ms.author: rogarana
ms.subservice: files
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 75ba8e1e2037ba8ef249b548dfb38e5fd1618cb2
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90564194"
---
# <a name="how-to-create-an-azure-premium-file-share"></a>Azure Premium dosya paylaşma oluşturma

Premium dosya paylaşımları, katı hal disk (SSD) depolama medyasında sunulur ve barındırma veritabanları ve yüksek performanslı bilgi işlem (HPC) dahil olmak üzere GÇ yoğunluklu iş yükleri için kullanışlıdır. Premium dosya paylaşımları, FileStorage hesabı olarak adlandırılan özel bir amaç depolama hesabı türünde barındırılır. Premium dosya paylaşımları, yüksek performans ve kurumsal ölçekli uygulamalar için tasarlanmıştır; tutarlı düşük gecikme süresi, yüksek ıOPS ve yüksek verimlilik paylaşımları sağlar.

Bu makalede, [Azure Portal](https://portal.azure.com/), Azure PowerShell modülünü ve Azure CLI kullanarak bu yeni hesap türünü nasıl oluşturacağınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- Azure CLı 'yı kullanmayı planlıyorsanız [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
- Azure PowerShell modülünü kullanmayı düşünüyorsanız, [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-4.6.0).

## <a name="create-a-filestorage-storage-account"></a>FileStorage depolama hesabı oluşturma

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır. Premium dosya paylaşımları bir dosya depolama hesabı gerektirir.

# <a name="portal"></a>[Portal](#tab/azure-portal)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com/) oturum açın.

Artık depolama hesabınızı oluşturmaya hazır olursunuz.

1. Azure portal sol taraftaki menüden **depolama hesapları** ' nı seçin.

    ![Ana sayfa Azure portal depolama hesabı seçin](media/storage-how-to-create-premium-fileshare/azure-portal-storage-accounts.png)

1. Açılan **Depolama Hesapları** penceresinde **Ekle**'yi seçin.
1. Depolama hesabının oluşturulacağı aboneliği seçin.
1. **Kaynak grubu** alanı altında **Yeni oluştur**’u seçin. Aşağıdaki görüntüde gösterildiği gibi yeni kaynak grubunuz için bir ad girin.

1. Ardından, depolama hesabınız için bir ad girin. Seçtiğiniz ad Azure genelinde benzersiz olmalıdır. Ad ayrıca 3 - 24 karakter uzunluğunda olmalıdır ve yalnızca rakam ve küçük harf içerebilir.
1. Depolama hesabınız için bir konum seçin veya varsayılan konumu kullanın.
1. **Performans** için **Premium**seçin.

    **Dosya depolama** için **Premium** ' u seçerek **Hesap türü** açılan listesinde kullanılabilir bir seçenek olması gerekir.

1. **Hesap türü** ' nü seçin ve **FileStorage**' ı seçin.
1. **Çoğaltmayı** **yerel olarak yedekli depolama (LRS)** varsayılan değerine ayarlı bırakın.

    ![Premium dosya paylaşımında depolama hesabı oluşturma](media/storage-how-to-create-premium-fileshare/create-filestorage-account.png)

1. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’u seçin.
1. **Oluştur**’u seçin.

Depolama hesabı kaynağınız oluşturulduktan sonra şuraya gidin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Komutunu kullanın `Connect-AzAccount` ve kimlik doğrulaması yapmak için ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

PowerShell ile yeni bir kaynak grubu oluşturmak için [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanın:

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-how-to-resource-group"
$location = "westus2"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-filestorage-storage-account"></a>FileStorage depolama hesabı oluşturma

PowerShell 'den bir dosya depolama depolama hesabı oluşturmak için [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) komutunu kullanın:

```powershell
$storageAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name "fileshowto" -SkuName "Premium_LRS" -Location "westus2" -Kind "FileStorage"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure Cloud Shell başlamak için [Azure Portal](https://portal.azure.com)oturum açın.

CLı 'nın yerel yüklemesinde oturum açmak istiyorsanız en son sürüme sahip olduğunuzdan emin olun ve oturum açın:

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure CLI ile yeni bir kaynak grubu oluşturmak için [az group create](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group create `
    --name files-howto-resource-group `
    --location westus2
```

### <a name="create-a-filestorage-storage-account"></a>FileStorage depolama hesabı oluşturma

Azure CLı 'dan bir FileStorage depolama hesabı oluşturmak için [az Storage Account Create](/cli/azure/storage/account) komutunu kullanın.

```azurecli-interactive
az storage account create `
    --name fileshowto `
    --resource-group files-howto-resource-group `
    --location westus `
    --sku Premium_LRS `
    --kind FileStorage
```

### <a name="get-the-storage-account-key"></a>Depolama hesabı anahtarını alma

Depolama hesabı anahtarları bir depolama hesabındaki kaynaklara erişimi denetler, bu makalede bir Premium dosya paylaşımının oluşturulması için anahtarı kullanırız. Bu anahtarlar, depolama hesabını oluşturduğunuzda otomatik olarak oluşturulur. [az storage account keys list](/cli/azure/storage/account/keys) komutunu kullanarak depolama hesabınızın depolama hesabı anahtarlarını alabilirsiniz:

```azurecli-interactive
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```
---

## <a name="create-a-premium-file-share"></a>Premium dosya paylaşımı oluşturma

Artık bir dosya depolama hesabı oluşturduğunuza göre, bu depolama hesabı içinde bir Premium dosya paylaşma oluşturabilirsiniz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Depolama hesabının sol menüsünde **Dosya hizmeti** bölümüne kaydırın ve ardından **dosyalar**' ı seçin.
1. Premium dosya paylaşma oluşturmak için **dosya paylaşma** ' yı seçin.
1. Dosya paylaşımınız için bir ad ve istediğiniz kotayı girip **Oluştur**' u seçin.

> [!NOTE]
> Sağlanan paylaşım boyutları paylaşım kotası ile belirtilir, dosya paylaşımları sağlanan boyutta faturalandırılır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/).

   ![Premium dosya paylaşımı oluşturma](media/storage-how-to-create-premium-fileshare/create-premium-file-share.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Azure PowerShell modülüyle Premium bir dosya paylaşımı oluşturmak için [New-Azstoragesshare](/powershell/module/az.storage/New-AzStorageShare) cmdlet 'ini kullanın.

> [!NOTE]
> Sağlanan paylaşım boyutları paylaşım kotası ile belirtilir, dosya paylaşımları sağlanan boyutta faturalandırılır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/).

```powershell
New-AzStorageShare `
   -Name myshare `
   -EnabledProtocol SMB `
   -Context $storageAcct.Context
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile Premium dosya paylaşma oluşturmak için [az Storage Share Create](/cli/azure/storage/share) komutunu kullanın.

> [!NOTE]
> Sağlanan paylaşım boyutları paylaşım kotası ile belirtilir, dosya paylaşımları sağlanan boyutta faturalandırılır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/).

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --enabled-protocol SMB \
    --name "myshare" 
```
---

## <a name="clean-up-resources"></a>Kaynakları temizleme

# <a name="portal"></a>[Portal](#tab/azure-portal)

Bu makalede oluşturulan kaynakları temizlemek isterseniz, kaynak grubunu silin. Kaynak grubunun silinmesi, ilişkili depolama hesabının yanı sıra kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Bu makalede oluşturulan kaynakları temizlemek isterseniz, kaynak grubunu silin. Kaynak grubunun silinmesi, ilişkili depolama hesabının yanı sıra kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynaklarını kaldırmak için [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanın: 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu makalede oluşturulan kaynakları temizlemek isterseniz, kaynak grubunu silin. Kaynak grubunun silinmesi, ilişkili depolama hesabının yanı sıra kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için [az group delete](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group delete --name myResourceGroup
```
---

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir Premium dosya paylaşma oluşturdunuz. Bu hesabın sunduğu performans hakkında daha fazla bilgi edinmek için Planlama Kılavuzu 'nun performans katmanı bölümüne ilerleyin.

> [!div class="nextstepaction"]
> [Dosya paylaşma katmanları](storage-files-planning.md#storage-tiers)
