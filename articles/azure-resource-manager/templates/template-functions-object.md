---
title: Şablon işlevleri-nesneler
description: Nesneleri ile çalışmak için Azure Resource Manager şablonda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: fede4d6c71e45b119e500d4c9c6f91765d052036
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84676803"
---
# <a name="object-functions-for-arm-templates"></a>ARM şablonları için nesne işlevleri

Kaynak Yöneticisi, Azure Resource Manager (ARM) şablonunuzda nesnelerle çalışmak için çeşitli işlevler sağlar.

* [vardır](#contains)
* [empty](#empty)
* [imin](#intersection)
* [nesnesinde](#json)
* [uzunluklu](#length)
* [birleşim](#union)

## <a name="contains"></a>şunu içerir

`contains(container, itemToFind)`

Bir dizinin bir değer içerip içermediğini denetler, bir nesne anahtar içeriyor veya dize bir alt dize içeriyor. Dize karşılaştırma büyük/küçük harfe duyarlıdır. Ancak, bir nesne bir anahtar içeriyorsa test edilirken karşılaştırma büyük/küçük harfe duyarlıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| kapsayıcı |Evet |dizi, nesne veya dize |Bulunacak değeri içeren değer. |
| ıtemtofind |Evet |dize veya tamsayı |Bulunacak değer. |

### <a name="return-value"></a>Döndürülen değer

Öğe bulunursa **true** ; Aksi takdirde, **false**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/contains.json) , nasıl kullanılacağını farklı türlerde gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| stringTrue | Bool | True |
| stringFalse | Bool | False |
| objectTrue | Bool | True |
| Objectfali | Bool | False |
| arrayTrue | Bool | True |
| arrayFalse | Bool | False |

## <a name="empty"></a>empty

`empty(itemToTest)`

Bir dizi, nesne veya dize boş olup olmadığını belirler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| ıtemtotest |Evet |dizi, nesne veya dize |Boş olup olmadığını denetlemek için değer. |

### <a name="return-value"></a>Döndürülen değer

Değer boşsa, **true** döndürür; Aksi takdirde, **false**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/empty.json) bir dizi, nesne ve dizenin boş olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayEmpty | Bool | True |
| objectEmpty | Bool | True |
| stringEmpty | Bool | True |

## <a name="intersection"></a>imin

`intersection(arg1, arg2, arg3, ...)`

Parametrelerden ortak öğelerle tek bir dizi veya nesne döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ilk değer. |
| arg2 |Evet |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ikinci değer. |
| ek bağımsız değişkenler |Hayır |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ek değerler. |

### <a name="return-value"></a>Döndürülen değer

Ortak öğeleri olan bir dizi veya nesne.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/intersection.json) , dizilere ve nesneleriyle kesişimin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Nesne | {"One": "a", "üç": "c"} |
| arrayOutput | Dizi | ["iki", "üç"] |

## <a name="json"></a>json

`json(arg1)`

JSON nesnesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |string |JSON 'a dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Belirtilen dizeden JSON nesnesi veya **null** belirtildiğinde boş bir nesne.

### <a name="remarks"></a>Açıklamalar

JSON nesnesine bir parametre değeri veya değişken eklemeniz gerekiyorsa, işleve geçirdiğiniz dizeyi oluşturmak için [Concat](template-functions-string.md#concat) işlevini kullanın.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/json.json) , JSON işlevinin nasıl kullanılacağını göstermektedir. Nesneyi temsil eden bir dizeyi geçebildiğine veya hiçbir değer gerekmiyorsa **null** değeri kullanmanıza dikkat edin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "jsonObject1": {
            "type": "string",
            "defaultValue": "null"
        },
        "jsonObject2": {
            "type": "string",
            "defaultValue": "{\"a\": \"b\"}"
        },
        "testValue": {
            "type": "string",
            "defaultValue": "demo value"
        }
    },
    "resources": [
    ],
    "outputs": {
        "jsonOutput1": {
            "type": "bool",
            "value": "[empty(json(parameters('jsonObject1')))]"
        },
        "jsonOutput2": {
            "type": "object",
            "value": "[json(parameters('jsonObject2'))]"
        },
        "paramOutput": {
            "type": "object",
            "value": "[json(concat('{\"a\": \"', parameters('testValue'), '\"}'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| jsonOutput1 | Boole | True |
| jsonOutput2 | Nesne | {"a": "b"} |
| paramOutput | Nesne | {"a": "demo değeri"}

## <a name="length"></a>length

`length(arg1)`

Bir dizideki öğelerin sayısını, bir dizedeki karakterleri veya bir nesnedeki kök düzeyi özellikleri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi, dize veya nesne |Öğe sayısının alınması için kullanılacak dizi, karakter sayısını almak için kullanılacak dize veya kök düzeyi özelliklerinin sayısını almak için kullanılacak nesne. |

### <a name="return-value"></a>Döndürülen değer

Bir int.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/length.json) , uzunluğunun bir dizi ve dize ile nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayLength | int | 3 |
| stringLength | int | 13 |
| objectLength | int | 4 |

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

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/union.json) , Union dizilerini ve nesneleriyle nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Nesne | {"One": "a", "iki": "b", "üç": "C2", "dört": "d", "beş": "e"} |
| arrayOutput | Dizi | ["bir", "iki", "üç", "dört"] |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager şablonundaki bölümlerin açıklaması için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
