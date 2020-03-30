---
title: Şablonlarda değişkenler
description: Azure Kaynak Yöneticisi şablonundaki değişkenlerin nasıl tanımlandığını açıklar.
ms.topic: conceptual
ms.date: 09/05/2019
ms.openlocfilehash: cf135959d30702ea58b7a1d4fdd82625a39245d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75483823"
---
# <a name="variables-in-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonundaki değişkenler

Bu makalede, Azure Kaynak Yöneticisi şablonunuzda değişkenlerin nasıl tanımlanış ve kullanılacağı açıklanmaktadır. Şablonunuzu basitleştirmek için değişkenler kullanırsınız. Şablonunuzun her yerine karmaşık ifadeleri yinelemek yerine, karmaşık ifadeyi içeren bir değişken tanımlarsınız. Ardından, bu değişkene şablonunuzun her içinde gerektiği gibi başvurursunuz.

Kaynak Yöneticisi dağıtım işlemlerini başlatmadan önce değişkenleri çözer. Değişken şablonda nerede kullanılırsa kullanılsın, Kaynak Yöneticisi bu değişkeni çözümlenen değerle değiştirir.

## <a name="define-variable"></a>Değişkeni tanımla

Aşağıdaki örnekte değişken tanımı gösterilmektedir. Depolama hesabı adı için bir dize değeri oluşturur. Parametre değeri almak için çeşitli şablon işlevleri kullanır ve onu benzersiz bir dizeyle birleştirir.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

[Değişkenler bölümündeki başvuru](template-functions-resource.md#reference) işlevini veya [liste](template-functions-resource.md#list) işlevlerinden herhangi birini kullanamazsınız. Bu işlevler kaynağın çalışma zamanı durumunu alır ve değişkenler çözüldüğünde dağıtımdan önce yürütülemez.

## <a name="use-variable"></a>Değişkeni kullan

Şablonda, [değişkenler](template-functions-deployment.md#variables) işlevini kullanarak parametrenin değerine başvurursunuz. Aşağıdaki örnekte, bir kaynak özelliği için değişkenin nasıl kullanılacağı gösterilmektedir.

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

Bir ortamı yapılandırmak için ilgili değerleri tutan değişkenleri tanımlayabilirsiniz. Değişkeni değerlere sahip bir nesne olarak tanımlarsınız. Aşağıdaki örnekte, iki ortam için değer tutan bir nesne gösterilmektedir - **test** ve **prod**.

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

Parametrelerde, hangi yapılandırma değerlerinin kullanılacağını gösteren bir değer oluşturursunuz.

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

Belirtilen ortamın ayarlarını almak için değişken ve parametreyi birlikte kullanın.

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="example-templates"></a>Örnek şablonlar

Aşağıdaki örneklerde değişkenleri kullanma senaryoları gösterilmektedir.

|Şablon  |Açıklama  |
|---------|---------|
| [değişken tanımları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Farklı değişken türlerini gösterir. Şablon herhangi bir kaynak dağıtmaz. Değişken değerleri inşa eder ve bu değerleri döndürür. |
| [yapılandırma değişkeni](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Yapılandırma değerlerini tanımlayan bir değişkenin kullanımını gösterir. Şablon herhangi bir kaynak dağıtmaz. Değişken değerleri inşa eder ve bu değerleri döndürür. |
| [ağ güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) ve [parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Bir ağ güvenlik grubuna güvenlik kuralları atamak için doğru biçimde bir dizi oluşturuyor. |

## <a name="next-steps"></a>Sonraki adımlar

* Değişkenlerin kullanılabilir özellikleri hakkında bilgi edinmek için [bkz.](template-syntax.md)
* Değişken oluşturma yla ilgili öneriler için en [iyi uygulamalar - değişkenler](template-best-practices.md#variables)' e bakın.
