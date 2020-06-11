---
title: Şablon işlevleri-diziler
description: Diziler ile çalışmak için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 4d4ee96888aee5421d88b5371ac25a69c0af4fd7
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84677857"
---
# <a name="array-functions-for-arm-templates"></a>ARM şablonları için dizi işlevleri

Kaynak Yöneticisi, Azure Resource Manager (ARM) şablonunuzda diziler ile çalışmak için çeşitli işlevler sağlar.

* [dizide](#array)
* [Concat](#concat)
* [vardır](#contains)
* [createArray](#createarray)
* [empty](#empty)
* [adı](#first)
* [imin](#intersection)
* [soyadına](#last)
* [uzunluklu](#length)
* [Biçimlendir](#max)
* [Min](#min)
* [aralığı](#range)
* [Şimdilik](#skip)
* [almanız](#take)
* [birleşim](#union)

Bir değere göre ayrılmış dize değerleri dizisini almak için bkz. [split](template-functions-string.md#split).

## <a name="array"></a>array

`array(convertToArray)`

Değeri bir diziye dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| convertToArray |Yes |int, String, array veya Object |Bir diziye dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/array.json) , farklı türlerde dizi işlevinin nasıl kullanılacağını göstermektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| ıntoutput | Dizi | 1 |
| stringOutput | Dizi | ["EFGH"] |
| objectOutput | Dizi | [{"a": "b", "c": "d"}] |

## <a name="concat"></a>concat

`concat(arg1, arg2, arg3, ...)`

Birden çok diziyi birleştirir ve birleştirilmiş diziyi döndürür ya da birden çok dize değerini birleştirir ve birleştirilmiş dizeyi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dizi veya dize |Birleştirme için ilk dizi veya dize. |
| ek bağımsız değişkenler |No |dizi veya dize |Birleştirme için sıralı sırada ek diziler veya dizeler. |

Bu işlev herhangi bir sayıda bağımsız değişken alabilir ve parametreler için dizeleri ya da dizileri kabul edebilir. Ancak, parametreleri için hem diziler hem de dizeler sağlayamıyoruz. Diziler yalnızca diğer dizilerle birleştirilir.

### <a name="return-value"></a>Döndürülen değer

Bir dize veya art arda eklenmiş değerler dizisi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-array.json) , iki dizinin nasıl birleştirileceğini gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| return | Dizi | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-string.json) iki dize değerinin nasıl birleştirileceğini ve birleştirilmiş bir dizenin nasıl dönebileceğinizi gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Içtoutput | Dize | önek-5yıj4yıjf5mbg72 |

## <a name="contains"></a>içerir

`contains(container, itemToFind)`

Bir dizinin bir değer içerip içermediğini denetler, bir nesne anahtar içeriyor veya dize bir alt dize içeriyor. Dize karşılaştırma büyük/küçük harfe duyarlıdır. Ancak, bir nesne bir anahtar içeriyorsa test edilirken karşılaştırma büyük/küçük harfe duyarlıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
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

| Name | Tür | Değer |
| ---- | ---- | ----- |
| stringTrue | Bool | True |
| stringFalse | Bool | False |
| objectTrue | Bool | True |
| Objectfali | Bool | False |
| arrayTrue | Bool | True |
| arrayFalse | Bool | False |

## <a name="createarray"></a>createarray

`createArray (arg1, arg2, arg3, ...)`

Parametrelerden bir dizi oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |String, Integer, array veya Object |Dizideki ilk değer. |
| ek bağımsız değişkenler |No |String, Integer, array veya Object |Dizideki ek değerler. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/createarray.json) , createarray 'in farklı türlerle nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| stringArray | Dizi | ["a", "b", "c"] |
| ıntarray | Dizi | [1, 2, 3] |
| objectArray | Dizi | [{"One": "a", "iki": "b", "üç": "c"}] |
| arrayArray | Dizi | [["bir", "iki", "üç"]] |

## <a name="empty"></a>empty

`empty(itemToTest)`

Bir dizi, nesne veya dize boş olup olmadığını belirler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
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

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayEmpty | Bool | True |
| objectEmpty | Bool | True |
| stringEmpty | Bool | True |

## <a name="first"></a>adı

`first(arg1)`

Dizinin ilk öğesini veya dizenin ilk karakterini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dizi veya dize |İlk öğe veya karakteri alma değeri. |

### <a name="return-value"></a>Döndürülen değer

Bir dizideki ilk öğenin türü (dize, int, dizi veya nesne) ya da bir dizenin ilk karakteri.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/first.json) , ilk işlevin dizi ve dize ile nasıl kullanılacağını göstermektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dize | bir |
| stringOutput | Dize | O |

## <a name="intersection"></a>imin

`intersection(arg1, arg2, arg3, ...)`

Parametrelerden ortak öğelerle tek bir dizi veya nesne döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ilk değer. |
| arg2 |Yes |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ikinci değer. |
| ek bağımsız değişkenler |No |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ek değerler. |

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

## <a name="last"></a>Son

`last (arg1)`

Dizinin son öğesini veya dizenin son karakterini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dizi veya dize |Son öğe veya karakteri alma değeri. |

### <a name="return-value"></a>Döndürülen değer

Bir dizideki son öğenin türü (dize, int, dizi veya nesne) ya da bir dizenin son karakteri.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/last.json) , son işlevin bir dizi ve dize ile nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dize | Üç |
| stringOutput | Dize | e |

## <a name="length"></a>length

`length(arg1)`

Bir dizideki öğelerin sayısını, bir dizedeki karakterleri veya bir nesnedeki kök düzeyi özellikleri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
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

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayLength | int | 3 |
| stringLength | int | 13 |
| objectLength | int | 4 |

Bu işlevi, kaynak oluştururken yineleme sayısını belirtmek için bir dizi ile birlikte kullanabilirsiniz. Aşağıdaki örnekte, **siteNames** parametresi Web sitelerini oluştururken kullanılacak bir ad dizisine başvuracaktır.

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

Bu işlevi bir dizi ile kullanma hakkında daha fazla bilgi için, bkz. [Azure Resource Manager birden fazla kaynak örneği oluşturma](copy-resources.md).

## <a name="max"></a>max

`max(arg1)`

Tamsayılar dizisinden en büyük değeri veya virgülle ayrılmış tamsayılar listesini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |tamsayılar dizisi veya virgülle ayrılmış tamsayılar listesi |En büyük değeri almak için koleksiyon. |

### <a name="return-value"></a>Döndürülen değer

En büyük değeri temsil eden bir int.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json) , en fazla bir diziyi ve tamsayılar listesini nasıl kullanacağınızı gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | int | 5 |
| ıntoutput | int | 5 |

## <a name="min"></a>dk

`min(arg1)`

Tamsayılar dizisinden en küçük değeri veya virgülle ayrılmış tamsayılar listesini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |tamsayılar dizisi veya virgülle ayrılmış tamsayılar listesi |En küçük değeri almak için koleksiyon. |

### <a name="return-value"></a>Döndürülen değer

En küçük değeri temsil eden bir int.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json) , en az bir dizi ve tamsayılar listesi ile min nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | int | 0 |
| ıntoutput | int | 0 |

## <a name="range"></a>aralık

`range(startIndex, count)`

Bir başlangıç tamnoktasından tamsayılar dizisi oluşturur ve bir dizi öğe içerir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| startIndex |Yes |int |Dizideki ilk tamsayı. StartIndex ve Count toplamı 2147483647 değerinden büyük olmamalıdır. |
| count |Yes |int |Dizideki tamsayıların sayısı. 10000 kadar negatif olmayan bir tamsayı olmalıdır. |

### <a name="return-value"></a>Döndürülen değer

Tamsayılar dizisi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/range.json) , Aralık işlevinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| rangeOutput | Dizi | [5, 6, 7] |

## <a name="skip"></a>Atla

`skip(originalValue, numberToSkip)`

Dizide belirtilen sayıdan sonraki tüm öğeleri içeren bir dizi döndürür veya dizedeki belirtilen sayıdan sonraki tüm karakterleri içeren bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| originalValue |Yes |dizi veya dize |Atlama için kullanılacak dizi veya dize. |
| numberToSkip |Yes |int |Atlanacak öğe veya karakter sayısı. Bu değer 0 veya daha azsa, değer içindeki tüm öğeler veya karakterler döndürülür. Dizi veya dizenin uzunluğundan daha büyükse boş bir dizi veya dize döndürülür. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi veya dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/skip.json) dizideki belirtilen sayıda öğeyi ve bir dizedeki belirtilen karakter sayısını atlar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dizi | ["üç"] |
| stringOutput | Dize | 2 3 |

## <a name="take"></a>almanız

`take(originalValue, numberToTake)`

Dizinin başından itibaren belirtilen sayıda öğe içeren bir dizi veya dizenin başından itibaren belirtilen sayıda karakter içeren bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| originalValue |Yes |dizi veya dize |Öğelerin ele aldığı dizi veya dize. |
| numberToTake |Yes |int |Gerçekleştirilecek öğe veya karakter sayısı. Bu değer 0 veya daha azsa, boş bir dizi veya dize döndürülür. Belirtilen dizi veya dizenin uzunluğundan daha büyükse, dizideki veya dizedeki tüm öğeler döndürülür. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi veya dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/take.json) , diziden belirtilen sayıda öğeyi ve bir dizeden karakter alır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dizi | ["bir", "iki"] |
| stringOutput | Dize | on |

## <a name="union"></a>birleşim

`union(arg1, arg2, arg3, ...)`

Parametrelerden tüm öğeleri içeren tek bir dizi veya nesne döndürür. Yinelenen değerler veya anahtarlar yalnızca bir kez dahil edilir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dizi veya nesne |Öğeleri birleştirmek için kullanılacak ilk değer. |
| arg2 |Yes |dizi veya nesne |Öğeleri birleştirmek için kullanılacak ikinci değer. |
| ek bağımsız değişkenler |No |dizi veya nesne |Öğeleri birleştirmek için kullanılacak ek değerler. |

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
