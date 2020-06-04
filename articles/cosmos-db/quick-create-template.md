---
title: Hızlı başlangıç-Azure Resource Manager şablonu kullanarak Azure Cosmos DB ve kapsayıcı oluşturma
description: Azure Resource Manager şablonu kullanarak bir Azure Cosmos veritabanının ve kapsayıcısının nasıl yapılacağını gösteren hızlı başlangıç
author: SnehaGunda
ms.author: sngun
tags: azure-resource-manager
ms.service: cosmos-db
ms.topic: quickstart
ms.date: 06/01/2020
ms.custom: subject-armqs
ms.openlocfilehash: 273305894e05b397d0f48acd7a483a9fdfc247ef
ms.sourcegitcommit: 58ff2addf1ffa32d529ee9661bbef8fbae3cddec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84324054"
---
# <a name="quickstart-create-an-azure-cosmos-db-and-a-container-by-using-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak Azure Cosmos DB ve kapsayıcı oluşturma

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Anahtar/değer veritabanlarını, belge veritabanlarını ve grafik veritabanlarını hızlıca oluşturmak ve sorgulamak için Azure Cosmos DB kullanabilirsiniz. Bu hızlı başlangıç, bir Azure Cosmos veritabanı ve bu veritabanı içindeki bir kapsayıcı oluşturmak için bir Kaynak Yöneticisi şablonu dağıtma işlemine odaklanmaktadır. Daha sonra bu kapsayıcıdaki verileri saklayabilirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bir Azure aboneliği veya ücretsiz Azure Cosmos DB deneme hesabı

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

## <a name="create-an-azure-cosmos-account-database-container"></a>Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json":::

Şablonda üç Azure kaynağı tanımlanmıştır:

* [Microsoft. DocumentDB/databaseAccounts](/azure/templates/microsoft.documentdb/databaseaccounts): bir Azure Cosmos hesabı oluşturun.

* [Microsoft. DocumentDB/databaseAccounts/sqlDatabases](/azure/templates/microsoft.documentdb/databaseaccounts/sqldatabases): bir Azure Cosmos veritabanı oluşturma.

* [Microsoft. DocumentDB/databaseAccounts/sqlDatabases/kapsayıcılar](/azure/templates/microsoft.documentdb/databaseaccounts/sqldatabases/containers): bir Azure Cosmos kapsayıcısı oluşturun.

Daha fazla Azure Cosmos DB Şablon örneği [hızlı başlangıç şablonu galerisinde](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Documentdb)bulunabilir.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir Azure Cosmos hesabı, veritabanı ve bir kapsayıcı oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-sql%2Fazuredeploy.json)

2. Aşağıdaki değerleri seçin veya girin.

   ![Kaynak Yöneticisi şablonu, Azure Cosmos DB tümleştirme, Portal dağıtma](./media/quick-create-template/create-cosmosdb-using-template-portal.png)

    Belirtilmediği takdirde, Azure Cosmos kaynaklarını oluşturmak için varsayılan değerleri kullanın.

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni oluştur**' u seçin, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam**' a tıklayın.
    * **Konum**: Bir konum seçin.  Örneğin **Orta ABD**.
    * **Hesap adı**: Azure Cosmos hesabı için bir ad girin. Bu, genel olarak benzersiz olmalıdır.
    * **Konum**: Azure Cosmos hesabınızı oluşturmak istediğiniz konumu girin. Azure Cosmos hesabı, kaynak grubuyla aynı konumda olabilir.
    * **Birincil bölge**: Azure Cosmos hesabının birincil çoğaltma bölgesi.
    * **İkincil bölge**: Azure Cosmos hesabı için ikincil çoğaltma bölgesi.
    * **Varsayılan tutarlılık düzeyi**: Azure Cosmos hesabı için varsayılan tutarlılık düzeyi.
    * **Maksimum Eskime durumu ön eki**: en fazla eski istek. Boundedstalet için gereklidir.
    * **Saniye cinsinden en fazla zaman aralığı**: en fazla gecikme süresi. Boundedstalet için gereklidir.
    * **Veritabanı adı**: Azure Cosmos veritabanının adı.
    * **Kapsayıcı adı**: Azure Cosmos kapsayıcısının adı.
    * **Aktarım hızı**: kapsayıcı için aktarım hızı, en düşük aktarım hızı DEĞERI 400 ru/sn 'dir.
    * **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**: Bunu seçin.

3. **Satın al**'ı seçin. Azure Cosmos hesabı başarıyla dağıtıldıktan sonra bir bildirim alırsınız:

   ![Kaynak Yöneticisi şablonu, Cosmos DB tümleştirme, Portal dağıtma bildirimi](./media/quick-create-template/resource-manager-template-portal-deployment-notification.png)

Şablonu dağıtmak için Azure portalı kullanılır. Azure portal ek olarak, Azure PowerShell, Azure CLı ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Azure Cosmos hesabını, veritabanını ve kapsayıcısını denetlemek için Azure portal kullanabilir ya da oluşturulan gizli anahtarı listelemek için aşağıdaki Azure CLı veya Azure PowerShell betiğini kullanabilirsiniz.

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
Artık gerekli değilse, Azure Cosmos hesabını ve ilgili kaynakları silen kaynak grubunu silin. Azure CLı veya Azure PowerShell kullanarak kaynak grubunu silmek için:

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

Bu hızlı başlangıçta, bir Azure Resource Manager şablonu kullanarak bir Azure Cosmos hesabı, veritabanı ve kapsayıcı oluşturdunuz ve dağıtımı doğruladı. Azure Cosmos DB ve Azure Resource Manager hakkında daha fazla bilgi edinmek için aşağıdaki makalelere devam edin.

- [Azure Cosmos DB genel bakışını](introduction.md) okuyun
- [Azure Resource Manager](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin
- Diğer [Azure Cosmos DB kaynak yöneticisi şablonlarını](resource-manager-samples.md) al
