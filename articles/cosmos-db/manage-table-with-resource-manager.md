---
title: Azure Cosmos DB Tablo API'si için Kaynak Yöneticisi şablonları
description: Azure Cosmos DB Tablo API'sini oluşturmak ve yapılandırmak için Azure Kaynak Yöneticisi şablonlarını kullanın.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: 6ab54e56368e7e26e807e4d1dc0592536dc9374a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246714"
---
# <a name="manage-azure-cosmos-db-table-api-resources-using-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Cosmos DB Tablo API kaynaklarını yönetme

Bu makalede, Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Cosmos DB hesaplarınızın, veritabanlarınızın ve kapsayıcılarınızın yönetimini otomatikleştirmek için farklı işlemlerin nasıl gerçekleştirilişeği açıklanmaktadır. Bu makalede yalnızca Tablo API hesapları için örnekler vardır, diğer API türü hesaplarına örnek bulmak için bkz: Azure Cosmos DB'nin ApI'sini [Cassandra,](manage-cassandra-with-resource-manager.md) [Gremlin,](manage-gremlin-with-resource-manager.md) [MongoDB](manage-mongodb-with-resource-manager.md), [SQL](manage-sql-with-resource-manager.md) makaleleri için azure kaynak yöneticisi şablonlarını kullanın.

## <a name="create-azure-cosmos-account-and-table"></a>Azure Cosmos hesabı ve tablosu oluşturma<a id="create-resource"></a>

Azure Kaynak Yöneticisi şablonu kullanarak Azure Cosmos DB kaynaklarını oluşturun. Bu şablon, 400 RU/s iş ortası olan bir tabloyla Tablo API'si için bir Azure Cosmos hesabı oluşturur. Şablonu kopyalayın ve aşağıda gösterildiği gibi dağıtın veya [Azure Quickstart Galerisi'ni](https://azure.microsoft.com/resources/templates/101-cosmosdb-table/) ziyaret edin ve Azure portalından dağıtın. Ayrıca şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturabilir `--template-file` ve parametreyle yerel yolu belirtebilirsiniz.

> [!NOTE]
> Hesap adları küçük ve 44 veya daha az karakter olmalıdır.
> RU/s'leri güncelleştirmek için, şablonu güncelleştirilmiş iş verme özelliği değerleriyle yeniden gönderin.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-table/azuredeploy.json":::

### <a name="deploy-via-powershell"></a>PowerShell üzerinden dağıt

PowerShell'i kullanarak Kaynak Yöneticisi şablonuna dağıtmak için komut dosyasını **kopyalayın** ve Azure Bulut Bulutu'nu açmak için **deneyin'i** seçin. Komut dosyasını yapıştırmak için kabuğu sağ tıklatın ve sonra **Yapıştır'ı**seçin:

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

Azure Bulut uyşu yerine PowerShell'in yerel olarak yüklenmiş bir sürümünü kullanmayı seçerseniz, Azure PowerShell modüllerini [yüklemeniz](/powershell/azure/install-az-ps) gerekir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın.

### <a name="deploy-via-the-azure-cli"></a>Azure CLI üzerinden dağıtma

Azure CLI'yi kullanarak Azure Kaynak Yöneticisi şablonuna dağıtmak için komut dosyasını **kopyalayın** ve Azure Bulut Bulutu'nu açmak için **deneyin'i** seçin. Komut dosyasını yapıştırmak için kabuğu sağ tıklatın ve sonra **Yapıştır'ı**seçin:

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

Komut, `az cosmosdb show` yeni oluşturulan Azure Cosmos hesabını oluşturulduktan sonra gösterir. Bulut Kabuğu kullanmak yerine Azure CLI'nin yerel olarak yüklenmiş bir sürümünü kullanmayı seçerseniz, [Azure CLI](/cli/azure/) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki ek kaynakları da inceleyebilirsiniz:

- [Azure Kaynak Yöneticisi belgeleri](/azure/azure-resource-manager/)
- [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB Hızlı Başlatma şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Sık karşılaşılan Azure Kaynak Yöneticisi dağıtım hatalarını giderme](../azure-resource-manager/templates/common-deployment-errors.md)