---
title: Azure dağıtımı için bağlantı şablonları | Microsoft Docs
description: Modüler şablon çözümü oluşturmak için Azure Resource Manager şablonda bağlantılı şablonların nasıl kullanılacağını açıklar. Parametre değerlerinin nasıl geçirileceğini, bir parametre dosyası ve dinamik olarak oluşturulan URL 'Leri gösterir.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: tomfitz
ms.openlocfilehash: 59af553f4080ca86e964b75234e4d812297d8541
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827341"
---
# <a name="using-linked-and-nested-templates-when-deploying-azure-resources"></a>Azure kaynaklarını dağıttığınızda bağlı ve iç içe Şablonlar kullanma

Çözümünüzü dağıtmak için, çok sayıda ilgili şablonla tek bir şablon ya da bir ana şablon kullanabilirsiniz. İlgili şablonlar, ana şablondan ya da ana şablon içinde iç içe yerleştirilmiş şablonlardan bağlantılı ayrı dosyalar olabilir.

Küçük ve orta ölçekli çözümler için tek bir şablonun anlaşılması ve korunması daha kolay olur. Tüm kaynakları ve değerleri tek bir dosyada görebilirsiniz. Bağlantılı şablonlar, gelişmiş senaryolar için çözümü hedeflenen bileşenlere bölebilir. Bu şablonları diğer senaryolar için kolayca yeniden kullanabilirsiniz.

Bağlı şablonları kullanırken, dağıtım sırasında parametre değerlerini alan bir ana şablon oluşturursunuz. Ana şablon tüm bağlantılı şablonları içerir ve gerektiğinde değerleri bu şablonlara geçirir.

Öğretici için bkz. [öğretici: bağlı Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-linked-templates.md).

> [!NOTE]
> Bağlantılı veya iç içe şablonlar için yalnızca [artımlı](deployment-modes.md) Dağıtım modunu kullanabilirsiniz.
>

## <a name="deployments-resource"></a>Dağıtımlar kaynağı

Başka bir şablona bağlantı sağlamak için ana şablonunuza bir **dağıtımlar** kaynağı ekleyin.

```json
"resources": [
  {
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2018-05-01",
    "name": "linkedTemplate",
    "properties": {
        "mode": "Incremental",
        <nested-template-or-external-template>
    }
  }
]
```

Dağıtım kaynağı için sağladığınız özellikler, bir dış şablona bağlansanız veya ana şablonda bir satır içi şablon iç içe geçme seçeneklerine göre farklılık gösterir.

## <a name="nested-template"></a>İç içe şablon

Şablonu ana şablon içine yerleştirmek için, **şablon** özelliğini kullanın ve Şablon sözdizimini belirtin.

```json
"resources": [
  {
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2018-05-01",
    "name": "nestedTemplate",
    "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[variables('storageName')]",
            "location": "West US",
            "kind": "StorageV2",
            "sku": {
                "name": "Standard_LRS"
            }
          }
        ]
      }
    }
  }
]
```

> [!NOTE]
> İç içe geçmiş şablonlar için, iç içe yerleştirilmiş şablonda tanımlanmış parametreleri veya değişkenleri kullanamazsınız. Ana şablondaki parametreleri ve değişkenleri kullanabilirsiniz. Yukarıdaki örnekte, `[variables('storageName')]`, iç içe şablondan değil ana şablondan bir değer alır. Bu kısıtlama dış şablonlar için geçerlidir.
>
> İç içe yerleştirilmiş bir şablonda tanımlı iki kaynak ve bir kaynak diğerine bağlı olduğunda, bağımlılığın değeri yalnızca bağımlı kaynağın adıdır:
> ```json
> "dependsOn": [
>   "[variables('storageAccountName')]"
> ],
> ```
>
> İç içe yerleştirilmiş şablonda dağıttığınız bir kaynağın iç içe yerleştirilmiş bir şablonun çıktılar bölümünde `reference` işlevini kullanamazsınız. Dağıtılan bir kaynağın değerlerini iç içe yerleştirilmiş bir şablonda döndürmek için, iç içe geçmiş şablonunuzu bağlı bir şablona dönüştürün.

İç içe şablon, standart şablonla [aynı özellikleri](resource-group-authoring-templates.md) gerektirir.

## <a name="external-template"></a>Dış şablon

Bir dış şablona bağlantı sağlamak için **Templatelink** özelliğini kullanın. Yerel bir dosya veya yalnızca yerel ağınızda bulunan bir dosya belirtemezsiniz. Yalnızca **http** ya da **https**içeren bir URI değeri sağlayabilirsiniz. Kaynak Yöneticisi şablona erişebilmelidir.

Bir seçenek, bağlantılı şablonunuzu bir depolama hesabına yerleştirmeli ve bu öğe için URI 'yi kullanacaktır.

Dış şablonunuz için parametreleri bir dış dosyada veya satır içi olarak sağlayabilirsiniz.

### <a name="external-parameters"></a>Dış parametreler

Bir dış parametre dosyası sağlarken, **Parameterslink** özelliğini kullanın:

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

Şablon veya parametreler için `contentVersion` özelliği sağlamanız gerekmez. İçerik sürüm değeri sağlamazsanız, şablonun geçerli sürümü dağıtılır. İçerik sürümü için bir değer sağlarsanız, bu bağlantı bağlantılı şablondaki sürümle eşleşmelidir; Aksi takdirde, dağıtım hata vererek başarısız olur.

### <a name="inline-parameters"></a>Satır içi parametreler

Ya da parametresini satır içi olarak sağlayabilirsiniz. Hem satır içi parametreleri hem de bir parametre dosyası bağlantısını kullanamazsınız. @No__t-0 ve `parameters` belirtildiğinde dağıtım bir hata ile başarısız olur.

Ana şablondan bağlantılı şablona bir değer geçirmek için, **Parameters** özelliğini kullanın.

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

## <a name="using-copy"></a>Kopyayı kullanma

İç içe geçmiş şablonla bir kaynağın birden çok örneğini oluşturmak için, **Microsoft. resources/dağıtımlar** kaynağı düzeyinde kopyalama öğesini ekleyin.

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
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(variables('storageName'), copyIndex())]",
            "location": "West US",
            "kind": "StorageV2",
            "sku": {
              "name": "Standard_LRS"
            }
            // no, copy doesn't work here
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

Önceki örneklerde, şablon bağlantıları için sabit kodlanmış URL değerleri gösteriliyordu. Bu yaklaşım basit bir şablon için çalışabilir, ancak büyük bir modüler şablonlar kümesiyle çalışırken iyi çalışmaz. Bunun yerine, ana şablon için temel URL 'YI depolayan bir statik değişken oluşturabilir ve ardından bu temel URL 'den bağlantılı şablonlar için dinamik olarak URL 'Ler oluşturabilirsiniz. Bu yaklaşımın avantajı, yalnızca ana şablondaki statik değişkeni değiştirmeniz gerektiğinden şablonu kolayca taşıyabilir veya çatalla aktarabilirsiniz. Ana şablon, ayrıştırılmış şablonun tamamında doğru URI 'Leri geçirir.

Aşağıdaki örnek, bağlantılı şablonlar (**Sharedtemplateurl** ve **vmtemplate**) için iki URL oluşturmak üzere temel URL 'yi nasıl kullanacağınızı gösterir.

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Ayrıca, geçerli şablonun temel URL 'sini almak için [Deployment ()](resource-group-template-functions-deployment.md#deployment) kullanabilirsiniz ve bunu, aynı konumdaki DIĞER şablonların URL 'sini almak için kullanabilirsiniz. Bu yaklaşım, şablon konumunuz değişirse veya şablon dosyasında sabit kodlama URL 'Lerinin oluşmasını önlemek istediğinizde yararlıdır. TemplateLink özelliği yalnızca bir URL ile uzak şablona bağlanılırken döndürülür. Yerel bir şablon kullanıyorsanız, bu özellik kullanılamaz.

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="get-values-from-linked-template"></a>Bağlı şablondan değerleri alma

Bağlı bir şablondan çıkış değeri almak için, özellik değerini şu şekilde bir sözdizimi ile alın: `"[reference('deploymentName').outputs.propertyName.value]"`.

Bağlantılı şablondan çıkış özelliği alınırken, özellik adı bir tire içeremez.

Aşağıdaki örneklerde, bağlantılı bir şablona nasıl başvurulacağını ve bir çıkış değerinin nasıl alınacağını gösterilmektedir. Bağlantılı şablon basit bir ileti döndürür.

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

Ana şablon, bağlantılı şablonu dağıtır ve döndürülen değeri alır. Dağıtım kaynağına adına göre başvurduğuna ve bağlı şablon tarafından döndürülen özelliğin adını kullandığını unutmayın.

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

Diğer kaynak türleri gibi, bağlantılı şablon ve diğer kaynaklar arasında bağımlılıklar da ayarlayabilirsiniz. Diğer kaynaklar bağlantılı şablondan bir çıkış değeri gerektirdiğinde, bağlantılı şablonun uygulamadan önce dağıtıldığından emin olun. Ya da bağlantılı şablon diğer kaynakları temel aldığından, bağlantılı şablondan önce diğer kaynakların dağıtıldığından emin olun.

Aşağıdaki örnek, genel bir IP adresi dağıtan ve kaynak KIMLIĞINI döndüren bir şablonu gösterir:

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

Yük dengeleyici dağıtma sırasında önceki şablondaki genel IP adresini kullanmak için, şablona bağlayın ve dağıtım kaynağına bir bağımlılık ekleyin. Yük dengeleyicideki genel IP adresi, bağlantılı şablondan çıkış değerine ayarlanır.

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

## <a name="linked-and-nested-templates-in-deployment-history"></a>Dağıtım geçmişinde bağlantılı ve iç içe geçmiş Şablonlar

Kaynak Yöneticisi her bir şablonu dağıtım geçmişinde ayrı bir dağıtım olarak işler. Bu nedenle, dağıtım geçmişinde üç bağlantılı veya iç içe şablon içeren bir ana şablon şu şekilde görünür:

![Dağıtım geçmişi](./media/resource-group-linked-templates/deployment-history.png)

Bu ayrı girdileri, dağıtımdan sonra çıkış değerlerini almak için geçmişte kullanabilirsiniz. Aşağıdaki şablon bir genel IP adresi oluşturur ve IP adresini verir:

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

Aşağıdaki şablon önceki şablona bağlantı sağlar. Üç genel IP adresi oluşturur.

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
    deployment=$(az group deployment show -g examplegroup -n $name);
    ip=$(echo $deployment | jq .properties.outputs.returnedIPAddress.value);
    echo "deployment $name returned $ip";
done
```

## <a name="securing-an-external-template"></a>Dış şablonun güvenliğini sağlama

Bağlı şablon dışarıdan kullanılabilir olmalıdır, ancak genel kullanıma açık olması gerekmez. Şablonunuzu yalnızca depolama hesabı sahibi tarafından erişilebilen bir özel depolama hesabına ekleyebilirsiniz. Daha sonra, dağıtım sırasında erişimi etkinleştirmek için bir paylaşılan erişim imzası (SAS) belirteci oluşturursunuz. Bu SAS belirtecini bağlantılı şablon için URI 'ye eklersiniz. Belirteç güvenli bir dize olarak geçirilse de, SAS belirteci dahil olmak üzere bağlantılı şablonun URI 'SI dağıtım işlemlerinde günlüğe kaydedilir. Pozlamayı sınırlandırmak için, belirteç için bir süre sonu ayarlayın.

Parametre dosyası bir SAS belirteci aracılığıyla da sınırlı olabilir.

Şu anda [Azure Storage güvenlik duvarının](../storage/common/storage-network-security.md)arkasındaki bir depolama hesabındaki bir şablona bağlayamazsınız.

Aşağıdaki örnek, bir şablona bağlanırken bir SAS belirtecinin nasıl geçirileceğini gösterir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
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
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="example-templates"></a>Örnek Şablonlar

Aşağıdaki örneklerde, bağlantılı şablonların yaygın kullanımları gösterilmektedir.

|Ana şablon  |Bağlantılı şablon |Açıklama  |
|---------|---------| ---------|
|[Hello World](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworldparent.json) |[bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/helloworld.json) | Bağlantılı şablondan dize döndürür. |
|[Genel IP adresi ile Load Balancer](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) |[bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) |Bağlı şablondan ortak IP adresini döndürür ve yük dengeleyicide bu değeri ayarlar. |
|[Birden çok IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip-parent.json) | [bağlantılı şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/static-public-ip.json) |Bağlı şablonda birkaç genel IP adresi oluşturur.  |

## <a name="next-steps"></a>Sonraki adımlar

* Öğreticiye gitmek için bkz. [öğretici: bağlı Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-linked-templates.md).
* Kaynaklarınızın dağıtım sırasını tanımlama hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonlarda bağımlılıkları tanımlama](resource-group-define-dependencies.md).
* Tek bir kaynağı tanımlama ve birçok örneğini oluşturma hakkında bilgi edinmek için, bkz. [Azure Resource Manager birden fazla kaynak örneği oluşturma](resource-group-create-multiple.md).
* Bir depolama hesabında bir şablon ayarlama ve SAS belirteci oluşturma adımları için bkz. [Kaynak Yöneticisi şablonları ile kaynak dağıtma ve Azure PowerShell](resource-group-template-deploy.md) [Kaynak Yöneticisi ŞABLONLARı ve Azure CLI ile kaynak dağıtma](resource-group-template-deploy-cli.md).
