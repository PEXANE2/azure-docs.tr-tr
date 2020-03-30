---
title: Dağıtım için bağlantı şablonları
description: Modüler bir şablon çözümü oluşturmak için Azure Kaynak Yöneticisi şablonundaki bağlantılı şablonların nasıl kullanılacağını açıklar. Parametrelerin nasıl geçirilir, parametre dosyası belirtilir ve dinamik olarak oluşturulan URL'leri gösterir.
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 322797383ee865ceb66c44793387da827aeb8879
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131920"
---
# <a name="using-linked-and-nested-templates-when-deploying-azure-resources"></a>Azure kaynaklarını dağıtırken bağlı ve iç içe şablonları kullanma

Karmaşık çözümleri dağıtmak için şablonunuzu ilgili birçok şablona ayırabilir ve bunları ana şablon aracılığıyla birlikte dağıtabilirsiniz. İlgili şablonlar, ana şablona katıştırılmış ayrı dosyalar veya şablon sözdizimi olabilir. Bu makalede, ana şablondan bir bağlantı üzerinden başvurulan ayrı bir şablon dosyasına başvurmak için **bağlantılı şablon** terimini kullanır. Ana şablon içinde gömülü şablon sözdizimine başvurmak için **iç içe şablon** terimini kullanır.

Küçük ve orta ölçekli çözümler için, tek bir şablonun anlaşılması ve bakımı daha kolaydır. Tüm kaynakları ve değerleri tek bir dosyada görebilirsiniz. Gelişmiş senaryolar için, bağlantılı şablonlar çözümü hedeflenen bileşenlere ayırmanızı sağlar. Bu şablonları diğer senaryolar için kolayca yeniden kullanabilirsiniz.

Bir öğretici için [Bkz. Öğretici: Bağlantılı Azure Kaynak Yöneticisi şablonları oluşturun.](template-tutorial-create-linked-templates.md)

> [!NOTE]
> Bağlantılı veya iç içe olan şablonlar için yalnızca [Artımlı](deployment-modes.md) dağıtım modunu kullanabilirsiniz.
>

## <a name="nested-template"></a>İç içe şablon

Şablona bir yuva yapmak için, ana şablonunuza dağıtım [kaynağı](/azure/templates/microsoft.resources/deployments) ekleyin. **Şablon** özelliğinde, şablon sözdizimini belirtin.

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

Aşağıdaki örnek, iç içe geçen bir şablon aracılığıyla bir depolama hesabı dağıtMaktadır.

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

### <a name="expression-evaluation-scope-in-nested-templates"></a>İç içe şablonlarda ifade değerlendirme kapsamı

İç içe bir şablon kullanırken, şablon ifadelerinin üst şablon kapsamında mı yoksa iç içe doğru şablon kapsamında mı değerlendirildiğini belirtebilirsiniz. Kapsam, [kaynak Grubu](template-functions-resource.md#resourcegroup) ve [abonelik](template-functions-resource.md#subscription) gibi parametrelerin, değişkenlerin ve işlevlerin nasıl çözüleceğini belirler.

Kapsamı özellik üzerinden `expressionEvaluationOptions` ayarlarsınız. Varsayılan olarak, `expressionEvaluationOptions` özellik ana `outer`şablon kapsamını kullandığı anlamına gelir ayarlanır. İç içe `inner` geçen şablon kapsamında değerlendirilecek ifadelere neden olacak değeri ayarlayın.

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

Aşağıdaki şablon, şablon ifadelerinin kapsama göre nasıl çözüldüğünü gösterir. Hem üst şablonda hem de iç içe geçen şablonda tanımlanan bir değişken içerir. `exampleVar` Değişkenin değerini döndürür.

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

'deki `exampleVar` `scope` özelliğin `expressionEvaluationOptions`değerine bağlı olarak değişikliklerin değeri Aşağıdaki tablo, her iki kapsam için sonuçları gösterir.

| `expressionEvaluationOptions` `scope` | Çıktı |
| ----- | ------ |
| Iç | iç içe şablondan |
| dış (veya varsayılan) | üst şablondan |

Aşağıdaki örnekte bir SQL sunucusu dağıtılır ve parola için kullanılacak anahtar kasa gizli alır. Kapsam, dinamik `inner` olarak anahtar kasa kimliğini (dış şablonlara `adminPassword.reference.keyVault` `parameters`bakın) oluşturduğu ndan ve iç içe geçen şablona bir parametre olarak ilettiği için ayarlanır.

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
> Kapsam `outer`ayarlandığında, iç içe geçen `reference` şablonda dağıttığınız bir kaynak için iç içe geçen şablonun çıktılar bölümündeki işlevi kullanamazsınız. İç içe geçmiş bir şablonda dağıtılmış bir kaynağın değerlerini döndürmek için kapsamı kullanın `inner` veya iç içe geçmiş şablonunuzu bağlantılı bir şablona dönüştürün.

## <a name="linked-template"></a>Bağlantılı şablon

Şablonbağlamak için ana şablonunuza [dağıtım kaynağı](/azure/templates/microsoft.resources/deployments) ekleyin. **templateLink** özelliğinde, şablonun URI'sini eklemek üzere belirtin. Aşağıdaki örnek, yeni bir depolama hesabı dağıtan şablona bağlanır.

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

Bağlı bir şablona başvururken, `uri` değeri yerel bir dosya veya yalnızca yerel ağınızda kullanılabilen bir dosya olmamalıdır. **Http** veya **https**olarak indirilebilir bir URI değeri sağlamalısınız. 

> [!NOTE]
>
> Örneğin, örneğin parametreyi kullanarak **http** veya **https**kullanan bir şeye çözüm `_artifactsLocation` sağlayan parametreleri kullanarak şablonlara başvurun:`"uri": "[concat(parameters('_artifactsLocation'), '/shared/os-disk-parts-md.json', parameters('_artifactsLocationSasToken'))]",`



Kaynak Yöneticisi şablona erişebilmeli. Bir seçenek, bağlantılı şablonunuzu bir depolama hesabına yerleştirmek ve söz öğe için URI'yi kullanmaktır.

### <a name="parameters-for-linked-template"></a>Bağlantılı şablon için parametreler

Bağlı şablonunuzun parametrelerini harici bir dosyada veya satır satırda sağlayabilirsiniz. Harici parametre dosyası sağlarken, **parametersLink** özelliğini kullanın:

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

Parametre değerlerini satır içinde geçirmek için **parametreler** özelliğini kullanın.

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

Hem satır içinde parametreleri hem de parametre dosyasına bağlantı kullanamazsınız. Dağıtım, her ikisi de `parametersLink` `parameters` ve belirtildiğinde bir hatayla başarısız olur.

## `contentVersion`

`contentVersion` Özellik `templateLink` veya `parametersLink` özellik için sağlamak zorunda değilsiniz. Bir `contentVersion`, şablonun geçerli sürümü sağlamazsanız dağıtılır. İçerik sürümü için bir değer sağlarsanız, bağlı şablondaki sürümle eşleşmelidir; aksi takdirde, dağıtım bir hata ile başarısız olur.

## <a name="using-variables-to-link-templates"></a>Şablonları bağlamak için değişkenleri kullanma

Önceki örneklerde şablon bağlantıları için sabit kodlanmış URL değerleri gösterilmektedir. Bu yaklaşım basit bir şablon için işe yarayabilir, ancak modüler şablonlar büyük bir dizi için iyi çalışmıyor. Bunun yerine, ana şablon için temel URL depolayan statik bir değişken oluşturabilir ve ardından bu temel URL'den bağlantılı şablonlar için dinamik olarak URL'ler oluşturabilirsiniz. Bu yaklaşımın yararı, yalnızca ana şablondaki statik değişkeni değiştirmeniz gerektiğinden şablonu kolayca taşıyabilmeniz veya çatallayabilmenizdir. Ana şablon, ayrıştırılan şablon boyunca doğru ÜR'lerden geçer.

Aşağıdaki örnek, bağlantılı şablonlar **(sharedTemplateUrl** ve **vmTemplate)** için iki URL oluşturmak için temel URL'nin nasıl kullanılacağını gösterir.

```json
"variables": {
  "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
  "sharedTemplateUrl": "[uri(variables('templateBaseUrl'), 'shared-resources.json')]",
  "vmTemplateUrl": "[uri(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Geçerli şablonun temel URL'sini almak için [dağıtım()](template-functions-deployment.md#deployment) ve aynı konumdaki diğer şablonların URL'sini almak için de kullanabilirsiniz. Bu yaklaşım, şablon konumunuz değişirse veya şablon dosyasındaki sabit kodlama URL'lerinden kaçınmak istiyorsanız yararlıdır. templateLink özelliği yalnızca URL'li uzak bir şablona bağlanırken döndürülür. Yerel bir şablon kullanıyorsanız, bu özellik kullanılamaz.

```json
"variables": {
  "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

Sonuç olarak, değişkeni bir `uri` `templateLink` özelliğin özelliğinde kullanırsınız.

```json
"templateLink": {
 "uri": "[variables('sharedTemplateUrl')]",
 "contentVersion":"1.0.0.0"
}
```

## <a name="using-copy"></a>Kopyalamayı kullanma

İç içe şablonu olan bir kaynağın birden çok örneği oluşturmak için, kopya öğesini **Microsoft.Resources/deployments** kaynağı düzeyinde ekleyin. Veya kapsam içse, kopyayı iç içe doğru şablona ekleyebilirsiniz.

Aşağıdaki örnek şablon, iç içe bir şablonla kopyanın nasıl kullanılacağını gösterir.

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

## <a name="get-values-from-linked-template"></a>Bağlı şablondan değerleri alma

Bağlı bir şablondan çıktı değeri almak için, sözdizimi `"[reference('deploymentName').outputs.propertyName.value]"`ile özellik değerini alın: .

Bağlı bir şablondan çıktı özelliği alırken, özellik adı tire içermemelidir.

Aşağıdaki örnekler, bağlantılı bir şablona nasıl başvurulup çıktı değeri alınabildiğini gösterir. Bağlı şablon basit bir iletiyi döndürür.  İlk olarak, bağlantılı şablon:

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

Ana şablon bağlı şablonu dağır ve döndürülen değeri alır. Dağıtım kaynağına ada göre başvurur ve bağlantılı şablon tarafından döndürülen özelliğin adını kullanır.

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

Diğer kaynak türlerinde olduğu gibi, bağlı şablon ve diğer kaynaklar arasındaki bağımlılıkları ayarlayabilirsiniz. Diğer kaynaklar bağlantılı şablondan bir çıktı değeri gerektirdiğinde, bağlantılı şablonun kendilerinden önce dağıtıldığınızdan emin olun. Veya bağlantılı şablon diğer kaynaklara dayandığında, bağlantılı şablondan önce diğer kaynakların dağıtıldıktan emin olun.

Aşağıdaki örnekte, ortak bir IP adresi dağıtan ve azure kaynağının kaynak kimliğini bu genel IP için döndüren bir şablon gösterilmektedir:

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

Yük dengeleyicisini dağıtırken önceki şablondaki genel IP adresini kullanmak için şablona bağlantı verin `Microsoft.Resources/deployments` ve kaynağa bağımlılık bildirin. Yük dengeleyicisindeki genel IP adresi, bağlı şablondan çıktı değerine ayarlanır.

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
                // this is where the output value from linkedTemplate is used
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
      // This is where the dependency is declared
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

Kaynak Yöneticisi, her şablonu dağıtım geçmişinde ayrı bir dağıtım olarak işler. Dağıtım geçmişinde üç bağlantılı veya iç içe geçmiş şabloniçeren bir ana şablon aşağıdaki gibi görünür:

![Dağıtım geçmişi](./media/linked-templates/deployment-history.png)

Dağıtımdan sonra çıktı değerlerini almak için geçmişteki bu ayrı girişleri kullanabilirsiniz. Aşağıdaki şablon ortak bir IP adresi oluşturur ve IP adresini çıkar:

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

Aşağıdaki şablon önceki şablona bağlantılar. Üç genel IP adresi oluşturur.

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

Dağıtımdan sonra, aşağıdaki PowerShell komut dosyasıyla çıktı değerlerini alabilirsiniz:

```azurepowershell-interactive
$loopCount = 3
for ($i = 0; $i -lt $loopCount; $i++)
{
  $name = 'linkedTemplate' + $i;
  $deployment = Get-AzResourceGroupDeployment -ResourceGroupName examplegroup -Name $name
  Write-Output "deployment $($deployment.DeploymentName) returned $($deployment.Outputs.returnedIPAddress.value)"
}
```

Veya, Bash kabuğundaki Azure CLI komut dosyası:

```azurecli-interactive
#!/bin/bash

for i in 0 1 2;
do
  name="linkedTemplate$i";
  deployment=$(az deployment group show -g examplegroup -n $name);
  ip=$(echo $deployment | jq .properties.outputs.returnedIPAddress.value);
  echo "deployment $name returned $ip";
done
```

## <a name="securing-an-external-template"></a>Harici bir şablonu koruma

Bağlantılı şablon dışarıdan kullanılabilir olsa da, genel olarak herkese açık olması gerekmez. Şablonunuzu yalnızca depolama hesabı sahibinin erişebileceği özel bir depolama hesabına ekleyebilirsiniz. Ardından, dağıtım sırasında erişimi etkinleştirmek için paylaşılan bir erişim imzası (SAS) belirteci oluşturursunuz. Bağlı şablon için URI'ye sas belirteci eklersiniz. Belirteç güvenli bir dize olarak geçirilmiş olsa da, SAS belirteci de dahil olmak üzere bağlantılı şablonun URI'si dağıtım işlemlerinde günlüğe kaydedilir. Pozlamayı sınırlamak için belirteç için bir son kullanma tarihi ayarlayın.

Parametre dosyası, Bir SAS belirteci aracılığıyla erişimle de sınırlandırılabilir.

Şu anda, [Azure Depolama güvenlik duvarının](../../storage/common/storage-network-security.md)arkasındaki depolama hesabındaki şablona bağlanamazsınız.

Aşağıdaki örnek, şablona bağlantı verirken SAS belirtecinin nasıl geçirilen ibareyi gösterir:

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

PowerShell'de, kapsayıcı için bir belirteç alır ve şablonları aşağıdaki komutlarla dağıtabilirsiniz. **Şablonda SasToken** parametresi tanımlandığına dikkat edin. **Bu, Yeni-AzResourceGroupDeployment** komutundaki bir parametre değildir.

```azurepowershell-interactive
Set-AzCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

Bash kabuğundaki Azure CLI için, kapsayıcı için bir belirteç alır ve şablonları aşağıdaki kodla dağıtırsınız:

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
az deployment group create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="example-templates"></a>Örnek şablonlar

Aşağıdaki örnekler, bağlantılı şablonların yaygın kullanımlarını gösterir.

|Ana şablon  |Bağlantılı şablon |Açıklama  |
|---------|---------| ---------|
|[Merhaba Dünya](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworldparent.json) |[bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworld.json) | Bağlı şablondan dize döndürür. |
|[Ortak IP adresine sahip Yük Dengeleyicisi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) |[bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) |Bağlantılı şablondan ortak IP adresini verir ve yük dengeleyicisinde bu değeri ayarlar. |
|[Birden çok IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip-parent.json) | [bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip.json) |Bağlantılı şablonda birkaç genel IP adresi oluşturur.  |

## <a name="next-steps"></a>Sonraki adımlar

* Bir öğreticiyi gözden geçirmek için [Bkz. Öğretici: Bağlantılı Azure Kaynak Yöneticisi şablonları oluşturun.](template-tutorial-create-linked-templates.md)
* Kaynaklarınız için dağıtım sırasını tanımlama hakkında bilgi edinmek için Azure [Kaynak Yöneticisi şablonlarında bağımlılıkları tanımlama'ya](define-resource-dependency.md)bakın.
* Bir kaynağı nasıl tanımlayabilirsiniz, ancak bunun birçok örneğini nasıl oluşturabilirsiniz öğrenmek için Azure [Kaynak Yöneticisi'nde birden çok kaynak örneği oluşturma](copy-resources.md)bölümüne bakın.
* Depolama hesabında şablon oluşturma ve SAS belirteci oluşturma yla ilgili adımlar için [bkz.](deploy-powershell.md) [Deploy resources with Resource Manager templates and Azure CLI](deploy-cli.md)
