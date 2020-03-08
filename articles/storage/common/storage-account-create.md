---
title: Depolama hesabı oluşturma
titleSuffix: Azure Storage
description: Azure portal, Azure PowerShell veya Azure CLı kullanarak bir depolama hesabı oluşturmayı öğrenin. Azure depolama hesabı, verilerinize depolama ve erişim için Microsoft Azure benzersiz bir ad alanı sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/07/2020
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: c2d1e8b4975be0657983192df00cc434da00a6f7
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78397195"
---
# <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

Azure depolama hesabı; Bloblar, dosyalar, kuyruklar, tablolar ve diskler içeren tüm Azure depolama veri nesnelerinizi içerir. Depolama hesabı, Azure depolama verileriniz için HTTP veya HTTPS üzerinden dünyanın herhangi bir yerinden erişilebilen benzersiz bir ad alanı sağlar. Azure Depolama hesabınızdaki veriler dayanıklı ve yüksek oranda kullanılabilir, güvenli ve yüksek düzeyde ölçeklenebilir.

Bu nasıl yapılır makalesinde [Azure Portal](https://portal.azure.com/), [Azure POWERSHELL](https://docs.microsoft.com/powershell/azure/overview), [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)veya [Azure Resource Manager şablonunu](../../azure-resource-manager/management/overview.md)kullanarak bir depolama hesabı oluşturmayı öğreneceksiniz.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Yok.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell ile bir Azure depolama hesabı oluşturmak için az 0,7 veya sonraki bir sürümü Azure PowerShell modülünü yüklediğinizden emin olun. Daha fazla bilgi için [Azure PowerShell az Module bölümüne giriş](/powershell/azure/new-azureps-module-az)konusuna bakın.

Geçerli sürümünüzü bulmak için aşağıdaki komutu çalıştırın:

```powershell
Get-InstalledModule -Name "Az"
```

Azure PowerShell yüklemek veya yükseltmek için bkz. [ınstall Azure PowerShell Module](/powershell/azure/install-Az-ps).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure 'da oturum açabilir ve Azure CLı komutlarını iki şekilde çalıştırabilirsiniz:

- CLı komutlarını Azure Cloud Shell Azure portal içinden çalıştırabilirsiniz.
- CLı 'yı yükleyebilir ve CLı komutlarını yerel olarak çalıştırabilirsiniz.

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLı, önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış. Azure portal sağ üst kısmındaki menüdeki **Cloud Shell** düğmesine tıklayın:

[![Cloud Shell](./media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

Düğme, bu nasıl yapılır makalesinde özetlenen adımları çalıştırmak için kullanabileceğiniz etkileşimli bir kabuk başlatır:

[![Portaldaki Cloud Shell penceresini gösteren ekran görüntüsü](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

Ayrıca, Azure CLI’yi yerel olarak yükleyip kullanabilirsiniz. Bu nasıl yapılır makalesi, Azure CLı sürüm 2.0.4 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

# <a name="template"></a>[Şablon](#tab/template)

Yok.

---

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Azure Portal](https://portal.azure.com) oturum açın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

`Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve kimlik doğrulaması yapmak için ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure Cloud Shell başlatmak için [Azure Portal](https://portal.azure.com)oturum açın.

CLı 'nın yerel yüklemesinde oturum açmak için [az Login](/cli/azure/reference-index#az-login) komutunu çalıştırın:

```azurecli-interactive
az login
```

# <a name="template"></a>[Şablon](#tab/template)

Yok

---

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Artık bir depolama hesabı oluşturmaya hazırsınız.

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır. Bu makalede, yeni bir kaynak grubu oluşturma gösterilmektedir.

**Genel amaçlı v2** depolama hesabı, tüm Azure Depolama hizmetlerine erişim sağlar: blob'lar, dosyalar, kuyruklar, tablolar ve diskler. Burada özetlenen adımlarda genel amaçlı v2 depolama hesabı oluşturulur, ancak herhangi bir tür depolama hesabı oluşturma adımları benzerdir.

# <a name="portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

İlk olarak, [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak PowerShell ile yeni bir kaynak grubu oluşturun:

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hard-coding it repeatedly
$resourceGroup = "storage-resource-group"
$location = "westus"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

`-Location` parametresi için hangi bölgeyi belirtlediğinizden emin değilseniz, [Get-AzLocation](/powershell/module/az.resources/get-azlocation) komutuyla aboneliğiniz için desteklenen bölgelerin bir listesini alabilirsiniz:

```powershell
Get-AzLocation | select Location
```

Ardından, [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) komutunu kullanarak Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) ile genel amaçlı v2 depolama hesabı oluşturun. Depolama hesabınızın adının Azure genelinde benzersiz olması gerektiğini unutmayın, bu nedenle yer tutucu değerini köşeli ayraç içinde kendi benzersiz bir değere değiştirin:

```powershell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name <account-name> `
  -Location $location `
  -SkuName Standard_RAGRS `
  -Kind StorageV2
```

> [!IMPORTANT]
> [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmayı planlıyorsanız, bu parametre listesine `-EnableHierarchicalNamespace $True` ekleyin.

Farklı bir çoğaltma seçeneği ile genel amaçlı v2 depolama hesabı oluşturmak için, **Skuname** parametresi için aşağıdaki tabloda istenen değeri değiştirin.

|Çoğaltma seçeneği  |SkuName parametresi  |
|---------|---------|
|Yerel olarak yedekli depolama (LRS)     |Standard_LRS         |
|Alanlar arası yedekli depolama (ZRS)     |Standard_ZRS         |
|Coğrafi olarak yedekli depolama (GRS)     |Standard_GRS         |
|Okuma erişimli coğrafi olarak yedekli depolama (GRS)     |Standard_RAGRS         |
|Coğrafi bölge yedekli depolama (GZRS) (Önizleme)    |Standard_GZRS         |
|Okuma Erişimli Coğrafi bölge yedekli depolama (RA-GZRS) (Önizleme)    |Standard_RAGZRS         |

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

Daha sonra, [az Storage Account Create](/cli/azure/storage/account#az_storage_account_create) komutunu kullanarak Okuma Erişimli Coğrafi olarak yedekli depolama ile genel amaçlı v2 depolama hesabı oluşturun. Depolama hesabınızın adının Azure genelinde benzersiz olması gerektiğini unutmayın, bu nedenle yer tutucu değerini köşeli ayraç içinde kendi benzersiz bir değere değiştirin:

```azurecli-interactive
az storage account create \
    --name <account-name> \
    --resource-group storage-resource-group \
    --location westus \
    --sku Standard_RAGRS \
    --kind StorageV2
```

> [!IMPORTANT]
> [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmayı planlıyorsanız, bu parametre listesine `--enable-hierarchical-namespace true` ekleyin. 

Farklı bir çoğaltma seçeneği ile genel amaçlı v2 depolama hesabı oluşturmak için, **SKU** parametresi için aşağıdaki tabloda istenen değeri değiştirin.

|Çoğaltma seçeneği  |sku parametresi  |
|---------|---------|
|Yerel olarak yedekli depolama (LRS)     |Standard_LRS         |
|Alanlar arası yedekli depolama (ZRS)     |Standard_ZRS         |
|Coğrafi olarak yedekli depolama (GRS)     |Standard_GRS         |
|Okuma erişimli coğrafi olarak yedekli depolama (GRS)     |Standard_RAGRS         |
|Coğrafi bölge yedekli depolama (GZRS) (Önizleme)    |Standard_GZRS         |
|Okuma Erişimli Coğrafi bölge yedekli depolama (RA-GZRS) (Önizleme)    |Standard_RAGZRS         |

# <a name="template"></a>[Şablon](#tab/template)

Bir depolama hesabı oluşturmak üzere bir Kaynak Yöneticisi şablonu dağıtmak için Azure PowerShell veya Azure CLı kullanabilirsiniz. Bu nasıl yapılır makalesinde kullanılan şablon [Azure Resource Manager hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Betikleri çalıştırmak için, Azure Cloud Shell 'i açmak üzere **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin.

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
> Bu şablon yalnızca örnek olarak işlev görür. Bu şablonun bir parçası olarak yapılandırılmayan çok sayıda depolama hesabı ayarı var. Örneğin, [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmak istiyorsanız, `StorageAccountPropertiesCreateParameters` nesnesinin `isHnsEnabledad` özelliğini `true`olarak ayarlayarak bu şablonu değiştirirsiniz. 

Bu şablonu değiştirme veya yenilerini oluşturma hakkında bilgi edinmek için bkz.:

- [Azure Resource Manager belgeleri](/azure/azure-resource-manager/).
- [Depolama hesabı şablon başvurusu](/azure/templates/microsoft.storage/allversions).
- [Ek depolama hesabı şablonu örnekleri](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Storage).

---

Kullanılabilir çoğaltma seçenekleri hakkında daha fazla bilgi için bkz. [Depolama çoğaltma seçenekleri](storage-redundancy.md).

## <a name="delete-a-storage-account"></a>Bir depolama hesabını silme

Bir depolama hesabını silmek, hesaptaki tüm veriler de dahil olmak üzere tüm hesabı siler ve geri alınamaz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com)depolama hesabına gidin.
1. **Sil**'e tıklayın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Depolama hesabını silmek için [Remove-AzStorageAccount](/powershell/module/az.storage/remove-azstorageaccount) komutunu kullanın:

```powershell
Remove-AzStorageAccount -Name <storage-account> -ResourceGroupName <resource-group>
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Depolama hesabını silmek için [az Storage Account Delete](/cli/azure/storage/account#az-storage-account-delete) komutunu kullanın:

```azurecli-interactive
az storage account delete --name <storage-account> --resource-group <resource-group>
```

# <a name="template"></a>[Şablon](#tab/template)

Depolama hesabını silmek için Azure PowerShell ya da Azure CLı kullanın.

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

Alternatif olarak, depolama hesabını ve bu kaynak grubundaki diğer kaynakları silen kaynak grubunu silebilirsiniz. Kaynak grubunu silme hakkında daha fazla bilgi için bkz. [kaynak grubunu ve kaynakları silme](../../azure-resource-manager/management/delete-resource-group.md).

> [!WARNING]
> Silinen depolama hesabını geri yüklemek veya silme işlemi öncesinde içinde yer alan içerikleri almak mümkün değildir. Hesabı silmeden önce kaydetmek istediğiniz şeyleri yedeklediğinizden emin olun. Bu ayrıca hesaptaki tüm kaynaklar için geçerlidir; bir blob, tablo, kuyruk veya dosya sildiğinizde bu işlem kalıcı olarak gerçekleştirilir.
>
> Bir Azure sanal makinesiyle ilişkili bir depolama hesabını silmeye çalışırsanız, depolama hesabının kullanımda olduğu hakkında bir hata alabilirsiniz. Bu hatayla ilgili sorun giderme konusunda yardım için bkz. [depolama hesaplarını silerken hata giderme](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde, genel amaçlı v2 standart depolama hesabı oluşturdunuz. Depolama hesabınıza blob 'ları yükleme ve indirme hakkında bilgi edinmek için, blob Storage hızlı başlangıçlarından birine devam edin.

# <a name="portal"></a>[Portal](#tab/azure-portal)

> [!div class="nextstepaction"]
> [Azure portalını kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

> [!div class="nextstepaction"]
> [PowerShell kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [Azure CLı kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-cli.md)

# <a name="template"></a>[Şablon](#tab/template)

> [!div class="nextstepaction"]
> [Azure portalını kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-portal.md)

---
