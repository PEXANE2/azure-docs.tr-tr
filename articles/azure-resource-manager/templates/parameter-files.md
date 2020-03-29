---
title: Parametre dosyası oluşturma
description: Azure Kaynak Yöneticisi şablonunun dağıtımı sırasında değerleri aktarmak için parametre dosyası oluşturma
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: bb52d9c5ebcb0820362e5de3d6b24b0b18d742e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76155630"
---
# <a name="create-resource-manager-parameter-file"></a>Kaynak Yöneticisi parametre dosyası oluşturma

Komut dosyanızdaki satır değerleri olarak parametreleri geçirmek yerine, parametre değerlerini içeren bir JSON dosyasını kullanmayı daha kolay bulabilirsiniz. Bu makalede, parametre dosyasının nasıl oluşturulacak gösterilmektedir.

## <a name="parameter-file"></a>Parametre dosyası

Parametre dosyası aşağıdaki biçime sahiptir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

Parametre değerlerinin parametre dosyasında düz metin olarak depolandık. Bu yaklaşım, bir kaynak için SKU belirtmek gibi hassas olmayan değerler için çalışır. Parolalar gibi hassas değerler için çalışmaz. Parametre olarak hassas bir değer geçirmeniz gerekiyorsa, değeri anahtar kasasında depolayın ve parametre dosyanızdaki anahtar kasasına başvurun. Hassas değer dağıtım sırasında güvenli bir şekilde alınır.

Aşağıdaki parametre dosyası düz bir metin değeri ve anahtar kasasında depolanan bir değer içerir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

Önemli bir kasadaki değerleri kullanma hakkında daha fazla bilgi için dağıtım [sırasında güvenli parametre değerini geçmek için Azure Anahtar Kasası'nı kullanın'a](key-vault-parameter.md)bakın.

## <a name="define-parameter-values"></a>Parametre değerlerini tanımlama

Parametre değerlerini nasıl tanımlayabileceğinizi anlamak için, dağıttığınız şablonu açın. Şablonun parametreler bölümüne bakın. Aşağıdaki örnekte, şablondaki parametreler gösterilmektedir.

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

Dikkat çeken ilk ayrıntı her parametrenin adıdır. Parametre dosyanızdaki değerler adlarıyla eşleşmelidir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
    },
    "storageAccountType": {
    }
  }
}
```

Parametrenin türüne dikkat edin. Parametre dosyanızdaki değerler aynı türe sahip olmalıdır. Bu şablon için her iki parametreyi de dizeleri olarak sağlayabilirsiniz.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

Ardından, varsayılan bir değer arayın. Bir parametrenin varsayılan değeri varsa, bir değer sağlayabilirsiniz, ancak bunu yapmak zorunda değilsiniz.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

Son olarak, izin verilen değerlere ve maksimum uzunluk gibi kısıtlamalara bakın. Parametre için sağlayabileceğiniz değerler aralığını söylerler.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

## <a name="parameter-type-formats"></a>Parametre türü biçimleri

Aşağıdaki örnek, farklı parametre türlerinin biçimlerini gösterir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

## <a name="file-name"></a>Dosya adı

Parametre dosyasını adlandırmak için genel kural şablon adına **.parametreler** eklemektir. Örneğin, şablonunuz **azuredeploy.json**olarak adlandırılmışsa, parametre dosyanız **azuredeploy.parameters.json**olarak adlandırılır. Bu adlandırma kuralı, şablon ve parametreler arasındaki bağlantıyı görmenize yardımcı olur.

Farklı ortamlara dağıtmak için birden fazla parametre dosyası oluşturun. Parametre dosyasını adlandırırken, dosyanın kullanımını tanımlamak için bir yol ekleyin. Örneğin, **azuredeploy.parameters-dev.json** ve **azuredeploy.parameters-prod.json** adresini kullanın


## <a name="parameter-precedence"></a>Parametre önceliği

Aynı dağıtım işleminde satır içinde parametreleri ve yerel parametre dosyasını kullanabilirsiniz. Örneğin, yerel parametre dosyasında bazı değerleri belirtebilir ve dağıtım sırasında satır içi başka değerler ekleyebilirsiniz. Hem yerel parametre dosyasında hem de satır satırda bir parametre için değerler sağlarsanız, satır satır değeri önce gelir.

Ancak, harici bir parametre dosyası kullandığınızda, satır veya yerel bir dosyadan diğer değerleri geçiremezsiniz. Tüm satır parametreleri yoksayılır. Dış dosyadaki tüm parametre değerlerini sağlayın.

## <a name="parameter-name-conflicts"></a>Parametre adı çakışmaları

Şablonunuz PowerShell komutundaki parametrelerden biriyle aynı ada sahip bir parametre içeriyorsa, PowerShell şablonunuzdaki parametreyi **FromTemplate**düzeltiyle birlikte sunar. Örneğin, şablonunuzda **ResourceGroupName** adlı bir [parametre, Yeni-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) cmdlet'teki **ResourceGroupName** parametresi ile çakışıyor. **KaynakGroupNameFromTemplate**için bir değer sağlamanız istenir. Dağıtım komutları için kullanılmayan parametre adlarını kullanarak bu karışıklığı önleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Şablonunuzdaparametrelerin nasıl tanımlandığını anlamak için [Azure Kaynak Yöneticisi şablonlarında parametrelere](template-parameters.md)bakın.
- Önemli bir kasadaki değerleri kullanma hakkında daha fazla bilgi için dağıtım [sırasında güvenli parametre değerini geçmek için Azure Anahtar Kasası'nı kullanın'a](key-vault-parameter.md)bakın.
- Parametreler hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi şablonlarında Parametreler'e](template-parameters.md)bakın.
