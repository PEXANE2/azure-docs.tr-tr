---
title: Şablon işlevleri-karşılaştırma
description: Değerleri karşılaştırmak için Azure Resource Manager şablonda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 01d66f43cf73dcc9228118db5a9b6149b19ee66d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84677840"
---
# <a name="comparison-functions-for-arm-templates"></a>ARM şablonları için karşılaştırma işlevleri

Kaynak Yöneticisi, Azure Resource Manager (ARM) şablonlarınıza karşılaştırmalar yapmak için çeşitli işlevler sağlar.

* [Coalesce](#coalesce)
* [eşittir](#equals)
* [büyüktür](#greater)
* [greaterOrEquals](#greaterorequals)
* [büyüktür](#less)
* [lessOrEquals](#lessorequals)

## <a name="coalesce"></a>Coalesce

`coalesce(arg1, arg2, arg3, ...)`

Parametrelerden null olmayan ilk değeri döndürür. Boş dizeler, boş diziler ve boş nesneler null değil.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int, String, array veya Object |Null için sınanacak ilk değer. |
| ek bağımsız değişkenler |Hayır |int, String, array veya Object |Null için sınanacak ek değerler. |

### <a name="return-value"></a>Döndürülen değer

Bir String, int, array veya Object olabilen, null olmayan ilk parametrelerin değeri. Tüm parametreler null ise null.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/coalesce.json) , farklı birleşim kullanımlarından gelen çıktıyı gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| stringOutput | Dize | default |
| ıntoutput | int | 1 |
| objectOutput | Nesne | {"First": "varsayılan"} |
| arrayOutput | Dizi |  [1] |
| Emptızput | Bool | True |

## <a name="equals"></a>eşittir

`equals(arg1, arg2)`

İki değerin birbirlerine eşit olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int, String, array veya Object |Eşitlik için denetlenecek ilk değer. |
| arg2 |Evet |int, String, array veya Object |Eşitlik için denetlenecek ikinci değer. |

### <a name="return-value"></a>Döndürülen değer

Değerler eşitse **true** değerini döndürür; Aksi takdirde, **false**.

### <a name="remarks"></a>Açıklamalar

Eşittir işlevi genellikle `condition` bir kaynağın dağıtılıp dağıtılmadığını test etmek için öğesiyle birlikte kullanılır.

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/equals.json) , eşitlik için farklı değer türlerini denetler. Tüm varsayılan değerler doğru döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 1
        },
        "firstString": {
            "type": "string",
            "defaultValue": "a"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "firstObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[equals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[equals(parameters('firstString'), parameters('secondString'))]"
        },
        "checkArrays": {
            "type": "bool",
            "value": "[equals(parameters('firstArray'), parameters('secondArray'))]"
        },
        "checkObjects": {
            "type": "bool",
            "value": "[equals(parameters('firstObject'), parameters('secondObject'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Iadeler | Bool | True |
| checkStrings | Bool | True |
| Checkarışın | Bool | True |
| checkObjects | Bool | True |

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) , **eşittir**ile [değil](template-functions-logical.md#not) .

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
}
```

Yukarıdaki örnekteki çıktı:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Checttequals | Bool | True |

## <a name="greater"></a>büyüktür

`greater(arg1, arg2)`

İlk değerin ikinci değerden büyük olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int veya String |Daha büyük karşılaştırma için ilk değer. |
| arg2 |Evet |int veya String |Daha büyük karşılaştırma için ikinci değer. |

### <a name="return-value"></a>Döndürülen değer

İlk değer ikinci değerden büyükse **true** değerini döndürür; Aksi takdirde, **false**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/greater.json) , bir değerin diğerine göre daha büyük olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greater(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greater(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Iadeler | Bool | False |
| checkStrings | Bool | True |

## <a name="greaterorequals"></a>greaterOrEquals

`greaterOrEquals(arg1, arg2)`

İlk değerin ikinci değere eşit veya ondan büyük olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int veya String |Daha büyük veya eşit karşılaştırma için ilk değer. |
| arg2 |Evet |int veya String |Daha büyük veya eşit karşılaştırma için ikinci değer. |

### <a name="return-value"></a>Döndürülen değer

İlk değer ikinci değerden büyük veya bu değere eşitse **true** değerini döndürür; Aksi takdirde, **false**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/greaterorequals.json) , bir değerin diğerine eşit veya ondan büyük olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Iadeler | Bool | False |
| checkStrings | Bool | True |

## <a name="less"></a>daha az

`less(arg1, arg2)`

İlk değerin ikinci değerden küçük olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int veya String |Daha az karşılaştırma için ilk değer. |
| arg2 |Evet |int veya String |Daha az karşılaştırma için ikinci değer. |

### <a name="return-value"></a>Döndürülen değer

İlk değer ikinci değerden küçükse **true** değerini döndürür; Aksi takdirde, **false**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/less.json) , bir değerin diğerine göre daha az olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[less(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[less(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Iadeler | Bool | True |
| checkStrings | Bool | False |

## <a name="lessorequals"></a>lessOrEquals

`lessOrEquals(arg1, arg2)`

İlk değerin ikinci değere eşit veya ondan küçük olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int veya String |Less veya eşittir karşılaştırması için ilk değer. |
| arg2 |Evet |int veya String |Less veya eşittir karşılaştırması için ikinci değer. |

### <a name="return-value"></a>Döndürülen değer

İlk değer ikinci değerden küçük veya bu değere eşitse **true** değerini döndürür; Aksi takdirde, **false**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/lessorequals.json) , bir değerin diğerine eşit veya ondan küçük olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Iadeler | Bool | True |
| checkStrings | Bool | False |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager şablonundaki bölümlerin açıklaması için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
