---
title: Kaynak Yöneticisi şablonlarla Azure Cosmos DB oluşturma ve yönetme
description: SQL (Core) API için Azure Cosmos DB oluşturmak ve yapılandırmak üzere Azure Resource Manager şablonları kullanma
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: 72a87c3b23e0eed6cfbf1614388702443f4e99d0
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78388007"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarıyla Azure Cosmos DB SQL (çekirdek) API kaynaklarını yönetme

Bu makalede Azure Cosmos DB hesaplarınızın, veritabanlarınızın ve kapsayıcılarınızın yönetimini otomatikleştirmeye yardımcı olmak için Azure Resource Manager şablonlarının nasıl kullanılacağını öğreneceksiniz.

Bu makalede yalnızca SQL API hesapları için Azure Resource Manager şablon örnekleri gösterilmektedir. [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md)ve [tablo](manage-table-with-resource-manager.md) API 'leri için şablon örnekleri de bulabilirsiniz.

<a id="create-resource"></a>

## <a name="create-an-azure-cosmos-account-database-and-container"></a>Azure Cosmos hesabı, veritabanı ve kapsayıcısı oluşturma

Aşağıdaki Azure Resource Manager şablonu ile bir Azure Cosmos hesabı oluşturur:

* 400 (RU/sn) aktarım hızını veritabanı düzeyinde paylaşan iki kapsayıcı.
* Adanmış 400 RU/sn aktarım hızı içeren bir kapsayıcı.

Azure Cosmos DB kaynaklarını oluşturmak için aşağıdaki örnek şablonu kopyalayın ve [PowerShell](#deploy-via-powershell) veya [Azure CLI](#deploy-via-azure-cli)aracılığıyla açıklandığı şekilde dağıtın.

* İsteğe bağlı olarak, [Azure hızlı başlangıç Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) ' ni ziyaret edebilir ve şablonu Azure Portal dağıtabilirsiniz.
* Ayrıca, şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturup `--template-file` parametresiyle yerel yolu belirtebilirsiniz.

> [!IMPORTANT]
>
> * Azure Cosmos hesabına konum eklediğinizde veya kaldırdığınızda, diğer özellikleri aynı anda değiştiremezsiniz. Bu işlemlerin ayrı olarak yapılması gerekir.
> * Hesap adları, tümü küçük harfle 44 karakter ile sınırlıdır.
> * Verimlilik değerlerini değiştirmek için, şablonu güncelleştirilmiş RU/s ile yeniden gönderin.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json":::

> [!NOTE]
> Büyük bölüm anahtarı olan bir kapsayıcı oluşturmak için, önceki şablonu `partitionKey` nesnesine `"version":2` özelliğini içerecek şekilde değiştirin.

### <a name="deploy-via-powershell"></a>PowerShell aracılığıyla dağıtma

Azure Resource Manager şablonunu dağıtmak için PowerShell 'i kullanmak için:

1. Betiği **kopyalayın** .
2. Azure Cloud Shell açmak için **deneyin** ' i seçin.
3. Azure Cloud Shell penceresine sağ tıklayıp **Yapıştır**' ı seçin.

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$sharedThroughput = Read-Host -Prompt "Enter the shared database throughput (i.e. 400)"
$sharedContainer1Name = Read-Host -Prompt "Enter the first shared container name"
$sharedContainer2Name = Read-Host -Prompt "Enter the second shared container name"
$dedicatedContainer1Name = Read-Host -Prompt "Enter the dedicated container name"
$dedicatedThroughput = Read-Host -Prompt "Enter the dedicated container throughput (i.e. 400)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -sharedThroughput $ $sharedThroughput `
    -sharedContainer1Name $sharedContainer1Name `
    -sharedContainer2Name $sharedContainer2Name `
    -dedicatedContainer1Name $dedicatedContainer1Name `
    -dedicatedThroughput $dedicatedThroughput

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Şablonu, Azure Cloud Shell yerine yerel olarak yüklü bir PowerShell sürümü ile dağıtmayı tercih edebilirsiniz. [Azure PowerShell modülünü yüklemeniz](/powershell/azure/install-az-ps)gerekir. Gerekli sürümü bulmak için `Get-Module -ListAvailable Az` çalıştırın.

### <a name="deploy-via-azure-cli"></a>Azure CLı aracılığıyla dağıtma

Azure Resource Manager şablonunu dağıtmak için Azure CLı 'yi kullanmak için:

1. Betiği **kopyalayın** .
2. Azure Cloud Shell açmak için **deneyin** ' i seçin.
3. Azure Cloud Shell penceresine sağ tıklayıp **Yapıştır**' ı seçin.

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the shared database throughput: sharedThroughput
read -p 'Enter the first shared container name: ' sharedContainer1Name
read -p 'Enter the second shared container name: ' sharedContainer2Name
read -p 'Enter the dedicated container name: ' dedicatedContainer1Name
read -p 'Enter the dedicated container throughput: dedicatedThroughput

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName \
   primaryRegion=$primaryRegion \
   secondaryRegion=$secondaryRegion \
   databaseName=$databaseName \
   sharedThroughput=$sharedThroughput \
   sharedContainer1Name=$sharedContainer1Name \
   sharedContainer2Name=$sharedContainer2Name \
   dedicatedContainer1Name=$dedicatedContainer1Name \
   dedicatedThroughput=$dedicatedThroughput

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

`az cosmosdb show` komutu, yeni oluşturulan Azure Cosmos hesabını sağlandıktan sonra gösterir. Şablonu, Azure Cloud Shell bunun yerine yerel olarak yüklü bir Azure CLı sürümü ile dağıtmayı seçebilirsiniz. Daha fazla bilgi için bkz. [Azure komut satırı arabirimi (CLI)](/cli/azure/) makalesi.

<a id="create-sproc"></a>

## <a name="create-an-azure-cosmos-db-container-with-server-side-functionality"></a>Sunucu tarafı işlevleriyle Azure Cosmos DB kapsayıcısı oluşturma

Saklı yordam, tetikleyici ve Kullanıcı tanımlı işlev içeren bir Azure Cosmos DB kapsayıcısı oluşturmak için Azure Resource Manager şablonu kullanabilirsiniz.

Aşağıdaki örnek şablonu kopyalayın ve [PowerShell](#deploy-with-powershell) veya [Azure CLI](#deploy-with-azure-cli)ile açıklandığı şekilde dağıtın.

* İsteğe bağlı olarak, [Azure hızlı başlangıç Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-sprocs/) ' ni ziyaret edebilir ve şablonu Azure Portal dağıtabilirsiniz.
* Ayrıca, şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturup `--template-file` parametresiyle yerel yolu belirtebilirsiniz.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql-container-sprocs/azuredeploy.json":::

### <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

Azure Resource Manager şablonunu dağıtmak için PowerShell 'i kullanmak için:

1. Betiği **kopyalayın** .
1. Azure Cloud Shell açmak için **deneyin** ' i seçin.
1. Azure Cloud Shell penceresine sağ tıklayıp **Yapıştır**' ı seçin.

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -containerName $containerName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Şablonu, Azure Cloud Shell yerine yerel olarak yüklü bir PowerShell sürümü ile dağıtmayı tercih edebilirsiniz. [Azure PowerShell modülünü yüklemeniz](/powershell/azure/install-az-ps)gerekir. Gerekli sürümü bulmak için `Get-Module -ListAvailable Az` çalıştırın.

### <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

Azure Resource Manager şablonunu dağıtmak için Azure CLı 'yi kullanmak için:

1. Betiği **kopyalayın** .
2. Azure Cloud Shell açmak için **deneyin** ' i seçin.
3. Azure Cloud Shell penceresine sağ tıklayıp **Yapıştır**' ı seçin.

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   containerName=$containerName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

## <a name="next-steps"></a>Sonraki adımlar

Bazı ek kaynaklar aşağıda verilmiştir:

* [Azure Resource Manager belgeleri](/azure/azure-resource-manager/)
* [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
* [Azure Cosmos DB hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Documentdb&pageNumber=1&sort=Popular)
* [Ortak Azure Resource Manager Dağıtım hatalarını giderme](../azure-resource-manager/templates/common-deployment-errors.md)
