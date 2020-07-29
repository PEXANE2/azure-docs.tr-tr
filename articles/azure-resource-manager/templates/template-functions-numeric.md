---
title: Şablon işlevleri-sayısal
description: Sayılarla çalışmak için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 00b44d971a487a0bbec27f3fc2d0746cedd6f874
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84677925"
---
# <a name="numeric-functions-for-arm-templates"></a>ARM şablonları için sayısal işlevler

Kaynak Yöneticisi, Azure Resource Manager (ARM) şablonunuzda tamsayılarla çalışmak için aşağıdaki işlevleri sağlar:

* [add](#add)
* [Copyındex](#copyindex)
* [div](#div)
* [float](#float)
* [int](#int)
* [Biçimlendir](#max)
* [Min](#min)
* [alma](#mod)
* [MUL](#mul)
* [alt](#sub)

## <a name="add"></a>add

`add(operand1, operand2)`

Belirtilen iki tam sayının toplamını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
|operand1 |Evet |int |Eklenecek ilk numara. |
|işlenen2 |Evet |int |Eklenecek ikinci sayı. |

### <a name="return-value"></a>Döndürülen değer

Parametrelerin toplamını içeren bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json) iki parametre ekler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Adresult | int | 8 |

## <a name="copyindex"></a>Copyındex

`copyIndex(loopName, offset)`

Yineleme döngüsünün dizinini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| loopName | Hayır | string | Yinelemeyi alma döngüsünün adı. |
| uzaklık |Hayır |int |Sıfır tabanlı yineleme değerine eklenecek sayı. |

### <a name="remarks"></a>Açıklamalar

Bu işlev her zaman bir **kopyalama** nesnesiyle birlikte kullanılır. **Konum**için hiçbir değer sağlanmazsa, geçerli yineleme değeri döndürülür. Yineleme değeri sıfırdan başlar.

**Loopname** özelliği, copyındex 'in bir kaynak yinelemeye mi yoksa Özellik yinelemesine mi başvurmadığını belirtmenizi sağlar. **Loopname**için değer sağlanmazsa, geçerli kaynak türü yinelemesi kullanılır. Bir özellik üzerinde yineleme yaparken **Loopname** için bir değer belirtin.

Kopyalama kullanma hakkında daha fazla bilgi için bkz.:

* [ARM şablonlarındaki kaynak yinelemesi](copy-resources.md)
* [ARM şablonlarındaki Özellik yinelemesi](copy-properties.md)
* [ARM şablonlarında değişken yineleme](copy-variables.md)
* [ARM şablonlarındaki çıkış yinelemesi](copy-outputs.md)

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir kopya döngüsünü ve ada dahil olan dizin değerini gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {}
}
```

### <a name="return-value"></a>Döndürülen değer

Yinelemenin geçerli dizinini temsil eden bir tamsayı.

## <a name="div"></a>div

`div(operand1, operand2)`

Belirtilen iki tamsayının tamsayı bölümünü döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |int |Bölünmekte olan sayı. |
| işlenen2 |Evet |int |Bölmek için kullanılan sayı. 0 olamaz. |

### <a name="return-value"></a>Döndürülen değer

Bölümü temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json) bir parametreyi başka bir parametreye böler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| divResult | int | 2 |

## <a name="float"></a>float

`float(arg1)`

Değeri kayan noktalı sayıya dönüştürür. Bu işlevi yalnızca, bir mantıksal uygulama gibi özel parametreleri uygulamaya geçirirken kullanırsınız.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya tamsayı |Kayan noktalı sayıya dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Bir kayan nokta numarası.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir mantıksal uygulamaya parametreleri geçirmek için float 'in nasıl kullanılacağını gösterir:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
            "custom1": {
                "value": "[float('3.0')]"
            },
            "custom2": {
                "value": "[float(3)]"
            },
```

## <a name="int"></a>int

`int(valueToConvert)`

Belirtilen değeri bir tamsayıya dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| valueToConvert |Evet |dize veya tamsayı |Tamsayıya dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Dönüştürülmüş değerin bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json) , Kullanıcı tarafından sağlanmış parametre değerini Integer olarak dönüştürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": {
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| intResult | int | 4 |

## <a name="max"></a>max

`max (arg1)`

Tamsayılar dizisinden en büyük değeri veya virgülle ayrılmış tamsayılar listesini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayılar dizisi veya virgülle ayrılmış tamsayılar listesi |En büyük değeri almak için koleksiyon. |

### <a name="return-value"></a>Döndürülen değer

Koleksiyondaki en büyük değeri temsil eden bir tamsayı.

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

`min (arg1)`

Tamsayılar dizisinden en küçük değeri veya virgülle ayrılmış tamsayılar listesini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayılar dizisi veya virgülle ayrılmış tamsayılar listesi |En küçük değeri almak için koleksiyon. |

### <a name="return-value"></a>Döndürülen değer

Koleksiyondan en küçük değeri temsil eden bir tamsayı.

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

## <a name="mod"></a>alma

`mod(operand1, operand2)`

Belirtilen iki tamsayının kullanıldığı tamsayı bölümünün kalanını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |int |Bölünmekte olan sayı. |
| işlenen2 |Evet |int |Bölmek için kullanılan sayı 0 olamaz. |

### <a name="return-value"></a>Döndürülen değer

Kalanı temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json) , bir parametreyi başka bir parametreye bölmek için geri kalanı döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| modResult | int | 1 |

## <a name="mul"></a>MUL

`mul(operand1, operand2)`

Belirtilen iki tamsayının çarpma sayısını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |int |Çarpılacak ilk sayı. |
| işlenen2 |Evet |int |Çarpılacak ikinci sayı. |

### <a name="return-value"></a>Döndürülen değer

Çarpma temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json) bir parametreyi başka bir parametreyle çarpar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Çoklu sonuç | int | 15 |

## <a name="sub"></a>alt

`sub(operand1, operand2)`

Belirtilen iki tamsayının çıkarma sayısını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |int |Çıkarılan sayı. |
| işlenen2 |Evet |int |Çıkarılan sayı. |

### <a name="return-value"></a>Döndürülen değer

Çıkarma temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json) bir parametreyi başka bir parametreden çıkartır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer to subtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| Alt sonuç | int | 4 |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager şablonundaki bölümlerin açıklaması için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
* Kaynak türünü oluştururken belirtilen sayıda yinelemek için, bkz. [Azure Resource Manager birden fazla kaynak örneği oluşturma](copy-resources.md).
