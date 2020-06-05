---
title: Azure Resource Manager şablonuyla çalışma alanı oluşturma
titleSuffix: Azure Machine Learning
description: Yeni bir Azure Machine Learning çalışma alanı oluşturmak için Azure Resource Manager şablonu kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: larryfr
author: Blackmist
ms.date: 05/19/2020
ms.custom: seoapril2019
ms.openlocfilehash: 5280fe3c4a06479ae49c5064832aca6affd8167f
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84433499"
---
# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning"></a>Azure Machine Learning için bir çalışma alanı oluşturmak üzere Azure Resource Manager şablonu kullanma

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]
<br>

Bu makalede, Azure Resource Manager şablonları kullanarak Azure Machine Learning çalışma alanı oluşturmanın birkaç yolunu öğreneceksiniz. Kaynak Yöneticisi şablonu, kaynakları tek ve eşgüdümlü bir işlem olarak oluşturmayı kolaylaştırır. Şablon, bir dağıtım için gereken kaynakları tanımlayan bir JSON belgesidir. Ayrıca, dağıtım parametrelerini de belirtebilir. Parametreler, şablon kullanılırken giriş değerleri sağlamak için kullanılır.

Daha fazla bilgi için bkz. [Azure Resource Manager şablonuyla uygulama dağıtma](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="prerequisites"></a>Önkoşullar

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

    > [!NOTE]
    > Çalışma alanı adı büyük/küçük harfe duyarlıdır.

    Diğer hizmetlerin adları rastgele oluşturulur.

> [!TIP]
> Bu belgeyle ilişkili şablon yeni bir Azure Container Registry oluştururken, kapsayıcı kayıt defteri oluşturmadan de yeni bir çalışma alanı oluşturabilirsiniz. Bir kapsayıcı kayıt defteri gerektiren bir işlem gerçekleştirdiğinizde bu bir tane oluşturulur. Örneğin, bir modeli eğitme veya dağıtma.
>
> Ayrıca, yeni bir tane oluşturmak yerine Azure Resource Manager şablonundaki mevcut bir kapsayıcı kayıt defterine veya depolama hesabına başvurabilirsiniz.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

Şablonlar hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Resource Manager şablonları yaz](../azure-resource-manager/templates/template-syntax.md)
* [Azure Resource Manager şablonlarıyla uygulama dağıtma](../azure-resource-manager/templates/deploy-powershell.md)
* [Microsoft. MachineLearningServices kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

### <a name="advanced-template"></a>Gelişmiş şablon

Aşağıdaki örnek şablon, üç ayarı olan bir çalışma alanının nasıl oluşturulacağını göstermektedir:

* Çalışma alanı için yüksek gizlilik ayarlarını etkinleştir
* Çalışma alanı için şifrelemeyi etkinleştir
* Müşteri tarafından yönetilen anahtarları almak için mevcut bir Azure Key Vault kullanır

Daha fazla bilgi için bkz. [bekleyen şifreleme](concept-enterprise-security.md#encryption-at-rest).

> [!IMPORTANT]
> Bu şablonu kullanmadan önce aboneliğinizin karşılaması gereken bazı özel gereksinimler vardır:
> * __Azure Machine Learning__ uygulaması Azure aboneliğiniz için __katkıda bulunan__ olmalıdır.
> * Şifreleme anahtarı içeren bir Azure Key Vault var olmalıdır.
> * __Azure Cosmos DB__ uygulamasına __Get__, __Wrap__ve __sarmalama__ erişimini sağlayan Azure Key Vault bir erişim ilkeniz olması gerekir.
> * Azure Key Vault, Azure Machine Learning çalışma alanını oluşturmayı planladığınız bölgede olmalıdır.

__Azure Machine Learning uygulamayı katkıda bulunan olarak eklemek için__aşağıdaki komutları kullanın:

1. CLı 'dan Azure 'da kimlik doğrulaması yapmak için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az login
    ```
    
    [!INCLUDE [subscription-login](../../includes/machine-learning-cli-subscription.md)]

1. Azure Machine Learning uygulamasının nesne KIMLIĞINI almak için aşağıdaki komutu kullanın. Değer, Azure aboneliklerinizin her biri için farklı olabilir:

    ```azurecli-interactive
    az ad sp list --display-name "Azure Machine Learning" --query '[].[appDisplayName,objectId]' --output tsv
    ```

    Bu komut, GUID olan nesne KIMLIĞINI döndürür.

1. Aboneliğinize katkıda bulunan nesne KIMLIĞINI eklemek için aşağıdaki komutu kullanın. `<object-ID>`Önceki ADıMDAN GUID ile değiştirin. `<subscription-ID>`Azure aboneliğinizin adı veya kimliğiyle değiştirin:

    ```azurecli-interactive
    az role assignment create --role 'Contributor' --assignee-object-id <object-ID> --subscription <subscription-ID>
    ```

__Azure Key Vault bir anahtar eklemek Için__ __Azure CLI kullanarak Key Vault yönetme__ makalesindeki anahtar [Kasası ' nda anahtar, gizli dizi veya sertifika ekleme](../key-vault/general/manage-with-cli2.md#adding-a-key-secret-or-certificate-to-the-key-vault) bölümündeki bilgileri kullanın.

__Anahtar kasasına bir erişim ilkesi eklemek için aşağıdaki komutları kullanın__:

1. Azure Cosmos DB uygulamasının nesne KIMLIĞINI almak için aşağıdaki komutu kullanın. Değer, Azure aboneliklerinizin her biri için farklı olabilir:

    ```azurecli-interactive
    az ad sp list --display-name "Azure Cosmos DB" --query '[].[appDisplayName,objectId]' --output tsv
    ```
    
    Bu komut, GUID olan nesne KIMLIĞINI döndürür.

1. İlkeyi ayarlamak için aşağıdaki komutu kullanın. `<keyvault-name>`Varolan Azure Key Vault adıyla değiştirin. `<object-ID>`Önceki ADıMDAN GUID ile değiştirin:

    ```azurecli-interactive
    az keyvault set-policy --name <keyvault-name> --object-id <object-ID> --key-permissions get unwrapKey wrapKey
    ```

__To get the values__ `cmk_keyvault` Bu şablon için gereken (Key Vault kimliği) ve `resource_cmk_uri` (anahtar URI) parametrelerinin değerlerini almak için aşağıdaki adımları kullanın:

1. Key Vault KIMLIĞI almak için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query "id"
    ```

    Bu komut şuna benzer bir değer döndürür `/subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault` .

1. Müşteri tarafından yönetilen anahtar için URI değerini almak üzere aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az keyvault key show --vault-name mykeyvault --name mykey --query "key.kid"
    ```

    Bu komut şuna benzer bir değer döndürür `https://mykeyvault.vault.azure.net/keys/mykey/{guid}` .

__Örnek şablon__

:::code language="json" source="~/quickstart-templates/201-machine-learning-encrypted-workspace/azuredeploy.json":::

> [!IMPORTANT]
> Bir çalışma alanı oluşturulduktan sonra gizli verilerin, şifrelemenin, Anahtar Kasası KIMLIĞININ veya anahtar tanımlayıcılarının ayarlarını değiştiremezsiniz. Bu değerleri değiştirmek için yeni değerleri kullanarak yeni bir çalışma alanı oluşturmanız gerekir.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. [Özel şablondan kaynak dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template)bölümündeki adımları izleyin. __Şablonu Düzenle__ ekranına geldiğinizde, şablonu bu belgeden yapıştırın.
1. Şablonu kullanmak için __Kaydet__ ' i seçin. Aşağıdaki bilgileri sağlayın ve listelenen hüküm ve koşulları kabul edin:

   * Abonelik: Bu kaynaklar için kullanılacak Azure aboneliğini seçin.
   * Kaynak grubu: hizmetleri içeren bir kaynak grubu seçin veya oluşturun.
   * Çalışma alanı adı: oluşturulacak Azure Machine Learning çalışma alanı için kullanılacak ad. Çalışma alanı adı 3 ile 33 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve '-' içerebilir.
   * Konum: kaynakların oluşturulacağı konumu seçin.

Daha fazla bilgi için bkz. [özel şablondan kaynak dağıtma](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Bu örnekte, şablonu geçerli dizinde adlı bir dosyaya kaydettiğiniz varsayılır `azuredeploy.json` :

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace" -sku "basic"
```

Daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ile kaynak dağıtma ve Azure POWERSHELL](../azure-resource-manager/templates/deploy-powershell.md) [SAS belirteci ve Azure PowerShell ile özel kaynak yöneticisi şablonu dağıtma](../azure-resource-manager/templates/secure-template-with-sas-token.md).

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Bu örnekte, şablonu geçerli dizinde adlı bir dosyaya kaydettiğiniz varsayılır `azuredeploy.json` :

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

* Key Vault erişim ilkelerini inceleyin ve sonra şablonun özelliğini ayarlamak için bu ilkeleri kullanın `accessPolicies` . Erişim ilkelerini görüntülemek için aşağıdaki Azure CLı komutunu kullanın:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query properties.accessPolicies
    ```

    Şablonun bölümünü kullanma hakkında daha fazla bilgi için `accessPolicies` [Accesspolicyentry nesne başvurusuna](https://docs.microsoft.com/azure/templates/Microsoft.KeyVault/2018-02-14/vaults#AccessPolicyEntry)bakın.

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

    * **Remove** `"[resourceId('Microsoft.KeyVault/vaults', variables('keyVaultName'))]",` `dependsOn` Çalışma alanının bölümündeki satırı kaldırın. Ayrıca **Change** , `keyVault` `properties` çalışma alanının bölümündeki girişi parametreye başvuracak şekilde değiştirin `keyVaultId` :

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

    Bu değişikliklerden sonra, şablonu çalıştırırken mevcut Key Vault kaynağının KIMLIĞINI belirtebilirsiniz. Daha sonra şablon, `keyVault` çalışma alanının ÖZELLIĞINI kimliğine ayarlayarak Key Vault yeniden kullanacaktır.

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
