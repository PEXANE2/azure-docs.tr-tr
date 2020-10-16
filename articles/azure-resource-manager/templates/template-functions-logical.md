---
title: Şablon işlevleri-mantıksal
description: Mantıksal değerleri belirleyebilmek için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 10/12/2020
ms.openlocfilehash: ede41bd6c03eb7a01ae63526810d0310f31e4014
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978518"
---
# <a name="logical-functions-for-arm-templates"></a>ARM şablonları için mantıksal işlevler

Kaynak Yöneticisi, Azure Resource Manager (ARM) şablonlarınıza karşılaştırmalar yapmak için çeşitli işlevler sağlar.

* [and](#and)
* [bool](#bool)
* [yanlýþ](#false)
* [if](#if)
* [başlatılmadı](#not)
* [veya](#or)
* [değeri](#true)

## <a name="and"></a>ve

`and(arg1, arg2, ...)`

Tüm parametre değerlerinin doğru olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |boolean |Doğru olup olmadığını kontrol etmek için ilk değer. |
| arg2 |Yes |boolean |Doğru olup olmadığını kontrol etmek için ikinci değer. |
| ek bağımsız değişkenler |Hayır |boolean |Doğru olup olmadığını denetlemek için ek bağımsız değişkenler. |

### <a name="return-value"></a>Döndürülen değer

Tüm değerler true ise **true** döndürür; Aksi takdirde, **false**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) , mantıksal işlevlerin nasıl kullanılacağını göstermektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Yukarıdaki örnekteki çıktı:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| Andexamptaoutput | Bool | Yanlış |
| Orexamptaoutput | Bool | Doğru |
| Notexamptaoutput | Bool | Yanlış |

## <a name="bool"></a>bool

`bool(arg1)`

Parametreyi Boole değerine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |dize veya tamsayı |Boole değerine dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Dönüştürülmüş değerin Boole değeri.

### <a name="remarks"></a>Açıklamalar

Boolean değerleri almak için [true ()](#true) ve [false ()](#false) de kullanabilirsiniz.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json) , bool 'un dize veya tamsayı ile nasıl kullanılacağını göstermektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

Yukarıdaki örnekten alınan çıkış varsayılan değerleri:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| trueString | Bool | Doğru |
| Yanlışdize | Bool | Yanlış |
| Trueınt | Bool | Doğru |
| Yanlışint | Bool | Yanlış |

## <a name="false"></a>yanlış

`false()`

Yanlış değerini döndürür.

### <a name="parameters"></a>Parametreler

False işlevi herhangi bir parametreyi kabul etmez.

### <a name="return-value"></a>Döndürülen değer

Her zaman false olan bir Boole değeri.

### <a name="example"></a>Örnek

Aşağıdaki örnek, yanlış bir çıkış değeri döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "falseOutput": {
            "value": "[false()]",
            "type" : "bool"
        }
    }
}
```

Yukarıdaki örnekteki çıktı:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| Yanlışçıktı | Bool | Yanlış |

## <a name="if"></a>if

`if(condition, trueValue, falseValue)`

Bir koşulun doğru veya yanlış olduğunu temel alarak bir değer döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| koşul |Yes |boolean |Doğru veya yanlış olduğunu denetlemek için değer. |
| trueValue |Yes | dize, int, nesne veya dizi |Koşul doğru olduğunda döndürülecek değer. |
| Yanlışdeğer |Yes | dize, int, nesne veya dizi |Koşul false olduğunda döndürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

İlk parametre **true**olduğunda ikinci parametreyi döndürür; Aksi takdirde, üçüncü parametreyi döndürür.

### <a name="remarks"></a>Açıklamalar

Koşul **doğru**olduğunda, yalnızca true değeri değerlendirilir. Koşul **false**olduğunda, yalnızca false değeri değerlendirilir. **IF** işleviyle, yalnızca koşullu olarak geçerli olan ifadeleri ekleyebilirsiniz. Örneğin, bir koşul altında bulunan ancak diğer koşulun altında olmayan bir kaynağa başvurabilirsiniz. Aşağıdaki bölümde, ifadeleri koşullu olarak değerlendirmek için bir örnek gösterilmiştir.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json) , işlevinin nasıl kullanılacağını göstermektedir `if` .

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[if(equals('a', 'a'), json('{\"test\": \"value1\"}'), json('null'))]"
        }
    }
}
```

Yukarıdaki örnekteki çıktı:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| yesOutput | Dize | evet |
| noOutput | Dize | hayır |
| objectOutput | Nesne | {"test": "değer1"} |

Aşağıdaki [örnek şablonda](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/conditionWithReference.json) , bu işlevin yalnızca koşullu olarak geçerli olan ifadelerle nasıl kullanılacağı gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "logAnalytics": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "condition": "[not(empty(parameters('logAnalytics')))]",
            "name": "[concat(parameters('vmName'),'/omsOnboarding')]",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2017-03-30",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "MicrosoftMonitoringAgent",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "workspaceId": "[if(not(empty(parameters('logAnalytics'))), reference(parameters('logAnalytics'), '2015-11-01-preview').customerId, json('null'))]"
                },
                "protectedSettings": {
                    "workspaceKey": "[if(not(empty(parameters('logAnalytics'))), listKeys(parameters('logAnalytics'), '2015-11-01-preview').primarySharedKey, json('null'))]"
                }
            }
        }
    ],
    "outputs": {
        "mgmtStatus": {
            "type": "string",
            "value": "[if(not(empty(parameters('logAnalytics'))), 'Enabled monitoring for VM!', 'Nothing to enable')]"
        }
    }
}
```

## <a name="not"></a>not

`not(arg1)`

Boole değerini ters değerine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |boolean |Dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Parametre **false**olduğunda **true** döndürür. Parametre **true**olduğunda **false** döndürür.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) , mantıksal işlevlerin nasıl kullanılacağını göstermektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Yukarıdaki örnekteki çıktı:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| Andexamptaoutput | Bool | Yanlış |
| Orexamptaoutput | Bool | Doğru |
| Notexamptaoutput | Bool | Yanlış |

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) , [eşittir](template-functions-comparison.md#equals)ile **değil** .

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

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| Checttequals | Bool | Doğru |

## <a name="or"></a>veya

`or(arg1, arg2, ...)`

Herhangi bir parametre değerinin doğru olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |boolean |Doğru olup olmadığını kontrol etmek için ilk değer. |
| arg2 |Yes |boolean |Doğru olup olmadığını kontrol etmek için ikinci değer. |
| ek bağımsız değişkenler |Hayır |boolean |Doğru olup olmadığını denetlemek için ek bağımsız değişkenler. |

### <a name="return-value"></a>Döndürülen değer

Herhangi bir değer true olduğunda **true** değerini döndürür; Aksi takdirde, **false**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) , mantıksal işlevlerin nasıl kullanılacağını göstermektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Yukarıdaki örnekteki çıktı:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| Andexamptaoutput | Bool | Yanlış |
| Orexamptaoutput | Bool | Doğru |
| Notexamptaoutput | Bool | Yanlış |

## <a name="true"></a>true

`true()`

True değerini döndürür.

### <a name="parameters"></a>Parametreler

True işlevi herhangi bir parametreyi kabul etmez.

### <a name="return-value"></a>Döndürülen değer

Her zaman true olan bir Boole değeri.

### <a name="example"></a>Örnek

Aşağıdaki örnek, doğru bir çıkış değeri döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueOutput": {
            "value": "[true()]",
            "type" : "bool"
        }
    }
}
```

Yukarıdaki örnekteki çıktı:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| trueOutput | Bool | Doğru |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager şablonundaki bölümlerin açıklaması için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).

