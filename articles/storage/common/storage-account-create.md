---
title: Depolama hesabı oluşturma
titleSuffix: Azure Storage
description: Azure portalını, Azure PowerShell'i veya Azure CLI'yi kullanarak bir depolama hesabı oluşturmayı öğrenin. Azure depolama hesabı, verilerinizi depolamak ve erişmek için Microsoft Azure'da benzersiz bir ad alanı sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/07/2020
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: c2d1e8b4975be0657983192df00cc434da00a6f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255411"
---
# <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Azure depolama hesabı, tüm Azure Depolama veri nesnelerinizin tümlerini içerir: lekeler, dosyalar, kuyruklar, tablolar ve diskler. Depolama hesabı, Azure Depolama verileriniz için dünyanın herhangi bir yerinden HTTP veya HTTPS üzerinden erişilebilen benzersiz bir ad alanı sağlar. Azure depolama hesabınızdaki veriler dayanıklıdır ve yüksek oranda kullanılabilir, güvenli ve büyük ölçüde ölçeklenebilir.

Bu nasıl yapılsa makalede, [Azure portalı](https://portal.azure.com/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)veya [Azure Kaynak Yöneticisi şablonu](../../azure-resource-manager/management/overview.md)kullanarak bir depolama hesabı oluşturmayı öğrenirsiniz.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Yok.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell ile bir Azure depolama hesabı oluşturmak için Azure PowerShell modülü Az sürümünü 0,7 veya daha sonra yüklediğinizden emin olun. Daha fazla bilgi için Azure [PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

Geçerli sürümünüzü bulmak için aşağıdaki komutu çalıştırın:

```powershell
Get-InstalledModule -Name "Az"
```

Azure PowerShell'i yüklemek veya yükseltmek için Azure [PowerShell modüllerini yükle'ye](/powershell/azure/install-Az-ps)bakın.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure'da oturum açabilir ve Azure CLI komutlarını iki şekilde çalıştırabilirsiniz:

- Azure portalı nın içinden CLI komutlarını Azure Bulut Bulutu'nda çalıştırabilirsiniz.
- CLI'yi yükleyebilir ve CLI komutlarını yerel olarak çalıştırabilirsiniz.

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI önceden yüklenmiş ve hesabınızla birlikte kullanmak üzere yapılandırılmıştır. Azure portalının sağ üst bölümündeki menüdeki **Bulut Kabuğu** düğmesini tıklatın:

[![Cloud Shell](./media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

Düğme, bu nasıl yapılabilir makalesinde özetlenen adımları çalıştırmak için kullanabileceğiniz etkileşimli bir kabuk başlatır:

[![Portaldaki Bulut Kabuğu penceresini gösteren ekran görüntüsü](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

Ayrıca, Azure CLI’yi yerel olarak yükleyip kullanabilirsiniz. Bu nasıl yapılır makalesi, Azure CLI sürümü 2.0.4 veya sonraki sürümlerini çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

# <a name="template"></a>[Şablon](#tab/template)

Yok.

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

```azurecli-interactive
az login
```

# <a name="template"></a>[Şablon](#tab/template)

Yok

---

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Artık bir depolama hesabı oluşturmaya hazırsınız.

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır. Bu makalede, yeni bir kaynak grubu oluşturmak için nasıl gösterir.

**Genel amaçlı v2** depolama hesabı, tüm Azure Depolama hizmetlerine erişim sağlar: blob'lar, dosyalar, kuyruklar, tablolar ve diskler. Burada özetlenen adımlar genel amaçlı bir v2 depolama hesabı oluşturur, ancak herhangi bir depolama hesabı oluşturma adımları benzerdir.

# <a name="portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

İlk olarak, [Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak PowerShell ile yeni bir kaynak grubu oluşturun:

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hard-coding it repeatedly
$resourceGroup = "storage-resource-group"
$location = "westus"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

`-Location` Parametre için hangi bölgeyi belirtebileceğinizden emin değilseniz, [Get-AzLocation](/powershell/module/az.resources/get-azlocation) komutuyla aboneliğiniz için desteklenen bölgelerin listesini alabilirsiniz:

```powershell
Get-AzLocation | select Location
```

Ardından, [Yeni-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) komutunu kullanarak okuma erişimi coğrafi depolama (RA-GRS) içeren genel amaçlı bir v2 depolama hesabı oluşturun. Depolama hesabınızın adının Azure'da benzersiz olması gerektiğini unutmayın, bu nedenle parantez içinde yer tutucu değerini kendi benzersiz değerinizle değiştirin:

```powershell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name <account-name> `
  -Location $location `
  -SkuName Standard_RAGRS `
  -Kind StorageV2
```

> [!IMPORTANT]
> [Azure Veri Gölü Depolama'yı](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmayı `-EnableHierarchicalNamespace $True` planlıyorsanız, bu parametre listesine ekleyin.

Farklı bir çoğaltma seçeneğine sahip genel amaçlı bir v2 depolama hesabı oluşturmak için, Aşağıdaki tabloda **SkuName** parametresi için istenen değeri değiştirin.

|Çoğaltma seçeneği  |SkuName parametresi  |
|---------|---------|
|Yerel olarak yedekli depolama (LRS)     |Standard_LRS         |
|Alanlar arası yedekli depolama (ZRS)     |Standard_ZRS         |
|Coğrafi olarak yedekli depolama (GRS)     |Standard_GRS         |
|Okuma erişimli coğrafi olarak yedekli depolama (GRS)     |Standard_RAGRS         |
|Coğrafi bölge yedekli depolama (GZRS) (önizleme)    |Standard_GZRS         |
|Okuma-erişim coğrafi bölge yedekli depolama (RA-GZRS) (önizleme)    |Standard_RAGZRS         |

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

İlk olarak, [az group create](/cli/azure/group#az_group_create) komutunu kullanarak Azure CLI ile yeni bir kaynak grubu oluşturun.

```azurecli-interactive
az group create \
    --name storage-resource-group \
    --location westus
```

`--location` parametresi için hangi bölgeyi belirteceğinizden emin değilseniz, [az account list-locations](/cli/azure/account#az_account_list) komutuyla aboneliğiniz için desteklenen bölgelerin bir listesini alabilirsiniz.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Ardından, az depolama hesabı oluşturma komutunu kullanarak okuma erişimi coğrafi yedekli depolama alanına sahip genel amaçlı bir v2 depolama [hesabı oluşturun.](/cli/azure/storage/account#az_storage_account_create) Depolama hesabınızın adının Azure'da benzersiz olması gerektiğini unutmayın, bu nedenle parantez içinde yer tutucu değerini kendi benzersiz değerinizle değiştirin:

```azurecli-interactive
az storage account create \
    --name <account-name> \
    --resource-group storage-resource-group \
    --location westus \
    --sku Standard_RAGRS \
    --kind StorageV2
```

> [!IMPORTANT]
> [Azure Veri Gölü Depolama'yı](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmayı `--enable-hierarchical-namespace true` planlıyorsanız, bu parametre listesine ekleyin. 

Farklı bir çoğaltma seçeneğine sahip genel amaçlı bir v2 depolama hesabı oluşturmak için aşağıdaki tabloda **sku** parametresi için istenen değeri değiştirin.

|Çoğaltma seçeneği  |sku parametresi  |
|---------|---------|
|Yerel olarak yedekli depolama (LRS)     |Standard_LRS         |
|Alanlar arası yedekli depolama (ZRS)     |Standard_ZRS         |
|Coğrafi olarak yedekli depolama (GRS)     |Standard_GRS         |
|Okuma erişimli coğrafi olarak yedekli depolama (GRS)     |Standard_RAGRS         |
|Coğrafi bölge yedekli depolama (GZRS) (önizleme)    |Standard_GZRS         |
|Okuma-erişim coğrafi bölge yedekli depolama (RA-GZRS) (önizleme)    |Standard_RAGZRS         |

# <a name="template"></a>[Şablon](#tab/template)

Bir depolama hesabı oluşturmak için Bir Kaynak Yöneticisi şablonu dağıtmak için Azure Powershell veya Azure CLI'yi kullanabilirsiniz. Bu nasıl yapılır makalesinde kullanılan şablon [Azure Kaynak Yöneticisi hızlı başlatma şablonlarından](https://azure.microsoft.com/resources/templates/101-storage-account-create/)dır. Komut dosyalarını çalıştırmak için Azure Bulutu kabuğunu açmak için **Deneyin'i** seçin. Komut dosyasını yapıştırmak için kabuğu sağ tıklatın ve sonra **Yapıştır'ı**seçin.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-file "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

> [!NOTE]
> Bu şablon yalnızca bir örnek olarak hizmet vermektedir. Bu şablonun bir parçası olarak yapılandırılmamış birçok depolama hesabı ayarı vardır. Örneğin, [Azure Veri Gölü Depolama'sını](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmak istiyorsanız, nesnenin `isHnsEnabledad` özelliğini `StorageAccountPropertiesCreateParameters` `true`. 

Bu şablonu nasıl değiştirip yenilerini oluşturabilirsiniz öğrenmek için bkz:

- [Azure Kaynak Yöneticisi belgeleri](/azure/azure-resource-manager/).
- [Depolama hesabı şablonu başvurusu.](/azure/templates/microsoft.storage/allversions)
- [Ek depolama hesabı şablonu örnekleri.](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Storage)

---

Kullanılabilir çoğaltma seçenekleri hakkında daha fazla bilgi için bkz. [Depolama çoğaltma seçenekleri](storage-redundancy.md).

## <a name="delete-a-storage-account"></a>Bir depolama hesabını silme

Depolama hesabının silinmesi, hesaptaki tüm veriler de dahil olmak üzere tüm hesabı siler ve geri alınamaz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure portalındaki](https://portal.azure.com)depolama hesabına gidin.
1. **Sil'i**tıklatın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Depolama hesabını silmek için [Kaldır-AzStorageAccount](/powershell/module/az.storage/remove-azstorageaccount) komutunu kullanın:

```powershell
Remove-AzStorageAccount -Name <storage-account> -ResourceGroupName <resource-group>
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Depolama hesabını silmek için [az depolama hesabı silme](/cli/azure/storage/account#az-storage-account-delete) komutunu kullanın:

```azurecli-interactive
az storage account delete --name <storage-account> --resource-group <resource-group>
```

# <a name="template"></a>[Şablon](#tab/template)

Depolama hesabını silmek için Azure PowerShell veya Azure CLI'yi kullanın.

```azurepowershell-interactive
$storageResourceGroupName = Read-Host -Prompt "Enter the resource group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"
Remove-AzStorageAccount -Name $storageAccountName -ResourceGroupName $storageResourceGroupName
```

```azurecli-interactive
echo "Enter the resource group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az storage account delete --name storageAccountName --resource-group resourceGroupName
```

---

Alternatif olarak, depolama hesabını ve bu kaynak grubundaki diğer kaynakları silen kaynak grubunu silebilirsiniz. Kaynak grubunu silme hakkında daha fazla bilgi için [kaynak grubunu ve kaynakları sil](../../azure-resource-manager/management/delete-resource-group.md)bölümüne bakın.

> [!WARNING]
> Silinen depolama hesabını geri yüklemek veya silme işlemi öncesinde içinde yer alan içerikleri almak mümkün değildir. Hesabı silmeden önce kaydetmek istediğiniz şeyleri yedeklediğinizden emin olun. Bu ayrıca hesaptaki tüm kaynaklar için geçerlidir; bir blob, tablo, kuyruk veya dosya sildiğinizde bu işlem kalıcı olarak gerçekleştirilir.
>
> Bir Azure sanal makinesiyle ilişkili bir depolama hesabını silmeye çalışırsanız, depolama hesabının kullanımda olduğu hakkında bir hata alabilirsiniz. Bu hatayı giderme de yardım için, [depolama hesaplarını silerken Sorun Giderme hatalarına](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılabilir makalesinde, genel amaçlı bir v2 standart depolama hesabı oluşturdunuz. Depolama hesabınıza blob'ları nasıl yükleyip indirdiğinizi öğrenmek için Blob depolama hızlarından birine devam edin.

# <a name="portal"></a>[Portal](#tab/azure-portal)

> [!div class="nextstepaction"]
> [Azure portalını kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

> [!div class="nextstepaction"]
> [PowerShell kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [Azure CLI'yi kullanarak lekelerle çalışın](../blobs/storage-quickstart-blobs-cli.md)

# <a name="template"></a>[Şablon](#tab/template)

> [!div class="nextstepaction"]
> [Azure portalını kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-portal.md)

---
