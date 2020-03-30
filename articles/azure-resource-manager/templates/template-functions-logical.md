---
title: Şablon işlevleri - mantıksal
description: Mantıksal değerleri belirlemek için Azure Kaynak Yöneticisi şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: f058baa32e5f93a4177913287a5e9873fa7a9acb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156319"
---
# <a name="logical-functions-for-arm-templates"></a>ARM şablonları için mantıksal işlevler

Kaynak Yöneticisi, Azure Kaynak Yöneticisi (ARM) şablonlarınızda karşılaştırma yapmak için çeşitli işlevler sağlar.

* [Ve](#and)
* [bool](#bool)
* [if](#if)
* [Değil](#not)
* [Veya](#or)

## <a name="and"></a>ve

`and(arg1, arg2, ...)`

Tüm parametre değerlerinin doğru olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boole |Doğru olup olmadığını kontrol etmek için ilk değer. |
| arg2 |Evet |boole |Doğru olup olmadığını kontrol etmek için ikinci değer. |
| ek bağımsız değişkenler |Hayır |boole |Doğru olup olmadığını denetlemek için ek bağımsız değişkenler. |

### <a name="return-value"></a>Döndürülen değer

Tüm değerler doğruysa **True** döndürür; aksi takdirde, **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) mantıksal işlevlerin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

## <a name="bool"></a>bool

`bool(arg1)`

Parametreyi boolean'a dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya int |Boolean dönüştürmek için değer. |

### <a name="return-value"></a>Döndürülen değer
Dönüştürülen değerin bir boolean.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json) bir dize veya tamsayı ile bool nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Varsayılan değerlerle önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| trueString | Bool | True |
| falseString | Bool | False |
| trueInt | Bool | True |
| falseInt | Bool | False |

## <a name="if"></a>if

`if(condition, trueValue, falseValue)`

Bir koşulun doğru veya yanlış olup olmadığına bağlı olarak bir değer verir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| Durum |Evet |boole |Doğru mu yanlış mı olduğunu kontrol etmek için gereken değer. |
| Truevalue |Evet | dize, int, nesne veya dizi |Koşul doğru olduğunda döndürülecek değer. |
| Falsevalue |Evet | dize, int, nesne veya dizi |Koşul yanlış olduğunda döndürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

İlk parametre **True**olduğunda ikinci parametreyi verir; aksi takdirde, üçüncü parametre döndürür.

### <a name="remarks"></a>Açıklamalar

Koşul **True**olduğunda, yalnızca gerçek değer değerlendirilir. Koşul **False**olduğunda, yalnızca yanlış değer değerlendirilir. **if** işlevi ile, yalnızca koşullu geçerli ifadeler ekleyebilirsiniz. Örneğin, bir koşul altında var olan ancak diğer koşul altında olmayan bir kaynağa başvuruyapabilirsiniz. Koşullu değerlendirme ifadelerinin bir örneği aşağıdaki bölümde gösterilmiştir.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json) işlevin `if` nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| yesOutput | Dize | evet |
| noOutput | Dize | hayır |
| objectOutput | Nesne | { "test": "value1" } |

Aşağıdaki [örnek şablon,](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/conditionWithReference.json) bu işlevin yalnızca koşullu olarak geçerli olan ifadelerle nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Boolean değerini zıt değerine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boole |Dönüştürülecek değer. |

### <a name="return-value"></a>Döndürülen değer

Parametre **Yanlış**olduğunda **True'yı** döndürür. Parametre **Doğru**olduğunda **False'u** döndürür.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) mantıksal işlevlerin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

Aşağıdaki [örnek](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) [şabloneşitlerle](template-functions-comparison.md#equals) **değil** kullanır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

Önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| checkNotEquals | Bool | True |

## <a name="or"></a>or

`or(arg1, arg2, ...)`

Herhangi bir parametre değerinin doğru olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boole |Doğru olup olmadığını kontrol etmek için ilk değer. |
| arg2 |Evet |boole |Doğru olup olmadığını kontrol etmek için ikinci değer. |
| ek bağımsız değişkenler |Hayır |boole |Doğru olup olmadığını denetlemek için ek bağımsız değişkenler. |

### <a name="return-value"></a>Döndürülen değer

Herhangi bir değer doğruysa **True** döndürür; aksi takdirde, **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon,](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) mantıksal işlevlerin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Önceki örnekten çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Kaynak Yöneticisi şablonundaki bölümlerin açıklaması için [bkz.](template-syntax.md)
* Birden çok şablonu birleştirmek için bkz: [Azure Kaynak Yöneticisi ile bağlantılı şablonları kullanma.](linked-templates.md)
* Bir kaynak türü oluştururken belirli sayıda kez yeniden sıralamak için azure [kaynak yöneticisinde birden çok kaynak örneği oluşturma](copy-resources.md)bölümüne bakın.
* Oluşturduğunuz şablonu nasıl dağıtabileceğinizi görmek için Azure [Kaynak Yöneticisi şablonuyla bir uygulama dağıt'a](deploy-powershell.md)bakın.

