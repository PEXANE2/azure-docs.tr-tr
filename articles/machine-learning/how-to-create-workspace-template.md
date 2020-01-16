---
title: Azure Resource Manager şablonuyla çalışma alanı oluşturma
titleSuffix: Azure Machine Learning
description: Yeni bir Azure Machine Learning çalışma alanı oluşturmak için Azure Resource Manager şablonu kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 11/04/2019
ms.custom: seoapril2019
ms.openlocfilehash: 6cd450ac18007e31d9d8144fdb0e8554dd31c363
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75968654"
---
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning"></a>Azure Machine Learning için bir çalışma alanı oluşturmak üzere Azure Resource Manager şablonu kullanma

Bu makalede, Azure Resource Manager şablonları kullanarak Azure Machine Learning çalışma alanı oluşturmanın birkaç yolunu öğreneceksiniz. Kaynak Yöneticisi şablonu, kaynakları tek ve eşgüdümlü bir işlem olarak oluşturmayı kolaylaştırır. Şablon, bir dağıtım için gereken kaynakları tanımlayan bir JSON belgesidir. Ayrıca, dağıtım parametrelerini de belirtebilir. Parametreler, şablon kullanılırken giriş değerleri sağlamak için kullanılır.

Daha fazla bilgi için bkz. [Azure Resource Manager şablonuyla uygulama dağıtma](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="prerequisites"></a>Ön koşullar

* Bir **Azure aboneliği**. Bir tane yoksa, [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* CLı 'dan bir şablon kullanmak için [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) ya da [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)gerekir.

## <a name="resource-manager-template"></a>Resource Manager şablonu

Aşağıdaki Kaynak Yöneticisi şablonu, bir Azure Machine Learning çalışma alanı ve ilişkili Azure kaynakları oluşturmak için kullanılabilir:

[!code-json[create-azure-machine-learning-service-workspace](~/quickstart-templates/101-machine-learning-create/azuredeploy.json)]

Bu şablon aşağıdaki Azure hizmetlerini oluşturur:

* Azure Kaynak Grubu
* Azure Depolama Hesabı
* Azure Key Vault
* Azure Application Insights
* Azure Container Registry
* Azure Machine Learning çalışma alanı

Kaynak grubu, Hizmetleri tutan kapsayıcıdır. Azure Machine Learning çalışma alanı için çeşitli hizmetler gereklidir.

Örnek şablonda iki parametre vardır:

* Kaynak grubunun ve hizmetlerin oluşturulacağı **konum** .

    Şablon, çoğu kaynak için seçtiğiniz konumu kullanacaktır. Özel durum, diğer hizmetlerin bulunduğu tüm konumlarda kullanılamayan Application Insights hizmetidir. Kullanılabilir olmayan bir konum seçerseniz, hizmet Orta Güney ABD konumunda oluşturulur.

* Azure Machine Learning çalışma alanının kolay adı olan **çalışma alanı adı**.

    Diğer hizmetlerin adları rastgele oluşturulur.

> [!TIP]
> Bu belgeyle ilişkili şablon yeni bir Azure Container Registry oluştururken, kapsayıcı kayıt defteri oluşturmadan de yeni bir çalışma alanı oluşturabilirsiniz. Çalışma alanında kapsayıcı kayıt defteri varsa, bir kapsayıcı kayıt defteri gerektiren bir işlem gerçekleştirdiğinizde bir tane oluşturulacaktır. Örneğin, bir modeli eğitme veya dağıtma.
>
> Ayrıca, yeni bir tane oluşturmak yerine Azure Resource Manager şablonundaki mevcut bir kapsayıcı kayıt defterine veya depolama hesabına başvurabilirsiniz.

Şablonlar hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Resource Manager şablonları yaz](../azure-resource-manager/templates/template-syntax.md)
* [Azure Resource Manager şablonlarıyla uygulama dağıtma](../azure-resource-manager/templates/deploy-powershell.md)
* [Microsoft. MachineLearningServices kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

## <a name="use-the-azure-portal"></a>Azure Portal’ı kullanma

1. [Özel şablondan kaynak dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template)bölümündeki adımları izleyin. __Şablonu Düzenle__ ekranına geldiğinizde, şablonu bu belgeden yapıştırın.
1. Şablonu kullanmak için __Kaydet__ ' i seçin. Aşağıdaki bilgileri sağlayın ve listelenen hüküm ve koşulları kabul edin:

   * Abonelik: Bu kaynaklar için kullanılacak Azure aboneliğini seçin.
   * Kaynak grubu: hizmetleri içeren bir kaynak grubu seçin veya oluşturun.
   * Çalışma alanı adı: oluşturulacak Azure Machine Learning çalışma alanı için kullanılacak ad. Çalışma alanı adı 3 ile 33 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve '-' içerebilir.
   * Konum: kaynakların oluşturulacağı konumu seçin.

Daha fazla bilgi için bkz. [özel şablondan kaynak dağıtma](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Azure PowerShell’i kullanma

Bu örnekte, şablonu geçerli dizinde `azuredeploy.json` adlı bir dosyaya kaydettiğiniz varsayılır:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace" -sku "basic"
```

Daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ile kaynak dağıtma ve Azure POWERSHELL](../azure-resource-manager/templates/deploy-powershell.md) [SAS belirteci ve Azure PowerShell ile özel kaynak yöneticisi şablonu dağıtma](../azure-resource-manager/templates/secure-template-with-sas-token.md).

## <a name="use-azure-cli"></a>Azure CLI kullanma

Bu örnekte, şablonu geçerli dizinde `azuredeploy.json` adlı bir dosyaya kaydettiğiniz varsayılır:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace location=eastus sku=basic
```

Daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../azure-resource-manager/templates/deploy-cli.md) ve [SAS BELIRTECI ve Azure clı ile özel kaynak yöneticisi şablonu dağıtma](../azure-resource-manager/templates/secure-template-with-sas-token.md).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="resource-provider-errors"></a>Kaynak sağlayıcısı hataları

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure Key Vault erişim ilkesi ve Azure Resource Manager şablonları

Çalışma alanını ve ilişkili kaynakları (Azure Key Vault dahil) oluşturmak için bir Azure Resource Manager şablonu kullandığınızda, birden çok kez. Örneğin, şablonu bir sürekli tümleştirme ve dağıtım işlem hattının bir parçası ile aynı parametrelerle birden çok kez kullanmak.

Şablonlar aracılığıyla kaynak oluşturma işlemlerinin çoğu ıdempotent, ancak Key Vault şablon her kullanıldığında erişim ilkelerini temizler. Erişim ilkelerinin temizlenmesi, onu kullanan var olan bir çalışma alanının Key Vault erişimini keser. Örneğin, Azure Notebooks VM 'yi durdur/oluştur işlevleri başarısız olabilir.  

Bu sorundan kaçınmak için aşağıdaki yaklaşımlardan birini öneririz:

* Aynı parametreler için şablonu birden çok kez dağıtmayın. Ya da yeniden oluşturmak için şablonu kullanmadan önce mevcut kaynakları silin.

* Key Vault erişim ilkelerini inceleyin ve sonra şablonun `accessPolicies` özelliğini ayarlamak için bu ilkeleri kullanın. Erişim ilkelerini görüntülemek için aşağıdaki Azure CLı komutunu kullanın:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query properties.accessPolicies
    ```

    Şablonun `accessPolicies` bölümünü kullanma hakkında daha fazla bilgi için [Accesspolicyentry nesne başvurusuna](https://docs.microsoft.com/azure/templates/Microsoft.KeyVault/2018-02-14/vaults#AccessPolicyEntry)bakın.

* Key Vault kaynağının zaten var olup olmadığını denetleyin. Varsa, şablon aracılığıyla yeniden oluşturmayın. Örneğin, yeni bir tane oluşturmak yerine varolan Key Vault kullanmak için, şablonda aşağıdaki değişiklikleri yapın:

    * Mevcut bir Key Vault kaynağının KIMLIĞINI kabul eden bir parametre **ekleyin** :

        ```json
        "keyVaultId":{
          "type": "string",
          "metadata": {
            "description": "Specify the existing Key Vault ID."
          }
        }
      ```

    * Key Vault kaynağı oluşturan bölümü **kaldırın** :

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

    * `"[resourceId('Microsoft.KeyVault/vaults', variables('keyVaultName'))]",` satırını, çalışma alanının `dependsOn` bölümünden **kaldırın** . Ayrıca, `keyVaultId` parametresine başvurmak için çalışma alanının `properties` bölümündeki `keyVault` girişini de **değiştirin** :

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

    Bu değişikliklerden sonra, şablonu çalıştırırken mevcut Key Vault kaynağının KIMLIĞINI belirtebilirsiniz. Şablon, çalışma alanının `keyVault` özelliğini KIMLIĞINE ayarlayarak Key Vault yeniden kullanır.

    Key Vault KIMLIĞINI almak için özgün şablon çalıştırmasının çıkışına başvurabilirsiniz veya Azure CLı 'yi kullanabilirsiniz. Aşağıdaki komut, Key Vault kaynak KIMLIĞINI almak için Azure CLı kullanma örneğidir:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query id
    ```

    Bu komut aşağıdaki metne benzer bir değer döndürür:

    ```text
    /subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Yöneticisi şablonları ve Kaynak Yöneticisi REST API ile kaynakları dağıtın](../azure-resource-manager/templates/deploy-rest.md).
* [Visual Studio aracılığıyla Azure Kaynak grupları oluşturma ve dağıtma](../azure-resource-manager/templates/create-visual-studio-deployment-project.md).
