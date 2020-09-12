---
title: Parametre dosyası oluşturma
description: Azure Resource Manager şablonunun dağıtımı sırasında değerleri geçirmek için parametre dosyası oluşturma
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 2b6d942b21594fa608127bb8f403e72295671005
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89276669"
---
# <a name="create-resource-manager-parameter-file"></a>Kaynak Yöneticisi parametre dosyası oluştur

Komut dosyanıza satır içi değer olarak parametre geçirmek yerine, parametre değerlerini içeren bir JSON dosyası kullanmayı daha kolay bulabilirsiniz. Bu makalede parametre dosyasının nasıl oluşturulacağı gösterilmektedir.

## <a name="parameter-file"></a>Parametre dosyası

Parametre dosyası aşağıdaki biçimdedir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "value": "<second-value>"
    }
  }
}
```

Parametre değerlerinin parametre dosyasında düz metin olarak depolandığını unutmayın. Bu yaklaşım, bir kaynak için SKU belirtme gibi hassas olmayan değerler için geçerlidir. Parolalar gibi hassas değerler için çalışmaz. Hassas bir değeri parametre olarak geçirmeniz gerekiyorsa, değeri bir anahtar kasasında depolayın ve parametre dosyanızdaki anahtar kasasına başvurun. Hassas değer dağıtım sırasında güvenli bir şekilde alınır.

Aşağıdaki parametre dosyası bir düz metin değeri ve bir anahtar kasasında depolanan bir değer içerir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "reference": {
        "keyVault": {
          "id": "<resource-id-key-vault>"
        },
        "secretName": "<secret-name>"
      }
    }
  }
}
```

Anahtar kasasından değerleri kullanma hakkında daha fazla bilgi için bkz. [dağıtım sırasında güvenli parametre değeri geçirmek için Azure Key Vault kullanma](key-vault-parameter.md).

## <a name="define-parameter-values"></a>Parametre değerlerini tanımla

Parametre değerlerinin nasıl tanımlanacağını anlamak için, dağıttığınız şablonu açın. Şablonun parametreler bölümüne bakın. Aşağıdaki örnek, bir şablondan parametreleri gösterir.

```json
"parameters": {
  "storagePrefix": {
    "type": "string",
    "maxLength": 11
  },
  "storageAccountType": {
    "type": "string",
    "defaultValue": "Standard_LRS",
    "allowedValues": [
    "Standard_LRS",
    "Standard_GRS",
    "Standard_ZRS",
    "Premium_LRS"
    ]
  }
}
```

Bildirimin ilk ayrıntısı her parametrenin adıdır. Parametre dosyanızdaki değerlerin adlarıyla eşleşmesi gerekir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
    },
    "storageAccountType": {
    }
  }
}
```

Parametrenin türüne dikkat edin. Parametre dosyanızdaki değerler aynı türde olmalıdır. Bu şablon için, her iki parametreyi de dizeler olarak sağlayabilirsiniz.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": ""
    },
    "storageAccountType": {
      "value": ""
    }
  }
}
```

Ardından, varsayılan bir değer arayın. Bir parametre varsayılan değere sahipse, bir değer sağlayabilirsiniz ancak bunu yapmanız gerekmez.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "" // This value must be provided.
    },
    "storageAccountType": {
      "value": "" // This value is optional. Template will use default value if not provided.
    }
  }
}
```

Son olarak, izin verilen değerlere ve en fazla uzunluk gibi kısıtlamalara göz atın. Bu, size parametresi için sağlayabilmeniz için değer aralığı hakkında bilgi verir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "storage"
    },
    "storageAccountType": {
      "value": "Standard_ZRS"
    }
  }
}
```

Parametre dosyanız yalnızca şablonda tanımlanmış parametreler için değerler içerebilir. Parametre dosyanız şablondaki parametrelerle eşleşmeyen ek parametreler içeriyorsa, bir hata alırsınız.

## <a name="parameter-type-formats"></a>Parametre türü biçimleri

Aşağıdaki örnek farklı parametre türlerinin biçimlerini gösterir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "exampleString": {
      "value": "test string"
    },
    "exampleInt": {
      "value": 4
    },
    "exampleBool": {
      "value": true
    },
    "exampleArray": {
      "value": [
        "value 1",
        "value 2"
      ]
    },
    "exampleObject": {
      "value": {
        "property1": "value1",
        "property2": "value2"
      }
    }
   }
}
```

## <a name="deploy-template-with-parameter-file"></a>Parametre dosyası ile şablon dağıtma

Azure CLı ile yerel bir parametre dosyası geçirmek için, @ ve parametre dosyasının adını kullanın.

```azurecli
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

Daha fazla bilgi için bkz. [ARM şablonları ve Azure CLI ile kaynak dağıtma](./deploy-cli.md#parameters).

Yerel bir parametre dosyasını Azure PowerShell geçirmek için `TemplateParameterFile` parametresini kullanın.

```azurepowershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Daha fazla bilgi için bkz. [ARM şablonlarıyla kaynak dağıtma ve Azure PowerShell](./deploy-powershell.md#pass-parameter-values)

> [!NOTE]
> Portalda özel şablon dikey penceresinde bir parametre dosyası kullanmak mümkün değildir.

## <a name="file-name"></a>Dosya adı

Parametre dosyasını adlandırmak için genel kural, şablon adına **. parametreler** eklemektir. Örneğin, şablonunuz **azuredeploy.js**olarak adlandırılmışsa, parametre dosyanız ** üzerindeazuredeploy.parameters.js**olarak adlandırılır. Bu adlandırma kuralı, şablon ve parametreler arasındaki bağlantıyı görmenizi sağlar.

Farklı ortamlara dağıtmak için, birden fazla parametre dosyası oluşturun. Parametre dosyasını adlandırırken, kullanımını tanımlamak için bir yol ekleyin. Örneğin, **üzerindeazuredeploy.parameters-dev.js** ve **azuredeploy.parameters-prod.js** kullanın

## <a name="parameter-precedence"></a>Parametre önceliği

Aynı dağıtım işleminde satır içi parametreleri ve yerel bir parametre dosyasını kullanabilirsiniz. Örneğin, yerel parametre dosyasında bazı değerler belirtebilir ve dağıtım sırasında satır içi diğer değerleri ekleyebilirsiniz. Hem yerel parametre dosyasında hem de satır içi bir parametre için değerler sağlarsanız, satır içi değer önceliklidir.

Dosyaya URI sağlayarak bir dış parametre dosyası kullanmak mümkündür. Dış parametre dosyası kullandığınızda, diğer değerleri satır içi veya yerel bir dosyadan geçiremezsiniz. Tüm satır içi parametreler yok sayılır. Dış dosyadaki tüm parametre değerlerini belirtin.

## <a name="parameter-name-conflicts"></a>Parametre adı çakışmaları

Şablonunuz PowerShell komutundaki parametrelerden biriyle aynı ada sahip bir parametre içeriyorsa, PowerShell, şablondaki parametreyi sonek **FromTemplate**ile gösterir. Örneğin, şablonunuzda **resourcegroupname** adlı bir parametre [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) cmdlet 'inin **resourcegroupname** parametresiyle çakışıyor. **Resourcegroupnamefromtemplate**için bir değer sağlamanız istenir. Dağıtım komutları için kullanılmayan parametre adlarını kullanarak bu karışıklık kullanmaktan kaçınabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- Şablonunuzda parametrelerin nasıl tanımlanacağını anlamak için bkz. [Azure Resource Manager şablonlarındaki parametreler](template-parameters.md).
- Anahtar kasasından değerleri kullanma hakkında daha fazla bilgi için bkz. [dağıtım sırasında güvenli parametre değeri geçirmek için Azure Key Vault kullanma](key-vault-parameter.md).
- Parametreler hakkında daha fazla bilgi için [Azure Resource Manager şablonlarındaki parametreler](template-parameters.md)bölümüne bakın.
