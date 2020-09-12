---
title: Dağıtım için şablonları bağlama
description: Modüler şablon çözümü oluşturmak için Azure Resource Manager şablonda bağlantılı şablonların nasıl kullanılacağını açıklar. Parametre değerlerinin nasıl geçirileceğini, bir parametre dosyası ve dinamik olarak oluşturulan URL 'Leri gösterir.
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: f1fe07faeaddae3367fb1f8b4a37f7b0630b6e83
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89535567"
---
# <a name="using-linked-and-nested-templates-when-deploying-azure-resources"></a>Azure kaynaklarını dağıtırken bağlı ve iç içe şablonları kullanma

Karmaşık çözümleri dağıtmak için şablonunuzu birçok ilgili şablona bölebilir ve ardından bunları bir ana şablon aracılığıyla dağıtabilirsiniz. İlgili şablonlar, ana şablon içine katıştırılmış ayrı dosyalar veya şablon söz dizimi olabilir. Bu makalede, ana şablondan bir bağlantı aracılığıyla başvurulan ayrı bir şablon dosyasına başvurmak için **bağlantılı şablon** terimi kullanılmaktadır. Ana şablon içindeki katıştırılmış şablon söz dizimini ifade etmek için **iç içe geçmiş şablon** terimini kullanır.

Küçük ve orta ölçekli çözümler için tek bir şablonun anlaşılması ve korunması daha kolay olur. Tüm kaynakları ve değerleri tek bir dosyada görebilirsiniz. Bağlantılı şablonlar, gelişmiş senaryolar için çözümü hedeflenen bileşenlere bölebilir. Bu şablonları diğer senaryolar için kolayca yeniden kullanabilirsiniz.

Öğretici için bkz. [öğretici: bağlı Azure Resource Manager şablonları oluşturma](./deployment-tutorial-linked-template.md).

> [!NOTE]
> Bağlı veya iç içe şablonlar için, Dağıtım modunu yalnızca [artımlı](deployment-modes.md)olarak ayarlayabilirsiniz. Ancak, ana şablon, tamamlanmış modda dağıtılabilir. Ana şablonu tüm modda dağıtırsanız, bağlantılı veya iç içe yerleştirilmiş şablon aynı kaynak grubunu hedefliyorsa, bağlantılı veya iç içe yerleştirilmiş şablonda dağıtılan kaynaklar, tamamlanmış mod dağıtımı değerlendirmesine dahil edilir. Ana şablonda dağıtılan kaynakların birleştirilmiş koleksiyonu ve bağlantılı veya iç içe yerleştirilmiş şablonlar kaynak grubundaki mevcut kaynaklarla karşılaştırılır. Bu Birleşik koleksiyonda bulunmayan kaynaklar silinir.
>
> Bağlantılı veya iç içe yerleştirilmiş şablon farklı bir kaynak grubunu hedefliyorsa, bu dağıtım artımlı mod kullanır.
>

## <a name="nested-template"></a>İç içe şablon

Bir şablonu iç içe aktarmak için ana şablonunuza bir [dağıtımlar kaynağı](/azure/templates/microsoft.resources/deployments) ekleyin. **Şablon** özelliğinde, Şablon sözdizimini belirtin.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-10-01",
      "name": "nestedTemplate1",
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
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-10-01",
      "name": "nestedTemplate1",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

İç içe geçmiş bir şablon kullanırken, şablon ifadelerinin üst şablon kapsamında mi yoksa iç içe yerleştirilmiş şablon içinde mi değerlendirileceğini belirtebilirsiniz. Kapsam, parametrelerin, değişkenlerin ve [resourceGroup](template-functions-resource.md#resourcegroup) ve [abonelik](template-functions-resource.md#subscription) gibi işlevlerin nasıl çözümlendiğini belirler.

Kapsamını özelliği aracılığıyla ayarlarsınız `expressionEvaluationOptions` . Varsayılan olarak, `expressionEvaluationOptions` özelliği olarak ayarlanır, yani `outer` üst şablon kapsamını kullanır. `inner`İfadenin, iç içe geçmiş şablonun kapsamı içinde değerlendirilmesini sağlamak için değerini olarak ayarlayın.

```json
{
  "type": "Microsoft.Resources/deployments",
  "apiVersion": "2019-10-01",
  "name": "nestedTemplate1",
  "properties": {
  "expressionEvaluationOptions": {
    "scope": "inner"
  },
  ...
```

Aşağıdaki şablon, şablon ifadelerinin kapsama göre nasıl çözümlendiğini gösterir. `exampleVar`Hem üst şablonda hem de iç içe yerleştirilmiş şablonda tanımlı adlı bir değişken içerir. Değişkenin değerini döndürür.

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
      "apiVersion": "2019-10-01",
      "name": "nestedTemplate1",
      "properties": {
        "expressionEvaluationOptions": {
          "scope": "inner"
        },
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

`exampleVar`İçindeki özelliğinin değerine bağlı olarak değişir `scope` `expressionEvaluationOptions` . Aşağıdaki tabloda her iki kapsamın sonuçları gösterilmektedir.

| `expressionEvaluationOptions` kapsam | Çıkış |
| ----- | ------ |
| Dahili | iç içe şablondan |
| dış (veya varsayılan) | üst şablondan |

Aşağıdaki örnek, bir SQL Server dağıtır ve parola için kullanılacak bir Anahtar Kasası gizli anahtarı alır. Kapsam, `inner` anahtar KASASı kimliğini dinamik olarak oluşturduğundan ( `adminPassword.reference.keyVault` dış şablonlarda bkz `parameters` .) olarak ayarlanır ve bunu iç içe geçmiş şablona bir parametre olarak geçirir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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
      "apiVersion": "2019-10-01",
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
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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
> Kapsam olarak ayarlandığında `outer` , `reference` iç içe yerleştirilmiş şablonda dağıttığınız bir kaynak için iç içe şablonun çıktılar bölümünde işlevini kullanamazsınız. Dağıtılan bir kaynağın değerlerini iç içe geçmiş bir şablonda döndürmek için `inner` kapsam kullanın veya iç içe geçmiş şablonunuzu bağlı bir şablona dönüştürün.

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
      "apiVersion": "2019-10-01",
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

Bağlı bir şablona başvururken, değeri `uri` yerel bir dosya veya yerel ağınızda bulunan bir dosya olmamalıdır. **Http** veya **https**olarak indirilebilir bir URI değeri sağlamanız gerekir.

> [!NOTE]
>
> Son olarak, **http** veya **https**kullanan bir şeyi kullanarak, örneğin, şöyle bir parametre kullanarak şablonlara başvurabilirsiniz `_artifactsLocation` : `"uri": "[concat(parameters('_artifactsLocation'), '/shared/os-disk-parts-md.json', parameters('_artifactsLocationSasToken'))]",`

Kaynak Yöneticisi şablona erişebilmelidir. Bir seçenek, bağlantılı şablonunuzu bir depolama hesabına yerleştirmeli ve bu öğe için URI 'yi kullanacaktır.

### <a name="parameters-for-linked-template"></a>Bağlantılı şablon parametreleri

Bağlı şablonunuz için parametreleri bir dış dosyada ya da satır içi olarak sağlayabilirsiniz. Bir dış parametre dosyası sağlarken, **Parameterslink** özelliğini kullanın:

```json
"resources": [
  {
  "type": "Microsoft.Resources/deployments",
  "apiVersion": "2019-10-01",
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
   "apiVersion": "2019-10-01",
   "name": "linkedTemplate",
   "properties": {
     "mode": "Incremental",
     "templateLink": {
      "uri":"https://mystorageaccount.blob.core.windows.net/AzureTemplates/newStorageAccount.json",
      "contentVersion":"1.0.0.0"
     },
     "parameters": {
      "storageAccountName":{"value": "[parameters('storageAccountName')]"}
    }
   }
  }
]
```

Hem satır içi parametreleri hem de bir parametre dosyası bağlantısını kullanamazsınız. Hem hem de belirtildiğinde dağıtım bir hata ile başarısız olur `parametersLink` `parameters` .

## <a name="template-specs"></a>Şablon belirtimleri

Bağlı şablonlarınızı erişilebilir bir uç noktada tutmak yerine, ana şablonu ve onun bağlantılı şablonlarını dağıtabileceğiniz tek bir varlığa paketleyen bir [şablon belirtimi](template-specs.md) oluşturabilirsiniz. Şablon belirtimi, Azure aboneliğinizdeki bir kaynaktır. Şablonu kuruluşunuzdaki kullanıcılarla güvenli bir şekilde paylaşmayı kolaylaştırır. Şablon belirtimine erişim vermek için rol tabanlı erişim denetimi (RBAC) kullanırsınız. Bu özellik şu anda önizleme aşamasındadır.

Daha fazla bilgi için bkz.

- [Öğretici: bağlantılı şablonlarla bir şablon belirtimi oluşturun](./template-specs-create-linked.md).
- [Öğretici: bir şablon belirtimini bağlantılı şablon olarak dağıtın](./template-specs-deploy-linked-template.md).

## <a name="contentversion"></a>contentVersion

`contentVersion`Veya özelliği için özelliği sağlamanız gerekmez `templateLink` `parametersLink` . Bir sağlamazsanız `contentVersion` , şablonun geçerli sürümü dağıtılır. İçerik sürümü için bir değer sağlarsanız, bu bağlantı bağlantılı şablondaki sürümle eşleşmelidir; Aksi takdirde, dağıtım hata vererek başarısız olur.

## <a name="using-variables-to-link-templates"></a>Şablonları bağlamak için değişkenleri kullanma

Önceki örneklerde, şablon bağlantıları için sabit kodlanmış URL değerleri gösteriliyordu. Bu yaklaşım basit bir şablon için çalışabilir, ancak büyük bir modüler şablonlar kümesi için iyi çalışmaz. Bunun yerine, ana şablon için temel URL 'YI depolayan bir statik değişken oluşturabilir ve ardından bu temel URL 'den bağlantılı şablonlar için dinamik olarak URL 'Ler oluşturabilirsiniz. Bu yaklaşımın avantajı, yalnızca ana şablondaki statik değişkeni değiştirmeniz gerektiği için şablonu kolayca taşıyabilir veya çataldan taşıyabilirsiniz. Ana şablon, ayrıştırılmış şablonun tamamında doğru URI 'Leri geçirir.

Aşağıdaki örnek, bağlantılı şablonlar (**Sharedtemplateurl** ve **vmtemplate**) için iki URL oluşturmak üzere temel URL 'yi nasıl kullanacağınızı gösterir.

```json
"variables": {
  "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
  "sharedTemplateUrl": "[uri(variables('templateBaseUrl'), 'shared-resources.json')]",
  "vmTemplateUrl": "[uri(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Ayrıca, geçerli şablonun temel URL 'sini almak için [Deployment ()](template-functions-deployment.md#deployment) kullanabilirsiniz ve bunu, aynı konumdaki DIĞER şablonların URL 'sini almak için kullanabilirsiniz. Bu yaklaşım, şablon konumunuz değişirse veya şablon dosyasında sabit kodlama URL 'Lerinin oluşmasını önlemek istediğinizde yararlıdır. TemplateLink özelliği yalnızca bir URL ile uzak şablona bağlanılırken döndürülür. Yerel bir şablon kullanıyorsanız, bu özellik kullanılamaz.

```json
"variables": {
  "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

Sonuç olarak, değişkenini `uri` bir özelliğin özelliğinde kullanırsınız `templateLink` .

```json
"templateLink": {
 "uri": "[variables('sharedTemplateUrl')]",
 "contentVersion":"1.0.0.0"
}
```

## <a name="using-copy"></a>Kopyayı kullanma

İç içe geçmiş şablonla bir kaynağın birden çok örneğini oluşturmak için, **Microsoft. resources/dağıtımlar** kaynağı düzeyinde kopyalama öğesini ekleyin. Ya da kapsam iç ise, iç içe geçmiş şablon içinde kopyayı ekleyebilirsiniz.

Aşağıdaki örnek şablon, kopyalamanın iç içe geçmiş bir şablonla nasıl kullanılacağını göstermektedir.

```json
"resources": [
  {
  "type": "Microsoft.Resources/deployments",
  "apiVersion": "2019-10-01",
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
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Bağlı bir şablondan çıkış değeri almak için, özellik değerini şu şekilde olan sözdizimiyle alın: `"[reference('deploymentName').outputs.propertyName.value]"` .

Bağlantılı şablondan çıkış özelliği alınırken, özellik adı bir tire içermemelidir.

Aşağıdaki örneklerde, bağlantılı bir şablona nasıl başvurulacağını ve bir çıkış değerinin nasıl alınacağını gösterilmektedir. Bağlantılı şablon basit bir ileti döndürür.  İlk olarak, bağlantılı şablon:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Ana şablon, bağlantılı şablonu dağıtır ve döndürülen değeri alır. Dağıtım kaynağına adına göre başvurduğuna ve bağlı şablon tarafından döndürülen özelliğin adını kullandığını unutmayın.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-10-01",
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

Diğer kaynak türlerinde olduğu gibi, bağlantılı şablon ve diğer kaynaklar arasında bağımlılıklar da ayarlayabilirsiniz. Diğer kaynaklar bağlantılı şablondan bir çıkış değeri gerektirdiğinde, bağlantılı şablonun uygulamadan önce dağıtıldığından emin olun. Ya da bağlantılı şablon diğer kaynakları temel aldığından, bağlantılı şablondan önce diğer kaynakların dağıtıldığından emin olun.

Aşağıdaki örnek, genel bir IP adresi dağıtan ve bu genel IP için Azure kaynağının kaynak KIMLIĞINI döndüren bir şablonu gösterir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yük dengeleyici dağıtma sırasında önceki şablondaki genel IP adresini kullanmak için, şablona bağlayın ve kaynak üzerinde bir bağımlılık bildirin `Microsoft.Resources/deployments` . Yük dengeleyicideki genel IP adresi, bağlantılı şablondan çıkış değerine ayarlanır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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
      "apiVersion": "2019-10-01",
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

Kaynak Yöneticisi her bir şablonu dağıtım geçmişinde ayrı bir dağıtım olarak işler. Dağıtım geçmişinde üç bağlantılı veya iç içe şablon içeren bir ana şablon şu şekilde görünür:

![Dağıtım geçmişi](./media/linked-templates/deployment-history.png)

Bu ayrı girdileri, dağıtımdan sonra çıkış değerlerini almak için geçmişte kullanabilirsiniz. Aşağıdaki şablon bir genel IP adresi oluşturur ve IP adresini verir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Aşağıdaki şablon önceki şablona bağlantı sağlar. Üç genel IP adresi oluşturur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-10-01",
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

Dağıtımdan sonra aşağıdaki PowerShell betiği ile çıkış değerlerini alabilirsiniz:

```azurepowershell-interactive
$loopCount = 3
for ($i = 0; $i -lt $loopCount; $i++)
{
  $name = 'linkedTemplate' + $i;
  $deployment = Get-AzResourceGroupDeployment -ResourceGroupName examplegroup -Name $name
  Write-Output "deployment $($deployment.DeploymentName) returned $($deployment.Outputs.returnedIPAddress.value)"
}
```

Veya, bir bash kabuğu 'nda Azure CLı betiği:

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

## <a name="securing-an-external-template"></a>Dış şablonun güvenliğini sağlama

Bağlı şablon dışarıdan kullanılabilir olmalıdır, ancak genel kullanıma açık olması gerekmez. Şablonunuzu yalnızca depolama hesabı sahibi tarafından erişilebilen bir özel depolama hesabına ekleyebilirsiniz. Daha sonra, dağıtım sırasında erişimi etkinleştirmek için bir paylaşılan erişim imzası (SAS) belirteci oluşturursunuz. Bu SAS belirtecini bağlantılı şablon için URI 'ye eklersiniz. Belirteç güvenli bir dize olarak geçirilse de, SAS belirteci dahil olmak üzere bağlantılı şablonun URI 'SI dağıtım işlemlerinde günlüğe kaydedilir. Pozlamayı sınırlandırmak için, belirteç için bir süre sonu ayarlayın.

Parametre dosyası bir SAS belirteci aracılığıyla da sınırlı olabilir.

Şu anda [Azure Storage güvenlik duvarının](../../storage/common/storage-network-security.md)arkasındaki bir depolama hesabındaki bir şablona bağlayamazsınız.

> [!IMPORTANT]
> Bağlı şablonunuzu bir SAS belirteci ile güvenli hale getirmek yerine, bir [şablon belirtimi](template-specs.md)oluşturmayı düşünün. Şablon belirtimi, ana şablonu ve bağlı şablonlarını Azure aboneliğinizde bir kaynak olarak güvenli bir şekilde depolar. Şablonu dağıtması gereken kullanıcılara erişim vermek için RBAC 'yi kullanırsınız.

Aşağıdaki örnek, bir şablona bağlanırken bir SAS belirtecinin nasıl geçirileceğini gösterir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  "containerSasToken": { "type": "securestring" }
  },
  "resources": [
  {
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2019-10-01",
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

PowerShell 'de, kapsayıcı için bir belirteç alır ve aşağıdaki komutlarla şablonları dağıtırsınız. **Containersastoken** parametresinin şablonda tanımlandığından emin olun. **New-AzResourceGroupDeployment** komutunda bir parametre değildir.

```azurepowershell-interactive
Set-AzCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

Bash kabuğu 'nda Azure CLı için, kapsayıcı için bir belirteç alır ve aşağıdaki kodla şablonları dağıtabilirsiniz:

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

## <a name="example-templates"></a>Örnek Şablonlar

Aşağıdaki örneklerde, bağlantılı şablonların yaygın kullanımları gösterilmektedir.

|Ana şablon  |Bağlantılı şablon |Description  |
|---------|---------| ---------|
|[Hello World](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworldparent.json) |[bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworld.json) | Bağlantılı şablondan dize döndürür. |
|[Genel IP adresi ile Load Balancer](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) |[bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) |Bağlı şablondan ortak IP adresini döndürür ve yük dengeleyicide bu değeri ayarlar. |
|[Birden çok IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip-parent.json) | [bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip.json) |Bağlı şablonda birkaç genel IP adresi oluşturur.  |

## <a name="next-steps"></a>Sonraki adımlar

* Öğreticiye gitmek için bkz. [öğretici: bağlı Azure Resource Manager şablonları oluşturma](./deployment-tutorial-linked-template.md).
* Kaynaklarınızın dağıtım sırasını tanımlama hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonlarda bağımlılıkları tanımlama](define-resource-dependency.md).
* Tek bir kaynağı tanımlama ve birçok örneğini oluşturma hakkında bilgi edinmek için, bkz. [Azure Resource Manager birden fazla kaynak örneği oluşturma](copy-resources.md).
* Bir depolama hesabında bir şablon ayarlama ve SAS belirteci oluşturma adımları için bkz. [Kaynak Yöneticisi şablonları ile kaynak dağıtma ve Azure PowerShell](deploy-powershell.md) [Kaynak Yöneticisi ŞABLONLARı ve Azure CLI ile kaynak dağıtma](deploy-cli.md).
