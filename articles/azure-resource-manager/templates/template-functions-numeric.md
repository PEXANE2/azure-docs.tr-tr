---
title: Şablon işlevleri - sayısal
description: Sayılarla çalışmak için Azure Kaynak Yöneticisi şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 11/08/2017
ms.openlocfilehash: 2ca5c539036d002b83b8141132a0ebf2530dc6af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156353"
---
# <a name="numeric-functions-for-arm-templates"></a>ARM şablonları için sayısal işlevler

Kaynak Yöneticisi, Azure Kaynak Yöneticisi (ARM) şablonunuzda ki tümsekler ile çalışmak için aşağıdaki işlevleri sağlar:

* [add](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [float](#float)
* [int](#int)
* [Max](#max)
* [Dk](#min)
* [mod](#mod)
* [Mul](#mul)
* [Alt](#sub)

<a id="add" />

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="add"></a>add
`add(operand1, operand2)`

Sağlanan iki toplamın toplamını verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
|operand1 |Evet |int |Eklenecek ilk sayı. |
|operand2 |Evet |int |Eklenecek ikinci sayı. |

### <a name="return-value"></a>Döndürülen değer

Parametrelerin toplamını içeren bir sonsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şabloniki](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json) parametre ekler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| addResult | int | 8 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json
```

<a id="copyindex" />

## <a name="copyindex"></a>copyIndex
`copyIndex(loopName, offset)`

Yineleme döngüsü dizini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| loopName | Hayır | string | Yineleme almak için döngü nün adı. |
| uzaklık |Hayır |int |Sıfır tabanlı yineleme değerine eklenecek sayı. |

### <a name="remarks"></a>Açıklamalar

Bu işlev her zaman bir **kopyalama** nesnesi ile kullanılır. **Ofset**için değer sağlanmadıysa, geçerli yineleme değeri döndürülür. Yineleme değeri sıfırdan başlar. Kaynakları veya değişkenleri tanımlarken yineleme döngülerini kullanabilirsiniz.

**loopName** özelliği, copyIndex'in bir kaynak yinelemesine mi yoksa özellik yinelemesine mi atıfta bulunup bulunmadığını belirtmenize olanak tanır. **loopName**için değer sağlanmadıysa, geçerli kaynak türü yinelemesi kullanılır. Bir özellik üzerinde yinelenirken **loopName** için bir değer sağlayın.

**copyIndex'i**nasıl kullandığınızın tam açıklaması için [bkz.](copy-resources.md)

Bir değişkeni tanımlarken **copyIndex** kullanma örneği için, [bkz.](template-syntax.md#variables)

### <a name="example"></a>Örnek

Aşağıdaki örnekte bir kopya döngüsü ve addahil dizin değeri gösterilmektedir.

```json
"resources": [
  {
    "name": "[concat('examplecopy-', copyIndex())]",
    "type": "Microsoft.Web/sites",
    "copy": {
      "name": "websitescopy",
      "count": "[parameters('count')]"
    },
    ...
  }
]
```

### <a name="return-value"></a>Döndürülen değer

Yinelemenin geçerli dizinini temsil eden bir sonsyon.

<a id="div" />

## <a name="div"></a>div
`div(operand1, operand2)`

Sağlanan iki tümsenin insabölümünü verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |int |Bölünen sayı. |
| operand2 |Evet |int |Bölmek için kullanılan sayı. 0 olamaz. |

### <a name="return-value"></a>Döndürülen değer

Tümeni temsil eden bir karşıcı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json) bir parametreyi başka bir parametreye böler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| divResult | int | 2 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json
```

<a id="float" />

## <a name="float"></a>float
`float(arg1)`

Değeri kayan nokta numarasına dönüştürür. Bu işlevi yalnızca Mantık Uygulaması gibi bir uygulamaya özel parametreler aktarırken kullanırsınız.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya int |Kayan nokta numarasına dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer
Kayan nokta numarası.

### <a name="example"></a>Örnek

Aşağıdaki örnekte, bir Mantık Uygulamasına parametreleri geçirmek için float nasıl kullanılacağı gösterilmektedir:

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

<a id="int" />

## <a name="int"></a>int
`int(valueToConvert)`

Belirtilen değeri bir karşıcıya dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| valueToConvert |Evet |dize veya int |Bir karşıcıya dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Dönüştürülen değerin bir karşıcısı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json) kullanıcı tarafından sağlanan parametre değerini birandıkaya dönüştürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| intResult | int | 4 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

<a id="max" />

## <a name="max"></a>max
`max (arg1)`

Bir dizi tamsayı veya virgülle ayrılmış tamsayılar listesinden en büyük değeri verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |isterler dizisi veya virgülle ayrılmış bir insa listesi |En yüksek değeri elde etmek için koleksiyon. |

### <a name="return-value"></a>Döndürülen değer

Koleksiyondaki en büyük değeri temsil eden bir karşıcı.

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

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

<a id="min" />

## <a name="min"></a>min
`min (arg1)`

Bir tamsede dizisinden veya virgülle ayrılmış bir tamsayılar listesinden en az değeri verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |isterler dizisi veya virgülle ayrılmış bir insa listesi |En düşük değeri almak için toplama. |

### <a name="return-value"></a>Döndürülen değer

Koleksiyondaki minimum değeri temsil eden bir sonda.

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

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

<a id="mod" />

## <a name="mod"></a>mod
`mod(operand1, operand2)`

Sağlanan iki tümseci kullanarak, tümseci bölümünün geri kalanını verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |int |Bölünen sayı. |
| operand2 |Evet |int |Bölmek için kullanılan sayı 0 olamaz. |

### <a name="return-value"></a>Döndürülen değer
Geri kalanını temsil eden bir sonsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json) bir parametrenin kalan kısmını başka bir parametreye bölerek döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| modResult | int | 1 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

<a id="mul" />

## <a name="mul"></a>Mul
`mul(operand1, operand2)`

Sağlanan iki tümsenin çoğalmasını verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |int |Çarpılacayailk sayı. |
| operand2 |Evet |int |Çarpılmak için ikinci sayı. |

### <a name="return-value"></a>Döndürülen değer

Çarpma işlemini temsil eden bir karşıcı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json) bir parametreyi başka bir parametreyle çarpar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| mulResult | int | 15 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

<a id="sub" />

## <a name="sub"></a>Alt
`sub(operand1, operand2)`

Sağlanan iki tümsenin çıkarını verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |int |Çıkarılan sayı. |
| operand2 |Evet |int |Çıkarılan sayı. |

### <a name="return-value"></a>Döndürülen değer
Çıkarmayı temsil eden bir karşımat.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json) başka bir parametreden bir parametre çıkarır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| subResult | int | 4 |

Bu örnek şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
az deployment group create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunları kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Kaynak Yöneticisi şablonundaki bölümlerin açıklaması için [bkz.](template-syntax.md)
* Birden çok şablonu birleştirmek için bkz: [Azure Kaynak Yöneticisi ile bağlantılı şablonları kullanma.](linked-templates.md)
* Bir kaynak türü oluştururken belirli sayıda kez yeniden sıralamak için azure [kaynak yöneticisinde birden çok kaynak örneği oluşturma](copy-resources.md)bölümüne bakın.
* Oluşturduğunuz şablonu nasıl dağıtabileceğinizi görmek için Azure [Kaynak Yöneticisi şablonuyla bir uygulama dağıt'a](deploy-powershell.md)bakın.

