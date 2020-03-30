---
title: Şablon işlevleri - string
description: Dizeleri ile çalışmak için bir Azure Kaynak Yöneticisi şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 07/31/2019
ms.openlocfilehash: 070133c3db538e5df76644b62c25ced916adc4af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156285"
---
# <a name="string-functions-for-arm-templates"></a>ARM şablonları için dize işlevleri

Kaynak Yöneticisi, Azure Kaynak Yöneticisi (ARM) şablonlarınızdaki dizeleri ile çalışmak için aşağıdaki işlevleri sağlar:

* [base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [Concat](#concat)
* [Içerir](#contains)
* [dataUri](#datauri)
* [dataUritostring](#datauritostring)
* [empty](#empty)
* [Endswith](#endswith)
* [Ilk](#first)
* [Biçim](#format)
* [Guıd](#guid)
* [ındexof](#indexof)
* [Son](#last)
* [Lastındexof](#lastindexof)
* [Uzun -luğu](#length)
* [newGuid](#newguid)
* [Padleft](#padleft)
* [Değiştirmek](#replace)
* [Atlamak](#skip)
* [Split](#split)
* [Startswith](#startswith)
* [Dize](#string)
* [Dize](#substring)
* [almak](#take)
* [Tolower](#tolower)
* [Toupper](#toupper)
* [Döşeme](#trim)
* [uniqueString](#uniquestring)
* [Urı](#uri)
* [uriComponent](#uricomponent)
* [uriComponentToString](#uricomponenttostring)
* [utcŞimdi](#utcnow)

## <a name="base64"></a>base64

`base64(inputString)`

Giriş dizesinin base64 temsilini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| ınputstring |Evet |string |Base64 gösterimi olarak döndürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Base64 gösterimini içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) base64 işlevinin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| base64Çıktı | Dize | b25lLCB0d28sIHRocmVl |
| toStringOutput | Dize | Bir iki üç |
| toJsonÇıktı | Nesne | {"bir": "a", "iki": "b"} |

## <a name="base64tojson"></a>base64ToJson

`base64tojson`

Base64 gösterimini JSON nesnesine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| base64Değer |Evet |string |Bir JSON nesnesine dönüştürmek için base64 gösterimi. |

### <a name="return-value"></a>Döndürülen değer

Bir JSON nesnesi.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) base64 değerini dönüştürmek için base64ToJson işlevini kullanır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| base64Çıktı | Dize | b25lLCB0d28sIHRocmVl |
| toStringOutput | Dize | Bir iki üç |
| toJsonÇıktı | Nesne | {"bir": "a", "iki": "b"} |

## <a name="base64tostring"></a>base64ToString

`base64ToString(base64Value)`

Base64 gösterimini bir dize dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| base64Değer |Evet |string |Bir dize dönüştürmek için base64 gösterimi. |

### <a name="return-value"></a>Döndürülen değer

Dönüştürülmüş base64 değerinin bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) base64 değerini dönüştürmek için base64ToString işlevini kullanır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| base64Çıktı | Dize | b25lLCB0d28sIHRocmVl |
| toStringOutput | Dize | Bir iki üç |
| toJsonÇıktı | Nesne | {"bir": "a", "iki": "b"} |

## <a name="concat"></a>concat

`concat (arg1, arg2, arg3, ...)`

Birden çok dize değerlerini birleştirir ve sıkıştırılmış dizeyi döndürür veya birden çok diziyi birleştirir ve sıkıştırılmış diziyi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya dizi |Concatenation için ilk dize veya dizi. |
| ek bağımsız değişkenler |Hayır |dize veya dizi |Ek dizeleri veya dizileri eşleştirme için sırayla sırayla. |

Bu işlev herhangi bir sayıda bağımsız değişken alabilir ve parametreler için dizeleri veya dizileri kabul edebilir. Ancak, parametreler için hem dizileri hem de dizeleri sağlayamazsınız. Dizeleri yalnızca diğer dizeleri ile concatenated vardır.

### <a name="return-value"></a>Döndürülen değer

Bir dize veya birdizi concatenated değerleri.

### <a name="examples"></a>Örnekler

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

## <a name="contains"></a>içerir

`contains (container, itemToFind)`

Bir dizi bir değer, bir nesne bir anahtar veya bir dize bir alt dize içerir olup olmadığını denetler. Dize karşılaştırması büyük/küçük harf duyarlıdır. Ancak, bir nesne bir anahtar içeriyorsa sınanırken, karşılaştırma büyük/küçük harf duyarsızdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| kapsayıcı |Evet |dizi, nesne veya dize |Bulunacak değeri içeren değer. |
| itemToFind |Evet |dize veya int |Bulunması gereken değer. |

### <a name="return-value"></a>Döndürülen değer

Öğe bulunursa **doğru;** aksi takdirde, **False**.

### <a name="examples"></a>Örnekler

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

## <a name="datauri"></a>dataUri

`dataUri(stringToConvert)`

Bir değeri veri URI'sine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToConvert |Evet |string |Veri URI'sine dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Veri URI olarak biçimlendirilmiş bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) bir değeri veri URI'sine dönüştürür ve veri URI'sini bir dize dönüştürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| dataUriOutput | Dize | veri:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | Dize | Merhaba Dünya! |

## <a name="datauritostring"></a>dataUritostring

`dataUriToString(dataUriToConvert)`

Uri biçimlendirilmiş bir veri değerini bir dize dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| dataUritoconvert |Evet |string |Dönüştürmek için veri URI değeri. |

### <a name="return-value"></a>Döndürülen değer

Dönüştürülen değeri içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) bir değeri veri URI'sine dönüştürür ve veri URI'sini bir dize dönüştürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| dataUriOutput | Dize | veri:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | Dize | Merhaba Dünya! |

## <a name="empty"></a>empty

`empty(itemToTest)`

Bir dizinin, nesnenin veya dizenin boş olup olmadığını belirler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| itemToTest |Evet |dizi, nesne veya dize |Boş olup olmadığını kontrol etmek için değer. |

### <a name="return-value"></a>Döndürülen değer

Değer boşsa **True** döndürür; aksi takdirde, **False**.

### <a name="examples"></a>Örnekler

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

## <a name="endswith"></a>endsWith

`endsWith(stringToSearch, stringToFind)`

Bir dize bir değer ile bitenolup olmadığını belirler. Karşılaştırma büyük/küçük harf duyarsız.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToSearch |Evet |string |Bulunacak öğeyi içeren değer. |
| stringToFind |Evet |string |Bulunması gereken değer. |

### <a name="return-value"></a>Döndürülen değer

Dize son karakter veya karakterleri değeri eşleşirse **doğru;** aksi takdirde, **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) startsWith ve endsWith işlevlerinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| başlarTrue | Bool | True |
| startsCapTrue | Bool | True |
| başlarFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

## <a name="first"></a>Ilk

`first(arg1)`

Dize ilk karakteri veya dizinin ilk öğesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |İlk öğeyi veya karakteri almak için değer. |

### <a name="return-value"></a>Döndürülen değer

Bir dizideki ilk öğenin ilk karakterin ini veya türü (dize, int, dizi veya nesne) dizesi.

### <a name="examples"></a>Örnekler

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

## <a name="format"></a>biçim

`format(formatString, arg1, arg2, ...)`

Giriş değerlerinden biçimlendirilmiş bir dize oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| formatString | Evet | string | Bileşik biçim dizesi. |
| arg1 | Evet | dize, sonda veya boolean | Biçimlendirilmiş dizeye dahil edilen değer. |
| ek bağımsız değişkenler | Hayır | dize, sonda veya boolean | Biçimlendirilmiş dizeye eklenmiş değerler. |

### <a name="remarks"></a>Açıklamalar

Şablonunuzda bir dize biçimlendirmek için bu işlevi kullanın. [System.String.Format](/dotnet/api/system.string.format) yöntemiyle aynı biçimlendirme seçeneklerini .NET'te kullanır.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablon, biçim işlevinin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "greeting": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "name": {
            "type": "string",
            "defaultValue": "User"
        },
        "numberToFormat": {
            "type": "int",
            "defaultValue": 8175133
        }
    },
    "resources": [
    ],
    "outputs": {
        "formatTest": {
            "type": "string",
            "value": "[format('{0}, {1}. Formatted number: {2:N0}', parameters('greeting'), parameters('name'), parameters('numberToFormat'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| formatTest | Dize | Merhaba, Kullanıcı. Biçimlendirilmiş sayı: 8.175.133 |

## <a name="guid"></a>Guıd

`guid(baseString, ...)`

Parametreler olarak sağlanan değerleri temel alan, genel olarak benzersiz bir tanımlayıcı biçiminde bir değer oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| baseString |Evet |string |GUID oluşturmak için karma işlevinde kullanılan değer. |
| gerektiğinde ek parametreler |Hayır |string |Benzersizlik düzeyini belirten değeri oluşturmak için gerektiği kadar dize ekleyebilirsiniz. |

### <a name="remarks"></a>Açıklamalar

Bu işlev, genel olarak benzersiz bir tanımlayıcı biçiminde bir değer oluşturmanız gerektiğinde yararlıdır. Sonuç için benzersizlik kapsamını sınırlayan parametre değerleri sağlarsınız. Adın abonelik, kaynak grubu veya dağıtım için benzersiz olup olmadığını belirtebilirsiniz.

Döndürülen değer rasgele bir dize değil, parametrelerüzerinde bir karma işlevsonucudur. Döndürülen değer 36 karakter uzunluğundadır. Dünya çapında benzersiz değildir. Parametrelerin karma değerine dayanmayan yeni bir GUID oluşturmak için [yeniGuid](#newguid) işlevini kullanın.

Aşağıdaki örnekler, sık kullanılan düzeyler için benzersiz bir değer oluşturmak için guid'in nasıl kullanılacağını gösterir.

Benzersiz abonelik kapsamı

```json
"[guid(subscription().subscriptionId)]"
```

Kaynak grubuna özgü dürbünü

```json
"[guid(resourceGroup().id)]"
```

Kaynak grubu için dağıtıma yönelik benzersiz kapsamda

```json
"[guid(resourceGroup().id, deployment().name)]"
```

### <a name="return-value"></a>Döndürülen değer

Genel olarak benzersiz bir tanımlayıcı biçiminde 36 karakter içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/guid.json) guid sonuçları döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [],
    "outputs": {
        "guidPerSubscription": {
            "value": "[guid(subscription().subscriptionId)]",
            "type": "string"
        },
        "guidPerResourceGroup": {
            "value": "[guid(resourceGroup().id)]",
            "type": "string"
        },
        "guidPerDeployment": {
            "value": "[guid(resourceGroup().id, deployment().name)]",
            "type": "string"
        }
    }
}
```

## <a name="indexof"></a>ındexof

`indexOf(stringToSearch, stringToFind)`

Bir dize içindeki bir değerin ilk konumunu döndürür. Karşılaştırma büyük/küçük harf duyarsız.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToSearch |Evet |string |Bulunacak öğeyi içeren değer. |
| stringToFind |Evet |string |Bulunması gereken değer. |

### <a name="return-value"></a>Döndürülen değer

Öğenin bulunabilmek için konumunu temsil eden bir karşıcı. Değer sıfır tabanlıdır. Öğe bulunmazsa, -1 döndürülür.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) indexOf ve lastIndexOf işlevlerinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

## <a name="last"></a>Son

`last (arg1)`

Dize son karakteri veya dizinin son öğesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |Son öğeyi veya karakteri almak için değer. |

### <a name="return-value"></a>Döndürülen değer

Bir dizideki son öğenin son karakterinin veya türü (dize, int, dizi veya nesne) dizesi.

### <a name="examples"></a>Örnekler

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

## <a name="lastindexof"></a>Lastındexof

`lastIndexOf(stringToSearch, stringToFind)`

Bir dize içindeki bir değerin son konumunu döndürür. Karşılaştırma büyük/küçük harf duyarsız.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToSearch |Evet |string |Bulunacak öğeyi içeren değer. |
| stringToFind |Evet |string |Bulunması gereken değer. |

### <a name="return-value"></a>Döndürülen değer

Öğenin bulunacak son konumunu temsil eden bir karşıcı. Değer sıfır tabanlıdır. Öğe bulunmazsa, -1 döndürülür.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) indexOf ve lastIndexOf işlevlerinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

## <a name="length"></a>length

`length(string)`

Bir dizedeki karakter sayısını, dizideki öğeleri veya bir nesnedeki kök düzeyi özelliklerini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi, dize veya nesne |Öğe sayısını almak için kullanılacak dizi, karakter sayısını almak için kullanılacak dize veya kök düzeyi özelliklerinin sayısını almak için kullanılacak nesne. |

### <a name="return-value"></a>Döndürülen değer

Bir int. 

### <a name="examples"></a>Örnekler

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

## <a name="newguid"></a>newGuid

`newGuid()`

Genel olarak benzersiz bir tanımlayıcı biçiminde bir değer verir. **Bu işlev yalnızca bir parametre için varsayılan değerde kullanılabilir.**

### <a name="remarks"></a>Açıklamalar

Bu işlevi yalnızca bir parametrenin varsayılan değeri için bir ifade içinde kullanabilirsiniz. Bu işlevi şablonda başka bir yerde kullanmak bir hata döndürür. Her çağrıldığında farklı bir değer döndürdüğünden, işlev şablonun diğer bölümlerinde izin verilmez. Aynı şablonu aynı parametrelerle dağıtmak güvenilir bir şekilde aynı sonuçları üretmez.

YeniGuid işlevi [kılavuz](#guid) işlevden farklıdır, çünkü herhangi bir parametre almaz. Guid'i aynı parametreyle aradiğinizde, her seferinde aynı tanımlayıcıyı döndürür. Belirli bir ortam için aynı GUID'i güvenilir bir şekilde oluşturmanız gerektiğinde guid'i kullanın. Kaynakları bir test ortamına dağıtmak gibi her seferinde farklı bir tanımlayıcıya ihtiyacınız olduğunda newGuid'i kullanın.

Daha önceki [başarılı bir dağıtımı yeniden dağıtmak için bu seçeneği](rollback-on-error.md)kullanırsanız ve önceki dağıtım da yeniGuid kullanan bir parametre içeriyorsa, parametre yeniden değerlendirilmez. Bunun yerine, önceki dağıtımdaki parametre değeri geri alma dağıtımında otomatik olarak yeniden kullanılır.

Bir test ortamında, yalnızca kısa bir süre için yaşayan kaynakları tekrar tekrar dağıtmanız gerekebilir. Benzersiz adlar oluşturmak yerine, benzersiz adlar oluşturmak için [uniqueString](#uniquestring) ile newGuid'i kullanabilirsiniz.

Varsayılan değer için yeniGuid işlevine dayanan bir şablonu yeniden dağıtA erken dikkatli olun. Yeniden dağıttığınızda ve parametre için bir değer sağlamadığınızda, işlev yeniden değerlendirilir. Yeni bir kaynak oluşturmak yerine varolan bir kaynağı güncelleştirmek istiyorsanız, önceki dağıtımdan parametre değerini geçirin.

### <a name="return-value"></a>Döndürülen değer

Genel olarak benzersiz bir tanımlayıcı biçiminde 36 karakter içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablon, yeni bir tanımlayıcısı olan bir parametre gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "guidValue": {
            "type": "string",
            "defaultValue": "[newGuid()]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "guidOutput": {
            "type": "string",
            "value": "[parameters('guidValue')]"
        }
    }
}
```

Önceki örnekteki çıktı her dağıtım için değişir, ancak benzer olacaktır:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| guidOutput | string | b76a51fc-bd72-4a77-b9a2-3c29e7d2e551 |

Aşağıdaki örnek, bir depolama hesabı için benzersiz bir ad oluşturmak için yeniGuid işlevini kullanır. Bu şablon, depolama hesabının kısa bir süre için var olduğu ve yeniden dağıtılmadığı test ortamı için çalışabilir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "guidValue": {
            "type": "string",
            "defaultValue": "[newGuid()]"
        }
    },
    "variables": {
        "storageName": "[concat('storage', uniqueString(parameters('guidValue')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "location": "West US",
            "apiVersion": "2018-07-01",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {}
        }
    ],
    "outputs": {
        "nameOutput": {
            "type": "string",
            "value": "[variables('storageName')]"
        }
    }
}
```

Önceki örnekteki çıktı her dağıtım için değişir, ancak benzer olacaktır:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| nameOutput | string | depolamanziwvyru7uxie |


## <a name="padleft"></a>Padleft

`padLeft(valueToPad, totalLength, paddingCharacter)`

Toplam belirtilen uzunluğa ulaşana kadar sola karakterler ekleyerek sağ hizalanmış bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| valueToPad |Evet |dize veya int |Sağa hizalama değeri. |
| totalUzunluk |Evet |int |Döndürülen dizedeki toplam karakter sayısı. |
| dolguKarakter |Hayır |tek karakter |Toplam uzunluğa ulaşılına kadar sol dolgu için kullanılacak karakter. Varsayılan değer bir boşluk. |

Özgün dize pad'e karakter sayısından daha uzunsa, karakter eklenmez.

### <a name="return-value"></a>Döndürülen değer

En az belirtilen karakter sayısına sahip bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/padleft.json) toplam karakter sayısına ulaşana kadar sıfır karakterini ekleyerek kullanıcı tarafından sağlanan parametre değerinin nasıl pad yapılacağını gösterir. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| stringOutput | Dize | 0000000123 |

## <a name="replace"></a>Değiştirmek

`replace(originalString, oldString, newString)`

Bir dize deki tüm örnekleri başka bir dizeyle değiştirilen yeni bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| originalString |Evet |string |Bir dize tüm örnekleri başka bir dize ile değiştirilir olan değer. |
| oldString |Evet |string |Özgün dizeden kaldırılacak dize. |
| newString |Evet |string |Kaldırılan dize yerine eklenecek dize. |

### <a name="return-value"></a>Döndürülen değer

Değiştirilen karakterleri içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/replace.json) kullanıcı tarafından sağlanan dizedeki tüm tirelerin nasıl kaldırılış yapılacağını ve dizenin bir bölümünün başka bir dizeyle nasıl değiştirilebildiğini gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secondOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| ilkÇıktı | Dize | 1231231234 |
| secondOutput | Dize | 123-123-xxxx |

## <a name="skip"></a>Atla

`skip(originalValue, numberToSkip)`

Belirtilen karakter sayısından sonra tüm karakterleri içeren bir dize veya belirtilen sayıdan sonra tüm öğeleri içeren bir dizi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| Originalvalue |Evet |dizi veya dize |Atlama için kullanılacak dizi veya dize. |
| numaraToSkip |Evet |int |Atlasılayabilmek için öğelerin veya karakterlerin sayısı. Bu değer 0 veya daha az ise, değerdeki tüm öğeler veya karakterler döndürülür. Dizi veya dize uzunluğundan daha büyükse, boş bir dizi veya dize döndürülür. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi veya dize.

### <a name="examples"></a>Örnekler

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

## <a name="split"></a>split

`split(inputString, delimiter)`

Belirtilen sınırsızlar tarafından sınırlaştırılıyor olan girdi dizesinin alt dizeleri' ni içeren bir dizi dize verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| ınputstring |Evet |string |Bölünecek dize. |
| sınırlayıcı |Evet |dize veya dize dizisi |Dize yi bölmek için kullanılacak sınır layıcı. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/split.json) giriş dizesini virgülle ve virgülveya yarı iki nokta üst üste ile böler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| ilkÇıktı | Dizi | ["bir", "iki", "üç"] |
| secondOutput | Dizi | ["bir", "iki", "üç"] |

## <a name="startswith"></a>startsWith

`startsWith(stringToSearch, stringToFind)`

Bir dize bir değerle başlayıp başlamayacağını belirler. Karşılaştırma büyük/küçük harf duyarsız.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToSearch |Evet |string |Bulunacak öğeyi içeren değer. |
| stringToFind |Evet |string |Bulunması gereken değer. |

### <a name="return-value"></a>Döndürülen değer

Dizedeki ilk karakter veya karakterler değerle eşleşirse **doğru;** aksi takdirde, **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) startsWith ve endsWith işlevlerinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| başlarTrue | Bool | True |
| startsCapTrue | Bool | True |
| başlarFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

## <a name="string"></a>string

`string(valueToConvert)`

Belirtilen değeri bir dize dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| valueToConvert |Evet | Herhangi biri |Dize dönüştürmek için değer. Nesneler ve diziler de dahil olmak üzere her tür değer dönüştürülebilir. |

### <a name="return-value"></a>Döndürülen değer

Dönüştürülen değer in bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/string.json) farklı türde değerlerin dizeleri nasıl dönüştürülür gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Dize | {"valueA":10,"valueB":"Örnek Metin"} |
| diziÇıktı | Dize | ["a","b","c"] |
| intOutput | Dize | 5 |

## <a name="substring"></a>Dize

`substring(stringToParse, startIndex, length)`

Belirtilen karakter konumunda başlayan ve belirtilen karakter sayısını içeren bir alt dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToParse |Evet |string |Alt dize ayıklanır orijinal dize. |
| Startındex |Hayır |int |Substring için sıfır tabanlı başlangıç karakteri konumu. |
| length |Hayır |int |Alt dize için karakter sayısı. Dize içindeki bir konuma başvurmalı. Sıfır veya daha büyük olmalı. |

### <a name="return-value"></a>Döndürülen değer

Alt dize. Veya uzunluk sıfırise boş bir dize.

### <a name="remarks"></a>Açıklamalar

Alt dize dize sonuna kadar uzandığında veya uzunluk sıfırdan az olduğunda işlev başarısız olur. Aşağıdaki örnekhata ile başarısız "Dizin ve uzunluk parametreleri dize içinde bir konuma başvurmak gerekir. Dizin parametresi: '0', uzunluk parametresi: '11', dize parametresinin uzunluğu: '10'.".

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/substring.json) bir parametreden bir alt dize ayıklar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| substringOutput | Dize | iki |

## <a name="take"></a>almak

`take(originalValue, numberToTake)`

Dize başından belirtilen karakter sayısına sahip bir dize veya dizinin başlangıcından belirtilen öğe sayısına sahip bir dizi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| Originalvalue |Evet |dizi veya dize |Öğeleri almak için dizi veya dize. |
| numberToTake |Evet |int |Ele alınacak öğe veya karakter sayısı. Bu değer 0 veya daha az ise, boş bir dizi veya dize döndürülür. Verilen dizinin veya dizenin uzunluğundan daha büyükse, dizi veya dizedeki tüm öğeler döndürülür. |

### <a name="return-value"></a>Döndürülen değer

Bir dizi veya dize.

### <a name="examples"></a>Örnekler

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

## <a name="tolower"></a>Tolower

`toLower(stringToChange)`

Belirtilen dizeyi küçük harfe dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToChange |Evet |string |Küçük harfe dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Dize küçük harfe dönüştürüldü.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) bir parametre değerini küçük harfe ve büyük harfe dönüştürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| toLowerOutput | Dize | Bir iki üç |
| toUpperOutput | Dize | Bir iki üç |

## <a name="toupper"></a>Toupper

`toUpper(stringToChange)`

Belirtilen dizeyi büyük harfe dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToChange |Evet |string |Büyük harfe dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Dize büyük harfe dönüştürüldü.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) bir parametre değerini küçük harfe ve büyük harfe dönüştürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| toLowerOutput | Dize | Bir iki üç |
| toUpperOutput | Dize | Bir iki üç |

## <a name="trim"></a>Döşeme

`trim (stringToTrim)`

Belirtilen dizeden tüm satır baş ve sondaki beyaz boşluk karakterlerini kaldırır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToTrim |Evet |string |Kırpma değeri. |

### <a name="return-value"></a>Döndürülen değer

Önde gelen ve beyaz boşluk karakterleri izleyerek dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/trim.json) parametredeki beyaz alan karakterlerini kırpar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| return | Dize | Bir iki üç |

## <a name="uniquestring"></a>uniqueString

`uniqueString (baseString, ...)`

Parametreler olarak sağlanan değerleri temel alan bir deterministik karma dize oluşturur. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| baseString |Evet |string |Benzersiz bir dize oluşturmak için karma işlevinde kullanılan değer. |
| gerektiğinde ek parametreler |Hayır |string |Benzersizlik düzeyini belirten değeri oluşturmak için gerektiği kadar dize ekleyebilirsiniz. |

### <a name="remarks"></a>Açıklamalar

Bu işlev, bir kaynak için benzersiz bir ad oluşturmanız gerektiğinde yararlıdır. Sonuç için benzersizlik kapsamını sınırlayan parametre değerleri sağlarsınız. Adın abonelik, kaynak grubu veya dağıtım için benzersiz olup olmadığını belirtebilirsiniz. 

Döndürülen değer rasgele bir dize değil, karma işlevin sonucudur. Döndürülen değer 13 karakter uzunluğundadır. Dünya çapında benzersiz değildir. Anlamlı bir ad oluşturmak için değeri adlandırma kuralınızdan bir önek ile birleştirmek isteyebilirsiniz. Aşağıdaki örnek, döndürülen değerin biçimini gösterir. Gerçek değer sağlanan parametrelere göre değişir.

    tcvhiyu5h2o5o

Aşağıdaki örnekler, sık kullanılan düzeyler için benzersiz bir değer oluşturmak için uniqueString'in nasıl kullanılacağını gösterir.

Benzersiz abonelik kapsamı

```json
"[uniqueString(subscription().subscriptionId)]"
```

Kaynak grubuna özgü dürbünü

```json
"[uniqueString(resourceGroup().id)]"
```

Kaynak grubu için dağıtıma yönelik benzersiz kapsamda

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

Aşağıdaki örnek, kaynak grubunuza dayalı bir depolama hesabı için benzersiz bir ad oluşturmanın nasıl yapılacağını gösterir. Kaynak grubunun içinde, aynı şekilde oluşturulmuşsa ad benzersiz değildir.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

Bir şablonu her dağıttYaptığınızda yeni bir benzersiz ad oluşturmanız gerekiyorsa ve kaynağı güncelleştirmeyi düşünmüyorsanız, uniqueString ile [utcNow](#utcnow) işlevini kullanabilirsiniz. Bu yaklaşımı bir test ortamında kullanabilirsiniz. Örneğin, [bkz. utcNow](#utcnow).

### <a name="return-value"></a>Döndürülen değer

13 karakter içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uniquestring.json) benzersiz dize sonuçları döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

## <a name="uri"></a>Urı

`uri (baseUri, relativeUri)`

BaseUri ve relativeUri dizesini birleştirerek mutlak bir URI oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| Baseuri |Evet |string |Temel uri dizesi. Bu tablodan sonra açıklandığı gibi, sondaki eğik çizginin ('/') işlenmesiyle ilgili davranışı gözlemlemek için dikkatli olmaya özen.  |
| Relativeuri |Evet |string |Göreceli uri dize baz uri dize eklemek için. |

* **BaseUri** bir iz çizgi biterse, sonuç sadece **baseUri** **relativeUri**tarafından takip edilir.

* **BaseUri** bir iz eğik çizgi ile sona ermezse iki şey olur.  

   * **BaseUri** hiç eğik çizgi varsa (kenara "/ " ön yakın) sonuç sadece **baseUri relativeUri**tarafından takip edilir. **baseUri**

   * **BaseUri** bazı kesikler varsa, ama bir eğik çizgi ile bitmiyorsa, son eğik çizgi itibaren her şey **baseUri** kaldırılır ve sonuç **relativeUri** tarafından takip **baseUri**olduğunu .
     
İşte bazı örnekler:

```
uri('http://contoso.org/firstpath', 'myscript.sh') -> http://contoso.org/myscript.sh
uri('http://contoso.org/firstpath/', 'myscript.sh') -> http://contoso.org/firstpath/myscript.sh
uri('http://contoso.org/firstpath/azuredeploy.json', 'myscript.sh') -> http://contoso.org/firstpath/myscript.sh
uri('http://contoso.org/firstpath/azuredeploy.json/', 'myscript.sh') -> http://contoso.org/firstpath/azuredeploy.json/myscript.sh
```
Tam ayrıntılar **için, baseUri** ve **relativeUri** parametreleri [RFC 3986, bölüm 5'te](https://tools.ietf.org/html/rfc3986#section-5)belirtildiği gibi çözülür.

### <a name="return-value"></a>Döndürülen değer

Taban ve göreli değerler için mutlak URI'yi temsil eden bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek, üst şablonun değerini temel alan iç içe bir şablona nasıl bağlantı oluşturulabildiğini gösterir.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) uri, uriComponent ve uriComponentToString'in nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| uriOutput | Dize | `http://contoso.com/resources/nested/azuredeploy.json` |
| componentOutput | Dize | `http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json` |
| toStringOutput | Dize | `http://contoso.com/resources/nested/azuredeploy.json` |

## <a name="uricomponent"></a>uriComponent

`uricomponent(stringToEncode)`

Bir URI kodlar.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToEncode |Evet |string |Kodlamak için değer. |

### <a name="return-value"></a>Döndürülen değer

URI kodlanmış değer dizisi.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) uri, uriComponent ve uriComponentToString'in nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| uriOutput | Dize | `http://contoso.com/resources/nested/azuredeploy.json` |
| componentOutput | Dize | `http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json` |
| toStringOutput | Dize | `http://contoso.com/resources/nested/azuredeploy.json` |

## <a name="uricomponenttostring"></a>uriComponentToString

`uriComponentToString(uriEncodedString)`

URI kodlanmış bir değer dizesini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Evet |string |URI kodlanmış değeri bir dize dönüştürmek için. |

### <a name="return-value"></a>Döndürülen değer

Uri kodlanmış değerin şifresi çözülmüş bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) uri, uriComponent ve uriComponentToString'in nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| uriOutput | Dize | `http://contoso.com/resources/nested/azuredeploy.json` |
| componentOutput | Dize | `http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json` |
| toStringOutput | Dize | `http://contoso.com/resources/nested/azuredeploy.json` |

## <a name="utcnow"></a>utcŞimdi

`utcNow(format)`

Geçerli (UTC) tarih saati değerini belirtilen biçimde verir. Format sağlanmadıysa, ISO 8601 (yyyyMddTHHmmssZ) biçimi kullanılır. **Bu işlev yalnızca bir parametre için varsayılan değerde kullanılabilir.**

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| biçim |Hayır |string |URI kodlanmış değeri bir dize dönüştürmek için. Standart [biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings)kullanın. |

### <a name="remarks"></a>Açıklamalar

Bu işlevi yalnızca bir parametrenin varsayılan değeri için bir ifade içinde kullanabilirsiniz. Bu işlevi şablonda başka bir yerde kullanmak bir hata döndürür. Her çağrıldığında farklı bir değer döndürdüğünden, işlev şablonun diğer bölümlerinde izin verilmez. Aynı şablonu aynı parametrelerle dağıtmak güvenilir bir şekilde aynı sonuçları üretmez.

Daha önceki [başarılı bir dağıtımı yeniden dağıtmak için bu seçeneği](rollback-on-error.md)kullanırsanız ve önceki dağıtım utcNow kullanan bir parametre içeriyorsa, parametre yeniden değerlendirilmez. Bunun yerine, önceki dağıtımdaki parametre değeri geri alma dağıtımında otomatik olarak yeniden kullanılır.

Varsayılan bir değer için utcNow işlevine dayanan bir şablonu yeniden dağıtılırken dikkatli olun. Yeniden dağıttığınızda ve parametre için bir değer sağlamadığınızda, işlev yeniden değerlendirilir. Yeni bir kaynak oluşturmak yerine varolan bir kaynağı güncelleştirmek istiyorsanız, önceki dağıtımdan parametre değerini geçirin.

### <a name="return-value"></a>Döndürülen değer

Geçerli UTC tarih saati değeri.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablon, tarih saati değeri için farklı biçimleri gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcValue": {
            "type": "string",
            "defaultValue": "[utcNow()]"
        },
        "utcShortValue": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "utcCustomValue": {
            "type": "string",
            "defaultValue": "[utcNow('M d')]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "utcOutput": {
            "type": "string",
            "value": "[parameters('utcValue')]"
        },
        "utcShortOutput": {
            "type": "string",
            "value": "[parameters('utcShortValue')]"
        },
        "utcCustomOutput": {
            "type": "string",
            "value": "[parameters('utcCustomValue')]"
        }
    }
}
```

Önceki örnekteki çıktı her dağıtım için değişir, ancak benzer olacaktır:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| utcOutput | string | 20190305T175318Z |
| utcShortOutput | string | 03/05/2019 |
| utcCustomOutput | string | 3 5 |

Sonraki örnek, etiket değeri ayarlarken işlevden bir değerin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcShort": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "rgName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "name": "[parameters('rgName')]",
            "location": "westeurope",
            "tags":{
                "createdDate": "[parameters('utcShort')]"
            },
            "properties":{}
        }
    ],
    "outputs": {
        "utcShort": {
            "type": "string",
            "value": "[parameters('utcShort')]"
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Kaynak Yöneticisi şablonundaki bölümlerin açıklaması için [bkz.](template-syntax.md)
* Birden çok şablonu birleştirmek için bkz: [Azure Kaynak Yöneticisi ile bağlantılı şablonları kullanma.](linked-templates.md)
* Bir kaynak türü oluştururken belirli sayıda kez yeniden sıralamak için azure [kaynak yöneticisinde birden çok kaynak örneği oluşturma](copy-resources.md)bölümüne bakın.
* Oluşturduğunuz şablonu nasıl dağıtabileceğinizi görmek için Azure [Kaynak Yöneticisi şablonuyla bir uygulama dağıt'a](deploy-powershell.md)bakın.

