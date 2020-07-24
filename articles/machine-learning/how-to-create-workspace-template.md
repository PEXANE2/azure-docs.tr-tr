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
ms.date: 07/09/2020
ms.custom: seoapril2019
ms.openlocfilehash: 49a1b190ece4ae4e937757e88af325a29f4825c5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87031125"
---
# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning"></a>Azure Machine Learning için bir çalışma alanı oluşturmak üzere Azure Resource Manager şablonu kullanma

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]
<br>

Bu makalede, Azure Resource Manager şablonları kullanarak Azure Machine Learning çalışma alanı oluşturmanın birkaç yolunu öğreneceksiniz. Kaynak Yöneticisi şablonu, kaynakları tek ve eşgüdümlü bir işlem olarak oluşturmayı kolaylaştırır. Şablon, bir dağıtım için gereken kaynakları tanımlayan bir JSON belgesidir. Ayrıca, dağıtım parametrelerini de belirtebilir. Parametreler, şablon kullanılırken giriş değerleri sağlamak için kullanılır.

Daha fazla bilgi için bkz. [Azure Resource Manager şablonuyla uygulama dağıtma](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="prerequisites"></a>Önkoşullar

* Bir **Azure aboneliği**. Bir tane yoksa, [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

* CLı 'dan bir şablon kullanmak için [Azure PowerShell](https://docs.microsoft.com/powershell/azure/?view=azps-1.2.0) ya da [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)gerekir.

## <a name="workspace-resource-manager-template"></a>Çalışma alanı Kaynak Yöneticisi şablonu

Bu belge boyunca kullanılan Azure Resource Manager şablonu, Azure hızlı başlangıç şablonları GitHub deposunun [201-Machine-Learning-Advanced](https://github.com/Azure/azure-quickstart-templates/blob/master/201-machine-learning-advanced/azuredeploy.json) dizininde bulunabilir.

Bu şablon aşağıdaki Azure hizmetlerini oluşturur:

* Azure Depolama Hesabı
* Azure Key Vault
* Azure Application Insights
* Azure Container Registry
* Azure Machine Learning çalışma alanı

Kaynak grubu, Hizmetleri tutan kapsayıcıdır. Azure Machine Learning çalışma alanı için çeşitli hizmetler gereklidir.

Örnek şablonda iki **gerekli** parametre vardır:

* Kaynakların oluşturulacağı **konum** .

    Şablon, çoğu kaynak için seçtiğiniz konumu kullanacaktır. Özel durum, diğer hizmetlerin bulunduğu tüm konumlarda kullanılamayan Application Insights hizmetidir. Kullanılabilir olmayan bir konum seçerseniz, hizmet Orta Güney ABD konumunda oluşturulur.

* Azure Machine Learning çalışma alanının kolay adı olan **Çalışmaalanıadı**.

    > [!NOTE]
    > Çalışma alanı adı büyük/küçük harfe duyarlıdır.

    Diğer hizmetlerin adları rastgele oluşturulur.

> [!TIP]
> Bu belgeyle ilişkili şablon yeni bir Azure Container Registry oluştururken, kapsayıcı kayıt defteri oluşturmadan de yeni bir çalışma alanı oluşturabilirsiniz. Bir kapsayıcı kayıt defteri gerektiren bir işlem gerçekleştirdiğinizde bu bir tane oluşturulur. Örneğin, bir modeli eğitme veya dağıtma.
>
> Ayrıca, yeni bir tane oluşturmak yerine Azure Resource Manager şablonundaki mevcut bir kapsayıcı kayıt defterine veya depolama hesabına başvurabilirsiniz. Ancak, kullandığınız kapsayıcı kayıt defterinde __yönetici hesabının__ etkinleştirilmiş olması gerekir. Yönetici hesabını etkinleştirme hakkında daha fazla bilgi için bkz. [yönetici hesabı](/azure/container-registry/container-registry-authentication#admin-account).

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

Şablonlar hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Resource Manager şablonları yaz](../azure-resource-manager/templates/template-syntax.md)
* [Azure Resource Manager şablonlarıyla uygulama dağıtma](../azure-resource-manager/templates/deploy-powershell.md)
* [Microsoft. MachineLearningServices kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

## <a name="deploy-template"></a>Şablon dağıtma

Şablonunuzu dağıtmak için bir kaynak grubu oluşturmanız gerekir.

Grafik Kullanıcı arabirimini kullanmayı tercih ediyorsanız [Azure Portal](#use-the-azure-portal) bölümüne bakın.

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

```azurecli
az group create --name "examplegroup" --location "eastus"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

```azurepowershell
New-AzResourceGroup -Name "examplegroup" -Location "eastus"
```

---

Kaynak grubunuz başarıyla oluşturulduktan sonra, aşağıdaki komutla şablonu dağıtın:

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

```azurecli
az deployment group create \
    --name "exampledeployment" \
    --resource-group "examplegroup" \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" \
    --parameters workspaceName="exampleworkspace" location="eastus"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name "exampledeployment" `
  -ResourceGroupName "examplegroup" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
  -workspaceName "exampleworkspace" `
  -location "eastus"
```

---

Varsayılan olarak, şablonun bir parçası olarak oluşturulan tüm kaynaklar yenidir. Ancak, var olan kaynakları kullanma seçeneğiniz de vardır. Şablona ek parametreler sağlayarak, mevcut kaynakları kullanabilirsiniz. Örneğin, var olan bir depolama hesabını kullanmak istiyorsanız **Storageaccountoption** değerini **mevcut** olarak ayarlayın ve depolama hesabınızın adını **storageAccountName** parametresinde belirtin.

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

```azurecli
az deployment group create \
    --name "exampledeployment" \
    --resource-group "examplegroup" \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" \
    --parameters workspaceName="exampleworkspace" \
      location="eastus" \
      storageAccountOption="existing" \
      storageAccountName="existingstorageaccountname"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name "exampledeployment" `
  -ResourceGroupName "examplegroup" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
  -workspaceName "exampleworkspace" `
  -location "eastus" `
  -storageAccountOption "existing" `
  -storageAccountName "existingstorageaccountname"
```

---

## <a name="deploy-an-encrypted-workspace"></a>Şifrelenmiş bir çalışma alanı dağıtma

Aşağıdaki örnek şablon, üç ayarı olan bir çalışma alanının nasıl oluşturulacağını göstermektedir:

* Çalışma alanı için yüksek gizlilik ayarlarını etkinleştir
* Çalışma alanı için şifrelemeyi etkinleştir
* Müşteri tarafından yönetilen anahtarları almak için mevcut bir Azure Key Vault kullanır

> [!IMPORTANT]
> Bir çalışma alanı oluşturulduktan sonra gizli verilerin, şifrelemenin, Anahtar Kasası KIMLIĞININ veya anahtar tanımlayıcılarının ayarlarını değiştiremezsiniz. Bu değerleri değiştirmek için yeni değerleri kullanarak yeni bir çalışma alanı oluşturmanız gerekir.

Daha fazla bilgi için bkz. [bekleyen şifreleme](concept-enterprise-security.md#encryption-at-rest).

> [!IMPORTANT]
> Bu şablonu kullanmadan önce aboneliğinizin karşılaması gereken bazı özel gereksinimler vardır:
>
> * __Azure Machine Learning__ uygulaması Azure aboneliğiniz için __katkıda bulunan__ olmalıdır.
> * Şifreleme anahtarı içeren bir Azure Key Vault var olmalıdır.
> * __Azure Cosmos DB__ uygulamasına __Get__, __Wrap__ve __sarmalama__ erişimini sağlayan Azure Key Vault bir erişim ilkeniz olması gerekir.
> * Azure Key Vault, Azure Machine Learning çalışma alanını oluşturmayı planladığınız bölgede olmalıdır.

__Azure Machine Learning uygulamayı katkıda bulunan olarak eklemek için__aşağıdaki komutları kullanın:

1. Azure hesabınızda oturum açın ve abonelik KIMLIĞINIZI alın. Bu abonelik, Azure Machine Learning çalışma alanınızı içeren bir aynı olmalıdır.  

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az account list --query '[].[name,id]' --output tsv
    ```

    > [!TIP]
    > Başka bir abonelik seçmek için komutunu kullanın `az account set -s <subscription name or ID>` ve geçiş yapılacak abonelik adını veya kimliğini belirtin. Abonelik seçimi hakkında daha fazla bilgi için bkz. [birden çok Azure aboneliği kullanma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). 

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

    ```azurepowershell
    Get-AzSubscription
    ```

    > [!TIP]
    > Başka bir abonelik seçmek için komutunu kullanın `Az-SetContext -SubscriptionId <subscription ID>` ve geçiş yapılacak abonelik adını veya kimliğini belirtin. Abonelik seçimi hakkında daha fazla bilgi için bkz. [birden çok Azure aboneliği kullanma](https://docs.microsoft.com/powershell/azure/manage-subscriptions-azureps?view=azps-4.3.0).

    ---

1. Azure Machine Learning uygulamasının nesne KIMLIĞINI almak için aşağıdaki komutu kullanın. Değer, Azure aboneliklerinizin her biri için farklı olabilir:

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az ad sp list --display-name "Azure Machine Learning" --query '[].[appDisplayName,objectId]' --output tsv
    ```

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

    ```azurepowershell
    Get-AzADServicePrincipal --DisplayName "Azure Machine Learning" | select-object DisplayName, Id
    ```

    ---
    Bu komut, GUID olan nesne KIMLIĞINI döndürür.

1. Aboneliğinize katkıda bulunan nesne KIMLIĞINI eklemek için aşağıdaki komutu kullanın. `<object-ID>`Hizmet sorumlusunun nesne kimliğiyle değiştirin. `<subscription-ID>`Azure aboneliğinizin adı veya kimliğiyle değiştirin:

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az role assignment create --role 'Contributor' --assignee-object-id <object-ID> --subscription <subscription-ID>
    ```

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

    ```azurepowershell
    New-AzRoleAssignment --ObjectId <object-ID> --RoleDefinitionName "Contributor" -Scope /subscriptions/<subscription-ID>
    ```

    ---

1. Mevcut bir Azure Key Vault bir anahtar oluşturmak için aşağıdaki komutlardan birini kullanın. `<keyvault-name>`Anahtar kasasının adıyla değiştirin. `<key-name>`Anahtar için kullanılacak adla değiştirin:

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az keyvault key create --vault-name <keyvault-name> --name <key-name> --protection software
    ```

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

    ```azurepowershell
    Add-AzKeyVaultKey -VaultName <keyvault-name> -Name <key-name> -Destination 'Software'
    ```
    --- 

__Anahtar kasasına bir erişim ilkesi eklemek için aşağıdaki komutları kullanın__:

1. Azure Cosmos DB uygulamasının nesne KIMLIĞINI almak için aşağıdaki komutu kullanın. Değer, Azure aboneliklerinizin her biri için farklı olabilir:

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az ad sp list --display-name "Azure Cosmos DB" --query '[].[appDisplayName,objectId]' --output tsv
    ```

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

    ```azurepowershell
    Get-AzADServicePrincipal --DisplayName "Azure Cosmos DB" | select-object DisplayName, Id
    ```
    ---

    Bu komut, GUID olan nesne KIMLIĞINI döndürür. Daha sonra Kaydet

1. İlkeyi ayarlamak için aşağıdaki komutu kullanın. `<keyvault-name>`Varolan Azure Key Vault adıyla değiştirin. `<object-ID>`Önceki ADıMDAN GUID ile değiştirin:

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az keyvault set-policy --name <keyvault-name> --object-id <object-ID> --key-permissions get unwrapKey wrapKey
    ```

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)
    
    ```azurepowershell
    Set-AzKeyVaultAccessPolicy -VaultName <keyvault-name> -ObjectId <object-ID> -PermissionsToKeys get, unwrapKey, wrapKey
    ```
    ---    

__To get the values__ `cmk_keyvault` Bu şablon için gereken (Key Vault kimliği) ve `resource_cmk_uri` (anahtar URI) parametrelerinin değerlerini almak için aşağıdaki adımları kullanın:

1. Key Vault KIMLIĞI almak için aşağıdaki komutu kullanın:

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az keyvault show --name <keyvault-name> --query 'id' --output tsv
    ```

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

    ```azurepowershell
    Get-AzureRMKeyVault -VaultName '<keyvault-name>'
    ```
    ---

    Bu komut şuna benzer bir değer döndürür `/subscriptions/{subscription-guid}/resourceGroups/<resource-group-name>/providers/Microsoft.KeyVault/vaults/<keyvault-name>` .

1. Müşteri tarafından yönetilen anahtar için URI değerini almak üzere aşağıdaki komutu kullanın:

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az keyvault key show --vault-name <keyvault-name> --name <key-name> --query 'key.kid' --output tsv
    ```

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

    ```azurepowershell
    Get-AzureKeyVaultKey -VaultName '<keyvault-name>' -KeyName '<key-name>'
    ```
    ---

    Bu komut şuna benzer bir değer döndürür `https://mykeyvault.vault.azure.net/keys/mykey/{guid}` .

> [!IMPORTANT]
> Bir çalışma alanı oluşturulduktan sonra gizli verilerin, şifrelemenin, Anahtar Kasası KIMLIĞININ veya anahtar tanımlayıcılarının ayarlarını değiştiremezsiniz. Bu değerleri değiştirmek için yeni değerleri kullanarak yeni bir çalışma alanı oluşturmanız gerekir.

Yukarıdaki adımları başarılı bir şekilde tamamladıktan sonra, şablonunuzu normalde yaptığınız gibi dağıtın. Müşteri tarafından yönetilen anahtarların kullanımını etkinleştirmek için aşağıdaki parametreleri ayarlayın:

* **Encryption_status** **etkin**.
* **cmk_keyvault** `cmk_keyvault` önceki adımlarda elde edilen değere cmk_keyvault.
* **resource_cmk_uri** `resource_cmk_uri` önceki adımlarda elde edilen değere resource_cmk_uri.

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

```azurecli
az deployment group create \
    --name "exampledeployment" \
    --resource-group "examplegroup" \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" \
    --parameters workspaceName="exampleworkspace" \
      location="eastus" \
      encryption_status="Enabled" \
      cmk_keyvault="/subscriptions/{subscription-guid}/resourceGroups/<resource-group-name>/providers/Microsoft.KeyVault/vaults/<keyvault-name>" \
      resource_cmk_uri="https://mykeyvault.vault.azure.net/keys/mykey/{guid}" \
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name "exampledeployment" `
  -ResourceGroupName "examplegroup" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
  -workspaceName "exampleworkspace" `
  -location "eastus" `
  -encryption_status "Enabled" `
  -cmk_keyvault "/subscriptions/{subscription-guid}/resourceGroups/<resource-group-name>/providers/Microsoft.KeyVault/vaults/<keyvault-name>" `
  -resource_cmk_uri "https://mykeyvault.vault.azure.net/keys/mykey/{guid}"
```
---

Müşteri tarafından yönetilen bir anahtar kullanırken Azure Machine Learning, Cosmos DB örneğini içeren ikincil bir kaynak grubu oluşturur. Daha fazla bilgi için bkz. [rest-Cosmos DB şifreleme](concept-enterprise-security.md#encryption-at-rest).

Verileriniz için sağlayabilmeniz için ek bir yapılandırma **confidential_data** parametresini **true**olarak ayarlayasağlamaktır. Bunu yaptığınızda, şunları yapar:

* , Aboneliğinizde daha önceki kümelerin oluşturulmadığından Azure Machine Learning işlem kümeleri için yerel karalama diskini şifrelemeye başlar. Abonelikte daha önce bir küme oluşturduysanız, işlem kümelerinizde etkin olan boş disk şifrelemesini sağlamak için bir destek bileti açın.
* Çalıştırmalar arasındaki yerel karalama diskini temizler.
* Anahtar Kasası 'nı kullanarak depolama hesabı, kapsayıcı kayıt defteri ve SSH hesabı için kimlik bilgilerini yürütme katmanından işlem kümelerinize güvenli bir şekilde geçirir.
* Temel alınan toplu iş havuzlarının AzureMachineLearningService dışında herhangi bir dış hizmet tarafından çağrılıp çağrılmaması için IP filtrelemeyi sağlar.

    > [!IMPORTANT]
    > Bir çalışma alanı oluşturulduktan sonra gizli verilerin, şifrelemenin, Anahtar Kasası KIMLIĞININ veya anahtar tanımlayıcılarının ayarlarını değiştiremezsiniz. Bu değerleri değiştirmek için yeni değerleri kullanarak yeni bir çalışma alanı oluşturmanız gerekir.

  Daha fazla bilgi için bkz. [bekleyen şifreleme](concept-enterprise-security.md#encryption-at-rest).

## <a name="deploy-workspace-behind-a-virtual-network"></a>Çalışma alanını bir sanal ağın arkasına dağıtma

`vnetOption`Parametre değerini ya da olarak ayarlayarak `new` , bir `existing` sanal ağın arkasındaki bir çalışma alanı tarafından kullanılan kaynakları oluşturabilirsiniz.

> [!IMPORTANT]
> Kapsayıcı kayıt defteri için yalnızca ' Premium ' SKU desteklenir.

> [!IMPORTANT]
> Application Insights, bir sanal ağın arkasında dağıtımı desteklemez.

### <a name="only-deploy-workspace-behind-private-endpoint"></a>Çalışma alanını yalnızca özel uç nokta arkasında dağıt

İlişkili kaynaklarınız bir sanal ağın arkasında değilse, **privateEndpointType** `AutoAproval` `ManualApproval` çalışma alanını özel bir uç noktanın arkasında dağıtmak için privateendpointtype parametresini veya olarak ayarlayabilirsiniz.

> [!IMPORTANT]
> Dağıtım yalnızca özel uç noktaları destekleyen bölgelerde geçerlidir.

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

```azurecli
az deployment group create \
    --name "exampledeployment" \
    --resource-group "examplegroup" \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" \
    --parameters workspaceName="exampleworkspace" \
      location="eastus" \
      privateEndpointType="AutoApproval"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name "exampledeployment" `
  -ResourceGroupName "examplegroup" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
  -workspaceName "exampleworkspace" `
  -location "eastus" `
  -privateEndpointType "AutoApproval"
```

---

### <a name="use-a-new-virtual-network"></a>Yeni bir sanal ağ kullan

Yeni bir sanal ağın arkasındaki bir kaynağı dağıtmak için, **Vnetoption** 'yi ilgili kaynak için sanal ağ ayarlarıyla birlikte **Yeni** olarak ayarlayın. Aşağıdaki dağıtımda, depolama hesabı kaynağıyla yeni bir sanal ağın arkasındaki bir çalışma alanının nasıl dağıtılacağı gösterilmektedir.

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

```azurecli
az deployment group create \
    --name "exampledeployment" \
    --resource-group "examplegroup" \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" \
    --parameters workspaceName="exampleworkspace" \
      location="eastus" \
      vnetOption="new" \
      vnetName="examplevnet" \
      storageAccountBehindVNet="true"
      privateEndpointType="AutoApproval"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name "exampledeployment" `
  -ResourceGroupName "examplegroup" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
  -workspaceName "exampleworkspace" `
  -location "eastus" `
  -vnetOption "new" `
  -vnetName "examplevnet" `
  -storageAccountBehindVNet "true"
  -privateEndpointType "AutoApproval"
```

---

Alternatif olarak, bir sanal ağın arkasında bir veya daha fazla bağımlı kaynak dağıtabilirsiniz.

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

```azurecli
az deployment group create \
    --name "exampledeployment" \
    --resource-group "examplegroup" \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" \
    --parameters workspaceName="exampleworkspace" \
      location="eastus" \
      vnetOption="new" \
      vnetName="examplevnet" \
      storageAccountBehindVNet="true" \
      keyVaultBehindVNet="true" \
      containerRegistryBehindVNet="true" \
      containerRegistryOption="new" \
      containerRegistrySku="Premium"
      privateEndpointType="AutoApproval"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name "exampledeployment" `
  -ResourceGroupName "examplegroup" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
  -workspaceName "exampleworkspace" `
  -location "eastus" `
  -vnetOption "new" `
  -vnetName "examplevnet" `
  -storageAccountBehindVNet "true"
  -keyVaultBehindVNet "true" `
  -containerRegistryBehindVNet "true" `
  -containerRegistryOption "new" `
  -containerRegistrySku "Premium"
  -privateEndpointType "AutoApproval"
```

---

<!-- Workspaces need a private endpoint when associated resources are behind a virtual network to work properly. To set up a private endpoint for the workspace with a new virtual network:

> [!IMPORTANT]
> The deployment is only valid in regions which support private endpoints.

# [Azure CLI](#tab/azcli)

```azurecli
az deployment group create \
    --name "exampledeployment" \
    --resource-group "examplegroup" \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" \
    --parameters workspaceName="exampleworkspace" \
      location="eastus" \
      vnetOption="new" \
      vnetName="examplevnet" \
      privateEndpointType="AutoApproval"
```

# [Azure PowerShell](#tab/azpowershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name "exampledeployment" `
  -ResourceGroupName "examplegroup" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
  -workspaceName "exampleworkspace" `
  -location "eastus" `
  -vnetOption "new" `
  -vnetName "examplevnet" `
  -privateEndpointType "AutoApproval"
```

--- -->

### <a name="use-an-existing-virtual-network--resources"></a>Mevcut bir sanal ağ & kaynaklarını kullan

Mevcut ilişkili kaynaklarla bir çalışma alanı dağıtmak için **Vnetoption** parametresini alt ağ parametreleriyle birlikte **var** olarak ayarlamanız gerekir. Ancak, dağıtımdan **önce** her bir kaynak için sanal ağda hizmet uç noktaları oluşturmanız gerekir. Yeni sanal ağ dağıtımları gibi, kaynaklarınızın bir sanal ağın arkasında veya tümü olabilir.

> [!IMPORTANT]
> Alt ağda `Microsoft.Storage` hizmet uç noktası olmalıdır

> [!IMPORTANT]
> Alt ağlar özel uç noktaların oluşturulmasına izin vermez. Alt ağı etkinleştirmek için özel uç noktayı devre dışı bırakın.

1. Kaynaklar için hizmet uç noktalarını etkinleştirin.

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az network vnet subnet update --resource-group "examplegroup" --vnet-name "examplevnet" --name "examplesubnet" --service-endpoints "Microsoft.Storage"
    az network vnet subnet update --resource-group "examplegroup" --vnet-name "examplevnet" --name "examplesubnet" --service-endpoints "Microsoft.KeyVault"
    az network vnet subnet update --resource-group "examplegroup" --vnet-name "examplevnet" --name "examplesubnet" --service-endpoints "Microsoft.ContainerRegistry"
    ```

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)

    ```azurepowershell
    Get-AzVirtualNetwork -ResourceGroupName "examplegroup" -Name "examplevnet" | Set-AzVirtualNetworkSubnetConfig -Name "examplesubnet" -AddressPrefix "<subnet prefix>" -ServiceEndpoint "Microsoft.Storage" | Set-AzVirtualNetwork
    Get-AzVirtualNetwork -ResourceGroupName "examplegroup" -Name "examplevnet" | Set-AzVirtualNetworkSubnetConfig -Name "examplesubnet" -AddressPrefix "<subnet prefix>" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzVirtualNetwork
    Get-AzVirtualNetwork -ResourceGroupName "examplegroup" -Name "examplevnet" | Set-AzVirtualNetworkSubnetConfig -Name "examplesubnet" -AddressPrefix "<subnet prefix>" -ServiceEndpoint "Microsoft.ContainerRegistry" | Set-AzVirtualNetwork
    ```

    ---

1. Çalışma alanını dağıtma

    # <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

    ```azurecli
    az deployment group create \
    --name "exampledeployment" \
    --resource-group "examplegroup" \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" \
    --parameters workspaceName="exampleworkspace" \
      location="eastus" \
      vnetOption="existing" \
      vnetName="examplevnet" \
      vnetResourceGroupName="examplegroup" \
      storageAccountBehindVNet="true" \
      keyVaultBehindVNet="true" \
      containerRegistryBehindVNet="true" \
      containerRegistryOption="new" \
      containerRegistrySku="Premium" \
      subnetName="examplesubnet" \
      subnetOption="existing"
      privateEndpointType="AutoApproval"
    ```

    # <a name="azure-powershell"></a>[Azure PowerShell](#tab/azpowershell)
    ```azurepowershell
    New-AzResourceGroupDeployment `
      -Name "exampledeployment" `
      -ResourceGroupName "examplegroup" `
      -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
      -workspaceName "exampleworkspace" `
      -location "eastus" `
      -vnetOption "existing" `
      -vnetName "examplevnet" `
      -vnetResourceGroupName "examplegroup" `
      -storageAccountBehindVNet "true"
      -keyVaultBehindVNet "true" `
      -containerRegistryBehindVNet "true" `
      -containerRegistryOption "new" `
      -containerRegistrySku "Premium" `
      -subnetName "examplesubnet" `
      -subnetOption "existing"
      -privateEndpointType "AutoApproval"
    ```
    ---

<!-- Workspaces need a private endpoint when associated resources are behind a virtual network to work properly. To set up a private endpoint for the workspace with an existing virtual network:

> [!IMPORTANT]
> The deployment is only valid in regions which support private endpoints.

# [Azure CLI](#tab/azcli)

```azurecli
az deployment group create \
    --name "exampledeployment" \
    --resource-group "examplegroup" \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" \
    --parameters workspaceName="exampleworkspace" \
      location="eastus" \
      vnetOption="existing" \
      vnetName="examplevnet" \
      vnetResourceGroupName="rg" \
      privateEndpointType="AutoApproval" \
      subnetName="subnet" \
      subnetOption="existing"
```

# [Azure PowerShell](#tab/azpowershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name "exampledeployment" `
  -ResourceGroupName "examplegroup" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
  -workspaceName "exampleworkspace" `
  -location "eastus" `
  -vnetOption "existing" `
  -vnetName "examplevnet" `
  -vnetResourceGroupName "rg"
  -privateEndpointType "AutoApproval"
  -subnetName "subnet"
  -subnetOption "existing"
```

--- -->

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. [Özel şablondan kaynak dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template)bölümündeki adımları izleyin. __Şablon seçin__ ekranına geldiğinizde, açılan listeden **201-Machine-Learning-Advanced** şablonunu seçin.
1. Şablonu kullanmak için __şablon seç__ ' i seçin. Dağıtım senaryonuza bağlı olarak aşağıdaki gerekli bilgileri ve diğer parametreleri sağlayın.

   * Abonelik: Bu kaynaklar için kullanılacak Azure aboneliğini seçin.
   * Kaynak grubu: hizmetleri içeren bir kaynak grubu seçin veya oluşturun.
   * Bölge: kaynakların oluşturulacağı Azure bölgesini seçin.
   * Çalışma alanı adı: oluşturulacak Azure Machine Learning çalışma alanı için kullanılacak ad. Çalışma alanı adı 3 ile 33 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve '-' içerebilir.
   * Konum: kaynakların oluşturulacağı konumu seçin.
1. __Gözden geçir ve oluştur__’u seçin.
1. __Gözden geçir + oluştur__ ekranında, listelenen hüküm ve koşulları kabul edin ve __Oluştur__' u seçin.

Daha fazla bilgi için bkz. [özel şablondan kaynak dağıtma](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="resource-provider-errors"></a>Kaynak sağlayıcısı hataları

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure Key Vault erişim ilkesi ve Azure Resource Manager şablonları

Çalışma alanını ve ilişkili kaynakları (Azure Key Vault dahil) oluşturmak için bir Azure Resource Manager şablonu kullandığınızda, birden çok kez. Örneğin, şablonu bir sürekli tümleştirme ve dağıtım işlem hattının bir parçası ile aynı parametrelerle birden çok kez kullanmak.

Şablonlar aracılığıyla kaynak oluşturma işlemlerinin çoğu ıdempotent, ancak Key Vault şablon her kullanıldığında erişim ilkelerini temizler. Erişim ilkelerinin temizlenmesi, onu kullanan var olan bir çalışma alanının Key Vault erişimini keser. Örneğin, Azure Notebooks VM 'yi durdur/oluştur işlevleri başarısız olabilir.  

Bu sorundan kaçınmak için aşağıdaki yaklaşımlardan birini öneririz:

* Aynı parametreler için şablonu birden çok kez dağıtmayın. Ya da yeniden oluşturmak için şablonu kullanmadan önce mevcut kaynakları silin.

* Key Vault erişim ilkelerini inceleyin ve sonra şablonun özelliğini ayarlamak için bu ilkeleri kullanın `accessPolicies` . Erişim ilkelerini görüntülemek için aşağıdaki Azure CLı komutunu kullanın:

    ```azurecli
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

    ```azurecli
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query id
    ```

    Bu komut aşağıdaki metne benzer bir değer döndürür:

    ```text
    /subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Yöneticisi şablonları ve Kaynak Yöneticisi REST API ile kaynakları dağıtın](../azure-resource-manager/templates/deploy-rest.md).
* [Visual Studio aracılığıyla Azure Kaynak grupları oluşturma ve dağıtma](../azure-resource-manager/templates/create-visual-studio-deployment-project.md).
