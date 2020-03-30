---
title: Azure Kaynak Yöneticisi şablonu kullanarak Bir Azure Veri Gezgini kümesi ve veritabanı oluşturma
description: Azure Kaynak Yöneticisi şablonu kullanarak Azure Veri Gezgini kümesi ni ve veritabanını nasıl oluşturabilirsiniz öğrenin
author: orspod
ms.author: orspodek
ms.reviewer: lugoldbe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/26/2019
ms.openlocfilehash: 56639d8a29ad8eac465845c8d354d04b31ba6093
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75911956"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak Bir Azure Veri Gezgini kümesi ve veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [Powershell](create-cluster-database-powershell.md)
> * [C #](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [Azure Resource Manager şablonu](create-cluster-database-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini'ni kullanmak için öncelikle bir küme ve bu kümenin içinde bir veya daha fazla veritabanı oluşturmanız gerekir. Ardından veritabanına veri alarak (yükleyerek) sorgu çalıştırabilirsiniz. 

Bu makalede, bir [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/management/overview.md)kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturursunuz. Makalede, hangi kaynakların dağıtıldığı ve dağıtım yürütüldüğünde belirtilen parametrelerin nasıl tanımlanılığı gösterilmektedir. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablon oluşturma hakkında bilgi için [Azure Kaynak Yöneticisi şablonları yazma'ya](/azure/azure-resource-manager/resource-group-authoring-templates)bakın. Şablonda kullanılacak JSON sözdizimi ve özellikleri için [Microsoft.Kusto kaynak türlerine](/azure/templates/microsoft.kusto/allversions)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="azure-resource-manager-template-for-cluster-and-database-creation"></a>Küme ve veritabanı oluşturma için Azure Kaynak Yöneticisi şablonu

Bu makalede, varolan bir [hızlı başlangıç şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-kusto-cluster-database/azuredeploy.json) kullanıyorsunuz

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

Daha fazla şablon örneği bulmak için [Azure Hızlı Başlatma Şablonları'na](https://azure.microsoft.com/resources/templates/)bakın.

## <a name="deploy-the-template-and-verify-template-deployment"></a>Şablonu dağıtma ve şablon dağıtımını doğrula

Azure portalını kullanarak veya [powershell'i kullanarak](#use-powershell-to-deploy-the-template-and-verify-template-deployment)Azure Kaynak Yöneticisi [şablonuna](#use-the-azure-portal-to-deploy-the-template-and-verify-template-deployment) dağıtabilirsiniz.

### <a name="use-the-azure-portal-to-deploy-the-template-and-verify-template-deployment"></a>Şablonu dağıtmak ve şablon dağıtımını doğrulamak için Azure portalını kullanın

1. Bir küme ve veritabanı oluşturmak için dağıtımı başlatmak için aşağıdaki düğmeyi kullanın. Bu makaledeki adımların geri kalanını takip etmek için **yeni pencerede Aç'ı**sağ tıklatın ve seçin.

    [![Azure'a Dağıt](media/create-cluster-database-resource-manager/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-kusto-cluster-database%2Fazuredeploy.json)

    **Azure'a dağıtma** düğmesi Azure portalda doldurmanız gereken bir form sayfasını açar.

    ![Azure’a Dağıt](media/create-cluster-database-resource-manager/deploy-2-azure.png)

    Formu kullanarak [şablonu Azure portalında düzenleme yapabilir ve dağıtabilirsiniz.](/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal#edit-and-deploy-the-template)

1. **TEMELLER** ve **AYARLAR** bölümlerini tamamlayın. Benzersiz küme ve veritabanı adlarını seçin.
Bir Azure Veri Gezgini kümesi ve veritabanı oluşturmak birkaç dakika sürer.

1. Dağıtımı doğrulamak için, yeni kümenizi ve veritabanınızı bulmak için [Azure portalındaki](https://portal.azure.com) kaynak grubunu açarsınız. 

### <a name="use-powershell-to-deploy-the-template-and-verify-template-deployment"></a>Şablonu dağıtmak ve şablon dağıtımını doğrulamak için powershell'i kullanın

#### <a name="deploy-the-template-using-powershell"></a>Powershell'i kullanarak şablonu dağıtma

1. Aşağıdaki kod bloğundan **deneyin'i** seçin ve ardından Azure Bulutu kabuğunda oturum açma yönergelerini izleyin.

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

1. PowerShell komut dosyasını kopyalamak için **Kopyala'yı** seçin.
1. Kabuk konsoluna sağ tıklayın ve ardından **Yapıştır'ı**seçin.
Bir Azure Veri Gezgini kümesi ve veritabanı oluşturmak birkaç dakika sürer.

#### <a name="verify-the-deployment-using-powershell"></a>PowerShell'i kullanarak dağıtımı doğrulayın

Dağıtımı doğrulamak için aşağıdaki Azure PowerShell komut dosyasını kullanın.  Bulut Kabuğu hala açıksa, ilk satırı kopyalamanız/çalıştırmanız gerekmez (Oku-Ana Bilgisayar). PowerShell'de Azure Veri Gezgini kaynaklarını yönetme yle ilgili daha fazla bilgi için [Az.Kusto'u](/powershell/module/az.kusto/?view=azps-2.7.0)okuyun. 

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

[Azure Veri Gezgini kümesine ve veritabanına veri alma](ingest-data-overview.md)
