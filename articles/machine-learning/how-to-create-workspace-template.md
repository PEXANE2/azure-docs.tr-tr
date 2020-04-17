---
title: Azure Kaynak Yöneticisi şablonuyla çalışma alanı oluşturma
titleSuffix: Azure Machine Learning
description: Yeni bir Azure Machine Learning çalışma alanı oluşturmak için Azure Kaynak Yöneticisi şablonu nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 03/05/2020
ms.custom: seoapril2019
ms.openlocfilehash: 457979837b1c56eb85fc19c9a1fce5dc7df8c23b
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81481999"
---
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]
<br>

# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning"></a>Azure Makine Öğrenimi için bir çalışma alanı oluşturmak için Azure Kaynak Yöneticisi şablonu kullanın

Bu makalede, Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Machine Learning çalışma alanı oluşturmanın çeşitli yollarını öğrenirsiniz. Kaynak Yöneticisi şablonu, tek ve eşgüdümlü bir işlem olarak kaynak oluşturmayı kolaylaştırır. Şablon, dağıtım için gereken kaynakları tanımlayan bir JSON belgesidir. Dağıtım parametrelerini de belirtebilir. Parametreler şablonu kullanırken giriş değerlerini sağlamak için kullanılır.

Daha fazla bilgi için bkz: [Azure Kaynak Yöneticisi şablonuyla bir uygulama dağıt.](../azure-resource-manager/templates/deploy-powershell.md)

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği.** Yoksa, [Azure Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* CLI'den bir şablon kullanmak için [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) veya [Azure CLI'ye](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)ihtiyacınız var.

## <a name="resource-manager-template"></a>Resource Manager şablonu

Aşağıdaki Kaynak Yöneticisi şablonu, bir Azure Machine Learning çalışma alanı ve ilişkili Azure kaynakları oluşturmak için kullanılabilir:

[!code-json[create-azure-machine-learning-service-workspace](~/quickstart-templates/101-machine-learning-create/azuredeploy.json)]

Bu şablon aşağıdaki Azure hizmetlerini oluşturur:

* Azure Kaynak Grubu
* Azure Depolama Hesabı
* Azure Key Vault
* Azure Application Insights
* Azure Container Kayıt Defteri
* Azure Machine Learning çalışma alanı

Kaynak grubu hizmetleri tutan kapsayıcıdır. Çeşitli hizmetler Azure Machine Learning çalışma alanı tarafından gereklidir.

Örnek şablonun iki parametresi vardır:

* Kaynak grubunun ve hizmetlerinin oluşturulacağı **konum.**

    Şablon, çoğu kaynak için seçtiğiniz konumu kullanır. Özel durum, diğer hizmetlerin bulunduğu tüm konumlarda bulunmayan Application Insights hizmetidir. Kullanılabilir olmayan bir konum seçerseniz, hizmet Güney Orta ABD konumunda oluşturulur.

* Azure Machine Learning çalışma alanının dostu adı olan **çalışma alanı adı.**

    > [!NOTE]
    > Çalışma alanı adı büyük/küçük harf duyarsız.

    Diğer hizmetlerin adları rasgele oluşturulur.

> [!TIP]
> Bu belgeyle ilişkili şablon yeni bir Azure Kapsayıcı Kayıt Defteri oluştururken, kapsayıcı kayıt defteri oluşturmadan yeni bir çalışma alanı da oluşturabilirsiniz. Bir kapsayıcı kayıt defteri gerektiren bir işlem gerçekleştirdiğinizde oluşturulur. Örneğin, bir modeli eğitmek veya dağıtmak.
>
> Yeni bir kutu oluşturmak yerine Azure Kaynak Yöneticisi şablonundaki varolan bir kapsayıcı kayıt defterine veya depolama hesabına da başvuruda bulunabilirsiniz.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

Şablonlar hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Kaynak Yöneticisi şablonlarını yazar](../azure-resource-manager/templates/template-syntax.md)
* [Azure Kaynak Yöneticisi şablonları ile bir uygulama dağıtma](../azure-resource-manager/templates/deploy-powershell.md)
* [Microsoft.MachineLearningServices kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

### <a name="advanced-template"></a>Gelişmiş şablon

Aşağıdaki örnek şablon, üç ayarı olan bir çalışma alanının nasıl oluşturultur olduğunu gösterir:

* Çalışma alanı için yüksek gizlilik ayarlarını etkinleştirme
* Çalışma alanı için şifrelemeyi etkinleştirme
* Varolan bir Azure Anahtar Kasası kullanır

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the Azure Machine Learning workspace."
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus",
      "allowedValues": [
        "eastus",
        "eastus2",
        "southcentralus",
        "southeastasia",
        "westcentralus",
        "westeurope",
        "westus2"
      ],
      "metadata": {
        "description": "Specifies the location for all resources."
      }
    },
    "sku":{
      "type": "string",
      "defaultValue": "basic",
      "allowedValues": [
        "basic",
        "enterprise"
      ],
      "metadata": {
        "description": "Specifies the sku, also referred to as 'edition' of the Azure Machine Learning workspace."
      }
    },
    "hbi_workspace":{
      "type": "string",
      "defaultValue": "false",
      "allowedValues": [
        "false",
        "true"
      ],
      "metadata": {
        "description": "Specifies that the Azure Machine Learning workspace holds highly confidential data."
      }
    },
    "encryption_status":{
      "type": "string",
      "defaultValue": "Disabled",
      "allowedValues": [
        "Enabled",
        "Disabled"
      ],
      "metadata": {
        "description": "Specifies if the Azure Machine Learning workspace should be encrypted with the customer managed key."
      }
    },
    "cmk_keyvault":{
      "type": "string",
      "metadata": {
        "description": "Specifies the customer managed keyvault Resource Manager ID."
      }
    },
    "resource_cmk_uri":{
      "type": "string",
      "metadata": {
        "description": "Specifies the customer managed keyvault key uri."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('sa',uniqueString(resourceGroup().id))]",
    "storageAccountType": "Standard_LRS",
    "keyVaultName": "[concat('kv',uniqueString(resourceGroup().id))]",
    "tenantId": "[subscription().tenantId]",
    "applicationInsightsName": "[concat('ai',uniqueString(resourceGroup().id))]",
    "containerRegistryName": "[concat('cr',uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2018-07-01",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[variables('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {
        "encryption": {
          "services": {
            "blob": {
              "enabled": true
            },
            "file": {
              "enabled": true
            }
          },
          "keySource": "Microsoft.Storage"
        },
        "supportsHttpsTrafficOnly": true
      }
    },
    {
      "type": "Microsoft.KeyVault/vaults",
      "apiVersion": "2018-02-14",
      "name": "[variables('keyVaultName')]",
      "location": "[parameters('location')]",
      "properties": {
        "tenantId": "[variables('tenantId')]",
        "sku": {
          "name": "standard",
          "family": "A"
        },
        "accessPolicies": []
      }
    },
    {
      "type": "Microsoft.Insights/components",
      "apiVersion": "2015-05-01",
      "name": "[variables('applicationInsightsName')]",
      "location": "[if(or(equals(parameters('location'),'eastus2'),equals(parameters('location'),'westcentralus')),'southcentralus',parameters('location'))]",
      "kind": "web",
      "properties": {
        "Application_Type": "web"
      }
    },
    {
      "type": "Microsoft.ContainerRegistry/registries",
      "apiVersion": "2017-10-01",
      "name": "[variables('containerRegistryName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard"
      },
      "properties": {
        "adminUserEnabled": true
      }
    },
    {
      "type": "Microsoft.MachineLearningServices/workspaces",
      "apiVersion": "2020-01-01",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.KeyVault/vaults', variables('keyVaultName'))]",
        "[resourceId('Microsoft.Insights/components', variables('applicationInsightsName'))]",
        "[resourceId('Microsoft.ContainerRegistry/registries', variables('containerRegistryName'))]"
      ],
      "identity": {
        "type": "systemAssigned"
      },
      "sku": {
            "tier": "[parameters('sku')]",
            "name": "[parameters('sku')]"
      },
      "properties": {
        "friendlyName": "[parameters('workspaceName')]",
        "keyVault": "[resourceId('Microsoft.KeyVault/vaults',variables('keyVaultName'))]",
        "applicationInsights": "[resourceId('Microsoft.Insights/components',variables('applicationInsightsName'))]",
        "containerRegistry": "[resourceId('Microsoft.ContainerRegistry/registries',variables('containerRegistryName'))]",
        "storageAccount": "[resourceId('Microsoft.Storage/storageAccounts/',variables('storageAccountName'))]",
         "encryption": {
                "status": "[parameters('encryption_status')]",
                "keyVaultProperties": {
                    "keyVaultArmId": "[parameters('cmk_keyvault')]",
                    "keyIdentifier": "[parameters('resource_cmk_uri')]"
                  }
            },
        "hbiWorkspace": "[parameters('hbi_workspace')]"
      }
    }
  ]
}
```

Anahtar Kasası'nın kimliğini ve bu şablonun gerektirdiği URI anahtarını almak için Azure CLI'yi kullanabilirsiniz. Aşağıdaki komut, Key Vault kaynak kimliğini ve URI'yi almak için Azure CLI'yi kullanmanın bir örneğidir:

```azurecli-interactive
az keyvault show --name mykeyvault --resource-group myresourcegroup --query "[id, properties.vaultUri]"
```

Bu komut, aşağıdaki metne benzer bir değer döndürür. İlk değer kimlik, ikincisi URI'dir:

```text
[
  "/subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault",
  "https://mykeyvault.vault.azure.net/"
]
```

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. [Özel şablondan kaynakları dağıt'taki](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template)adımları izleyin. __Şablonu Edit__ ekranına vardığınızda, şablona bu belgeden yapıştırın.
1. Şablonu kullanmak için __Kaydet'i__ seçin. Aşağıdaki bilgileri sağlayın ve listelenen hüküm ve koşulları kabul edin:

   * Abonelik: Bu kaynaklar için kullanılacak Azure aboneliğini seçin.
   * Kaynak grubu: Hizmetleri içerecek bir kaynak grubu seçin veya oluşturun.
   * Çalışma alanı adı: Oluşturulacak Azure Machine Learning çalışma alanı için kullanılacak ad. Çalışma alanı adı 3 ile 33 karakter arasında olmalıdır. Sadece alfasayısal karakterler ve '-' içerebilir.
   * Konum: Kaynakların oluşturulacağı konumu seçin.

Daha fazla bilgi için [bkz.](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Bu örnek, şablonu geçerli dizinde adı `azuredeploy.json` verilen bir dosyaya kaydettiğinizi varsayar:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace" -sku "basic"
```

Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure PowerShell ile kaynakları dağıt'a](../azure-resource-manager/templates/deploy-powershell.md) bakın ve [SAS jetonu ve Azure PowerShell ile özel Kaynak Yöneticisi şablonu dağıtın.](../azure-resource-manager/templates/secure-template-with-sas-token.md)

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Bu örnek, şablonu geçerli dizinde adı `azuredeploy.json` verilen bir dosyaya kaydettiğinizi varsayar:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace location=eastus sku=basic
```

Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure CLI ile kaynakları dağıt](../azure-resource-manager/templates/deploy-cli.md) ve [SAS jetonu ve Azure CLI ile özel Kaynak Yöneticisi şablonu dağıt'a](../azure-resource-manager/templates/secure-template-with-sas-token.md)bakın.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="resource-provider-errors"></a>Kaynak sağlayıcı hataları

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure Anahtar Kasası erişim ilkesi ve Azure Kaynak Yöneticisi şablonları

Çalışma alanını ve ilişkili kaynakları (Azure Anahtar Kasası dahil) oluşturmak için birden çok kez bir Azure Kaynak Yöneticisi şablonu kullandığınızda. Örneğin, şablonu sürekli tümleştirme ve dağıtım ardışık bir parçası olarak aynı parametrelere sahip birden çok kez kullanma.

Şablonlar aracılığıyla kaynak oluşturma işlemlerinin çoğu iktidara gelir, ancak Anahtar Atlama şablonu her kullanıldığında erişim ilkelerini temizler. Erişim ilkelerini temizlemek, onu kullanan varolan çalışma alanı için Anahtar Kasası'na erişimi bozar. Örneğin, Azure Not Defterleri VM'nin Durdur/Oluştur işlevleri başarısız olabilir.  

Bu sorunu önlemek için aşağıdaki yaklaşımlardan birini öneririz:

* Şablonu aynı parametreler için birden fazla kez dağıtmayın. Veya şablonu yeniden oluşturmak için kullanmadan önce varolan kaynakları silin.

* Anahtar Kasa erişim ilkelerini inceleyin ve şablonun `accessPolicies` özelliğini ayarlamak için bu ilkeleri kullanın. Erişim ilkelerini görüntülemek için aşağıdaki Azure CLI komutunu kullanın:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query properties.accessPolicies
    ```

    Şablonun `accessPolicies` bölümünü kullanma hakkında daha fazla bilgi için [AccessPolicyEntry nesne başvurusuna](https://docs.microsoft.com/azure/templates/Microsoft.KeyVault/2018-02-14/vaults#AccessPolicyEntry)bakın.

* Anahtar Kasa sı kaynağının zaten var olup olmadığını denetleyin. Varsa, şablon aracılığıyla yeniden oluşturmayın. Örneğin, yeni bir tane oluşturmak yerine varolan Anahtar Kasasını kullanmak için şablonda aşağıdaki değişiklikleri yapın:

    * Varolan bir Key Vault kaynağının kimliğini kabul eden bir parametre **ekleyin:**

        ```json
        "keyVaultId":{
          "type": "string",
          "metadata": {
            "description": "Specify the existing Key Vault ID."
          }
        }
      ```

    * Anahtar Kasası kaynağı oluşturan bölümü **kaldırın:**

        ```json
        {
          "type": "Microsoft.KeyVault/vaults",
          "apiVersion": "2018-02-14",
          "name": "[variables('keyVaultName')]",
          "location": "[parameters('location')]",
          "properties": {
            "tenantId": "[variables('tenantId')]",
            "sku": {
              "name": "standard",
              "family": "A"
            },
            "accessPolicies": [
            ]
          }
        },
        ```

    * Satırı çalışma alanı `dependsOn` nın bölümünden **kaldırın.** `"[resourceId('Microsoft.KeyVault/vaults', variables('keyVaultName'))]",` Ayrıca parametrebaşvuru `properties` için çalışma alanı bölümünde `keyVaultId` `keyVault` giriş **değiştirin:**

        ```json
        {
          "type": "Microsoft.MachineLearningServices/workspaces",
          "apiVersion": "2019-11-01",
          "name": "[parameters('workspaceName')]",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
            "[resourceId('Microsoft.Insights/components', variables('applicationInsightsName'))]"
          ],
          "identity": {
            "type": "systemAssigned"
          },
          "sku": {
            "tier": "[parameters('sku')]",
            "name": "[parameters('sku')]"
          },
          "properties": {
            "friendlyName": "[parameters('workspaceName')]",
            "keyVault": "[parameters('keyVaultId')]",
            "applicationInsights": "[resourceId('Microsoft.Insights/components',variables('applicationInsightsName'))]",
            "storageAccount": "[resourceId('Microsoft.Storage/storageAccounts/',variables('storageAccountName'))]"
          }
        }
        ```

    Bu değişikliklerden sonra, şablonu çalıştırırken varolan Key Vault kaynağının kimliğini belirtebilirsiniz. Şablon daha sonra çalışma alanının `keyVault` özelliğini kimliğine ayarlayarak Anahtar Kasası'nı yeniden kullanır.

    Anahtar Kasası'nın kimliğini almak için özgün şablon çalışmasının çıktısına başvurulayabilir veya Azure CLI'yi kullanabilirsiniz. Aşağıdaki komut, Anahtar Kasa kaynak kimliğini almak için Azure CLI'yi kullanmanın bir örneğidir:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query id
    ```

    Bu komut, aşağıdaki metne benzer bir değer döndürür:

    ```text
    /subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Yöneticisi şablonları ve Kaynak Yöneticisi REST API ile kaynakları dağıtma.](../azure-resource-manager/templates/deploy-rest.md)
* [Visual Studio aracılığıyla Azure kaynak grupları oluşturma ve dağıtma.](../azure-resource-manager/templates/create-visual-studio-deployment-project.md)
