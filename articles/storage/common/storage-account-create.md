---
title: Depolama hesabı oluşturma
titleSuffix: Azure Storage
description: Bloblar, dosyalar, kuyruklar ve tablolar depolamak için bir depolama hesabı oluşturmayı öğrenin. Azure depolama hesabı, verilerinizi okumak ve yazmak için Microsoft Azure benzersiz bir ad alanı sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/24/2020
ms.author: tamram
ms.subservice: common
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: b7faec5023abe37802d8b08980f216934d70a6ac
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92784363"
---
# <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Azure depolama hesabı; Bloblar, dosyalar, kuyruklar, tablolar ve diskler içeren tüm Azure depolama veri nesnelerinizi içerir. Depolama hesabı, Azure depolama verileriniz için HTTP veya HTTPS üzerinden dünyanın herhangi bir yerinden erişilebilen benzersiz bir ad alanı sağlar. Azure Depolama hesabınızdaki veriler dayanıklı ve yüksek oranda kullanılabilir, güvenli ve yüksek düzeyde ölçeklenebilir.

Bu nasıl yapılır makalesinde [Azure Portal](https://portal.azure.com/), [Azure POWERSHELL](/powershell/azure/), [Azure CLI](/cli/azure)veya [Azure Resource Manager şablonunu](../../azure-resource-manager/management/overview.md)kullanarak bir depolama hesabı oluşturmayı öğreneceksiniz.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

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

[![Portalda Cloud Shell penceresini gösteren ekran görüntüsü](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

Ayrıca, Azure CLI’yi yerel olarak yükleyip kullanabilirsiniz. Bu makaledeki örneklerde Azure CLı sürüm 2.0.4 veya üzeri gerekir. `az --version`Yüklü sürümünüzü bulmak için ' i çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

# <a name="template"></a>[Şablon](#tab/template)

Yok.

---

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Azure Portal](https://portal.azure.com)’ında oturum açın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Komutuyla Azure aboneliğinizde oturum açın `Connect-AzAccount` ve kimlik doğrulaması yapmak için ekrandaki yönergeleri izleyin.

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

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır. Bu makalede, yeni bir kaynak grubu oluşturma gösterilmektedir.

**Genel amaçlı v2** depolama hesabı, tüm Azure Depolama hizmetlerine erişim sağlar: blob'lar, dosyalar, kuyruklar, tablolar ve diskler. Burada özetlenen adımlarda genel amaçlı v2 depolama hesabı oluşturulur, ancak herhangi bir tür depolama hesabı oluşturma adımları benzerdir. Depolama hesaplarının türleri ve diğer depolama hesabı ayarları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](storage-account-overview.md).

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

Parametresi için hangi bölgeyi `-Location` belirtlediğinizden emin değilseniz, [Get-azlocation](/powershell/module/az.resources/get-azlocation) komutuyla aboneliğiniz için desteklenen bölgelerin bir listesini alabilirsiniz:

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
> [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmayı planlıyorsanız, `-EnableHierarchicalNamespace $True` Bu parametre listesine dahil edin.

Farklı bir çoğaltma seçeneği ile genel amaçlı v2 depolama hesabı oluşturmak için, **Skuname** parametresi için aşağıdaki tabloda istenen değeri değiştirin.

|Çoğaltma seçeneği  |SkuName parametresi  |
|---------|---------|
|Yerel olarak yedekli depolama (LRS)     |Standard_LRS         |
|Alanlar arası yedekli depolama (ZRS)     |Standard_ZRS         |
|Coğrafi olarak yedekli depolama (GRS)     |Standard_GRS         |
|Okuma erişimli coğrafi olarak yedekli depolama (GRS)     |Standard_RAGRS         |
|Coğrafi alanlar arası yedekli depolama (GZRS)    |Standard_GZRS         |
|Okuma erişimli coğrafi alanlar arası yedekli depolama (RA-GZRS)    |Standard_RAGZRS         |

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
> [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmayı planlıyorsanız, `--enable-hierarchical-namespace true` Bu parametre listesine dahil edin.

Farklı bir çoğaltma seçeneği ile genel amaçlı v2 depolama hesabı oluşturmak için, **SKU** parametresi için aşağıdaki tabloda istenen değeri değiştirin.

|Çoğaltma seçeneği  |sku parametresi  |
|---------|---------|
|Yerel olarak yedekli depolama (LRS)     |Standard_LRS         |
|Alanlar arası yedekli depolama (ZRS)     |Standard_ZRS         |
|Coğrafi olarak yedekli depolama (GRS)     |Standard_GRS         |
|Okuma erişimli coğrafi olarak yedekli depolama (GRS)     |Standard_RAGRS         |
|Coğrafi alanlar arası yedekli depolama (GZRS)    |Standard_GZRS         |
|Okuma erişimli coğrafi alanlar arası yedekli depolama (RA-GZRS)    |Standard_RAGZRS         |

# <a name="template"></a>[Şablon](#tab/template)

Bir depolama hesabı oluşturmak üzere bir Kaynak Yöneticisi şablonu dağıtmak için Azure PowerShell veya Azure CLı kullanabilirsiniz. Bu nasıl yapılır makalesinde kullanılan şablon [Azure Resource Manager hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Betikleri çalıştırmak için **dene** ' yi seçerek Azure Cloud Shell açın. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır** ' ı seçin.

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
> Bu şablon yalnızca örnek olarak işlev görür. Bu şablonun bir parçası olarak yapılandırılmayan çok sayıda depolama hesabı ayarı var. Örneğin, [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)kullanmak istiyorsanız, `isHnsEnabledad` nesnesinin özelliğini olarak ayarlayarak bu şablonu değiştirirsiniz `StorageAccountPropertiesCreateParameters` `true` . 

Bu şablonu değiştirme veya yenilerini oluşturma hakkında bilgi edinmek için bkz.:

- [Azure Resource Manager belgeleri](../../azure-resource-manager/index.yml).
- [Depolama hesabı şablon başvurusu](/azure/templates/microsoft.storage/allversions).
- [Ek depolama hesabı şablonu örnekleri](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Storage).

---

## <a name="delete-a-storage-account"></a>Bir depolama hesabını silme

Bir depolama hesabını silmek, hesaptaki tüm veriler de dahil olmak üzere tüm hesabı siler ve geri alınamaz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com)depolama hesabına gidin.
1. **Sil** 'e tıklayın.

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
> Bir Azure sanal makinesiyle ilişkili bir depolama hesabını silmeye çalışırsanız, depolama hesabının kullanımda olduğu hakkında bir hata alabilirsiniz. Bu hatayla ilgili sorun giderme konusunda yardım için bkz. [depolama hesaplarını silerken hata giderme](../../virtual-machines/troubleshooting/index.yml).

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabına genel bakış](storage-account-overview.md)
- [Genel amaçlı v2 depolama hesabına yükseltme](storage-account-upgrade.md)
- [Bir Azure Depolama hesabını başka bir bölgeye taşıma](storage-account-move.md)