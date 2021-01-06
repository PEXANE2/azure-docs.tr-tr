---
title: Şablonlarda değişkenler
description: Azure Resource Manager şablonunda değişkenlerin nasıl tanımlanacağını açıklar (ARM şablonu).
ms.topic: conceptual
ms.date: 11/24/2020
ms.openlocfilehash: 7f782f9c7d3107472a74fcab73290c4cebf73693
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97934671"
---
# <a name="variables-in-arm-template"></a>ARM şablonundaki değişkenler

Bu makalede Azure Resource Manager şablonunuzda (ARM şablonu) değişkenlerin nasıl tanımlanacağı ve kullanılacağı açıklanmaktadır. Şablonunuzu basitleştirmek için değişkenler kullanırsınız. Şablonunuz genelinde karmaşık ifadeler yinelemek yerine, karmaşık ifadeyi içeren bir değişken tanımlarsınız. Daha sonra, bu değişkene şablonunuz için gereken şekilde başvurmanız gerekir.

Kaynak Yöneticisi, dağıtım işlemlerini başlatmadan önce değişkenleri çözer. Değişken şablonun herhangi bir yerinde kullanıldığında Resource Manager bu değişkenin yerine çözümlenen değeri koyar.

Her değişkenin biçimi, [veri türlerinden](template-syntax.md#data-types)biri ile aynı olmalıdır.

## <a name="define-variable"></a>Değişken tanımla

Aşağıdaki örnekte bir değişken tanımı gösterilmektedir. Depolama hesabı adı için bir dize değeri oluşturur. Bir parametre değeri almak için çeşitli şablon işlevleri kullanır ve onu benzersiz bir dizeye birleştirir.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Bölümünde [başvuru](template-functions-resource.md#reference) işlevini veya herhangi bir [liste](template-functions-resource.md#list) işlevini kullanamazsınız `variables` . Bu işlevler, bir kaynağın çalışma zamanı durumunu alır ve değişkenler çözümlendiğinde dağıtımdan önce yürütülemez.

## <a name="use-variable"></a>Değişken kullan

Şablonda, [değişkenler](template-functions-deployment.md#variables) işlevini kullanarak parametrenin değerine başvurarak. Aşağıdaki örnek, bir kaynak özelliği için değişkeninin nasıl kullanılacağını gösterir.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageName')]",
    ...
  }
]
```

## <a name="configuration-variables"></a>Yapılandırma değişkenleri

Bir ortamı yapılandırmak için ilgili değerleri tutan değişkenler tanımlayabilirsiniz. Değişkeni değerleriyle bir nesne olarak tanımlarsınız. Aşağıdaki örnek, iki ortam için değerleri tutan bir nesne gösterir- **Test** ve **Üretim**.

```json
"variables": {
  "environmentSettings": {
    "test": {
      "instanceSize": "Small",
      "instanceCount": 1
    },
    "prod": {
      "instanceSize": "Large",
      "instanceCount": 4
    }
  }
},
```

İçinde `parameters` , hangi yapılandırma değerlerinin kullanılacağını belirten bir değer oluşturursunuz.

```json
"parameters": {
  "environmentName": {
    "type": "string",
    "allowedValues": [
      "test",
      "prod"
    ]
  }
},
```

Belirtilen ortamın ayarlarını almak için değişkeni ve parametresini birlikte kullanın.

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="example-templates"></a>Örnek Şablonlar

Aşağıdaki örneklerde, değişkenlerini kullanma senaryoları gösterilmektedir.

|Şablon  |Description  |
|---------|---------|
| [değişken tanımları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Farklı değişken türlerini gösterir. Şablon hiçbir kaynak dağıtmaz. Değişken değerleri oluşturur ve bu değerleri döndürür. |
| [Yapılandırma değişkeni](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Yapılandırma değerlerini tanımlayan bir değişkenin kullanımını gösterir. Şablon hiçbir kaynak dağıtmaz. Değişken değerleri oluşturur ve bu değerleri döndürür. |
| [ağ güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) ve [parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Bir ağ güvenlik grubuna güvenlik kuralları atamak için doğru biçimde bir dizi oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

* Değişkenlerin kullanılabilir özellikleri hakkında bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Değişken oluşturma hakkında öneriler için bkz. [en iyi yöntemler-değişkenler](template-best-practices.md#variables).
