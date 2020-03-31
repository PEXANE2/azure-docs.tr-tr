---
title: Azure Cosmos DB Gremlin API için Kaynak Yöneticisi şablonları
description: Azure Cosmos DB Gremlin API oluşturmak ve yapılandırmak için Azure Kaynak Yöneticisi şablonlarını kullanın.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: e4c18d93f07cb2143dcc5bf9c93b9ac7298d2f7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246753"
---
# <a name="manage-azure-cosmos-db-gremlin-api-resources-using-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Cosmos DB Gremlin API kaynaklarını yönetme

Bu makalede, Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Cosmos DB hesaplarınızın, veritabanlarınızın ve kapsayıcılarınızın yönetimini otomatikleştirmek için farklı işlemlerin nasıl gerçekleştirilişeği açıklanmaktadır. Bu makalede, diğer API türü hesaplara örnek bulmak için yalnızca Gremlin API hesapları için örnekler vardır: Azure Cosmos DB'nin [Cassandra,](manage-cassandra-with-resource-manager.md) [SQL,](manage-sql-with-resource-manager.md) [MongoDB,](manage-mongodb-with-resource-manager.md) [Tablo](manage-table-with-resource-manager.md) makaleleri için Azure Kaynak Yöneticisi şablonlarını kullanın.

## <a name="create-azure-cosmos-db-api-for-mongodb-account-database-and-collection"></a>MongoDB hesabı, veritabanı ve koleksiyonu için Azure Cosmos DB API'sini oluşturma<a id="create-resource"></a>

Azure Kaynak Yöneticisi şablonu kullanarak Azure Cosmos DB kaynaklarını oluşturun. Bu şablon, veritabanı düzeyinde 400 RU/s iş verisini paylaşan iki grafikiçeren Gremlin API için bir Azure Cosmos hesabı oluşturur. Şablonu kopyalayın ve aşağıda gösterildiği gibi dağıtın veya [Azure Quickstart Galerisi'ni](https://azure.microsoft.com/resources/templates/101-cosmosdb-gremlin/) ziyaret edin ve Azure portalından dağıtın. Ayrıca şablonu yerel bilgisayarınıza indirebilir veya yeni bir şablon oluşturabilir `--template-file` ve parametreyle yerel yolu belirtebilirsiniz.

> [!NOTE]
> Hesap adları küçük ve 44 veya daha az karakter olmalıdır.
> RU/s'leri güncelleştirmek için, şablonu güncelleştirilmiş iş verme özelliği değerleriyle yeniden gönderin.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-gremlin/azuredeploy.json":::

## <a name="deploy-with-the-azure-cli"></a>Azure CLI ile dağıtma

Azure CLI'yi kullanarak Azure Kaynak Yöneticisi şablonuna dağıtmak için komut dosyasını **kopyalayın** ve Azure Bulut Bulutu'nu açmak için **deneyin'i** seçin. Komut dosyasını yapıştırmak için kabuğu sağ tıklatın ve sonra **Yapıştır'ı**seçin:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first graph name: ' graph1Name
read -p 'Enter the second graph name: ' graph2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   graph1Name=$graph1Name graph2Name=$graph2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

Komut, `az cosmosdb show` yeni oluşturulan Azure Cosmos hesabını oluşturulduktan sonra gösterir. Bulut Kabuğu kullanmak yerine Azure CLI'nin yerel olarak yüklenmiş bir sürümünü kullanmayı seçerseniz, [Azure CLI](/cli/azure/) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki ek kaynakları da inceleyebilirsiniz:

- [Azure Kaynak Yöneticisi belgeleri](/azure/azure-resource-manager/)
- [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB Hızlı Başlatma şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Sık karşılaşılan Azure Kaynak Yöneticisi dağıtım hatalarını giderme](../azure-resource-manager/templates/common-deployment-errors.md)