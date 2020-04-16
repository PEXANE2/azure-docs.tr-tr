---
title: Kaynak Yöneticisi şablonları ile Azure Cosmos DB oluşturma ve yönetme
description: SQL (Core) API için Azure Cosmos DB oluşturmak ve yapılandırmak için Azure Kaynak Yöneticisi şablonlarını kullanın
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mjbrown
ms.openlocfilehash: 3514b3e77781010fd56b43229f87854ea2d591e8
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81390900"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-with-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonları ile Azure Cosmos DB SQL (Core) API kaynaklarını yönetme

Bu makalede Azure Cosmos DB hesaplarınızın, veritabanlarınızın ve kapsayıcılarınızın yönetimini otomatikleştirmeye yardımcı olmak için Azure Resource Manager şablonlarının nasıl kullanılacağını öğreneceksiniz.

Bu makalede, yalnızca SQL API hesapları için Azure Kaynak Yöneticisi şablon uyrama örnekleri gösterilmektedir. Ayrıca [Cassandra,](manage-cassandra-with-resource-manager.md) [Gremlin,](manage-gremlin-with-resource-manager.md) [MongoDB](manage-mongodb-with-resource-manager.md)ve [Tablo](manage-table-with-resource-manager.md) API'leri için şablon örnekleri bulabilirsiniz.

<a id="create-resource"></a>

## <a name="create-an-azure-cosmos-account-database-and-container"></a>Azure Cosmos hesabı, veritabanı ve kapsayıcı oluşturma

Aşağıdaki Azure Kaynak Yöneticisi şablonu aşağıdakilere sahip bir Azure Cosmos hesabı oluşturur:

* Veritabanı düzeyinde saniyede 400 İstenen Birim (RU/s) iş verisini paylaşan iki kapsayıcı.
* Özel 400 RU/s iş meskenine sahip bir konteyner.

Azure Cosmos DB kaynaklarını oluşturmak için aşağıdaki örnek şablonu kopyalayın ve açıklandığı gibi [PowerShell](#deploy-via-powershell) veya [Azure CLI](#deploy-via-azure-cli)üzerinden dağıtın.

* İsteğe bağlı olarak, [Azure Quickstart Galerisi'ni](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) ziyaret edebilir ve şablonu Azure portalından dağıtabilirsiniz.
* Ayrıca şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturabilir `--template-file` ve parametreyle yerel yolu belirtebilirsiniz.

> [!IMPORTANT]
>
> * Azure Cosmos hesabına konum eklediğinizde veya kaldırdığınızda, aynı anda diğer özellikleri değiştiremezsiniz. Bu işlemler ayrı olarak yapılmalıdır.
> * Hesap adları 44 karakterle sınırlıdır, hepsi küçük harfle.
> * İş verme değerlerini değiştirmek için, şablonu güncelleştirilmiş RU/s ile yeniden gönderin.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json":::

> [!NOTE]
> Büyük bölme anahtarına sahip bir kapsayıcı oluşturmak için, önceki şablonu nesnenin `"version":2` içindeki özelliği içerecek şekilde değiştirin. `partitionKey`

### <a name="deploy-via-powershell"></a>PowerShell üzerinden dağıt

Azure Kaynak Yöneticisi şablonuna dağıtmak için PowerShell'i kullanmak için:

1. Senaryoyu **kopyala.**
2. Azure Bulut Suünü açmak için **Deneyin'i** seçin.
3. Azure Bulut BulutU penceresinde sağ tıklatın ve ardından **Yapıştır'ı**seçin.

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

Şablonu Azure Bulut BulutU yerine Yerel olarak yüklenen PowerShell sürümüyle dağıtmayı seçebilirsiniz. [Azure PowerShell modüllerini yüklemeniz](/powershell/azure/install-az-ps)gerekir. Gerekli `Get-Module -ListAvailable Az` sürümü bulmak için çalıştırın.

### <a name="deploy-via-azure-cli"></a>Azure CLI ile dağıtma

Azure Kaynak Yöneticisi şablonuna dağıtmak için Azure CLI'yi kullanmak için:

1. Senaryoyu **kopyala.**
2. Azure Bulut Suünü açmak için **Deneyin'i** seçin.
3. Azure Bulut BulutU penceresinde sağ tıklatın ve ardından **Yapıştır'ı**seçin.

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

Komut, `az cosmosdb show` yeni oluşturulan Azure Cosmos hesabını oluşturulduktan sonra gösterir. Şablonu Azure Bulut BulutU yerine Azure CLI'nin yerel olarak yüklenmiş sürümüyle dağıtmayı seçebilirsiniz. Daha fazla bilgi için [Azure Komut Satırı Arabirimi (CLI)](/cli/azure/) makalesine bakın.

<a id="create-sproc"></a>

## <a name="create-an-azure-cosmos-db-container-with-server-side-functionality"></a>Sunucu tarafı işlevselliği olan bir Azure Cosmos DB kapsayıcısı oluşturma

Depolanmış yordam, tetikleyici ve kullanıcı tanımlı işleve sahip bir Azure Cosmos DB kapsayıcısı oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.

Aşağıdaki örnek şablonu kopyalayın ve açıklandığı gibi [PowerShell](#deploy-with-powershell) veya [Azure CLI](#deploy-with-azure-cli)ile dağıtın.

* İsteğe bağlı olarak, [Azure Quickstart Galerisi'ni](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-sprocs/) ziyaret edebilir ve şablonu Azure portalından dağıtabilirsiniz.
* Ayrıca şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturabilir `--template-file` ve parametreyle yerel yolu belirtebilirsiniz.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql-container-sprocs/azuredeploy.json":::

### <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

Azure Kaynak Yöneticisi şablonuna dağıtmak için PowerShell'i kullanmak için:

1. Senaryoyu **kopyala.**
1. Azure Bulut Suünü açmak için **Deneyin'i** seçin.
1. Azure Bulut Kabuğu penceresine sağ tıklayın ve ardından **Yapıştır'ı**seçin.

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

Şablonu Azure Bulut BulutU yerine Yerel olarak yüklenen PowerShell sürümüyle dağıtmayı seçebilirsiniz. [Azure PowerShell modüllerini yüklemeniz](/powershell/azure/install-az-ps)gerekir. Gerekli `Get-Module -ListAvailable Az` sürümü bulmak için çalıştırın.

### <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

Azure Kaynak Yöneticisi şablonuna dağıtmak için Azure CLI'yi kullanmak için:

1. Senaryoyu **kopyala.**
2. Azure Bulut Suünü açmak için **Deneyin'i** seçin.
3. Azure Bulut BulutU penceresinde sağ tıklatın ve ardından **Yapıştır'ı**seçin.

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

Aşağıdaki ek kaynakları da inceleyebilirsiniz:

* [Azure Kaynak Yöneticisi belgeleri](/azure/azure-resource-manager/)
* [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
* [Azure Cosmos DB Hızlı Başlatma şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Documentdb&pageNumber=1&sort=Popular)
* [Sık karşılaşılan Azure Kaynak Yöneticisi dağıtım hatalarını giderme](../azure-resource-manager/templates/common-deployment-errors.md)
