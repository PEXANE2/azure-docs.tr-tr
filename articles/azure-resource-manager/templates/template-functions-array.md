---
title: Şablon işlevleri - diziler ve nesneler
description: Diziler ve nesnelerle çalışmak için Azure Kaynak Yöneticisi şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 07/31/2019
ms.openlocfilehash: 0b4bb80f6d7a7cc20a8b2dcc71e890f2ada7c5be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156384"
---
# <a name="array-and-object-functions-for-arm-templates"></a>ARM şablonları için dizi ve nesne işlevleri

Kaynak Yöneticisi, Azure Kaynak Yöneticisi (ARM) şablonunuzda diziler ve nesnelerle çalışmak için çeşitli işlevler sağlar.

* [Dizi](#array)
* [Coalesce](#coalesce)
* [Concat](#concat)
* [Içerir](#contains)
* [createArray](#createarray)
* [empty](#empty)
* [Ilk](#first)
* [Kesişim](#intersection)
* [Json](#json)
* [Son](#last)
* [Uzun -luğu](#length)
* [Max](#max)
* [Dk](#min)
* [Aralığı](#range)
* [Atlamak](#skip)
* [almak](#take)
* [Birliği](#union)

Bir değerle sınırlandırılmış dize değerleri dizisi almak için [bkz.](template-functions-string.md#split)

## <a name="array"></a>array

`array(convertToArray)`

Değeri bir diziye dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| convertToArray |Evet |int, string, dizi veya nesne |Bir diziye dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/array.json) dizi işlevinin farklı türlerde nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "intToConvert": {
            "type": "int",
            "defaultValue": 1
        },
        "stringToConvert": {
            "type": "string",
            "defaultValue": "efgh"
        },
        "objectToConvert": {
            "type": "object",
            "defaultValue": {"a": "b", "c": "d"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "intOutput": {
            "type": "array",
            "value": "[array(parameters('intToConvert'))]"
        },
        "stringOutput": {
            "type": "array",
            "value": "[array(parameters('stringToConvert'))]"
        },
        "objectOutput": {
            "type": "array",
            "value": "[array(parameters('objectToConvert'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| intOutput | Dizi | [1] |
| stringOutput | Dizi | ["efgh"] |
| objectOutput | Dizi | [{"a": "b", "c": "d"}] |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/array.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/array.json
```

## <a name="coalesce"></a>Coalesce

`coalesce(arg1, arg2, arg3, ...)`

Parametrelerden ilk null olmayan değeri verir. Boş dizeleri, boş diziler ve boş nesneler null değildir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int, string, dizi veya nesne |Null için test etmek için ilk değer. |
| ek args |Hayır |int, string, dizi veya nesne |Null için test etmek için ek değerler. |

### <a name="return-value"></a>Döndürülen değer

Dize, int, dizi veya nesne olabilecek ilk null olmayan parametrelerin değeri. Tüm parametreler null ise null.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/coalesce.json) birleşip farklı kullanımlardan elde edilen çıktıyı gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {
                "null1": null,
                "null2": null,
                "string": "default",
                "int": 1,
                "object": {"first": "default"},
                "array": [1]
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').string)]"
        },
        "intOutput": {
            "type": "int",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').int)]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').object)]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').array)]"
        },
        "emptyOutput": {
            "type": "bool",
            "value": "[empty(coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| stringOutput | Dize | default |
| intOutput | int | 1 |
| objectOutput | Nesne | {"first": "varsayılan"} |
| diziÇıktı | Dizi | [1] |
| emptyOutput | Bool | True |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/coalesce.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/coalesce.json
```

## <a name="concat"></a>concat

`concat(arg1, arg2, arg3, ...)`

Birden çok diziyi birleştirir ve sıkıştırılmış diziyi döndürür veya birden çok dize değerlerini birleştirir ve sıkıştırılmış dizeyi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |Concatenation için ilk dizi veya dize. |
| ek bağımsız değişkenler |Hayır |dizi veya dize |Birleştirme için sıralı sırayla ek diziler veya dizeleri. |

Bu işlev herhangi bir sayıda bağımsız değişken alabilir ve parametreler için dizeleri veya dizileri kabul edebilir. Ancak, parametreler için hem dizileri hem de dizeleri sağlayamazsınız. Diziler yalnızca diğer dizilerle kaplanır.

### <a name="return-value"></a>Döndürülen değer

Bir dize veya birdizi concatenated değerleri.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-array.json) iki dizinin nasıl birleştirilebildiğini gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstArray": {
            "type": "array",
            "defaultValue": [
                "1-1",
                "1-2",
                "1-3"
            ]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": [
                "2-1",
                "2-2",
                "2-3"
            ]
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| return | Dizi | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-array.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-array.json
```

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-string.json) iki dize değerini nasıl birleştirip, sıkıştırılmış bir dize yi nasıl döndüreceklerini gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| concatOutput | Dize | önek-5yj4yjf5mbg72 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-string.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-string.json
```

## <a name="contains"></a>içerir

`contains(container, itemToFind)`

Bir dizi bir değer, bir nesne bir anahtar veya bir dize bir alt dize içerir olup olmadığını denetler. Dize karşılaştırması büyük/küçük harf duyarlıdır. Ancak, bir nesne bir anahtar içeriyorsa sınanırken, karşılaştırma büyük/küçük harf duyarsızdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| kapsayıcı |Evet |dizi, nesne veya dize |Bulunacak değeri içeren değer. |
| itemToFind |Evet |dize veya int |Bulunması gereken değer. |

### <a name="return-value"></a>Döndürülen değer

Öğe bulunursa **doğru;** aksi takdirde, **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/contains.json) farklı türlerde içeren lerin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| stringTrue | Bool | True |
| stringFalse | Bool | False |
| objectTrue | Bool | True |
| objectFalse | Bool | False |
| diziGerçek | Bool | True |
| diziFalse | Bool | False |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/contains.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/contains.json
```

## <a name="createarray"></a>oluşturma dizisi

`createArray (arg1, arg2, arg3, ...)`

Parametrelerden bir dizi oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |String, Integer, Dizi veya Nesne |Dizideki ilk değer. |
| ek bağımsız değişkenler |Hayır |String, Integer, Dizi veya Nesne |Dizideki ek değerler. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/createarray.json) createArray'in farklı türleri ile nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringArray": {
            "type": "array",
            "value": "[createArray('a', 'b', 'c')]"
        },
        "intArray": {
            "type": "array",
            "value": "[createArray(1, 2, 3)]"
        },
        "objectArray": {
            "type": "array",
            "value": "[createArray(parameters('objectToTest'))]"
        },
        "arrayArray": {
            "type": "array",
            "value": "[createArray(parameters('arrayToTest'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| stringArray | Dizi | ["a", "b", "c"] |
| intArray | Dizi | [1, 2, 3] |
| objectArray | Dizi | [{"bir": "a", "iki": "b", "üç": "c"}] |
| arrayArray | Dizi | [["bir", "iki", "üç"]] |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/createarray.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/createarray.json
```

## <a name="empty"></a>empty

`empty(itemToTest)`

Bir dizinin, nesnenin veya dizenin boş olup olmadığını belirler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| itemToTest |Evet |dizi, nesne veya dize |Boş olup olmadığını kontrol etmek için değer. |

### <a name="return-value"></a>Döndürülen değer

Değer boşsa **True** döndürür; aksi takdirde, **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/empty.json) bir dizi, nesne ve dize boş olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| diziBoş | Bool | True |
| objectEmpty | Bool | True |
| stringBoş | Bool | True |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/empty.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/empty.json
```

## <a name="first"></a>Ilk

`first(arg1)`

Dizinin ilk öğesini veya dizedeki ilk karakteri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |İlk öğeyi veya karakteri almak için değer. |

### <a name="return-value"></a>Döndürülen değer

Bir dizideki ilk öğenin türü (dize, int, dizi veya nesne) veya bir dizedeki ilk karakter.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/first.json) bir dizi ve dize ile ilk işlevin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| diziÇıktı | Dize | bir |
| stringOutput | Dize | O |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/first.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/first.json
```

## <a name="intersection"></a>Kesişim

`intersection(arg1, arg2, arg3, ...)`

Parametrelerden ortak öğelerle tek bir dizi veya nesne döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ilk değer. |
| arg2 |Evet |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ikinci değer. |
| ek bağımsız değişkenler |Hayır |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ek değerler. |

### <a name="return-value"></a>Döndürülen değer

Ortak öğelere sahip bir dizi veya nesne.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/intersection.json) diziler ve nesnelerle kesişimin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "z", "three": "c"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[intersection(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[intersection(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Nesne | {"bir": "a", "üç": "c"} |
| diziÇıktı | Dizi | ["iki", "üç"] |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/intersection.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/intersection.json
```

## <a name="json"></a>json

`json(arg1)`

Bir JSON nesnesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |string |JSON'a dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Belirtilen dizeden JSON nesnesi veya **null** belirtildiğinde boş bir nesne.

### <a name="remarks"></a>Açıklamalar

JSON nesnesine bir parametre değeri veya değişken eklemeniz gerekiyorsa, işleve geçtiğiniz dizeyi oluşturmak için [concat](template-functions-string.md#concat) işlevini kullanın.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/json.json) json işlevinin diziler ve nesnelerle nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testValue": {
            "type": "string",
            "defaultValue": "demo value"
        }
    },
    "resources": [
    ],
    "outputs": {
        "jsonOutput": {
            "type": "object",
            "value": "[json('{\"a\": \"b\"}')]"
        },
        "nullOutput": {
            "type": "bool",
            "value": "[empty(json('null'))]"
        },
        "paramOutput": {
            "type": "object",
            "value": "[json(concat('{\"a\": \"', parameters('testValue'), '\"}'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| jsonÇıktı | Nesne | {"a": "b"} |
| nullOutput | Boole | True |
| paramOutput | Nesne | {"a": "demo değeri"}

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/json.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/json.json
```

## <a name="last"></a>Son

`last (arg1)`

Dizinin son öğesini veya dizedeki son karakteri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |Son öğeyi veya karakteri almak için değer. |

### <a name="return-value"></a>Döndürülen değer

Bir dizideki son öğenin türü (dize, int, dizi veya nesne) veya bir dizedeki son karakter.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/last.json) bir dizi ve dize ile son işlevin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| diziÇıktı | Dize | Üç |
| stringOutput | Dize | e |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/last.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/last.json
```

## <a name="length"></a>length

`length(arg1)`

Dizideki öğe sayısını, dizdeki karakterleri veya bir nesnedeki kök düzeyi özelliklerini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi, dize veya nesne |Öğe sayısını almak için kullanılacak dizi, karakter sayısını almak için kullanılacak dize veya kök düzeyi özelliklerinin sayısını almak için kullanılacak nesne. |

### <a name="return-value"></a>Döndürülen değer

Bir int.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/length.json) bir dizi ve dize ile uzunluğun nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {
                "propA": "one",
                "propB": "two",
                "propC": "three",
                "propD": {
                    "propD-1": "sub",
                    "propD-2": "sub"
                }
            }
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        },
        "objectLength": {
            "type": "int",
            "value": "[length(parameters('objectToTest'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| diziUzunluk | int | 3 |
| stringUzunluk | int | 13 |
| objectLength | int | 4 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/length.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/length.json
```

Kaynak oluştururken yineleme sayısını belirtmek için bu işlevi bir dizi ile kullanabilirsiniz. Aşağıdaki örnekte, parametre **site Adları,** web sitelerini oluştururken kullanılacak bir dizi adatıfta bulunur.

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

Bu işlevi bir diziyle kullanma hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi'nde birden çok kaynak örneği oluştur'a](copy-resources.md)bakın.

## <a name="max"></a>max

`max(arg1)`

Bir dizi tamsayı veya virgülle ayrılmış tamsayılar listesinden en büyük değeri verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |isterler dizisi veya virgülle ayrılmış bir insa listesi |En yüksek değeri elde etmek için koleksiyon. |

### <a name="return-value"></a>Döndürülen değer

Maksimum değeri temsil eden bir int.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json) bir dizi ve tamsayılar listesi ile max nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| diziÇıktı | int | 5 |
| intOutput | int | 5 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

## <a name="min"></a>min

`min(arg1)`

Bir tamsede dizisinden veya virgülle ayrılmış bir tamsayılar listesinden en az değeri verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |isterler dizisi veya virgülle ayrılmış bir insa listesi |En düşük değeri almak için toplama. |

### <a name="return-value"></a>Döndürülen değer

Minimum değeri temsil eden bir int.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json) min'in bir dizi ve tamsayılar listesiyle nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| diziÇıktı | int | 0 |
| intOutput | int | 0 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

## <a name="range"></a>aralık

`range(startIndex, count)`

Başlangıç tamsayının bir dizi tamsayı oluşturur ve bir dizi öğe yi içerer.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| Startındex |Evet |int |Dizideki ilk sonda. StartIndex ve count toplamı 2147483647'den büyük olmamalıdır. |
| count |Evet |int |Dizideki tümersayısı. 10000'e kadar negatif olmayan bir sayı olmalıdır. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi sonal.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/range.json) aralık işlevinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "startingInt": {
            "type": "int",
            "defaultValue": 5
        },
        "numberOfElements": {
            "type": "int",
            "defaultValue": 3
        }
    },
    "resources": [],
    "outputs": {
        "rangeOutput": {
            "type": "array",
            "value": "[range(parameters('startingInt'),parameters('numberOfElements'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| aralıkÇıktı | Dizi | [5, 6, 7] |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/range.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/range.json
```

## <a name="skip"></a>Atla

`skip(originalValue, numberToSkip)`

Dizide belirtilen sayıdan sonra tüm öğeleri içeren bir dizi döndürür veya dizedeki belirtilen sayıdan sonra tüm karakterleri içeren bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| Originalvalue |Evet |dizi veya dize |Atlama için kullanılacak dizi veya dize. |
| numaraToSkip |Evet |int |Atlasılayabilmek için öğelerin veya karakterlerin sayısı. Bu değer 0 veya daha az ise, değerdeki tüm öğeler veya karakterler döndürülür. Dizi veya dize uzunluğundan daha büyükse, boş bir dizi veya dize döndürülür. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi veya dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/skip.json) dizideki belirtilen öğe sayısını ve bir dizedeki belirtilen karakter sayısını atlar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| diziÇıktı | Dizi | ["üç"] |
| stringOutput | Dize | iki üç |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/skip.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/skip.json
```

## <a name="take"></a>almak

`take(originalValue, numberToTake)`

Dizinin başlangıcından belirtilen sayıda öğeiçeren bir dizi veya dize başından belirtilen karakter sayısına sahip bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| Originalvalue |Evet |dizi veya dize |Öğeleri almak için dizi veya dize. |
| numberToTake |Evet |int |Ele alınacak öğe veya karakter sayısı. Bu değer 0 veya daha az ise, boş bir dizi veya dize döndürülür. Verilen dizinin veya dizenin uzunluğundan daha büyükse, dizi veya dizedeki tüm öğeler döndürülür. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi veya dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/take.json) diziden belirtilen öğe sayısını ve bir dizeden karakterleri alır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| diziÇıktı | Dizi | ["bir", "iki"] |
| stringOutput | Dize | on |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/take.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/take.json
```

## <a name="union"></a>birleşim

`union(arg1, arg2, arg3, ...)`

Parametrelerden tüm öğeleri içeren tek bir dizi veya nesne döndürür. Yinelenen değerler veya anahtarlar yalnızca bir kez dahil edilir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya nesne |Öğeleri birleştirmek için kullanılacak ilk değer. |
| arg2 |Evet |dizi veya nesne |Öğeleri birleştirmek için kullanılacak ikinci değer. |
| ek bağımsız değişkenler |Hayır |dizi veya nesne |Öğeleri birleştirmek için kullanılacak ek değerler. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi veya nesne.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/union.json) diziler ve nesnelerle birleşim nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c1"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"three": "c2", "four": "d", "five": "e"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["three", "four"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[union(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[union(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Nesne | {"bir": "a", "iki": "b", "üç": "c2", "dört": "d", "beş": "e"} |
| diziÇıktı | Dizi | ["bir", "iki", "üç", "dört"] |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/union.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/union.json
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Kaynak Yöneticisi şablonundaki bölümlerin açıklaması için [bkz.](template-syntax.md)
* Birden çok şablonu birleştirmek için bkz: [Azure Kaynak Yöneticisi ile bağlantılı şablonları kullanma.](linked-templates.md)
* Bir kaynak türü oluştururken belirli sayıda kez yeniden sıralamak için azure [kaynak yöneticisinde birden çok kaynak örneği oluşturma](copy-resources.md)bölümüne bakın.
* Oluşturduğunuz şablonu nasıl dağıtabileceğinizi görmek için Azure [Kaynak Yöneticisi şablonuyla bir uygulama dağıt'a](deploy-powershell.md)bakın.

