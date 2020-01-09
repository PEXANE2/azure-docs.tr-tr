---
title: Azure Resource Manager şablonu kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturma
description: Bir Azure Resource Manager şablonu kullanarak Azure Veri Gezgini kümesi ve veritabanı oluşturmayı öğrenin
author: orspod
ms.author: orspodek
ms.reviewer: oflipman
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/26/2019
ms.openlocfilehash: 9a91f81c9d28f1b3c8d553a2a8a26d53ff1b2b3e
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75444484"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [Azure Resource Manager şablonu](create-cluster-database-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini kullanmak için, önce bir küme oluşturun ve bu kümede bir veya daha fazla veritabanı oluşturursunuz. Daha sonra sorguları bu verilere karşı çalıştırmak için bir veritabanına (yükleme) sahip olursunuz. 

Bu makalede, bir [Azure Resource Manager şablonu](../azure-resource-manager/management/overview.md)kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturacaksınız. Makalede nasıl tanımlamak için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen gösterilmektedir. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablon oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](/azure/azure-resource-manager/resource-group-authoring-templates). Bir şablonda kullanılacak JSON sözdizimi ve özellikler için bkz. [Microsoft. kusto kaynak türleri](/azure/templates/microsoft.kusto/allversions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="azure-resource-manager-template-for-cluster-and-database-creation"></a>Küme ve veritabanı oluşturma için Azure Resource Manager şablonu

Bu makalede, [mevcut bir hızlı başlangıç şablonunu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-kusto-cluster-database/azuredeploy.json) kullanacaksınız

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "clusters_kustocluster_name": {
          "type": "string",
          "defaultValue": "[concat('kusto', uniqueString(resourceGroup().id))]",
          "metadata": {
            "description": "Name of the cluster to create"
          }
      },
      "databases_kustodb_name": {
          "type": "string",
          "defaultValue": "kustodb",
          "metadata": {
            "description": "Name of the database to create"
          }
      },
      "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]",
        "metadata": {
          "description": "Location for all resources."
        }
      }
  },
  "variables": {},
  "resources": [
      {
          "name": "[parameters('clusters_kustocluster_name')]",
          "type": "Microsoft.Kusto/clusters",
          "sku": {
              "name": "Standard_D13_v2",
              "tier": "Standard",
              "capacity": 2
          },
          "apiVersion": "2019-09-07",
          "location": "[parameters('location')]",
          "tags": {
            "Created By": "GitHub quickstart template"
          }
      },
      {
          "name": "[concat(parameters('clusters_kustocluster_name'), '/', parameters('databases_kustodb_name'))]",
          "type": "Microsoft.Kusto/clusters/databases",
          "apiVersion": "2019-09-07",
          "location": "[parameters('location')]",
          "dependsOn": [
              "[resourceId('Microsoft.Kusto/clusters', parameters('clusters_kustocluster_name'))]"
          ],
          "properties": {
              "softDeletePeriodInDays": 365,
              "hotCachePeriodInDays": 31
          }
      }
  ]
}
```

Daha fazla şablon örneği bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/).

## <a name="deploy-the-template-and-verify-template-deployment"></a>Şablonu dağıtma ve şablon dağıtımını doğrulama

[Azure Portal](#use-the-azure-portal-to-deploy-the-template-and-verify-template-deployment) veya [powershell](#use-powershell-to-deploy-the-template-and-verify-template-deployment)'i kullanarak Azure Resource Manager şablonu dağıtabilirsiniz.

### <a name="use-the-azure-portal-to-deploy-the-template-and-verify-template-deployment"></a>Şablonu dağıtmak ve şablon dağıtımını doğrulamak için Azure portal kullanma

1. Bir küme ve veritabanı oluşturmak için, dağıtımı başlatmak üzere aşağıdaki düğmeyi kullanın. Sağ tıklayıp **Yeni pencerede aç**' ı seçerek bu makaledeki adımların geri kalanını izleyebilirsiniz.

    [![Azure’a dağıtma](media/create-cluster-database-resource-manager/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-kusto-cluster-database%2Fazuredeploy.json)

    **Azure'a dağıtma** düğmesi Azure portalda doldurmanız gereken bir form sayfasını açar.

    ![Azure'a Dağıt](media/create-cluster-database-resource-manager/deploy-2-azure.png)

    Formunu kullanarak [Azure Portal şablonu düzenleyebilir ve dağıtabilirsiniz](/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal#edit-and-deploy-the-template) .

1. Tüm **temel bilgiler** ve **Ayarlar** bölümleri. Benzersiz küme ve veritabanı adları ' nı seçin.
Azure Veri Gezgini kümesi ve veritabanının oluşturulması birkaç dakika sürer.

1. Dağıtımı doğrulamak için, yeni kümenizi ve veritabanınızı bulmak üzere [Azure Portal](https://portal.azure.com) kaynak grubunu açarsınız. 

### <a name="use-powershell-to-deploy-the-template-and-verify-template-deployment"></a>Şablonu dağıtmak ve şablon dağıtımını doğrulamak için PowerShell 'i kullanma

#### <a name="deploy-the-template-using-powershell"></a>PowerShell kullanarak şablonu dağıtma

1. Aşağıdaki kod bloğundan **deneyin** ' i seçin ve ardından Azure Cloud Shell 'de oturum açmak için yönergeleri izleyin.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${projectName}rg"
    $clusterName = "${projectName}cluster"
    $parameters = @{}
    $parameters.Add("clusters_kustocluster_name", $clusterName)
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-kusto-cluster-database/azuredeploy.json"
    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -TemplateParameterObject $parameters
    Write-Host "Press [ENTER] to continue ..."
    ```

1. Seçin **kopyalama** PowerShell betiğini kopyalanacak.
1. Kabuk konsoluna sağ tıklayın ve ardından **Yapıştır**' ı seçin.
Azure Veri Gezgini kümesi ve veritabanının oluşturulması birkaç dakika sürer.

#### <a name="verify-the-deployment-using-powershell"></a>PowerShell kullanarak dağıtımı doğrulama

Dağıtımı doğrulamak için aşağıdaki Azure PowerShell betiğini kullanın.  Cloud Shell hala açıksa, ilk satırı (Read-Host) kopyalamanız/çalıştırmanız gerekmez. PowerShell 'de Azure Veri Gezgini kaynaklarını yönetme hakkında daha fazla bilgi için [az. kusto](/powershell/module/az.kusto/?view=azps-2.7.0)makalesini okuyun. 

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"

Install-Module -Name Az.Kusto
$resourceGroupName = "${projectName}rg"
$clusterName = "${projectName}cluster"

Get-AzKustoCluster -ResourceGroupName $resourceGroupName -Name $clusterName
Write-Host "Press [ENTER] to continue ..."
```

[!INCLUDE [data-explorer-clean-resources](../../includes/data-explorer-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Verileri Azure Veri Gezgini kümesine ve veritabanına alma](ingest-data-overview.md)
