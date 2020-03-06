---
title: Dağıtım için şablonları bağlama
description: Modüler şablon çözüm oluşturmak için bir Azure Resource Manager şablonunda bağlı şablonların kullanmayı açıklar. Parametre değerlerini geçirmek için bir parametre dosyası ve dinamik olarak oluşturulan URL'leri belirtin gösterilmektedir.
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: e26b795a645ab9128dd738ba6a54b66ac0b7da2a
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78354985"
---
# <a name="using-linked-and-nested-templates-when-deploying-azure-resources"></a>Bağlı, şablonları Azure kaynakları dağıtılırken iç içe kullanma

Karmaşık çözümleri dağıtmak için şablonunuzu birçok ilgili şablona bölebilir ve ardından bunları bir ana şablon aracılığıyla dağıtabilirsiniz. İlgili şablonlar, ana şablon içine katıştırılmış ayrı dosyalar veya şablon söz dizimi olabilir. Bu makalede, ana şablondan bağlantılı ayrı bir şablon dosyasına başvurmak için **bağlantılı şablon** terimi kullanılmaktadır. Ana şablon içindeki katıştırılmış şablon söz dizimini ifade etmek için **iç içe geçmiş şablon** terimini kullanır.

Küçük ila orta çözümleri, tek bir şablon anlamak ve sürdürmek daha kolay olur. Tüm kaynaklar ve tek bir dosyada değerleri görebilirsiniz. Bağlantılı şablonlar, gelişmiş senaryolar için çözümü hedeflenen bileşenlere bölebilir. Bu şablonları diğer senaryolar için kolayca yeniden kullanabilirsiniz.

Öğretici için bkz. [öğretici: bağlı Azure Resource Manager şablonları oluşturma](template-tutorial-create-linked-templates.md).

> [!NOTE]
> Bağlantılı veya iç içe şablonlar için yalnızca [artımlı](deployment-modes.md) Dağıtım modunu kullanabilirsiniz.
>

## <a name="nested-template"></a>İç içe geçmiş şablon

Bir şablonu iç içe aktarmak için ana şablonunuza bir [dağıtımlar kaynağı](/azure/templates/microsoft.resources/deployments) ekleyin. **Şablon** özelliğinde, Şablon sözdizimini belirtin.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "name": "nestedTemplate1",
      "apiVersion": "2017-05-10",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "Incremental",
        "template": {
          <nested-template-syntax>
        }
      }
    }
  ],
  "outputs": {
  }
}
```

Aşağıdaki örnek, iç içe geçmiş bir şablon aracılığıyla bir depolama hesabı dağıtır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "name": "nestedTemplate1",
      "apiVersion": "2017-05-10",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2019-04-01",
              "name": "[parameters('storageAccountName')]",
              "location": "West US",
              "sku": {
                "name": "Standard_LRS"
              },
              "kind": "StorageV2"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
  }
}
```

### <a name="scope-for-expressions-in-nested-templates"></a>İç içe şablonlarda ifadelerin kapsamı

İç içe geçmiş bir şablon kullanırken, şablon ifadelerinin üst şablon kapsamında mi yoksa iç içe yerleştirilmiş şablon içinde mi değerlendirileceğini belirtebilirsiniz. Kapsam, parametrelerin, değişkenlerin ve [resourceGroup](template-functions-resource.md#resourcegroup) ve [abonelik](template-functions-resource.md#subscription) gibi işlevlerin nasıl çözümlendiğini belirler.

Kapsamı `expressionEvaluationOptions` özelliği aracılığıyla ayarlarsınız. Varsayılan olarak, `expressionEvaluationOptions` özelliği `outer`olarak ayarlanır, yani üst şablon kapsamını kullanır. Değeri, iç içe şablon olarak kapsam ifadelerine `inner` olarak ayarlayın.

```json
{
  "type": "Microsoft.Resources/deployments",
  "apiVersion": "2017-05-10",
  "name": "nestedTemplate1",
  "properties": {
  "expressionEvaluationOptions": {
    "scope": "inner"
  },
  ...
```

Aşağıdaki şablon, şablon ifadelerinin kapsama göre nasıl çözümlendiğini gösterir. Hem üst şablonda hem de iç içe yerleştirilmiş şablonda tanımlanan `exampleVar` adlı bir değişken içerir. Değişkenin değerini döndürür.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {
    "exampleVar": "from parent template"
  },
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "nestedTemplate1",
      "properties": {
        "expressionEvaluationOptions": {
          "scope": "inner"
        },
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "variables": {
            "exampleVar": "from nested template"
          },
          "resources": [
          ],
          "outputs": {
            "testVar": {
              "type": "string",
              "value": "[variables('exampleVar')]"
            }
          }
        }
      }
    }
  ],
  "outputs": {
    "messageFromLinkedTemplate": {
      "type": "string",
      "value": "[reference('nestedTemplate1').outputs.testVar.value]"
    }
  }
}
```

Değişkenin değeri, kapsama göre değişir. Aşağıdaki tabloda her iki kapsamın sonuçları gösterilmektedir.

| Kapsam | Çıktı |
| ----- | ------ |
| iç | iç içe şablondan |
| dış (veya varsayılan) | üst şablondan |

Aşağıdaki örnek, bir SQL Server dağıtır ve parola için kullanılacak bir Anahtar Kasası gizli anahtarı alır. Kapsam, Anahtar Kasası KIMLIĞINI dinamik olarak oluşturduğundan ve iç içe yerleştirilmiş şablona bir parametre olarak geçirdiğinde `inner` olarak ayarlanır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "The location where the resources will be deployed."
      }
    },
    "vaultName": {
      "type": "string",
      "metadata": {
        "description": "The name of the keyvault that contains the secret."
      }
    },
    "secretName": {
      "type": "string",
      "metadata": {
        "description": "The name of the secret."
      }
    },
    "vaultResourceGroupName": {
      "type": "string",
      "metadata": {
        "description": "The name of the resource group that contains the keyvault."
      }
    },
    "vaultSubscription": {
      "type": "string",
      "defaultValue": "[subscription().subscriptionId]",
      "metadata": {
        "description": "The name of the subscription that contains the keyvault."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "name": "dynamicSecret",
      "properties": {
        "mode": "Incremental",
        "expressionEvaluationOptions": {
          "scope": "inner"
        },
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminLogin": {
              "type": "string"
            },
            "adminPassword": {
              "type": "securestring"
            },
            "location": {
              "type": "string"
            }
          },
          "variables": {
            "sqlServerName": "[concat('sql-', uniqueString(resourceGroup().id, 'sql'))]"
          },
          "resources": [
            {
              "type": "Microsoft.Sql/servers",
              "apiVersion": "2018-06-01-preview",
              "name": "[variables('sqlServerName')]",
              "location": "[parameters('location')]",
              "properties": {
                "administratorLogin": "[parameters('adminLogin')]",
                "administratorLoginPassword": "[parameters('adminPassword')]"
              }
            }
          ],
          "outputs": {
            "sqlFQDN": {
              "type": "string",
              "value": "[reference(variables('sqlServerName')).fullyQualifiedDomainName]"
            }
          }
        },
        "parameters": {
          "location": {
            "value": "[parameters('location')]"
          },
          "adminLogin": {
            "value": "ghuser"
          },
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

> [!NOTE]
>
> Kapsam `outer`olarak ayarlandığında, iç içe yerleştirilmiş şablonda dağıttığınız bir kaynağın iç içe yerleştirilmiş bir şablonun çıktılar bölümünde `reference` işlevini kullanamazsınız. Dağıtılan bir kaynağın değerlerini iç içe geçmiş bir şablonda döndürmek için, iç kapsam kullanın veya iç içe geçmiş şablonunuzu bağlı bir şablona dönüştürün.

## <a name="linked-template"></a>Bağlantılı şablon

Bir şablonu bağlamak için ana şablonunuza bir [dağıtımlar kaynağı](/azure/templates/microsoft.resources/deployments) ekleyin. **Templatelink** özelliğinde, dahil edılecek şablonun URI 'sini belirtin. Aşağıdaki örnek, yeni bir depolama hesabı dağıtan bir şablona bağlantı sağlar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
  }
}
```

Yerel bir dosya ya da yalnızca yerel ağınızda kullanılabilir olan dosya belirtemezsiniz. Yalnızca **http** ya da **https**içeren bir URI değeri sağlayabilirsiniz. Kaynak Yöneticisi şablona erişebilmelidir. Bir seçenek bağlı şablonunuzu bir depolama hesabında yerleştirin ve bu öğe için bir URI kullanın oluşturmaktır.

Şablon veya parametreler için `contentVersion` özelliği sağlamanız gerekmez. Bir içerik sürümü değeri sağlamıyorsa şablonunun geçerli sürümünü dağıtılır. İçerik sürümü için bir değer belirtirseniz, bağlı şablonun sürümünde eşleşmelidir; Aksi takdirde, dağıtım, bir hata ile başarısız olur.

### <a name="parameters-for-linked-template"></a>Bağlantılı şablon parametreleri

Bağlı şablonunuz için parametreleri bir dış dosyada ya da satır içi olarak sağlayabilirsiniz. Bir dış parametre dosyası sağlarken, **Parameterslink** özelliğini kullanın:

```json
"resources": [
  {
  "type": "Microsoft.Resources/deployments",
  "apiVersion": "2018-05-01",
  "name": "linkedTemplate",
  "properties": {
    "mode": "Incremental",
    "templateLink": {
    "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
    "contentVersion":"1.0.0.0"
    },
    "parametersLink": {
    "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.parameters.json",
    "contentVersion":"1.0.0.0"
    }
  }
  }
]
```

Parametre değerlerini satır içi olarak geçirmek için **Parameters** özelliğini kullanın.

```json
"resources": [
  {
   "type": "Microsoft.Resources/deployments",
   "apiVersion": "2018-05-01",
   "name": "linkedTemplate",
   "properties": {
     "mode": "Incremental",
     "templateLink": {
      "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
      "contentVersion":"1.0.0.0"
     },
     "parameters": {
      "StorageAccountName":{"value": "[parameters('StorageAccountName')]"}
    }
   }
  }
]
```

Satır içi parametre hem de bir bağlantı için bir parametre dosyası kullanamazsınız. Hem `parametersLink` hem de `parameters` belirtildiğinde dağıtım bir hata ile başarısız olur.

## <a name="using-copy"></a>Kopyayı kullanma

İç içe geçmiş şablonla bir kaynağın birden çok örneğini oluşturmak için, **Microsoft. resources/dağıtımlar** kaynağı düzeyinde kopyalama öğesini ekleyin. Ya da kapsam iç ise, iç içe geçmiş şablon içinde kopyayı ekleyebilirsiniz.

Aşağıdaki örnek şablon, kopyalamanın iç içe geçmiş bir şablonla nasıl kullanılacağını göstermektedir.

```json
"resources": [
  {
  "type": "Microsoft.Resources/deployments",
  "apiVersion": "2018-05-01",
  "name": "[concat('nestedTemplate', copyIndex())]",
  // yes, copy works here
  "copy":{
    "name": "storagecopy",
    "count": 2
  },
  "properties": {
    "mode": "Incremental",
    "expressionEvaluationOptions": {
    "scope": "inner"
    },
    "template": {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
      {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(variables('storageName'), copyIndex())]",
      "location": "West US",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "StorageV2"
      // Copy works here when scope is inner
      // But, when scope is default or outer, you get an error
      //"copy":{
      //  "name": "storagecopy",
      //  "count": 2
      //}
      }
    ]
    }
  }
  }
]
```

## <a name="using-variables-to-link-templates"></a>Şablonları bağlamak için değişkenleri kullanma

Önceki örneklerde şablon bağlantılara sabit kodlanmış URL'si değerleri gösterdi. Bu yaklaşım için basit bir şablon çalışabilir ancak büyük bir dizi modüler şablonu ile çalışırken de çalışmıyor. Bunun yerine, ana şablon için temel URL saklayan statik bir değişken oluşturun ve ardından dinamik olarak URL'ler için bu temel URL bağlı şablonlardan oluşturma. Bu yaklaşımın avantajı, kolayca taşıyabilir veya yalnızca ana şablondaki statik değişkeni değiştirmeniz gerekir çünkü şablon çatal içindir. Ana Şablon doğru bir URI'leri ayrıştırılmış şablon boyunca geçirir.

Aşağıdaki örnek, bağlantılı şablonlar (**Sharedtemplateurl** ve **vmtemplate**) için iki URL oluşturmak üzere temel URL 'yi nasıl kullanacağınızı gösterir.

```json
"variables": {
  "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
  "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
  "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Ayrıca, geçerli şablonun temel URL 'sini almak için [Deployment ()](template-functions-deployment.md#deployment) kullanabilirsiniz ve bunu, aynı konumdaki DIĞER şablonların URL 'sini almak için kullanabilirsiniz. Bu yaklaşım, şablonu konumu değişikliklerinizi veya şablon dosyasında URL'leri Sabit kodlama kaçınmak istiyorsanız kullanışlıdır. TemplateLink özelliğindeki yalnızca bir URL ile bir uzak şablonuna bağlarken döndürülür. Yerel bir şablonu kullanıyorsanız, bu özellik kullanılabilir değil.

```json
"variables": {
  "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="get-values-from-linked-template"></a>Bağlı şablondan değerleri alma

Bağlı bir şablondan çıkış değeri almak için, özellik değerini şu şekilde olan sözdizimi ile alın: `"[reference('deploymentName').outputs.propertyName.value]"`.

Bir çıkış özelliği bağlı şablonundan alınırken, özellik adı bir tire içeremez.

Aşağıdaki örnekler, bağlı bir şablona başvurmak ve bir çıkış değeri almak nasıl ekleyebileceğiniz gösterilmektedir. Bağlantılı şablon, basit bir ileti döndürür.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "greetingMessage": {
      "value": "Hello World",
      "type" : "string"
    }
  }
}
```

Ana Şablon dağıtır bağlı şablonun ve döndürülen değer alır. Ada göre dağıtım kaynağına başvurduğundan ve bağlantılı şablon tarafından döndürülen özelliğin adını kullanan dikkat edin.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "name": "linkedTemplate",
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[uri(deployment().properties.templateLink.uri, 'helloworld.json')]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
    "messageFromLinkedTemplate": {
      "type": "string",
      "value": "[reference('linkedTemplate').outputs.greetingMessage.value]"
    }
  }
}
```

Gibi diğer kaynak türlerini, bağlı şablonun ve diğer kaynaklar arasındaki bağımlılıkları da ayarlayabilirsiniz. Diğer kaynaklar bağlantılı şablondan bir çıkış değeri gerektirdiğinde, bağlantılı şablonun uygulamadan önce dağıtıldığından emin olun. Ya da diğer kaynaklara bağlı şablonun kullanır, bağlı şablonun önce dağıtılan diğer kaynakları emin olun.

Aşağıdaki örnek, bir genel IP adresi dağıtır ve kaynak Kimliğini döndüren bir şablon gösterir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "publicIPAddresses_name": {
      "type": "string"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/publicIPAddresses",
      "apiVersion": "2018-11-01",
      "name": "[parameters('publicIPAddresses_name')]",
      "location": "eastus",
      "properties": {
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 4
      },
      "dependsOn": []
    }
  ],
  "outputs": {
    "resourceID": {
      "type": "string",
      "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
    }
  }
}
```

Önceki şablondan genel IP adresini yük dengeleyici dağıtırken kullanmak için şablona bağladığınız ve bağımlılık dağıtım kaynağı ekleyin. Genel IP adresini yük dengeleyici üzerindeki bağlantılı şablondan çıkış değerine ayarlanır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "loadBalancers_name": {
      "defaultValue": "mylb",
      "type": "string"
    },
    "publicIPAddresses_name": {
      "defaultValue": "myip",
      "type": "string"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/loadBalancers",
      "apiVersion": "2018-11-01",
      "name": "[parameters('loadBalancers_name')]",
      "location": "eastus",
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "LoadBalancerFrontEnd",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[reference('linkedTemplate').outputs.resourceID.value]"
              }
            }
          }
        ],
        "backendAddressPools": [],
        "loadBalancingRules": [],
        "probes": [],
        "inboundNatRules": [],
        "outboundNatRules": [],
        "inboundNatPools": []
      },
      "dependsOn": [
        "linkedTemplate"
      ]
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "name": "linkedTemplate",
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[uri(deployment().properties.templateLink.uri, 'publicip.json')]",
          "contentVersion": "1.0.0.0"
        },
        "parameters":{
          "publicIPAddresses_name":{"value": "[parameters('publicIPAddresses_name')]"}
        }
      }
    }
  ]
}
```

## <a name="deployment-history"></a>Dağıtım geçmişi

Kaynak Yöneticisi her şablon dağıtım geçmişini de ayrı bir dağıtım olarak işler. Dağıtım geçmişinde üç bağlantılı veya iç içe şablon içeren bir ana şablon şu şekilde görünür:

![Dağıtım geçmişi](./media/linked-templates/deployment-history.png)

Dağıtımdan sonra çıkış değerleri almak için bu ayrı girişleri geçmişinde kullanabilirsiniz. Aşağıdaki şablonu bir ortak IP adresi oluşturur ve IP adresini verir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "publicIPAddresses_name": {
      "type": "string"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/publicIPAddresses",
      "apiVersion": "2018-11-01",
      "name": "[parameters('publicIPAddresses_name')]",
      "location": "southcentralus",
      "properties": {
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Static",
        "idleTimeoutInMinutes": 4,
        "dnsSettings": {
          "domainNameLabel": "[concat(parameters('publicIPAddresses_name'), uniqueString(resourceGroup().id))]"
        }
      },
      "dependsOn": []
    }
  ],
  "outputs": {
    "returnedIPAddress": {
      "type": "string",
      "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]"
    }
  }
}
```

Önceki şablonda aşağıdaki şablon bağlantılar. Üç genel IP adresi oluşturur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "name": "[concat('linkedTemplate', copyIndex())]",
      "copy": {
        "count": 3,
        "name": "ip-loop"
      },
      "properties": {
        "mode": "Incremental",
        "templateLink": {
        "uri": "[uri(deployment().properties.templateLink.uri, 'static-public-ip.json')]",
        "contentVersion": "1.0.0.0"
        },
        "parameters":{
          "publicIPAddresses_name":{"value": "[concat('myip-', copyIndex())]"}
        }
      }
    }
  ]
}
```

Dağıtımdan sonra aşağıdaki PowerShell betiğini çıkış değerleri alabilir:

```azurepowershell-interactive
$loopCount = 3
for ($i = 0; $i -lt $loopCount; $i++)
{
  $name = 'linkedTemplate' + $i;
  $deployment = Get-AzResourceGroupDeployment -ResourceGroupName examplegroup -Name $name
  Write-Output "deployment $($deployment.DeploymentName) returned $($deployment.Outputs.returnedIPAddress.value)"
}
```

Veya, bir Bash Kabuğu'nda Azure CLI betiği:

```azurecli-interactive
#!/bin/bash

for i in 0 1 2;
do
  name="linkedTemplate$i";
  deployment=$(az group deployment show -g examplegroup -n $name);
  ip=$(echo $deployment | jq .properties.outputs.returnedIPAddress.value);
  echo "deployment $name returned $ip";
done
```

## <a name="securing-an-external-template"></a>Bir dış şablonu güvenliğini sağlama

Bağlı şablonun dışarıdan kullanılabilir olsa da, genel kullanıma sunuldu olması gerekmez. Yalnızca depolama hesabı sahibi tarafından erişilebilir bir özel depolama hesabına şablonunuza ekleyebilirsiniz. Ardından, dağıtım sırasında erişim sağlamak için paylaşılan erişim imzası (SAS) belirteci oluşturun. URI için bağlı şablonun SAS belirtecini ekleyin. SAS belirteci dahil olmak üzere bağlı şablon URI'si, belirteci güvenli bir dize olarak geçirilen olsa bile, dağıtım işlemleri günlüğe kaydedilir. Etkilenme sınırlamak için bir belirteç sona erme tarihi ayarlayın.

Parametre dosyasını bir SAS belirteci üzerinden erişim için sınırlı olabilir.

Şu anda [Azure Storage güvenlik duvarının](../../storage/common/storage-network-security.md)arkasındaki bir depolama hesabındaki bir şablona bağlayamazsınız.

Aşağıdaki örnek, bir şablona bağlanırken bir SAS belirteci geçirilecek gösterilmektedir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  "containerSasToken": { "type": "securestring" }
  },
  "resources": [
  {
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2018-05-01",
    "name": "linkedTemplate",
    "properties": {
    "mode": "Incremental",
    "templateLink": {
      "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
      "contentVersion": "1.0.0.0"
    }
    }
  }
  ],
  "outputs": {
  }
}
```

PowerShell'de, bir belirteç almak için kapsayıcı ve aşağıdaki komutları kullanarak şablonları dağıtabilirsiniz. **Containersastoken** parametresinin şablonda tanımlandığından emin olun. **New-AzResourceGroupDeployment** komutunda bir parametre değildir.

```azurepowershell-interactive
Set-AzCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

Azure CLI bir Bash kabuğunda için bir belirteç almak için kapsayıcı ve şablonları aşağıdaki kodla dağıtın:

```azurecli-interactive
#!/bin/bash

expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
  --resource-group ManageGroup \
  --name storagecontosotemplates \
  --query connectionString)
token=$(az storage container generate-sas \
  --name templates \
  --expiry $expiretime \
  --permissions r \
  --output tsv \
  --connection-string $connection)
url=$(az storage blob url \
  --container-name templates \
  --name parent.json \
  --output tsv \
  --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="example-templates"></a>Örnek şablonları

Aşağıdaki örnekler, bağlı şablonların'ın yaygın kullanımları gösterir.

|Ana şablon  |Bağlantılı şablon |Açıklama  |
|---------|---------| ---------|
|[Hello World](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworldparent.json) |[bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworld.json) | Bağlantılı şablondan dizeyi döndürür. |
|[Genel IP adresi ile Load Balancer](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) |[bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) |Bağlantılı şablondan genel IP adresini getirir ve yük dengeleyici bu değeri ayarlar. |
|[Birden çok IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip-parent.json) | [bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip.json) |Bağlantılı şablonunda birden fazla genel IP adresi oluşturur.  |

## <a name="next-steps"></a>Sonraki adımlar

* Öğreticiye gitmek için bkz. [öğretici: bağlı Azure Resource Manager şablonları oluşturma](template-tutorial-create-linked-templates.md).
* Kaynaklarınızın dağıtım sırasını tanımlama hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonlarda bağımlılıkları tanımlama](define-resource-dependency.md).
* Tek bir kaynağı tanımlama ve birçok örneğini oluşturma hakkında bilgi edinmek için, bkz. [Azure Resource Manager birden fazla kaynak örneği oluşturma](copy-resources.md).
* Bir depolama hesabında bir şablon ayarlama ve SAS belirteci oluşturma adımları için bkz. [Kaynak Yöneticisi şablonları ile kaynak dağıtma ve Azure PowerShell](deploy-powershell.md) [Kaynak Yöneticisi ŞABLONLARı ve Azure CLI ile kaynak dağıtma](deploy-cli.md).
