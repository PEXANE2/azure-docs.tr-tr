---
title: Şablonlarda değişkenler
description: Azure Resource Manager şablonunda (ARM şablonu) ve Bıcep dosyasında değişkenlerin nasıl tanımlanacağını açıklar.
ms.topic: conceptual
ms.date: 02/19/2021
ms.openlocfilehash: 3ab14c9acfcc2d6c9edd23fb3bc4d876cd5ac756
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102123386"
---
# <a name="variables-in-arm-templates"></a>ARM şablonlarındaki değişkenler

Bu makalede Azure Resource Manager şablonunuzda (ARM şablonunda) veya Bıcep dosyasında değişkenlerin nasıl tanımlanacağı ve kullanılacağı açıklanmaktadır. Şablonunuzu basitleştirmek için değişkenler kullanırsınız. Şablonunuz genelinde karmaşık ifadeler yinelemek yerine, karmaşık ifadeyi içeren bir değişken tanımlarsınız. Daha sonra bu değişkeni, şablonunuzun tamamında gereken şekilde kullanırsınız.

Kaynak Yöneticisi, dağıtım işlemlerini başlatmadan önce değişkenleri çözer. Değişken şablonun herhangi bir yerinde kullanıldığında Resource Manager bu değişkenin yerine çözümlenen değeri koyar.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="define-variable"></a>Değişken tanımla

Bir değişken tanımlarken, değişken için bir [veri türü](data-types.md) belirtmezsiniz. Bunun yerine bir değer veya şablon ifadesi sağlayın. Değişken türü çözümlenen değerden algılanır. Aşağıdaki örnek bir dize için bir değişken ayarlar.

# <a name="json"></a>[JSON](#tab/json)

```json
"variables": {
  "stringVar": "example value"
},
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
var stringVar = 'example value'
```

---

Değişkeni oluştururken bir parametre veya başka bir değişken değerini kullanabilirsiniz.

# <a name="json"></a>[JSON](#tab/json)

```json
"parameters": {
  "inputValue": {
    "defaultValue": "deployment parameter",
    "type": "string"
  }
},
"variables": {
  "stringVar": "myVariable",
  "concatToVar": "[concat(variables('stringVar'), '-addtovar') ]",
  "concatToParam": "[concat(parameters('inputValue'), '-addtoparam')]"
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param inputValue string = 'deployment parameter'

var stringVar = 'myVariable'
var concatToVar =  '${stringVar}-addtovar'
var concatToParam = '${inputValue}-addtoparam'
```

---

Değişken değerini oluşturmak için [şablon işlevlerini](template-functions.md) kullanabilirsiniz.

Aşağıdaki örnek, depolama hesabı adı için bir dize değeri oluşturur. Bir parametre değeri almak için çeşitli şablon işlevleri kullanır ve onu benzersiz bir dizeye birleştirir.

# <a name="json"></a>[JSON](#tab/json)

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
var storageName = '${toLower(storageNamePrefix)}${uniqueString(resourceGroup().id)}'
```

---

JSON şablonlarında, [başvuru](template-functions-resource.md#reference) işlevini veya değişken bildiriminde herhangi bir [liste](template-functions-resource.md#list) işlevini kullanamazsınız. Bu işlevler, bir kaynağın çalışma zamanı durumunu alır ve değişkenler çözümlendiğinde dağıtımdan önce yürütülemez.

Bıcep dosyalarında, bir değişken bildirirken başvuru ve liste işlevleri geçerlidir.

## <a name="use-variable"></a>Değişken kullan

Aşağıdaki örnek, bir kaynak özelliği için değişkeninin nasıl kullanılacağını gösterir.

# <a name="json"></a>[JSON](#tab/json)

JSON şablonunda, [değişkenler](template-functions-deployment.md#variables) işlevini kullanarak değişkenin değerine başvurarak.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageName')]",
    ...
  }
]
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

Bir bicep dosyasında, değişken adını sağlayarak değişkenin değerine başvurarak başvurabilirsiniz.

```bicep
var storageName = '${toLower(storageNamePrefix)}${uniqueString(resourceGroup().id)}'

resource demoAccount 'Microsoft.Storage/storageAccounts@2019-06-01' = {
  name: storageName
```

---

## <a name="example-template"></a>Örnek şablon

Aşağıdaki şablon hiçbir kaynak dağıtmaz. Farklı türlerde değişkenler bildirmenin bazı yollarını gösterir.

# <a name="json"></a>[JSON](#tab/json)

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variables.json":::

# <a name="bicep"></a>[Bicep](#tab/bicep)

Bıcep Şu anda döngüleri desteklemiyor.

:::code language="bicep" source="~/resourcemanager-templates/azure-resource-manager/variables.bicep":::

---

## <a name="configuration-variables"></a>Yapılandırma değişkenleri

Bir ortamı yapılandırmak için ilgili değerleri tutan değişkenler tanımlayabilirsiniz. Değişkeni değerleriyle bir nesne olarak tanımlarsınız. Aşağıdaki örnek, iki ortam için değerleri tutan bir nesne gösterir- **Test** ve **Üretim**. Dağıtım sırasında bu değerlerden birini geçirin.

# <a name="json"></a>[JSON](#tab/json)

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variablesconfigurations.json":::

# <a name="bicep"></a>[Bicep](#tab/bicep)

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/variablesconfigurations.bicep":::

---

## <a name="next-steps"></a>Sonraki adımlar

* Değişkenlerin kullanılabilir özellikleri hakkında bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Değişken oluşturma hakkında öneriler için bkz. [en iyi yöntemler-değişkenler](template-best-practices.md#variables).
* Ağ güvenlik grubuna güvenlik kuralları atayan örnek bir şablon için bkz. [ağ güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) ve [parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json).
