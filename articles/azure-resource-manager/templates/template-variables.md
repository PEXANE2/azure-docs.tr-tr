---
title: Şablonlarda değişkenler
description: Azure Resource Manager şablonunda değişkenlerin nasıl tanımlanacağını açıklar (ARM şablonu).
ms.topic: conceptual
ms.date: 11/24/2020
ms.openlocfilehash: 5d9b58d63e96656c45d3494d24099bbeadc46b11
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96353468"
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

Değişkenler bölümünde [başvuru](template-functions-resource.md#reference) işlevini veya [liste](template-functions-resource.md#list) işlevlerinden herhangi birini kullanamazsınız. Bu işlevler, bir kaynağın çalışma zamanı durumunu alır ve değişkenler çözümlendiğinde dağıtımdan önce yürütülemez.

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

Parametreler ' de, hangi yapılandırma değerlerinin kullanılacağını belirten bir değer oluşturursunuz.

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

|Şablon  |Açıklama  |
|---------|---------|
| [değişken tanımları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Farklı değişken türlerini gösterir. Şablon hiçbir kaynak dağıtmaz. Değişken değerleri oluşturur ve bu değerleri döndürür. |
| [Yapılandırma değişkeni](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Yapılandırma değerlerini tanımlayan bir değişkenin kullanımını gösterir. Şablon hiçbir kaynak dağıtmaz. Değişken değerleri oluşturur ve bu değerleri döndürür. |
| [ağ güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) ve [parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Bir ağ güvenlik grubuna güvenlik kuralları atamak için doğru biçimde bir dizi oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

* Değişkenlerin kullanılabilir özellikleri hakkında bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Değişken oluşturma hakkında öneriler için bkz. [en iyi yöntemler-değişkenler](template-best-practices.md#variables).
