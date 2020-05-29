---
title: Hızlı başlangıç-Azure Resource Manager şablonuna göre Azure Databricks çalışma alanı oluşturma
description: Bu hızlı başlangıçta, bir Azure Databricks çalışma alanı oluşturmak için Azure Resource Manager şablonunun nasıl kullanılacağı gösterilmektedir.
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.custom: mvc, subject-armqs
ms.date: 05/27/2020
ms.openlocfilehash: 8435704963e832020ecff27a11d00793ad28890c
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84171047"
---
# <a name="quickstart-create-an-azure-databricks-workspace-by-using-the-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonunu kullanarak Azure Databricks çalışma alanı oluşturma

Bu hızlı başlangıçta, bir Azure Databricks çalışma alanı oluşturmak için Azure Resource Manager şablonu kullanırsınız. Çalışma alanı oluşturulduktan sonra dağıtımı doğrularsınız.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlayabilmeniz için şunları yapmanız gerekir:

* Bir Azure aboneliğine sahip olmak [için ücretsiz olarak oluşturun](https://azure.microsoft.com/free/)

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/tree/master/101-databricks-workspace).

:::code language="json" source="~/quickstart-templates/101-databricks-workspace/azuredeploy.json" range="1-53":::

Şablonda tanımlanan Azure kaynağı Microsoft. Databricks/Workspaces: Azure Databricks çalışma alanı oluşturur. 

## <a name="deploy-the-template"></a>Şablonu dağıtma

Bu bölümde, Azure Resource Manager şablonunu kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir Azure Databricks çalışma alanı oluşturur.

   [![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-databricks-workspace%2Fazuredeploy.json)

2. Azure Databricks çalışma alanınızı oluşturmak için gerekli değerleri sağlayın

   ![Azure Resource Manager şablonu kullanarak Azure Databricks çalışma alanı oluşturma](./media/quickstart-create-databricks-workspace-resource-manager-template/create-databricks-workspace-using-resource-manager-template.png "Azure Resource Manager şablonu kullanarak Azure Databricks çalışma alanı oluşturma")

   Aşağıdaki değerleri sağlayın:

   |Özellik  |Açıklama  |
   |---------|---------|
   |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
   |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümüne ilişkin kaynakları tutan bir kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../azure-resource-manager/management/overview.md). |
   |**Konum**     | **Doğu ABD 2**’yi seçin. Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).        |
   |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın        |
   |**Fiyatlandırma Katmanı**     |  **Standart** veya **Premium** arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

3. **Gözden Geçir ve Oluştur**’u ve sonra **Oluştur**’u seçin.

4. Çalışma alanının oluşturulması birkaç dakika sürer. Bir çalışma alanı dağıtımı başarısız olduğunda, çalışma alanı hala hatalı durumda oluşturulur. Başarısız çalışma alanını silin ve dağıtım hatalarını çözen yeni bir çalışma alanı oluşturun. Başarısız çalışma alanını sildiğinizde, yönetilen kaynak grubu ve başarıyla dağıtılan tüm kaynaklar da silinir.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Azure Databricks çalışma alanını denetlemek için Azure portal kullanabilir ya da kaynağı listelemek için aşağıdaki Azure CLı veya Azure PowerShell komut dosyasını kullanabilirsiniz.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
echo "Enter your Azure Databricks workspace name:" &&
read databricksWorkspaceName &&
echo "Enter the resource group where the Azure Databricks workspace exists:" &&
read resourcegroupName &&
az databricks workspace show -g $resourcegroupName -n $databricksWorkspaceName
```

### <a name="azure-powershell"></a>Azure PowerShell

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the resource group name where your Azure Databricks workspace exists"
(Get-AzResource -ResourceType "Microsoft.Databricks/workspaces" -ResourceGroupName $resourceGroupName).Name
 Write-Host "Press [ENTER] to continue..."
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticilere devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmak isteyebilirsiniz. Artık gerekli değilse, Azure Databricks çalışma alanını ve ilgili yönetilen kaynakları silen kaynak grubunu silin. Azure CLı veya Azure PowerShell kullanarak kaynak grubunu silmek için:

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

### <a name="azure-powershell"></a>Azure PowerShell

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Resource Manager şablonu kullanarak bir Azure Databricks çalışma alanı oluşturdunuz ve dağıtımı doğruladı. Azure Databricks kullanılarak bir ETL işleminin (verileri ayıklama, dönüştürme ve yükleme) nasıl gerçekleştirileceğini öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme](databricks-extract-load-sql-data-warehouse.md)
