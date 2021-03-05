---
title: Şablonlardaki parametreler
description: Azure Resource Manager şablonunda (ARM şablonu) ve Bıcep dosyasında parametrelerin nasıl tanımlanacağını açıklar.
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: a8608e4733960d7f5ba7e5f548c47f16a2b244bb
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102123403"
---
# <a name="parameters-in-arm-templates"></a>ARM şablonlarındaki parametreler

Bu makalede, Azure Resource Manager şablonunuzda (ARM şablonunda) ve Bıcep dosyasında parametrelerin nasıl tanımlanacağı ve kullanılacağı açıklanmaktadır. Parametreler için farklı değerler sunarak, farklı ortamlar için bir şablonu yeniden kullanabilirsiniz.

Kaynak Yöneticisi, dağıtım işlemlerini başlatmadan önce parametre değerlerini çözer. Parametrenin şablonda kullanıldığı her yerde, Kaynak Yöneticisi çözümlenme değeriyle değiştirilir.

Her parametrenin [veri türlerinden](data-types.md)birine ayarlanması gerekir.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="minimal-declaration"></a>En az bildirim

En azından, her parametre bir ad ve tür gerektirir. Bıcep 'de, bir parametre değişken, kaynak, çıkış veya aynı kapsamdaki diğer bir parametreyle aynı ada sahip olamaz.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "demoString": {
    "type": "string"
  },
  "demoInt": {
    "type": "int"
  },
  "demoBool": {
    "type": "bool"
  },
  "demoObject": {
    "type": "object"
  },
  "demoArray": {
    "type": "array"
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param demoString string
param demoInt int
param demoBool bool
param demoObject object
param demoArray array
```

---

## <a name="secure-parameters"></a>Güvenli parametreler

Dize veya nesne parametrelerini güvenli olarak işaretleyebilirsiniz. Güvenli bir parametrenin değeri dağıtım geçmişine kaydedilmez ve günlüğe kaydedilmez.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "demoPassword": {
    "type": "secureString"
  },
  "demoSecretObject": {
    "type": "secureObject"
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@secure()
param demoPassword string

@secure()
param demoSecretObject object
```

---

## <a name="allowed-values"></a>İzin verilen değerler

Bir parametre için izin verilen değerleri tanımlayabilirsiniz. Bir dizide izin verilen değerleri sağlarsınız. İzin verilen değerlerden biri olmayan bir parametre için bir değer geçirilirse, doğrulama sırasında dağıtım başarısız olur.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "demoEnum": {
    "type": "string",
    "allowedValues": [
      "one",
      "two"
    ]
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@allowed([
  'one'
  'two'
])
param demoEnum string
```

---

## <a name="default-value"></a>Varsayılan değer

Bir parametre için varsayılan bir değer belirtebilirsiniz. Dağıtım sırasında bir değer sağlanmamışsa varsayılan değer kullanılır.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "demoParam": {
    "type": "string",
    "defaultValue": "Contoso"
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param demoParam string = 'Contoso'
```

---

Parametresi için diğer özelliklerle birlikte varsayılan bir değer belirtmek için aşağıdaki sözdizimini kullanın.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "demoParam": {
    "type": "string",
    "defaultValue": "Contoso",
    "allowedValues": [
      "Contoso",
      "Fabrikam"
    ]
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@allowed([
  'Contoso'
  'Fabrikam'
])
param demoParam string = 'Contoso'
```

---

Varsayılan değer ile ifadeleri kullanabilirsiniz. Parameters bölümünde [başvuru](template-functions-resource.md#reference) işlevini veya [liste](template-functions-resource.md#list) işlevlerinden herhangi birini kullanamazsınız. Bu işlevler, bir kaynağın çalışma zamanı durumunu alır ve parametreler çözümlendiğinde dağıtımdan önce yürütülemez.

Diğer parametre özellikleriyle ifadelere izin verilmez.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param location string = resourceGroup().location
```

---

Varsayılan bir değer oluşturmak için başka bir parametre değeri de kullanabilirsiniz. Aşağıdaki şablon, site adından bir konak planı adı oluşturur.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]"
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param siteName string = 'site${uniqueString(resourceGroup().id)}'
param hostingPlanName string = '${siteName}-plan'
```

---

## <a name="length-constraints"></a>Uzunluk kısıtlamaları

Dize ve dizi parametreleri için minimum ve maksimum uzunlukları belirtebilirsiniz. Bir veya her iki kısıtlamayı da ayarlayabilirsiniz. Dizeler için uzunluk, karakter sayısını belirtir. Diziler için, Uzunluk dizideki öğelerin sayısını belirtir.

Aşağıdaki örnek iki parametre bildirir. Tek parametre 3-24 karakter uzunluğunda olması gereken depolama hesabı adına yöneliktir. Diğer parametre 1-5 öğeden olması gereken bir dizidir.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "storageAccountName": {
    "type": "string",
    "minLength": 3,
    "maxLength": 24
  },
  "appNames": {
    "type": "array",
    "minLength": 1,
    "maxLength": 5
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@minLength(3)
@maxLength(24)
param storageAccountName string

@minLength(1)
@maxLength(5)
param appNames array
```

---

## <a name="integer-constraints"></a>Tamsayı kısıtlamaları

Tamsayı parametreleri için en düşük ve en yüksek değerleri ayarlayabilirsiniz. Bir veya her iki kısıtlamayı da ayarlayabilirsiniz.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "month": {
    "type": "int",
    "minValue": 1,
    "maxValue": 12
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@minValue(1)
@maxValue(12)
param month int
```

---

## <a name="description"></a>Açıklama

Şablonunuzun kullanıcılarının sağlanacak değeri anlamasına yardımcı olması için parametreye bir açıklama ekleyebilirsiniz. Şablonu Portal üzerinden dağıttığınızda, açıklamada sağladığınız metin, bu parametre için bir ipucu olarak otomatik olarak kullanılır. Yalnızca metin, parametre adından çıkarsandan daha fazla bilgi sağlıyorsa açıklama ekleyin.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "virtualMachineSize": {
    "type": "string",
    "metadata": {
      "description": "Must be at least Standard_A3 to support 2 NICs."
    },
    "defaultValue": "Standard_DS1_v2"
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
@description('Must be at least Standard_A3 to support 2 NICs.')
param virtualMachineSize string = 'Standard_DS1_v2'
```

---

## <a name="use-parameter"></a>Parametre kullan

JSON şablonunda [Parametreler](template-functions-deployment.md#parameters) işlevini kullanarak parametrenin değerine başvurarak. Bıcep 'de parametre adını kullanırsınız. Aşağıdaki örnek, Key Vault adı için bir parametre değeri kullanır.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vaultName": {
      "type": "string",
      "defaultValue": "[format('keyVault{0}', uniqueString(resourceGroup().id))]"
    }
  },
  "resources": [
    {
      "type": "Microsoft.KeyVault/vaults",
      "apiVersion": "2019-09-01",
      "name": "[parameters('vaultName')]",
      ...
    }
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param vaultName string = 'keyVault${uniqueString(resourceGroup().id)}'

resource keyvault 'Microsoft.KeyVault/vaults@2019-09-01' = {
  name: vaultName
  ...
}
```

---

## <a name="objects-as-parameters"></a>Parametre olarak nesneler

İlgili değerleri bir nesne olarak geçirerek düzenlemek daha kolay olabilir. Bu yaklaşım ayrıca şablondaki parametre sayısını azaltır.

Aşağıdaki örnek bir nesnesi olan bir parametresini gösterir. Varsayılan değer, nesne için beklenen özellikleri gösterir. Bu özellikler, dağıtılacak kaynağı tanımlarken kullanılır.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vNetSettings": {
      "type": "object",
      "defaultValue": {
        "name": "VNet1",
        "location": "eastus",
        "addressPrefixes": [
          {
            "name": "firstPrefix",
            "addressPrefix": "10.0.0.0/22"
          }
        ],
        "subnets": [
          {
            "name": "firstSubnet",
            "addressPrefix": "10.0.0.0/24"
          },
          {
            "name": "secondSubnet",
            "addressPrefix": "10.0.1.0/24"
          }
        ]
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Network/virtualNetworks",
      "apiVersion": "2020-06-01",
      "name": "[parameters('vNetSettings').name]",
      "location": "[parameters('vNetSettings').location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('vNetSettings').addressPrefixes[0].addressPrefix]"
          ]
        },
        "subnets": [
          {
            "name": "[parameters('vNetSettings').subnets[0].name]",
            "properties": {
              "addressPrefix": "[parameters('vNetSettings').subnets[0].addressPrefix]"
            }
          },
          {
            "name": "[parameters('vNetSettings').subnets[1].name]",
            "properties": {
              "addressPrefix": "[parameters('vNetSettings').subnets[1].addressPrefix]"
            }
          }
        ]
      }
    }
  ]
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param vNetSettings object = {
  name: 'VNet1'
  location: 'eastus'
  addressPrefixes: [
    {
      name: 'firstPrefix'
      addressPrefix: '10.0.0.0/22'
    }
  ]
  subnets: [
    {
      name: 'firstSubnet'
      addressPrefix: '10.0.0.0/24'
    }
    {
      name: 'secondSubnet'
      addressPrefix: '10.0.1.0/24'
    }
  ]
}
resource vnet 'Microsoft.Network/virtualNetworks@2020-06-01' = {
  name: vNetSettings.name
  location: vNetSettings.location
  properties: {
    addressSpace: {
      addressPrefixes: [
        vNetSettings.addressPrefixes[0].addressPrefix
      ]
    }
    subnets: [
      {
        name: vNetSettings.subnets[0].name
        properties: {
          addressPrefix: vNetSettings.subnets[0].addressPrefix
        }
      }
      {
        name: vNetSettings.subnets[1].name
        properties: {
          addressPrefix: vNetSettings.subnets[1].addressPrefix
        }
      }
    ]
  }
}
```

---

## <a name="example-templates"></a>Örnek Şablonlar

Aşağıdaki örneklerde, parametreleri kullanmaya yönelik senaryolar gösterilmektedir.

|Şablon  |Açıklama  |
|---------|---------|
|[Varsayılan değerler için işlevlere sahip parametreler](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Parametreler için varsayılan değerleri tanımlarken şablon işlevlerinin nasıl kullanılacağını gösterir. Şablon hiçbir kaynak dağıtmaz. Parametre değerlerini oluşturur ve bu değerleri döndürür. |
|[Parameter nesnesi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Bir parametre için bir nesne kullanmayı gösterir. Şablon hiçbir kaynak dağıtmaz. Parametre değerlerini oluşturur ve bu değerleri döndürür. |

## <a name="next-steps"></a>Sonraki adımlar

* Parametrelerin kullanılabilir özellikleri hakkında bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Parametre değerlerini bir dosya olarak geçirme hakkında bilgi edinmek için bkz. [Kaynak Yöneticisi parametre dosyası oluşturma](parameter-files.md).
* Parametreleri oluşturma hakkında öneriler için bkz. [en iyi yöntemler-parametreler](template-best-practices.md#parameters).
