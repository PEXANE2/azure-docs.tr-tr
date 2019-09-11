---
title: Azure Resource Manager şablon işlevleri-mantıksal | Microsoft Docs
description: Mantıksal değerleri belirleyebilmek için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/15/2019
ms.author: tomfitz
ms.openlocfilehash: ea91798a1c0ca0aad729128ce4694a85165f3c3b
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70194794"
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Azure Resource Manager şablonları için mantıksal işlevler

Kaynak Yöneticisi, şablonlarınıza karşılaştırmalar yapmak için çeşitli işlevler sağlar.

* [and](#and)
* [bool](#bool)
* [if](#if)
* [not](#not)
* [or](#or)

## <a name="and"></a>and

`and(arg1, arg2, ...)`

Tüm parametre değerlerinin doğru olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boolean |Doğru olup olmadığını kontrol etmek için ilk değer. |
| arg2 |Evet |boolean |Doğru olup olmadığını kontrol etmek için ikinci değer. |
| ek bağımsız değişkenler |Hayır |boolean |Doğru olup olmadığını denetlemek için ek bağımsız değişkenler. |

### <a name="return-value"></a>Dönüş değeri

Tüm değerler true ise **true** döndürür; Aksi takdirde, **false**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) , mantıksal işlevlerin nasıl kullanılacağını göstermektedir.

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

Yukarıdaki örnekteki çıktı:

| Name | Tür | Value |
| ---- | ---- | ----- |
| Andexamptaoutput | Bool | False |
| Orexamptaoutput | Bool | Doğru |
| Notexamptaoutput | Bool | False |

## <a name="bool"></a>bool

`bool(arg1)`

Parametreyi Boole değerine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya tamsayı |Boole değerine dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri
Dönüştürülmüş değerin Boole değeri.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json) , bool 'un dize veya tamsayı ile nasıl kullanılacağını göstermektedir.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Value |
| ---- | ---- | ----- |
| trueString | Bool | Doğru |
| Yanlışdize | Bool | False |
| Trueınt | Bool | Doğru |
| Yanlışint | Bool | False |

## <a name="if"></a>if

`if(condition, trueValue, falseValue)`

Bir koşulun doğru veya yanlış olduğunu temel alarak bir değer döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| condition |Evet |boolean |Doğru veya yanlış olduğunu denetlemek için değer. |
| trueValue |Evet | dize, int, nesne veya dizi |Koşul doğru olduğunda döndürülecek değer. |
| Yanlışdeğer |Evet | dize, int, nesne veya dizi |Koşul false olduğunda döndürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

İlk parametre **true**olduğunda ikinci parametreyi döndürür; Aksi takdirde, üçüncü parametreyi döndürür.

### <a name="remarks"></a>Açıklamalar

Koşul **doğru**olduğunda, yalnızca true değeri değerlendirilir. Koşul **false**olduğunda, yalnızca false değeri değerlendirilir. **IF** işleviyle, yalnızca koşullu olarak geçerli olan ifadeleri ekleyebilirsiniz. Örneğin, bir koşul altında bulunan ancak diğer koşulun altında olmayan bir kaynağa başvurabilirsiniz. Aşağıdaki bölümde, ifadeleri koşullu olarak değerlendirmek için bir örnek gösterilmiştir.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json) , `if` işlevinin nasıl kullanılacağını göstermektedir.

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

Yukarıdaki örnekteki çıktı:

| Name | Tür | Value |
| ---- | ---- | ----- |
| yesOutput | Dize | evet |
| noOutput | Dize | hayır |
| objectOutput | Object | {"test": "değer1"} |

Aşağıdaki [örnek şablonda](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/conditionWithReference.json) , bu işlevin yalnızca koşullu olarak geçerli olan ifadelerle nasıl kullanılacağı gösterilmektedir.

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

## <a name="not"></a>başlatılmadı

`not(arg1)`

Boole değerini ters değerine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boolean |Dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Parametre **false**olduğunda **true** döndürür. Parametre **true**olduğunda **false** döndürür.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) , mantıksal işlevlerin nasıl kullanılacağını göstermektedir.

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

Yukarıdaki örnekteki çıktı:

| Name | Tür | Value |
| ---- | ---- | ----- |
| Andexamptaoutput | Bool | False |
| Orexamptaoutput | Bool | Doğru |
| Notexamptaoutput | Bool | False |

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) , [eşittir](resource-group-template-functions-comparison.md#equals)ile **değil** .

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

Yukarıdaki örnekteki çıktı:

| Name | Tür | Value |
| ---- | ---- | ----- |
| Checttequals | Bool | Doğru |

## <a name="or"></a>veya

`or(arg1, arg2, ...)`

Herhangi bir parametre değerinin doğru olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boolean |Doğru olup olmadığını kontrol etmek için ilk değer. |
| arg2 |Evet |boolean |Doğru olup olmadığını kontrol etmek için ikinci değer. |
| ek bağımsız değişkenler |Hayır |boolean |Doğru olup olmadığını denetlemek için ek bağımsız değişkenler. |

### <a name="return-value"></a>Dönüş değeri

Herhangi bir değer true olduğunda **true** değerini döndürür; Aksi takdirde, **false**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) , mantıksal işlevlerin nasıl kullanılacağını göstermektedir.

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

Yukarıdaki örnekteki çıktı:

| Name | Tür | Value |
| ---- | ---- | ----- |
| Andexamptaoutput | Bool | False |
| Orexamptaoutput | Bool | Doğru |
| Notexamptaoutput | Bool | False |

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

