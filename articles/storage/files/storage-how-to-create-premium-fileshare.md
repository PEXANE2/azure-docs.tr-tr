---
title: Premium Azure dosya paylaşımı oluşturma
description: Bu makalede, premium Azure dosya paylaşımını nasıl oluşturabileceğinizi öğrenirsiniz.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 05/05/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 7680a28b165dc252159cf95311439508d3c867e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529116"
---
# <a name="how-to-create-an-premium-azure-file-share"></a>Premium Azure dosya paylaşımı nasıl oluşturulur?
Premium dosya paylaşımları katı hal diski (SSD) depolama medyasında sunulur ve veritabanlarının barındırışlanması ve yüksek performanslı bilgi işlem (HPC) dahil olmak üzere IO yoğun iş yükleri için yararlıdır. Premium dosya paylaşımları, FileStorage hesabı adı verilen özel amaçlı bir depolama hesabı türünde barındırılır. Premium dosya paylaşımları, tutarlı düşük gecikme süresi, yüksek IOPS ve yüksek iş payı sağlayan yüksek performanslı ve kurumsal ölçekli uygulamalar için tasarlanmıştır.

Bu makalede, [Azure portalı](https://portal.azure.com/), Azure PowerShell ve Azure CLI kullanarak bu yeni hesap türünü nasıl oluşturabileceğiniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Premium Azure dosya paylaşımları da dahil olmak üzere Azure kaynaklarına erişmek için bir Azure aboneliğine ihtiyacınız olacak. Zaten bir aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-premium-file-share-using-the-azure-portal"></a>Azure portalını kullanarak premium dosya paylaşımı oluşturma

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com/)oturum açın.

### <a name="create-a-filestorage-storage-account"></a>Dosya depolama depolama hesabı oluşturma

Artık depolama hesabınızı oluşturmaya hazırsınız.

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır. Bu makalede, yeni bir kaynak grubu oluşturmak için nasıl gösterir.

1. Azure portalında, sol menüde **Depolama Hesapları'nı** seçin.

    ![Azure portalı ana sayfası seçme depolama hesabı](media/storage-how-to-create-premium-fileshare/azure-portal-storage-accounts.png)

1. Açılan **Depolama Hesapları** penceresinde **Ekle**'yi seçin.
1. Depolama hesabının oluşturulacağı aboneliği seçin.
1. **Kaynak grubu** alanı altında **Yeni oluştur**’u seçin. Aşağıdaki görüntüde gösterildiği gibi yeni kaynak grubunuz için bir ad girin.

1. Ardından, depolama hesabınız için bir ad girin. Seçtiğiniz ad Azure genelinde benzersiz olmalıdır. Ad ayrıca 3 - 24 karakter uzunluğunda olmalıdır ve yalnızca rakam ve küçük harf içerebilir.
1. Depolama hesabınız için bir konum seçin veya varsayılan konumu kullanın.
1. **Performans** için **Premium**seçin.
1. **Hesap türünü** seçin ve **FileStorage'ı**seçin.
1. **Çoğaltma** kümesini Yerel olarak **yedekli depolama (LRS)** varsayılan değerine bırakın.

    ![Premium dosya paylaşımı için depolama hesabı oluşturma](media/storage-how-to-create-premium-fileshare/create-filestorage-account.png)

1. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’u seçin.
1. **Oluştur'u**seçin.

Depolama hesabı kaynağınız oluşturulduktan sonra, bu kaynak için gidin.

### <a name="create-a-premium-file-share"></a>Premium dosya paylaşımı oluşturma

1. Depolama hesabının sol menüsünde Dosya **hizmeti** bölümüne gidin ve **ardından Dosyalar'ı**seçin.
1. Premium dosya paylaşımı oluşturmak için **Dosya paylaşımı'nı** seçin.
1. Dosya paylaşımınız için bir ad ve istenilen kota girin ve ardından **Oluştur'u**seçin.

> [!NOTE]
> Verilen hisse boyutları hisse kotası ile belirtilir, dosya payları sağlanan boyuta fatura edilir, daha fazla ayrıntı için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/files/) bakın.

   ![Premium dosya paylaşımı oluşturma](media/storage-how-to-create-premium-fileshare/create-premium-file-share.png)

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynakları temizlemek istiyorsanız, kaynak grubunu silmeniz yeterlidir. Kaynak grubunun silinmesi, ilişkili depolama hesabının yanı sıra kaynak grubuyla ilişkili diğer kaynakları da siler.

## <a name="create-a-premium-file-share-using-powershell"></a>PowerShell'i kullanarak premium dosya paylaşımı oluşturma

### <a name="create-an-account-using-powershell"></a>PowerShell kullanarak hesap oluşturma

İlk olarak, [PowerShellGet](/powershell/scripting/gallery/installing-psget) modülünün en son sürümünü yükleyin.

Ardından PowerShell modülünüzü yükseltin, Azure aboneliğinizde oturum açın, bir kaynak grubu oluşturun ve ardından bir depolama hesabı oluşturun.

### <a name="upgrade-your-powershell-module"></a>PowerShell modülünüzü yükseltin

PowerShell ile premium dosya paylaşımıyla etkileşimde bulunmak için bir Az.Storage modülü sürüm 1.4.0 veya en son Az.Storage modüllerini yüklemeniz gerekir.

Yüksek izinlerle bir PowerShell oturumu açarak başlayın.

Az.Storage modüllerini yükleyin:

```powershell
Install-Module Az.Storage -Repository PSGallery -AllowClobber -Force
```

### <a name="sign-in-to-your-azure-subscription"></a>Azure Aboneliğinizde Oturum Açın

Komutu `Connect-AzAccount` kullanın ve kimlik doğrulaması için ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

PowerShell ile yeni bir kaynak grubu oluşturmak için [Yeni Kaynak Grubu](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanın:

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-how-to-resource-group"
$location = "westus2"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-filestorage-storage-account"></a>FileStorage depolama hesabı oluşturma

PowerShell'den bir FileStorage depolama hesabı oluşturmak için [Yeni Depolama Hesabı](/powershell/module/az.storage/New-azStorageAccount) komutunu kullanın:

```powershell
$storageAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name "fileshowto" -SkuName "Premium_LRS" -Location "westus2" -Kind "FileStorage"
```

### <a name="create-a-premium-file-share"></a>Premium dosya paylaşımı oluşturma

Artık bir FileStorage hesabınız olduğuna göre, bir premium dosya paylaşımı oluşturabilirsiniz. Bir tane oluşturmak için [New-AzStorageShare](/powershell/module/az.storage/New-AzStorageShare) cmdlet'i kullanın.

> [!NOTE]
> Verilen hisse boyutları hisse kotası ile belirtilir, dosya payları sağlanan boyuta fatura edilir, daha fazla ayrıntı için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/files/) bakın.

```powershell
New-AzStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu ve yeni depolama hesabı da dahil olmak üzere ilişkili kaynaklarını kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanın: 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="create-a-premium-file-share-using-azure-cli"></a>Azure CLI'yi kullanarak premium dosya paylaşımı oluşturma

Azure Bulut Kabuğu'nu başlatmak için [Azure portalında](https://portal.azure.com)oturum açın.

CLI'nin yerel yüklemesine giriş yapmak istiyorsanız, önce en son sürüme sahip olduğunuzdan emin olun, ardından giriş komutunu çalıştırın:

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

Azure CLI'den bir FileStorage depolama hesabı oluşturmak için [az depolama hesabı oluşturma](/cli/azure/storage/account) komutunu kullanın.

```azurecli-interactive
az storage account create `
    --name fileshowto `
    --resource-group files-howto-resource-group `
    --location westus `
    --sku Premium_LRS `
    --kind FileStorage
```

### <a name="get-the-storage-account-key"></a>Depolama hesabı anahtarını alma

Depolama hesabı anahtarları bir depolama hesabındaki kaynaklara erişimi kontrol ediyor, bu makalede, premium dosya paylaşımı oluşturmak için anahtarı kullanıyoruz. Bu anahtarlar, depolama hesabını oluşturduğunuzda otomatik olarak oluşturulur. [az storage account keys list](/cli/azure/storage/account/keys) komutunu kullanarak depolama hesabınızın depolama hesabı anahtarlarını alabilirsiniz:

```azurecli-interactive
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

### <a name="create-a-premium-file-share"></a>Premium dosya paylaşımı oluşturma

Artık bir dosya depolama hesabınız olduğuna göre, bir premium dosya paylaşımı oluşturabilirsiniz. Oluşturmak için [az depolama payı oluşturma](/cli/azure/storage/share) komutunu kullanın.

> [!NOTE]
> Verilen hisse boyutları hisse kotası ile belirtilir, dosya payları sağlanan boyuta fatura edilir, daha fazla ayrıntı için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/files/) bakın.

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için [az group delete](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir premium dosya paylaşımı oluşturdunuz. Bu hesabın sunduğu performans hakkında bilgi edinmek için planlama kılavuzunun performans katmanı bölümüne devam edin.

> [!div class="nextstepaction"]
> [Dosya paylaşım katmanları](storage-files-planning.md#storage-tiers)