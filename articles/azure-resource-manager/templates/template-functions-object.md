---
title: Şablon işlevleri-nesneler
description: Nesneleri ile çalışmak için Azure Resource Manager şablonda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 10/12/2020
ms.openlocfilehash: 632e92bb798a5e8469079ef4693b7f321617f88c
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91977893"
---
# <a name="object-functions-for-arm-templates"></a>ARM şablonları için nesne işlevleri

Kaynak Yöneticisi, Azure Resource Manager (ARM) şablonunuzda nesnelerle çalışmak için çeşitli işlevler sağlar.

* [vardır](#contains)
* [createObject](#createobject)
* [empty](#empty)
* [imin](#intersection)
* [nesnesinde](#json)
* [length](#length)
* [null](#null)
* [birleşim](#union)

## <a name="contains"></a>contains

`contains(container, itemToFind)`

Bir dizinin bir değer içerip içermediğini denetler, bir nesne anahtar içeriyor veya dize bir alt dize içeriyor. Dize karşılaştırma büyük/küçük harfe duyarlıdır. Ancak, bir nesne bir anahtar içeriyorsa test edilirken karşılaştırma büyük/küçük harfe duyarlıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| kapsayıcı |Yes |dizi, nesne veya dize |Bulunacak değeri içeren değer. |
| ıtemtofind |Yes |dize veya tamsayı |Bulunacak değer. |

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

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringTrue | Bool | Doğru |
| stringFalse | Bool | Yanlış |
| objectTrue | Bool | Doğru |
| Objectfali | Bool | Yanlış |
| arrayTrue | Bool | Doğru |
| arrayFalse | Bool | Yanlış |

## <a name="createobject"></a>createObject

`createObject(key1, value1, key2, value2, ...)`

Anahtarlar ve değerlerden bir nesne oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| key1 |Hayır |dize |Anahtarın adı. |
| value1 |Hayır |int, Boolean, String, nesne veya dizi |Anahtarın değeri. |
| ek anahtarlar |Hayır |dize |Anahtarların ek adları. |
| ek değerler |Hayır |int, Boolean, String, nesne veya dizi |Anahtarlar için ek değerler. |

İşlev yalnızca çift sayıda parametreyi kabul eder. Her anahtar eşleşen bir değere sahip olmalıdır.

### <a name="return-value"></a>Döndürülen değer

Her anahtar ve değer çiftine sahip bir nesne.

### <a name="example"></a>Örnek

Aşağıdaki örnek, farklı değer türlerindeki bir nesne oluşturur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "newObject": {
            "type": "object",
            "value": "[createObject('intProp', 1, 'stringProp', 'abc', 'boolProp', true(), 'arrayProp', createArray('a', 'b', 'c'), 'objectProp', createObject('key1', 'value1'))]"
        }
    }
}
```

Önceki örneğin varsayılan değerleri ile çıkış, `newObject` aşağıdaki değere sahip adlı bir nesnedir:

```json
{
  "intProp": 1,
  "stringProp": "abc",
  "boolProp": true,
  "arrayProp": ["a", "b", "c"],
  "objectProp": {"key1": "value1"}
}
```

## <a name="empty"></a>empty

`empty(itemToTest)`

Bir dizi, nesne veya dize boş olup olmadığını belirler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| ıtemtotest |Yes |dizi, nesne veya dize |Boş olup olmadığını denetlemek için değer. |

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

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayEmpty | Bool | Doğru |
| objectEmpty | Bool | Doğru |
| stringEmpty | Bool | Doğru |

## <a name="intersection"></a>imin

`intersection(arg1, arg2, arg3, ...)`

Parametrelerden ortak öğelerle tek bir dizi veya nesne döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ilk değer. |
| arg2 |Yes |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ikinci değer. |
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

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Nesne | {"One": "a", "üç": "c"} |
| arrayOutput | Dizi | ["iki", "üç"] |

## <a name="json"></a>json

`json(arg1)`

Geçerli bir JSON dizesini JSON veri türüne dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dize |JSON 'a dönüştürülecek değer. Dize, düzgün şekilde biçimlendirilen bir JSON dizesi olmalıdır. |

### <a name="return-value"></a>Döndürülen değer

Belirtilen dizeden JSON veri türü veya **null** belirtildiğinde boş bir değer.

### <a name="remarks"></a>Açıklamalar

JSON nesnesine bir parametre değeri veya değişken eklemeniz gerekiyorsa, işleve geçirdiğiniz dizeyi oluşturmak için [Concat](template-functions-string.md#concat) işlevini kullanın.

Null bir değer almak için [null ()](#null) de kullanabilirsiniz.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/json.json) , JSON işlevinin nasıl kullanılacağını göstermektedir. Boş bir nesne için **null değer** geçirebildiğine dikkat edin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "jsonEmptyObject": {
            "type": "string",
            "defaultValue": "null"
        },
        "jsonObject": {
            "type": "string",
            "defaultValue": "{\"a\": \"b\"}"
        },
        "jsonString": {
            "type": "string",
            "defaultValue": "\"test\""
        },
        "jsonBoolean": {
            "type": "string",
            "defaultValue": "true"
        },
        "jsonInt": {
            "type": "string",
            "defaultValue": "3"
        },
        "jsonArray": {
            "type": "string",
            "defaultValue": "[[1,2,3 ]"
        },
        "concatValue": {
            "type": "string",
            "defaultValue": "demo value"
        }
    },
    "resources": [
    ],
    "outputs": {
        "emptyObjectOutput": {
            "type": "bool",
            "value": "[empty(json(parameters('jsonEmptyObject')))]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[json(parameters('jsonObject'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[json(parameters('jsonString'))]"
        },
        "booleanOutput": {
            "type": "bool",
            "value": "[json(parameters('jsonBoolean'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[json(parameters('jsonInt'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[json(parameters('jsonArray'))]"
        },
        "concatObjectOutput": {
            "type": "object",
            "value": "[json(concat('{\"a\": \"', parameters('concatValue'), '\"}'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| emptyObjectOutput | Boole | Doğru |
| objectOutput | Nesne | {"a": "b"} |
| stringOutput | Dize | test |
| booleanOutput | Boole | Doğru |
| ıntoutput | Tamsayı | 3 |
| arrayOutput | Dizi | [ 1, 2, 3 ] |
| Içtobjectoutput | Nesne | {"a": "demo değeri"} |

## <a name="length"></a>length

`length(arg1)`

Bir dizideki öğelerin sayısını, bir dizedeki karakterleri veya bir nesnedeki kök düzeyi özellikleri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dizi, dize veya nesne |Öğe sayısının alınması için kullanılacak dizi, karakter sayısını almak için kullanılacak dize veya kök düzeyi özelliklerinin sayısını almak için kullanılacak nesne. |

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

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayLength | int | 3 |
| stringLength | int | 13 |
| objectLength | int | 4 |

## <a name="null"></a>null

`null()`

Null döndürür.

### <a name="parameters"></a>Parametreler

Null işlevi herhangi bir parametreyi kabul etmez.

### <a name="return-value"></a>Döndürülen değer

Her zaman null olan bir değer.

### <a name="example"></a>Örnek

Aşağıdaki örnek, null işlevini kullanır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "emptyOutput": {
            "type": "bool",
            "value": "[empty(null())]"
        },
    }
}
```

Yukarıdaki örnekteki çıktı:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| Emptızput | Bool | Doğru |

## <a name="union"></a>birleşim

`union(arg1, arg2, arg3, ...)`

Parametrelerden tüm öğeleri içeren tek bir dizi veya nesne döndürür. Yinelenen değerler veya anahtarlar yalnızca bir kez dahil edilir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dizi veya nesne |Öğeleri birleştirmek için kullanılacak ilk değer. |
| arg2 |Yes |dizi veya nesne |Öğeleri birleştirmek için kullanılacak ikinci değer. |
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

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Nesne | {"One": "a", "iki": "b", "üç": "C2", "dört": "d", "beş": "e"} |
| arrayOutput | Dizi | ["bir", "iki", "üç", "dört"] |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager şablonundaki bölümlerin açıklaması için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
