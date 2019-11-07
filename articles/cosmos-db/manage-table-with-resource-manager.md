---
title: Azure Cosmos DB için Azure Resource Manager şablonlar Tablo API'si
description: Azure Cosmos DB Tablo API'si oluşturmak ve yapılandırmak için Azure Resource Manager şablonları kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.openlocfilehash: aa169ed7001ad858a2a0373f9d5bbbe770a2a727
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73604472"
---
# <a name="manage-azure-cosmos-db-table-api-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak Azure Cosmos DB Tablo API'si kaynaklarını yönetme

Bu makalede, Azure Resource Manager şablonları kullanarak Azure Cosmos DB hesaplarınızın, veritabanlarınızın ve kapsayıcılarınızın yönetimini otomatikleştirmek için farklı işlemlerin nasıl gerçekleştirileceği açıklanır. Bu makalede yalnızca Tablo API'si hesap örnekleri bulunur, diğer API türü hesaplara yönelik örnekler bulunur. bkz. [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md), [SQL](manage-sql-with-resource-manager.md) makaleleri için Azure Cosmos DB API 'si ile Kaynak Yöneticisi şablonları kullanma.

## Azure Cosmos hesabı ve tablosu oluşturma<a id="create-resource"></a>

Azure Resource Manager şablonu kullanarak Azure Cosmos DB kaynakları oluşturun. Bu şablon, 400 RU/sn aktarım hızı ile bir tablo içeren Tablo API'si için bir Azure Cosmos hesabı oluşturur. Şablonu kopyalayın ve aşağıda gösterildiği gibi dağıtın veya [Azure hızlı başlangıç Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-table/) ' ni ziyaret edin ve Azure Portal dağıtın. Ayrıca, şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturup `--template-file` parametresiyle yerel yolu belirtebilirsiniz.

> [!NOTE]
> Hesap adları küçük harf ve 31 karakter < olmalıdır.

[!code-json[create-cosmos-table](~/quickstart-templates/101-cosmosdb-table/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>PowerShell aracılığıyla dağıtma

PowerShell kullanarak Kaynak Yöneticisi şablonunu dağıtmak için betiği **kopyalayın** ve Azure Cloud Shell 'i açmak için **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$tableName = Read-Host -Prompt "Enter the table name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json" `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -tableName $tableName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Azure Cloud Shell yerine PowerShell 'in yerel olarak yüklü bir sürümünü kullanmayı seçerseniz Azure PowerShell modülünü [yüklemelisiniz](/powershell/azure/install-az-ps) . Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın.

### <a name="deploy-via-azure-cli"></a>Azure CLı aracılığıyla dağıtma

Azure CLı kullanarak Kaynak Yöneticisi şablonunu dağıtmak için betiği **kopyalayın** ve Azure Cloud Shell 'i açmak için **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the table name: ' tableName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion tableName=$tableName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

`az cosmosdb show` komutu, yeni oluşturulan Azure Cosmos hesabını, sağlandıktan sonra gösterir. CloudShell kullanmak yerine yerel olarak yüklenmiş bir Azure CLı sürümü kullanmayı seçerseniz, bkz. [Azure komut satırı arabirimi (CLI)](/cli/azure/) makalesi.

## Bir tablodaki üretilen işi (RU/s) güncelleştirme<a id="table-ru-update"></a>

Aşağıdaki şablon bir tablonun verimini güncelleştirecek. Şablonu kopyalayın ve aşağıda gösterildiği gibi dağıtın veya [Azure hızlı başlangıç Galerisi](https://azure.microsoft.com/resources/templates/101-cosmosdb-table-ru-update/) ' ni ziyaret edin ve Azure Portal dağıtın. Ayrıca, şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturup `--template-file` parametresiyle yerel yolu belirtebilirsiniz.

[!code-json[cosmosdb-table-ru-update](~/quickstart-templates/101-cosmosdb-table-ru-update/azuredeploy.json)]

### <a name="deploy-table-throughput-via-powershell"></a>PowerShell aracılığıyla tablo aktarım hızını dağıtma

PowerShell kullanarak Kaynak Yöneticisi şablonunu dağıtmak için betiği **kopyalayın** ve Azure Cloud Shell 'i açmak için **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$tableName = Read-Host -Prompt "Enter the table name"
$throughput = Read-Host -Prompt "Enter new throughput for table"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -tableName $tableName `
    -throughput $throughput
```

### <a name="deploy-table-template-via-azure-cli"></a>Azure CLı aracılığıyla tablo şablonu dağıtma

Azure CLı kullanarak Kaynak Yöneticisi şablonunu dağıtmak için, Azure Cloud Shell 'i açmak üzere **deneyin** ' i seçin. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the table name: ' tableName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json \
   --parameters accountName=$accountName tableName=$tableName throughput=$throughput
```

## <a name="next-steps"></a>Sonraki Adımlar

Bazı ek kaynaklar aşağıda verilmiştir:

- [Azure Resource Manager belgeleri](/azure/azure-resource-manager/)
- [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Ortak Azure Resource Manager Dağıtım hatalarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md)