---
title: Azure Cosmos DB için Kaynak Yöneticisi şablonlar Tablo API'si
description: Azure Cosmos DB Tablo API'si oluşturmak ve yapılandırmak için Azure Resource Manager şablonları kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/19/2020
ms.author: mjbrown
ms.openlocfilehash: a12b3ad3aba42870f72cda5d4b574b5b88b8d5e4
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85262302"
---
# <a name="manage-azure-cosmos-db-table-api-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak Azure Cosmos DB Tablo API'si kaynaklarını yönetme

Bu makalede, Azure Cosmos DB hesaplarınızı, veritabanlarınızı ve Kapsayıcılarınızı dağıtmayı ve yönetmeyi kolaylaştırmak için Azure Resource Manager şablonlarını nasıl kullanacağınızı öğreneceksiniz.

Bu makalede yalnızca Tablo API'si hesap örnekleri bulunur, diğer API türü hesaplara yönelik örnekler bulunur. bkz. [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md), [SQL](manage-sql-with-resource-manager.md) makaleleri için Azure Cosmos DB API 'si ile Azure Resource Manager şablonları kullanma.

> [!IMPORTANT]
>
> * Hesap adları, tümü küçük harfle 44 karakter ile sınırlıdır.
> * Verimlilik değerlerini değiştirmek için, şablonu güncelleştirilmiş RU/s ile yeniden dağıtın.
> * Azure Cosmos hesabına konum eklediğinizde veya kaldırdığınızda, diğer özellikleri aynı anda değiştiremezsiniz. Bu işlemlerin ayrı olarak yapılması gerekir.

Aşağıdaki Azure Cosmos DB kaynaklarından herhangi birini oluşturmak için aşağıdaki örnek şablonu yeni bir JSON dosyasına kopyalayın. İsteğe bağlı olarak, farklı adlara ve değerlere sahip aynı kaynağın birden çok örneğini dağıttığınızda kullanmak üzere bir Parameters JSON dosyası oluşturabilirsiniz. [Azure Portal](../azure-resource-manager/templates/deploy-portal.md), [Azure CLI](../azure-resource-manager/templates/deploy-cli.md), [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) ve [GitHub](../azure-resource-manager/templates/deploy-to-azure-button.md)dahil Azure Resource Manager şablonları dağıtmanın birçok yolu vardır.

> [!TIP]
> Tablo API'si kullanırken paylaşılan aktarım hızını etkinleştirmek için Azure portal hesap düzeyinde aktarım hızını etkinleştirin.

<a id="create-autoscale"></a>

## <a name="azure-cosmos-account-for-table-with-autoscale-throughput"></a>Otomatik ölçeklendirme üretilen iş tablosu için Azure Cosmos hesabı

Bu şablon, otomatik ölçeklendirme işleme içeren bir tabloyla Tablo API'si için bir Azure Cosmos hesabı oluşturur. Bu şablon, Azure hızlı başlangıç şablonları galerisinden tek tıklamayla dağıtım için de kullanılabilir.

[![Azure’a dağıtma](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-table-autoscale%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-table-autoscale/azuredeploy.json":::

<a id="create-manual"></a>

## <a name="azure-cosmos-account-for-table-with-standard-provisioned-throughput"></a>Standart sağlanan aktarım hızı içeren tablo için Azure Cosmos hesabı

Bu şablon, standart işleme sahip bir tabloyla Tablo API'si için bir Azure Cosmos hesabı oluşturur. Bu şablon, Azure hızlı başlangıç şablonları galerisinden tek tıklamayla dağıtım için de kullanılabilir.

[![Azure’a dağıtma](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-table%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-table/azuredeploy.json":::

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki ek kaynakları da inceleyebilirsiniz:

* [Azure Resource Manager belgeleri](/azure/azure-resource-manager/)
* [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
* [Azure Cosmos DB hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
* [Ortak Azure Resource Manager Dağıtım hatalarını giderme](../azure-resource-manager/templates/common-deployment-errors.md)
