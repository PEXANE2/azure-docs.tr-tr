---
title: Şablon belirtimini oluşturma ve dağıtma
description: ARM şablonundan bir şablon belirtimi oluşturmayı öğrenin. Ardından, şablon belirtimini aboneliğinizdeki bir kaynak grubuna dağıtın.
author: tfitzmac
ms.date: 12/01/2020
ms.topic: quickstart
ms.author: tomfitz
ms.openlocfilehash: 03cf2013f1cec9722af5d7e72285d9f11d8a6bc1
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96518966"
---
# <a name="quickstart-create-and-deploy-template-spec-preview"></a>Hızlı başlangıç: şablon belirtimini oluşturma ve dağıtma (Önizleme)

Bu hızlı başlangıçta, bir Azure Resource Manager şablonu (ARM şablonu) bir [şablon](template-specs.md) belirtimine paketlemeyi ve ardından bu şablon belirtimini dağıtmayı gösterir. Şablon spec, depolama hesabı dağıtan bir ARM şablonu içerir.

## <a name="prerequisites"></a>Önkoşullar

Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

> [!NOTE]
> Şablon özellikleri şu anda önizleme aşamasındadır. Azure PowerShell ile kullanmak için, [5.0.0 veya üzeri sürümünü](/powershell/azure/install-az-ps)yüklemelisiniz. Azure CLı ile kullanmak için, [Version 2.14.2 veya üstünü](/cli/azure/install-azure-cli)kullanın.

## <a name="create-template-spec"></a>Şablon belirtimi oluşturma

Şablon belirtimi, **Microsoft. resources/Templatespec** adlı bir kaynak türüdür. Şablon belirtimini oluşturmak için Azure portal, Azure PowerShell, Azure CLı veya ARM şablonunu kullanabilirsiniz. Tüm seçeneklerde, şablon belirtimi içinde paketlenmiş bir ARM şablonuna ihtiyacınız vardır.

PowerShell ve CLı ile ARM şablonu komutuna bir parametre olarak geçirilir. ARM şablonuyla, şablon belirtimi içinde paketlenecek ARM şablonu, şablon belirtimi tanımına katıştırılır.

Bu seçenekler aşağıda gösterilmiştir.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Ekranın en üstünden, **arama kaynakları, hizmetler ve docs**' de **Şablon Özellikleri**' ni girin ve ardından **Şablon Özellikleri**' ni seçin.
1. **Şablon Oluştur belirtimini** seçin.
1. Aşağıdaki değerleri seçin veya girin:

    - **Ad**: şablon belirtimi için bir ad girin.  Örneğin, **storageSpec**
    - **Abonelik**: şablon belirtimini oluşturmak için kullanılan bir Azure aboneliği seçin.
    - **Kaynak grubu**: **Yeni oluştur**' u seçin ve ardından yeni bir kaynak grubu adı girin.  Örneğin, **Templatespecrg**.
    - **Konum**: kaynak grubu için bir konum seçin. Örneğin,  **Batı ABD 2**.
    - **Sürüm**: şablon belirtimi için bir sürüm girin. Örneğin, **1,0** veya **v 1.0**.

1. **İleri ' yi seçin: şablonu Düzenle**.
1. Şablon içeriğini aşağıdaki JSON ile değiştirin:

    :::code language="json" source="~/quickstart-templates/101-storage-account-create/azuredeploy.json":::

    Bu şablon spec içinde paketlenecektir.
1. **Gözden geçir + oluştur**' u seçin.
1. **Oluştur**’u seçin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. PowerShell ile bir şablon belirtimi oluşturduğunuzda yerel bir şablon geçirebilirsiniz. Aşağıdaki şablonu kopyalayın ve **azuredeploy.js** adlı bir dosyaya yerel olarak kaydedin. Bu hızlı başlangıç, bir yola **c:\Templates\azuredeploy.js** kaydettiğiniz, ancak herhangi bir yolu kullanabileceğiniz varsayılmaktadır.

    :::code language="json" source="~/quickstart-templates/101-storage-account-create/azuredeploy.json":::

1. Şablon belirtimini içeren yeni bir kaynak grubu oluşturun.

    ```azurepowershell
    New-AzResourceGroup `
      -Name templateSpecRG `
      -Location westus2
    ```

1. Ardından, bu kaynak grubunda şablon belirtimini oluşturun. Yeni şablon belirtimini **storageSpec** adını verirsiniz.

    ```azurepowershell
    New-AzTemplateSpec `
      -Name storageSpec `
      -Version "1.0" `
      -ResourceGroupName templateSpecRG `
      -Location westus2 `
      -TemplateFile "c:\Templates\azuredeploy.json"
    ```

# <a name="cli"></a>[CLI](#tab/azure-cli)

1. CLı ile bir şablon belirtimi oluşturduğunuzda yerel bir şablon geçirebilirsiniz. Aşağıdaki şablonu kopyalayın ve **azuredeploy.js** adlı bir dosyaya yerel olarak kaydedin. Bu hızlı başlangıç, bir yola **c:\Templates\azuredeploy.js** kaydettiğiniz, ancak herhangi bir yolu kullanabileceğiniz varsayılmaktadır.

    :::code language="json" source="~/quickstart-templates/101-storage-account-create/azuredeploy.json":::

1. Şablon belirtimini içeren yeni bir kaynak grubu oluşturun.

    ```azurecli
    az group create \
      --name templateSpecRG \
      --location westus2
    ```

1. Ardından, bu kaynak grubunda şablon belirtimini oluşturun. Yeni şablon belirtimini **storageSpec** adını verirsiniz.

    ```azurecli
    az ts create \
      --name storageSpec \
      --version "1.0" \
      --resource-group templateSpecRG \
      --location "westus2" \
      --template-file "c:\Templates\azuredeploy.json"
    ```

# <a name="arm-template"></a>[ARM şablonu](#tab/azure-resource-manager)

1. Şablon belirtimini oluşturmak için ARM şablonu kullandığınızda, şablon kaynak tanımına katıştırılır. Aşağıdaki şablonu kopyalayın ve **azuredeploy.js** olarak yerel olarak kaydedin. Bu hızlı başlangıç, bir yola **c:\Templates\azuredeploy.js** kaydettiğiniz, ancak herhangi bir yolu kullanabileceğiniz varsayılmaktadır.

    > [!NOTE]
    > Katıştırılmış şablonda, ikinci bir sol köşeli ayraç ile tüm [şablon ifadelerine](template-expressions.md) kaçmalıdır. `"[[`Yerine kullanın `"[` . JSON dizileri hala tek sol köşeli ayraç kullanır.

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "functions": [],
      "variables": {},
      "resources": [
        {
          "type": "Microsoft.Resources/templateSpecs",
          "apiVersion": "2019-06-01-preview",
          "name": "storageSpec",
          "location": "westus2",
          "properties": {
            "displayName": "Storage template spec"
          },
          "tags": {},
          "resources": [
            {
              "type": "versions",
              "apiVersion": "2019-06-01-preview",
              "name": "1.0",
              "location": "westus2",
              "dependsOn": [ "storageSpec" ],
              "properties": {
                "template": {
                  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                  "contentVersion": "1.0.0.0",
                  "parameters": {
                    "storageAccountType": {
                      "type": "string",
                      "defaultValue": "Standard_LRS",
                      "allowedValues": [
                        "Standard_LRS",
                        "Standard_GRS",
                        "Standard_ZRS",
                        "Premium_LRS"
                      ],
                      "metadata": {
                        "description": "Storage Account type"
                      }
                    },
                    "location": {
                      "type": "string",
                      "defaultValue": "[[resourceGroup().location]",
                      "metadata": {
                        "description": "Location for all resources."
                      }
                    }
                  },
                  "variables": {
                    "storageAccountName": "[[concat('store', uniquestring(resourceGroup().id))]"
                  },
                  "resources": [
                    {
                      "type": "Microsoft.Storage/storageAccounts",
                      "apiVersion": "2019-04-01",
                      "name": "[[variables('storageAccountName')]",
                      "location": "[[parameters('location')]",
                      "sku": {
                        "name": "[[parameters('storageAccountType')]"
                      },
                      "kind": "StorageV2",
                      "properties": {}
                    }
                  ],
                  "outputs": {
                    "storageAccountName": {
                      "type": "string",
                      "value": "[[variables('storageAccountName')]"
                    }
                  }
                }
              },
              "tags": {}
            }
          ]
        }
      ],
      "outputs": {}
    }
    ```

1. Yeni bir kaynak grubu oluşturmak için Azure CLı veya PowerShell kullanın.

    ```azurepowershell
    New-AzResourceGroup `
      -Name templateSpecRG `
      -Location westus2
    ```

    ```azurecli
    az group create \
      --name templateSpecRG \
      --location westus2
    ```

1. Azure CLı veya PowerShell ile şablonunuzu dağıtın.

    ```azurepowershell
    New-AzResourceGroupDeployment `
      -ResourceGroupName templateSpecRG `
      -TemplateFile "c:\Templates\azuredeploy.json"
    ```

    ```azurecli
    az deployment group create \
      --name templateSpecRG \
      --template-file "c:\Templates\azuredeploy.json"
    ```

---

## <a name="deploy-template-spec"></a>Şablon belirtimini dağıt

Artık şablon belirtimini dağıtabilirsiniz. Şablon belirtiminin dağıtımı, Azure PowerShell veya Azure CLı içindeki şablon belirtiminin kaynak KIMLIĞINI geçirmeniz dışında, içerdiği şablonu dağıtmaya benzer. Aynı dağıtım komutlarını kullanırsınız ve gerekirse, şablon belirtiminin parametre değerlerini geçirin.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Azure portal, son yordamda oluşturduğunuz kaynak grubunu açın.  Örneğin **Templatespecrg**.
1. Oluşturduğunuz şablon belirtimini seçin. Örneğin, **storageSpec**.
1. **Dağıt**'ı seçin.
1. Aşağıdaki değerleri seçin veya girin:

    - **Abonelik**: kaynağı oluşturmak Için bir Azure aboneliği seçin.
    - **Kaynak grubu**: **Yeni oluştur** ' u seçin ve ardından **storagerg** yazın.
    - **Depolama hesabı türü**: **Standard_GRS** seçin.

    Yeni bir kaynak grubu oluşturur ve şablonu şablon spec içinde yeni kaynak grubuna dağıtırsınız.

1. **Gözden geçir ve oluştur**’u seçin.
1. **Oluştur**’u seçin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Yeni depolama hesabını içerecek bir kaynak grubu oluşturun.

    ```azurepowershell
    New-AzResourceGroup `
      -Name storageRG `
      -Location westus2
    ```

1. Şablon belirtiminin kaynak KIMLIĞINI alın.

    ```azurepowershell
    $id = (Get-AzTemplateSpec -ResourceGroupName templateSpecRG -Name storageSpec -Version "1.0").Versions.Id
    ```

1. Şablon belirtimini dağıtın.

    ```azurepowershell
    New-AzResourceGroupDeployment `
      -TemplateSpecId $id `
      -ResourceGroupName storageRG
    ```

1. Bir ARM şablonunda yaptığınız gibi parametreleri tam olarak sağlarsınız. Şablon belirtimini depolama hesabı türü için bir parametre ile yeniden dağıtın.

    ```azurepowershell
    New-AzResourceGroupDeployment `
      -TemplateSpecId $id `
      -ResourceGroupName storageRG `
      -storageAccountType Standard_GRS
    ```

# <a name="cli"></a>[CLI](#tab/azure-cli)

1. Yeni depolama hesabını içerecek bir kaynak grubu oluşturun.

    ```azurecli
    az group create \
      --name storageRG \
      --location westus2
    ```

1. Şablon belirtiminin kaynak KIMLIĞINI alın.

    ```azurecli
    id = $(az ts show --name storageSpec --resource-group templateSpecRG --version "1.0" --query "id")
    ```

    > [!NOTE]
    > Şablon spec KIMLIĞI alma ve bunu Windows PowerShell 'de bir değişkene atama ile ilgili bilinen bir sorun vardır.

1. Şablon belirtimini dağıtın.

    ```azurecli
    az deployment group create \
      --resource-group storageRG \
      --template-spec $id
    ```

1. Bir ARM şablonunda yaptığınız gibi parametreleri tam olarak sağlarsınız. Şablon belirtimini depolama hesabı türü için bir parametre ile yeniden dağıtın.

    ```azurecli
    az deployment group create \
      --resource-group storageRG \
      --template-spec $id \
      --parameters storageAccountType='Standard_GRS'
    ```

# <a name="arm-template"></a>[ARM şablonu](#tab/azure-resource-manager)

1. Aşağıdaki şablonu kopyalayın ve **storage.js** adlı bir dosyaya yerel olarak kaydedin.

    ```json
       {
      "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "functions": [],
      "variables": {},
      "resources": [
        {
          "type": "Microsoft.Resources/deployments",
          "apiVersion": "2020-06-01",
          "name": "demo",
          "properties": {
            "templateLink": {
              "id": "[resourceId('templateSpecRG', 'Microsoft.Resources/templateSpecs/versions', 'storageSpec', '1.0')]"
            },
            "parameters": {
            },
            "mode": "Incremental"
          }
        }
      ],
      "outputs": {}
    }
    ```

1. Depolama hesabı için yeni bir kaynak grubu oluşturmak üzere Azure CLı veya PowerShell 'i kullanın.

    ```azurepowershell
    New-AzResourceGroup `
      -Name storageRG `
      -Location westus2
    ```

    ```azurecli
    az group create \
      --name storageRG \
      --location westus2
    ```

1. Azure CLı veya PowerShell ile şablonunuzu dağıtın.

    ```azurepowershell
    New-AzResourceGroupDeployment `
      -ResourceGroupName storageRG `
      -TemplateFile "c:\Templates\storage.json"
    ```

    ```azurecli
    az deployment group create \
      --name storageRG \
      --template-file "c:\Templates\storage.json"
    ```

---

## <a name="grant-access"></a>Erişim verme

Kuruluşunuzdaki diğer kullanıcıların şablon belirtimini dağıtmasını sağlamak istiyorsanız, bunlara okuma erişimi vermeniz gerekir. Okuyucu rolünü, paylaşmak istediğiniz şablon özelliklerini içeren kaynak grubu için bir Azure AD grubuna atayabilirsiniz. Daha fazla bilgi için bkz. [öğretici: Azure PowerShell kullanarak Azure kaynaklarına grup erişimi verme](../../role-based-access-control/tutorial-role-assignments-group-powershell.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta dağıttığınız kaynağı temizlemek için, oluşturduğunuz her iki kaynak grubunu silin.

1. Azure portalında sol menüden Kaynak grubu’nu seçin.

1. Ada göre filtrele alanına kaynak grubu adını (templateSpecRG ve storageRG) girin.

1. Kaynak grubu adını seçin.

1. Üstteki menüden Kaynak grubunu sil’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantılı şablonlar içeren bir şablon belirtimi oluşturma hakkında bilgi edinmek için bkz. [bağlantılı şablonun şablon belirtimini oluşturma](template-specs-create-linked.md).
