---
title: Quickstart - Azure Kaynak Yöneticisi şablonu kullanarak bir Azure Cosmos DB ve kapsayıcı oluşturun
description: Azure Kaynak Yöneticisi şablonu kullanarak Azure Cosmos veritabanının ve kapsayıcının nasıl yapılacağını gösteren hızlı başlangıç
author: SnehaGunda
ms.author: sngun
tags: azure-resource-manager
ms.service: cosmos-db
ms.topic: quickstart
ms.date: 02/27/2020
ms.custom: subject-armqs
ms.openlocfilehash: f524a1e1db426b9b9dafb2fb95d77538a34b04ec
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605459"
---
# <a name="quickstart-create-an-azure-cosmos-db-and-a-container-by-using-azure-resource-manager-template"></a>Hızlı başlatma: Azure Kaynak Yöneticisi şablonu kullanarak bir Azure Cosmos DB ve kapsayıcı oluşturun

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Anahtar/değer veritabanlarını, belge veritabanlarını ve grafik veritabanlarını hızla oluşturmak ve sorgulamak için Azure Cosmos DB'yi kullanabilirsiniz. Bu hızlı başlangıç, bir Azure Cosmos veritabanı ve bu veritabanı içinde bir kapsayıcı oluşturmak için bir Kaynak Yöneticisi şablonu dağıtma işlemine odaklanır. Verileri daha sonra bu kapsayıcıda depolayabilirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliği veya ücretsiz Azure Cosmos DB deneme hesabı

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

## <a name="create-an-azure-cosmos-account-database-container"></a>Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-cosmosdb-create/)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-create/azuredeploy.json":::

Şablonda üç Azure kaynağı tanımlanır:

* [Microsoft.DocumentDB/databaseAccounts](/azure/templates/microsoft.documentdb/databaseaccounts): Bir Azure Cosmos hesabı oluşturun.

* [Microsoft.DocumentDB/databaseAccounts/sqlDatabases](/azure/templates/microsoft.documentdb/databaseaccounts/sqldatabases): Azure Cosmos veritabanı oluşturun.

* [Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers](/azure/templates/microsoft.documentdb/databaseaccounts/sqldatabases/containers): Bir Azure Cosmos kapsayıcısı oluşturun.

Daha fazla Azure Cosmos DB şablon örnekleri [quickstart şablon galerisinde](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Documentdb)bulunabilir.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir Azure Cosmos hesabı, bir veritabanı ve bir kapsayıcı oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-create%2Fazuredeploy.json)

2. Aşağıdaki değerleri seçin veya girin.

   ![Kaynak Yöneticisi şablonu, Azure Cosmos DB entegrasyonu, portalı dağıtma](./media/quick-create-template/create-cosmosdb-using-template-portal.png)

    Belirtilmediği sürece, Azure Cosmos kaynaklarını oluşturmak için varsayılan değerleri kullanın.

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni Oluştur'u**seçin, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam'ı**tıklatın.
    * **Konum**: Bir konum seçin.  Örneğin, **Orta ABD**.
    * **Hesap Adı**: Azure Cosmos hesabının adını girin. Dünya çapında benzersiz olmalı.
    * **Konum**: Azure Cosmos hesabınızı oluşturmak istediğiniz bir konum girin. Azure Cosmos hesabı kaynak grubuyla aynı konumda olabilir.
    * **Birincil Bölge**: Azure Cosmos hesabının birincil yineleme bölgesi.
    * **İkincil bölge**: Azure Cosmos hesabının ikincil yineleme bölgesi.
    * **Veritabanı Adı**: Azure Cosmos veritabanının adı.
    * **Konteyner Adı**: Azure Cosmos kapsayıcısının adı.
    * **Elde Edilen Kaynak**: Konteynerin iş mesuliyni, minimum iş bedeli 400 RU/s'dir.
    * **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**: Seçin.

3. **Satın al**'ı seçin. Azure Cosmos hesabı başarıyla dağıtıldıktan sonra bir bildirim alırsınız:

   ![Kaynak Yöneticisi şablonu, Cosmos DB entegrasyonu, portal bildirimi dağıtma](./media/quick-create-template/resource-manager-template-portal-deployment-notification.png)

Azure portalı şablonu dağıtmak için kullanılır. Azure portalına ek olarak, Azure PowerShell, Azure CLI ve REST API'yi de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için [şablonları dağıt'a](../azure-resource-manager/templates/deploy-powershell.md)bakın.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Azure Cosmos hesabını, veritabanını ve kapsayıcıyı denetlemek için Azure portalını kullanabilir veya oluşturulan sırrı listelemek için aşağıdaki Azure CLI veya Azure PowerShell komut dosyasını kullanabilirsiniz.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter your Azure Cosmos account name:" &&
read cosmosAccountName &&
echo "Enter the resource group where the Azure Cosmos account exists:" &&
read resourcegroupName &&
az cosmosdb show -g $resourcegroupName -n $cosmosAccountName
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the resource group name where your Azure Cosmos account exists"
(Get-AzResource -ResourceType "Microsoft.DocumentDB/databaseAccounts" -ResourceGroupName $resourceGroupName).Name
 Write-Host "Press [ENTER] to continue..."
```

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmak isteyebilirsiniz.
Artık gerekmediğinde, Azure Cosmos hesabını ve ilgili kaynakları silen kaynak grubunu silin. Azure CLI veya Azure Powershell kullanarak kaynak grubunu silmek için:

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Kaynak Yöneticisi şablonu kullanarak bir Azure Cosmos hesabı, veritabanı ve kapsayıcı oluşturdunuz ve dağıtımı doğruladınız. Azure Cosmos DB ve Azure Kaynak Yöneticisi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere devam edin.

- Azure [Cosmos DB'ye Genel Bakış](introduction.md)
- [Azure Kaynak Yöneticisi](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin
- Diğer [Azure Cosmos DB Kaynak Yöneticisi şablonlarını](resource-manager-samples.md) alın