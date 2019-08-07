---
title: Azure Resource Manager şablonları kullanarak Azure Cosmos DB oluşturma ve yönetme
description: SQL (Core) API için Azure Cosmos DB oluşturmak ve yapılandırmak üzere Azure Resource Manager şablonları kullanma
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.openlocfilehash: b4d121e0628512f7bbd6aedc0a9067b31d46d0ed
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68814981"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak Azure Cosmos DB SQL (çekirdek) API kaynaklarını yönetme

## Azure Cosmos hesabı, veritabanı ve kapsayıcısı oluşturma<a id="create-resource"></a>

Azure Resource Manager şablonu kullanarak Azure Cosmos DB kaynakları oluşturun. Bu şablon, veritabanı düzeyinde 400 RU/sn aktarım hızını paylaşan iki kapsayıcıyla bir Azure Cosmos hesabı oluşturur. Şablonu kopyalayın ve aşağıda gösterildiği gibi dağıtın veya [Azure hızlı başlangıç Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) ' ni ziyaret edin ve Azure Portal dağıtın. Ayrıca, şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturup `--template-file` parametresi ile yerel yolu belirtebilirsiniz.

> [!NOTE]
>
> - Şu anda Kullanıcı tanımlı Işlevleri (UDF 'ler), saklı yordamları ve Tetikleyicileri Kaynak Yöneticisi şablonları kullanarak dağıtamazsınız.
> - Azure Cosmos hesabına eş zamanlı olarak konum ekleyemez veya bunları kaldıramaz ve diğer özellikleri değiştirebilirsiniz. Bunların ayrı işlemler olarak yapılması gerekir.
> - Hesap adları küçük harf ve 31 karakter < olmalıdır.

[!code-json[create-cosmosdb-sql](~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>PowerShell aracılığıyla dağıtma

PowerShell kullanarak Kaynak Yöneticisi şablonunu dağıtmak için betiği **kopyalayın** ve Azure Cloud Shell 'i açmak için **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$container1Name = Read-Host -Prompt "Enter the first container name"
$container2Name = Read-Host -Prompt "Enter the second container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -container1Name $container1Name `
    -container2Name $container2Name

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Azure Cloud Shell yerine PowerShell 'in yerel olarak yüklü bir sürümünü kullanmayı seçerseniz Azure PowerShell modülünü [yüklemelisiniz](/powershell/azure/install-az-ps) . Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın.

### <a name="deploy-via-azure-cli"></a>Azure CLı aracılığıyla dağıtma

Azure CLı kullanarak Kaynak Yöneticisi şablonunu dağıtmak için, Azure Cloud Shell 'i açmak üzere **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first container name: ' container1Name
read -p 'Enter the second container name: ' container2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   container1Name=$container1Name container2Name=$container2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

Komut `az cosmosdb show` , yeni oluşturulan Azure Cosmos hesabını, sağlandıktan sonra gösterir. CloudShell kullanmak yerine yerel olarak yüklenmiş bir Azure CLı sürümü kullanmayı seçerseniz, bkz. [Azure komut satırı arabirimi (CLI)](/cli/azure/) makalesi.

## Bir veritabanında üretilen iş (RU/s) güncelleştirme<a id="database-ru-update"></a>

Aşağıdaki şablon bir veritabanının verimini güncelleştirecek. Şablonu kopyalayın ve aşağıda gösterildiği gibi dağıtın veya [Azure hızlı başlangıç Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-database-ru-update/) ' ni ziyaret edin ve Azure Portal dağıtın. Ayrıca, şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturup `--template-file` parametresi ile yerel yolu belirtebilirsiniz.

[!code-json[cosmosdb-sql-database-ru-update](~/quickstart-templates/101-cosmosdb-sql-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-powershell"></a>PowerShell aracılığıyla veritabanı şablonu dağıtma

PowerShell kullanarak Kaynak Yöneticisi şablonunu dağıtmak için betiği **kopyalayın** ve Azure Cloud Shell 'i açmak için **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$throughput = Read-Host -Prompt "Enter new throughput for database"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -throughput $throughput
```

### <a name="deploy-database-template-via-azure-cli"></a>Azure CLı aracılığıyla veritabanı şablonu dağıtma

Azure CLı kullanarak Kaynak Yöneticisi şablonunu dağıtmak için, Azure Cloud Shell 'i açmak üzere **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## Kapsayıcıda güncelleştirme üretilen iş (RU/sn)<a id="container-ru-update"></a>

Aşağıdaki şablon bir kapsayıcının verimini güncelleştirecek. Şablonu kopyalayın ve aşağıda gösterildiği gibi dağıtın veya [Azure hızlı başlangıç Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-ru-update/) ' ni ziyaret edin ve Azure Portal dağıtın. Ayrıca, şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturup `--template-file` parametresi ile yerel yolu belirtebilirsiniz.

[!code-json[cosmosdb-sql-container-ru-update](~/quickstart-templates/101-cosmosdb-sql-container-ru-update/azuredeploy.json)]

### <a name="deploy-container-template-via-powershell"></a>PowerShell aracılığıyla kapsayıcı şablonu dağıtma

PowerShell kullanarak Kaynak Yöneticisi şablonunu dağıtmak için betiği **kopyalayın** ve Azure Cloud Shell 'i açmak için **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"
$throughput = Read-Host -Prompt "Enter new throughput for container"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -containerName $containerName `
    -throughput $throughput
```

### <a name="deploy-container-template-via-azure-cli"></a>Azure CLı aracılığıyla kapsayıcı şablonu dağıtma

Azure CLı kullanarak Kaynak Yöneticisi şablonunu dağıtmak için, Azure Cloud Shell 'i açmak üzere **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName containerName=$containerName throughput=$throughput
```

## <a name="next-steps"></a>Sonraki Adımlar

Bazı ek kaynaklar aşağıda verilmiştir:

- [Azure Resource Manager belgeleri](/azure/azure-resource-manager/)
- [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Ortak Azure Resource Manager Dağıtım hatalarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md)