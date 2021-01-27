---
title: Şablonlarda değişkenler
description: Azure Resource Manager şablonunda değişkenlerin nasıl tanımlanacağını açıklar (ARM şablonu).
ms.topic: conceptual
ms.date: 01/26/2021
ms.openlocfilehash: feecc4b5df77e6a3bf51294cb12aabf44899dde5
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98874443"
---
# <a name="variables-in-arm-template"></a>ARM şablonundaki değişkenler

Bu makalede Azure Resource Manager şablonunuzda (ARM şablonu) değişkenlerin nasıl tanımlanacağı ve kullanılacağı açıklanmaktadır. Şablonunuzu basitleştirmek için değişkenler kullanırsınız. Şablonunuz genelinde karmaşık ifadeler yinelemek yerine, karmaşık ifadeyi içeren bir değişken tanımlarsınız. Daha sonra, bu değişkene şablonunuz için gereken şekilde başvurmanız gerekir.

Kaynak Yöneticisi, dağıtım işlemlerini başlatmadan önce değişkenleri çözer. Değişken şablonun herhangi bir yerinde kullanıldığında Resource Manager bu değişkenin yerine çözümlenen değeri koyar.

## <a name="define-variable"></a>Değişken tanımla

Bir değişken tanımlarken, bir [veri türüne](template-syntax.md#data-types)çözümlenen bir değer veya şablon ifadesi sağlayın. Değişkeni oluştururken bir parametre veya başka bir değişken değerini kullanabilirsiniz.

Değişken bildiriminde [şablon işlevlerini](template-functions.md) kullanabilirsiniz, ancak [başvuru](template-functions-resource.md#reference) işlevini veya herhangi bir [liste](template-functions-resource.md#list) işlevini kullanamazsınız. Bu işlevler, bir kaynağın çalışma zamanı durumunu alır ve değişkenler çözümlendiğinde dağıtımdan önce yürütülemez.

Aşağıdaki örnekte bir değişken tanımı gösterilmektedir. Depolama hesabı adı için bir dize değeri oluşturur. Bir parametre değeri almak için çeşitli şablon işlevleri kullanır ve onu benzersiz bir dizeye birleştirir.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

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

## <a name="example-template"></a>Örnek şablon

Aşağıdaki şablon hiçbir kaynak dağıtmaz. Yalnızca değişkenleri bildirmenin bazı yollarını gösterir.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variables.json":::

## <a name="configuration-variables"></a>Yapılandırma değişkenleri

Bir ortamı yapılandırmak için ilgili değerleri tutan değişkenler tanımlayabilirsiniz. Değişkeni değerleriyle bir nesne olarak tanımlarsınız. Aşağıdaki örnek, iki ortam için değerleri tutan bir nesne gösterir- **Test** ve **Üretim**. Dağıtım sırasında bu değerlerden birini geçirin.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variablesconfigurations.json":::

## <a name="next-steps"></a>Sonraki adımlar

* Değişkenlerin kullanılabilir özellikleri hakkında bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Değişken oluşturma hakkında öneriler için bkz. [en iyi yöntemler-değişkenler](template-best-practices.md#variables).
* Ağ güvenlik grubuna güvenlik kuralları atayan örnek bir şablon için bkz. [ağ güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) ve [parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json).
